---
description: ALTER SCHEMA (Transact-SQL)
title: ALTER SCHEMA (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/09/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER SCHEMA
- ALTER_SCHEMA_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], transferring
- transferring objects between schemas
- ALTER SCHEMA statement
- schemas [SQL Server], modifying
- modifying schemas
ms.assetid: 0a760138-460e-410a-a3c1-d60af03bf2ed
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c87d341f26970e368374702dfd20b99b6f56c441
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339301"
---
# <a name="alter-schema-transact-sql"></a>ALTER SCHEMA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Überträgt ein sicherungsfähiges Element zwischen Schemas.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ALTER SCHEMA schema_name   
   TRANSFER [ <entity_type> :: ] securable_name   
[;]  
  
<entity_type> ::=  
    {  
    Object | Type | XML Schema Collection  
    }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER SCHEMA schema_name   
   TRANSFER [ OBJECT :: ] securable_name   
[;]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *schema_name*  
 Der Name eines Schemas in der aktuellen Datenbank, in das das sicherungsfähige Element verschoben wird. Kann weder SYS noch INFORMATION_SCHEMA sein.  
  
 \<entity_type>  
 Die Klasse der Entität, für die der Besitzer geändert wird. Object ist der Standardwert.  
  
 *securable_name*  
 Bezeichnet den ein- oder zweiteiligen Namen eines sicherungsfähigen schemabezogenen Elements, das in das Schema verschoben werden soll.  
  
## <a name="remarks"></a>Bemerkungen  
 Benutzer und Schemas vollkommen voneinander getrennt.  
  
 ALTER SCHEMA kann nur zum Verschieben von sicherungsfähigen Elementen zwischen Schemas in derselben Datenbank verwendet werden. Zum Ändern oder Löschen eines sicherungsfähigen Elements in einem Schema verwenden Sie die für das sicherungsfähige Element spezifische ALTER- oder DROP-Anweisung.  
  
 Wenn ein einteiliger Name für *securable_name* verwendet wird, werden die derzeit gültigen Regeln zur Namensauflösung zum Auffinden des sicherungsfähigen Elements angewendet.  
  
 Alle dem sicherungsfähigen Element zugeordneten Berechtigungen werden gelöscht, wenn das sicherungsfähige Element in das neue Schema verschoben wird. Wurde der Besitzer des sicherungsfähigen Elements explizit festgelegt, bleibt der Besitzer unverändert. Wenn der Besitzer des sicherungsfähigen Elements auf SCHEMA OWNER festgelegt wurde, bleibt diese Einstellung zunächst erhalten. Nach dem Verschieben wird SCHEMA OWNER jedoch zum Besitzer des neuen Schemas aufgelöst. principal_id des neuen Besitzers ist NULL.  
  
 Durch das Verschieben einer gespeicherten Prozedur, Funktion, Sicht oder eines Triggers wird der möglicherweise vorhandene Schemaname des entsprechenden Objekts nicht geändert. Dies gilt sowohl für eine Bezeichnung in der definition-Spalte der [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)-Katalogsicht als auch für eine Bezeichnung, die über die integrierte Funktion [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) abgerufen wird. Daher ist es empfehlenswert, ALTER SCHEMA nicht zum Verschieben dieser Objekttypen zu verwenden. Löschen Sie stattdessen das Objekt, und erstellen Sie es neu im zugehörigen neuen Schema.  
  
 Beim Verschieben eines Objekts wie z.B. einer Tabelle oder eines Synonyms werden Verweise auf das Objekt nicht automatisch aktualisiert. Sie müssen Objekte, die auf das verschobene Objekt verweisen, manuell ändern. Wenn Sie z.B. eine Tabelle verschieben und in einem Trigger auf diese Tabelle verwiesen wird, müssen Sie den Trigger ändern, damit für ihn der Schemaname verwendet wird. Mit [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) können Sie Objektabhängigkeiten auflisten, bevor Sie das Objekt verschieben.  

 Wenn Sie das Schema einer Tabelle mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ändern möchten, können Sie im Objekt-Explorer mit der rechten Maustaste auf die Tabelle und anschließend auf **Entwurf** klicken. Drücken Sie **F4**, um das Eigenschaftenfenster zu öffnen. Wählen Sie im Feld **Schema** ein neues Schema aus.  
 
 ALTER SCHEMA verwendet eine Sperre auf Schemaebene.
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Berechtigungen  
 Für die Übertragung eines sicherungsfähigen Elements aus einem anderen Schema benötigt der aktuelle Benutzer die CONTROL-Berechtigung für das sicherungsfähige Element (nicht das Schema) sowie die ALTER-Berechtigung für das Zielschema.  
  
 Wenn das sicherungsfähige Element über eine EXECUTE AS OWNER-Spezifikation verfügt und der Benutzer als SCHEMA OWNER festgelegt ist, benötigt der Benutzer auch die IMPERSONATE-Berechtigung für den Besitzer des Zielschemas.  
  
 Alle dem sicherungsfähigen Element, das übertragen wird, zugeordneten Berechtigungen werden gelöscht, wenn es verschoben wird.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-transferring-ownership-of-a-table"></a>A. Übertragen des Besitzes einer Tabelle  
 Im folgenden Beispiel wird das `HumanResources`-Schema durch Übertragen der `Address`-Tabelle aus dem `Person`-Schema in das Schema „HumanResources“ geändert.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER SCHEMA HumanResources TRANSFER Person.Address;  
GO  
```  
  
### <a name="b-transferring-ownership-of-a-type"></a>B. Übertragen des Besitzes eines Typs  
 Im folgenden Beispiel wird ein Typ im `Production`-Schema erstellt und dann an das `Person`-Schema übertragen.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
CREATE TYPE Production.TestType FROM [VARCHAR](10) NOT NULL ;  
GO  
  
-- Check the type owner.  
SELECT sys.types.name, sys.types.schema_id, sys.schemas.name  
    FROM sys.types JOIN sys.schemas   
        ON sys.types.schema_id = sys.schemas.schema_id   
    WHERE sys.types.name = 'TestType' ;  
GO  
  
-- Change the type to the Person schema.  
ALTER SCHEMA Person TRANSFER type::Production.TestType ;  
GO  
  
-- Check the type owner.  
SELECT sys.types.name, sys.types.schema_id, sys.schemas.name  
    FROM sys.types JOIN sys.schemas   
        ON sys.types.schema_id = sys.schemas.schema_id   
    WHERE sys.types.name = 'TestType' ;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-transferring-ownership-of-a-table"></a>C. Übertragen des Besitzes einer Tabelle  
 Im folgenden Beispiel werden eine `Region`-Tabelle im `dbo`-Schema und ein `Sales`-Schema erstellt. Anschließend wird die `Region`-Tabelle aus dem `dbo`-Schema in das `Sales`-Schema verschoben.  
  
```sql  
CREATE TABLE dbo.Region   
    (Region_id INT NOT NULL,  
    Region_Name CHAR(5) NOT NULL)  
WITH (DISTRIBUTION = REPLICATE);  
GO  
  
CREATE SCHEMA Sales;  
GO  
  
ALTER SCHEMA Sales TRANSFER OBJECT::dbo.Region;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/create-schema-transact-sql.md)   
 [DROP SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/drop-schema-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

