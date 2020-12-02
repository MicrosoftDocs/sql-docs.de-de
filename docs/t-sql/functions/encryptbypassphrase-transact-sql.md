---
description: ENCRYPTBYPASSPHRASE (Transact-SQL)
title: ENCRYPTBYPASSPHRASE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ENCRYPTBYPASSPHRASE
- ENCRYPTBYPASSPHRASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTBYPASSPHRASE function
- encryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], ENCRYPTBYPASSPHRASE function
ms.assetid: f8dbb9e6-94d6-40d7-8b38-6833a409d597
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f8e11d7e9f36d6619c626d9387cce766fbce5b0d
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124749"
---
# <a name="encryptbypassphrase-transact-sql"></a>ENCRYPTBYPASSPHRASE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Verschlüsselt Daten unter Verwendung des Triple DES-Algorithmus mit der Schlüsselbitlänge 128 mit einer Passphrase.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
EncryptByPassPhrase ( { 'passphrase' | @passphrase }   
    , { 'cleartext' | @cleartext }  
  [ , { add_authenticator | @add_authenticator }  
    , { authenticator | @authenticator } ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *passphrase*  
 Eine Passphrase, aus der ein symmetrischer Schlüssel generiert wird.  
  
 @passphrase  
 Eine Variable vom Typ **nvarchar**, **char**, **varchar**, **binary**, **varbinary** oder **nchar**, die eine Passphrase enthält, aus der ein symmetrischer Schlüssel generiert wird.  
  
 *Klartext*  
 Der zu verschlüsselnde Klartext.  
  
 @cleartext  
 Eine Variable vom Typ **nvarchar**, **char**, **varchar**, **binary**, **varbinary** oder **nchar**, die den Klartext enthält. Die maximale Größe beträgt 8.000 Bytes.  
  
 *add_authenticator*  
 Gibt an, ob ein Authentifikator zusammen mit dem Klartext verschlüsselt wird. Mit dem Wert 1 wird ein Authentifikator hinzugefügt. **int**.  
  
 @add_authenticator  
 Gibt an, ob ein Hash zusammen mit dem Klartext verschlüsselt wird.  
  
 *authenticator*  
 Daten, von denen ein Authentifikator abgeleitet wird. **sysname**.  
  
 @authenticator  
 Eine Variable, die Daten enthält, von denen ein Authentifikator abgeleitet wird.  
  
## <a name="return-types"></a>Rückgabetypen  
 **varbinary** mit einer maximalen Größe von 8.000 Byte.  
  
## <a name="remarks"></a>Hinweise  
 Eine Passphrase ist ein Kennwort, das Leerzeichen enthält. Die Verwendung eines Passphrases bietet den Vorteil, dass ein aussagekräftiger Ausdruck oder Satz leichter zu merken ist als eine vergleichsweise lange Zeichenfolge.  
  
 Die Kennwortkomplexität wird mit dieser Funktion nicht überprüft.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird ein Datensatz in der `SalesCreditCard`-Tabelle aktualisiert und der Wert der Kreditkartennummer, die in der `CardNumber_EncryptedbyPassphrase`-Spalte gespeichert ist, mithilfe des Primärschlüssels als Authentifikator verschlüsselt.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create a column in which to store the encrypted data.  
ALTER TABLE Sales.CreditCard   
    ADD CardNumber_EncryptedbyPassphrase VARBINARY(256);   
GO  
-- First get the passphrase from the user.  
DECLARE @PassphraseEnteredByUser NVARCHAR(128);  
SET @PassphraseEnteredByUser   
    = 'A little learning is a dangerous thing!';  
  
-- Update the record for the user's credit card.  
-- In this case, the record is number 3681.  
UPDATE Sales.CreditCard  
SET CardNumber_EncryptedbyPassphrase = EncryptByPassPhrase(@PassphraseEnteredByUser  
    , CardNumber, 1, CONVERT(varbinary, CreditCardID))  
WHERE CreditCardID = '3681';  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [DECRYPTBYPASSPHRASE &#40;Transact-SQL&#41;](../../t-sql/functions/decryptbypassphrase-transact-sql.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
