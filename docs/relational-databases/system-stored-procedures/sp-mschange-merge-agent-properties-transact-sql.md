---
description: sp_MSchange_merge_agent_properties (Transact-SQL)
title: sp_MSchange_merge_agent_properties (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_MSchange_merge_agent_properties_TSQL
- sp_MSchange_merge_agent_properties
helpviewer_keywords:
- sp_MSchange_merge_agent_properties
ms.assetid: f775fa0f-28c7-4863-89ce-7bcfa1ab8b5e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 89622d10a242ef2bc7fe97a4a0a75457d8dbe412
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195435"
---
# <a name="sp_mschange_merge_agent_properties-transact-sql"></a>sp_MSchange_merge_agent_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ändert die Eigenschaften eines Merge-Agent Auftrags, der auf einem Verteiler mit oder einer höheren Version ausgeführt wird [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Diese gespeicherte Prozedur wird zum Ändern von Eigenschaften verwendet, wenn der Verleger in einer Instanz von [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] ausgeführt wird. Diese gespeicherte Prozedur wird auf dem Verteiler für die Verteilungsdatenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_MSchange_merge_agent_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @subscriber = ] 'subscriber'   
        , [ @subscriber_db = ] 'subscriber_db'   
        , [ @property = ] 'property'   
        , [ @value = ] 'value' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publisher = ] 'publisher'` Der Name des Verlegers. *Publisher* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @publisher_db = ] 'publisher_db'` Der Name der Veröffentlichungs Datenbank. *publisher_db* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @publication = ] 'publication'` Der Name der Veröffentlichung. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @subscriber = ] 'subscriber'` Der Name des Abonnenten. *Subscriber* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @subscriber_db = ] 'subscriber_db'` Der Name der Abonnement Datenbank. *subscriber_db* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @property = ] 'property'` Die Veröffentlichungs Eigenschaft, die geändert werden soll. *Property* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @value = ] 'value'` Der neue Eigenschafts Wert. der Wert ist vom Datentyp **nvarchar (524)** und hat den Standard *Wert* NULL.  
  
 Diese Tabelle beschreibt die änderbaren Eigenschaften des Merge-Agent-Auftrags sowie die Einschränkungen für die Werte dieser Eigenschaften.  
  
|Eigenschaft|Wert|BESCHREIBUNG|  
|--------------|-----------|-----------------|  
|**description**||Kurze Beschreibung des Abonnements.|  
|**merge_job_login**||Anmeldename für das [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Konto, unter dem der Agent ausgeführt wird.|  
|**merge_job_password**||Kennwort für das Windows-Konto, unter dem der Agentauftrag ausgeführt wird.|  
|**publisher_login**||Anmeldename, der beim Herstellen der Verbindung mit einem Verleger zum Synchronisieren des Abonnements verwendet werden soll.|  
|**publisher_password**||Herausgeber Kennwort<br /><br /> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]|  
|**publisher_security_mode**|**1**|Windows-Authentifizierung.<br /><br /> [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]|  
||**0**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung.|  
|**subscriber_login**||Anmeldename, der beim Herstellen der Verbindung mit einem Abonnenten zum Synchronisieren des Abonnements verwendet werden soll.|  
|**subscriber_password**||Kennwort des Abonnenten.<br /><br /> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]|  
|**subscriber_security_mode**|**1**|Windows-Authentifizierung.<br /><br /> [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]|  
||**0**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung.|  
  
> [!NOTE]  
>  Nach dem Ändern des Anmeldenamens oder Kennworts eines Agents müssen Sie den Agent beenden und neu starten, damit die Änderungen in Kraft treten.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_MSchange_merge_agent_properties** wird bei der Mergereplikation verwendet.  
  
 Wenn der Verleger auf einer Instanz von oder einer höheren [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Version ausgeführt wird, sollten Sie [sp_changemergesubscription](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md) verwenden, um die Eigenschaften eines Merge-Agent Auftrags zu ändern, der ein Pushabonnement synchronisiert, das auf dem Verteiler ausgeführt wird.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** auf dem Verteiler können **sp_MSchange_merge_agent_properties** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_addmergepushsubscription_agent &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md)   
 [sp_addmergesubscription &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)  
  
  
