---
description: DROP DATABASE ENCRYPTION KEY (Transact-SQL)
title: DROP DATABASE ENCRYPTION KEY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP DATABASE ENCRYPTION KEY
- DROP_DATABASE_ENCRYPTION_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database encryption key, drop
- DROP DATABASE ENCRYPTION KEY statement
ms.assetid: 9231bd89-75e1-45c4-b4c8-13f08695af68
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 36e0c4d453c646b7fe3136d33337a604efe89bcc
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186617"
---
# <a name="drop-database-encryption-key-transact-sql"></a>DROP DATABASE ENCRYPTION KEY (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Löscht einen Verschlüsselungsschlüssel für eine Datenbank, der bei der transparenten Datenbankverschlüsselung verwendet wird. Weitere Informationen zur transparenten Datenbankverschlüsselung finden Sie unter [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  
  
> [!IMPORTANT]  
>  Auch wenn die Verschlüsselung einer Datenbank deaktiviert wurde, sollte immer eine Sicherung des Zertifikats beibehalten werden, mit dem die Datenbankverschlüsselung geschützt wurde. Selbst wenn die Datenbank nicht mehr verschlüsselt ist, können Teile des Transaktionsprotokolls nach wie vor geschützt sein. Für bestimmte Vorgänge wird das Zertifikat ggf. weiterhin benötigt, bis eine vollständige Sicherung der Datenbank ausgeführt wurde.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
DROP DATABASE ENCRYPTION KEY  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Bemerkungen
 Wenn die Datenbank verschlüsselt ist, müssen Sie die Verschlüsselung der Datenbank zunächst mit der ALTER DATABASE-Anweisung aufheben. Warten Sie, bis die Entschlüsselung abgeschlossen ist, bevor Sie den Verschlüsselungsschlüssel der Datenbank entfernen. Weitere Informationen zur ALTER DATABASE-Anweisung finden Sie unter [ALTER DATABASE SET-Optionen &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md). Der Status der Datenbank wird mit der dynamischen Verwaltungssicht [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) angezeigt.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die CONTROL-Berechtigung für die Datenbank.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die Datenbankverschlüsselung entfernt und der Datenbank-Verschlüsselungsschlüssel gelöscht.  
  
```sql  
ALTER DATABASE AdventureWorks2012  
SET ENCRYPTION OFF;  
GO  
/* Wait for decryption operation to complete, look for a   
value of  1 in the query below. */  
SELECT encryption_state  
FROM sys.dm_database_encryption_keys;  
GO  
USE AdventureWorks2012;  
GO  
DROP DATABASE ENCRYPTION KEY;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Im folgenden Beispiel wird die TDE-Verschlüsselung entfernt und der Datenbank-Verschlüsselungsschlüssel gelöscht.  
  
```sql  
ALTER DATABASE AdventureWorksPDW2012  
    SET ENCRYPTION OFF;  
GO  
/* Wait for decryption operation to complete, look for a   
value of  1 in the query below. */  
WITH dek_encryption_state AS   
(  
    SELECT ISNULL(db_map.database_id, dek.database_id) AS database_id, encryption_state  
    FROM sys.dm_pdw_nodes_database_encryption_keys AS dek  
        INNER JOIN sys.pdw_nodes_pdw_physical_databases AS node_db_map  
           ON dek.database_id = node_db_map.database_id AND dek.pdw_node_id = node_db_map.pdw_node_id  
        LEFT JOIN sys.pdw_database_mappings AS db_map  
            ON node_db_map .physical_name = db_map.physical_name  
        INNER JOIN sys.dm_pdw_nodes AS nodes  
            ON nodes.pdw_node_id = dek.pdw_node_id  
    WHERE dek.encryptor_thumbprint <> 0x  
)  
SELECT TOP 1 encryption_state  
       FROM  dek_encryption_state  
       WHERE dek_encryption_state.database_id = DB_ID('AdventureWorksPDW2012 ')  
       ORDER BY (CASE encryption_state WHEN 3 THEN -1 ELSE encryption_state END) DESC;   
GO  
USE AdventureWorksPDW2012;  
GO  
DROP DATABASE ENCRYPTION KEY;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Transparente Datenverschlüsselung &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md)   
 [SQL Server-Verschlüsselung](../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Verschlüsselungsschlüssel für SQL Server und Datenbank &#40;Datenbank-Engine&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [ALTER DATABASE SET-Optionen &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [CREATE DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-encryption-key-transact-sql.md)   
 [ALTER DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-encryption-key-transact-sql.md)   
 [sys.dm_database_encryption_keys &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)  
  
  

