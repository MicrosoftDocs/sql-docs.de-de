---
description: sysmail_add_principalprofile_sp (Transact-SQL)
title: sysmail_add_principalprofile_sp (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_add_principalprofile_sp_TSQL
- sysmail_add_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_principalprofile_sp
ms.assetid: b2a0b313-abb9-4c23-8511-db77ca8172b3
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 7e3c8085bdbdf45deac3fe6190bbad263e4d7d5b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88489040"
---
# <a name="sysmail_add_principalprofile_sp-transact-sql"></a>sysmail_add_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Erteilt einem msdb-Daten Bank Prinzipal die Berechtigung, ein Datenbank-E-Mail Profil zu verwenden. Der Daten Bank Prinzipal muss einem SQL Server Authentifizierungs Benutzer, einem Windows-Benutzer oder einer Windows-Gruppe zugeordnet werden.
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sysmail_add_principalprofile_sp  { [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ,  
    { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' }  
    [ , [ @is_default ] = 'is_default' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @principal_id = ] principal_id` Die ID des Daten Bank Benutzers oder der Daten Bank Rolle in der **msdb** -Datenbank für die Zuordnung. *principal_id* ist vom Datentyp **int**und hat den Standardwert NULL. Es muss entweder *principal_id* oder *principal_name* angegeben werden. Wenn *principal_id* auf **0** festgelegt ist, wird dieses Profil öffentlich, wobei der Zugriff auf alle Prinzipale in der Datenbank erteilt wird.  
  
`[ @principal_name = ] 'principal_name'` Der Name des Daten Bank Benutzers oder der Daten Bank Rolle in der **msdb** -Datenbank für die Zuordnung. *principal_name* ist vom Datentyp **sysname**und hat den Standardwert NULL. Es muss entweder *principal_id* oder *principal_name* angegeben werden. Wenn *principal_name* auf **'public'** festgelegt ist, wird dieses Profil öffentlich, wobei der Zugriff auf alle Prinzipale in der Datenbank erteilt wird.  
  
`[ @profile_id = ] profile_id` Die ID des Profils für die Zuordnung. *profile_id* ist vom Datentyp **int**und hat den Standardwert NULL. Es muss entweder *profile_id* oder *profile_name* angegeben werden.  
  
`[ @profile_name = ] 'profile_name'` Der Name des Profils für die Zuordnung. *profile_name* ist vom Datentyp **sysname**und hat keinen Standardwert. Es muss entweder *profile_id* oder *profile_name* angegeben werden.  
  
`[ @is_default = ] is_default` Gibt an, ob dieses Profil das Standardprofil für den Prinzipal ist. Ein Prinzipal muss genau ein Standardprofil haben. *is_default* ist vom Datentyp **bit**und hat keinen Standardwert.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn Sie ein Profil als öffentlich festlegen möchten, geben Sie die ** \@ principal_id** **0** oder eine ** \@ principal_name** **Public**an. Ein öffentliches Profil ist für alle Benutzer der **msdb** -Datenbank verfügbar. Allerdings müssen diese Benutzer auch Mitglieder von **DatabaseMailUserRole** sein, wenn sie **sp_send_dbmail**ausführen können sollen.  
  
 Ein Datenbankbenutzer kann nur ein Standardprofil besitzen. Wenn ** \@ is_default** '**1**' ist und der Benutzer bereits einem oder mehreren Profilen zugeordnet ist, wird das angegebene Profil zum Standardprofil für den Benutzer. Das zuvor als Standardprofil verwendete Profil ist dem Benutzer weiter zugeordnet, es ist jedoch nicht mehr als Standardprofil festgelegt.  
  
 Wenn ** \@ is_default** "**0**" ist und keine andere Zuordnung vorhanden ist, gibt die gespeicherte Prozedur einen Fehler zurück.  
  
 Die gespeicherte Prozedur **sysmail_add_principalprofile_sp** befindet sich in der **msdb** -Datenbank mit dem **dbo** -Schema als Besitzer. Handelt es sich bei der aktuellen Datenbank nicht um **msdb**, muss die Prozedur mit einem dreiteiligen Namen ausgeführt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Über die Ausführungsberechtigungen für diese Prozedur verfügen standardmäßig die Mitglieder der festen Serverrolle **sysadmin** .  
  
## <a name="examples"></a>Beispiele  
 **A. Erstellen einer Zuordnung und Festlegen des Standardprofils**  
  
 Im folgenden Beispiel wird eine Zuordnung zwischen dem Profil `AdventureWorks Administrator Profile` und dem **msdb** -Datenbankbenutzer `ApplicationUser`erstellt. Als Profil wird das Standardprofil des Benutzers verwendet.  
  
```  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @principal_name = 'ApplicationUser',  
    @profile_name = 'AdventureWorks Administrator Profile',  
    @is_default = 1 ;  
```  
  
 **B. Festlegen eines Profils als öffentliches Standardprofil**  
  
 In diesem Beispiel wird das Profil `AdventureWorks Public Profile` als öffentliches Standardprofil für Benutzer in der **msdb** -Datenbank festgelegt.  
  
```  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @principal_name = 'public',  
    @profile_name = 'AdventureWorks Public Profile',  
    @is_default = 1 ;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbank-E-Mail](../../relational-databases/database-mail/database-mail.md)   
 [Konfigurationsobjekte Datenbank-E-Mail](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Datenbank-E-Mail gespeicherter Prozeduren &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
