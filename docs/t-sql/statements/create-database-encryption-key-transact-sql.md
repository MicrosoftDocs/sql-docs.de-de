---
description: CREATE DATABASE ENCRYPTION KEY (Transact-SQL)
title: CREATE DATABASE ENCRYPTION KEY (Transact-SQL)
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASE_ENCRYPTION_KEY_TSQL
- ENCRYPTION_KEY_TSQL
- sql13.swb.dbencryptionkeyo.f1
- ENCRYPTION KEY
- DATABASE ENCRYPTION KEY
- CREATE_DATABASE_ENCRYPTION_KEY_TSQL
- CREATE DATABASE ENCRYPTION KEY
- sql13.swb.dbencryptionkeyg.f1
- CREATE DATABASE ENCRYPTION
- CREATE_DATABASE_ENCRYPTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database encryption key
- CREATE DATABASE ENCRYPTION KEY statement
- database encryption key, create
ms.assetid: 2ee95a32-5140-41bd-9ab3-a947b9990688
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: af62041c92ca78b52204aac7d736465d5d01012b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270397"
---
# <a name="create-database-encryption-key-transact-sql"></a>CREATE DATABASE ENCRYPTION KEY (Transact-SQL)

[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

 Erstellt einen Verschlüsselungsschlüssel für eine Datenbank, der für die transparente Datenbankverschlüsselung verwendet wird. Weitere Informationen zur transparenten Datenbankverschlüsselung finden Sie unter [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server  

CREATE DATABASE ENCRYPTION KEY  
       WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
   ENCRYPTION BY SERVER   
    {  
        CERTIFICATE Encryptor_Name |  
        ASYMMETRIC KEY Encryptor_Name  
    }  
[ ; ]  
```  
  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  

CREATE DATABASE ENCRYPTION KEY  
       WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
   ENCRYPTION BY SERVER CERTIFICATE Encryptor_Name   
[ ; ]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

WITH ALGORITHM = {AES_128 \| AES_192 \| AES_256 \| TRIPLE_DES_3KEY}  
Gibt den Verschlüsselungsalgorithmus an, der für den Verschlüsselungsschlüssel verwendet wird.

> [!WARNING]
> Ab SQL Server 2016 gelten andere Algorithmen als AES_128, AES_192 und AES_256 als veraltet. Um ältere Algorithmen zu verwenden (was nicht empfohlen wird), müssen Sie den Kompatibilitätsgrad zwischen Datenbanken auf höchsten 120 festlegen.  
  
ENCRYPTION BY SERVER CERTIFICATE Encryptor_Name  
Gibt den Namen der Verschlüsselung an, die zum Verschlüsseln des Verschlüsselungsschlüssels für die Datenbank verwendet wird.  
  
ENCRYPTION BY SERVER ASYMMETRIC KEY Encryptor_Name  
Gibt den Namen des asymmetrischen Schlüssels an, der zum Verschlüsseln des Verschlüsselungsschlüssels für die Datenbank verwendet wird. Um den Verschlüsselungsschlüssel für die Datenbank mit einem asymmetrischen Schlüssel zu verschlüsseln, muss sich der asymmetrische Schlüssel auf einem erweiterbaren Schlüsselverwaltungsanbieter befinden.  
  
## <a name="remarks"></a>Bemerkungen  
Bevor eine Datenbank mit *Transparent Database Encryption* (TDE) verschlüsselt werden kann, wird ein Verschlüsselungsschlüssel für eine Datenbank wird benötigt. Wenn eine Datenbank transparent verschlüsselt ist, so ist die gesamte Datenbank auf Dateiebene ohne spezielle Codeänderungen verschlüsselt. Das Zertifikat oder der asymmetrische Schlüssel, das bzw. der zum Verschlüsseln des Verschlüsselungsschlüssels für die Datenbank verwendet wird, muss sich in der master-Systemdatenbank befinden.  
  
Datenbankverschlüsselungsanweisungen sind nur für Benutzerdatenbanken zulässig.  
  
Der Verschlüsselungsschlüssel für die Datenbank kann nicht aus der Datenbank exportiert werden. Er ist nur für das System, für Benutzer, die Debugberechtigungen auf dem Server haben, und für Benutzer verfügbar, die Zugriff auf die Zertifikate zum Verschlüsseln und Entschlüssen des Verschlüsselungsschlüssels für die Datenbank haben.  
  
Der Verschlüsselungsschlüssel für die Datenbank muss nicht erneut generiert werden, wenn ein Datenbankbesitzer (DBO) geändert wird.  
  
Für eine [!INCLUDE[ssSDS](../../includes/sssds-md.md)]-Datenbank wird automatisch ein Verschlüsselungsschlüssel erstellt. Es ist nicht erforderlich, mit der CREATE DATABASE ENCRYPTION KEY-Anweisung einen Schlüssel zu erstellen.  
  
## <a name="permissions"></a>Berechtigungen  
Erfordert die CONTROL-Berechtigung für die Datenbank und die VIEW DEFINITION-Berechtigung für das Zertifikat oder den asymmetrischen Schlüssel, die zum Verschlüsseln des Verschlüsselungsschlüssels für die Datenbank verwendet werden.  
  
## <a name="examples"></a>Beispiele  
Weitere Beispiele zur Verwendung von TDE finden Sie unter [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md), [Aktivieren von TDE in SQL Server mithilfe von EKM](../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md) und [Erweiterbare Schlüsselverwaltung mit Azure Key Vault &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).  
  
Im folgenden Beispiel wird der Datenbank-Verschlüsselungsschlüssel mithilfe des `AES_256`-Algorithmus erstellt, und der private Schlüssel wird mit einem Zertifikat namens `MyServerCert` geschützt.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE DATABASE ENCRYPTION KEY  
WITH ALGORITHM = AES_256  
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[Transparente Datenverschlüsselung &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md)   
[SQL Server-Verschlüsselung](../../relational-databases/security/encryption/sql-server-encryption.md)   
[Verschlüsselungsschlüssel für SQL Server und Datenbank &#40;Datenbank-Engine&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
[Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)   
[ALTER DATABASE SET-Optionen &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
[ALTER DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-encryption-key-transact-sql.md)   
[DROP DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-encryption-key-transact-sql.md)   
[sys.dm_database_encryption_keys &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)  
    
