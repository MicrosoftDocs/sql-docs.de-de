---
description: CREATE SYMMETRIC KEY (Transact-SQL)
title: CREATE SYMMETRIC KEY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE SYMMETRIC KEY
- SYMMETRIC KEP
- CREATE_SYMMETRIC_KEY_TSQL
- SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE SYMMETRIC KEY statement
- temporary symmetric keys [SQL Server]
- symmetric keys [SQL Server], creating
- symmetric keys [SQL Server]
ms.assetid: b5d23572-b79d-4cf1-9eef-d648fa3b1358
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: 1bdfcae4b6d7ef8461cb5ddec4bb1133a92d6d93
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421900"
---
# <a name="create-symmetric-key-transact-sql"></a>CREATE SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Generiert einen symmetrischen Schlüssel und gibt seine Eigenschaften in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an.  
  
 Diese Funktion ist inkompatibel mit Datenbankexport über Data-Tier Application Framework (DACFx). Sie müssen alle symmetrischen Schlüssel vor dem Export löschen.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md) 

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)] 
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CREATE SYMMETRIC KEY key_name   
    [ AUTHORIZATION owner_name ]  
    [ FROM PROVIDER provider_name ]  
    WITH 
      [
          <key_options> [ , ... n ]  
        | ENCRYPTION BY <encrypting_mechanism> [ , ... n ] 
      ]
  
<key_options> ::=  
      KEY_SOURCE = 'pass_phrase'  
    | ALGORITHM = <algorithm>  
    | IDENTITY_VALUE = 'identity_phrase'  
    | PROVIDER_KEY_NAME = 'key_name_in_provider'   
    | CREATION_DISPOSITION = {CREATE_NEW | OPEN_EXISTING }  
  
<algorithm> ::=  
    DES | TRIPLE_DES | TRIPLE_DES_3KEY | RC2 | RC4 | RC4_128  
    | DESX | AES_128 | AES_192 | AES_256   
  
<encrypting_mechanism> ::=  
      CERTIFICATE certificate_name   
    | PASSWORD = 'password'   
    | SYMMETRIC KEY symmetric_key_name   
    | ASYMMETRIC KEY asym_key_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *Key_name*  
 Gibt den eindeutigen Namen an, unter dem der symmetrische Schlüssel in der Datenbank bekannt ist. Temporäre Schlüssel werden festgelegt, wenn der _key_name_ mit einem Nummernzeichen (#) beginnt. Ein Beispiel: **#temporaryKey900007**. Das Erstellen eines symmetrischen Schlüssels, dessen Name mit mehreren Nummernzeichen (#) beginnt, ist nicht möglich. Sie können keinen temporären symmetrischen Schlüssel mit einem EKM-Anbieter erstellen.  
  
 AUTHORIZATION *owner_name*  
 Gibt den Namen des Datenbankbenutzers oder der Anwendungsrolle an, der bzw. die diesen Schlüssel besitzen wird.  
  
 FROM PROVIDER *provider_name*  
 Gibt einen Anbieter für erweiterte Schlüsselverwaltung (Extensible Key Management, EKM) und einen Namen an. Der Schlüssel wird nicht vom EKM-Gerät exportiert. Der Anbieter muss zuerst mit der CREATE PROVIDER-Anweisung definiert werden. Weitere Informationen zum Erstellen von externen Schlüsselanbietern finden Sie unter [Erweiterbare Schlüsselverwaltung &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
> [!NOTE]  
>  Diese Option ist in einer enthaltenen Datenbank nicht verfügbar.  
  
 KEY_SOURCE **='** _pass\_phrase_ **'**  
 Gibt einen Passphrase an, aus dem der Schlüssel abgeleitet werden soll.  
  
 IDENTITY_VALUE **='** _identity\_phrase_ **'**  
 Gibt einen Identity-Ausdruck an, aus dem ein GUID zum Kennzeichnen von Daten generiert wird, die mit einem temporären Schlüssel verschlüsselt werden.  
  
 PROVIDER_KEY_NAME **='** _key\_name\_in\_provider_ **'**  
 Gibt den Namen an, auf den der Anbieter für erweiterte Schlüsselverwaltung (Extensible Key Management, EKM) verweist.  
  
> [!NOTE]  
>  Diese Option ist in einer enthaltenen Datenbank nicht verfügbar.  
  
 CREATION_DISPOSITION **=** CREATE_NEW  
 Erstellt einen neuen Schlüssel auf dem Extensible Key Management-Gerät.  Wenn bereits ein Schlüssel auf dem Gerät vorhanden ist, gibt die Anweisung eine Fehlermeldung zurück.  
  
 CREATION_DISPOSITION **=** OPEN_EXISTING  
 Ordnet einen symmetrischen Schlüssel von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einem vorhandenen Extensible Key Management-Schlüssel zu. Wenn CREATION_DISPOSITION = OPEN_EXISTING nicht bereitgestellt wird, wird der Standardwert CREATE_NEW angenommen.  
  
 *certificate_name*  
 Gibt den Namen des Zertifikats an, das zum Verschlüsseln des symmetrischen Schlüssels verwendet wird. Das Zertifikat muss bereits in der Datenbank vorhanden sein.  
  
 **'** *password* **'**  
 Gibt ein Kennwort an, von dem ein TRIPLE_DES-Schlüssel zum Sichern des symmetrischen Schlüssels abgeleitet wird. *password* muss den Anforderungen der Windows-Kennwortrichtlinien des Computers entsprechen, auf dem die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird. Verwenden Sie immer sichere Kennwörter.  
  
 *symmetric_key_name*  
 Gibt einen symmetrischen Schlüssel an, der zum Verschlüsseln des erstellten Schlüssels verwendet wird. Der angegebene Schlüssel muss bereits in der Datenbank vorhanden und geöffnet sein.  
  
 *Asym_Key_Name*  
 Gibt einen asymmetrischen Schlüssel an, der zum Verschlüsseln des erstellten Schlüssels verwendet wird. Dieser asymmetrische Schlüssel muss bereits in der Datenbank vorhanden sein.  
  
 \<algorithm>  
Geben Sie den Verschlüsselungsalgorithmus an.   
> [!WARNING]  
> Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]gelten alle anderen Algorithmen als AES_128, AES_192 und AES_256 als veraltet. Sie müssen den Kompatibilitätsgrad zwischen Datenbanken auf maximal 120 festlegen, um ältere Algorithmen zu verwenden (nicht empfohlen).  
  
## <a name="remarks"></a>Bemerkungen  
 Beim Erstellen eines symmetrischen Schlüssels muss der symmetrische Schlüssel mithilfe mindestens eines der folgenden Elemente verschlüsselt werden: Zertifikat, Kennwort, symmetrischer Schlüssel, asymmetrischer Schlüssel oder PROVIDER. Der Schlüssel kann mehrere Verschlüsselungen jedes Typs aufweisen. Ein einzelner symmetrischer Schlüssel kann demnach mit mehreren Zertifikaten, Kennwörtern, symmetrischen Schlüsseln und asymmetrischen Schlüsseln gleichzeitig verschlüsselt sein.  
  
> [!CAUTION]  
>  Wenn ein symmetrischer Schlüssel mit einem Kennwort anstatt mit einem Zertifikat (oder einem anderen Schlüssel) verschlüsselt ist, wird der TRIPLE_DES-Verschlüsselungsalgorithmus verwendet, um das Kennwort zu verschlüsseln. Daher werden Schlüssel, die mit einem starken Verschlüsselungsalgorithmus wie z. B. AES erstellt werden, selbst mit einem schwächeren Algorithmus verschlüsselt.  
  
 Mit dem optionalen Kennwort kann der symmetrische Schlüssel verschlüsselt werden, bevor der Schlüssel an mehrere Benutzer verteilt wird.  
  
 Temporäre Schlüssel befinden sich im Besitz des Benutzers, der sie erstellt. Temporäre Schlüssel gelten nur für die aktuelle Sitzung.  
  
 Von IDENTITY_VALUE wird ein GUID generiert, mit dem Daten gekennzeichnet werden, die mit dem neuen symmetrischen Schlüssel verschlüsselt werden. Mithilfe dieser Kennzeichnung können Schlüssel den verschlüsselten Daten zugeordnet werden. Von einem spezifischen Ausdruck wird immer dieselbe GUID generiert. Nachdem ein Ausdruck zum Generieren eines GUIDs verwendet wurde, kann der Ausdruck nicht mehr wiederverwendet werden, es sei denn, es ist mindestens eine Sitzung vorhanden, in der der Ausdruck aktiv verwendet wird. IDENTITY_VALUE ist eine optionale Klausel. Die Verwendung dieser Klausel wird jedoch empfohlen, wenn Sie mit einem temporären Schlüssel verschlüsselte Daten speichern.  
  
 Es gibt keinen Standardverschlüsselungsalgorithmus.  
  
> [!IMPORTANT]  
>  Die Verwendung der RC4- und RC4_128-Datenstromchiffren wird zum Schutz vertraulicher Daten nicht empfohlen. Die Verschlüsselung mit solchen Schlüsseln wird von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht weiter codiert.  
  
 Informationen zu symmetrischen Schlüsseln werden in der [sys.symmetric_keys](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)-Katalogsicht angezeigt.  
  
 Symmetrische Schlüssel können nicht mit symmetrischen Schlüsseln, die mit dem Verschlüsselungsanbieter erstellt wurden, verschlüsselt werden.  
  
 **Erläuterung der DES-Algorithmen:**  
  
-   DESX wurde falsch benannt. Symmetrische Schlüssel, die mit ALGORITHM = DESX erstellt sind, verwenden eigentlich die TRIPLE DES-Chiffre mit einem 192-Bit-Schlüssel. Der DESX-Algorithmus wird nicht bereitgestellt. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
-   Symmetrische Schlüssel, die mit ALGORITHM = TRIPLE_DES_3KEY erstellt sind, verwenden die TRIPLE DES-Chiffre mit einem 192-Bit-Schlüssel.  
-   Symmetrische Schlüssel, die mit ALGORITHM = TRIPLE_DES erstellt sind, verwenden die TRIPLE DES-Chiffre mit einem 128-Bit-Schlüssel.  
  
 **Veraltung des RC4-Algorithmus:**  
  
 Die wiederholte Verwendung der gleichen RC4- oder RC4_128-KEY_GUID für unterschiedliche Datenblocks führt zum gleichen RC4-Schlüssel, da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht automatisch eine Salt bereitstellt. Die wiederholte Verwendung des gleichen RC4-Schlüssels stellt einen bekannter Fehler dar, der zu einer sehr schwachen Verschlüsselung führt. Deshalb wurden das RC4-Schlüsselwort und das RC4_128-Schlüsselwort als veraltet festgelegt. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
> [!WARNING]  
>  Der RC4-Algorithmus wird nur aus Gründen der Abwärtskompatibilität unterstützt. Neues Material kann nur mit RC4 oder RC4_128 verschlüsselt werden, wenn die Datenbank den Kompatibilitätsgrad 90 oder 100 besitzt. (Nicht empfohlen.) Verwenden Sie stattdessen einen neueren Algorithmus, z. B. einen der AES-Algorithmen. In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] kann mit RC4 oder RC4_128 verschlüsseltes Material in jedem Kompatibilitätsgrad entschlüsselt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER ANY SYMMETRIC KEY-Berechtigung in der Datenbank. Falls die AUTHORIZATION-Klausel angegeben ist, ist die IMPERSONATE-Berechtigung für den Datenbankbenutzer oder die ALTER-Berechtigung für die Anwendungsrolle erforderlich. Falls die Verschlüsselung mit einem Zertifikat oder asymmetrischen Schlüssel erfolgt, ist die VIEW DEFINITION-Berechtigung für das Zertifikat oder den asymmetrischen Schlüssel erforderlich. Nur Windows-Anmeldungen, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldungen und Anwendungsrollen können symmetrische Schlüssel besitzen. Gruppen und Rollen können keine symmetrischen Schlüssel besitzen.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-creating-a-symmetric-key"></a>A. Erstellen eines symmetrischen Schlüssels  
 Im folgenden Beispiel wird ein symmetrischer Schlüssel mit Namen `JanainaKey09` erstellt, wobei der `AES 256`-Algorithmus verwendet wird. Anschließend wird der neue Schlüssel mit dem Zertifikat `Shipping04` verschlüsselt.  
  
```sql  
CREATE SYMMETRIC KEY JanainaKey09   
WITH ALGORITHM = AES_256  
ENCRYPTION BY CERTIFICATE Shipping04;  
GO  
```  
  
### <a name="b-creating-a-temporary-symmetric-key"></a>B. Erstellen eines temporären symmetrischen Schlüssels  
 Im folgenden Beispiel wird ein temporärer symmetrischer Schlüssel mit Namen `#MarketingXXV` aus dem folgenden Passphrase erstellt: `The square of the hypotenuse is equal to the sum of the squares of the sides`. Der Schlüssel wird mit einem GUID bereitgestellt, der aus der Zeichenfolge `Pythagoras` generiert und mit dem Zertifikat `Marketing25` verschlüsselt wird.  
  
```sql 
CREATE SYMMETRIC KEY #MarketingXXV   
WITH ALGORITHM = AES_128,  
KEY_SOURCE   
     = 'The square of the hypotenuse is equal to the sum of the squares of the sides',  
IDENTITY_VALUE = 'Pythagoras'  
ENCRYPTION BY CERTIFICATE Marketing25;  
GO  
```  
  
### <a name="c-creating-a-symmetric-key-using-an-extensible-key-management-ekm-device"></a>C. Das Erstellen eines symmetrischen Schlüssels mit einem Extensible Key Management (EKM)-Gerät  
 Im folgenden Beispiel wird ein symmetrischer Schlüssel mit der Bezeichnung `MySymKey` erstellt, indem ein Anbieter mit dem Namen `MyEKMProvider` und ein Schlüssel mit der Bezeichnung `KeyForSensitiveData` verwendet werden. `User1` wird autorisiert, und es wird davon ausgegangen, dass der Systemadministrator den Anbieter `MyEKMProvider` bereits in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] registriert hat.  
  
```sql  
CREATE SYMMETRIC KEY MySymKey  
AUTHORIZATION User1  
FROM PROVIDER EKMProvider  
WITH  
PROVIDER_KEY_NAME='KeyForSensitiveData',  
CREATION_DISPOSITION=OPEN_EXISTING;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Auswählen eines Verschlüsselungsalgorithmus](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.symmetric_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [Erweiterbare Schlüsselverwaltung &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [Erweiterbare Schlüsselverwaltung mit Azure Key Vault &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
