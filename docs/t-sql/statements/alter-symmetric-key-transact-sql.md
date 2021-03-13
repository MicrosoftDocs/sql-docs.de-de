---
description: ALTER SYMMETRIC KEY (Transact-SQL)
title: ALTER SYMMETRIC KEY (Transact-SQL)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER SYMMETRIC KEY
- ALTER_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- modifying symmetric keys
- encryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], modifying
- cryptography [SQL Server], symmetric keys
- ALTER SYMMETRIC KEY statement
ms.assetid: d3c776a4-7d71-4e6f-84fc-1db47400c465
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: e233b2a262edf8866e2080c9718a5bbcd29411b6
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421901"
---
# <a name="alter-symmetric-key-transact-sql"></a>ALTER SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Ändert die Eigenschaften eines symmetrischen Schlüssels.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
ALTER SYMMETRIC KEY Key_name <alter_option>  
  
<alter_option> ::=  
   ADD ENCRYPTION BY <encrypting_mechanism> [ , ... n ]  
   |   
   DROP ENCRYPTION BY <encrypting_mechanism> [ , ... n ]  
<encrypting_mechanism> ::=  
   CERTIFICATE certificate_name  
   |  
   PASSWORD = 'password'  
   |  
   SYMMETRIC KEY Symmetric_Key_Name  
   |  
   ASYMMETRIC KEY Asym_Key_Name  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *Key_name*  
 Der Name des zu ändernden symmetrischen Schlüssels in der Datenbank.  
  
 ADD ENCRYPTION BY   
 Fügt die Verschlüsselung mithilfe der angegebenen Methode hinzu.  
  
 DROP ENCRYPTION BY  
 Löscht die Verschlüsselung mithilfe der angegebenen Methode. Es können nicht alle Verschlüsselungen für einen symmetrischen Schlüssel entfernt werden.  
  
 CERTIFICATE *certificate_name*  
 Gibt das zum Verschlüsseln des symmetrischen Schlüssels verwendete Zertifikat an. Dieses Zertifikat muss bereits in der Datenbank vorhanden sein.  
  
 PASSWORD **='** _password_ **'**  
 Gibt das zum Verschlüsseln des symmetrischen Schlüssels verwendete Kennwort an. *password* muss den Anforderungen der Windows-Kennwortrichtlinien des Computers entsprechen, auf dem die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird.  
  
 SYMMETRIC KEY *Symmetric_Key_Name*  
 Gibt den symmetrischen Schlüssel an, der zum Verschlüsseln des zu ändernden symmetrischen Schlüssels verwendet wird. Dieser symmetrische Schlüssel muss bereits in der Datenbank vorhanden und geöffnet sein.  
  
 ASYMMETRIC KEY *Asym_Key_Name*  
 Gibt den asymmetrischen Schlüssel an, der zum Verschlüsseln des zu ändernden symmetrischen Schlüssels verwendet wird. Dieser asymmetrische Schlüssel muss bereits in der Datenbank vorhanden sein.  
  
## <a name="remarks"></a>Bemerkungen  
  
> [!CAUTION]  
>  Wenn ein symmetrischer Schlüssel mit einem Kennwort anstatt mit einem öffentlichen Schlüssel des Datenbank-Hauptschlüssels verschlüsselt ist, wird der TRIPLE_DES-Verschlüsselungsalgorithmus verwendet. Daher werden Schlüssel, die mit einem starken Verschlüsselungsalgorithmus wie z. B. AES erstellt werden, selbst mit einem schwächeren Algorithmus verschlüsselt.  
  
 Sie können die Verschlüsselung des symmetrischen Schlüssels ändern, indem Sie die Ausdrücke ADD ENCRYPTION und DROP ENCRYPTION verwenden. Ein Schlüssel kann nicht unverschlüsselt vorhanden sein. Daher ist es die bewährte Methode, die neue Form der Verschlüsselung hinzuzufügen, bevor die alte Form der Verschlüsselung entfernt wird.  
  
 Wenn Sie den Besitzer eines symmetrischen Schlüssels ändern möchten, können Sie [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md) verwenden.  
  
> [!NOTE]  
>  Der RC4-Algorithmus wird nur aus Gründen der Abwärtskompatibilität unterstützt. Neues Material kann nur mit RC4 oder RC4_128 verschlüsselt werden, wenn die Datenbank den Kompatibilitätsgrad 90 oder 100 besitzt. (Nicht empfohlen.) Verwenden Sie stattdessen einen neueren Algorithmus, z. B. einen der AES-Algorithmen. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] kann mit RC4 oder RC4_128 verschlüsseltes Material in jedem Kompatibilitätsgrad entschlüsselt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER-Berechtigung für den symmetrischen Schlüssel. Falls die Verschlüsselung mit einem Zertifikat oder asymmetrischen Schlüssel hinzugefügt wird, ist die VIEW DEFINITION-Berechtigung für das Zertifikat oder den asymmetrischen Schlüssel erforderlich. Falls die Verschlüsselung mit einem Zertifikat oder asymmetrischen Schlüssel gelöscht wird, ist die CONTROL-Berechtigung für das Zertifikat oder den asymmetrischen Schlüssel erforderlich.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die Verschlüsselungsmethode geändert, die zum Schutz eines symmetrischen Schlüssels verwendet wird. Der symmetrische Schlüssel `JanainaKey043` wird mithilfe des Zertifikats `Shipping04` bei der Erstellung des Schlüssels verschlüsselt. Da der Schlüssel nicht ohne Verschlüsselung gespeichert werden kann, wird in diesem Beispiel die Verschlüsselung mit einem Kennwort hinzugefügt. Anschließend wird die Verschlüsselung durch das Zertifikat entfernt.  
  
```sql  
CREATE SYMMETRIC KEY JanainaKey043 WITH ALGORITHM = AES_256   
    ENCRYPTION BY CERTIFICATE Shipping04;  
-- Open the key.   
OPEN SYMMETRIC KEY JanainaKey043 DECRYPTION BY CERTIFICATE Shipping04  
    WITH PASSWORD = '<enterStrongPasswordHere>';   
-- First, encrypt the key with a password.  
ALTER SYMMETRIC KEY JanainaKey043   
    ADD ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>';  
-- Now remove encryption by the certificate.  
ALTER SYMMETRIC KEY JanainaKey043   
    DROP ENCRYPTION BY CERTIFICATE Shipping04;  
CLOSE SYMMETRIC KEY JanainaKey043;  
```  
  
## <a name="see-also"></a>Siehe auch  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [CLOSE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/close-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
