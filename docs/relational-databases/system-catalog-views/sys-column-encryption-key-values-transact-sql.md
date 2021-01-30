---
description: sys.column_encryption_key_values (Transact-SQL)
title: sys.column_encryption_key_values (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- column_encryption_key_values
- column_encryption_key_values_TSQL
- sys.column_encryption_key_values
- sys.column_encryption_key_values_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_encryption_key_values catalog view
ms.assetid: 440875ab-b0e9-4966-8c16-01503558fedd
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6217afd2b80b7d68fc037a24b0c3c71f1030a761
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198759"
---
# <a name="syscolumn_encryption_key_values-transact-sql"></a>sys.column_encryption_key_values (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt Informationen zu verschlüsselten Werten von Spalten Verschlüsselungsschlüsseln (ceks) zurück, die entweder mit dem [Create Column Encryption Key](../../t-sql/statements/create-column-encryption-key-transact-sql.md) -oder dem [ALTER COLUMN Encryption Key-&#40;Transact-SQL&#41;](../../t-sql/statements/alter-column-encryption-key-transact-sql.md) -Anweisung erstellt wurden. Jede Zeile stellt den Wert eines Cek dar, der mit einem Spalten Hauptschlüssel (Column Master Key, CMK) verschlüsselt ist.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**column_encryption_key_id**|**int**|ID des Cek in der Datenbank.|  
|**column_master_key_id**|**int**|ID des Spalten Hauptschlüssels, der verwendet wurde, um den Cek-Wert zu verschlüsseln.|  
|**encrypted_value**|**varbinary(8000)**|Der Cek-Wert, der mit dem in column_master_key_id angegebenen CMK verschlüsselt ist.|  
|**encryption_algorithm_name**|**sysname**|Der Name eines Algorithmus, der zum Verschlüsseln des Cek-Werts verwendet wird.<br /><br /> Der Name des Verschlüsselungsalgorithmus, der zum Verschlüsseln des Werts verwendet wird. Der Algorithmus für die Systemanbieter muss  **RSA_OAEP** werden.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die **View any Column Encryption Key** -Berechtigung.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-encryption-key-transact-sql.md)   
 [ALTER COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-column-encryption-key-transact-sql.md)   
 [DROP COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-column-encryption-key-transact-sql.md)   
 [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-master-key-transact-sql.md)   
 [Sicherheitskatalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [sys.column_encryption_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)   
 [sys.column_master_keys &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md)   
 [Übersicht über die Schlüsselverwaltung für Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)   
 [Verwalten von Schlüsseln für Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)   

  
  
