---
description: Erstellen eines Analysis Services-Auftragsschritts
title: Erstellen eines Analysis Services-Auftragsschritts
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- job steps [Analysis Services]
ms.assetid: 03d4bb86-514b-4a55-97b9-c2c0fa08b428
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 56640f5889a8e58e22bd0290b4aaea03d796cc80
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92035057"
---
# <a name="create-an-analysis-services-job-step"></a>Erstellen eines Analysis Services-Auftragsschritts

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details hierzu finden Sie unter [Unterschiede bei T-SQL zwischen SQL Server und Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Thema wird beschrieben, wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Auftragsschritte in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] erstellt und definiert werden, mit denen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Analysis Services-Befehle und -Abfragen durch die Verwendung von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] oder SQL Server Management Objects ausgeführt werden.  
  
-   **Vorbereitungen:**  
  
    [Einschränkungen](#Restrictions)  
  
    [Security](#Security)  
  
-   **So erstellen Sie einen SQL Server-Auftrag mithilfe von Analysis Services-Befehlen bzw. -Abfragen mit**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server Management Objects](#SMO)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Einschränkungen  
  
-   Wenn beim Auftragsschritt ein Analysis Services-Befehl verwendet wird, muss die Befehlsanweisung eine **Execute** -Methode von XML for Analysis Services sein. Die Anweisung enthält möglicherweise keinen vollständigen SOAP-Umschlag (Simple Object Access Protocol) oder eine **Discover** -Methode von XML for Analysis. Während [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] vollständige SOAP-Umschläge und die **Discover** -Methode unterstützt, ist das bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Auftragsschritten nicht der Fall. Weitere Informationen zu XML for Analysis Services finden Sie unter [Übersicht über XMLA for Analysis (XMLA)](/previous-versions/sql/).  
  
-   Wenn beim Auftragsschritt ein Analysis Services-Abfrage verwendet wird, muss die Abfrageanweisung eine MDX-Abfrage (Multidimensional Expressions, mehrdimensionale Ausdrücke) sein. Weitere Informationen zu MDX finden Sie unter [Grundlegendes zur MDX-Anweisung (MDX)](/analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services?viewFallbackFrom=sql-server-ver15).  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
  
#### <a name="permissions"></a><a name="Permissions"></a>Berechtigungen  
  
-   Um einen Auftragsschritt auszuführen, der das Analysis Services-Subsystem verwendet, muss ein Benutzer Mitglied der festen Serverrolle **sysadmin** sein oder Zugriff auf ein gültiges Proxykonto haben, das für die Verwendung dieses Subsystems definiert ist. Darüber hinaus muss es sich bei dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Dienstkonto oder Proxy um einen Analysis Services-Administrator und ein gültiges Windows-Domänenkonto handeln.  
  
-   Nur Mitglieder der festen Serverrolle **sysadmin** sind berechtigt, die Ausgabe eines Auftragsschritts in eine Datei zu schreiben. Wenn der Auftragsschritt von Benutzern ausgeführt wird, die in der **msdb** -Datenbank Mitglied der **SQLAgentUserRole** -Datenbankrolle sind, können die Ausgabedaten nur in eine Tabelle geschrieben werden. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent schreibt die Ausgabedaten des Auftragsschritts in der **SQLAgentUserRole** -Datenbank in die **SQLAgentUserRole** -Tabelle.  
  
-   Ausführliche Informationen finden Sie unter [Implementieren der SQL Server-Agent-Sicherheit](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-create-an-analysis-services-command-job-step"></a>So erstellen Sie einen Auftragsschritt für den Analysis Services-Befehl  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]her, und erweitern Sie dann diese Instanz.  
  
2.  Erweitern Sie **SQL Server-Agent**, erstellen Sie einen neuen Auftrag, oder klicken Sie mit der rechten Maustaste auf einen vorhandenen Auftrag, und klicken Sie dann auf **Eigenschaften**. Weitere Informationen zum Erstellen eines Auftrags finden Sie unter [Erstellen von Aufträgen](../../ssms/agent/create-jobs.md).  
  
3.  Klicken Sie im Dialogfeld **Auftragseigenschaften** auf die Registerkarte **Schritte** , und klicken Sie dann auf **Neu**.  
  
4.  Geben Sie im Dialogfeld **Neuer Auftragsschritt** unter **Schrittname**einen Schrittnamen für den Auftrag ein.  
  
5.  Klicken Sie in der Liste **Typ** auf **SQL Server Analysis Services-Befehl**.  
  
6.  Wählen Sie in der Liste **Ausführen als** einen Proxy aus, der für die Verwendung des Analysis Services-Befehlssubsystems definiert ist. Ein Benutzer, der Mitglied der festen Serverrolle **sysadmin** ist, kann zur Ausführung dieses Auftragsschritts auch **SQL-Agent-Dienstkonto** auswählen.  
  
7.  Wählen Sie den **Server** aus, auf dem der Auftragsschritt ausgeführt wird, oder geben Sie den Servernamen ein.  
  
8.  Geben Sie im Feld **Befehl** die auszuführende Anweisung ein, oder klicken Sie auf **Öffnen** , um eine Anweisung auszuwählen.  
  
9. Klicken Sie auf die Seite **Erweitert** , um die Optionen für diesen Auftragsschritt zu definieren. Legen Sie beispielsweise fest, welche Aktion der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ausführen soll, wenn der Auftragsschritt erfolgreich ausgeführt wird oder fehlschlägt, wie viele Versuche zur Ausführung des Auftragsschritts unternommen werden sollen und wohin die Ausgabe des Auftragsschritts geschrieben werden soll.  
  
#### <a name="to-create-an-analysis-services-query-job-step"></a>So erstellen Sie einen Auftragsschritt für die Analysis Services-Abfrage  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]her, und erweitern Sie dann diese Instanz.  
  
2.  Erweitern Sie **SQL Server-Agent**, erstellen Sie einen neuen Auftrag, oder klicken Sie mit der rechten Maustaste auf einen vorhandenen Auftrag, und klicken Sie dann auf **Eigenschaften**. Weitere Informationen zum Erstellen eines Auftrags finden Sie unter [Erstellen von Aufträgen](../../ssms/agent/create-jobs.md).  
  
3.  Klicken Sie im Dialogfeld **Auftragseigenschaften** auf die Seite **Schritte** und dann auf **Neu**.  
  
4.  Geben Sie im Dialogfeld **Neuer Auftragsschritt** unter **Schrittname**einen Schrittnamen für den Auftrag ein.  
  
5.  Klicken Sie in der Liste **Typ** auf **SQL Server Analysis Services-Abfrage**.  
  
6.  Wählen Sie in der Liste **Ausführen als** einen Proxy aus, der für die Verwendung des Analysis Services-Abfragesubsystems definiert ist. Ein Benutzer, der Mitglied der festen Serverrolle **sysadmin** ist, kann zur Ausführung dieses Auftragsschritts auch **SQL-Agent-Dienstkonto** auswählen.  
  
7.  Wählen Sie einen Wert unter **Server** und **Datenbank** für die Ausführung des Auftragsschritts aus, oder geben Sie den Server- bzw. Datenbanknamen ein.  
  
8.  Geben Sie im Feld **Befehl** die auszuführende Anweisung ein, oder klicken Sie auf **Öffnen** , um eine Anweisung auszuwählen.  
  
9. Klicken Sie auf die Seite **Erweitert** , um die Optionen für diesen Auftragsschritt zu definieren. Legen Sie beispielsweise fest, welche Aktion der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ausführen soll, wenn der Auftragsschritt erfolgreich ausgeführt wird oder fehlschlägt, wie viele Versuche zur Ausführung des Auftragsschritts unternommen werden sollen und wohin die Ausgabe des Auftragsschritts geschrieben werden soll.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Verwenden von Transact-SQL  
  
#### <a name="to-create-an-analysis-services-command-job-step"></a>So erstellen Sie einen Auftragsschritt für den Analysis Services-Befehl  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde_md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    -- Creates a job step that uses XMLA to create a relational data source that
    -- references the AdventureWorks2012 Microsoft SQL Server database.  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name =
            N'Create a relational data source that references the AdventureWorks2012 Microsoft SQL Server database',  
        @subsystem = N'ANALYSISCOMMAND',  
        @command =
            N' <Create xmlns="https://schemas.microsoft.com/analysisservices/2003/engine">  
        <ParentObject>  
            <DatabaseID>AdventureWorks2012</DatabaseID>  
        </ParentObject>  
        <ObjectDefinition>  
            <DataSource xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xsi:type="RelationalDataSource">  
                <ID>AdventureWorks2012</ID>  
                <Name>Adventure Works 2012</Name>  
                <ConnectionString>Data Source=localhost;Initial Catalog=AdventureWorks2012;Integrated Security=True</ConnectionString>  
                <ImpersonationInfo>  
                    <ImpersonationMode>ImpersonateServiceAccount</ImpersonationMode>  
                </ImpersonationInfo>  
                <ManagedProvider>System.Data.SqlClient</ManagedProvider>  
                <Timeout>PT0S</Timeout>  
            </DataSource>  
        </ObjectDefinition>  
    </Create>', ;  
    GO  
    ```  
  
Weitere Informationen finden Sie unter [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md).  
  
#### <a name="to-create-an-analysis-services-query-job-step"></a>So erstellen Sie einen Auftragsschritt für die Analysis Services-Abfrage  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde_md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    -- Creates a job step that uses MDX to return data  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Returns the Internet sales amount by state',  
        @subsystem = N'ANALYSISQUERY',  
        @command = N' SELECT  
       [Measures].[Internet Sales Amount] ON COLUMNS,  
       [Customer].[State-Province].Members ON ROWS  
    FROM [AdventureWorks2012]',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    ```  
  
Weitere Informationen finden Sie unter [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Verwendung von SQL Server Management Objects  
**So erstellen Sie einen PowerShell-Skript-Auftragsschritt**  
  
Verwenden Sie die **JobStep** -Klasse in einer von Ihnen ausgewählten Programmiersprache, z. B. XMLA oder MDX. Weitere Informationen finden Sie unter [SQL Server Management Objects (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md).  
