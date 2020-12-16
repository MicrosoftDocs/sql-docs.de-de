---
description: DB_ID (Transact-SQL)
title: DB_ID (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/13/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DB_ID_TSQL
- DB_ID
dev_langs:
- TSQL
helpviewer_keywords:
- viewing database ID numbers
- IDs [SQL Server], databases
- database IDs [SQL Server]
- identification numbers [SQL Server], databases
- displaying database ID numbers
- DB_ID function
ms.assetid: 7b3aef89-a6fd-4144-b468-bf87ebf381b8
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ce91e79d28bbf701b3cd9bd10fb6c3e3f17e2c41
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472121"
---
# <a name="db_id-transact-sql"></a>DB_ID (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Diese Funktion gibt die Datenbank-ID für eine angegebene Datenbank zurück.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DB_ID ( [ 'database_name' ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
'*database_name*'  
Der Name der Datenbank, deren Datenbank-ID von `DB_ID` zurückgegeben wird. Wenn *database_name* beim Aufruf von `DB_ID` ausgelassen wird, gibt `DB_ID` die ID der aktuellen Datenbank zurück.
  
## <a name="return-types"></a>Rückgabetypen
**int**

## <a name="remarks"></a>Bemerkungen
`DB_ID` wird möglicherweise nur verwendet, um die Datenbank-ID der aktuellen Datenbank in Azure SQL-Datenbank zurückzugeben. NULL wird zurückgegeben, wenn der angegebene Datenbankname von dem der aktuellen Datenbank abweicht.

> [!NOTE]
> Bei der Verwendung mit Azure SQL-Datenbank gibt `DB_ID` möglicherweise nicht dasselbe Ergebnis zurück wie das Abfragen von `database_id` in **sys.databases**. Wenn der Aufrufer von `DB_ID` das Ergebnis mit anderen **sys**-Ansichten vergleicht, sollte stattdessen **sys.databases** abgefragt werden.
  
## <a name="permissions"></a>Berechtigungen  
Wenn der Aufrufer von `DB_ID` keine spezifische Nicht-**Master**- oder Nicht-**tempdb**-Datenbank besitzt, sind mindestens die Berechtigungen `ALTER ANY DATABASE` oder `VIEW ANY DATABASE` auf Serverebene erforderlich, um die entsprechende `DB_ID`-Zeile anzuzeigen. `DB_ID` benötigt zumindest die Berechtigung `CREATE DATABASE` für die **Master**-Datenbank. Die Datenbank, mit der der Aufrufer eine Verbindung herstellt, wird immer in **sys.databases** angezeigt.
  
> [!IMPORTANT]  
>  Standardmäßig verfügt die öffentliche Rolle über die Berechtigung `VIEW ANY DATABASE`, sodass alle Anmeldenamen auf Datenbankinformationen zugreifen können. Verhindern Sie, dass ein Anmeldename eine Datenbank erkennt, indem Sie die Berechtigung `VIEW ANY DATABASE` mit `REVOKE` widerrufen, sodass sie nicht mehr öffentlich ist, oder die Berechtigung `VIEW ANY DATABASE` mit `DENY` für individuelle Anmeldungen verweigern.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-returning-the-database-id-of-the-current-database"></a>A. Zurückgeben der Datenbank-ID der aktuellen Datenbank  
Im folgenden Beispiel wird die Datenbank-ID der aktuellen Datenbank zurückgegeben.
  
```sql
SELECT DB_ID() AS [Database ID];  
GO  
```  
  
### <a name="b-returning-the-database-id-of-a-specified-database"></a>B. Zurückgeben der Datenbank-ID einer angegebenen Datenbank  
Im folgenden Beispiel wird die Datenbank-ID der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank zurückgegeben.
  
```sql
SELECT DB_ID(N'AdventureWorks2008R2') AS [Database ID];  
GO  
```  
  
### <a name="c-using-db_id-to-specify-the-value-of-a-system-function-parameter"></a>C. Angeben des Werts eines Systemfunktionsparameters mithilfe von DB_ID  
Im folgenden Beispiel wird mithilfe von `DB_ID` die Datenbank-ID der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank in der Systemfunktion `sys.dm_db_index_operational_stats` zurückgegeben. Der erste Parameter dieser Funktion ist eine Datenbank-ID.
  
```sql
DECLARE @db_id INT;  
DECLARE @object_id INT;  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');  
IF @db_id IS NULL   
  BEGIN;  
    PRINT N'Invalid database';  
  END;  
ELSE IF @object_id IS NULL  
  BEGIN;  
    PRINT N'Invalid object';  
  END;  
ELSE  
  BEGIN;  
    SELECT * FROM sys.dm_db_index_operational_stats(@db_id, @object_id, NULL, NULL);  
  END;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-return-the-id-of-the-current-database"></a>D: Rückgabe der ID der aktuellen Datenbank  
Im folgenden Beispiel wird die Datenbank-ID der aktuellen Datenbank zurückgegeben.
  
```sql
SELECT DB_ID();  
```  
  
### <a name="e-return-the-id-of-a-named-database"></a>E. Zurückgeben der ID der benannten Datenbank.  
Im folgenden Beispiel wird die Datenbank-ID der AdventureWorksDW2012-Datenbank zurückgegeben.
  
```sql
SELECT DB_ID('AdventureWorksPDW2012');  
```  
  
## <a name="see-also"></a>Siehe auch
[DB_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/db-name-transact-sql.md)  
[Metadata Functions &#40;Transact-SQL&#41; (Metadatenfunktionen (Transact-SQL))](../../t-sql/functions/metadata-functions-transact-sql.md)  
[sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
[sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)
  
  

