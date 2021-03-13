---
description: CREATE ASYMMETRIC KEY (Transact-SQL)
title: CREATE ASYMMETRIC KEY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ASYMMETRIC_KEY_TSQL
- CREATE ASYMMETRIC KEY
- CREATE_ASYMMETRIC_KEY_TSQL
- ASYMMETRIC KEY
dev_langs:
- TSQL
helpviewer_keywords:
- asymmetric keys [SQL Server], creating
- encryption [SQL Server], asymmetric keys
- CREATE ASYMMETRIC KEY statement
- asymmetric keys [SQL Server]
- cryptography [SQL Server], asymmetric keys
ms.assetid: 141bc976-7631-49f6-82bd-a235028645b1
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: 1d0f2513ff1d81f1611eac5616ab8f1dbacfe07e
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421941"
---
# <a name="create-asymmetric-key-transact-sql"></a>CREATE ASYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Erstellt einen asymmetrischen Schlüssel in der Datenbank.  
  
 Diese Funktion ist inkompatibel mit Datenbankexport über Data-Tier Application Framework (DACFx). Sie müssen alle asymmetrischen Schlüssel vor dem Export löschen.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CREATE ASYMMETRIC KEY asym_key_name   
   [ AUTHORIZATION database_principal_name ]  
   [ FROM <asym_key_source> ]  
   [ WITH <key_option> ] 
   [ ENCRYPTION BY <encrypting_mechanism> ] 
   [ ; ]
  
<asym_key_source>::=  
     FILE = 'path_to_strong-name_file'  
   | EXECUTABLE FILE = 'path_to_executable_file'  
   | ASSEMBLY assembly_name  
   | PROVIDER provider_name  
  
<key_option> ::=  
   ALGORITHM = <algorithm>  
      |  
   PROVIDER_KEY_NAME = 'key_name_in_provider'  
      |  
      CREATION_DISPOSITION = { CREATE_NEW | OPEN_EXISTING }  
  
<algorithm> ::=  
      { RSA_4096 | RSA_3072 | RSA_2048 | RSA_1024 | RSA_512 }   
  
<encrypting_mechanism> ::=  
    PASSWORD = 'password'   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *Asym_Key_Name*  
 Der Name des asymmetrischen Schlüssels in der Datenbank. Namen von asymmetrischen Schlüsseln müssen den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen und innerhalb der Datenbank eindeutig sein.  

 AUTHORIZATION *database_principal_name*  
 Gibt den Besitzer des asymmetrischen Schlüssels an. Eine Rolle oder Gruppe kann nicht als Besitzer angegeben sein. Wird diese Option ausgelassen, wird der aktuelle Benutzer als Besitzer verwendet.  
  
 FROM *Asym_Key_Source*  
 Gibt die Quelle an, aus der das asymmetrische Schlüsselpaar geladen werden soll.  
  
 FILE ='*path_to_strong-name_file*'  
 Gibt den Pfad zu einer Datei mit starkem Namen an, aus der das Schlüsselpaar geladen werden soll. Beschränkt auf 260 Zeichen von MAX_PATH von der Windows-API.  
  
> [!NOTE]  
>  Diese Option ist in einer enthaltenen Datenbank nicht verfügbar.  
  
 EXECUTABLE FILE ='*path_to_executable_file*'  
 Gibt den Pfad einer Assemblydatei an, aus der der öffentliche Schlüssel geladen werden soll. Beschränkt auf 260 Zeichen von MAX_PATH von der Windows-API.  
  
> [!NOTE]  
>  Diese Option ist in einer enthaltenen Datenbank nicht verfügbar.  
  
 ASSEMBLY *assembly_name*  
 Gibt den Namen einer signierten Assembly an, die bereits in die Datenbank geladen wurde. Aus ihr wird der öffentliche Schlüssel geladen.  
  
 PROVIDER *provider_name*  
 Gibt den Namen eines Anbieters für erweiterte Schlüsselverwaltung (Extensible Key Management, EKM) an. Der Anbieter muss zuerst mit der CREATE PROVIDER-Anweisung definiert werden. Weitere Informationen zum Verwalten externer Schlüssel finden Sie unter [Erweiterbare Schlüsselverwaltung &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
 ALGORITHM = \<algorithm>  
 Die folgenden fünf Algorithmen können zur Verfügung gestellt werden: RSA_4096, RSA_3072, RSA_2048, RSA_1024 und RSA_512.  
  
 RSA_1024 und RSA_512 sind veraltet. Um RSA_1024 oder RSA_512 zu verwenden (was nicht empfohlen wird), müssen Sie den Kompatibilitätsgrad zwischen Datenbanken auf höchstens 120 festlegen.  
  
 PROVIDER_KEY_NAME = '*key_name_in_provider*'  
 Gibt den Schlüsselnamen des externen Anbieters an.  
  
 CREATION_DISPOSITION = CREATE_NEW  
 Erstellt einen neuen Schlüssel auf dem Extensible Key Management-Gerät. PROVIDER_KEY_NAME muss verwendet werden, um Schlüsselnamen auf dem Gerät anzugeben. Wenn bereits ein Schlüssel auf dem Gerät vorhanden ist, gibt die Anweisung eine Fehlermeldung zurück.  
  
 CREATION_DISPOSITION = OPEN_EXISTING  
 Ordnet einen asymmetrischen Schlüssel von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einem vorhandenen EKM-Schlüssel zu. PROVIDER_KEY_NAME muss verwendet werden, um Schlüsselnamen auf dem Gerät anzugeben. Wenn CREATION_DISPOSITION = OPEN_EXISTING nicht bereitgestellt wird, ist der Standardwert CREATE_NEW.  
  
 ENCRYPTION BY PASSWORD = '*password*'  
 Gibt das Kennwort an, mit dem der private Schlüssel verschlüsselt werden soll. Wenn diese Klausel nicht vorhanden ist, wird der private Schlüssel mit dem Datenbank-Hauptschlüssel verschlüsselt. *password* kann maximal 128 Zeichen umfassen. *password* muss den Anforderungen der Windows-Kennwortrichtlinien des Computers entsprechen, auf dem die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird.  
  
## <a name="remarks"></a>Bemerkungen  
 Ein *asymmetrischer Schlüssel* ist eine sicherungsfähige Entität auf Datenbankebene. Standardmäßig enthält diese Entität sowohl einen öffentlichen als auch einen privaten Schlüssel. Bei einer Ausführung ohne die FROM-Klausel generiert CREATE ASYMMETRIC KEY ein neues Schlüsselpaar. Bei einer Ausführung mit FROM-Klausel importiert CREATE ASYMMETRIC KEY ein Schlüsselpaar aus einer Datei oder einen öffentlichen Schlüssel aus einer Assembly oder DLL-Datei.  
  
 Standardmäßig ist der private Schlüssel mit dem Datenbank-Hauptschlüssel geschützt. Falls kein Datenbank-Hauptschlüssel erstellt wurde, ist ein Kennwort zum Schützen des privaten Schlüssels erforderlich.  
  
 Der private Schlüssel kann eine Länge von 512, 1024 oder 2048 Bits aufweisen.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die CREATE ASYMMETRIC KEY-Berechtigung in der Datenbank. Wenn die AUTHORIZATION-Klausel angegeben ist, ist die IMPERSONATE-Berechtigung für den Datenbankprinzipal oder die ALTER-Berechtigung für die Anwendungsrolle erforderlich. Nur Windows-Anmeldungen, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldungen und Anwendungsrollen können asymmetrische Schlüssel besitzen. Gruppen und Rollen können keine asymmetrischen Schlüssel besitzen.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-creating-an-asymmetric-key"></a>A. Erstellen eines asymmetrischen Schlüssels  
 Im folgenden Beispiel wird der asymmetrische Schlüssel `PacificSales09` mithilfe des `RSA_2048`-Algorithmus erstellt, und der private Schlüssel wird mit einem Kennwort geschützt.  
  
```sql  
CREATE ASYMMETRIC KEY PacificSales09   
    WITH ALGORITHM = RSA_2048   
    ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>';   
GO  
```  
  
### <a name="b-creating-an-asymmetric-key-from-a-file-giving-authorization-to-a-user"></a>B. Erstellen eines asymmetrischen Schlüssels aus einer Datei, wobei die Autorisierung an einen Benutzer erteilt wird  
 Im folgenden Beispiel wird der asymmetrische Schlüssel `PacificSales19` aus einem in einer Datei gespeicherten Schlüsselpaar erstellt und der Besitz des asymmetrischen Schlüssels Benutzer `Christina` zugewiesen. Der private Schlüssel ist durch den Datenbankhauptschlüssel geschützt, der vor dem asymmetrischen Schlüssel erstellt werden muss.  
  
```sql  
CREATE ASYMMETRIC KEY PacificSales19  
    AUTHORIZATION Christina  
    FROM FILE = 'c:\PacSales\Managers\ChristinaCerts.tmp';  
GO  
```  
  
### <a name="c-creating-an-asymmetric-key-from-an-ekm-provider"></a>C. Erstellen eines asymmetrischen Schlüssels von einem EKM-Anbieter  
 Das folgende Beispiel erstellt den asymmetrischen Schlüssel `EKM_askey1` aus einem Schlüsselpaar, das in einem EKM-Anbieter namens `EKM_Provider1` gespeichert ist, und einem Schlüssel für diesen Anbieter namens `key10_user1`.  
  
```sql  
CREATE ASYMMETRIC KEY EKM_askey1   
    FROM PROVIDER EKM_Provider1  
    WITH   
        ALGORITHM = RSA_2048,   
        CREATION_DISPOSITION = CREATE_NEW  
        , PROVIDER_KEY_NAME  = 'key10_user1' ;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-asymmetric-key-transact-sql.md)  
 [DROP ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)  
 [ASYMKEYPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/asymkeyproperty-transact-sql.md)  
 [ASYMKEY_ID &#40;Transact-SQL&#41;](../../t-sql/functions/asymkey-id-transact-sql.md)  
 [Auswählen eines Verschlüsselungsalgorithmus](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)  
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)  
 [Erweiterbare Schlüsselverwaltung mit Azure Key Vault &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
