---
description: sp_replqueuemonitor (Transact-SQL)
title: sp_replqueuemonitor (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replqueuemonitor
- sp_replqueuemonitor_TSQL
helpviewer_keywords:
- sp_replqueuemonitor
ms.assetid: 6909a3f1-43a2-4df5-a6a5-9e6f347ac841
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8fa6635b4f31692e5f45e848043e44496e2d702f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211666"
---
# <a name="sp_replqueuemonitor-transact-sql"></a>sp_replqueuemonitor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Listet die Warteschlangen Nachrichten aus einer [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Warteschlange oder [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing für Abonnements mit verzögertem Update über eine angegebene Veröffentlichung. Falls [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Warteschlangen verwendet werden, wird diese gespeicherte Prozedur auf dem Abonnenten für die Abonnementdatenbank ausgeführt. Falls Message Queuing verwendet wird, wird sie auf dem Verteiler für die Verteilungsdatenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_replqueuemonitor [ @publisher = ] 'publisher'  
    [ , [ @publisherdb = ] 'publisher_db' ]  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @tranid = ] 'tranid' ]  
    [ , [ @queuetype = ] 'queuetype' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publisher = ] 'publisher'` Der Name des Verlegers. *Publisher* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Der Server muss für das Veröffentlichen konfiguriert sein. NULL gibt alle Verleger an.  
  
`[ @publisherdb = ] 'publisher_db' ]` Der Name der Veröffentlichungs Datenbank. *publisher_db* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. NULL gibt alle Veröffentlichungsdatenbanken an.  
  
`[ @publication = ] 'publication' ]` Der Name der Veröffentlichung. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. NULL gibt alle Veröffentlichungen an.  
  
`[ @tranid = ] 'tranid' ]` Die Transaktions-ID. *tranid* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. NULL gibt alle Transaktionen an.  
  
`[ @queuetype = ] 'queuetype' ]` Der Typ der Warteschlange, in der Transaktionen gespeichert werden. Queue *Type* ist vom Datentyp **tinyint** . der Standardwert ist **0**. die folgenden Werte sind möglich.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**0**|Alle Warteschlangentypen|  
|**1**|Message Queuing|  
|**2**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Warteschlange|  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_replqueuemonitor** wird bei der Momentaufnahme-oder Transaktions Replikation mit Abonnements mit verzögertem Update über eine Warteschlange verwendet. Die Warteschlangennachrichten, die keine SQL-Befehle enthalten oder Teil eines umfassenden SQL-Befehls sind, werden nicht angezeigt.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_replqueuemonitor** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
