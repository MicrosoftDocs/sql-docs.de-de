---
description: DECRYPTBYCERT (Transact-SQL)
title: DECRYPTBYCERT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DecryptByCert_TSQL
- DECRYPTBYCERT
dev_langs:
- TSQL
helpviewer_keywords:
- certificates [SQL Server], decryption
- decryption [SQL Server], certificates
- DECRYPTBYCERT function
ms.assetid: 4950d787-40fa-4e26-bce8-2cb2ceca12fb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 792f4ec052b038e53565648f63fcbfb94144f49d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193722"
---
# <a name="decryptbycert-transact-sql"></a>DECRYPTBYCERT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Diese Funktion verwendet den privaten Schlüssel eines Zertifikats zum Entschlüsseln verschlüsselter Daten.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DecryptByCert ( certificate_ID , { 'ciphertext' | @ciphertext }   
    [ , { 'cert_password' | @cert_password } ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *certificate_ID*  
Die ID eines Zertifikats in der Datenbank. *certificate_ID* weist den Datentyp **int** auf.  
  
 *ciphertext*  
Die Datenzeichenfolge, die mithilfe des öffentlichen Schlüssels des Zertifikats verschlüsselt wurde.  
  
 @ciphertext  
Eine Variable vom Typ **varbinary**, die Daten enthält, die mit dem Zertifikat verschlüsselt wurden.  
  
 *cert_password*  
Das Kennwort, das zum Verschlüsseln des privaten Schlüssels des Zertifikats verwendet wurde. *cert_password* muss das Unicode-Datenformat aufweisen.  
  
 @cert_password  
Eine Variable vom Typ **nchar** oder **nvarchar**, die das Kennwort enthält, mit dem der private Schlüssel des Zertifikats verschlüsselt wurde. *\@cert_password* muss das Unicode-Datenformat aufweisen.  

## <a name="return-types"></a>Rückgabetypen  
**varbinary** mit einer maximalen Größe von 8.000 Byte.  
  
## <a name="remarks"></a>Bemerkungen  
Diese Funktion entschlüsselt Daten mithilfe des privaten Schlüssels eines Zertifikats. Kryptografische Umwandlungen, die asymmetrische Schlüssel verwenden, nehmen umfangreiche Ressourcen in Anspruch. Deshalb wird empfohlen, dass Entwickler die Verwendung von [ENCRYPTBYCERT](./encryptbycert-transact-sql.md) und DECRYPTBYCERT bei der routinemäßigen Verschlüsselung und Entschlüsselung von Benutzerdaten vermeiden.  

## <a name="permissions"></a>Berechtigungen  
`DECRYPTBYCERT` erfordert die CONTROL-Berechtigung für das Zertifikat.  
  
## <a name="examples"></a>Beispiele  
In diesem Beispiel werden Zeilen aus `[AdventureWorks2012].[ProtectedData04]` ausgewählt, die als Daten markiert sind, die ursprünglich mit dem Zertifikat `JanainaCert02` verschlüsselt wurden. Im Beispiel wird zunächst der private Schlüssel des Zertifikats `JanainaCert02` mit dem Kennwort des Zertifikats `pGFD4bb925DGvbd2439587y` entschlüsselt. Dann wird der Chiffretext mit diesem privaten Schlüssel entschlüsselt. Die entschlüsselten Daten werden im Beispiel von **varbinary** in **nvarchar** konvertiert.  

```sql  
SELECT CONVERT(NVARCHAR(max), DecryptByCert(Cert_Id('JanainaCert02'),  
    ProtectedData, N'pGFD4bb925DGvbd2439587y'))  
FROM [AdventureWorks2012].[ProtectedData04]   
WHERE Description   
    = N'data encrypted by certificate '' JanainaCert02''';  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ENCRYPTBYCERT &#40;Transact-SQL&#41;](../../t-sql/functions/encryptbycert-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)   
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
