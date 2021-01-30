---
description: sp_addpullsubscription (Transact-SQL)
title: sp_addpullsubscription (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpullsubscription
- sp_addpullsubscription_TSQL
helpviewer_keywords:
- sp_addpullsubscription
ms.assetid: 0f4bbedc-0c1c-414a-b82a-6fd47f0a6a7f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c119173ba184ef8fe3a03c59e90965650bb9c35c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206678"
---
# <a name="sp_addpullsubscription-transact-sql"></a>sp_addpullsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Fügt ein Pullabonnement einer Momentaufnahmen- oder Transaktionsveröffentlichung hinzu. Diese gespeicherte Prozedur wird auf dem Abonnenten für die Datenbank ausgeführt, für die das Pullabonnement erstellt werden soll.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_addpullsubscription [ @publisher= ] 'publisher'  
    [ , [ @publisher_db= ] 'publisher_db' ]  
        , [ @publication= ] 'publication'  
    [ , [ @independent_agent= ] 'independent_agent' ]  
    [ , [ @subscription_type= ] 'subscription_type' ]  
    [ , [ @description= ] 'description' ]  
    [ , [ @update_mode= ] 'update_mode' ]  
    [ , [ @immediate_sync = ] immediate_sync ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publisher = ] 'publisher'` Der Name des Verlegers. *Publisher* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  

> [!NOTE]
> Der Server Name kann als angegeben werden `<Hostname>,<PortNumber>` . Möglicherweise müssen Sie die Portnummer für die Verbindung angeben, wenn SQL Server unter Linux oder Windows mit einem benutzerdefinierten Port bereitgestellt wird und der-Browser Dienst deaktiviert ist.
  
`[ @publisher_db = ] 'publisher_db'` Der Name der Verleger Datenbank. *publisher_db* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. *publisher_db* wird von Oracle-Verlegern ignoriert.  
  
`[ @publication = ] 'publication'` Der Name der Veröffentlichung. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @independent_agent = ] 'independent_agent'` Gibt an, ob eine eigenständige Verteilungs-Agent für diese Veröffentlichung vorhanden ist. *independent_agent* ist vom Datentyp **nvarchar (5)** und hat den Standardwert true. **True** gibt an, dass eine eigenständige Verteilungs-Agent für diese Veröffentlichung vorhanden ist. Wenn der Wert **false** ist, gibt es für jedes Paar aus Verleger Datenbank und Abonnenten Datenbank eine Verteilungs-Agent. *independent_agent* ist eine Eigenschaft der Veröffentlichung und muss hier denselben Wert wie auf dem Verleger haben.  
  
`[ @subscription_type = ] 'subscription_type'` Der Abonnementtyp. *subscription_type* ist vom Datentyp **nvarchar (9)** und hat den Standardwert **Anonymous**. Sie müssen den Wert **Pull** für *subscription_type* angeben, es sei denn, Sie möchten ein Abonnement erstellen, ohne das Abonnement auf dem Verleger zu registrieren. In diesem Fall müssen Sie den Wert **Anonym** angeben. Dies ist notwendig für Fälle, in denen Sie während der Abonnementkonfiguration keine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verbindung mit dem Verleger herstellen können.  
  
`[ @description = ] 'description'` Die Beschreibung der Veröffentlichung. die *Beschreibung* ist vom Datentyp **nvarchar (100)** und hat den Standardwert NULL.  
  
`[ @update_mode = ] 'update_mode'` Der Typ des Updates. *update_mode* ist vom Datentyp **nvarchar (30)**. die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|Schreib **geschützt (Standard** )|Das Abonnement ist schreibgeschützt. Änderungen am Abonnenten werden nicht an den Verleger zurückgesendet. Sollte verwendet werden, wenn Updates nicht auf dem Abonnenten vorgenommen werden.|  
|**synctran**|Aktiviert die Unterstützung für das sofortige Aktualisieren von Abonnements.|  
|**queued tran**|Aktiviert das verzögerte Aktualisieren über eine Warteschlange für das Abonnement. Daten können auf dem Abonnenten geändert werden; die Änderungen werden in einer Warteschlange gespeichert und an den Verleger weitergegeben.|  
|**Failover**|Aktiviert das sofortige Aktualisieren für das Abonnement, wobei als Failover das verzögerte Aktualisieren über eine Warteschlange verwendet wird. Daten können auf dem Abonnenten geändert werden; die Änderungen werden sofort an den Verleger weitergegeben. Wenn der Verleger und der Abonnent nicht verbunden sind, können Datenänderungen auf dem Abonnenten in einer Warteschlange gespeichert werden, bis Abonnent und Verleger erneut verbunden sind.|  
|**queued failover**|Ermöglicht das Abonnement als Update über eine Warteschlange, mit der Möglichkeit des Wechsels zum sofortigen Updatemodus. Daten können auf dem Abonnenten geändert und in einer Warteschlange gespeichert werden, bis eine Verbindung zwischen dem Abonnenten und dem Verleger hergestellt wird. Wenn eine kontinuierliche Verbindung hergestellt wird, kann der Updatemodus in den sofortigen Updatemodus geändert werden. *Wird für Oracle-Verleger nicht unterstützt*.|  
  
`[ @immediate_sync = ] immediate_sync` Gibt an, ob die Synchronisierungs Dateien bei jeder Ausführung des Momentaufnahmen-Agent erstellt oder neu erstellt werden. *immediate_sync* ist vom Typ **Bit** mit dem Standardwert 1 und muss auf denselben Wert festgelegt werden wie *immediate_sync* in **sp_addpublication**. *immediate_sync* ist eine Eigenschaft der Veröffentlichung und muss hier denselben Wert wie auf dem Verleger haben.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_addpullsubscription** wird bei der Momentaufnahme-und Transaktions Replikation verwendet.  
  
> [!IMPORTANT]  
>  Bei Abonnements mit verzögertem Update über eine Warteschlange verwenden Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung für Verbindungen mit Abonnenten. Geben Sie für die Verbindung zu den einzelnen Abonnenten jeweils ein anderes Konto an. Beim Erstellen eines Pullabonnements, das verzögerte Updates über eine Warteschlange unterstützt, legt die Replikation immer die Verwendung der Windows-Authentifizierung für die Verbindung fest (für Pullabonnements kann die Replikation nicht auf Metadaten beim Abonnenten zugreifen, die für die Verwendung der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung erforderlich sind). In diesem Fall sollten Sie [sp_changesubscription](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md) ausführen, um die Verbindung [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nach dem Konfigurieren des Abonnements für die Verwendung der Authentifizierung zu ändern.  
  
 Wenn die [MSreplication_subscriptions &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/msreplication-subscriptions-transact-sql.md) Tabelle auf dem Abonnenten nicht vorhanden ist, wird Sie von **sp_addpullsubscription** erstellt. Außerdem wird der [MSreplication_subscriptions &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/msreplication-subscriptions-transact-sql.md) -Tabelle eine Zeile hinzugefügt. Bei Pullabonnements sollten [sp_addsubscription &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) zuerst auf dem Verleger aufgerufen werden.  
  
## <a name="example"></a>Beispiel  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addpullsubscription-t_1.sql)]  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_addpullsubscription** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Erstellen eines aktualisierbaren Abonnements für eine Transaktions Veröffentlichung](../../relational-databases/replication/publish/create-an-updatable-subscription-to-a-transactional-publication.md) [Abonnieren von Veröffentlichungen](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addpullsubscription_agent &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)   
 [sp_change_subscription_properties &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md)   
 [sp_droppullsubscription &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
