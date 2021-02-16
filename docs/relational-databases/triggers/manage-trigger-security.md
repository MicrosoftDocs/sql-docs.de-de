---
description: Verwalten der Triggersicherheit
title: Verwalten der Triggersicherheit | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- triggers [SQL Server], security
ms.assetid: e94720a8-a3a2-4364-b0a3-bbe86e3ce4d5
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 931ba1487b8212c6c088b044df5e0b9fa30b5ef1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347597"
---
# <a name="manage-trigger-security"></a>Verwalten der Triggersicherheit

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Standardmäßig werden sowohl DML- als auch DDL-Trigger im Kontext des Benutzers ausgeführt, der den Trigger aufruft. Genauer ist der Benutzer, der den Trigger aufruft, derjenige Benutzer, der die Anweisung ausführt, die zum Ausführen des Triggers führt. Wenn die Benutzerin **Mary** beispielsweise eine DELETE-Anweisung ausführt, die dazu führt, dass der DML-Trigger **DML_trigMary** ausgeführt wird, wird der Code in **DML_trigMary** im Kontext der Privilegien für **Mary** ausgeführt. Dieses Standardverhalten kann von Benutzern ausgenutzt werden, die bösartigen Code in die Datenbank oder Serverinstanz einschleusen möchten. Beispielsweise wird der folgende DDL-Trigger vom Benutzer **JohnDoe** erstellt:  

```sql
CREATE TRIGGER DDL_trigJohnDoe
ON DATABASE
FOR ALTER_TABLE
AS
SET NOCOUNT ON;

BEGIN TRY
  EXEC(N'
    USE [master];
    GRANT CONTROL SERVER TO [JohnDoe];
');
END TRY
BEGIN CATCH
  DECLARE @DoNothing INT;
END CATCH;
GO
```

Dieser Trigger bewirkt Folgendes: Sobald ein Benutzer mit der Berechtigung, `GRANT CONTROL SERVER`-Anweisungen auszuführen, wie z. B. ein Mitglied der festen Serverrolle **sysadmin**, eine `ALTER TABLE`-Anweisung ausführt, wird **JohnDoe** die `CONTROL SERVER`-Berechtigung erteilt. Mit anderen Worten heißt dies, dass **JohnDoe** zwar die `CONTROL SERVER`-Berechtigung nicht sich selbst erteilen darf, jedoch den Triggercode aktivieren kann, der ihm diese Berechtigung verschafft. Sowohl DML- als auch DDL-Trigger sind diesen Sicherheitsrisiken ausgesetzt.  
  
## <a name="trigger-security-best-practices"></a>Bewährte Methoden für die Triggersicherheit  
 Sie können folgende Maßnahmen ergreifen, um zu vermeiden, dass Triggercode mit ausgeweiteten Privilegien ausgeführt wird:  
  
::: moniker range=">=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current"

-   Eruieren Sie die in der Datenbank und der Serverinstanz vorhandenen DML- und DDL-Trigger, indem Sie die Katalogsichten [sys.triggers](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) und [sys.server_triggers](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md) abfragen. Die folgende Abfrage gibt alle DML-Trigger und DDL-Trigger auf Datenbankebene der aktuellen Datenbank zurück, sowie alle DDL-Trigger auf Serverebene der Serverinstanz:  
  
    ```sql
    SELECT type, name, parent_class_desc FROM sys.triggers
    UNION ALL
    SELECT type, name, parent_class_desc FROM sys.server_triggers;
    ```  

   > [!NOTE]
   > Nur **sys.triggers** ist für [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] verfügbar, wenn Sie keine [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] verwenden.

::: moniker-end

::: moniker range="=azuresqldb-current"

-   Beachten Sie die in der Datenbank vorhandenen DML- und DDL-Trigger, indem Sie die Katalogsicht [sys.triggers](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) abfragen. Die folgende Abfrage gibt alle DML-Trigger und DDL-Trigger auf Datenbankebene in der aktuellen Datenbank zurück:  
  
    ```sql
    SELECT type, name, parent_class_desc FROM sys.triggers;
    ```  
  
::: moniker-end

-   Verwenden Sie [DISABLE TRIGGER](../../t-sql/statements/disable-trigger-transact-sql.md) , um die Trigger zu deaktivieren, die die Integrität der Datenbank oder des Servers beeinträchtigen können, falls sie mit ausgeweiteten Privilegien ausgeführt werden. Die folgende Anweisung deaktiviert alle DDL-Trigger auf Datenbankebene in der aktuellen Datenbank:  
  
    ```sql
    DISABLE TRIGGER ALL ON DATABASE;
    ```  
  
     Die folgende Anweisung deaktiviert alle DDL-Trigger auf Serverebene in der Serverinstanz:  
  
    ```sql
    DISABLE TRIGGER ALL ON ALL SERVER;
    ```  
  
     Die folgende Anweisung deaktiviert alle DML-Trigger in der aktuellen Datenbank:  
  
    ```sql
    DECLARE @schema_name sysname, @trigger_name sysname, @object_name sysname;
    DECLARE @sql nvarchar(max);
    DECLARE trig_cur CURSOR FORWARD_ONLY READ_ONLY FOR
        SELECT SCHEMA_NAME(schema_id) AS schema_name,
            name AS trigger_name,
            OBJECT_NAME(parent_object_id) AS object_name
        FROM sys.objects WHERE type IN ('TR', 'TA');

    OPEN trig_cur;
    FETCH NEXT FROM trig_cur INTO @schema_name, @trigger_name, @object_name;
  
    WHILE @@FETCH_STATUS = 0
    BEGIN
        SELECT @sql = N'DISABLE TRIGGER ' + QUOTENAME(@schema_name) + N'.'
            + QUOTENAME(@trigger_name)
            + N' ON ' + QUOTENAME(@schema_name) + N'.'
            + QUOTENAME(@object_name) + N'; ';
        EXEC (@sql);
        FETCH NEXT FROM trig_cur INTO @schema_name, @trigger_name, @object_name;
    END;
    GO

    -- Verify triggers are disabled. Should return an empty result set.
    SELECT * FROM sys.triggers WHERE is_disabled = 0;
    GO

    CLOSE trig_cur;
    DEALLOCATE trig_cur;
    ```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DML-Trigger](../../relational-databases/triggers/dml-triggers.md)   
 [DDL-Trigger](../../relational-databases/triggers/ddl-triggers.md)  
 [Logon-Trigger](../../relational-databases/triggers/logon-triggers.md)  
  
