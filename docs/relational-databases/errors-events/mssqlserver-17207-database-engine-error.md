---
description: MSSQLSERVER_17207
title: MSSQLSERVER_17207
ms.custom: ''
ms.date: 07/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17204 (Database Engine error)
ms.assetid: ''
author: PijoCoder
ms.author: mathoma
ms.openlocfilehash: b9e39ff2fd02cfde0d4e4a825a0be06b258b3ee3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349022"
---
# <a name="mssqlserver_17207"></a>MSSQLSERVER_17207
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |
| :-------- | :---- |
|Produktname|SQL Server|  
|Ereignis-ID|17207|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|DBLKIO_OS2DISKERROR|  
|Meldungstext|%ls: Betriebssystemfehler %ls beim Erstellen oder Öffnen der Datei "%ls". Diagnostizieren und korrigieren Sie den Betriebssystemfehler, und wiederholen Sie den Vorgang.|  


## <a name="explanation"></a>Erklärung  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] konnte die angegebene Datei aufgrund des angegebenen Betriebssystemfehlers nicht öffnen.  

Möglicherweise wird im Windows-Anwendungsereignis- oder im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll Fehler 17207 verzeichnet, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine Datenbank und/oder Dateien des Transaktionsprotokolls nicht öffnen kann. Hier sehen Sie ein Beispiel dafür, wie dieser Fehler aussehen kann.

``` 
Error: 17207, Severity: 16, State: 1.
FileMgr::StartSecondaryDataFiles: Operating system error 2(The system cannot find the file specified.) occurred while creating or opening file 'F:\MSSQL\DATA\MyDB_FG1_1.ndf'. Diagnose and correct the operating system error, and retry the operation.
```

Möglicherweise begegnen Ihnen diese Fehler während des Startvorgangs der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz oder während allen Datenbankvorgängen, die versuchen, die Datenbank zu starten (z. B. ALTER DATABASE). In manchen Szenarien können sowohl 17207- als auch 17204-Fehler auftreten, und bei anderen Gelegenheiten sehen Sie nur einen von beiden.

Wenn diese Fehler bei einer Benutzerdatenbank auftreten, verbleibt die Datenbank im Status RECOVERY_PENDING, und Anwendungen können nicht auf die Datenbank zugreifen. Wenn diese Fehler bei einer Systemdatenbank auftreten, wird die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz nicht gestartet, und Sie können keine Verbindung mit dieser Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Dies könnte auch dazu führen, dass eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failoverclusterressource in den Offlinemodus versetzt wird.

Wenn das Problem mit Ihrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] FileStream-Dateigruppe zusammenhängt, werden Sie feststellen, dass lediglich der vollständige Verzeichnispfad anstelle eines Dateinamens aufgeführt ist. Beispiel: 
```
Error: 17207, Severity: 16, State: 1.
STREAMFCB::Startup: Operating system error 2(The system cannot find the file specified.) occurred while creating or opening file 'C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\DATA\bpa_files_test_fs_1\bpa_files_test_fs_1'. Diagnose and correct the operating system error, and retry the operation.
```

## <a name="cause"></a>Ursache
Bevor eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank verwendet werden kann, muss die Datenbank gestartet werden. Der Datenbank-Startvorgang umfasst die Initialisierung verschiedener Datenstrukturen, die die Datenbank und ihre Dateien darstellen, das Öffnen aller Dateien, die zur Datenbank gehören und schließlich das Ausführen der Wiederherstellung für die Datenbank. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet die Windows-API-Funktion [CreateFile](/windows/win32/api/fileapi/nf-fileapi-createfilea), um die Dateien zu öffnen, die zu einer Datenbank gehören.
 
Die Nachrichten 17207 (und 17204) weisen darauf hin, dass ein Fehler auftrat, während [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versuchte, die Datenbankdateien während des Startvorgangs zu öffnen.
 
Diese Fehlermeldungen enthalten die folgenden Informationen:
1. Den Namen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Funktion, die versucht, die Datei zu öffnen. Der Funktionsname, den Sie normalerweise in diesen Fehlermeldungen finden, ist einer der folgenden:
   - FCB::Open                              - bei der Datei ist ein Fehler aufgetreten, als [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sie zu öffnen versuchte
   - FileMgr::StartPrimaryDataFiles         - eine primäre Datendatei oder eine Datei, die der primären Dateigruppe angehört
   - FileMgr::StartSecondaryDataFiles       - eine Datei, die zu einer sekundären Dateigruppe gehört
   - FileMgr::StartLogFiles                 - eine Transaktionsprotokolldatei
   - STREAMFCB::Startup                     - SQL FileStream-Container
   - FCB::RemoveAlternateStreams
  
      
1. In den Statusinformationen wird zwischen mehreren Stellen innerhalb einer Funktion unterschieden, die diese Fehlermeldung generieren können.
1. Der vollständige physische Pfad der Datei
1. Die Datei-ID der Datei
1. Der Betriebssystem-Fehlercode und die Fehlerbeschreibung In manchen Fällen sehen Sie nur den Fehlercode.
 
Die Betriebssystem-Fehlerinformationen die in diesen Fehlermeldungen angegeben werden, bilden die Grundursache, die zu Fehler 17204 geführt hat. Häufige Gründe für diese Fehlermeldungen sind ein Berechtigungsproblem oder ein nicht ordnungsgemäßer Dateipfad.


## <a name="user-action"></a>Benutzeraktion  
1. Voraussetzung zum Beheben von Fehler 17207 sind das Verständnis des zugehörigen Betriebssystem-Fehlercodes und die Diagnose des Fehlers. Nach dem Beheben der Betriebssystem-Fehlerbedingung können Sie versuchen, die Datenbank neu zu starten (beispielsweise mithilfe von ALTER DATABASE SET ONLINE) oder die betroffene Datenbank mithilfe der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz online zu schalten. In einigen Fällen lässt sich der Betriebssystemfehler unter Umständen nicht beheben, sodass Sie bestimmte Korrekturmaßnahmen ergreifen müssen. Wir behandeln diese Aktionen in diesem Abschnitt.
1. Wenn die 17207-Fehlermeldung nur einen Fehlercode und keine Fehlerbeschreibung enthält, können Sie versuchen, den Fehlercode mithilfe des folgenden Befehls an einer Betriebssystemshell aufzulösen: net helpmsg <error code> . Wenn Sie einen 8stelligen Statuscode als Fehlercode erhalten, können Sie auf Informationsquellen wie [How do I convert an HRESULT to a Win32 error code?](https://devblogs.microsoft.com/oldnewthing/20061103-07/?p=29133) (Gewusst wie: Konvertieren eines HRESULT-Werts in einen Win32-Fehlercode) zurückgreifen, um diese Statuscodes in Betriebssystemfehler zu decodieren.
1. Wenn Sie den ```Access is Denied```-Betriebssystemfehler „5“ empfangen, ziehen Sie diese Methoden in Erwägung:
   -  Überprüfen Sie die für die Datei festgelegten Berechtigungen, indem Sie die Dateieigenschaften im Windows-Explorer aufrufen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt die Zugriffssteuerung für die verschiedenen Dateiressourcen mithilfe von Windows-Gruppen bereit. Vergewissern Sie sich, dass die richtige Gruppe (mit einem Namen wie SQLServerMSSQLUser$ComputerName$MSSQLSERVER oder SQLServerMSSQLUser$ComputerName$InstanceName) über die erforderlichen Berechtigungen für die Datenbankdatei verfügt, die in der Fehlermeldung genannt wird. Weitere Details finden Sie unter [Konfigurieren von Dateisystemberechtigungen für den Datenbank-Engine-Zugriff](/previous-versions/sql/2014/database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access?view=sql-server-2014&preserve-view=true). Achten Sie darauf, dass die Windows-Gruppe das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienststartkonto oder die Dienst-SID tatsächlich enthält.
   -  Überprüfen Sie das Benutzerkonto, unter dem der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienst aktuell ausgeführt wird. Sie können diese Informationen mithilfe des Windows Task-Managers abrufen. Suchen Sie nach dem Wert für „Benutzername“ der „sqlservr.exe“-Programmdatei. Wenn Sie vor Kurzem das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienstkonto geändert haben, berücksichtigen Sie außerdem, dass bei der unterstützten Durchführung dieses Vorgangs das SQL Server-Konfigurations-Manager-Hilfsprogramm verwendet wird. Weitere Informationen dazu finden Sie unter [SQL Server-Konfigurations-Manager](../sql-server-configuration-manager.md). 
   -  Das Konto, das für den Identitätswechsel und den Zugriff auf die Datenbankdatei verwendet wird, kann je nach Typ des Vorgangs (Öffnen von Datenbanken während des Serverstarts, Anfügen einer Datenbank, Datenbankwiederherstellung usw.) variieren. Arbeiten Sie das Thema [Sichern von Daten- und Protokolldateien](/previous-versions/sql/sql-server-2008-r2/ms189128(v=sql.105)) durch, um zu verstehen, welcher Vorgang welche Berechtigungen für welche Konten festlegt. Mit Tools wie dem [Prozessmonitor](/sysinternals/downloads/procmon) von Windows SysInternals können Sie nachvollziehen, ob der Dateizugriff im Sicherheitskontext des Dienststartkontos der SQL Server-Instanz (oder Dienst-SID) oder eines Identitätswechselkontos erfolgt.

      Wenn SQL Server die Identität des Benutzerkontos annimmt, mit dem der ALTER DATABASE- oder CREATE DATABASE-Vorgang ausgeführt wird, finden Sie im Tool „Prozessmonitor“ die folgenden Informationen (Beispiel).

        ```
        Date & Time:      3/27/2010 8:26:08 PM
        Event Class:        File System
        Operation:          CreateFile
        Result:                ACCESS DENIED
        Path:                  C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\DATA\attach_test.mdf
        TID:                   4288
        Duration:             0.0000366
        Desired Access:Generic Read/Write
        Disposition:        Open
        Options:            Synchronous IO Non-Alert, Non-Directory File, Open No Recall
        Attributes:          N
        ShareMode:       Read
        AllocationSize:   n/a
        Impersonating: DomainName\UserName
        ```
  
1. Wenn Sie den Betriebssystemfehler 3 `The system cannot find the file specified` erhalten:
   - Überprüfen Sie den gesamten Pfad aus der Fehlermeldung.
   - Stellen Sie sicher, dass das Laufwerk und der Ordnerpfad im Windows-Explorer sichtbar und zugänglich sind.
   - Überprüfen Sie im Windows-Ereignisprotokoll, ob Probleme mit diesem Laufwerk vorliegen.
   - Wenn der Pfad falsch und diese Datenbank bereits im System vorhanden ist, können Sie den Pfad der Datenbankdatei mithilfe der Anleitungen aus dem Artikel [Verschieben von Datenbankdateien](../databases/move-database-files.md) bearbeiten. Möglicherweise müssen Sie dieses Verfahren verwenden, insbesondere bei Systemdatenbankdateien, in denen ein Fehler des Typs 17204 oder 17207 auftritt, und wenn Sie an einem Notfallwiederherstellungsszenario arbeiten, in dem die angegebenen Laufwerke nicht verfügbar sind. In diesem Thema wird auch erläutert, wie Sie den aktuellen Speicherort der verschiedenen Systemdatenbanken (master, model, tempdb, msdb und mssqlsystemresource) herausfinden können.
   - Wenn dieser Fehler angezeigt wird, weil die Datenbankdateien fehlen, müssen Sie die Datenbank aus einer gültigen Sicherung wiederherstellen:
     - Wenn die Datenbankdatei, auf die sich die Fehlermeldung bezieht, zu einer sekundären Dateigruppe gehört, können Sie diese Dateigruppe optional als offline markieren, die Datenbank online schalten und dann nur diese Dateigruppe wiederherstellen. Weitere Informationen finden Sie im OFFLINE-Abschnitt des Themas [ALTER DATABASE-Optionen für Dateien und Dateigruppen (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).
     - Wenn die Datei, die den Fehler erzeugt hat, eine Transaktionsprotokolldatei ist, sollten Sie die Abschnitte zu „FOR ATTACH“ und „FOR ATTACH_REBUILD_LOG“ im Thema [Create Database](../../t-sql/statements/create-database-transact-sql.md) lesen, um zu verstehen, wie Sie die fehlenden Transaktionsprotokolldateien neu erstellen können.
   - Stellen Sie sicher, dass ein beliebiger Datenträger oder eine beliebige Netzwerkadresse (z. B. ein iSCSI-Laufwerk) verfügbar ist, bevor [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versucht, auf die Datenbankdateien an diesen Speicherorten zuzugreifen. Bei Bedarf können Sie die erforderlichen Abhängigkeiten in der Clusterverwaltung oder im Dienststeuerungs-Manager erstellen.

1. Wenn Sie den Betriebssystemfehler 32 `The process cannot access the file because it is being used by another process` erhalten:
   - Mit Windows Sysinternals-Tools wie [Process Explorer](/sysinternals/downloads/process-explorer) oder [Handle](/sysinternals/downloads/handle) können Sie herausfinden, ob ein anderer Prozess oder Dienst eine exklusive Sperre für diese Datenbankdatei erhalten hat.
   - Verhindern Sie, dass dieser Prozess auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbankdateien zugreift. Ein gängiges Beispiel hierfür ist z. B. Antivirussoftware (weitere Informationen im folgenden [KB-Artikel](https://support.microsoft.com/help/309422/choosing-antivirus-software-for-computers-that-run-sql-server)).
   - Stellen Sie in einer Clusterumgebung sicher, dass der Prozess „sqlservr.exe“ aus dem vorherigen besitzenden Knoten tatsächlich die Handles für die Datenbankdateien freigegeben hat. Obwohl dies normalerweise nicht der Fall ist, können Fehlkonfigurationen des Clusters oder der E/A-Pfade zu solchen Problemen führen.
