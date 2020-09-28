---
description: Schedule a Job
title: Schedule a Job
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- scheduling jobs [SQL Server]
- SQL Server Agent jobs, scheduling
- jobs [SQL Server Agent], scheduling
ms.assetid: f626390a-a3df-4970-b7a7-a0529e4a109c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 1c0b22064c44505d321f4dfb7484c99335c4b676
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88418086"
---
# <a name="schedule-a-job"></a>Schedule a Job
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Thema wird beschrieben, wie ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Auftrag geplant wird.  
  
-   **Vorbereitungen:**  
  
    [Security](#Security)  
  
-   **So planen Sie einen Auftrag mit**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server Management Objects](#SMO)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
Ausführliche Informationen finden Sie unter [Implementieren der SQL Server-Agent-Sicherheit](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-create-and-attach-a-schedule-to-a-job"></a>So erstellen Sie einen Zeitplan und weisen ihn einem Auftrag zu  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]her, und erweitern Sie dann diese Instanz.  
  
2.  Erweitern Sie **SQL Server-Agent**, erweitern Sie **Aufträge**, klicken Sie mit der rechten Maustaste auf den zu planenden Auftrag, und klicken Sie auf **Eigenschaften**.  
  
3.  Wählen Sie die Seite **Zeitpläne** aus, und klicken Sie dann auf **Neu**.  
  
4.  Geben Sie in das Feld **Name** einen Namen für den neuen Zeitplan ein.  
  
5.  Deaktivieren Sie das Kontrollkästchen **Aktiviert** , wenn der Zeitplan nicht unmittelbar nach seiner Erstellung wirksam werden soll.  
  
6.  Wählen Sie für **Zeitplantyp**eine der folgenden Möglichkeiten aus:  
  
    -   Klicken Sie auf **Automatisch starten, wenn der SQL Server-Agent startet** , um den Auftrag zu starten, wenn der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Dienst gestartet wird.  
  
    -   Klicken Sie auf **Starten, wenn sich die CPUs im Leerlauf befinden** , um den Auftrag zu starten, wenn die CPUs eine Leerlaufbedingung erfüllen.  
  
    -   Klicken Sie auf **Wiederholt** , wenn ein Zeitplan wiederholt ausgeführt werden soll. Um den wiederholten Zeitplan festzulegen, vervollständigen Sie im Dialogfeld die Gruppen **Häufigkeit**, **Häufigkeit pro Tag**und **Dauer** .  
  
    -   Klicken Sie auf **Einmal** , wenn der Zeitplan nur einmal ausgeführt werden soll. Um den **Einmal** -Zeitplan festzulegen, vervollständigen Sie im Dialogfeld die Gruppe **Einmalig** .  
  
#### <a name="to-attach-a-schedule-to-a-job"></a>So weisen Sie einen Zeitplan einem Auftrag zu  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]her, und erweitern Sie dann diese Instanz.  
  
2.  Erweitern Sie **SQL Server-Agent**, erweitern Sie **Aufträge**, klicken Sie mit der rechten Maustaste auf den zu planenden Auftrag, und klicken Sie auf **Eigenschaften**.  
  
3.  Wählen Sie die Seite **Zeitpläne** aus, und klicken Sie dann auf **Auswählen**.  
  
4.  Wählen Sie den Zeitplan aus, den Sie zuweisen möchten, und klicken Sie auf **OK**.  
  
5.  Doppelklicken Sie im Dialogfeld **Auftragseigenschaften** auf den zugewiesenen Zeitplan.  
  
6.  Überprüfen Sie, ob das **Startdatum** ordnungsgemäß festgelegt ist. Falls nicht, legen Sie das Datum fest, an dem der Zeitplan starten soll, und klicken Sie auf **OK**.  
  
7.  Klicken Sie im Dialogfeld **Auftragseigenschaften** auf **OK**.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Verwenden von Transact-SQL  
  
#### <a name="to-schedule-a-job"></a>So planen Sie einen Auftrag  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde_md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    USE msdb ;  
    GO  
    -- creates a schedule named NightlyJobs.   
    -- Jobs that use this schedule execute every day when the time on the server is 01:00.   
    EXEC sp_add_schedule  
        @schedule_name = N'NightlyJobs' ,  
        @freq_type = 4,  
        @freq_interval = 1,  
        @active_start_time = 010000 ;  
    GO  
    -- attaches the schedule to the job BackupDatabase  
    EXEC sp_attach_schedule  
       @job_name = N'BackupDatabase',  
       @schedule_name = N'NightlyJobs' ;  
    GO  
    ```  
  
Weitere Informationen finden Sie unter [sp_add_schedule (Transact-SQL)](https://msdn.microsoft.com/9060aae3-3ddd-40a5-83bb-3ea7ab1ffbd7) und [sp_attach_schedule (Transact-SQL)](https://msdn.microsoft.com/80c80eaf-cf23-4ed8-b8dd-65fe59830dd1).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Verwendung von SQL Server Management Objects  
Verwenden Sie die **JobSchedule** -Klasse, indem Sie eine von Ihnen ausgewählte Programmiersprache, z. B. Visual Basic, Visual C# oder PowerShell verwenden. Weitere Informationen finden Sie unter[SQL Server Management Objects (SMO)](https://msdn.microsoft.com/library/ms162169.aspx).  
  
