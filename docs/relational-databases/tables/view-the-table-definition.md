---
description: Anzeigen der Tabellendefinition
title: Anzeigen der Tabellendefinition
ms.custom: ''
ms.date: 03/09/2021
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- showing table properties
- displaying table properties
- tables [SQL Server], properties
- viewing table properties
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 49a0a836cb2e61bef9b4edb280685392f62544ec
ms.sourcegitcommit: 98acedd435aecfda1b3c4c23d3f0c3c1a12682a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102532334"
---
# <a name="view-the-table-definition"></a>Anzeigen der Tabellendefinition
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Sie können die Eigenschaften einer Tabelle in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]anzeigen.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Security](#Security)  
  
-   **So zeigen Sie Tabelleneigenschaften an mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Sie können nur Eigenschaften in einer Tabelle sehen, wenn Sie entweder die Tabelle besitzen oder Ihnen Berechtigungen für diese Tabelle gewährt wurden.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-show-table-properties-in-the-properties-window"></a>So zeigen Sie Tabelleneigenschaften im Eigenschaftenfenster an  
  
1.  Wählen Sie im Objekt-Explorer die Tabelle aus, deren Eigenschaften Sie anzeigen möchten.  
  
2.  Klicken Sie mit der rechten Maustaste auf die Tabelle, und wählen Sie im Kontextmenü die Option **Eigenschaften** aus. Weitere Informationen finden Sie unter [Tabelleneigenschaften – SSMS](../../relational-databases/tables/table-properties-ssms.md).  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-show-table-properties"></a>So zeigen Sie Tabelleneigenschaften an  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. Im Beispiel wird die gespeicherte Systemprozedur „sp_help“ ausgeführt, um alle Spalteninformationen für das angegebene Objekt zurückzugeben.  
  
```sql  
EXEC sp_help 'dbo.mytable';
```  
    
 Weitere Informationen finden Sie unter [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md).

 Alternativ dazu können Sie die Systemkatalogsichten auch direkt abfragen, um Objektmetadateninformationen zu Tabellen, Schemas und Spalten zu erhalten. Beispiel:  
  
```sql
SELECT s.name, t.name, c.* FROM sys.columns AS c
INNER JOIN sys.tables AS t ON t.object_id = c.object_id
INNER JOIN sys.schemas AS s ON s.schema_id = t.schema_id
WHERE t.object_id = object_id('mytable') AND s.name = 'dbo';
```
    
 Weitere Informationen finden Sie unter 

* [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)    
* [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)    
* [sys.schemas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md)     

