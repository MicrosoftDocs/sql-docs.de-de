---
title: Bereitstellen von R-Code in gespeicherten Prozeduren
description: Betten Sie R-Sprachcode in eine gespeicherte Prozedur von SQL Server ein, um ihn für alle Clientanwendungen mit Zugriff auf eine SQL Server-Datenbank verfügbar zu machen.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: a959bf5c29a125aefb390bef1428e6a35745e665
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353754"
---
# <a name="operationalize-r-code-using-stored-procedures-in-sql-server-machine-learning-services"></a>Operationalisieren von R-Code mithilfe von gespeicherten Prozeduren in SQL Server-Machine Learning Services
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Bei der Verwendung von R- und Python-Funktionen in SQL Server-Machine Learning Services ist die häufigste Methode zum Verschieben von Lösungen in eine Produktionsumgebung das Einbetten von Code in gespeicherten Prozeduren. In diesem Artikel werden die wichtigsten Punkte zusammengefasst, die SQL-Entwickler beim Operationalisieren von R-Code mithilfe von SQL Server beachten müssen.

## <a name="deploy-production-ready-script-using-t-sql-and-stored-procedures"></a>Bereitstellen eines produktionsbereiten Skripts mithilfe von T-SQL und gespeicherten Prozeduren

In der Regel erfordert die Integration von Data Science-Lösungen eine umfangreiche Neuprogrammierung zur Unterstützung von Leistung und Integration. Diese Aufgabe wird mit SQL Server-Machine Learning Services vereinfacht, da R- und Python-Code in SQL Server ausgeführt und mithilfe von gespeicherten Prozeduren aufgerufen werden können. Weitere Informationen zur Einbettung von Code in gespeicherten Prozeduren finden Sie unter:

+ [Erstellen und Ausführen einfacher R-Skripts in SQL Server](../tutorials/quickstart-r-create-script.md)
+ [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)

Ein umfangreicheres Beispiel für die Bereitstellung von R-Code in der Produktion mithilfe von gespeicherten Prozeduren finden Sie unter [R-Tutorial: Vorhersagen von Taxi-Fahrpreisen in New York City mit binärer Klassifizierung](../tutorials/r-taxi-classification-introduction.md).

## <a name="guidelines-for-optimizing-r-code-for-sql"></a>Richtlinien zur Optimierung von R-Code für SQL

Die Konvertierung von R-Code in SQL gestaltet sich einfacher, wenn der R- oder Python-Code zuvor optimiert wird. Dazu gehört das Vermeiden von Datentypen, die Probleme verursachen, das Vermeiden von unnötigen Datenkonvertierungen sowie das Umschreiben des R-Codes in einen einzelnen Funktionsaufruf, der leicht parametrisiert werden kann. Weitere Informationen finden Sie unter

+ [R-Bibliotheken und -Datentypen](r-libraries-and-data-types.md)
+ [Verwenden von sqlrutils-Hilfsfunktionen](ref-r-sqlrutils.md)

## <a name="integrate-r-and-python-with-applications"></a>Integrieren von R und Python in Anwendungen

Da R oder Python aus einer gespeicherten Prozedur ausgeführt werden kann, können Sie Skripts aus jeder beliebigen Anwendung ausführen, die eine T-SQL-Anweisung senden und die Ergebnisse verarbeiten kann. Beispielsweise können Sie ein Modell nach einem Zeitplan erneut trainieren, indem Sie die Aufgabe [T-SQL-Anweisung ausführen](../../integration-services/control-flow/execute-t-sql-statement-task.md) in Integration Services oder einen anderen Auftragsplaner verwenden, der eine gespeicherte Prozedur ausführen kann.

Die Bewertung ist eine wichtige Aufgabe, die auf einfache Weise automatisiert oder aus externen Anwendungen gestartet werden kann. Das Modell wird im Voraus mit R, Python oder einer gespeicherten Prozedur trainiert und in einer Tabelle [im Binärformat gespeichert](../tutorials/walkthrough-build-and-save-the-model.md). Anschließend kann das Modell als Teil des Aufrufs einer gespeicherten Prozedur in eine Variable geladen werden, wobei eine der folgenden T-SQL-Optionen für die Bewertung verwendet wird:

+ Echtzeitbewertung (für kleine Batches optimiert)
+ Bewertung einer einzelnen Zeile (für Aufrufe aus einer Anwendung)
+ [Native Bewertung](../predictions/native-scoring-predict-transact-sql.md) (für eine schnelle Batchvorhersage in SQL Server ohne Aufruf von R)

Das folgende Tutorial enthält ein Beispiel für die Bewertung mithilfe einer gespeicherten Prozedur sowohl im Batchmodus als auch für eine einzelne Zeile:

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ [Exemplarische End-to-End-Vorgehensweise zu Data Science für R in SQL Server](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
+ [R-Tutorial: Vorhersagen von Taxi-Fahrpreisen in New York City mit binärer Klassifizierung](../tutorials/r-taxi-classification-introduction.md)
::: moniker-end

## <a name="boost-performance-and-scale"></a>Steigern von Leistung und Skalierung

Obwohl die Open-Source-Sprache R bekannte Einschränkungen in Bezug auf große Datasets aufweist, können die in SQL Server-Machine Learning Services enthaltenen [RevoScaleR-Paket-APIs](ref-r-revoscaler.md) mit großen Datasets ausgeführt werden und von datenbankinternen Berechnungen mithilfe mehrerer Threads, Kerne und Prozesse profitieren.

Wenn Ihre R-Lösung komplexe Aggregationen verwendet oder große Datasets enthält, können Sie die hocheffizienten In-Memory-Aggregationen und die Columnstore-Indizes von SQL Server nutzen und die statistischen Berechnungen und Bewertungen mit R-Code verarbeiten.

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"

## <a name="adapt-r-code-for-other-platforms-or-compute-contexts"></a>Anpassen von R-Code für andere Plattformen oder Computekontexte

Den R-Code, den Sie für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Daten ausführen, können Sie auch für andere Datenquellen verwenden, wie z. B. Spark mit HDFS, wenn Sie beim SQL Server-Setup die Option [Eigenständiger Server](../install/sql-machine-learning-standalone-windows-install.md) aktivieren oder das Nicht-SQL-Markenprodukt Microsoft Machine Learning Server (früher **Microsoft R Server**) verwenden:

+ [Dokumentation zu Machine Learning Server](/r-server/)

::: moniker-end
