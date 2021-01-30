---
description: sp_expired_subscription_cleanup (Transact-SQL)
title: sp_expired_subscription_cleanup (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_expired_subscription_cleanup
- SP_EXPIRED_SUBSCRIPTION_CLEANUP_TSQL
helpviewer_keywords:
- sp_expired_subscription_cleanup
ms.assetid: 6abc29fe-d77a-4673-9d99-ae31c688012c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1c555f2aaabf3e245261a07cc0bc7257e2601926
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187975"
---
# <a name="sp_expired_subscription_cleanup-transact-sql"></a>sp_expired_subscription_cleanup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Überprüft den Status aller Abonnements für jede Veröffentlichung und löscht abgelaufene Abonnements. Diese gespeicherte Prozedur wird auf dem Verleger für eine beliebige Datenbank oder auf dem Verteiler für die Verteilungs Datenbank für einen nicht-- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verleger ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_expired_subscription_cleanup [ [ @publisher = ] 'publisher' ]   
```  
  
## <a name="arguments"></a>Argumente  
`[ @publisher = ] 'publisher'` Der Name eines nicht-- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verlegers. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Sie sollten diesen Parameter nicht für einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verleger festlegen.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_expired_subscription_cleanup** wird bei allen Replikations Typen verwendet.  
  
 **sp_expired_subscription_cleanup** wird vom Auftrag Cleanup abgelaufener Abonnements ausgeführt, um alle 24 Stunden abgelaufene Abonnements aus Veröffentlichungs Datenbanken zu erkennen und zu entfernen. Wenn ein Abonnement nicht mehr aktuell ist, wenn es also während der Beibehaltungsdauer nicht mit dem Verleger synchronisiert wurde, wird die Veröffentlichung als abgelaufen bezeichnet. Die Ablaufverfolgungsdaten des Abonnements werden dann auf dem Verleger gelöscht. Weitere Informationen finden Sie unter [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_expired_subscription_cleanup** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_mergesubscription_cleanup &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-mergesubscription-cleanup-transact-sql.md)   
 [sp_subscription_cleanup &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
