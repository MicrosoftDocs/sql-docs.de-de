---
description: Erstellen eines Auftrags
title: Erstellen eines Auftrags
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], creating
- SQL Server Agent jobs, creating
ms.assetid: b35af2b6-6594-40d1-9861-4d5dd906048c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: fece3364c7d4399f323503f4e7bf58a01f295ba8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338825"
---
# <a name="create-a-job"></a>Erstellen eines Auftrags
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Thema wird beschrieben, wie Sie eine SQL Server-Agent-Auftragskategorie in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] oder SQL Server Management Objects (SMO) erstellen können.  
  
Informationen zum Hinzufügen von Auftragsschritten, Zeitplänen, Warnungen und Benachrichtigungen, die an Benutzer gesendet werden können, finden Sie in den Links im Abschnitt "Siehe auch".  
  
-   **Vorbereitungen:**  
  
    [Einschränkungen](#Restrictions)  
  
    [Security](#Security)  
  
-   **So erstellen Sie einen Auftrag Sicht mit**  
  
    [SQL Server Management Studio](#SSMSProcedure),  
  
    [Transact-SQL](#TsqlProcedure)  
  
    [SQL Server Management Objects](#SMOProcedure)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Einschränkungen  
  
-   Um einen Auftrag erstellen zu können, muss ein Benutzer Mitglied einer der festen Datenbankrollen des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents oder Mitglied der festen Serverrolle **sysadmin** sein. Ein Auftrag kann nur von seinem Besitzer bzw. Mitgliedern der **sysadmin** -Rolle bearbeitet werden. Weitere Informationen zu den festen Datenbankrollen des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents finden Sie unter [Feste Datenbankrollen des SQL Server-Agents](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
-   Wenn Sie einen Auftrag einem anderen Anmeldenamen zuweisen, ist nicht sichergestellt, dass die Berechtigungen des neuen Besitzers zum erfolgreichen Ausführen des Auftrags ausreichen.  
  
-   Lokale Aufträge werden vom lokalen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent zwischengespeichert. Aus diesem Grund erzwingen alle Änderungen implizit das erneute Zwischenspeichern des Auftrags durch den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent. Da der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent den Auftrag erst zwischenspeichert, wenn **sp_add_jobserver** aufgerufen wird, ist es effizienter, **sp_add_jobserver** zuletzt aufzurufen.  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
  
-   Den Besitzer eines Auftrags können Sie nur ändern, wenn Sie als Systemadministrator angemeldet sind.  
  
-   Aus Sicherheitsgründen kann nur der Auftragsbesitzer bzw. ein Mitglied der **sysadmin** -Rolle die Definition des Auftrags ändern. Nur Mitglieder der festen Serverrolle **sysadmin** können anderen Benutzern den Auftragsbesitz zuweisen. Zudem können sie unabhängig vom Auftragsbesitzer alle Aufträge ausführen.  
  
    > [!NOTE]  
    > Wenn Sie den Auftragsbesitz einem Benutzer zuweisen, der kein Mitglied der festen Serverrolle **sysadmin** ist, und wenn in dem Auftrag Schritte ausgeführt werden, für die Proxykonten erforderlich sind (beispielsweise die [!INCLUDE[ssIS](../../includes/ssis_md.md)] -Paketausführung), müssen Sie sicherstellen, dass der Benutzer auf dieses Proxykonto zugreifen kann. Andernfalls schlägt der Auftrag fehl.  
  
#### <a name="permissions"></a><a name="Permissions"></a>Berechtigungen  
Ausführliche Informationen finden Sie unter [Implementieren der SQL Server-Agent-Sicherheit](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-create-a-sql-server-agent-job"></a>So erstellen Sie einen Auftrag für den SQL Server-Agent  
  
1.  Klicken Sie im **Objekt-Explorer** auf das Pluszeichen, um den Server zu erweitern, auf dem Sie den SQL Server-Agent-Auftrag erstellen möchten.  
  
2.  Klicken Sie auf das Pluszeichen, um **SQL Server-Agent** zu erweitern.  
  
3.  Klicken Sie mit der rechten Maustaste auf den Ordner **Aufträge**, und wählen Sie dann **Neuer Auftrag** aus.  
  
4.  Ändern Sie im Dialogfeld **Neuer Auftrag** auf der Seite **Allgemein** die allgemeinen Eigenschaften des Auftrags. Weitere Informationen zu den verfügbaren Optionen auf dieser Seite finden Sie unter [Auftragseigenschaften – Neuer Auftrag &#40;Seite „Allgemein“&#41;](../../ssms/agent/job-properties-new-job-general-page.md).  
  
5.  Organisieren Sie die Auftragsschritte auf der Seite **Schritte** . Weitere Informationen zu den verfügbaren Optionen auf dieser Seite finden Sie unter [Auftragseigenschaften – Neuer Auftrag &#40;Seite „Schritte“&#41;](../../ssms/agent/job-properties-new-job-steps-page.md).  
  
6.  Organisieren Sie auf der Seite **Zeitpläne** Zeitpläne für den Auftrag. Weitere Informationen zu den verfügbaren Optionen auf dieser Seite finden Sie unter [Auftragseigenschaften – Neuer Auftrag &#40;Seite „Zeitpläne“&#41;](../../ssms/agent/job-properties-new-job-schedules-page.md).  
  
7.  Organisieren Sie auf der Seite **Warnungen** die Warnungen für den Auftrag. Weitere Informationen zu den verfügbaren Optionen auf dieser Seite finden Sie unter [Auftragseigenschaften – Neuer Auftrag &#40;Seite „Warnungen“&#41;](../../ssms/agent/job-properties-new-job-alerts-page.md).  
  
8.  Legen Sie auf der Seite **Benachrichtigungen** die Aktionen für den [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent fest, die bei Abschluss des Auftrags auszuführen sind. Weitere Informationen zu den verfügbaren Optionen auf dieser Seite finden Sie unter [Auftragseigenschaften – Neuer Auftrag &#40;Seite „Benachrichtigungen“&#41;](../../ssms/agent/job-properties-new-job-notifications-page.md).  
  
9. Verwalten Sie auf der Seite **Ziele** die Zielserver für den Auftrag. Weitere Informationen zu den verfügbaren Optionen auf dieser Seite finden Sie unter [Auftragseigenschaften – Neuer Auftrag &#40;Seite „Ziele“&#41;](../../ssms/agent/job-properties-new-job-targets-page.md).  
  
10. Wenn Sie fertig sind, klicken Sie auf **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Verwenden von Transact-SQL  
  
#### <a name="to-create-a-sql-server-agent-job"></a>So erstellen Sie einen Auftrag für den SQL Server-Agent  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde_md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    USE msdb ;  
    GO  
    EXEC dbo.sp_add_job  
        @job_name = N'Weekly Sales Data Backup' ;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'TSQL',  
        @command = N'ALTER DATABASE SALES SET READ_ONLY',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    EXEC dbo.sp_add_schedule  
        @schedule_name = N'RunOnce',  
        @freq_type = 1,  
        @active_start_time = 233000 ;  
    USE msdb ;  
    GO  
    EXEC sp_attach_schedule  
       @job_name = N'Weekly Sales Data Backup',  
       @schedule_name = N'RunOnce';  
    GO  
    EXEC dbo.sp_add_jobserver  
        @job_name = N'Weekly Sales Data Backup';  
    GO  
    ```  
  
Weitere Informationen finden Sie unter  
  
-   [sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
  
-   [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)  
  
-   [sp_add_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)  
  
-   [sp_attach_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
-   [sp_add_jobserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)  
  
## <a name="using-sql-server-management-objects"></a><a name="SMOProcedure"></a>Verwendung von SQL Server Management Objects  
**So erstellen Sie einen Auftrag für den SQL Server-Agent**  
  
Rufen Sie die **Create** -Methode der **Job** -Klasse in einer Programmiersprache Ihrer Wahl auf, z. B. Visual Basic, Visual C# oder PowerShell. Beispielcode hierzu finden Sie unter [Planen von automatischen, administrativen Tasks im SQL Server-Agent](../../relational-databases/server-management-objects-smo/tasks/scheduling-automatic-administrative-tasks-in-sql-server-agent.md).  
