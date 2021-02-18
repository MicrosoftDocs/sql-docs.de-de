---
description: Assign Alerts to an Operator
title: Assign Alerts to an Operator
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, operators
- assigning alerts to operator
- SQL Server Agent, alerts
- alerts [SQL Server], assigning to operator
- jobs [SQL Server Agent], operators
- notifications [SQL Server], job status
ms.assetid: aa818155-6fa2-4565-a09f-5c7e31c89754
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: e204b890d848cb8307c09e7fb96eb82322a52f63
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354887"
---
# <a name="assign-alerts-to-an-operator"></a>Assign Alerts to an Operator

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Artikel wird beschrieben, wie Warnungen des [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agents Operatoren zugewiesen werden, damit diese in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)] Benachrichtigungen über Aufträge empfangen können.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Einschränkungen  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] kann das gesamte Warnungssystem auf einfache Weise über eine grafische Oberfläche verwaltet werden. Für die Konfiguration einer Warnungsinfrastruktur sollte [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] verwendet werden.  
  
-   Zum Senden einer Benachrichtigung als Reaktion auf eine Warnung müssen Sie zunächst den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent für das Senden von E-Mail konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren von SQL Server-Agent-Mail zum Verwenden von Datenbank-E-Mails](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md).  
  
-   Wenn beim Senden einer E-Mail- oder Pagerbenachrichtigung ein Fehler auftritt, wird der Fehler im Fehlerprotokoll des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Diensts aufgezeichnet.  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
  
#### <a name="permissions"></a><a name="Permissions"></a>Berechtigungen  
Nur Mitglieder der festen Serverrolle **sysadmin** können Operatoren Warnungen zuweisen.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-assign-alerts-to-an-operator"></a>So weisen Sie einem Operator Warnungen zu  
  
1.  Klicken Sie im **Objekt-Explorer** auf das Pluszeichen, um den Server zu erweitern, der den Operator enthält, dem Sie die Warnung zuweisen möchten.  
  
2.  Klicken Sie auf das Pluszeichen, um **SQL Server-Agent** zu erweitern.  
  
3.  Klicken Sie auf das Pluszeichen, um den Ordner **Operatoren** zu erweitern.  
  
4.  Klicken Sie mit der rechten Maustaste auf den Operator, dem Sie eine Warnung zuweisen möchten, wählen Sie **Eigenschaften** aus, und wählen Sie die Seite **Benachrichtigungen** aus.  
  
5.  Wählen Sie im Dialogfeld _operator\_name_ **-Eigenschaften** unter **Seite auswählen** die Option **Benachrichtigungen** aus.  
  
6.  Wählen Sie unter **Benachrichtigungen an diesen Benutzer anzeigen von** die Option **Warnung** aus, um eine Liste der Warnungen anzuzeigen, die an diesen Operator gesendet wurden, oder wählen Sie **Aufträge** aus, um eine Liste der Aufträge anzuzeigen, von denen Benachrichtigungen an diesen Operator gesendet werden. Aktivieren Sie mindestens eines der folgenden Kontrollkästchen, um die Benachrichtigungsmethode für jede Benachrichtigung zu definieren: **E-Mail**, **Pager** oder **NET SEND**.  
  
7.  Wenn Sie fertig sind, klicken Sie auf **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Verwenden von Transact-SQL  
  
#### <a name="to-assign-alerts-to-an-operator"></a>So weisen Sie einem Operator Warnungen zu  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde_md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    -- adds an e-mail notification for the specified alert (Test Alert)  
    -- This example assumes that Test Alert already exists
    -- and that François Ajenstat is a valid operator name.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_notification  
     @alert_name = N'Test Alert',  
     @operator_name = N'François Ajenstat',  
     @notification_method = 1 ;  
    GO  
    ```  
  
Weitere Informationen finden Sie unter [sp_add_notification (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md).  
