---
title: Häufige Skripterstellungsfehler in R
description: In diesem Artikel werden mehrere häufige Skripterstellungsfehler beschrieben, die möglicherweise beim Ausführen von R-Code in SQL Server auftreten.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 05/31/2018
ms.topic: troubleshooting
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 0f9f086ec09830e65f85ca2df5232e8b401cb9eb
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186414"
---
# <a name="common-r-scripting-errors-in-sql-server"></a>Häufige R-Skripterstellungsfehler in SQL Server
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

In diesem Artikel werden mehrere häufige Skripterstellungsfehler beschrieben, die beim Ausführen von R-Code in SQL Server auftreten können. Es handelt sich dabei aber nicht um eine vollständige Liste. Es gibt viele Pakete, und in den verschiedenen Versionen desselben Pakets können unterschiedliche Fehler auftreten.

## <a name="valid-script-fails-in-t-sql-or-in-stored-procedures"></a>Gültiges Skript schlägt in T-SQL oder in gespeicherten Prozeduren fehl.

Bevor Sie einen R-Code in einer gespeicherten Prozedur umbrechen, empfiehlt es sich, den R-Code in einer externen IDE oder in einem der R-Tools wie RTerm oder RGui auszuführen. Mithilfe dieser Methoden können Sie den Code testen und debuggen, indem Sie die detaillierten Fehlermeldungen verwenden, die von R zurückgegeben werden.

Manchmal kann es jedoch vorkommen, dass Code, der in einer externen IDE oder einem Dienstprogramm einwandfrei funktioniert, in einer gespeicherten Prozedur oder in einem SQL Server-Computekontext jedoch nicht ausgeführt werden kann. In diesem Fall müssen Sie zunächst eine Vielzahl von Problemen überprüfen, bevor Sie davon ausgehen können, dass das Paket in SQL Server nicht funktioniert.

1. Überprüfen Sie, ob Launchpad ausgeführt wird.

2. Überprüfen Sie die Meldungen, um festzustellen, ob die Eingabe- oder Ausgabedaten Spalten nicht kompatible oder nicht unterstützte Datentypen enthalten. Beispielsweise geben Abfragen für eine SQL-Datenbank häufig GUIDs oder RowGUIDs zurück, die beide nicht unterstützt werden. Weitere Informationen finden Sie unter [R-Bibliotheken und Datentypen](../r/r-libraries-and-data-types.md).

3. Lesen Sie die Hilfeseiten für einzelne R-Funktionen, um zu bestimmen, ob alle Parameter für den SQL Server-Computekontext unterstützt werden. Verwenden Sie für die ScaleR-Hilfe die Inline-R-Hilfebefehle, oder lesen Sie die Informationen unter [Referenz zu Paketen](/r-server/r-reference/revoscaler/revoscaler).

Wenn die R-Laufzeit funktionsfähig ist, das Skript jedoch Fehler zurückgibt, empfiehlt es sich, das Skript in einer dedizierten R-Entwicklungsumgebung wie R Tools für Visual Studio zu debuggen.

Außerdem wird empfohlen, das Skript zu überprüfen und leicht umzuschreiben, um Probleme mit Datentypen zu vermeiden, die beim Verschieben von Daten zwischen R und der Datenbank-Engine auftreten können. Weitere Informationen finden Sie unter [R-Bibliotheken und Datentypen](../r/r-libraries-and-data-types.md).

Darüber hinaus können Sie das sqlrutils-Paket verwenden, um Ihr R-Skript in einem Format zu bündeln, das einfacher als gespeicherte Prozedur verwendet werden kann. Weitere Informationen finden Sie unter
* [sqlrutils-Paket](../r/ref-r-sqlrutils.md)
* [Erstellen einer gespeicherten Prozedur mithilfe von sqlrutils](/machine-learning-server/r-reference/sqlrutils/how-to-create-a-stored-procedure-from-r)

## <a name="script-returns-inconsistent-results"></a>Skript gibt inkonsistente Ergebnisse zurück

R-Skripts können aus unterschiedlichen Gründen verschiedene Werte in einem SQL Server-Kontext zurückgeben:

- Die implizite Typkonvertierung wird für einige Datentypen automatisch ausgeführt, wenn die Daten zwischen SQL Server und R übertragen werden. Weitere Informationen finden Sie unter [R-Bibliotheken und-Datentypen](../r/r-libraries-and-data-types.md).

- Stellen Sie fest, ob Bitanzahl ein Faktor ist. Beispielsweise gibt es häufig Unterschiede in den Ergebnissen von mathematischen Vorgängen für 32-Bit- und 64-Bit-Gleitkommabibliotheken.

- Stellen Sie fest, ob NaNs in einem Vorgang erstellt wurden. Dadurch können die Ergebnisse ungültig werden.

- Kleine Unterschiede können vergrößert werden, wenn Sie einen Kehrwert von einer Zahl nahe Null (0) nehmen.

- Kumulierte Rundungsfehler können beispielsweise Werte kleiner als Null () statt Null verursachen.

## <a name="implied-authentication-for-remote-execution-via-odbc"></a>Implizite Authentifizierung für die Remoteausführung über ODBC

Wenn Sie eine Verbindung zum [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Computer herstellen, um R-Befehle mit den **RevoScaleR**-Funktionen auszuführen, wird möglicherweise eine Fehlermeldung angezeigt, wenn ODBC-Aufrufe verwendet werden, die Daten auf den Server schreiben. Dieser Fehler tritt nur auf, wenn Sie die Windows-Authentifizierung verwenden.

Der Grund hierfür ist, dass die für R Services erstellten Workerkonten nicht über die Berechtigung zum Herstellen einer Verbindung mit dem Server verfügen. Daher können ODBC-Aufrufe nicht in Ihrem Namen ausgeführt werden. Das Problem tritt nicht bei SQL-Anmeldungen auf, weil die Anmeldeinformationen bei SQL-Anmeldungen explizit vom R-Client an die SQL Server-Instanz und dann an ODBC weitergegeben werden. Allerdings sind SQL-Anmeldungen auch weniger sicher als die Verwendung der Windows-Authentifizierung.

Um die sichere Übermittlung der Windows-Anmeldeinformationen aus einem Skript zu ermöglichen, das remote initiiert wird, muss SQL Server die Anmeldeinformationen emulieren. Dieser Prozess wird als _implizierte Authentifizierung_ bezeichnet. Voraussetzung dafür ist, dass die Workerkonten, die R- oder Python-Skripte auf dem SQL Server-Computer ausführen, über die richtigen Berechtigungen verfügen.

1. Öffnen Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] als Administrator auf der Instanz, auf der Sie R-Code ausführen möchten.

2. Führen Sie das folgende Skript aus. Achten Sie darauf, den Namen der Benutzergruppe zu bearbeiten, wenn Sie die Standardeinstellung geändert haben. Bearbeiten Sie außerdem den Namen des Computers und der Instanz.

    ```sql
    USE [master]
    GO
    
    CREATE LOGIN [computername\\SQLRUserGroup] FROM WINDOWS WITH
    DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE=[language]
    GO
    ```

## <a name="avoid-clearing-the-workspace-while-youre-running-r-in-a-sql-compute-context"></a>Vermeiden Sie das Löschen des Arbeitsbereichs während der Ausführung von R in einem SQL-Computekontext.

Das Löschen des Arbeitsbereichs ist bei der Arbeit auf der R-Konsole zwar üblich, kann aber zu unerwarteten Ergebnissen in einem SQL-Computekontexts führen.

`revoScriptConnection` ist ein Objekt im R-Arbeitsbereich, das Informationen über eine R-Sitzung enthält, die aus SQL Server aufgerufen wird. Wenn Ihr R-Code jedoch einen Befehl zum Löschen des Arbeitsbereichs enthält (wie z. B. `rm(list=ls())`), werden alle Informationen über die Sitzung und andere Objekte im R-Arbeitsbereich ebenfalls gelöscht.

Um dieses Problem zu umgehen, sollten Sie willkürliches Löschen von Variablen und anderen Objekten während der Ausführung von R in SQL Server vermeiden. Sie können bestimmte Variablen löschen, indem Sie die Funktion **remove** verwenden:

```R
remove('name1', 'name2', ...)
```

Wenn mehrere zu löschende Variablen vorhanden sind, wird empfohlen, die Namen temporärer Variablen in einer Liste zu speichern und eine regelmäßige automatische Speicherbereinigung dieser Liste durchzuführen.



## <a name="next-steps"></a>Nächste Schritte

[Machine Learning Services – Problembehandlung und bekannte Probleme](machine-learning-troubleshooting-overview.md)

[Datensammlung zur Behandlung von Machine Learning-Problemen](data-collection-ml-troubleshooting-process.md)

[Installieren von SQL Server-Machine Learning Services](../install/sql-machine-learning-services-windows-install.md)

[Problembehandlung bei Datenbank-Engine-Verbindungen](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)