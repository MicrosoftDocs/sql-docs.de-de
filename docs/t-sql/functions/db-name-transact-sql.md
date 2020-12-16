---
description: DB_NAME (Transact-SQL)
title: DB_NAME (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DB_NAME
- DB_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database names [SQL Server], DB_NAME
- names [SQL Server], databases
- viewing database names
- displaying database names
- DB_NAME function
ms.assetid: e21fb33a-a3ea-49b0-bb6b-8f789a675a0e
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9650590f08469e7f69d8e76559b62500af8c5fe8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478651"
---
# <a name="db_name-transact-sql"></a>DB_NAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Diese Funktion gibt den Namen einer angegebenen Datenbank zurück.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DB_NAME ( [ database_id ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*database_id*  

Die Identifikationsnummer (ID) der Datenbank, deren Name von `DB_NAME` zurückgegeben wird. Wenn *database_id* beim Aufruf von `DB_NAME` ausgelassen wird, gibt `DB_NAME` den Namen der aktuellen Datenbank zurück.
  
## <a name="return-types"></a>Rückgabetypen
**nvarchar(128)**
  
## <a name="permissions"></a>Berechtigungen  

Wenn der Aufrufer von `DB_NAME` keine spezifische Nicht-**Master**- oder Nicht-**tempdb**-Datenbank besitzt, sind mindestens die Berechtigungen `ALTER ANY DATABASE` oder `VIEW ANY DATABASE` auf Serverebene erforderlich, um die entsprechende `DB_ID`-Zeile anzuzeigen. `DB_ID` benötigt zumindest die Berechtigung `CREATE DATABASE` für die **Master**-Datenbank. Die Datenbank, mit der der Aufrufer eine Verbindung herstellt, wird immer in **sys.databases** angezeigt.
  
> [!IMPORTANT]  
>  Standardmäßig verfügt die öffentliche Rolle über die Berechtigung `VIEW ANY DATABASE`, sodass alle Anmeldenamen auf Datenbankinformationen zugreifen können. Verhindern Sie, dass ein Anmeldename eine Datenbank erkennt, indem Sie die Berechtigung `VIEW ANY DATABASE` mit `REVOKE` widerrufen, sodass sie nicht mehr öffentlich ist, oder die Berechtigung `VIEW ANY DATABASE` mit `DENY` für individuelle Anmeldungen verweigern.
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-returning-the-current-database-name"></a>A. Zurückgeben des aktuellen Datenbanknamens  
In diesem Beispiel wird der Name der aktuellen Datenbank zurückgegeben.
  
```sql
SELECT DB_NAME() AS [Current Database];  
GO  
```  
  
### <a name="b-returning-the-database-name-of-a-specified-database-id"></a>B. Zurückgeben des Datenbanknamens einer angegebenen Datenbank-ID  
Im folgenden Beispiel wird der Name der Datenbank mit der ID `3` zurückgegeben.
  
```sql
USE master;  
GO  
SELECT DB_NAME(3) AS [Database Name];  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-return-the-current-database-name"></a>C. Zurückgeben des aktuellen Datenbanknamens  
  
```sql
SELECT DB_NAME() AS [Current Database];  
```  
  
### <a name="d-return-the-name-of-a-database-by-using-the-database-id"></a>D: Zurückgeben des Namens einer Datenbank unter Verwendung der Datenbank-ID  
Im folgenden Beispiel werden die Datenbanknamen und database_id aller Datenbanken zurückgegeben.
  
```sql
SELECT DB_NAME(database_id) AS [Database], database_id  
FROM sys.databases;  
```  
  
## <a name="see-also"></a>Siehe auch
[DB_ID &#40;Transact-SQL&#41;](../../t-sql/functions/db-id-transact-sql.md)  
[Metadata Functions &#40;Transact-SQL&#41; (Metadatenfunktionen (Transact-SQL))](../../t-sql/functions/metadata-functions-transact-sql.md)  
[sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
  
  

