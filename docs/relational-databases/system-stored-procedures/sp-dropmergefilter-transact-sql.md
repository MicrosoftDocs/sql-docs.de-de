---
description: sp_dropmergefilter (Transact-SQL)
title: sp_dropmergefilter (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropmergefilter_TSQL
- sp_dropmergefilter
helpviewer_keywords:
- sp_dropmergefilter
ms.assetid: 798586d7-05f3-4a5e-bea8-a34b7b52d0fd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2e57a65a6ab74050b855721d41a6d7304691f55e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208242"
---
# <a name="sp_dropmergefilter-transact-sql"></a>sp_dropmergefilter (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Löscht einen Mergefilter. **sp_dropmergefilter** löscht alle Mergefilterspalten, die für den zu löschenden Mergefilter definiert sind. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_dropmergefilter [ @publication= ] 'publication', [ @article= ] 'article'     , [ @filtername= ] 'filtername'  
    [ , [ @force_invalidate_snapshot= ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publication = ] 'publication'` Der Name der Veröffentlichung. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @article = ] 'article'` Der Name des Artikels. der *Artikel* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @filtername = ] 'filtername'` Der Name des Filters, der gelöscht werden soll. *Filter Name* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Aktiviert oder deaktiviert die Möglichkeit, eine Momentaufnahme für ungültig zu erklären. *force_invalidate_snapshot* ist ein **Bit** und hat den Standardwert **0**.  
  
 der Wert **0** gibt an, dass Änderungen am Mergeartikel nicht bewirken, dass die Momentaufnahme ungültig wird.  
  
 **1** bedeutet, dass Änderungen am Mergeartikel bewirken können, dass die Momentaufnahme ungültig wird. Wenn dies der Fall ist, wird mit dem Wert **1** die Berechtigung zum Eintreten der neuen Momentaufnahme erteilt.  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` Aktiviert oder deaktiviert die Möglichkeit, ein Abonnement als ungültig zu markieren. *force_reinit_subscription* ist ein **Bit** und hat den Standardwert **0**.  
  
 der Wert **0** gibt an, dass Änderungen am Mergeartikelfilter nicht bewirken, dass die Abonnements ungültig werden.  
  
 der Wert **1** bedeutet, dass die Abonnements durch Änderungen am Mergeartikelfilter ungültig werden.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_dropmergefilter** wird bei der Mergereplikation verwendet.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_dropmergefilter** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ändern von Veröffentlichungs- und Artikeleigenschaften](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md)   
 [sp_changemergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergefilter-transact-sql.md)   
 [sp_helpmergefilter &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-helpmergefilter-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
