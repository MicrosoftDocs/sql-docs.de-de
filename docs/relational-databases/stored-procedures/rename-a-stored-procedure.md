---
title: Umbenennen einer gespeicherten Prozedur | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine gespeicherte Prozedur in SQL Server 2019 (15.x) mithilfe von SQL Server Management Studio oder Transact-SQL umbenennen.
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], renaming
- renaming stored procedures
ms.assetid: 5d2e4c68-7e0b-4405-8919-f5b203e46770
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 433fa034c85418919aa48886c3a7c06757de890f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347677"
---
# <a name="rename-a-stored-procedure"></a>Umbenennen einer gespeicherten Prozedur

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

In diesem Thema wird beschrieben, wie Sie eine gespeicherte Prozedur in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]umbenennen.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
     [Security](#Security)  
  
-   **Umbenennen einer gespeicherten Prozedur mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Prozedurnamen müssen den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md)entsprechen.  
  
-   Beim Umbenennen einer gespeicherten Prozedur werden die `object_id` und alle Berechtigungen zurückbehalten, die speziell dieser Prozedur zugewiesen wurden. Durch das Löschen und Neuerstellen des Objekts wird eine neue `object_id` erstellt, und es werden alle Berechtigungen entfernt, die speziell dieser Prozedur zugewiesen wurden.

-   Beim Umbenennen einer gespeicherten Prozedur wird der Name des entsprechenden Objekts in der Definitionsspalte der **sys.sql_modules**-Katalogsicht nicht geändert. Um dies zu tun, müssen Sie die gespeicherte Prozedur löschen und mit dem neuen Namen neu erstellen.  

-   Das Ändern des Namens oder der Definition einer Prozedur kann dazu führen, dass abhängige Objekte fehlschlagen, wenn sie nicht entsprechend den Änderungen an der Prozedur aktualisiert werden. Weitere Informationen finden Sie unter [Anzeigen der Abhängigkeiten einer gespeicherten Prozedur](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 CREATE PROCEDURE  
 Erfordert die CREATE PROCEDURE-Berechtigung für die Datenbank und die ALTER-Berechtigung für das Schema, in dem die Prozedur erstellt wird, oder die Mitgliedschaft in der festen Datenbankrolle **db_ddladmin**.  
  
 ALTER PROCEDURE  
 Erfordert die ALTER-Berechtigung für die Prozedur oder die Mitgliedschaft in der festen Datenbankrolle **db_ddladmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-rename-a-stored-procedure"></a>So benennen Sie eine gespeicherte Prozedur um  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit einer Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] her, und erweitern Sie dann diese Instanz.  
2.  Erweitern Sie **Datenbanken**, erweitern Sie die Datenbank, zu der die Prozedur gehört, und erweitern Sie dann **Programmierbarkeit**.  
3.  [Anzeigen der Abhängigkeiten einer gespeicherten Prozedur](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md).  
4.  Erweitern Sie **Gespeicherte Prozeduren**, klicken Sie mit der rechten Maustaste auf die umzubenennende Prozedur, und klicken Sie dann auf **Umbenennen**.  
5.  Ändern Sie den Namen der Prozedur.  
6.  Ändern Sie den Namen der Prozedur in abhängigen Objekten oder Skripts, in denen auf den Namen verwiesen wird.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-rename-a-stored-procedure"></a>So benennen Sie eine gespeicherte Prozedur um  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird gezeigt, wie eine Prozedur umbenannt wird, indem sie gelöscht und mit einem neuen Namen neu erstellt wird. Im ersten Beispiel wird die gespeicherte Prozedur `'HumanResources.uspGetAllEmployeesTest`erstellt. Im zweiten Beispiel wird die gespeicherte Prozedur in `HumanResources.uspEveryEmployeeTest`umbenannt.  
  
```sql  
--Create the stored procedure.  
USE AdventureWorks2012;  
GO  

CREATE PROCEDURE HumanResources.uspGetAllEmployeesTest  
AS  
    SET NOCOUNT ON;  
    SELECT LastName, FirstName, Department  
    FROM HumanResources.vEmployeeDepartmentHistory;  
GO  
  
--Rename the stored procedure.  
EXEC sp_rename 'HumanResources.uspGetAllEmployeesTest', 'uspEveryEmployeeTest'; 
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [Erstellen einer gespeicherten Prozedur](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [Ändern einer gespeicherten Prozedur](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [Löschen einer gespeicherten Prozedur](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)   
 [Anzeigen der Definition einer gespeicherten Prozedur](../../relational-databases/stored-procedures/view-the-definition-of-a-stored-procedure.md)   
 [Anzeigen der Abhängigkeiten einer gespeicherten Prozedur](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)  
  
  
