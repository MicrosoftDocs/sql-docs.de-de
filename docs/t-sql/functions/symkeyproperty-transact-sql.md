---
description: SYMKEYPROPERTY (Transact-SQL)
title: SYMKEYPROPERTY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SYMKEYPROPERTY_TSQL
- SYMKEYPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- SYMKEYPROPERTY
ms.assetid: 3d1f7075-3a3c-4660-8cd0-ed938b86fecd
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 5a612e2a503009598f48d6c999e75a29de0b5dd9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205475"
---
# <a name="symkeyproperty-transact-sql"></a>SYMKEYPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt den Algorithmus eines aus einem EKM-Modul erstellten symmetrischen Schlüssels zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
SYMKEYPROPERTY ( Key_ID , 'algorithm_desc' | 'string_sid' | 'sid' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *Key_ID*  
 Die Key_ID eines symmetrischen Schlüssels in der Datenbank. Um die Key_ID zu ermitteln, wenn Sie nur den Schlüsselnamen kennen, verwenden Sie die SYMKEY_ID. *Key_ID* ist vom Datentyp **int**.  
  
 **‚** algorithm_desc **’**  
 Gibt an, dass die Ausgabe die Algorithmusbeschreibung des symmetrischen Schlüssels zurückgibt. Nur verfügbar für aus einem EKM-Modul erstellte symmetrische Schlüssel.  
  
## <a name="return-types"></a>Rückgabetypen  
 **sql_variant**  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert bestimmte Berechtigungen für den symmetrischen Schlüssel, und dem Aufrufer darf die VIEW-Berechtigung für den symmetrischen Schlüssel nicht verweigert worden sein.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel gibt den Algorithmus des symmetrischen Schlüssels mit der Key_ID 256 zurück.  
  
```sql  
SELECT SYMKEYPROPERTY(256, 'algorithm_desc') AS Algorithm ;  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [ASYMKEY_ID &#40;Transact-SQL&#41;](../../t-sql/functions/asymkey-id-transact-sql.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.symmetric_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [Sicherheitskatalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [KEY_ID &#40;Transact-SQL&#41;](../../t-sql/functions/key-id-transact-sql.md)   
 [ASYMKEYPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/asymkeyproperty-transact-sql.md)  
  
  
