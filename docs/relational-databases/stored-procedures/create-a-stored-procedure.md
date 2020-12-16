---
title: Erstellen einer gespeicherten Prozedur | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine gespeicherte Transact-SQL mithilfe von SQL Server Management Studio und der CREATE PROCEDURE-Anweisung von Transact-SQL erstellen.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: quickstart
helpviewer_keywords:
- new stored procedures
- stored procedures [SQL Server], creating
- creating stored procedures
ms.assetid: 76e8a6ba-1381-4620-b356-4311e1331ca7
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c1ab9e2681f229fba5294f061d8d83e98aabac64
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475321"
---
# <a name="create-a-stored-procedure"></a>Erstellen einer gespeicherten Prozedur
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


In diesem Thema wird das Erstellen einer gespeicherten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Prozedur mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] und der [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung CREATE PROCEDURE beschrieben.  
  
-   **Vorbereitungen:**  [Berechtigungen](#Permissions)  
  
-   **So erstellen Sie eine Prozedur mithilfe von:**  [SQL Server Management Studio](#SSMSProcedure), [Transact-SQL](#TsqlProcedure)  
  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die CREATE PROCEDURE-Berechtigung in der Datenbank und die ALTER-Berechtigung auf dem Schema, in dem die Prozedur erstellt wird.  
  
##  <a name="how-to-create-a-stored-procedure"></a><a name="Procedures"></a> So erstellen Sie eine gespeicherte Prozedur  
 Sie können eine der folgenden Anwendungen verwenden:  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So erstellen Sie eine Prozedur im Objekt-Explorer**  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] her, und erweitern Sie dann diese Instanz.  
  
2.  Erweitern Sie **Datenbanken**, erweitern Sie die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Datenbank, und erweitern Sie dann **Programmierbarkeit**.  
  
3.  Klicken Sie mit der rechten Maustaste auf **Gespeicherte Prozeduren**, und klicken Sie dann auf **Neue gespeicherte Prozedur**.  
  
4.  Klicken Sie im Menü **Abfrage** auf **Werte für Vorlagenparameter angeben**.  
  
5.  Geben Sie im Dialogfeld **Werte für Vorlagenparameter angeben** die folgenden Werte für die angezeigten Parameter ein.  
  
    |Parameter|Wert|  
    |---------------|-----------|  
    |Autor|*Ihr Name*|  
    |Erstellt am|*Das heutige Datum*|  
    |BESCHREIBUNG|Gibt Mitarbeiterdaten zurück.|  
    |Prozedurname|HumanResources.uspGetEmployeesTest|  
    |@Param1|@LastName|  
    |@Datatype_For_Param1|**nvarchar**(50)|  
    |Default_Value_For_Param1|NULL|  
    |@Param2|@FirstName|  
    |@Datatype_For_Param2|**nvarchar**(50)|  
    |Default_Value_For_Param2|NULL|  
  
6.  Klicken Sie auf **OK**.  
  
7.  Ersetzen Sie im **Abfrage-Editor** die SELECT-Anweisung durch die folgende Anweisung:  
  
    ```sql  
    SELECT FirstName, LastName, Department  
    FROM HumanResources.vEmployeeDepartmentHistory  
    WHERE FirstName = @FirstName AND LastName = @LastName  
        AND EndDate IS NULL;  
    ```  
  
8.  Zum Testen der Syntax klicken Sie im Menü **Abfrage** auf **Analysieren**. Wenn eine Fehlermeldung zurückgegeben wird, vergleichen Sie die Anweisungen mit den Informationen oben und korrigieren Sie sie gegebenenfalls.  
  
9. Zum Erstellen der Prozedur klicken Sie im Menü **Abfrage** auf **Ausführen**. Die Prozedur wird als Objekt in der Datenbank erstellt.  
  
10. Damit die Prozedur im Objekt-Explorer angezeigt wird, klicken Sie mit der rechten Maustaste auf **Gespeicherte Prozeduren** und wählen **Aktualisieren** aus.  
  
11. Um die Prozedur auszuführen, klicken Sie im Objekt-Explorer mit der rechten Maustaste auf den Namen **HumanResources.uspGetEmployeesTest** der gespeicherten Prozedur, und wählen Sie **Gespeicherte Prozedur ausführen** aus.  
  
12. Geben Sie im Fenster **Prozedur ausführen** den Wert „Margheim“ für den Parameter @LastName und den Wert „Diane“ für den Parameter @FirstName ein.  
  
> [!WARNING]  
>  Überprüfen Sie alle Benutzereingaben. Verketten Sie keine Benutzereingaben, bevor Sie sie überprüft haben. Führen Sie niemals Befehle aus, die sich aus nicht überprüften Benutzereingaben zusammensetzen.  
  
###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So erstellen Sie eine Prozedur im Abfrage-Editor**  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz her.  
  
2.  Klicken Sie im Menü **Datei** auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird die gleiche gespeicherte Prozedur wie oben mit einem anderen Prozedurnamen erstellt.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    CREATE PROCEDURE HumanResources.uspGetEmployeesTest2   
        @LastName nvarchar(50),   
        @FirstName nvarchar(50)   
    AS   
  
        SET NOCOUNT ON;  
        SELECT FirstName, LastName, Department  
        FROM HumanResources.vEmployeeDepartmentHistory  
        WHERE FirstName = @FirstName AND LastName = @LastName  
        AND EndDate IS NULL;  
    GO  
  
    ```  
  
4.  Um die Prozedur auszuführen, kopieren Sie das folgende Beispiel und fügen es in ein neues Abfragefenster ein und klicken auf **Ausführen**. Beachten Sie, dass verschiedene Methoden zum Angeben von Parameterwerten dargestellt werden.  
  
    ```  
    EXECUTE HumanResources.uspGetEmployeesTest2 N'Ackerman', N'Pilar';  
    -- Or  
    EXEC HumanResources.uspGetEmployeesTest2 @LastName = N'Ackerman', @FirstName = N'Pilar';  
    GO  
    -- Or  
    EXECUTE HumanResources.uspGetEmployeesTest2 @FirstName = N'Pilar', @LastName = N'Ackerman';  
    GO  
  
    ```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)  
  
  
