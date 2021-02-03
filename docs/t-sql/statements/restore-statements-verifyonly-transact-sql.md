---
description: RESTORE-Anweisungen - VERIFYONLY (Transact-SQL)
title: RESTORE VERIFYONLY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- VERIFYONLY
- RESTORE VERIFYONLY
- VERIFYONLY_TSQL
- RESTORE_VERIFYONLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE VERIFYONLY statement
- backups [SQL Server], verifying
- verifying backups
- checking backups
ms.assetid: cba3b6a0-b48e-4c94-812b-5b3cbb408bd6
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: b76a49d34d0603d62ee7160bb2f4ef9c32d482a7
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237795"
---
# <a name="restore-statements---verifyonly-transact-sql"></a>RESTORE-Anweisungen - VERIFYONLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Überprüft die Sicherung, stellt sie jedoch nicht wieder her, und überprüft die Vollständigkeit des Sicherungssatzes sowie die Lesbarkeit der gesamten Sicherung. Allerdings versucht RESTORE VERIFYONLY nicht, die Struktur der auf den Sicherungsvolumes befindlichen Daten zu überprüfen. In [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wurde RESTORE VERIFYONLY um zusätzliche Prüfungen der Daten erweitert, sodass Fehler mit größerer Wahrscheinlichkeit gefunden werden. Das Ziel liegt darin, einen tatsächlichen Wiederherstellungsvorgang so authentisch wie möglich zu imitieren. Weitere Informationen finden Sie in den Hinweisen.  

 Wenn die Sicherung gültig ist, gibt [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] eine Erfolgsmeldung zurück.  
  
> [!NOTE]  
>  Eine Beschreibung der Argumente finden Sie unter [RESTORE-Argumente &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
RESTORE VERIFYONLY  
FROM <backup_device> [ ,...n ]  
[ WITH    
 {  
   LOADHISTORY   
  
--Restore Operation Option  
 | MOVE 'logical_file_name_in_backup' TO 'operating_system_file_name'   
          [ ,...n ]   
  
--Backup Set Options  
 | FILE = { backup_set_file_number | @backup_set_file_number }   
 | PASSWORD = { password | @password_variable }   
  
--Media Set Options  
 | MEDIANAME = { media_name | @media_name_variable }   
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }  
  
--Error Management Options  
 | { CHECKSUM | NO_CHECKSUM }   
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }  
  
--Monitoring Options  
 | STATS [ = percentage ]   
  
--Tape Options  
 | { REWIND | NOREWIND }   
 | { UNLOAD | NOUNLOAD }    
 } [ ,...n ]  
]  
[;]  
  
<backup_device> ::=  
{   
   { logical_backup_device_name |  
      @logical_backup_device_name_var }  
   | { DISK | TAPE | URL } = { 'physical_backup_device_name' |  
       @physical_backup_device_name_var }   
}  
  
```  
 > [!NOTE] 
> URL ist das Format, das verwendet wird, um den Speicherort und den Dateinamen für Microsoft Azure Blob Storage anzugeben und wird ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 Cu2 unterstützt. Obwohl es sich bei Microsoft Azure Storage um einen Dienst handelt, ist die Implementierung mit einem Datenträger und Band vergleichbar, um für alle drei Geräte eine konsistente und nahtlose Wiederherstellung zu ermöglichen.
 
## <a name="arguments"></a>Argumente  
 Eine Beschreibung der RESTORE VERIFYONLY-Argumente finden Sie unter [RESTORE-Argumente &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
## <a name="general-remarks"></a>Allgemeine Hinweise  
 Der Mediensatz oder der Sicherungssatz muss ein Minimum an einwandfreien Daten enthalten, damit er als Microsoft Tape Format interpretiert werden kann. Andernfalls wird RESTORE VERIFYONLY angehalten und zeigt an, dass das Format der Sicherung ungültig ist.  
  
 Von RESTORE VERIFYONLY werden u. a. folgende Überprüfungen ausgeführt:  
  
-   Vollständigkeit des Sicherungssatzes und Lesbarkeit aller Volumes.  
  
-   Richtigkeit einiger Headerfelder der Datenbankseiten, z. B. die Seiten-ID (als ob die Daten geschrieben werden sollten).  
  
-   Prüfsumme (falls auf dem Medium vorhanden).  
  
-   Verfügbarkeit von Speicherplatz auf den Zielgeräten.  
  
> [!NOTE]  
>  Die Verwendung von RESTORE VERIFYONLY für eine Datenbankmomentaufnahme ist nicht möglich. Sie können eine Datenbank-Momentaufnahme vor einem Wiederherstellungsvorgang überprüfen, indem Sie DBCC CHECKDB ausführen.  
  
> [!NOTE]  
>  Mit Momentaufnahmesicherungen bestätigt RESTORE VERIFYONLY das Vorhandensein der Momentaufnahmen an den in der Sicherungsdatei angegebenen Speicherorten. Momentaufnahmesicherungen sind ein neues Feature in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]. Weitere Informationen zu Momentaufnahmesicherungen finden Sie unter [ Dateimomentaufnahme-Sicherungen für Datenbankdateien in Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
## <a name="security"></a>Sicherheit  
 Bei einem Sicherungsvorgang können optional Kennwörter für einen Mediensatz, einen Sicherungssatz oder für beides angegeben werden. Wurde ein Kennwort für einen Mediensatz oder Sicherungssatz definiert, müssen die richtigen Kennwörter in der RESTORE-Anweisung angegeben werden. Über diese Kennwörter werden nicht autorisierte Wiederherstellungsoptionen und unbefugtes Anfügen von Sicherungssätzen an Medien mithilfe der Tools von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verhindert. Mit einem Kennwort kann jedoch das Überschreiben eines Mediums mithilfe der Option FORMAT der BACKUP-Anweisung nicht verhindert werden.  
  
> [!IMPORTANT]  
>  Dieses Kennwort bietet also nur unzureichenden Schutz. Es soll vermeiden, dass autorisierte wie nicht autorisierte Benutzer mithilfe von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Tools fehlerhafte Wiederherstellungen durchführen. Es verhindert jedoch nicht das Lesen der Sicherungsdaten mit anderen Mitteln oder das Ersetzen des Kennworts. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]Eine bewährte Methode für den Schutz von Sicherungen ist das Verwahren von Sicherungsbändern an einem sicheren Ort oder das Sichern in Datenträgerdateien, die durch eine adäquate Zugriffssteuerungsliste (ACL, Access Control List) geschützt sind. Die ACLs sollten für den Verzeichnisstamm eingerichtet werden, unter dem die Sicherungen erstellt werden.  
  
### <a name="permissions"></a>Berechtigungen  
 In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höheren Versionen benötigen Sie die CREATE DATABASE-Berechtigung, um Informationen zu Sicherungssätzen oder Sicherungsmedien abzurufen. Weitere Informationen finden Sie unter [GRANT (Datenbankberechtigungen) &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
 
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die Sicherung vom Datenträger verifiziert.
  
```sql  
RESTORE VERIFYONLY FROM DISK = 'D:\AdventureWorks.bak';
GO
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Mediensätze, Medienfamilien und Sicherungssätze &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Sicherungsverlauf und Headerinformationen &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  
