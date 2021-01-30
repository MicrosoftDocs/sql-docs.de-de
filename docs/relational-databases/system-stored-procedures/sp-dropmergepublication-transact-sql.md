---
description: sp_dropmergepublication (Transact-SQL)
title: sp_dropmergepublication (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropmergepublication
- sp_dropmergepublication_TSQL
helpviewer_keywords:
- sp_dropmergepublication
ms.assetid: 9e1cb96e-5889-4f97-88cd-f60cf313ce68
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 35024724255eea714de42f8fb3a974188730fcef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208213"
---
# <a name="sp_dropmergepublication-transact-sql"></a>sp_dropmergepublication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Löscht eine Mergeveröffentlichung und den zugehörigen Momentaufnahme-Agent. Vor dem Löschen einer Mergeveröffentlichung müssen alle Abonnements gelöscht werden. Die Artikel in der Veröffentlichung werden automatisch gelöscht. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_dropmergepublication [ @publication= ] 'publication'   
    [ , [ @ignore_distributor = ] ignore_distributor ]   
    [ , [ @reserved = ] reserved ]  
    [ , [ @ignore_merge_metadata = ] ignore_merge_metadata ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publication = ] 'publication'` Der Name der zu löschenden Veröffentlichung. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert. Wenn **alle** vorhanden sind, werden alle vorhandenen Mergeveröffentlichungen und der ihr zugeordnete Momentaufnahmen-Agent Auftrag entfernt. Wenn Sie einen bestimmten Wert für die *Veröffentlichung* angeben, werden nur diese Veröffentlichung und der zugehörige Momentaufnahmen-Agent Auftrag gelöscht.  
  
`[ @ignore_distributor = ] ignore_distributor` Wird verwendet, um eine Veröffentlichung zu löschen, ohne Cleanuptasks auf dem Verteiler auszuführen. *ignore_distributor* ist vom Typ **Bit**. der Standardwert ist **0**. Dieser Parameter wird auch bei der Neuinstallation des Verteilers verwendet.  
  
`[ @reserved = ] reserved` Ist für die zukünftige Verwendung reserviert. *reserved* ist vom Typ **Bit**. der Standardwert ist **0**.  
  
`[ @ignore_merge_metadata = ] ignore_merge_metadata` Nur interne Verwendung.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_dropmergepublication** wird bei der Mergereplikation verwendet.  
  
 **sp_dropmergepublication** löscht alle Artikel, die einer Veröffentlichung zugeordnet sind, rekursiv und löscht dann die Veröffentlichung selbst. Solange für eine Veröffentlichung ein Abonnement vorhanden ist, kann sie nicht gelöscht werden. Weitere Informationen zum Entfernen von Abonnements finden Sie unter [Löschen eines Pushabonnements](../../relational-databases/replication/delete-a-push-subscription.md) und [Löschen eines](../../relational-databases/replication/delete-a-pull-subscription.md)Pullabonnements.  
  
 Durch das Ausführen **sp_dropmergepublication** zum Löschen einer Veröffentlichung werden veröffentlichte Objekte nicht aus der Veröffentlichungs Datenbank oder den entsprechenden Objekten aus der Abonnement Datenbank entfernt. Verwenden \<object> Sie Drop, um diese Objekte bei Bedarf manuell zu entfernen.  
  
## <a name="example"></a>Beispiel  
 [!code-sql[HowTo#sp_dropmergepublication](../../relational-databases/replication/codesnippet/tsql/sp-dropmergepublication-_1.sql)]  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_dropmergepublication** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Löschen einer Veröffentlichung](../../relational-databases/replication/publish/delete-a-publication.md)   
 [sp_addmergepublication &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [Gespeicherte Automatisierungsprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
