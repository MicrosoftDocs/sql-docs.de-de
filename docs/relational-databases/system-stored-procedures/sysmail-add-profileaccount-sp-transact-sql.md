---
description: sysmail_add_profileaccount_sp (Transact-SQL)
title: sysmail_add_profileaccount_sp (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_add_profileaccount_sp
- sysmail_add_profileaccount_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_profileaccount_sp
ms.assetid: 7cbf430f-1997-45ea-9707-0086184de744
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 209240de58b09e48c4d35f145640f1edf97f1c4a
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89538530"
---
# <a name="sysmail_add_profileaccount_sp-transact-sql"></a>sysmail_add_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fügt einem Profil für die Datenbank-E-Mail ein Konto für die Datenbank-E-Mail hinzu. Führen Sie **sysmail_add_profileaccount_sp** aus, nachdem ein Daten Bankkonto mit [sysmail_add_account_sp &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sysmail-add-account-sp-transact-sql.md)erstellt und ein Datenbankprofil mit [sysmail_add_profile_sp &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sysmail-add-profile-sp-transact-sql.md)erstellt wurde.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sysmail_add_profileaccount_sp { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ,  
    { [ @account_id = ] account_id | [ @account_name = ] 'account_name' }  
    [ , [ @sequence_number = ] sequence_number ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @profile_id = ] profile_id` Die Profil-ID, der das Konto hinzugefügt werden soll. *profile_id* ist vom Datentyp **int**und hat den Standardwert NULL. Es muss entweder *profile_id* oder *profile_name* angegeben werden.  
  
`[ @profile_name = ] 'profile_name'` Der Profilname, dem das Konto hinzugefügt werden soll. *profile_name* ist vom Datentyp **sysname**und hat den Standardwert NULL. Es muss entweder *profile_id* oder *profile_name* angegeben werden.  
  
`[ @account_id = ] account_id` Die Konto-ID, die dem Profil hinzugefügt werden soll. *account_id* ist vom Datentyp **int**und hat den Standardwert NULL. Es muss entweder *account_id* oder *account_name* angegeben werden.  
  
`[ @account_name = ] 'account_name'` Der Name des Kontos, das dem Profil hinzugefügt werden soll. *account_name* ist vom Datentyp **sysname**und hat den Standardwert NULL. Es muss entweder *account_id* oder *account_name* angegeben werden.  
  
`[ @sequence_number = ] sequence_number` Die Sequenznummer des Kontos innerhalb des Profils. *sequence_number* ist vom Datentyp **int**und hat keinen Standardwert. Über die Sequenznummer wird die Reihenfolge festgelegt, in der Konten im Profil verwendet werden.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 Sowohl das Profil als auch das Konto müssen bereits vorhanden sein. Andernfalls gibt die gespeicherte Prozedur einen Fehler zurück.  
  
 Beachten Sie, dass diese gespeicherte Prozedur nicht die Sequenznummer eines Kontos ändert, das bereits dem angegebenen Profil zugeordnet ist. Weitere Informationen zum Aktualisieren der Sequenznummer eines Kontos finden Sie unter [sysmail_update_profileaccount_sp &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sysmail-update-profileaccount-sp-transact-sql.md).  
  
 Über die Sequenznummer wird die Reihenfolge festgelegt, in der Konten im Profil von Datenbank-E-Mail verwendet werden. Für eine neue E-Mail-Nachricht beginnt Datenbank-E-Mail mit dem Konto mit der niedrigsten Sequenznummer. Wenn dieses Konto fehlschlägt, verwendet Datenbank-E-Mail das Konto mit der nächsthöheren Sequenznummer usw., bis entweder Datenbank-E-Mail die Nachricht erfolgreich versendet oder das Konto mit der höchsten Sequenznummer fehlschlägt. Wenn das Konto mit der höchsten Sequenznummer fehlschlägt, unterbricht die Datenbank-E-Mail die Versuche zum Senden der E-Mail für den Zeitraum, der im *AccountRetryDelay* -Parameter von **sysmail_configure_sp**konfiguriert ist. Danach wird das Senden der E-Mail erneut gestartet, wobei mit der niedrigsten Sequenznummer begonnen wird. Verwenden Sie den *AccountRetryAttempts* -Parameter von **sysmail_configure_sp**, um zu konfigurieren, wie oft der externe Mailprozess versuchen soll, die E-Mail-Nachricht mithilfe der einzelnen Konten im angegebenen Profil zu senden.  
  
 Sind mehrere Konten mit der gleichen Sequenznummer vorhanden, verwendet die Datenbank-E-Mail nur eines dieser Konten für eine bestimmte E-Mail-Nachricht. In diesem Fall kann Datenbank-E-Mail nicht sicherstellen, welches der Konten für diese Sequenznummer verwendet wird oder dass für die einzelnen Nachrichten jeweils dasselbe Konto verwendet wird.  
  
 Die gespeicherte Prozedur **sysmail_add_profileaccount_sp** befindet sich in der **msdb** -Datenbank mit dem **dbo** -Schema als Besitzer. Handelt es sich bei der aktuellen Datenbank nicht um **msdb**, muss die Prozedur mit einem dreiteiligen Namen ausgeführt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Über die Ausführungsberechtigungen für diese Prozedur verfügen standardmäßig die Mitglieder der festen Serverrolle **sysadmin** .  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird das Profil `AdventureWorks Administrator` dem Konto `Audit Account`zugeordnet. Das Konto für die Überwachung weist die Sequenznummer 1 auf.  
  
```  
EXECUTE msdb.dbo.sysmail_add_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator',  
    @account_name = 'Audit Account',  
    @sequence_number = 1 ;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbank-E-Mail](../../relational-databases/database-mail/database-mail.md)   
 [Erstellen eines Datenbank-E-Mail Kontos](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Konfigurationsobjekte Datenbank-E-Mail](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Datenbank-E-Mail gespeicherter Prozeduren &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
