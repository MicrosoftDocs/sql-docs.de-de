---
title: sp_dropsubscription (Transact-SQL) | Microsoft-Dokumentation
description: Löscht Abonnements für einen Artikel, eine Veröffentlichung oder Abonnements auf dem Verleger. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropsubscription
- sp_dropsubscription_TSQL
helpviewer_keywords:
- sp_dropsubscription
ms.assetid: 7551f345-5510-4684-ab53-f9057249d13a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3097c61204e0a9ae310af87fb83901a252c50e67
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157952"
---
# <a name="sp_dropsubscription-transact-sql"></a>sp_dropsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Löscht Abonnements für bestimmte Artikel, Veröffentlichungen oder Abonnementgruppen auf dem Verleger. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_dropsubscription [ [ @publication= ] 'publication' ]  
    [ , [ @article= ] 'article' ]  
        , [ @subscriber= ] 'subscriber'  
    [ , [ @destination_db= ] 'destination_db' ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @reserved= ] 'reserved' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publication = ] 'publication'` Der Name der zugeordneten Veröffentlichung. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Wenn **alle**, werden alle Abonnements für alle Veröffentlichungen für den angegebenen Abonnenten abgebrochen. die *Veröffentlichung* ist ein erforderlicher Parameter.  
  
`[ @article = ] 'article'` Der Name des Artikels. der *Artikel* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Wenn **alle**, werden Abonnements für alle Artikel für jede angegebene Veröffentlichung und jeden Abonnenten gelöscht. Verwenden Sie **all** für Veröffentlichungen, die sofortiges Aktualisieren ermöglichen.  
  
`[ @subscriber = ] 'subscriber'` Der Name des Abonnenten, dessen Abonnements gelöscht werden. *Subscriber* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert. Wenn **alle**, werden alle Abonnements für alle Abonnenten gelöscht.  
  
`[ @destination_db = ] 'destination_db'` Der Name der Zieldatenbank. *destination_db* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Bei einem Wert von NULL werden alle Abonnements dieses Abonnenten gelöscht.  
  
`[ @ignore_distributor = ] ignore_distributor`  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @reserved = ] 'reserved'`  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_dropsubscription** wird bei der Momentaufnahme-und Transaktions Replikation verwendet.  
  
 Wenn Sie das Abonnement für den Artikel einer immediate-sync-Veröffentlichung löschen, können Sie das Abonnement nur wieder hinzufügen, wenn Sie die Abonnements für alle Artikel der Veröffentlichung löschen und sie alle in einem Schritt wieder hinzufügen.  
  
## <a name="example"></a>Beispiel  
 [!code-sql[HowTo#sp_droptransubscription](../../relational-databases/replication/codesnippet/tsql/sp-dropsubscription-tran_1.sql)]  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** , der festen Daten Bank Rolle **db_owner** oder der Benutzer, der das Abonnement erstellt hat, können **sp_dropsubscription** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Löschen eines Pushabonnements](../../relational-databases/replication/delete-a-push-subscription.md)   
 [sp_addsubscription &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubstatus &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-changesubstatus-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
