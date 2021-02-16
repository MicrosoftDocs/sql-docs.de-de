---
description: FILE_NAME (Transact-SQL)
title: FILE_NAME (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FILE_NAME_TSQL
- FILE_NAME
dev_langs:
- TSQL
helpviewer_keywords:
- viewing file names
- file names [SQL Server], FILE_NAME
- IDs [SQL Server], files
- file IDs [SQL Server]
- names [SQL Server], files
- displaying file names
- identification numbers [SQL Server], files
- FILE_NAME function
- logical file names [SQL Server]
ms.assetid: 68b298aa-ce47-4af5-b59f-9a1b46d48326
author: cawrites
ms.author: chadam
ms.openlocfilehash: 92d58be48c0e04fb28d81df0795eaad78fc9c111
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346051"
---
# <a name="file_name-transact-sql"></a>FILE_NAME (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Diese Funktion gibt den logischen Dateinamen für die angegebene Datei-ID zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
FILE_NAME ( file_id )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*file_id*  
Die Datei-ID, deren Dateiname `FILE_NAME` zurückgegeben wird. *file_id* weist den Datentyp **int** auf.  
  
## <a name="return-types"></a>Rückgabetypen  
**nvarchar(128)**  
  
## <a name="remarks"></a>Hinweise  
*file_ID* entspricht der „file_id“-Spalte in der Katalogsicht „sys.master_files“ oder in der Katalogsicht „sys.database_files“.  
  
## <a name="examples"></a>Beispiele  
In diesem Beispiel werden die Dateinamen für `file_ID 1` und `file_ID` in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank zurückgegeben.  
  
```sql  
SELECT FILE_NAME(1) AS 'File Name 1', FILE_NAME(2) AS 'File Name 2';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
File Name 1                File Name 2  
-------------------------  ------------------------  
AdventureWorks2012_Data    AdventureWorks2012_Log  

(1 row(s) affected)
``` 
  
## <a name="see-also"></a>Siehe auch  
 [FILE_IDEX &#40;Transact-SQL&#41;](../../t-sql/functions/file-idex-transact-sql.md)   
 [Metadata Functions &#40;Transact-SQL&#41; (Metadatenfunktionen &#40;Transact-SQL&#41;)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
