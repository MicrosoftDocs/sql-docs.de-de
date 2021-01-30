---
description: sp_control_dbmasterkey_password (Transact-SQL)
title: sp_control_dbmasterkey_password (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_control_dbmasterkey_password
- sp_control_dbmasterkey_password_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_control_dbmasterkey_password
ms.assetid: 63979a87-42a2-446e-8e43-30481faaf3ca
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 06de3460a55eecba6c1525576caec85d6baa905f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190035"
---
# <a name="sp_control_dbmasterkey_password-transact-sql"></a>sp_control_dbmasterkey_password (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Wird zum Hinzufügen oder Löschen von Anmeldeinformationen verwendet, die das zum Öffnen des Datenbank-Hauptschlüssels benötigte Kennwort enthalten.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_control_dbmasterkey_password @db_name = 'database_name,  
     @password = 'master_key_password' , @action = { 'add' | 'drop' }  
```  
  
## <a name="arguments"></a>Argumente  
 @db_name= N '*database_name*'  
 Gibt den Namen der Datenbank an, die diesen Anmeldeinformationen zugeordnet ist. Es darf sich nicht um eine Systemdatenbank handeln. *database_name* ist vom Datentyp **nvarchar**.  
  
 @password= N '*Kennwort*'  
 Gibt das Kennwort des Hauptschlüssels an. Das *Kennwort* ist " **nvarchar**".  
  
 @action= N ' Add '  
 Gibt an, dass dem Anmeldeinformationenspeicher Anmeldeinformationen für die angegebene Datenbank hinzugefügt werden. In den Anmeldeinformationen ist das Kennwort für den Datenbank-Hauptschlüssel enthalten. Der an Übergabe Wert @action ist **nvarchar**.  
  
 @action= N ' Drop '  
 Gibt an, dass Anmeldeinformationen für die angegebene Datenbank aus dem Anmeldeinformationenspeicher gelöscht werden. Der an Übergabe Wert @action ist **nvarchar**.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ein Datenbank-Hauptschlüssel zum Entschlüsseln oder Verschlüsseln eines Schlüssels benötigt wird, wird von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versucht, den Datenbank-Hauptschlüssel mit dem Diensthauptschlüssel der Instanz zu entschlüsseln. Wenn bei der Entschlüsselung ein Fehler auftritt, wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im Anmelde Informationsspeicher nach Anmelde Informationen des Haupt Schlüssels durchsucht, die dieselbe Familien-GUID wie die Datenbank aufweisen, für die der Hauptschlüssel benötigt wird. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , den Datenbank-Hauptschlüssel mit allen übereinstimmenden Anmeldeinformationen zu entschlüsseln, bis die Entschlüsselung erfolgreich ausgeführt wurde oder keine Anmeldeinformationen mehr vorhanden sind.  
  
> [!CAUTION]  
>  Wenn von sa und anderen Serverprinzipalen mit hohen Privilegien nicht auf eine Datenbank zugegriffen werden soll, ist das Erstellen von Hauptschlüssel-Anmeldeinformationen nicht empfehlenswert. Sie können eine Datenbank so konfigurieren, dass ihre Schlüsselhierarchie nicht vom Diensthauptschlüssel entschlüsselt werden kann. Diese Option wird als sicherer Schutz für Datenbanken mit verschlüsselten Informationen unterstützt, auf die von sa oder anderen Serverprinzipalen mit hohen Privilegien nicht zugegriffen werden soll. Durch das Erstellen eines Hauptschlüssels für eine Datenbank dieser Art wird der sichere Schutz entfernt, sodass sa und andere Serverprinzipale mit hohen Privilegien die Datenbank entschlüsseln können.  
  
 Anmelde Informationen, die mit sp_control_dbmasterkey_password erstellt werden, sind in der [sys.master_key_passwords](../../relational-databases/system-catalog-views/sys-master-key-passwords-transact-sql.md) -Katalog Sicht sichtbar. Die Namen der für Datenbank-Hauptschlüssel erstellten Anmeldeinformationen besitzen das folgende Format: `##DBMKEY_<database_family_guid>_<random_password_guid>##`. Das Kennwort wird als Anmeldeinformation-Kennwort gespeichert. Für jedes dem Anmeldeinformationenspeicher hinzugefügte Kennwort ist eine Zeile in sys.credentials vorhanden.  
  
 sp_control_dbmasterkey_password kann nicht verwendet werden, um Anmeldeinformationen für die folgenden Systemdatenbanken zu erstellen: master, model, msdb oder tempdb.  
  
 sp_control_dbmasterkey_password überprüft nicht, ob der Hauptschlüssel der angegebenen Datenbank mit dem Kennwort geöffnet werden kann.  
  
 Falls Sie ein Kennwort angeben, das bereits in Anmeldeinformationen für die angegebene Datenbank gespeichert ist, erzeugt sp_control_dbmasterkey_password einen Fehler.  
  
> [!NOTE]  
>  Zwei Datenbanken aus unterschiedlichen Serverinstanzen können denselben Familien-GUID verwenden. In diesem Fall verwenden die Datenbanken dieselben Hauptschlüssel-Datensätze im Anmeldeinformationenspeicher.  
  
 Die an sp_control_dbmasterkey_password übergebenen Parameter werden in Ablaufverfolgungen nicht angezeigt.  
  
> [!NOTE]  
>  Wenn Sie zum Öffnen des Datenbank-Hauptschlüssels die Anmeldeinformationen verwenden, die mit sp_control_dbmasterkey_password hinzugefügt wurden, wird der Datenbank-Hauptschlüssel mit dem Diensthauptschlüssel neu verschlüsselt. Wenn sich die Datenbank im schreibgeschützten Modus befindet, schlägt die Neuverschlüsselung fehl, und der Datenbank-Hauptschlüssel bleibt unverschlüsselt. Für den anschließenden Zugriff auf den Datenbank-Hauptschlüssel müssen Sie die OPEN MASTER KEY-Anweisung und ein Kennwort verwenden. Sie können die Verwendung eines Kennworts vermeiden, wenn Sie die Anmeldeinformationen erstellen, bevor Sie die Datenbank in den schreibgeschützten Modus versetzen.  
  
 **Potenzielles Problem** mit der Abwärtskompatibilität: Derzeit überprüft die gespeicherte Prozedur nicht, ob ein Hauptschlüssel vorhanden ist. Dies wird aus Gründen der Abwärtskompatibilität zugelassen, es wird jedoch eine Warnung angezeigt. Dieses Verhalten ist als veraltet markiert. In einer zukünftigen Version muss der Hauptschlüssel vorhanden sein, und das in der gespeicherten **sp_control_dbmasterkey_password** Prozedur verwendete Kennwort muss das gleiche Kennwort wie eines der Kenn Wörter sein, die zum Verschlüsseln des Datenbank-Haupt Schlüssels verwendet werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** .  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-creating-a-credential-for-the-adventureworks2012-master-key"></a>A. Erstellen von Anmeldeinformationen für den AdventureWorks2012-Hauptschlüssel  
 Im folgenden Beispiel werden Anmeldeinformationen für den Hauptschlüssel der `AdventureWorks2012`-Datenbank erstellt, und das Kennwort für den Hauptschlüssel wird als geheimer Eintrag in den Anmeldeinformationen gespeichert. Da alle Parameter, die an übermittelt werden, `sp_control_dbmasterkey_password` vom Datentyp **nvarchar** sein müssen, werden die Text Zeichenfolgen mit dem Umwandlungs Operator konvertiert `N` .  
  
```  
EXEC sp_control_dbmasterkey_password @db_name = N'AdventureWorks2012',   
    @password = N'sdfjlkj#mM00sdfdsf98093258jJlfdk4', @action = N'add';  
GO  
```  
  
### <a name="b-dropping-a-credential-for-a-database-master-key"></a>B. Löschen von Anmeldeinformationen für einen Datenbank-Hauptschlüssel  
 Im folgenden Beispiel werden die in Beispiel A erstellten Anmeldeinformationen entfernt. Beachten Sie, dass alle Parameter, einschließlich des Kennworts, erforderlich sind.  
  
```  
EXEC sp_control_dbmasterkey_password @db_name = N'AdventureWorks2012',   
    @password = N'sdfjlkj#mM00sdfdsf98093258jJlfdk4', @action = N'drop';  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Einrichten einer verschlüsselten Spiegeldatenbank](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)   
 [Security Stored Procedures &#40;Transact-SQL&#41; (Gespeicherte Sicherheitsprozeduren (Transact-SQL))](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)   
 [Anmeldeinformationen &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)  
  
  
