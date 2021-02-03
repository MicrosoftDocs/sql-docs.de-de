---
description: CREATE COLUMN MASTER KEY (Transact-SQL)
title: CREATE COLUMN MASTER KEY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SQL13.SWB.NEWCOLUMNMASTERKEYDEF.GENERAL.F1
- SQL13.SWB.COLUMNMASTERKEYDEF.GENERAL.F1
- CREATE COLUMN MASTER KEY
- COLUMN MASTER KEY
- CREATE_COLUMN_MASTER_KEY_TSQL
- COLUMN_MASTER_KEY_TSQL
- SQL13.SWB.NEWCOLUMNMASTERKEY.GENERAL.F1
- SQL13.SWB.COLUMNMASTERKEY.GENERAL.F1
dev_langs:
- TSQL
helpviewer_keywords:
- column master key definition
- column master key, create
- CREATE COLUMN MASTER KEY statement
- Always Encrypted, create column master key
ms.assetid: f8926b95-e146-4e3f-b56b-add0c0d0a30e
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 6e18a1e0721428dcbfdb649be46ca83fd0d42972
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204938"
---
# <a name="create-column-master-key-transact-sql"></a>CREATE COLUMN MASTER KEY (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Erstellt ein Spaltenhauptschlüssel-Metadatenobjekt in einer Datenbank. Ein Metadateneintrag für einen Spaltenhauptschlüssel stellt einen Schlüssel dar, der in einem externen Schlüsselspeicher gespeichert ist. Der Schlüssel schützt, also verschlüsselt Spaltenverschlüsselungsschlüssel, wenn Sie [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) oder [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md) verwenden. Durch mehrere Spaltenhauptschlüssel wird die regelmäßige Rotation von Schlüsseln ermöglicht und dadurch die Sicherheit erhöht. Erstellen Sie einen Spaltenhauptschlüssel in einem Schlüsselspeicher und das zugehörige Metadatenobjekt in der Datenbank, indem Sie den Objekt-Explorer in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder PowerShell verwenden. Weitere Informationen finden Sie unter [Übersicht über die Schlüsselverwaltung für Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
 

> [!IMPORTANT]
> Das Erstellen von Enclave-fähigen Schlüsseln (mit „ENCLAVE_COMPUTATIONS“) erfordert [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

## <a name="syntax"></a>Syntax  

```syntaxsql
CREATE COLUMN MASTER KEY key_name   
    WITH (  
        KEY_STORE_PROVIDER_NAME = 'key_store_provider_name',  
        KEY_PATH = 'key_path'   
        [,ENCLAVE_COMPUTATIONS (SIGNATURE = signature)]
         )   
[;]  
```  

## <a name="arguments"></a>Argumente
*key_name*  
Der Name des Spaltenhauptschlüssels in der Datenbank.  
  
*key_store_provider_name*  
Gibt den Namen eines Schlüsselspeicheranbieters an. Ein Schlüsselspeicheranbieter ist eine clientseitige Softwarekomponente, die einen Schlüsselspeicher mit dem Spaltenhauptschlüssel enthält. 

Ein für Always Encrypted aktivierter Clienttreiber:

- Verwendet den Namen eines Schlüsselspeicheranbieters 
- Sucht nach dem Schlüsselspeicheranbieter in der Registrierung der Schlüsselspeicheranbieter des Treibers 

Der Treiber verwendet dann den Anbieter, um Spaltenverschlüsselungsschlüssel zu entschlüsseln. Die Spaltenverschlüsselungsschlüssel werden durch einen Spaltenhauptschlüssel geschützt. Der Spaltenhauptschlüssel ist im zugrunde liegenden Schlüsselspeicher gespeichert. Anschließend wird ein Klartextwert des Spaltenverschlüsselungsschlüssels verwendet, um Abfrageparameter zu entschlüsseln, die verschlüsselten Datenbankspalten entsprechen. Mit dem Spaltenverschlüsselungsschlüssel werden auch Abfrageergebnisse aus verschlüsselten Spalten entschlüsselt.  
  
Für Always Encrypted aktivierte Clienttreiberbibliotheken enthalten Schlüsselspeicheranbieter für beliebte Schlüsselspeicher.   
  
Die verfügbaren Anbieter hängen vom Typ und von der Version des Clienttreibers ab. Informationen zu bestimmten Treibern finden Sie in der Always Encrypted-Dokumentation: [Entwickeln von Anwendungen mithilfe von Always Encrypted](../../relational-databases/security/encryption/always-encrypted-client-development.md).


Die folgende Tabelle enthält die Namen von Systemanbietern:  
  
|Name des Schlüsselspeicheranbieters|Zugrunde liegender Schlüsselspeicher|  
    |-----------------------------|--------------------------|
    |'MSSQL_CERTIFICATE_STORE'|Windows-Zertifikatspeicher| 
    |'MSSQL_CSP_PROVIDER'|Ein Speicher, z.B. ein Hardwaresicherheitsmodul (HSM), der Microsoft CryptoAPI unterstützt.|
    |'MSSQL_CNG_STORE'|Ein Speicher, z.B. ein Hardwaresicherheitsmodul (HSM), der Cryptography API: Next Generation unterstützt.|  
    |'AZURE_KEY_VAULT'|Weitere Informationen finden Sie unter [Erste Schritte mit Azure Key Vault](/azure/key-vault/general/overview).|  
    |'MSSQL_JAVA_KEYSTORE'| Java-Schlüsselspeicher
  

In Ihrem für Always Encrypted aktivierten Clienttreiber können Sie einen benutzerdefinierten Schlüsselspeicheranbieter einrichten, der Spaltenhauptschlüssel speichert, für die kein integrierter Schlüsselspeicheranbieter vorhanden ist. Die Namen der benutzerdefinierten Schlüsselspeicheranbieter dürfen nicht mit „MSSQL_“ beginnen. Dieses Präfix ist für [!INCLUDE[msCoName](../../includes/msconame-md.md)]-Schlüsselspeicheranbieter reserviert. 

  
key_path  
Der Pfad des Schlüssels im Speicher des Spaltenhauptschlüssels. Der Schlüsselpfad muss für jede Clientanwendung gültig sein, die Daten verschlüsseln oder entschlüsseln soll. Die Daten werden in einer Spalte gespeichert, die (indirekt) durch den Spaltenhauptschlüssel geschützt wird, auf den verwiesen wird. Die Clientanwendung muss auf den Schlüssel zugreifen können. Das Format des Schlüsselpfads hängt vom Schlüsselspeicheranbieter ab. In der folgenden Liste werden die Formate der Schlüsselpfade für bestimmte System-Schlüsselspeicheranbieter von Microsoft beschrieben:  
  
-   **Anbietername:** MSSQL_CERTIFICATE_STORE  
  
    **Format des Schlüsselpfads:** *CertificateStoreName*/*CertificateStoreLocation*/*CertificateThumbprint*  
  
     Hierbei gilt:  
  
    *CertificateStoreLocation*  
    Der Zertifikatspeicherort, der dem aktuellen Benutzer oder dem lokalen Computer entsprechen muss. Weitere Informationen finden Sie unter [Local Machine and Current User Certificate Stores (Zertifikatspeicher des lokalen Computers bzw. des aktuellen Benutzers)](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores).  
  
    *CertificateStore*  
    Der Name des Zertifikatspeichers, z.B. „My“.  
  
    *CertificateThumbprint*  
    Zertifikatfingerabdruck.  
  
    **Beispiele:**  
  
    ```  
    N'CurrentUser/My/BBF037EC4A133ADCA89FFAEC16CA5BFA8878FB94'  
  
    N'LocalMachine/My/CA5BFA8878FB94BBF037EC4A133ADCA89FFAEC16'  
    ```  
  
-   **Anbietername:** MSSQL_CSP_PROVIDER  
  
    **Format des Schlüsselpfads:** *ProviderName*/*KeyIdentifier*  
  
    Hierbei gilt:  
  
    *ProviderName*  
    Der Name eines Kryptografiedienstanbieters (Cryptography Service Provider, CSP), der CAPI implementiert, für den Speicher des Spaltenhauptschlüssels. Wenn Sie ein HSM als Schlüsselspeicher verwenden, muss der Anbietername dem Namen des CSP entsprechen, den Ihr HSM-Anbieter bereitstellt. Der Anbieter muss auf einem Clientcomputer installiert sein.  
  
    *KeyIdentifier*  
    Der Bezeichner des Schlüssels im Schlüsselspeicher, der als Spaltenhauptschlüssel verwendet wird.  
  
    **Beispiele:**  
  
    ```  
    N'My HSM CSP Provider/AlwaysEncryptedKey1'  
    ```  
  
-   **Anbietername:** MSSQL_CNG_STORE  
  
    **Format des Schlüsselpfads:** *ProviderName*/*KeyIdentifier*  
  
    Hierbei gilt:  
  
    *ProviderName*  
    Der Name des Schlüsselspeicheranbieters (Key Storage Provider, KSP), der die CNG-API (Cryptography: Next Generation) implementiert, für den Speicher des Spaltenhauptschlüssels. Wenn Sie ein HSM als Schlüsselspeicher verwenden, muss der Anbietername dem Namen des KSP entsprechen, den Ihr HSM-Anbieter bereitstellt. Der Anbieter muss auf einem Clientcomputer installiert sein.  
  
    *KeyIdentifier*  
    Der Bezeichner des Schlüssels im Schlüsselspeicher, der als Spaltenhauptschlüssel verwendet wird.  
  
    **Beispiele:**  
  
    ```  
    N'My HSM CNG Provider/AlwaysEncryptedKey1'  
    ```  

-   **Anbietername:** AZURE_KEY_STORE  
  
    **Format des Schlüsselpfads:** *KeyUrl*  
  
    Hierbei gilt:  
  
    *KeyUrl*  
    Die URL des Schlüssels in Azure Key Vault

ENCLAVE_COMPUTATIONS  
Gibt an, dass der Spaltenhauptschlüssel Enclave-fähig ist. Sie können alle mit dem Spaltenhauptschlüssel verschlüsselten Spaltenverschlüsselungsschlüssel für eine serverseitige Secure Enclave freigeben und für Berechnungen innerhalb der Enclave verwenden. Weitere Informationen finden Sie unter [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

*Signatur*  
Ein binäres Literal, das das Ergebnis des digitalen Signierens von *Schlüsselpfad* und der Einstellung „ENCLAVE_COMPUTATIONS“ mit dem Spaltenhauptschlüssel ist. Die Signatur zeigt an, ob „ENCLAVE_COMPUTATIONS“ angegeben wurde. Die Signatur schützt die signierten Werte vor Änderungen durch nicht autorisierte Benutzer. Ein Always Encrypted-fähiger Clienttreiber überprüft die Signatur und gibt einen Fehler an die Anwendung zurück, wenn die Signatur ungültig ist. Die Signatur muss mit clientseitigen Tools generiert werden. Weitere Informationen finden Sie unter [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

## <a name="remarks"></a>Bemerkungen

Erstellen Sie einen Metadateneintrag für einen Spaltenhauptschlüssel, bevor Sie einen Metadateneintrag für einen Spaltenverschlüsselungsschlüssel in der Datenbank erstellen und bevor Spalten in der Datenbank mithilfe von Always Encrypted verschlüsselt werden können. Ein Metadateneintrag für einen Spaltenhauptschlüssel enthält nicht den tatsächlichen Spaltenhauptschlüssel. Der Spaltenhauptschlüssel muss in einem externen Spaltenschlüsselspeicher (außerhalb von SQL Server) gespeichert werden. Der Name des Schlüsselspeicheranbieters und der Pfad des Spaltenhauptschlüssels in den Metadaten müssen für eine Clientanwendung gültig sein. Die Clientanwendung muss den Spaltenhauptschlüssel verwenden, um einen Spaltenverschlüsselungsschlüssel zu entschlüsseln. Der Spaltenverschlüsselungsschlüssel ist mit dem Spaltenhauptschlüssel verschlüsselt. Die Clientanwendung muss auch verschlüsselte Spalten abfragen.

Es wird empfohlen, Tools wie SQL Server Management Studio (SSMS) oder PowerShell zu verwenden, um Spaltenhauptschlüssel zu verwalten. Solche Tools generieren Signaturen (wenn Sie Always Encrypted mit Secure Enclaves verwenden) und geben automatisch `CREATE COLUMN MASTER KEY`-Anweisungen aus, um Metadatenobjekte für Spaltenverschlüsselungsschlüssel zu erstellen. Weitere Informationen finden Sie unter [Bereitstellen von Always Encrypted-Schlüsseln mithilfe von SQL Server Management Studio](../../relational-databases/security/encryption/configure-always-encrypted-keys-using-ssms.md) und unter [Bereitstellen von Always Encrypted-Schlüsseln mithilfe von PowerShell](../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md). 

  
## <a name="permissions"></a>Berechtigungen  
Erfordert die Berechtigung **ALTER ANY COLUMN MASTER KEY**.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-creating-a-column-master-key"></a>A. Erstellen eines Spaltenhauptschlüssels  
Im folgenden Beispiel wird ein Metadateneintrag für einen Spaltenhauptschlüssel für einen Spaltenhauptschlüssel erstellt. Der Spaltenhauptschlüssel wird im Zertifikatspeicher für Clientanwendungen gespeichert, die den Anbieter „MSSQL_CERTIFICATE_STORE“ verwenden, um auf den Spaltenhauptschlüssel zuzugreifen:  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
   );  
```  
  
Erstellen Sie einen Metadateneintrag für einen Spaltenhauptschlüssel für einen Spaltenhauptschlüssel. Clientanwendungen, die den Anbieter „MSSQL_CNG_STORE“ verwenden, greifen auf den Spaltenhauptschlüssel zu:  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'MSSQL_CNG_STORE',    
    KEY_PATH = N'My HSM CNG Provider/AlwaysEncryptedKey'  
);  
```  
  
Erstellen Sie einen Metadateneintrag für einen Spaltenhauptschlüssel für einen Spaltenhauptschlüssel. Der Spaltenhauptschlüssel wird in Azure Key Vault für Clientanwendungen gespeichert, die den Anbieter „AZURE_KEY_VAULT“ verwenden, um auf den Spaltenhauptschlüssel zuzugreifen.  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',  
    KEY_PATH = N'https://myvault.vault.azure.net:443/keys/  
        MyCMK/4c05f1a41b12488f9cba2ea964b6a700');  
```  
  
Erstellen Sie einen Metadateneintrag für einen Spaltenhauptschlüssel für einen Spaltenhauptschlüssel. Der Spaltenhauptschlüssel wird in einem benutzerdefinierten Speicher für Spaltenhauptschlüssel gespeichert:  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = 'CUSTOM_KEY_STORE',    
    KEY_PATH = 'https://contoso.vault/sales_db_tce_key'  
);  
```  
### <a name="b-creating-an-enclave-enabled-column-master-key"></a>B. Erstellen eines Enclave-fähigen Spaltenhauptschlüssels  
Im folgenden Beispiel wird ein Metadateneintrag für einen Spaltenhauptschlüssel für einen Enclave-fähigen Spaltenhauptschlüssel erstellt. Der Enclave-fähigen Spaltenhauptschlüssel wird in einem Zertifikatspeicher für Clientanwendungen gespeichert, die den Anbieter „MSSQL_CERTIFICATE_STORE“ verwenden, um auf den Spaltenhauptschlüssel zuzugreifen:  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
     ENCLAVE_COMPUTATIONS (SIGNATURE = 0xA80F5B123F5E092FFBD6014FC2226D792746468C901D9404938E9F5A0972F38DADBC9FCBA94D9E740F3339754991B6CE26543DEB0D094D8A2FFE8B43F0C7061A1FFF65E30FDDF39A1B954F5BA206AAC3260B0657232020542419990261D878318CC38EF4E853970ED69A8D4A306693B8659AAC1C4E4109DE5EB148FD0E1FDBBC32F002C1D8199D313227AD689279D8DEEF91064DF122C19C3767C463723AB663A6F8412AE17E745922C0E3A257EAEF215532588ACCBD440A03C7BC100A38BD0609A119E1EF7C5C6F1B086C68AB8873DBC6487B270340E868F9203661AFF0492CEC436ABF7C4713CE64E38CF66C794B55636BFA55E5B6554AF570CF73F1BE1DBD)
  );  
```  
  
Erstellen Sie einen Metadateneintrag für einen Spaltenhauptschlüssel für einen Enclave-fähigen Spaltenhauptschlüssel. Der Enclave-fähige Spaltenhauptschlüssel wird in Azure Key Vault für Clientanwendungen gespeichert, die den Anbieter „AZURE_KEY_VAULT“ verwenden, um auf den Spaltenhauptschlüssel zuzugreifen.  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',  
    KEY_PATH = N'https://myvault.vault.azure.net:443/keys/MyCMK/4c05f1a41b12488f9cba2ea964b6a700');
    ENCLAVE_COMPUTATIONS (SIGNATURE = 0xA80F5B123F5E092FFBD6014FC2226D792746468C901D9404938E9F5A0972F38DADBC9FCBA94D9E740F3339754991B6CE26543DEB0D094D8A2FFE8B43F0C7061A1FFF65E30FDDF39A1B954F5BA206AAC3260B0657232020582413990261D878318CC38EF4E853970ED69A8D4A306693B8659AAC1C4E4109DE5EB148FD0E1FDBBC32F002C1D8199D313227AD689279D8DEEF91064DF122C19C3767C463723AB663A6F8412AE17E745922C0E3A257EAEF215532588ACCBD440A03C7BC100A38BD0609A119E1EF7C5C6F1B086C68AB8873DBC6487B270340E868F9203661AFF0492CEC436ABF7C4713CE64E38CF66C794B55636BFA55E5B6554AF570CF73F1BE1DBD)
  );
```  
  
## <a name="see-also"></a>Weitere Informationen
 
* [DROP COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-column-master-key-transact-sql.md)   
* [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-encryption-key-transact-sql.md)
* [sys.column_master_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
* [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
* [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md)   
* [Übersicht über die Schlüsselverwaltung für Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)   
* [Verwalten von Schlüsseln für Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)   
