---
description: FILEPROPERTYEX (Transact-SQL)
title: FILEPROPERTYEX (Transact-SQL) | Microsoft-Dokumentation
ms.date: 07/23/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FILEPROPERTYEX_TSQL
- FILEPROPERTYEX
dev_langs:
- TSQL
helpviewer_keywords:
- viewing file properties
- displaying file properties
- file properties [SQL Server]
- FILEPROPERTYEX function
- file names [SQL Server], FILEPROPERTYEX
author: stevestein
ms.author: sstein
ms.openlocfilehash: c0d486f72f1f63a49916cadd3b32c096e60d0061
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350828"
---
# <a name="filepropertyex-transact-sql"></a>FILEPROPERTYEX (Transact-SQL)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Gibt den Wert für die angegebene erweiterte Dateieigenschaft zurück, wenn ein Dateiname in der aktuellen Datenbank und ein Eigenschaftsname angegeben sind. Gibt für Dateien, die es in der aktuellen Datenbank nicht gibt, oder für erweiterte Dateieigenschaften, die nicht vorhanden sind, den Wert NULL zurück. Derzeit gibt es erweiterte Dateieigenschaften nur für Datenbanken, die sich in Azure Blob-Speicher befinden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
FILEPROPERTYEX ( name , property )  
```  
  
## <a name="arguments"></a>Argumente  
 *name*  
 Ein Ausdruck, der den Namen der Datei enthält, die der aktuellen Datenbank zugeordnet ist, für die Eigenschaftsinformationen zurückgegeben werden sollen. *file_name* ist vom Datentyp **nchar(128)**.  
  
 *property*  
 Ein Ausdruck, der den Namen der zurückzugebenden Dateieigenschaft enthält. *property* ist vom Datentyp **varchar(128)**. Die folgenden Werte sind möglich.  


  
|Wert|BESCHREIBUNG|
|-----------|-----------------|  
|**BlobTier**|Die Ebene des Azure-Zielseitenblobs. Gilt nur für Standard- und GeneralPurpose-Datenbanken, in denen Azure-Seitenblob-Speicher verwendet wird.|
|**AccountType**|Der Speicherkontotyp, der angibt, ob es sich um Blobspeicher oder Dateispeicher handelt und ob der Speicher Premium- oder Standard-Speicher ist.|
|**IsInferredTier**|Gibt an, ob die Ebene eine implizite (abgeleitete) Ebene, die mit der Datengröße vergrößert werden kann, oder eine explizite (feste) Ebene ist.|
|**IsPageBlob**|Gibt an, ob das Zielblob ein Seitenblob ist oder nicht.|
  
## <a name="return-types"></a>Rückgabetypen  
 **sql_variant**  
  
## <a name="remarks"></a>Bemerkungen  
 *file_name* entspricht der **name**-Spalte in den Katalogsichten **sys.master_files** oder **sys.database_files**.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die Einstellung für Datenbankdateien zurückgegeben:
```sql
SELECT s.file_id,
       s.type_desc,
       s.name,
       FILEPROPERTYEX(s.name, 'BlobTier') AS BlobTier,
       FILEPROPERTYEX(s.name, 'AccountType') AS AccountType,
       FILEPROPERTYEX(s.name, 'IsInferredTier') AS IsInferredTier,
       FILEPROPERTYEX(s.name, 'IsPageBlob') AS IsPageBlob
FROM sys.database_files AS s
WHERE s.type_desc IN ('ROWS', 'LOG');
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
file_id  type_desc  name  BlobTier  AccountType  IsInferredTier  IsPageBlob
--------------------------------------------------------------------------------------
1     ROWS      data_0  P30  PremiumBlobStorage  0   1
2     LOG       log     P30  PremiumBlobStorage  0   1

(2 rows affected)
```  
  
## <a name="see-also"></a>Siehe auch  
 [FILEGROUPPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/filegroupproperty-transact-sql.md)   
 [Metadata Functions &#40;Transact-SQL&#41; (Metadatenfunktionen &#40;Transact-SQL&#41;)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
