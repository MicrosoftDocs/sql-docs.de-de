---
title: Konvertieren von R-Code für SQL Server
description: Migrieren Sie R-Code für die Lösungsbereitstellung und den Zugriff auf relationale Daten auf SQL Server zu einer gespeicherten SQL Server-Prozedur.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 0b0d3e451c3b95bfa2401f80176dbdb659f1cb86
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336288"
---
# <a name="convert-r-code-for-execution-in-sql-server-in-database-instances"></a>Konvertieren von R-Code für die Ausführung in (datenbankinternen) SQL Server-Instanzen
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

In diesem Artikel finden Sie eine allgemeine Anleitung zum Anpassen von R-Code, damit dieser in SQL Server funktioniert. 

Wenn Sie R-Code aus RStudio oder einer anderen Umgebung zu SQL Server verschieben, funktioniert der Code in den meisten Fällen ohne weitere Anpassungen: wenn der Code beispielsweise einfach ist, etwa eine Funktion, die Eingaben erfasst und einen Wert zurückgibt. Es ist auch einfacher, Lösungen zu portieren, die die Pakete **RevoScaleR** oder **MicrosoftML** verwenden, die die Ausführung in verschiedenen Ausführungskontexten mit minimalen Änderungen unterstützen.

Allerdings erfordert Ihr Code möglicherweise wesentliche Änderungen, wenn eine der folgenden Bedingungen vorliegt:

+ Sie verwenden R-Bibliotheken, die auf das Netzwerk zugreifen oder nicht auf SQL Server installiert werden können.
+ Der Code führt separate Aufrufe von Datenquellen außerhalb von SQL Server aus, z. B. Excel-Arbeitsblätter, Dateien in Freigaben und andere Datenbanken. 
+ Sie möchten den Code im *\@script*-Parameter von [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) ausführen und die gespeicherte Prozedur parametrisieren.
+ Ihre ursprüngliche Lösung umfasst mehrere Schritte, die bei unabhängiger Ausführung in einer Produktionsumgebung effizienter durchgeführt werden können, z. B. Datenaufbereitung oder Featureentwicklung im Vergleich zu Modelltraining, Bewertung oder Berichterstellung.
+ Sie möchten die Leistung durch Ändern der Bibliotheken optimieren, indem Sie die parallele Ausführung verwenden oder einen Teil der Verarbeitungsvorgänge auf SQL Server abladen. 

## <a name="step-1-plan-requirements-and-resources"></a>Schritt 1: Planen der Anforderungen und Ressourcen

### <a name="packages"></a>Pakete

+ Ermitteln Sie, welche Pakete Sie benötigen, und stellen Sie sicher, dass sie auf SQL Server funktionieren.
 
+ Installieren Sie die Pakete vorab in der Standardpaketbibliothek, die von Machine Learning Services verwendet wird. Benutzerbibliotheken werden nicht unterstützt.

### <a name="data-sources"></a>Datenquellen

+ Wenn Sie Ihren R-Code in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) einbetten möchten, identifizieren Sie die primären und sekundären Datenquellen. 

  + **Primäre** Datenquellen sind große Datasets, z. B. Modelltrainingsdaten oder Eingabedaten für Vorhersagen. Sie sollten Ihr größtes Dataset dem Eingabeparameter von [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) zuordnen.

  + **Sekundäre** Datenquellen sind in der Regel kleinere Datasets, z. B. Faktorlisten oder zusätzliche Gruppierungsvariablen. 
  
  Aktuell unterstützt sp_execute_external_script nur ein einzelnes Dataset als Eingabe für die gespeicherte Prozedur. Sie können jedoch mehrere skalare oder binäre Eingaben hinzufügen.

  Aufrufe gespeicherter Prozeduren, denen EXECUTE vorangestellt ist, können nicht als Eingabe für [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) verwendet werden. Sie können Abfragen, Sichten oder andere gültige SELECT-Anweisungen verwenden.

+ Bestimmen Sie, welche Ausgaben Sie benötigen. Wenn Sie R-Code mit sp_execute_external_script ausführen, kann die gespeicherte Prozedur nur einen Datenrahmen ausgeben. Allerdings können Sie auch mehrere skalare Ausgaben, einschließlich Plots und Modelle im Binärformat, sowie andere Skalarwerte ausgeben, die von R-Code oder SQL-Parametern abgeleitet werden.

### <a name="data-types"></a>Datentypen

+ Stellen Sie eine Prüfliste der möglichen Probleme bei Datentypen zusammen.

  Alle R-Datentypen werden von SQL Server Machine Learning Services unterstützt. Allerdings unterstützt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine größere Vielfalt von Datentypen als R. Daher sind einige implizite Datentypkonvertierungen erforderlich, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Daten an oder von R übermittelt werden. Möglicherweise müssen Sie einige Daten explizit umwandeln oder konvertieren. 

  NULL-Werte werden unterstützt. R nutzt jedoch das Datenkonstrukt `na`, um einen fehlenden Wert darzustellen, der dem Wert NULL ähnelt.

+ Ziehen Sie in Betracht, Abhängigkeiten von Daten zu entfernen, die nicht von R verwendet werden können: beispielsweise können die Datentypen ROWID und GUID von SQL Server nicht von R verarbeitet werden, was zu Fehlern führt.

  Weitere Informationen finden Sie unter [R-Bibliotheken und -Datentypen](../r/r-libraries-and-data-types.md).

## <a name="step-2-convert-or-repackage-code"></a>Schritt 2: Konvertieren oder erneutes Packen von Code

Wie viele Änderungen Sie an Ihrem Code vornehmen hängt davon ab, ob Sie den R-Code von einem Remoteclient für die Ausführung im SQL Server-Computekontext übermitteln möchten oder planen, den Code als Teil einer gespeicherten Prozedur bereitzustellen, wodurch eine bessere Leistung und Datensicherheit erzielt werden kann. Die Umschließung Ihres Codes in einer gespeicherten Prozedur umfasst zusätzliche Anforderungen. 

+ Definieren Sie Ihre primären Eingabedaten nach Möglichkeit als SQL-Abfrage, um Datenverschiebung zu vermeiden.

+ Wenn Sie R in einer gespeicherten Prozedur ausführen, können Sie mehrere **skalare** Eingaben übergeben. Fügen Sie zu allen Parametern, die Sie in der Ausgabe verwenden möchten, das Schlüsselwort **OUTPUT** hinzu. 

  Die folgende skalare Eingabe `@model_name` enthält beispielsweise den Modellnamen, der in den Ergebnissen ebenfalls in seiner eigenen Spalte ausgegeben wird:

  ```sql
  EXEC sp_execute_external_script @model_name="DefaultModel" OUTPUT, @language=N'R', @script=N'R code here'
  ```

+ Alle Variablen, die Sie als Parameter der gespeicherten Prozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) übergeben, müssen Variablen im R-Code zugeordnet werden. Standardmäßig werden die Variablen nach Namen zugeordnet.

  Alle Spalten im Eingabedataset müssen ebenfalls Variablen im R-Skript zugeordnet sein.  Angenommen, Ihr R-Skript enthält eine Formel wie die folgende:

  ```R
  formula <- ArrDelay ~ CRSDepTime + DayOfWeek + CRSDepHour:DayOfWeek
  ```
  
  Ein Fehler wird ausgelöst, wenn das Eingabedataset keine Spalten mit den folgenden übereinstimmenden Namen enthält: ArrDelay, CRSDepTime, DayOfWeek, CRSDepHour und DayOfWeek.

+ In manchen Fällen muss ein Ausgabeschema für die Ergebnisse im Voraus definiert werden.

  Beispielsweise müssen Sie die Klausel **WITH RESULT SET** zum Festlegen des Schemas verwenden, um die Daten in eine Tabelle einzufügen.

  Das Ausgabeschema ist ebenfalls erforderlich, wenn das R-Skript das Argument `@parallel=1` verwendet. Grund hierfür ist, dass mehrere Prozesse von SQL Server zum Ausführen der Abfrage möglicherweise parallel erstellt und die Ergebnisse am Ende gesammelten werden. Daher muss das Ausgabeschema vorbereitet werden, bevor die parallelen Prozesse erstellt werden können.
  
  In anderen Fällen können Sie das Ergebnisschema auslassen, indem Sie die Option **WITH RESULT SETS UNDEFINED** verwenden. Diese Anweisung gibt das Dataset aus dem R-Skript zurück, ohne die Spalten zu benennen oder die SQL-Datentypen festzulegen.

+ Ziehen Sie in Betracht, Zeitsteuerungs- oder Nachverfolgungsdaten mithilfe von T-SQL anstelle von R zu generieren.

  Beispielsweise können Sie die Systemzeit oder andere Informationen für die Überwachung und Speicherung übergeben, indem Sie einen T-SQL-Aufruf hinzufügen, der an die Ergebnisse übergeben wird, anstatt ähnliche Daten im R-Skript zu generieren. 

### <a name="improve-performance-and-security"></a>Verbessern der Leistung und Sicherheit

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ Vermeiden Sie es, Vorhersagen oder Zwischenergebnisse in Dateien zu schreiben. Schreiben Sie Vorhersagen stattdessen in eine Tabelle, um Datenverschiebung zu vermeiden.
::: moniker-end

+ Führen Sie alle Abfragen im Voraus aus, und überprüfen Sie die SQL Server-Abfragepläne, um Aufgaben zu identifizieren, die parallel ausgeführt werden können.

  Wenn die Eingabeabfrage parallelisiert werden kann, legen Sie `@parallel=1` als Teil Ihrer Argumente auf [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) fest. 

  Eine Parallelverarbeitung mit diesem Flag ist in der Regel jedes Mal möglich, wenn SQL Server mit partitionierten Tabellen arbeiten oder eine Abfrage zwischen mehreren Prozessen verteilen kann und die Ergebnisse am Ende aggregiert. Eine Parallelverarbeitung mit diesem Flag ist nicht möglich, wenn Sie Modelle mithilfe von Algorithmen trainieren, die ein Lesen aller Daten voraussetzen, oder wenn Sie Aggregate erstellen müssen.

+ Überprüfen Sie Ihren R-Code, um Schritte zu identifizieren, die unabhängig oder effizienter ausgeführt werden können, und verwenden Sie dazu einen separat gespeicherten Prozeduraufruf. Beispielsweise können Sie bessere Leistung erzielen, indem Sie die Featureentwicklung oder Featureextraktion separat durchführen und die Werte in einer Tabelle speichern.

+ Suchen Sie nach Möglichkeiten, T-SQL anstelle von R-Code für gruppenbasierte Berechnungen.

  ::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
  Diese R-Lösung veranschaulicht beispielsweise, wie benutzerdefinierte T-SQL-Funktionen und R dieselbe Featureentwicklungsaufgabe durchführen können: [Exemplarische Vorgehensweise für Data Science](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md).
  ::: moniker-end

+ Ersetzen Sie konventionelle R-Funktionen nach Möglichkeit durch **ScaleR**-Funktionen, die verteilte Ausführung unterstützen. Weitere Informationen finden Sie unter [Grundlegender R- und ScaleR-Funktionen im Vergleich](/machine-learning-server/r-reference/revoscaler/revoscaler-compared-to-base-r).

+ Lassen Sie sich von einem Datenbankentwickler beraten, um Möglichkeiten zu finden, die Leistung mithilfe von SQL Server-Features zu verbessern, z. B. mit [arbeitsspeicheroptimierten Tabellen](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md) oder mit dem [Resource Governor](../../relational-databases/resource-governor/resource-governor.md), sofern Sie über die Enterprise Edition verfügen.

## <a name="step-3-prepare-for-deployment"></a>Schritt 3: Vorbereiten der Bereitstellung

+ Benachrichtigen Sie den Administrator, damit Pakete installiert und im Voraus getestet werden, bevor Sie Ihren Code bereitstellen. 

  In einer Entwicklungsumgebung lassen sich Pakete als Teil des Codes durchaus installieren. In einer Produktionsumgebung empfiehlt sich diese Vorgehensweise jedoch nicht. 

  Benutzerbibliotheken werden unabhängig davon, ob Sie eine gespeicherte Prozedur verwenden oder R-Code im SQL Server-Computekontext ausführen, nicht unterstützt.

### <a name="package-your-r-code-in-a-stored-procedure"></a>Packen Ihres R-Codes in eine gespeicherte Prozedur

+ Wenn Ihr Code relativ einfach ist, können Sie ihn wie in den folgenden Beispielen beschrieben ohne weitere Anpassungen in eine benutzerdefinierte T-SQL-Funktion einbetten:

  + [Featureentwicklung mit T-SQL und R](../tutorials/r-taxi-classification-create-features.md)

+ Wenn der Code komplexer ist, verwenden Sie das R-Paket **sqlrutils** zum Konvertieren Ihres Codes. Dieses Paket wurde dazu konzipiert, erfahrene R-Benutzer beim Schreiben von gutem Code für gespeicherte Prozeduren zu unterstützen. 

  Der erste Schritt besteht darin, Ihren R-Code in eine einzelne Funktion mit klar definierten Eingaben und Ausgaben umzuschreiben.

  Verwenden Sie anschließend das Paket **sqlrutils**, um die Eingaben und Ausgaben im korrekten Format zu generieren. Das Paket **sqlrutils** generiert den gesamten Code für die gespeicherte Prozedur für Sie. Es kann die gespeicherte Prozedur auch in der Datenbank registrieren. 

  Weitere Informationen und Beispiele finden Sie unter [sqlrutils (SQL)](ref-r-sqlrutils.md).

### <a name="integrate-with-other-workflows"></a>Integration in andere Workflows

+ Nutzen Sie T-SQL-Tools und ETL-Prozesse. Führen Sie Featureentwicklung, Featureextraktion und Datenbereinigung im Rahmen Ihrer Datenworkflows im Voraus durch.

  Wenn Sie in einer dedizierten R-Entwicklungsumgebung wie [!INCLUDE[rsql_rtvs_md](../../includes/rsql-rtvs-md.md)] oder RStudio arbeiten, übertragen Sie Daten womöglich per Pull auf Ihren Computer, analysieren die Daten iterativ und geben dann die Ergebnisse aus oder zeigen sie an. 
  
  Bei einer Migration von eigenständigem R-Code zu SQL Server kann jedoch ein Großteil dieses Prozesses vereinfacht oder an andere SQL Server-Tools delegiert werden. 

+ Verwenden Sie sichere, asynchrone Vorgehensweisen für die Visualisierung.

  SQL Server-Benutzer können oft nicht auf Dateien auf dem Server zugreifen, und SQL-Clienttools unterstützen das R-Grafikgerät in der Regel nicht. Wenn Sie Plots oder andere Grafiken im Rahmen der Lösung generieren, sollten Sie das Exportieren der Plots als Binärdaten und das Speichern bzw. Schreiben dieser in eine Tabelle in Betracht ziehen.

+ Umschließen Sie Vorhersage- und Bewertungsfunktionen für den direkten Zugriff von Anwendungen in gespeicherten Prozeduren.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Beispiele behandeln die Bereitstellung einer R-Lösung in SQL Server:

+ [Tutorial: Entwickeln eines Vorhersagemodells in R mit SQL Machine Learning](../tutorials/r-predictive-model-introduction.md)

+ [R-Tutorial: Vorhersagen von Taxi-Fahrpreisen in New York City mit binärer Klassifizierung](../tutorials/r-taxi-classification-introduction.md)

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ Das Beispiel [End-to-End-Data Science-Lösung](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md) enthält einen Vergleich der Featureentwicklung in R und T-SQL
::: moniker-end
