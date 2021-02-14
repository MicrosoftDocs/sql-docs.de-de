---
title: Datenvirtualisierungserweiterung
description: Datenvirtualisierungserweiterung für Azure Data Studio
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: rajmera3
ms.author: raajmera
ms.reviewer: alayu, sstein, maghan
ms.custom: ''
ms.date: 11/04/2019
ms.openlocfilehash: a428322867df8585160a3363d174b2cd92bd1f17
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052051"
---
# <a name="data-virtualization-extension-for-azure-data-studio"></a>Datenvirtualisierungserweiterung für Azure Data Studio

Die Datenvirtualisierungserweiterung für Azure Data Studio unterstützt den [Assistenten zum Erstellen externer Tabellen mit ODBC-Datenquellen](../../relational-databases/polybase/data-virtualization.md).

## <a name="install-the-data-virtualization-extension"></a>Installieren der Datenvirtualisierungserweiterung

Um die Datenvirtualisierungserweiterung zu installieren, besuchen Sie [Erweitern der Funktionalität von Azure Data Studio](./add-extensions.md).

## <a name="changes-in-release-10"></a>Änderungen in Release 1.0

* Erweiterung in „Datenvirtualisierung“ umbenannt.
* Assistent für die Erstellung externer Tabellen:
  * Geführte Notebooks für die Virtualisierung von MongoDB- und Teradata-Quellen.
  * Hinzugefügtes Dialogfeld zum Ausfüllen von Variablen in MongoDB- und Teradata- Virtualisierungsnotebooks.

## <a name="changes-in-release-016"></a>Änderungen in Release 0.16

* Assistent für die Erstellung externer Tabellen:
  * Verbesserte Fehlerbehandlung beim Laden von Tabellen und Sichten auf der Seite „Objektzuordnung“.

## <a name="changes-in-release-015"></a>Änderungen in Release 0.15

* Assistent für die Erstellung externer Tabellen:
  *  Die Zeit zum Laden von Tabellen- und Spalteninformationen auf der Objektzuordnungsseite wurde reduziert.
  * Es wurde ein Fehler in Verbindung mit dem Laden vorhandener datenbankweit gültiger Anmeldeinformationen auf der Verbindungsdetailsseite behoben.
* Assistent zum Erstellen einer externen Tabelle aus CSV-Dateien:
  * Erweiterte standardmäßige Stichprobengröße, die für die PROSE-Analyse verwendet wird.

## <a name="changes-in-release-0141"></a>Änderungen im Version 0.14.1

* Unterstützung für CTP 3.1-Datenquellen

## <a name="changes-in-release-0121"></a>Änderungen in Version 0.12.1

* Der Verbindungstyp **SQL Server big data cluster** (Big-Data-Cluster für SQL Server) wurde aus dieser Version entfernt. Alle Funktionen, die zuvor über die Verbindung mit dem Big-Data-Cluster für SQL Server zur Verfügung standen, sind jetzt Teil der SQL Server-Verbindung.
* Die HDFS-Suche ist über den Ordner **Data Services** möglich.
* Bei Notebooks funktionieren PySpark und andere Big Data-Kernel, wenn eine Verbindung mit der SQL Server-Masterinstanz im Big Data-Cluster für SQL Server besteht.
* Assistent für die Erstellung externer Tabellen:
  * Das Erstellen externer Tabelle mithilfe von vorhandenen externen Datenquellen wird nun unterstützt.
  * Die Leistung verschiedener Funktionen des Assistenten wurde verbessert.
  * Die Verarbeitung von Objektnamen mit Sonderzeichen wurde verbessert. In einigen Fällen wurden dabei Fehler im Assistenten ausgelöst.
  * Die Zuverlässigkeit der Seite „Object Mapping“ (Objektzuordnung) wurde verbessert.
  * Systemdatenbanken(`DWConfiguration`, `DWDiagnostics`, `DWQueue`) wurden aus der Dropdownliste der Datenbanken entfernt.
  * Der Name des externen Dateiformatobjekts im Assistenten für die **Erstellung externer Tabellen aus CSV-Dateien** kann jetzt festgelegt werden.
  * Die Schaltfläche „Aktualisieren“ wurde zur ersten Seite des Assistenten für die **Erstellung externer Tabellen aus CSV-Dateien** hinzugefügt.

## <a name="release-notes-v0110"></a>Hinweise zu Version 0.11.0

* Jupyter Notebook (insbesondere die Kernel für Python3 und Spark) wird nun in Azure Data Studio unterstützt. Diese Erweiterung ist nicht mehr erforderlich, damit Notebooks verwendet werden kann.
* Es wurden mehrere Fehlerbehebungen der Assistenten für externe Daten behoben:
  * Die Typzuordnungen für Oracle wurden entsprechend den Änderungen aktualisiert, die an SQL Server 2019 CTP 2.3 vorgenommen wurden.
  * Behoben: Neue Schemas, die in die Steuerelemente für die Tabellenzuordnung eingegeben wurden, gehen verloren.
  * Behoben: Beim Überprüfen eines Datenbankknotens in den Tabellenzuordnungen wurden nicht alle Tabellen und Ansichten berücksichtigt.

## <a name="release-notes-v0102"></a>Hinweise zu Version 0.10.2

### <a name="sql-server-2019-support"></a>Unterstützung für SQL Server 2019

Die Unterstützung für SQL Server 2019 wurde aktualisiert. Nach dem Herstellen einer Verbindung mit einer Big Data-Clusterinstanz für SQL Server wird ein neuer _Data Services_-Ordner in der Explorer-Strukturansicht angezeigt. Der Ordner enthält Startpunkte für Aktionen wie das Öffnen eines neuen Notebooks für die Verbindung, das Übermitteln von Spark-Aufträgen und das Arbeiten mit HDFS. Für einige Aktionen wie _Create External Data_ (Erstellen externer Daten) über eine HDFS-Datei bzw. einen HDFS-Ordner muss die Erweiterung für _SQL Server 2019_ installiert werden.

### <a name="notebook-support"></a>Notebook-Unterstützung

Wir haben wichtige Updates an der Benutzeroberfläche für Notebooks vorgenommen. Der Fokus liegt darauf, dass freigegebene Notebooks einfach gelesen werden können. Deshalb werden die Konturen um Zellen jetzt nur noch angezeigt, wenn mit dem Mauszeiger darauf gezeigt oder auf diese geklickt wird. Außerdem wurde Hoversupport für einfache Aktionen auf Zellenebene hinzugefügt, sodass nun nicht mehr auf die Zellen geklickt werden muss, und der Ausführungsstatus ist nun genauer, da u. a. eine Ausführungsanzahl und eine animierte Schaltfläche _zum Beenden der Ausführung_ hinzugefügt wurde. Zudem wurden Tastenkombinationen für die Optionen _New Notebook_ (Neues Notebook, `Ctrl+Shift+N`), _Run Cell_ (Zelle ausführen, `F5`), _New Code Cell_ (Neue Codezelle, `Ctrl+Shift+C`) und _New Text Cell_ (Neue Textzelle, `Ctrl+Shift+T`) hinzugefügt. Alle wichtigen Aktionen sollen über Tastenkombinationen gestartet werden können. Senden Sie uns gern Feedback zu noch fehlenden Tastenkombinationen.

Weitere Verbesserungen und Fehlerbehebungen:

* Die Benutzer der Erweiterung für _SQL Server 2019_ werden jetzt dazu aufgefordert, ein Installationsverzeichnis für Python-Abhängigkeiten auszuwählen. Außerdem ist Python nicht mehr in der `.vsix file` enthalten, wodurch die Gesamtgröße der Erweiterung verringert wird. Die Python-Abhängigkeiten unterstützen Kernel für Spark und Python3.
* Neue Notebooks können nun über die Befehlszeile gestartet werden. Über die Argumente `--command=notebook.command.new --server=myservername` sollten Sie ein neues Notebook öffnen und eine Verbindung mit diesem Server herstellen können.
* Es wurden Leistungskorrekturen für Notebooks mit sehr langen Codezellen vorgenommen. Bei Codezellen mit mehr als 250 Zeilen wird eine Scrollleiste hinzugefügt.
* Verbesserte Unterstützung von `.ipynb`-Dateien. Es werden jetzt alle Versionen ab Version 3 unterstützt. 

    >[!Note]
    > Dateiupdates auf Version 4 oder höher werden gespeichert.

* Die Benutzereinstellung `notebook.enabled` wurde entfernt, da der integrierte Notebookviewer jetzt stabil ist.
* Das Design mit hohem Kontrast wird nun mit einer Reihe von Korrekturen des Objektlayouts unterstützt.
* Fehler 3680 wurde behoben: Ausgaben enthielten manchmal eine Reihe von falschen `,,,`-Zeichen.
* Fehler 3602 wurde behoben: Der Editor wurde für Zellen ausgeblendet, wenn aus Azure Data Studio navigiert wurde.
* Die Verwendung von Rasteransichten für den MIME-Ausgabetyp `application/vnd.dataresource+json` wurde hinzugefügt. Dies bedeutet, dass viele Notebooks, die diese verwenden (z. B. durch Festlegen von `pd.options.display.html.table_schema` in einem Python-Notebook), eine bessere Tabellenausgabe erhalten.

#### <a name="known-issues"></a>Bekannte Probleme

* Wenn ein Notebook geöffnet wird, wird das Dialogfeld „Install Python“ (Python installieren) angezeigt. Wenn diese Installation abgebrochen wird, zeigen die Kernel und die Dropdownlisten unter „Anfügen an“ nicht die erwarteten Werte an. Sie können das Problem umgehen, indem Sie die Python-Installation abschließen.
* Wenn ein Notebook mit einem nicht unterstützten Kernel geöffnet wird, kann die Verwendung der Dropdownlisten für die Kernel oder für die Option _Anfügen an_ dazu führen, dass Azure Data Studio nicht mehr reagiert. Schließen Sie Azure Data Studio, und stellen Sie sicher, dass Sie einen unterstützten Kernel verwenden (Python3, Spark | R, Spark | Scala, Pyspark, PySpark3).
* Der Link zur Spark-Benutzeroberfläche löst einen Fehler aus, wenn PySpark3 oder andere Spark-Kernel für den SQL Server-Endpunkt verwendet werden. Sie können dieses Problem umgehen, indem Sie auf dem Dashboard auf „Spark UI“ (Spark-Benutzeroberfläche) klicken oder eine Verbindung über den Verbindungstyp „SQL Server Big Data Cluster“ (Big Data-Cluster für SQL Server) herstellen. Dabei sollte automatisch der richtige Link zur Spark-Benutzeroberfläche verwendet werden.

### <a name="extensibility-improvements"></a>Verbesserungen der Erweiterbarkeit

In diesem Release wurde eine Reihe von Verbesserungen zur Extenderunterstützung hinzugefügt.

* Eine neue `ObjectExplorerNodeProvider`-API ermöglicht es Erweiterungen, Ordner zu SQL Server-Instanzen oder anderen Verbindungsknoten hinzuzufügen. Auf diese Weise wird der Knoten `Data Services` zu SQL Server 2019-Instanzen hinzugefügt. Dieser kann jedoch auch verwendet werden, um problemlos Ordner zur Überwachung oder andere Ordner zur Benutzeroberfläche hinzuzufügen.
* Es sind zwei neue Kontextschlüsselwerte verfügbar, mit denen Sie Dashboardbeiträge anzeigen bzw. ausblenden können.
  * `mssql:iscluster` gibt an, ob es sich um ein Big-Data-Cluster für SQL Server 2019 handelt.
  * `mssql:servermajorversion` gibt die Serverversion an (15 für SQL Server 2019, 14 für SQL Server 2017 usw.). Dies kann hilfreich sein, wenn beispielsweise bestimmte Features nur für SQL Server 2017 oder höher angezeigt werden sollen.

## <a name="release-notes-v080"></a>Hinweise zu Version 0.8.0

*Notebooks:*

* Das Hinzufügen von Zellen vor oder nach bereits vorhandenen Zellen wird nun durch Auswählen der Schaltfläche „Weitere Aktionen“ der Zelle unterstützt.
* Die Option **Neue Verbindung hinzufügen** wurde zu den Verbindungen hinzugefügt, die in der Dropdownliste „Anfügen an“ enthalten sind.
* Der Befehl **Reinstall Notebook Dependencies** (Notebookabhängigkeiten erneut installieren) wurde hinzugefügt, um Paketupdates für Python zu unterstützen und Probleme zu beheben, bei denen die Installation durch das Schließen der Anwendung abgebrochen wurde. Sie können diesen Befehl über die Befehlspalette ausführen (verwenden Sie dafür die Tastenkombination `Ctrl/Cmd+Shift+P`, und geben Sie `Reinstall Notebook Dependencies` ein).
* Das Python-Paket „PROSE“ wurde auf die Version 1.1.0 aktualisiert, mit der eine Reihe von Fehlern behoben wird. Verwenden Sie den Befehl **Reinstall Notebook Dependencies** (Notebookabhängigkeiten erneut installieren), um dieses Paket zu aktualisieren.
* Der Befehl **Ausgabe löschen** wird jetzt unterstützt, wenn Sie die Zellenschaltfläche **Weitere Aktionen** auswählen.
* Die folgenden von Kunden gemeldeten Probleme wurden behoben:
  * Die Notebooksitzung konnte aufgrund von PATH-Problemen nicht unter Windows gestartet werden.
  * Das Notebook konnte nicht über den Stammordner eines Laufwerks (z. B. „C:\“ oder „D:\“) gestartet werden.
  * [2820](https://github.com/Microsoft/azuredatastudio/issues/2820): Über ADS in VS Code erstellte Notebooks können nicht bearbeitet werden.
  * Der Link zur Spark-Benutzeroberfläche funktioniert nun, wenn ein Spark-Kernel ausgeführt wird.
  * Die Option „Managed Packages“ (Verwaltete Pakete) wurde in „Install Packages“ (Pakete installieren) umbenannt.

*Create External Data* (Externe Daten erstellen):

* Fehlermeldungen können kopiert werden und wurden der Einfachheit halber in eine Zusammenfassung und eine detaillierte Ansicht aufgeteilt.
* Das Layout der Benutzeroberflächen, die Zuverlässigkeit und die Fehlerbehandlung wurden verbessert.
* Die folgenden von Kunden gemeldeten Probleme wurden behoben:
  * Tabellen mit ungültigen Spaltenzuordnungen werden als „Deaktiviert“ angezeigt, und der Fehler wird in einer Warnung erläutert.

## <a name="release-notes-v072"></a>Hinweise zu Version 0.7.2

* Der Azure-Ressourcen-Explorer ist nun in Azure Data Studio integriert und wurde aus dieser Erweiterung entfernt. Vielen Dank für Ihr Feedback!
* Die Leistung von Notebooks wurde mithilfe mehrerer Markdownzellen verbessert.
* Die Größe von Codezellen in Notebooks wird automatisch angepasst. Anhand der Zellensymbolleiste wird eine Mindestgröße festgelegt.
* Benachrichtigen Sie den Benutzer, wenn Sie Notebookabhängigkeiten installieren. Insbesondere unter Windows kann dies einige Zeit in Anspruch nehmen, weshalb Benachrichtigungen jetzt in der Ansicht „Tasks“ (Aufgaben) angezeigt werden.
* Die erneute Installation von Notebookabhängigkeiten wird jetzt unterstützt. Dies ist hilfreich, wenn der Benutzer zuvor Azure Data Studio vor dem Abschluss der Installation geschlossen hat.
* Die Zellenausführung in Notebooks kann nun abgebrochen werden.
* Die Zuverlässigkeit bei der Verwendung des Assistenten für die Erstellung externer Tabellen wurde insbesondere bei Verbindungsfehlern verbessert.
* Die Verwendung des Assistenten für die Erstellung externer Tabellen wird blockiert, wenn PolyBase nicht aktiviert ist oder nicht auf dem Zielserver ausgeführt wird.
* Fehler in Bezug auf die Rechtschreibung und Benennung im Zusammenhang mit SQL Server 2019 und dem Assistenten für die Erstellung externer Tabellen wurden behoben.
* Es wurde mehrere Fehler aus der Debugging-Konsole für Azure Data Studio entfernt.