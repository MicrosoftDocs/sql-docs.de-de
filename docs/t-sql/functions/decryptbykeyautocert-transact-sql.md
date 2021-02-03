---
description: DECRYPTBYKEYAUTOCERT (Transact-SQL)
title: DECRYPTBYKEYAUTOCERT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/09/2015
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECRYPTBYKEYAUTOCERT
- DECRYPTBYKEYAUTOCERT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DECRYPTBYKEYAUTOCERT function
ms.assetid: 6b45fa2e-ffaa-46f7-86ff-5624596eda4a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 11cc7a88ad012554fac63b08030e31686ef1cc8b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207203"
---
# <a name="decryptbykeyautocert-transact-sql"></a>DECRYPTBYKEYAUTOCERT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Diese Funktion entschlüsselt Daten mit einem symmetrischen Schlüssel. Dieser symmetrische Schlüssel wird automatisch mit einem Zertifikat entschlüsselt.  

 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DecryptByKeyAutoCert ( cert_ID , cert_password   
    , { 'ciphertext' | @ciphertext }  
  [ , { add_authenticator | @add_authenticator }   
  [ , { authenticator | @authenticator } ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *CERT_ID*  
Die ID des Zertifikats, das zum Schützen des symmetrischen Schlüssels verwendet wird. *cert_ID* verfügt über den Datentyp **int**.  
  
*cert_password*  
Das Kennwort, das zum Verschlüsseln des privaten Schlüssels des Zertifikats verwendet wurde. Kann den Wert `NULL` aufweisen, wenn der Datenbank-Hauptschlüssel den privaten Schlüssel schützt. *cert_password* verfügt über einen **nvarchar**-Datentyp.  

'*ciphertext*'  
Die Zeichenfolge der Daten, die mit dem Schlüssel verschlüsselt wurden. *ciphertext* verfügt über einen **varbinary**-Datentyp.  

@ciphertext  
Eine Variable vom Typ **varbinary**, die Daten enthält, die mit dem Schlüssel verschlüsselt wurden.  

*add_authenticator*  
Gibt an, ob durch den ursprünglichen Verschlüsselungsprozess ein Authentifikator zusammen mit dem Klartext einbezogen und verschlüsselt wurde. Muss mit dem Wert übereinstimmen, der während der Datenverschlüsselung an [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) übergeben wurde. *add_authenticator* weist den Wert 1 auf, wenn im Verschlüsselungsprozess ein Authentifikator verwendet wurde. *add_authenticator* verfügt über einen **int**-Datentyp.  
  
@add_authenticator  
Eine Variable, die angibt, ob durch den ursprünglichen Verschlüsselungsprozess ein Authentifikator zusammen mit dem Klartext einbezogen, und verschlüsselt, wurde. Muss mit dem Wert übereinstimmen, der während der Datenverschlüsselung an [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) übergeben wurde. *\@add_authenticator* weist den Datentyp **int** auf.  
  
*authenticator*  
Die Daten, die als Grundlage für die Generierung des Authentifikators verwendet werden. Diese müssen mit dem Wert übereinstimmen, der für [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) bereitgestellt wurde. *authenticator* verfügt über einen **sysname**-Datentyp.  
  
@authenticator  
Eine Variable, die Daten für die Generierung durch den Authentifikator enthält. Diese müssen mit dem Wert übereinstimmen, der für [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) bereitgestellt wurde. *\@authenticator* weist den Datentyp **sysname** auf.  
  
## <a name="return-types"></a>Rückgabetypen  
**varbinary** mit einer maximalen Größe von 8.000 Byte.  
  
## <a name="remarks"></a>Bemerkungen  
`DECRYPTBYKEYAUTOCERT` kombiniert die Funktionen von `OPEN SYMMETRIC KEY` und `DECRYPTBYKEY`. In einem einzelnen Vorgang wird zunächst ein symmetrischer Schlüssel entschlüsselt, und mit diesem Schlüssel wird dann der verschlüsselte Chiffretext entschlüsselt.  
  
## <a name="permissions"></a>Berechtigungen  
Erfordert die `VIEW DEFINITION`-Berechtigung für den symmetrischen Schlüssel und die `CONTROL`-Berechtigung für das Zertifikat.   
  
## <a name="examples"></a>Beispiele  
In diesem Beispiel wird dargestellt, wie `DECRYPTBYKEYAUTOCERT` den Entschlüsselungscode vereinfachen kann. Dieser Code sollte für eine [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank ausgeführt werden, die nicht bereits über einen Datenbank-Hauptschlüssel verfügt.  
  
```sql  
--Create the keys and certificate.  
USE AdventureWorks2012;  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'mzkvdlk979438teag$$ds987yghn)(*&4fdg^';  
OPEN MASTER KEY DECRYPTION BY PASSWORD = 'mzkvdlk979438teag$$ds987yghn)(*&4fdg^';  
CREATE CERTIFICATE HumanResources037   
   WITH SUBJECT = 'Sammamish HR',   
   EXPIRY_DATE = '10/31/2009';  
CREATE SYMMETRIC KEY SSN_Key_01 WITH ALGORITHM = DES  
    ENCRYPTION BY CERTIFICATE HumanResources037;  
GO  
----Add a column of encrypted data.  
ALTER TABLE HumanResources.Employee  
    ADD EncryptedNationalIDNumber varbinary(128);   
OPEN SYMMETRIC KEY SSN_Key_01  
   DECRYPTION BY CERTIFICATE HumanResources037 ;  
UPDATE HumanResources.Employee  
SET EncryptedNationalIDNumber  
    = EncryptByKey(Key_GUID('SSN_Key_01'), NationalIDNumber);  
GO  
--  
--Close the key used to encrypt the data.  
CLOSE SYMMETRIC KEY SSN_Key_01;  
--  
--There are two ways to decrypt the stored data.  
--  
--OPTION ONE, using DecryptByKey()  
--1. Open the symmetric key  
--2. Decrypt the data  
--3. Close the symmetric key  
OPEN SYMMETRIC KEY SSN_Key_01  
   DECRYPTION BY CERTIFICATE HumanResources037;  
SELECT NationalIDNumber, EncryptedNationalIDNumber    
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKey(EncryptedNationalIDNumber))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
CLOSE SYMMETRIC KEY SSN_Key_01;  
--  
--OPTION TWO, using DecryptByKeyAutoCert()  
SELECT NationalIDNumber, EncryptedNationalIDNumber   
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKeyAutoCert ( cert_ID('HumanResources037') , NULL ,EncryptedNationalIDNumber))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../t-sql/functions/encryptbykey-transact-sql.md)   
 [DECRYPTBYKEY &#40;Transact-SQL&#41;](../../t-sql/functions/decryptbykey-transact-sql.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
