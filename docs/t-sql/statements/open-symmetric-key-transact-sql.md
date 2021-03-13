---
description: OPEN SYMMETRIC KEY (Transact-SQL)
title: OPEN SYMMETRIC KEY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPEN SYMMETRIC KEY
- OPEN_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- symmetric keys [SQL Server], opening
- OPEN SYMMETRIC KEY statement
ms.assetid: ff019a7c-c373-46c7-ac43-ffb7e2ee60b3
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: 8939563a1c5cfc2c9ca44527493fdf120d9b3897
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421880"
---
# <a name="open-symmetric-key-transact-sql"></a>OPEN SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Entschlüsselt einen symmetrischen Schlüssel und stellt ihn zur Verwendung zur Verfügung.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
OPEN SYMMETRIC KEY Key_name DECRYPTION BY <decryption_mechanism>  
  
<decryption_mechanism> ::=  
    CERTIFICATE certificate_name [ WITH PASSWORD = 'password' ]  
    |  
    ASYMMETRIC KEY asym_key_name [ WITH PASSWORD = 'password' ]  
    |  
    SYMMETRIC KEY decrypting_Key_name  
    |  
    PASSWORD = 'decryption_password'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *Key_name*  
 Der Name des zu öffnenden symmetrischen Schlüssels.  
  
 CERTIFICATE *certificate_name*  
 Der Name eines Zertifikats, mit dessen privatem Schlüssel der symmetrische Schlüssel entschlüsselt wird.  
  
 ASYMMETRIC KEY *asym_key_name*  
 Der Name eines asymmetrischen Schlüssels, mit dessen privatem Schlüssel der symmetrische Schlüssel entschlüsselt wird.  
  
 WITH PASSWORD ='*password*'  
 Das Kennwort, mit dem der private Schlüssel des Zertifikats oder des asymmetrischen Schlüssels verschlüsselt wurde.  
  
 SYMMETRIC KEY *decrypting_key_name*  
 Der Name eines symmetrischen Schlüssels, mit dem der symmetrische Schlüssel, der geöffnet wird, entschlüsselt wird.  
  
 PASSWORD ='*password*'  
 Das Kennwort, mit dem der symmetrische Schlüssel geschützt wurde.  
  
## <a name="remarks"></a>Hinweise  
 Geöffnete symmetrische Schlüssel werden an die Sitzung gebunden und nicht an den Sicherheitskontext. Ein geöffneter Schlüssel ist weiterhin verfügbar, bis er entweder explizit geschlossen oder die Sitzung beendet wird. Wenn Sie einen symmetrischen Schlüssel öffnen und dann den Kontext wechseln, bleibt der Schlüssel geöffnet und in dem durch Identitätswechsel übernommenen Kontext verfügbar. Informationen zu offenen symmetrischen Schlüsseln werden in der Katalogsicht [sys.openkeys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-openkeys-transact-sql.md) angezeigt.  
  
 Falls der symmetrische Schlüssel mit einem anderen Schlüssel verschlüsselt wurde, muss zuerst dieser Schlüssel geöffnet werden.  
  
 Falls der symmetrische Schlüssel bereits geöffnet ist, ist die Abfrage **NO_OP**.  
  
 Falls das falsche Kennwort, das falsche Zertifikat oder der falsche Schlüssel zum Entschlüsseln des symmetrischen Schlüssels bereitgestellt wurde, erzeugt die Abfrage einen Fehler.  
  
 Symmetrische Schlüssel, die von Kryptografieanbietern erstellt wurden, können nicht geöffnet werden. Verschlüsselungs- und Entschlüsselungsverfahren, die diese Art von symmetrischem Schlüssel verwenden, können ohne die **OPEN**-Anweisung erfolgreich verwendet werden, da der Kryptografieanbieter den Schlüssel öffnet und schließt.  
  
## <a name="permissions"></a>Berechtigungen  
 Der Aufrufer benötigt bestimmte Berechtigungen für den Schlüssel, und die VIEW DEFINITION-Berechtigung für den Schlüssel darf ihm nicht verweigert worden sein. Zusätzliche Anforderungen hängen vom Entschlüsselungsmechanismus ab:  
  
-   DECRYPTION BY CERTIFICATE: CONTROL-Berechtigung für das Zertifikat und Kenntnis des Kennworts, mit dem der private Schlüssel verschlüsselt wird.  
  
-   DECRYPTION BY ASYMMETRIC KEY: CONTROL-Berechtigung für den asymmetrischen Schlüssel und Kenntnis des Kennworts, mit dem der private Schlüssel verschlüsselt wird.  
  
-   DECRYPTION BY PASSWORD: Kenntnis eines der Kennwörter, mit denen der symmetrische Schlüssel verschlüsselt wird.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-opening-a-symmetric-key-by-using-a-certificate"></a>A. Öffnen eines symmetrischen Schlüssels mithilfe eines Zertifikats  
 Im folgenden Beispiel wird der symmetrische Schlüssel `SymKeyMarketing3` geöffnet und mit dem privaten Schlüssel des `MarketingCert9`-Zertifikats entschlüsselt.  
  
```sql  
USE AdventureWorks2012;  
OPEN SYMMETRIC KEY SymKeyMarketing3   
    DECRYPTION BY CERTIFICATE MarketingCert9;  
GO  
```  
  
### <a name="b-opening-a-symmetric-key-by-using-another-symmetric-key"></a>B. Öffnen eines symmetrischen Schlüssels mithilfe eines anderen symmetrischen Schlüssels  
 Im folgenden Beispiel wird der symmetrische Schlüssel `MarketingKey11` geöffnet und mit dem symmetrischen Schlüssel `HarnpadoungsatayaSE3` entschlüsselt.  
  
```sql  
USE AdventureWorks2012;  
-- First open the symmetric key that you want for decryption.  
OPEN SYMMETRIC KEY HarnpadoungsatayaSE3   
    DECRYPTION BY CERTIFICATE sariyaCert01;  
-- Use the key that is already open to decrypt MarketingKey11.  
OPEN SYMMETRIC KEY MarketingKey11   
    DECRYPTION BY SYMMETRIC KEY HarnpadoungsatayaSE3;  
GO   
```  
  
## <a name="see-also"></a>Siehe auch  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [CLOSE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/close-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Erweiterbare Schlüsselverwaltung &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
