---
description: MSSQLSERVER_5120
title: MSSQLSERVER_5120
ms.custom: ''
ms.date: 07/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5120 (Database Engine error)
ms.assetid: ''
author: PijoCoder
ms.author: mathoma
ms.openlocfilehash: 7e9ec54be052303c0b350a0037aa3f2ee9caa0d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348755"
---
# <a name="mssqlserver_5120"></a>MSSQLSERVER_5120
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|SQL Server|  
|Ereignis-ID|5120|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|DSK_FCB_FAILURE|  
|Meldungstext|Tabellenfehler: Die physische Datei '%.*ls' kann nicht geöffnet werden. Betriebssystemfehler %d: '%ls'.|  
  
## <a name="explanation"></a>Erklärung  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] konnte eine Datenbankdatei nicht öffnen.  Der in der Meldung angegebene Betriebssystemfehler verweist auf die zugrunde liegenden spezifischeren Ursachen des Fehlers. Dieser Fehler tritt häufig in Kombination mit anderen Fehlern wie [17204](mssqlserver-17204-database-engine-error.md) oder [17207](mssqlserver-17207-database-engine-error.md) auf.
  
## <a name="user-action"></a>Benutzeraktion  
  
  Diagnostizieren und korrigieren Sie den Betriebssystemfehler, und wiederholen Sie dann den Vorgang. Es gibt mehrere Zustände, die Microsoft dabei helfen können, den Bereich im Produkt einzugrenzen, in dem der Fehler auftritt. 
  
### <a name="access-is-denied"></a>Zugriff verweigert 
Wenn Sie den `Access is Denied`-Betriebssystemfehler „5“ empfangen, ziehen Sie diese Methoden in Erwägung:
   -  Überprüfen Sie die für die Datei festgelegten Berechtigungen, indem Sie die Eigenschaften der Datei im Windows-Explorer untersuchen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet Windows-Gruppen, um Zugriffssteuerung für die verschiedenen Dateiressourcen bereitzustellen. Vergewissern Sie sich, dass die entsprechende Gruppe [mit Namen wie SQLServerMSSQLUser$ComputerName$MSSQLSERVER oder SQLServerMSSQLUser$ComputerName$InstanceName] über die erforderlichen Berechtigungen für die Datenbankdatei verfügt, die in der Fehlermeldung genannt wird. Weitere Informationen finden Sie unter [Konfigurieren von Dateisystemberechtigungen für den Datenbank-Engine-Zugriff](/previous-versions/sql/2014/database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access?view=sql-server-2014&preserve-view=true). Achten Sie darauf, dass die Windows-Gruppe das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienststartkonto oder die Dienst-SID tatsächlich enthält.
   -  Überprüfen Sie das Benutzerkonto, unter dem der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienst aktuell ausgeführt wird. Sie können diese Informationen mithilfe des Windows Task-Managers abrufen. Suchen Sie nach dem Wert für „Benutzername“ der „sqlservr.exe“-Programmdatei. Wenn Sie vor Kurzem das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienstkonto geändert haben, berücksichtigen Sie außerdem, dass bei der unterstützten Durchführung dieses Vorgangs das [SQL Server-Konfigurations-Manager](../sql-server-configuration-manager.md)-Hilfsprogramm verwendet wird. 
   -  Das Konto, das für den Identitätswechsel und den Zugriff auf die Datenbankdatei verwendet wird, kann je nach Typ des Vorgangs (Öffnen von Datenbanken während des Serverstarts, Anfügen einer Datenbank, Datenbankwiederherstellung usw.) variieren. Arbeiten Sie das Thema [Sichern von Daten- und Protokolldateien](/previous-versions/sql/sql-server-2008-r2/ms189128(v=sql.105)) durch, um zu verstehen, welcher Vorgang welche Berechtigungen für welche Konten festlegt. Verwenden Sie ein Tool wie den [Prozessmonitor](/sysinternals/downloads/procmon) von Windows SysInternals, um zu verstehen, ob der Dateizugriff unter dem Sicherheitskontext des Dienststartkontos der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz [oder Dienst-SID] oder einem Konto mit Identitätswechsel erfolgt.

      Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Benutzeranmeldeinformationen der Anmeldung, mit der der ALTER DATABASE- oder CREATE DATABASE-Vorgang ausgeführt wird, durch Identitätswechsel darstellt, werden im Prozessmonitor-Tool beispielsweise die folgenden Informationen angezeigt:
      
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
  
  
### <a name="attaching-files-that-reside-on-a-network-attached-storage"></a>Anfügen von Dateien, die sich in einem Network Attached Storage (NAS) befinden  
Wenn Sie eine Datenbank, die sich in einem NAS befindet, nicht erneut anfügen können, wird möglicherweise eine Meldung wie die folgende im Anwendungsprotokoll protokolliert:

`Msg 5120, Level 16, State 101, Line 1 Unable to open the physical file "\\servername\sharename\filename.mdf". Operating system error 5: (Access is denied.).`

Dieses Problem tritt auf, weil [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Dateiberechtigungen zurücksetzt, wenn die Datenbank getrennt wird. Beim Versuch, die Datenbank erneut anzufügen, tritt aufgrund von eingeschränkten Freigabeberechtigungen ein Fehler auf.

Führen Sie die folgenden Schritte aus, um das Problem zu beheben:
1. Verwenden Sie zum Starten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Startoption -T. Verwenden Sie diese Startoption, um im [SQL Server-Konfigurations-Manager](../sql-server-configuration-manager.md) das Ablaufverfolgungsflag 1802 zu aktivieren (Informationen zu 1802 finden Sie unter [Ablaufverfolgungsflags](../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md)). Weitere Informationen zum Ändern der Startparameter finden Sie unter [Startoptionen für den Datenbank-Engine-Dienst](../../database-engine/configure-windows/database-engine-service-startup-options.md).

2. Verwenden Sie zum Trennen der Datenbank den folgenden Befehl:
   ```sql
    exec sp_detach_db DatabaseName
    go 
   ```

3. Verwenden Sie den folgenden Befehl, um die Datenbank erneut anzufügen.
   ```sql
   exec sp_attach_db DatabaseName, '\\Network-attached storage_Path\DatabaseMDFFile.mdf', '\\Network-attached storage_Path\DatabaseLDFFile.ldf'
   go
   ```
