---
description: sys.dm_database_encryption_keys (Transact-SQL)
title: sys.dm_database_encryption_keys (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_database_encryption_keys
- sys.dm_database_encryption_keys_TSQL
- dm_database_encryption_keys
- dm_database_encryption_keys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_database_encryption_keys dynamic management view
ms.assetid: 56fee8f3-06eb-4fff-969e-abeaa0c4b8e4
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 685f7549d3e3f7558e386ef7876b9fa1f9a75c58
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100337149"
---
# <a name="sysdm_database_encryption_keys-transact-sql"></a>sys.dm_database_encryption_keys (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt Informationen über den Verschlüsselungsstatus einer Datenbank und die ihr zugeordneten Verschlüsselungsschlüssel für die Datenbank zurück. Weitere Informationen zur Datenbankverschlüsselung finden Sie unter [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  
 
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|database_id|**int**|Die ID der Datenbank.|  
|encryption_state|**int**|Gibt an, ob die Datenbank verschlüsselt oder nicht verschlüsselt ist.<br /><br /> 0 = Kein Verschlüsselungsschlüssel für die Datenbank vorhanden, keine Verschlüsselung<br /><br /> 1 = Unverschlüsselt<br /><br /> 2 = Verschlüsselung wird ausgeführt<br /><br /> 3 = Verschlüsselt.<br /><br /> 4 = Schlüsseländerung wird ausgeführt<br /><br /> 5 = Entschlüsselung wird ausgeführt<br /><br /> 6 = Schutzänderung wird ausgeführt (Das Zertifikat oder der asymmetrische Schlüssel, das bzw. der zum Verschlüsseln des Verschlüsselungsschlüssels für die Datenbank verwendet wird, wird geändert.)|  
|create_date|**datetime**|Zeigt das Datum (in UTC) an, an dem der Verschlüsselungsschlüssel erstellt wurde.|  
|regenerate_date|**datetime**|Zeigt das Datum (in UTC) an, an dem der Verschlüsselungsschlüssel neu generiert wurde.|  
|modify_date|**datetime**|Zeigt das Datum (in UTC) an, an dem der Verschlüsselungsschlüssel geändert wurde.|  
|set_date|**datetime**|Zeigt das Datum (in UTC) an, an dem der Verschlüsselungsschlüssel auf die Datenbank angewendet wurde.|  
|opened_date|**datetime**|Zeigt an, wann (in UTC) der Daten Bank Schlüssel zuletzt geöffnet wurde.|  
|key_algorithm|**nvarchar(32)**|Zeigt den Algorithmus an, der für den Schlüssel verwendet wird.|  
|key_length|**int**|Zeigt die Länge des Schlüssels an.|  
|encryptor_thumbprint|**varbinary(20)**|Zeigt den Fingerabdruck der Verschlüsselung an.|  
|encryptor_type|**nvarchar(32)**|**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] bis [aktuelle Version](/troubleshoot/sql/general/determine-version-edition-update-level)).<br /><br /> Beschreibt die Verschlüsselung.|  
|percent_complete|**real**|Prozentualer Anteil der bereits abgeschlossenen Änderung des Verschlüsselungsstatus einer Datenbank. Dieser Wert ist 0, wenn es keine Statusänderung gibt.|
|encryption_state_desc|**nvarchar(32)**|**Gilt für**:  [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] und höher.<br><br> Eine Zeichenfolge, die angibt, ob die Datenbank verschlüsselt oder nicht verschlüsselt ist.<br><br>Keine<br><br>Unverschlüsselte<br><br>.<br><br>DECRYPTION_IN_PROGRESS<br><br>ENCRYPTION_IN_PROGRESS<br><br>KEY_CHANGE_IN_PROGRESS<br><br>PROTECTION_CHANGE_IN_PROGRESS|
|encryption_scan_state|**int**|**Gilt für**:  [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] und höher.<br><br>Gibt den aktuellen Status des Verschlüsselungs Scans an. <br><br>0 = Es wurde kein Scan initiiert, TDE ist nicht aktiviert.<br><br>1 = Überprüfung wird ausgeführt.<br><br>2 = Scan wird ausgeführt, aber angehalten, der Benutzer kann fortgesetzt werden.<br><br>3 = die Überprüfung wurde aus irgendeinem Grund abgebrochen. es ist ein manueller Eingriff erforderlich. Weitere Unterstützung erhalten Sie Microsoft-Support.<br><br>4 = die Überprüfung wurde erfolgreich abgeschlossen, TDE ist aktiviert, und die Verschlüsselung ist abgeschlossen.|
|encryption_scan_state_desc|**nvarchar(32)**|**Gilt für**:  [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] und höher.<br><br>Eine Zeichenfolge, die den aktuellen Status des Verschlüsselungs Scans angibt.<br><br> Keine<br><br>RUNNING<br><br>SUSPENDED<br><br>ABORTED<br><br>Ganz|
|encryption_scan_modify_date|**datetime**|**Gilt für**:  [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] und höher.<br><br> Zeigt das Datum (in UTC) an, an dem der Verschlüsselungs Überprüfungs Zustand zuletzt geändert wurde.|
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   

## <a name="see-also"></a>Weitere Informationen  

 [Sicherheitsbezogene dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Transparente Datenverschlüsselung &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md)   
 [SQL Server-Verschlüsselung](../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Verschlüsselungsschlüssel für SQL Server und Datenbank &#40;Datenbank-Engine&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [ALTER DATABASE SET-Optionen &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [CREATE DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-encryption-key-transact-sql.md)   
 [ALTER DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-encryption-key-transact-sql.md)   
 [DROP DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-encryption-key-transact-sql.md)  
  
