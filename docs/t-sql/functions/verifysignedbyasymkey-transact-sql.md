---
description: VERIFYSIGNEDBYASYMKEY (Transact-SQL)
title: VERIFYSIGNEDBYASYMKEY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- VERIFYSIGNEDBYASYMKEY_TSQL
- VERIFYSIGNEDBYASYMKEY
dev_langs:
- TSQL
helpviewer_keywords:
- verifying digitally signed data for changes
- VERIFYSIGNEDBYASYMKEY
- testing digitally signed data for changes
- checking digitally signed data for changes
- signatures [SQL Server]
- digital signatures [SQL Server]
ms.assetid: 9f7c6e0b-5ba4-4dbb-994d-5bd59f4908de
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 4f2a75cf3da8220e861d8320b2454683c3b65a1f
ms.sourcegitcommit: 197a6ffb643f93592edf9e90b04810a18be61133
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2020
ms.locfileid: "91380595"
---
# <a name="verifysignedbyasymkey-transact-sql"></a>VERIFYSIGNEDBYASYMKEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Testet, ob digital signierte Daten seit der Signierung geändert wurden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
VerifySignedByAsymKey( Asym_Key_ID , clear_text , signature )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *Asym_Key_ID*  
 Die ID eines asymmetrischen Schlüsselzertifikats in der Datenbank.  
  
 *clear_text*  
 Die überprüften Klartextdaten.  
  
 *Signatur*  
 Die Signatur, die an die signierten Daten angefügt wurde. *signature* ist vom Datentyp **varbinary**.  
  
## <a name="return-types"></a>Rückgabetypen  
 **int**  
  
 Gibt 1 zurück, wenn die Signaturen übereinstimmen, andernfalls 0.  
  
## <a name="remarks"></a>Hinweise  
 **VerifySignedByAsymKey** entschlüsselt die Signatur der Daten mit dem öffentlichen Schlüssel des angegebenen asymmetrischen Schlüssels und vergleicht den entschlüsselten Wert mit einem neu berechneten MD5-Hash der Daten. Wenn die Werte zusammenpassen, wird die Signatur als gültig bestätigt.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DEFINITION-Berechtigung für den asymmetrischen Schlüssel.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-testing-for-data-with-a-valid-signature"></a>A. Testen von Daten mit einer gültigen Signatur  
 Im folgenden Beispiel wird 1 zurückgegeben, wenn die ausgewählten Daten nach der Signierung mit dem asymmetrischen Schlüssel `WillisKey74`nicht geändert wurden. Im Beispiel wird 0 zurückgegeben, wenn die Daten manipuliert wurden.  
  
```sql
SELECT Data,  
     VerifySignedByAsymKey( AsymKey_Id( 'WillisKey74' ), SignedData,  
     DataSignature ) as IsSignatureValid  
FROM [AdventureWorks2012].[SignedData04]   
WHERE Description = N'data encrypted by asymmetric key ''WillisKey74''';  
GO  
RETURN;  
```  
  
### <a name="b-returning-a-result-set-that-contains-data-with-a-valid-signature"></a>B. Zurückgeben eines Resultsets, das Daten mit einer gültigen Signatur enthält  
 Im folgenden Beispiel werden Zeilen in `SignedData04` mit Daten zurückgegeben, die nach der Signierung mit dem asymmetrischen Schlüssel `WillisKey74`nicht geändert wurden. Es wird die `AsymKey_ID` -Funktion aufgerufen, um die ID des asymmetrischen Schlüssels aus der Datenbank abzurufen.  
  
```sql
SELECT Data   
FROM [AdventureWorks2012].[SignedData04]   
WHERE VerifySignedByAsymKey( AsymKey_Id( 'WillisKey74' ), Data,  
     DataSignature ) = 1  
AND Description = N'data encrypted by asymmetric key ''WillisKey74''';  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [ASYMKEY_ID &#40;Transact-SQL&#41;](../../t-sql/functions/asymkey-id-transact-sql.md)   
 [SIGNBYASYMKEY &#40;Transact-SQL&#41;](../../t-sql/functions/signbyasymkey-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [ALTER ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-asymmetric-key-transact-sql.md)   
 [DROP ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
