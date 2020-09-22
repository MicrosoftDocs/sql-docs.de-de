---
description: ALTER CRYPTOGRAPHIC PROVIDER (Transact-SQL)
title: ALTER CRYPTOGRAPHIC PROVIDER (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/20/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_CRYPTOGRAPHIC_PROVIDER_TSQL
- ALTER CRYPTOGRAPHIC PROVIDER
- ALTER_CRYPTOGRAPHIC_TSQL
- ALTER CRYPTOGRAPHIC
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER CRYPTOGRAPHIC PROVIDER
ms.assetid: 876b6348-fb29-49e1-befc-4217979f6416
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 95c7f778abe9417a108e4df6982b73d3037f5ae9
ms.sourcegitcommit: ac9feb0b10847b369b77f3c03f8200c86ee4f4e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90688766"
---
# <a name="alter-cryptographic-provider-transact-sql"></a>ALTER CRYPTOGRAPHIC PROVIDER (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ändert einen Kryptografieanbieter innerhalb von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von einem EKM-Anbieter (Extensible Key Management) aus.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
ALTER CRYPTOGRAPHIC PROVIDER provider_name   
    [ FROM FILE = path_of_DLL ]  
    ENABLE | DISABLE  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *provider_name*  
 Der Name des EKM-Anbieters.  
  
 *Path_of_DLL*  
 Der Pfad der DLL-Datei, die die EKM-Schnittstelle von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implementiert.  
  
 ENABLE | DISABLE  
 Aktiviert bzw. deaktiviert einen Anbieter.  
  
## <a name="remarks"></a>Hinweise  
 Wenn der Anbieter die DLL-Datei ändert, mit der in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]die erweiterbare Schlüsselverwaltung implementiert wird, müssen Sie die ALTER CRYPTOGRAPHIC PROVIDER-Anweisung verwenden.  
  
 Wenn der Pfad der DLL-Datei mithilfe der ALTER CRYPTOGRAPHIC PROVIDER-Anweisung aktualisiert wird, führt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] folgende Aktionen aus:  
-   Deaktiviert den Anbieter.  
-   Überprüft die DLL-Signatur und stellt sicher, dass die DLL-Datei über den GUID verfügt, der auch im Katalog aufgezeichnet ist.  
-   Aktualisiert die DLL-Version im Katalog.  
  

Wenn ein EKM-Anbieter auf DISABLE festgelegt wird, tritt bei neuen Verbindungen bei dem Versuch, den Anbieter mit Verschlüsselungsanweisungen zu verwenden, ein Fehler auf.  
  
Um einen Anbieter zu deaktivieren, müssen alle Sitzungen, die den Anbieter verwenden, beendet werden.  
  
Wenn von einer DLL des EKM-Anbieters nicht alle erforderlichen Methoden implementiert werden, kann ALTER CRYPTOGRAPHIC PROVIDER den Fehler 33085 zurückgeben:  
  
 `One or more methods cannot be found in cryptographic provider library '%.*ls'.`  
  
Wenn die Headerdatei, mit der die DLL des EKM-Anbieters erstellt wurde, veraltet ist, kann ALTER CRYPTOGRAPHIC PROVIDER den Fehler 33032 zurückgeben:  
  
 `SQL Crypto API version '%02d.%02d' implemented by provider is not supported. Supported version is '%02d.%02d'.`  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die CONTROL-Berechtigung für den Kryptografieanbieter.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird ein Kryptografieanbieter mit dem Namen `SecurityProvider` in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]in eine neuere Version einer DLL-Datei geändert. Diese neue Version erhält den Namen `c:\SecurityProvider\SecurityProvider_v2.dll` und wird auf dem Server installiert. Das Zertifikat des Anbieters muss auf dem Server installiert sein.  
  
1. Deaktivieren Sie den Anbieter, um das Upgrade durchzuführen. Dadurch werden alle geöffneten kryptografischen Sitzungen beendet.  
```sql  
ALTER CRYPTOGRAPHIC PROVIDER SecurityProvider   
DISABLE;  
GO  
```  

2. Aktualisieren Sie die DLL-Datei des Anbieters. Die GUID muss mit der vorherigen Version identisch sein, die Version kann sich jedoch unterscheiden.  
```sql  
ALTER CRYPTOGRAPHIC PROVIDER SecurityProvider  
FROM FILE = 'c:\SecurityProvider\SecurityProvider_v2.dll';  
GO  
```  

3. Aktivieren Sie den aktualisierten Anbieter.   
```sql  
ALTER CRYPTOGRAPHIC PROVIDER SecurityProvider   
ENABLE;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erweiterbare Schlüsselverwaltung &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/create-cryptographic-provider-transact-sql.md)   
 [DROP CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-cryptographic-provider-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [Erweiterbare Schlüsselverwaltung mit Azure Key Vault &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
