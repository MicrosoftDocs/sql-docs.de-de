---
description: Stop a Job
title: Stop a Job
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], stopping
- SQL Server Agent jobs, stopping
- stopping jobs
ms.assetid: 4249328a-24d8-4284-9d1d-7d04ed90e3d7
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 89c8b041cbeaba8d82d1a1c9750e2bf88e04d405
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472231"
---
# <a name="stop-a-job"></a>Stop a Job
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Artikel wird beschrieben, wie ein [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Auftrag beendet wird. Ein Auftrag ist eine festgelegte Reihe von Aktionen, die der SQL Server-Agent ausführt.  
  
-   **Vorbereitungen:**  ,  
  
    [Einschränkungen](#Restrictions)  
  
    [Security](#Security)  
  
-   **So halten Sie einen Auftrag an mit**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server Management Objects](#SMO)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Einschränkungen  
  
-   Wenn ein Auftrag aktuell einen Schritt des Typs **CmdExec** oder **PowerShell** ausführt, wird der ausgeführte Prozess (z. B. Programm.exe) vorzeitig beendet. Dies kann zu unvorhersehbarem Verhalten führen, so werden z.&nbsp;B. Dateien, die vom Prozess verwendet werden, geöffnet bleiben.  
  
-   Bei einem Multiserverauftrag wird eine STOP-Anweisung für den Auftrag an alle Zielserver des Auftrags gesendet.  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
Ausführliche Informationen finden Sie unter [Implementieren der SQL Server-Agent-Sicherheit](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-stop-a-job"></a>So beenden Sie einen Auftrag  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]her, und erweitern Sie dann diese Instanz.  
  
2.  Erweitern Sie **SQL Server-Agent**, erweitern Sie **Aufträge**, klicken Sie mit der rechten Maustaste auf den zu beendenden Auftrag, und klicken Sie dann auf **Auftrag beenden**.  
  
3.  Wenn Sie mehrere Aufträge beenden möchten, klicken Sie mit der rechten Maustaste auf **Auftragsaktivitätsmonitor**, und klicken Sie dann auf **Auftragsaktivitäten anzeigen**. Wählen Sie im Auftragsaktivitätsmonitor die Aufträge aus, die beendet werden sollen, klicken Sie mit der rechten Maustaste auf Ihre Auswahl, und klicken Sie dann auf **Aufträge beenden**.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Verwenden von Transact-SQL  
  
#### <a name="to-stop-a-job"></a>So beenden Sie einen Auftrag  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde_md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    -- stops a job named Weekly Sales Data Backup  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_stop_job  
        N'Weekly Sales Data Backup' ;  
    GO  
    ```  
  
Weitere Informationen finden Sie unter [sp_stop_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-stop-job-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Verwendung von SQL Server Management Objects  
**So beenden Sie einen Auftrag**  
  
Rufen Sie die **Stop** -Methode der **Job** -Klasse in einer Programmiersprache Ihrer Wahl auf, z. B. Visual Basic, Visual C# oder PowerShell. Weitere Informationen finden Sie unter [SQL Server Management Objects (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md).  
