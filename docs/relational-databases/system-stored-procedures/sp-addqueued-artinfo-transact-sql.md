---
description: sp_addqueued_artinfo (Transact-SQL)
title: sp_addqueued_artinfo (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addqueued_artinfo
- sp_addqueued_artinfo_TSQL
helpviewer_keywords:
- sp_addqueued_artinfo
ms.assetid: decdb6eb-3dcd-4053-a21d-fd367c3fbafb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c0899ff490b553471fc766413fdf0cca27955fa0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206659"
---
# <a name="sp_addqueued_artinfo-transact-sql"></a>sp_addqueued_artinfo (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  
  
> [!IMPORTANT]  
>  Die [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) Prozedur sollte anstelle **sp_addqueued_artinfo** verwendet werden. [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) generiert ein Skript, das die **sp_addqueued_artinfo** und **sp_addsynctrigger** Aufrufe enthält.  
  
 Erstellt die [MSsubscription_articles](../../relational-databases/system-tables/mssubscription-articles-transact-sql.md) Tabelle auf dem Abonnenten, die zum Nachverfolgen von Artikel Abonnement Informationen (verzögertes Aktualisieren über eine Warteschlange und sofortiges Update mit verzögertem Update als Failover) verwendet wird. Diese gespeicherte Prozedur wird auf dem Abonnenten für die Abonnement Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_addqueued_artinfo [ @artid= ] 'artid'  
        , [ @article= ] 'article'  
        , [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'  
        , [ @dest_table= ] 'dest_table'  
        , [ @owner = ] 'owner'  
        , [ @cft_table= ] 'cft_table'  
```  
  
## <a name="arguments"></a>Argumente  
`[ @artid = ] 'artid'` Der Name der Artikel-ID. *artid* ist vom Datentyp **int** und hat keinen Standardwert  
  
`[ @article = ] 'article'` Der Name des Artikels, für den ein Skript erstellt werden soll. *Artikel* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert  
  
`[ @publisher = ] 'publisher'` Der Name des Verleger Servers. *Publisher* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @publisher_db = ] 'publisher_db'` Der Name der Verleger Datenbank. *publisher_db* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @publication = ] 'publication'` Der Name der Veröffentlichung, für die ein Skript erstellt werden soll. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @dest_table = ] _'dest_table'` Der Name der Ziel Tabelle. *dest_table* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
 [**@owner =** ] **'**_Besitzer_**'**  
 Entspricht dem Eigentümer des Abonnements. *Owner* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @cft_table = ] 'cft_table'` Der Name der Konflikt Tabelle mit verzögertem Update über eine Warteschlange für diesen Artikel. *cft_table* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_addqueued_artinfo** wird von der Verteilungs-Agent als Teil der Abonnement Initialisierung verwendet. Diese gespeicherte Prozedur wird normalerweise nicht von Benutzern ausgeführt, kann jedoch hilfreich sein, wenn der Benutzer manuell ein Abonnement einrichten muss.  
  
 [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) anstelle **sp_addqueued_artinfo**.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_addqueued_artinfo** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [sp_script_synctran_commands &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md)   
 [MSsubscription_articles &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/mssubscription-articles-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
