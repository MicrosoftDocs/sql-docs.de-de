---
description: ALTER ASYMMETRIC KEY (Transact-SQL)
title: ALTER ASYMMETRIC KEY (Transact-SQL) | Microsoft-Dokumentation
ms.date: 04/12/2017
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_ASYMMETRIC_KEY_TSQL
- ALTER ASYMMETRIC KEY
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTION BY PASSWORD option
- passwords [SQL Server], asymmetric keys
- encryption [SQL Server], asymmetric keys
- DECRYPTION BY PASSWORD option
- ALTER ASYMMETRIC KEY statement
- asymmetric keys [SQL Server], modifying
ms.assetid: 958e95d6-fbe6-43e8-abbd-ccedbac2dbac
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: 4cf01b70a9ebeff0a339b710e506f033da396c40
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421904"
---
# <a name="alter-asymmetric-key-transact-sql"></a>ALTER ASYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Ändert die Eigenschaften eines asymmetrischen Schlüssels.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
ALTER ASYMMETRIC KEY Asym_Key_Name <alter_option>  
  
<alter_option> ::=  
      <password_change_option>   
    | REMOVE PRIVATE KEY   

<password_change_option> ::=  
    WITH PRIVATE KEY ( <password_option> [ , <password_option> ] )  

<password_option> ::=  
      ENCRYPTION BY PASSWORD = 'strongPassword'  
    | DECRYPTION BY PASSWORD = 'oldPassword'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *Asym_Key_Name*  
 Der Name des asymmetrischen Schlüssels in der Datenbank.  
  
 REMOVE PRIVATE KEY  
 Entfernt den privaten Schlüssel aus dem asymmetrischen Schlüssel. Der öffentliche Schlüssel wird nicht entfernt.  
  
 WITH PRIVATE KEY  
 Ändert den Schutz des privaten Schlüssels.  
  
 ENCRYPTION BY PASSWORD **='** _strongPassword_*_'_*  
 Gibt ein neues Kennwort zum Schutz des privaten Schlüssels an. *password* muss den Anforderungen der Windows-Kennwortrichtlinien des Computers entsprechen, auf dem die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird. Falls diese Option ausgelassen wird, wird der private Schlüssel mit dem Datenbank-Hauptschlüssel verschlüsselt.  
  
 DECRYPTION BY PASSWORD **='** _oldPassword_*_'_*  
 Gibt das alte Kennwort an, mit dem der private Schlüssel zurzeit verschlüsselt ist. Das Kennwort ist nicht erforderlich, wenn der private Schlüssel mit dem Datenbank-Hauptschlüssel verschlüsselt ist.  
  
## <a name="remarks"></a>Hinweise  
 Falls kein Datenbank-Hauptschlüssel vorhanden ist, muss die ENCRYPTION BY PASSWORD-Option angegeben sein. Der Vorgang erzeugt einen Fehler, wenn kein Kennwort angegeben ist. Weitere Informationen zum Erstellen eines Datenbankmasterschlüssels finden Sie unter [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md).  
  
 Mit ALTER ASYMMETRIC KEY kann der Schutz des privaten Schlüssels durch Angeben der PRIVATE KEY-Optionen geändert werden, wie in der folgenden Tabelle veranschaulicht ist.  
  
|Ändern des Schutzes|ENCRYPTION BY PASSWORD|DECRYPTION BY PASSWORD|  
|----------------------------|----------------------------|----------------------------|  
|Altes Kennwort wird geändert zu neuem Kennwort|Erforderlich|Erforderlich|  
|Kennwort wird geändert zu Hauptschlüssel|Auslassen|Erforderlich|  
|Hauptschlüssel wird geändert zu Kennwort|Erforderlich|Auslassen|  
  
 Der Datenbank-Hauptschlüssel muss geöffnet werden, bevor er zum Schutz eines privaten Schlüssels verwendet werden kann. Weitere Informationen finden Sie unter [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-master-key-transact-sql.md).  
  
 Zum Ändern des Besitzes eines asymmetrischen Schlüssels können Sie [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md) verwenden.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die CONTROL-Berechtigung für den asymmetrischen Schlüssel, wenn der private Schlüssel entfernt wird.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-changing-the-password-of-the-private-key"></a>A. Ändern des Kennworts für den privaten Schlüssel  
 Im folgenden Beispiel wird das Kennwort geändert, das zum Schutz des privaten Schlüssels des asymmetrischen Schlüssels `PacificSales09` verwendet wird. Das neue Kennwort lautet `<enterStrongPasswordHere>`.  
  
```sql  
ALTER ASYMMETRIC KEY PacificSales09   
    WITH PRIVATE KEY (  
    DECRYPTION BY PASSWORD = '<oldPassword>',  
    ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>');  
GO  
```  
  
### <a name="b-removing-the-private-key-from-an-asymmetric-key"></a>B. Entfernen des privaten Schlüssels aus einem asymmetrischen Schlüssel  
 Im folgenden Beispiel wird der private Schlüssel aus `PacificSales19` entfernt, wobei nur der öffentliche Schlüssel beibehalten wird.  
  
```sql  
ALTER ASYMMETRIC KEY PacificSales19 REMOVE PRIVATE KEY;  
GO  
```  
  
### <a name="c-removing-password-protection-from-a-private-key"></a>C. Entfernen des Kennwortschutzes aus einem privaten Schlüssel  
 Im folgenden Beispiel wird der Kennwortschutz für einen privaten Schlüssel entfernt, und der private Schlüssel wird mit dem Datenbank-Hauptschlüssel geschützt.  
  
```sql  
OPEN MASTER KEY DECRYPTION BY PASSWORD = '<database master key password>';  
ALTER ASYMMETRIC KEY PacificSales09 WITH PRIVATE KEY (  
    DECRYPTION BY PASSWORD = '<enterStrongPasswordHere>' );  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [DROP ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)   
 [Verschlüsselungsschlüssel für SQL Server und Datenbank &#40;Datenbank-Engine&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-master-key-transact-sql.md)   
 [Erweiterbare Schlüsselverwaltung &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
