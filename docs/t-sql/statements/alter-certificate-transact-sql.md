---
description: ALTER CERTIFICATE (Transact-SQL)
title: ALTER CERTIFICATE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_CERTIFICATE_TSQL
- ALTER CERTIFICATE
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTION BY PASSWORD option
- encryption [SQL Server], certificates
- modifying certificates
- private keys [SQL Server]
- ALTER CERTIFICATE statement
- certificates [SQL Server], modifying
ms.assetid: da4dc25e-72e0-4036-87ce-22de83160836
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a483870b4cc4d7419d853c5002f06c29b9713120
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348559"
---
# <a name="alter-certificate-transact-sql"></a>ALTER CERTIFICATE (Transact-SQL)

[!INCLUDE [sql-asdb-pdw](../../includes/applies-to-version/sql-asdb-pdw.md)]

  Ändert das Kennwort für die Verschlüsselung des privaten Schlüssels eines Zertifikats, entfernt den privaten Schlüssel oder importiert den privaten Schlüssel, falls dieser nicht vorhanden ist. Ändert die Verfügbarkeit eines Zertifikats für [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ALTER CERTIFICATE certificate_name   
      REMOVE PRIVATE KEY  
    | WITH PRIVATE KEY ( <private_key_spec> )  
    | WITH ACTIVE FOR BEGIN_DIALOG = { ON | OFF }  
  
<private_key_spec> ::=   
      {   
        { FILE = 'path_to_private_key' | BINARY = private_key_bits }  
         [ , DECRYPTION BY PASSWORD = 'current_password' ]  
         [ , ENCRYPTION BY PASSWORD = 'new_password' ]  
      }  
    |  
      {  
         [ DECRYPTION BY PASSWORD = 'current_password' ]  
         [ [ , ] ENCRYPTION BY PASSWORD = 'new_password' ]  
      }  
``` 
 
 
```syntaxsql  
-- Syntax for Parallel Data Warehouse  
  
ALTER CERTIFICATE certificate_name   
{  
      REMOVE PRIVATE KEY  
    | WITH PRIVATE KEY (   
        FILE = '<path_to_private_key>',  
        DECRYPTION BY PASSWORD = '<key password>' )
}  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *certificate_name*  
 Der eindeutige Name des Zertifikats in der Datenbank.  
  
 REMOVE PRIVATE KEY  
 Gibt an, dass der private Schlüssel nicht mehr in der Datenbank verwaltet werden soll.  
  
 WITH PRIVATE KEY: Gibt an, dass der private Schlüssel des Zertifikats in SQL Server geladen wird.

 FILE ='*path_to_private_key*'  
 Gibt den vollständigen Pfad einschließlich des Dateinamens für den privaten Schlüssel an. Dieser Parameter kann ein lokaler Pfad oder ein UNC-Pfad zu einem Netzwerkspeicherort sein. Auf diese Datei wird im Sicherheitskontext des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienstkontos zugegriffen. Wenn Sie diese Option verwenden, stellen Sie sicher, dass das Dienstkonto Zugriff auf die angegebene Datei hat.
 
 Wird nur ein Dateiname angegeben, wird die Datei in den Standardordner für Benutzerdaten für die Instanz gespeichert. Bei diesem Ordner kann es sich (muss aber nicht) um den Ordner „[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] DATA“ handeln. Für SQL Server Express LocalDB entspricht der Standardordner für Benutzerdaten für die Instanz dem von der Umgebungsvariable `%USERPROFILE%` angegebenen Pfad für das Konto, mit dem die Instanz erstellt wurde.  
  
 BINARY = '*private_key_bits*'  
 **Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.  
  
 Private Schlüsselbits, die als binäre Konstante angegeben sind. Diese Bits können in verschlüsselter Form vorhanden sein. Bei Verschlüsselung muss der Benutzer ein Entschlüsselungskennwort bereitstellen. Kennwortrichtlinienüberprüfungen werden für dieses Kennwort nicht ausgeführt. Die privaten Schlüsselbits müssen in einem PVK-Dateiformat vorliegen.  
  
 DECRYPTION BY PASSWORD = '*current_password*'  
 Gibt das zum Entschlüsseln des privaten Schlüssels erforderliche Kennwort an.  
  
 ENCRYPTION BY PASSWORD = '*new_password*'  
 Gibt das Kennwort an, mit dem der private Schlüssel des Zertifikats in der Datenbank verschlüsselt wird. *new_password* muss den Anforderungen der Windows-Kennwortrichtlinien des Computers entsprechen, auf dem die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird. Weitere Informationen finden Sie unter [Password Policy](../../relational-databases/security/password-policy.md).  
  
 ACTIVE FOR BEGIN_DIALOG **=** { ON | OFF }  
 Stellt das Zertifikat für den Initiator einer [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Dialogkonversation zur Verfügung.  
  
## <a name="remarks"></a>Hinweise  
 Der private Schlüssel muss dem öffentlichen Schlüssel entsprechen, der mit *certificate_name* angegeben ist.  
  
 Die DECRYPTION BY PASSWORD-Klausel kann ausgelassen werden, falls das Kennwort in der Datei mit einem NULL-Kennwort geschützt ist.  
  
 Ein privater Zertifikatsschlüssel, der bereits in der Datenbank vorhanden ist, wird beim Importieren automatisch durch den Datenbank-Hauptschlüssel geschützt. Verwenden Sie die Klausel „ENCRYPTION BY PASSWORD“, um den privaten Schlüssel mit einem Kennwort zu schützen.  
  
 Mit der Option REMOVE PRIVATE KEY wird der private Schlüssel des Zertifikats aus der Datenbank gelöscht. Sie können den privaten Schlüssel entfernen, wenn das Zertifikat zum Überprüfen von Signaturen oder für [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Szenarien, die keinen privaten Schlüssel erfordern, verwendet wird. Den privaten Schlüssel eines Zertifikats, das einen symmetrischen Schlüssel schützt, dürfen Sie nicht entfernen. Der private Schlüssel muss wiederhergestellt werden, wenn Sie zusätzliche Module oder Zeichenfolgen, die mit dem Zertifikat überprüft werden sollen, signieren oder einen mit dem Zertifikat verschlüsselten Wert entschlüsseln möchten.   
  
 Sie müssen kein Entschlüsselungskennwort angeben, wenn der private Schlüssel mithilfe des Datenbank-Hauptschlüssels verschlüsselt wird.  
 
 Wenn Sie das Kennwort für die Verschlüsselung des privaten Schlüssels ändern möchten, geben Sie weder die FILE- noch die BINARY-Klausel an.
  
> [!IMPORTANT]  
>  Erstellen Sie immer eine Archivierungskopie eines privaten Schlüssels, bevor Sie ihn aus einer Datenbank entfernen. Weitere Informationen finden Sie unter [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md) und [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md).  
  
 Die Option WITH PRIVATE KEY ist in einer enthaltenen Datenbank nicht verfügbar.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER-Berechtigung für das Zertifikat.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-removing-the-private-key-of-a-certificate"></a>A. Entfernen des privaten Schlüssels eines Zertifikats  
  
```sql  
ALTER CERTIFICATE Shipping04   
    REMOVE PRIVATE KEY;  
GO  
```  
  
### <a name="b-changing-the-password-that-is-used-to-encrypt-the-private-key"></a>B. Ändern des Kennworts, das zum Verschlüsseln des privaten Schlüssels verwendet wird  
  
```sql  
ALTER CERTIFICATE Shipping11   
    WITH PRIVATE KEY (DECRYPTION BY PASSWORD = '95hkjdskghFDGGG4%',  
    ENCRYPTION BY PASSWORD = '34958tosdgfkh##38');  
GO  
```  
  
### <a name="c-importing-a-private-key-for-a-certificate-that-is-already-present-in-the-database"></a>C. Importieren eines privaten Schlüssels für ein Zertifikat, das bereits in der Datenbank vorhanden ist  
  
```sql  
ALTER CERTIFICATE Shipping13   
    WITH PRIVATE KEY (FILE = 'c:\importedkeys\Shipping13',  
    DECRYPTION BY PASSWORD = 'GDFLKl8^^GGG4000%');  
GO  
```  
  
### <a name="d-changing-the-protection-of-the-private-key-from-a-password-to-the-database-master-key"></a>D: Ändern des Schutzes für den privaten Schlüssel von einem Kennwort in den Datenbank-Hauptschlüssel  
  
```sql  
ALTER CERTIFICATE Shipping15   
    WITH PRIVATE KEY (DECRYPTION BY PASSWORD = '95hk000eEnvjkjy#F%');  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)  
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)  
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)  
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
 [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md)  
 [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md)  
 [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)  
 [CERTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/certproperty-transact-sql.md)  
  
  

