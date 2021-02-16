---
description: FILE_IDEX (Transact-SQL)
title: FILE_IDEX (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FILE_IDEX
- FILE_IDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- FILE_IDEX function
- IDs [SQL Server], files
- file IDs [SQL Server]
- names [SQL Server], files
- identification numbers [SQL Server], files
- file names [SQL Server], FILE_IDEX
ms.assetid: 7532fea5-ee5e-4edd-b98b-111a7ba56c8e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 70fe6ce44d8c3a9d1d4aefd1b53070eae767e5c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350843"
---
# <a name="file_idex-transact-sql"></a>FILE_IDEX (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Diese Funktion gibt die Datei-ID für den angegebenen logischen Dateinamen der Daten-, Protokoll- oder Volltextdatei in der aktuellen Datenbank zurück. 
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
FILE_IDEX ( file_name )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *file_name*  
Ein Ausdruck vom Typ **sysname**, der den Datei-ID-Wert „FILE_IDEX“ für den Namen der Datei zurückgibt. 
  
## <a name="return-types"></a>Rückgabetypen  
**int**  
  
**NULL** bei Fehler  
  
## <a name="remarks"></a>Hinweise  
*file_name* entspricht dem logischen Dateinamen, der in der Spalte **name** in den Katalogsichten [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md) oder [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) angezeigt wird.  
  
Verwenden Sie `FILE_IDEX` in einer SELECT-Liste, einer WHERE-Klausel oder an einer beliebigen Stelle, an der ein Ausdruck zulässig ist. Weitere Informationen finden Sie unter [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-retrieving-the-file-id-of-a-specified-file"></a>A. Abrufen der Datei-ID einer angegebenen Datei  
In diesem Beispiel wird die Datei-ID für die Datei `AdventureWorks_Data` zurückgegeben.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT FILE_IDEX('AdventureWorks2012_Data') AS 'File ID';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
File ID   
-------   
1  
(1 row(s) affected)  
```  
  
### <a name="b-retrieving-the-file-id-when-the-file-name-is-not-known"></a>B. Abrufen der Datei-ID, wenn der Dateiname nicht bekannt ist  
In diesem Beispiel wird die Datei-ID für die Protokolldatei `AdventureWorks` zurückgegeben. Der Transact-SQL-Codeausschnitt (T-SQL) wählt den logischen Dateinamen aus der Katalogsicht `sys.database_files` aus, wobei der Dateityp `1` entspricht (Protokoll).  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT FILE_IDEX((SELECT TOP (1) name FROM sys.database_files WHERE type = 1)) AS 'File ID';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
File ID   
-------   
2  
```  
  
### <a name="c-retrieving-the-file-id-of-a-full-text-catalog-file"></a>C. Abrufen der Datei-ID einer Volltextkatalogdatei  
In diesem Beispiel wird die Datei-ID einer Volltextdatei zurückgegeben. Der Transact-SQL-Codeausschnitt (T-SQL) wählt den logischen Dateinamen aus der Katalogsicht `sys.database_files` aus, wobei der Dateityp `4` entspricht (Volltextdatei). Dieser Code gibt NULL zurück, wenn kein Volltextkatalog vorhanden ist.
  
```sql  
SELECT FILE_IDEX((SELECT name FROM sys.master_files WHERE type = 4))  
AS 'File_ID';  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Metadata Functions &#40;Transact-SQL&#41; (Metadatenfunktionen &#40;Transact-SQL&#41;)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
