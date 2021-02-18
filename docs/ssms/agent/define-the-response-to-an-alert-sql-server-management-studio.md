---
description: Definieren der Reaktion auf eine Warnung
title: Definieren der Reaktion auf eine Warnung
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, alerts
- alerts [SQL Server], responding to
- responding to alerts
ms.assetid: c86ca6eb-c59f-46e9-bc32-d474e7c3b170
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 53787a3f08aab02d1a2fee4500570f4033b579fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342711"
---
# <a name="define-the-response-to-an-alert"></a>Definieren der Reaktion auf eine Warnung

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Artikel wird beschrieben, wie die Reaktion von [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Warnungen in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)] definiert wird.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Einschränkungen  
  
-   Die Pager- und **net send** -Optionen werden in zukünftigen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht mehr im [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktionen zurzeit verwenden.  
  
-   Beachten Sie, dass E-Mail- und Pagerbenachrichtigungen an Operatoren nur versendet werden können, wenn der SQL Server-Agent für die Verwendung von Datenbank-E-Mail konfiguriert ist. Weitere Informationen finden Sie unter [Zuweisen von Warnungen zu einem Operator](assign-alerts-to-an-operator.md).  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] können Aufträge problemlos mithilfe einer grafischen Oberfläche verwaltet werden. Dies ist die empfohlene Vorgehensweise für die Erstellung und Verwaltung der Auftragsinfrastruktur.  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
  
#### <a name="permissions"></a><a name="Permissions"></a>Berechtigungen  
Nur Mitglieder der festen Serverrolle **sysadmin** können die Antwort auf eine Warnung definieren.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-define-the-response-to-an-alert"></a>So definieren Sie die Antwort auf eine Warnung  
  
1.  Klicken Sie im **Objekt-Explorer** auf das Pluszeichen, um den Server zu erweitern, der die Warnung enthält, für die Sie eine Warnung definieren möchten.  
  
2.  Klicken Sie auf das Pluszeichen, um **SQL Server-Agent** zu erweitern.  
  
3.  Klicken Sie auf das Pluszeichen, um den Ordner **Warnungen** zu erweitern.  
  
4.  Klicken Sie mit der rechten Maustaste auf die Warnung, für die Sie eine Antwort definieren möchten, und wählen Sie **Eigenschaften** aus.  
  
5.  Wählen Sie im Dialogfeld **Eigenschaften von Warnung _Warnungsname_** unter **Seite auswählen** die Option **Antwort** aus.  
  
6.  Aktivieren Sie das Kontrollkästchen **Auftrag ausführen** , und wählen Sie aus der Liste unter dem Kontrollkästchen **Auftrag ausführen** einen Auftrag aus, der ausgeführt werden soll, wenn die Warnung angezeigt wird. Sie können einen neuen Auftrag erstellen, indem Sie auf **Neuer Auftrag** klicken. Um weitere Informationen zu dem Auftrag anzuzeigen, klicken Sie auf **Auftrag anzeigen**. Weitere Informationen zu den verfügbaren Optionen in den Dialogfeldern **Neuer Auftrag** und **Auftragseigenschaften** - _Auftragsname_ finden Sie unter [Erstellen eines Auftrags](../../ssms/agent/create-a-job.md) und [Anzeigen eines Auftrags](../../ssms/agent/view-a-job.md).  
  
7.  Aktivieren Sie das Kontrollkästchen **Operatoren benachrichtigen** , sofern Sie Operatoren benachrichtigen möchten, wenn eine Warnung aktiviert ist. Wählen Sie in der Liste **Operator** mindestens eine der folgenden Methoden für die Benachrichtigung eines Operators bzw. von Operatoren aus: **E-Mail**, **Pager** oder **NET SEND**. Sie können einen neuen Operator erstellen, indem Sie auf **Neuer Operator** klicken. Um weitere Informationen über einen Operator anzuzeigen, klicken Sie auf **Operator anzeigen**. Weitere Informationen zu den verfügbaren Optionen im Dialogfeld **Neuer Operator** und im Dialogfeld zum **Anzeigen von Operatoreigenschaften** unter [Create an Operator](../../ssms/agent/create-an-operator.md) und [View Information About an Operator](../../ssms/agent/view-information-about-an-operator.md).  
  
8.  Wenn Sie fertig sind, klicken Sie auf **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Verwenden von Transact-SQL  
  
#### <a name="to-define-the-response-to-an-alert"></a>So definieren Sie die Antwort auf eine Warnung  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde_md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    -- adds an e-mail notification for Test Alert.  
    -- assumes that Test Alert already exists and that
    -- François Ajenstat is a valid operator name   
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_notification  
     @alert_name = N'Test Alert',  
     @operator_name = N'François Ajenstat',  
     @notification_method = 1 ;  
    GO  
    ```  
  
Weitere Informationen finden Sie unter [sp_add_notification (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md).