---
title: Delete an Alert
description: Delete an Alert
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, alerts
- alerts [SQL Server], deleting
- deleting alerts
- canceling alerts
- dropping alerts
- disabling alerts
- removing alerts
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 02/04/2021
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: bf4cefa3d6bcd307b80ae4f4dc0944cdb18bcdba
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978784"
---
# <a name="delete-an-alert"></a>Delete an Alert

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Artikel wird beschrieben, wie Sie Microsoft SQL Server-Agent-Warnungen mit SQL Server Management Studio oder Transact-SQL löschen.

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Voraussetzungen

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Einschränkungen

Durch das Entfernen einer Warnung werden auch die zugeordneten Benachrichtigungen entfernt.

### <a name="security"></a><a name="Security"></a>Sicherheit

#### <a name="permissions"></a><a name="Permissions"></a>Berechtigungen

Standardmäßig können nur Mitglieder der festen Serverrolle **sysadmin** Warnungen löschen.  

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Verwenden von SQL Server Management Studio

### <a name="to-delete-an-alert"></a>So löschen Sie eine Warnung

1. Klicken Sie im **Objekt-Explorer** auf das Pluszeichen, um den Server zu erweitern, der die SQL Server-Agent-Warnung enthält, die Sie löschen möchten.

2. Klicken Sie auf das Pluszeichen, um **SQL Server-Agent** zu erweitern.

3. Klicken Sie auf das Pluszeichen, um den Ordner **Warnungen** zu erweitern.

4. Klicken Sie mit der rechten Maustaste auf die Warnung, die Sie löschen möchten, und klicken Sie dann auf **Löschen**.

5. Überprüfen Sie im Dialogfeld **Objekt löschen**, ob Sie die richtige Warnung ausgewählt haben, und klicken Sie auf **OK**.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Verwenden von Transact-SQL

### <a name="to-delete-an-alert"></a>So löschen Sie eine Warnung

1. Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde_md.md)]-Instanz her.

2. Klicken Sie in der Standardleiste auf **Neue Abfrage**.  

3. Kopieren Sie das folgende Beispiel in das Abfragefenster, und klicken Sie dann auf **Ausführen**.

    ```
    -- deletes the SQL Server Agent alert called 'Test Alert.'
    USE msdb ;
    GO
  
    EXEC dbo.sp_delete_alert
       @name = N'Test Alert' ;
    GO
    ```

Weitere Informationen finden Sie unter [sp_delete_alert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-alert-transact-sql.md).
