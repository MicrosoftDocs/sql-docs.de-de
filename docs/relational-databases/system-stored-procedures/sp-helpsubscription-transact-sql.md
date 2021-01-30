---
description: sp_helpsubscription (Transact-SQL)
title: sp_helpsubscription (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpsubscription_TSQL
- sp_helpsubscription
helpviewer_keywords:
- sp_helpsubscription
ms.assetid: ff96bcbf-e2b9-4da8-8515-d80d4ce86c16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 73fc43bf20a9306d0224392d984b3f2bff74ec6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192899"
---
# <a name="sp_helpsubscription-transact-sql"></a>sp_helpsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Listet Abonnementinformationen bezüglich einer bestimmten Veröffentlichung, eines Artikels, eines Abonnenten oder einer Gruppe von Abonnements auf. Diese gespeicherte Prozedur wird auf einem Verleger für die Veröffentlichungsdatenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_helpsubscription [ [ @publication = ] 'publication' ]   
    [ , [ @article = ] 'article' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @destination_db = ] 'destination_db' ]   
    [ , [ @found=] found OUTPUT ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publication = ] 'publication'` Der Name der zugeordneten Veröffentlichung. *Publication* ist vom **Datentyp vom Datentyp sysname** und **%** hat den Standardwert, mit dem alle Abonnement Informationen für diesen Server zurückgegeben werden.  
  
`[ @article = ] 'article'` Der Name des Artikels. der *Artikel* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert **%** , mit dem alle Abonnement Informationen für die ausgewählten Veröffentlichungen und Abonnenten zurückgegeben werden. Wenn **dies der Wert ist,** wird nur ein Eintrag für das vollständige Abonnement für eine Veröffentlichung zurückgegeben.  
  
`[ @subscriber = ] 'subscriber'` Der Name des Abonnenten, auf dem Abonnement Informationen abgerufen werden sollen. *Subscriber* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert **%** , mit dem alle Abonnement Informationen für die ausgewählten Veröffentlichungen und Artikel zurückgegeben werden.  
  
`[ @destination_db = ] 'destination_db'` Der Name der Zieldatenbank. *destination_db* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert **%** .  
  
`[ @found = ] 'found'OUTPUT` Ein Flag, das die Rückgabe von Zeilen angibt. " *found*" ist vom Datentyp **int** und ein Output-Parameter. der Standardwert ist 23456.  
  
 **1** gibt an, dass die Veröffentlichung gefunden wurde.  
  
 **0** gibt an, dass die Veröffentlichung nicht gefunden wurde.  
  
`[ @publisher = ] 'publisher'` Der Name des Verlegers. *Publisher* ist vom Datentyp **vom Datentyp sysname**. der Standardwert ist der Name des aktuellen Servers.  
  
> [!NOTE]  
>  der *Verleger* darf nicht angegeben werden, es sei denn, es handelt sich um einen Oracle-Verleger.  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**Abonnenten**|**sysname**|Der Name des Abonnenten.|  
|**ung**|**sysname**|Name der Veröffentlichung.|  
|**Artikel**|**sysname**|Der Name des Artikels.|  
|**Zieldatenbank**|**sysname**|Name der Zieldatenbank, in der replizierte Daten gespeichert werden.|  
|**Abonnementstatus**|**tinyint**|Abonnementstatus:<br /><br /> **0** = inaktiv<br /><br /> **1** = abonniert<br /><br /> **2** = aktiv|  
|**Synchronisierungstyp**|**tinyint**|Synchronisierungsart des Abonnements:<br /><br /> **1** = automatisch<br /><br /> **2** = keine|  
|**Abonnementtyp**|**int**|Typ des Abonnements:<br /><br /> **0** = Push<br /><br /> **1** = Pull<br /><br /> **2** = anonym|  
|**full subscription**|**bit**|Gibt an, ob alle Artikel in der Veröffentlichung abonniert werden:<br /><br /> **0** = Nein<br /><br /> **1** = Ja|  
|**Abonnement Name**|**nvarchar(255)**|Name des Abonnements.|  
|**Aktualisierungs Modus**|**int**|**0** = schreibgeschützt<br /><br /> **1** = Abonnement mit sofortigem Update|  
|**distribution job id**|**Binary (16)**|Auftrags-ID des Verteilungs-Agents.|  
|**loopback_detection**|**bit**|Bestimmt, ob der Verteilungs-Agent Transaktionen des Abonnenten zurück an den Abonnenten sendet:<br /><br /> **0** = sendet zurück.<br /><br /> **1** = sendet nicht zurück.<br /><br /> Wird bei der bidirektionalen Transaktionsreplikation verwendet. Weitere Informationen finden Sie unter [Bidirectional Transactional Replication](../../relational-databases/replication/transactional/bidirectional-transactional-replication.md).|  
|**offload_enabled**|**bit**|Gibt an, ob festgelegt wurde, dass die Ausführung eines ausgelagerten Replikations-Agents auf dem Abonnenten ausgeführt wird.<br /><br /> Wenn der Wert **0** ist, wird der-Agent auf dem Verleger ausgeführt.<br /><br /> Wenn der Wert **1** ist, wird der-Agent auf dem Abonnenten ausgeführt.|  
|**offload_server**|**sysname**|Name des Servers, der für die Aktivierung des Remote-Agents aktiviert ist. Wenn der Wert NULL ist, wird der aktuelle offload_server in [MSdistribution_agents](../../relational-databases/system-tables/msdistribution-agents-transact-sql.md) Tabelle aufgeführt.|  
|**dts_package_name**|**sysname**|Gibt den Namen des DTS-Pakets (Data Transformation Services) an.|  
|**dts_package_location**|**int**|Speicherort des DTS-Pakets, wenn dem Abonnement eines zugewiesen wurde. Wenn ein Paket vorhanden ist, gibt der Wert **0** den Speicherort des Pakets auf dem **Verteiler** an. Der Wert **1** gibt den **Abonnenten** an.|  
|**subscriber_security_mode**|**smallint**|Der Sicherheitsmodus auf dem Abonnenten, wobei **1** die Windows-Authentifizierung und **0** die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentifizierung bedeutet.|  
|**subscriber_login**|**sysname**|Der Anmeldename auf dem Abonnenten.|  
|**subscriber_password**||Das tatsächliche Abonnentenkennwort wird nie zurückgegeben. Das Ergebnis wird durch eine "**&#42;&#42;&#42;&#42;&#42;&#42;**"-Zeichenfolge maskiert.|  
|**job_login**|**sysname**|Name des Windows-Kontos, unter dem der Verteilungs-Agent ausgeführt wird.|  
|**job_password**||Das tatsächliche Auftragskennwort wird nie zurückgegeben. Das Ergebnis wird durch eine "**&#42;&#42;&#42;&#42;&#42;&#42;**"-Zeichenfolge maskiert.|  
|**distrib_agent_name**|**nvarchar (100)**|Name des Agentauftrags, der das Abonnement synchronisiert.|  
|**subscriber_type**|**tinyint**|Typ des Abonnenten. Folgende Werte sind möglich:<br /><br /> **0** = SQL Server Abonnent<br /><br /> **1** = ODBC-Datenquellen Server<br /><br /> **2** = Microsoft Jet-Datenbank (veraltet)<br /><br /> **3** = OLE DB Anbieter|  
|**subscriber_provider**|**sysname**|Eindeutiger Programmbezeichner (PROGID, Programmatic Identifier), mit dem der OLE DB-Anbieter für die Nicht-SQL Server-Datenquelle registriert wird.|  
|**subscriber_datasource**|**nvarchar(4000)**|Name der Datenquelle im vom OLE DB-Anbieter unterstützten Format.|  
|**subscriber_providerstring**|**nvarchar(4000)**|Für den OLE DB-Anbieter spezifische Verbindungszeichenfolge, die die Datenquelle identifiziert.|  
|**subscriber_location**|**nvarchar(4000)**|Speicherort der Datenbank im vom OLE DB-Anbieter unterstützten Format.|  
|**subscriber_catalog**|**sysname**|Der Katalog, der beim Herstellen einer Verbindung mit dem OLE DB-Anbieter verwendet werden soll.|  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_helpsubscription** wird bei der Momentaufnahme-und Transaktions Replikation verwendet.  
  
## <a name="permissions"></a>Berechtigungen  
 Ausführungs Berechtigungen werden standardmäßig der **Public** -Rolle erteilt. Benutzern werden nur Informationen für Abonnements zurückgegeben, die sie erstellt haben. Informationen zu allen Abonnements werden an Mitglieder der festen Server Rolle **sysadmin** auf dem Verleger oder an die Mitglieder der festen Daten Bank Rolle **db_owner** in der Veröffentlichungs Datenbank zurückgegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_addsubscription &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubstatus &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-changesubstatus-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
