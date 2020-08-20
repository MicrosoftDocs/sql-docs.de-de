---
description: ENABLE TRIGGER (Transact-SQL)
title: ENABLE TRIGGER (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ENABLE TRIGGER
- ENABLE_TSQL
- ENABLE_TRIGGER_TSQL
- ENABLE
dev_langs:
- TSQL
helpviewer_keywords:
- DDL triggers, enabling
- triggers [SQL Server], enabling
- DML triggers, enabling
- ENABLE TRIGGER statement
ms.assetid: 6e21f0ad-68d0-432f-9c7c-a119dd2d3fc9
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 31f558799af8244bf14faeff1af16717ead48417
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88496693"
---
# <a name="enable-trigger-transact-sql"></a>ENABLE TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Aktiviert einen DML-, DDL- oder LOGON-Trigger.  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
ENABLE TRIGGER { [ schema_name . ] trigger_name [ ,...n ] | ALL }  
ON { object_name | DATABASE | ALL SERVER } [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*schema_name*  
Der Name des Schemas, zu dem der Trigger gehört. *schema_name* kann für DDL- oder LOGON-Trigger nicht angegeben werden.  
  
*trigger_name*  
Der Name des Triggers, der aktiviert werden soll.  
  
ALL  
Gibt an, dass alle im Bereich der ON-Klausel definierten Trigger aktiviert sind.  
  
*object_name*  
Der Name der Tabelle oder Sicht, in der der DML-Trigger *trigger_name* zur Ausführung erstellt wurde.  
  
DATABASE  
Für einen DDL-Trigger wird dadurch angegeben, dass *trigger_name* zur Ausführung mit dem Datenbankbereich erstellt oder geändert wurde.  
  
ALL SERVER  
**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
Für einen DDL-Trigger wird hiermit angegeben, dass *trigger_name* zur Ausführung mit dem Serverbereich erstellt oder geändert wurde. ALL SERVER gilt auch für LOGON-Trigger.  
  
> [!NOTE]  
>  Diese Option ist in einer enthaltenen Datenbank nicht verfügbar.  
  
## <a name="remarks"></a>Bemerkungen  
Durch das Aktivieren eines Triggers wird dieser nicht neu erstellt. Ein deaktivierter Trigger ist weiterhin als Objekt in der aktuellen Datenbank vorhanden, wird jedoch nicht ausgelöst. Durch das Aktivieren eines Triggers wird dieser ausgelöst, wenn eine beliebige [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung ausgeführt wird, für die er ursprünglich programmiert wurde. Trigger werden mit [DISABLE TRIGGER](../../t-sql/statements/disable-trigger-transact-sql.md) deaktiviert. Für Tabellen definierte DML-Trigger können auch mithilfe von [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) deaktiviert oder aktiviert werden.  
  
## <a name="permissions"></a>Berechtigungen  
Zum Aktivieren eines DML-Triggers benötigt der Benutzer mindestens die ALTER-Berechtigung für die Tabelle oder Sicht, in der der Trigger erstellt wurde.  
  
Zum Aktivieren eines DDL-Triggers mit Serverbereich (ON ALL SERVER) oder eines LOGON-Triggers benötigt der Benutzer die CONTROL SERVER-Berechtigung auf dem Server. Zum Aktivieren eines DDL-Triggers mit Datenbankbereich (ON DATABASE) benötigt der Benutzer mindestens die ALTER ANY DATABASE DDL TRIGGER-Berechtigung in der aktuellen Datenbank.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-enabling-a-dml-trigger-on-a-table"></a>A. Aktivieren eines DML-Triggers für eine Tabelle  
Im folgenden Beispiel wird der `uAddress`-Trigger, der für die `Address`-Tabelle in der AdventureWorks-Datenbank erstellt wurde, deaktiviert und anschließend aktiviert.  
  
```sql  
DISABLE TRIGGER Person.uAddress ON Person.Address;  
GO  
ENABLE Trigger Person.uAddress ON Person.Address;  
GO  
```  
  
### <a name="b-enabling-a-ddl-trigger"></a>B. Aktivieren eines DDL-Triggers  
Im folgenden Beispiel wird der DDL-Trigger `safety` mit einem Datenbankbereich erstellt und anschließend deaktiviert bzw. aktiviert.  
  
```sql  
CREATE TRIGGER safety   
ON DATABASE   
FOR DROP_TABLE, ALTER_TABLE   
AS   
   PRINT 'You must disable Trigger "safety" to drop or alter tables!'   
   ROLLBACK;  
GO  
DISABLE TRIGGER safety ON DATABASE;  
GO  
ENABLE TRIGGER safety ON DATABASE;  
GO  
```  
  
### <a name="c-enabling-all-triggers-that-were-defined-with-the-same-scope"></a>C. Aktivieren aller Trigger, die mit dem gleichen Bereich definiert wurden  
Im folgenden Beispiel werden alle DDL-Trigger aktiviert, die im Serverbereich erstellt wurden.  
  
**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
```sql  
ENABLE Trigger ALL ON ALL SERVER;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)  
  
  
