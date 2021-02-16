---
description: BACKUP CERTIFICATE (Transact-SQL)
title: BACKUP CERTIFICATE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DUMP_CERTIFICATE_TSQL
- BACKUP CERTIFICATE
- sql13.swb.exportcertificate.f1
- DUMP CERTIFICATE
- BACKUP_CERTIFICATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- encryption [SQL Server], certificates
- decryption [SQL Server], certificates
- exporting certificates
- certificates [SQL Server], exporting
- BACKUP CERTIFICATE statement
- backing up certificates [SQL Server]
- decryption [SQL Server]
- cryptography [SQL Server], certificates
ms.assetid: 509b9462-819b-4c45-baae-3d2d90d14a1c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 2ccce23a78d046729ccfdac4bdd34037b0a491b1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340617"
---
# <a name="backup-certificate-transact-sql"></a>BACKUP CERTIFICATE (Transact-SQL)
[!INCLUDE [sql-pdw](../../includes/applies-to-version/sql-pdw.md)]

  Exportiert ein Zertifikat in eine Datei.  
  
 ![Linksymbol](../../database-engine/configure-windows/media/topic-link.gif "Linksymbol") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server  
  
BACKUP CERTIFICATE certname TO FILE = 'path_to_file'  
    [ WITH PRIVATE KEY   
      (   
        FILE = 'path_to_private_key_file' ,  
        ENCRYPTION BY PASSWORD = 'encryption_password'   
        [ , DECRYPTION BY PASSWORD = 'decryption_password' ]   
      )   
    ]  
```  
  
   
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
BACKUP CERTIFICATE certname TO FILE ='path_to_file'  
      WITH PRIVATE KEY   
      (   
        FILE ='path_to_private_key_file',  
        ENCRYPTION BY PASSWORD ='encryption_password'   
      )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *certname*  
 Der Name des Zertifikats für die Sicherung.

 TO FILE = '*path_to_file*'  
 Gibt den vollständigen Pfad einschließlich des Dateinamens zu der Datei an, in der das Zertifikat gespeichert werden soll. Dieser Pfad kann ein lokaler Pfad oder ein UNC-Pfad zu einer Netzwerkadresse sein. Wenn nur ein Dateiname angegeben ist, wird die Datei im Standardordner für Benutzerdaten der Instanz gespeichert (entweder [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-DATA-Ordner oder nicht). Für SQL Server Express LocalDB entspricht der standardmäßige Benutzerdatenordner der Instanz dem von der Umgebungsvariable `%USERPROFILE%` angegebenen Pfad für das Konto, das die Instanz erstellt hat.  

 WITH PRIVATE KEY gibt an, dass der private Schlüssel des Zertifikats in einer Datei gespeichert werden muss. Diese Klausel ist optional.

 FILE = '*path_to_private_key_file*'  
 Gibt den vollständigen Pfad einschließlich des Dateinamens zu der Datei an, in der der private Schlüssel gespeichert werden soll. Dieser Pfad kann ein lokaler Pfad oder ein UNC-Pfad zu einer Netzwerkadresse sein. Wenn nur ein Dateiname angegeben ist, wird die Datei im Standardordner für Benutzerdaten der Instanz gespeichert (entweder [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-DATA-Ordner oder nicht). Für SQL Server Express LocalDB entspricht der standardmäßige Benutzerdatenordner der Instanz dem von der Umgebungsvariable `%USERPROFILE%` angegebenen Pfad für das Konto, das die Instanz erstellt hat.  

 ENCRYPTION BY PASSWORD = '*encryption_password*'  
 Das Kennwort, das zum Verschlüsseln des privaten Schlüssels verwendet wird, bevor der Schlüssel in die Sicherungsdatei geschrieben wird. Das Kennwort unterliegt Komplexitätsüberprüfungen.  
  
 DECRYPTION BY PASSWORD = '*decryption_password*'  
 Das Kennwort, das zum Entschlüsseln des privaten Schlüssels verwendet wird, bevor der Schlüssel gesichert wird. Dieses Argument ist nicht erforderlich, wenn das Zertifikat mit dem Hauptschlüssel verschlüsselt ist. 
  
## <a name="remarks"></a>Hinweise  
 Falls der private Schlüssel mit einem Kennwort in der Datenbank verschlüsselt wird, muss das Kennwort für die Entschlüsselung angegeben werden.  
  
 Bei der Sicherung des privaten Schlüssels in einer Datei ist eine Verschlüsselung erforderlich. Das Kennwort, mit dem der private Schlüssel in der Datei geschützt wird, ist nicht dasselbe Kennwort, mit dem der private Schlüssel des Zertifikats in der Datenbank verschlüsselt wird.  

 Private Schlüssel werden im PVK-Dateiformat gespeichert.

 Verwenden Sie zum Wiederherstellen eines gesicherten Zertifikats mit oder ohne privatem Schlüssel die [CREATE CERTIFICATE](../../t-sql/statements/create-certificate-transact-sql.md)-Anweisung.
 
 Verwenden Sie zum Wiederherstellen eines privaten Schlüssels in einem vorhandenen Zertifikat in der Datenbank die [ALTER CERTIFICATE](../../t-sql/statements/alter-certificate-transact-sql.md)-Anweisung.
 
 Bei einer Sicherung werden die Dateien für das Dienstkonto der SQL Server-Instanz der ACL hinzugefügt. Wenn Sie das Zertifikat auf einem Server wiederherstellen müssen, der unter einem anderen Konto ausgeführt wird, müssen Sie die Berechtigungen für die Dateien so anpassen, dass sie von dem neuen Konto gelesen werden können. 
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die CONTROL-Berechtigung für das Zertifikat und die Kenntnis des Kennworts, das zum Verschlüsseln des privaten Schlüssels verwendet wurde. Falls nur der öffentliche Teil des Zertifikats gesichert wird, sind bei diesem Befehl bestimmte Berechtigungen für das Zertifikat erforderlich, und dem Aufrufer darf die VIEW-Berechtigung für das Zertifikat nicht verweigert worden sein.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-exporting-a-certificate-to-a-file"></a>A. Exportieren eines Zertifikats in eine Datei  
 Im folgenden Beispiel wird ein Zertifikat in eine Datei exportiert.  
  
```sql
BACKUP CERTIFICATE sales05 TO FILE = 'c:\storedcerts\sales05cert';  
GO  
```  
  
### <a name="b-exporting-a-certificate-and-a-private-key"></a>B. Exportieren eines Zertifikats und eines privaten Schlüssels  
 Im folgenden Beispiel wird der private Schlüssel des zu sichernden Zertifikats mit dem Kennwort `997jkhUbhk$w4ez0876hKHJH5gh` verschlüsselt.  
  
```sql
BACKUP CERTIFICATE sales05 TO FILE = 'c:\storedcerts\sales05cert'  
    WITH PRIVATE KEY ( FILE = 'c:\storedkeys\sales05key' ,   
    ENCRYPTION BY PASSWORD = '997jkhUbhk$w4ez0876hKHJH5gh' );  
GO  
```  
  
### <a name="c-exporting-a-certificate-that-has-an-encrypted-private-key"></a>C. Exportieren eines Zertifikats mit einem verschlüsselten privaten Schlüssel  
 Im folgenden Beispiel ist der private Schlüssel des Zertifikats in der Datenbank verschlüsselt. Der private Schlüssel muss mit dem Kennwort `9875t6#6rfid7vble7r` entschlüsselt werden. Beim Speichern des Zertifikats in der Sicherungsdatei wird der private Schlüssel mit dem Kennwort `9n34khUbhk$w4ecJH5gh` verschlüsselt.  
  
```sql
BACKUP CERTIFICATE sales09 TO FILE = 'c:\storedcerts\sales09cert'   
    WITH PRIVATE KEY ( DECRYPTION BY PASSWORD = '9875t6#6rfid7vble7r' ,  
    FILE = 'c:\storedkeys\sales09key' ,   
    ENCRYPTION BY PASSWORD = '9n34khUbhk$w4ecJH5gh' );  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)  
 [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md)  
 [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md)  
 [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)  
 [CERTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/certproperty-transact-sql.md)  
  
  

