---
description: Erstellen von DML-Triggern
title: Erstellen von DML-Triggern | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], DML triggers
- deferred name resolution, DML triggers
- WITH ENCRYPTION clause
- IF UPDATE
- SET statement, DML triggers
- DML triggers, programming
- testing column changes
- results [SQL Server], DML triggers
ms.assetid: b2b52258-642b-462e-8e0f-18c09d2eccf4
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 48d3f41bb6001676492423385b96e212de068d6f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97403315"
---
# <a name="create-dml-triggers"></a>Erstellen von DML-Triggern
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In diesem Thema wird beschrieben, wie ein [!INCLUDE[tsql](../../includes/tsql-md.md)] -DML-Trigger mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] unter Verwendung der [!INCLUDE[tsql](../../includes/tsql-md.md)] - CREATE TRIGGER-Anweisung erstellt wird.  
  
##  <a name="before-you-begin"></a><a name="Top"></a> Vorbereitungen  
  
### <a name="limitations-and-restrictions"></a>Einschränkungen  
 Eine Liste der Einschränkungen in Zusammenhang mit der Erstellung von DML-Triggern finden Sie unter [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
###  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Es ist die ALTER-Berechtigung für die Tabelle oder Sicht erforderlich, für die der Trigger erstellt wird.  
  
##  <a name="how-to-create-a-dml-trigger"></a><a name="Procedures"></a> So erstellen Sie einen DML-Trigger  
 Sie können eine der folgenden Anwendungen verwenden:  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] her, und erweitern Sie dann diese Instanz.  
  
2.  Erweitern Sie **Datenbanken**, die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Datenbank und **Tabellen** , und erweitern Sie dann die Tabelle **Purchasing.PurchaseOrderHeader**.  
  
3.  Klicken Sie mit der rechten Maustaste auf **Trigger**, und wählen Sie **Neuer Trigger** aus.  
  
4.  Klicken Sie im Menü **Abfrage** auf **Werte für Vorlagenparameter angeben**. Alternativ können Sie die Tastenkombination STRG+UMSCHALT+M drücken, um das Dialogfeld **Werte für Vorlagenparameter angeben** zu öffnen.  
  
5.  Geben Sie im Dialogfeld **Werte für Vorlagenparameter angeben** die folgenden Werte für die angezeigten Parameter ein.  
  
    |Parameter|Wert|  
    |---------------|-----------|  
    |Autor|*Ihr Name*|  
    |Erstellt am|*Das heutige Datum*|  
    |BESCHREIBUNG|Überprüft die Anbieterbonität, bevor eine neue Bestellung mit dem einzufügenden Anbieter zugelassen wird.|  
    |Schema_Name|Erwerb|  
    |Trigger_Name|NewPODetail2|  
    |Table_Name|PurchaseOrderDetail|  
    |Data_Modification_Statement|UPDATE und DELETE aus der Liste entfernen.|  
  
6.  Klicken Sie auf **OK**.  
  
7.  Ersetzen Sie im **Abfrage-Editor** den Kommentar `-- Insert statements for trigger here` durch die folgende Anweisung:  
  
    ```sql  
    IF @@ROWCOUNT = 1  
    BEGIN  
       UPDATE Purchasing.PurchaseOrderHeader  
       SET SubTotal = SubTotal + LineTotal  
       FROM inserted  
       WHERE PurchaseOrderHeader.PurchaseOrderID = inserted.PurchaseOrderID  
  
    END  
    ELSE  
    BEGIN  
          UPDATE Purchasing.PurchaseOrderHeader  
       SET SubTotal = SubTotal +   
          (SELECT SUM(LineTotal)  
          FROM inserted  
          WHERE PurchaseOrderHeader.PurchaseOrderID  
           = inserted.PurchaseOrderID)  
       WHERE PurchaseOrderHeader.PurchaseOrderID IN  
          (SELECT PurchaseOrderID FROM inserted)  
    END;  
    ```  
  
8.  Zum Überprüfen der Syntax klicken Sie im Menü **Abfrage** auf **Analysieren**. Wenn eine Fehlermeldung zurückgegeben wird, vergleichen Sie die Anweisung mit den Informationen oben, korrigieren diese gegebenenfalls und wiederholen diesen Schritt.  
  
9. Zum Erstellen des DML-Triggers klicken Sie im Menü **Abfrage** auf **Ausführen**. Der DML-Trigger wird als Objekt in der Datenbank erstellt.  
  
10. Zum Anzeigen des DML-Triggers im Objekt-Explorer klicken Sie mit der rechten Maustaste auf **Trigger** und wählen **Aktualisieren** aus.  

 [Vorbereitungen](#Top)  
  
###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] her, und erweitern Sie dann diese Instanz.  
  
2.  Klicken Sie im Menü **Datei** auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird derselbe gespeicherte DML-Trigger wie oben erstellt.  
  
    ```sql  
    -- Trigger valid for multirow and single row inserts  
    -- and optimal for single row inserts.  
    USE AdventureWorks2012;  
    GO  
    CREATE TRIGGER NewPODetail3  
    ON Purchasing.PurchaseOrderDetail  
    FOR INSERT AS  
    IF @@ROWCOUNT = 1  
    BEGIN  
       UPDATE Purchasing.PurchaseOrderHeader  
       SET SubTotal = SubTotal + LineTotal  
       FROM inserted  
       WHERE PurchaseOrderHeader.PurchaseOrderID = inserted.PurchaseOrderID  
  
    END  
    ELSE  
    BEGIN  
          UPDATE Purchasing.PurchaseOrderHeader  
       SET SubTotal = SubTotal +   
          (SELECT SUM(LineTotal)  
          FROM inserted  
          WHERE PurchaseOrderHeader.PurchaseOrderID  
           = inserted.PurchaseOrderID)  
       WHERE PurchaseOrderHeader.PurchaseOrderID IN  
          (SELECT PurchaseOrderID FROM inserted)  
    END;  
    ```  
  
 
  
