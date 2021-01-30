---
description: sysmail_help_account_sp (Transact-SQL)
title: sysmail_help_account_sp (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_account_sp_TSQL
- sysmail_help_account_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_account_sp
ms.assetid: 87c7c39c-8e05-4e68-9272-45f908809c3b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1931cbc58177686c1b7f09b069bc9ba061447e76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181978"
---
# <a name="sysmail_help_account_sp-transact-sql"></a>sysmail_help_account_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Listet Informationen (mit Ausnahme von Kennwörtern) zu Datenbank-E-Mail-Konten auf.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sysmail_help_account_sp [ [ @account_id = ] account_id | [ @account_name = ] 'account_name' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @account_id = ] account_id` Die Konto-ID des Kontos, für das Informationen aufgelistet werden sollen. *account_id* ist vom Datentyp **int** und hat den Standardwert NULL.  
  
`[ @account_name = ] 'account_name'` Der Name des Kontos, für das Informationen aufgelistet werden sollen. *account_name* ist vom Datentyp **sysname** und hat den Standardwert NULL.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="result-sets"></a>Resultsets  
 Gibt ein Resultset mit den nachfolgend aufgelisteten Spalten zurück.  
  
| Spaltenname | Datentyp | BESCHREIBUNG |
| ----------- | --------- | ----------- |
|**account_id**|**int**|ID des Kontos|  
|**name**|**sysname**|Der Kontoname.|  
|**description**|**nvarchar(256)**|Beschreibung des Kontos|  
|**email_address**|**nvarchar(128)**|E-Mail-Adresse, von der aus Nachrichten versandt werden|  
|**display_name**|**nvarchar(128)**|Der Anzeigename des Kontos.|  
|**replyto_address**|**nvarchar(128)**|Adresse, an die Antworten auf die Nachrichten von diesem Konto versandt werden|  
|**Server Type**|**sysname**|Typ des E-Mail-Servers für das Konto|  
|**Servername**|**sysname**|Name des E-Mail-Servers für das Konto|  
|**port**|**int**|Portnummer, die der E-Mail-Server verwendet|  
|**username**|**nvarchar(128)**|Der Benutzername für die Anmeldung am E-Mail-Server, wenn der E-Mail-Server eine Authentifizierung verwendet. Wenn **username** den Wert NULL hat, verwendet Datenbank-E-Mail keine Authentifizierung für dieses Konto.|  
|**use_default_credentials**|**bit**|Gibt an, ob E-Mail mithilfe der Anmeldeinformationen von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]an den SMTP-Server gesendet wird. **use_default_credentials** ist vom Datentyp bit und hat keinen Standardwert. Wenn dieser Parameter 1 ist, verwendet Datenbank-E-Mail keine Anmeldeinformationen des [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] -Dienstes. Wenn dieser Parameter 0 ist, verwendet Datenbank-E-Mail den **\@ Benutzernamen** und das **\@ Kennwort** für die Authentifizierung auf dem SMTP-Server. Wenn **\@ username** und **\@ Password** NULL sind, verwendet Datenbank-E-Mail die anonyme Authentifizierung. Wenden Sie sich an Ihren SMTP-Administrator, bevor Sie diesen Parameter angeben.|  
|**enable_ssl**|**bit**|Gibt an, ob Datenbank-E-Mail die Kommunikation mit Transport Layer Security (TLS) verschlüsselt, das zuvor als Secure Sockets Layer (SSL) bezeichnet wurde. Verwenden Sie diese Option, wenn TLS auf dem SMTP-Server erforderlich ist. **enable_ssl** ist vom Datentyp bit und hat keinen Standardwert. 1 gibt an Datenbank-E-Mail die Kommunikation mit TLS verschlüsselt. der Wert 0 gibt an, Datenbank-E-Mail die e-Mail ohne TLS-Verschlüsselung sendet.|  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn keine *account_id* - oder *account_name* -Parameter bereitgestellt werden, listet **sysmail_help_account** Informationen für alle Datenbank-E-Mail-Konten in der Microsoft SQL Server-Instanz auf.  
  
 Die gespeicherte Prozedur **sysmail_help_account_sp** wird in der **msdb** -Datenbank gespeichert und befindet sich im Besitz des **dbo** -Schemas. Handelt es sich bei der aktuellen Datenbank nicht um **msdb**, muss die Prozedur mit einem dreiteiligen Namen ausgeführt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Über die Ausführungsberechtigungen für diese Prozedur verfügen standardmäßig die Mitglieder der festen Serverrolle **sysadmin** .  
  
## <a name="examples"></a>Beispiele  
 **A. Auflisten der Informationen für alle Konten**  
  
 Im folgenden Beispiel werden die Kontodaten für alle Konten in der Instanz aufgelistet.  
  
```  
EXECUTE msdb.dbo.sysmail_help_account_sp ;  
```  
  
 Es folgt ein Beispielresultset, das auf Zeilenlänge umformatiert wurde:  
  
```  
account_id  name                         description                             email_address             display_name                     replyto_address servertype servername                port        username use_default_credentials enable_ssl  
----------- ---------------------------- --------------------------------------- ------------------------- -------------------------------- --------------- ---------- ------------------------- ----------- -------- ----------------------- ----------  
148         AdventureWorks Administrator Mail account for administrative e-mail. dba@Adventure-Works.com   AdventureWorks Automated Mailer  NULL            SMTP       smtp.Adventure-Works.com  25          NULL 0                          0        
149         Audit Account                Account for audit e-mail.               audit@Adventure-Works.com Automated Mailer (Audit)         NULL            SMTP       smtp.Adventure-Works.com  25          NULL 0                          0        
```  
  
 **B. Auflisten der Informationen für ein spezifisches Konto**  
  
 Im folgenden Beispiel werden die Kontodaten für das Konto mit dem Namen `AdventureWorks Administrator`aufgelistet.  
  
```  
EXECUTE msdb.dbo.sysmail_help_account_sp  
    @account_name = 'AdventureWorks Administrator' ;  
```  
  
 Es folgt ein Beispielresultset, das auf Zeilenlänge umformatiert wurde:  
  
```  
account_id  name                         description                             email_address             display_name                     replyto_address servertype servername                port        username use_default_credentials enable_ssl  
----------- ---------------------------- ------------------------------------------------------ ------------------------- ---------------- ---------- ------------------------- ----------- -------- ----------------------- ----------  
148         AdventureWorks Administrator Mail account for administrative e-mail. dba@Adventure-Works.com   AdventureWorks Automated Mailer  NULL            SMTP       smtp.Adventure-Works.com  25          NULL     0                       0       
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbank-E-Mail](../../relational-databases/database-mail/database-mail.md)   
 [Erstellen eines Datenbank-E-Mail Kontos](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Datenbank-E-Mail gespeicherter Prozeduren &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
