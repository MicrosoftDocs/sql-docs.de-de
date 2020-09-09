---
description: sp_helplogreader_agent (Transact-SQL)
title: sp_helplogreader_agent (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_helplogreader_agent
- sp_helplogreader_agent_TSQL
helpviewer_keywords:
- sp_helplogreader_agent
ms.assetid: ff837209-e2b3-481a-a48f-8530bfe53d97
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 888a6f8266d53e0297ffe3fe42dbfba11a4929f9
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89541741"
---
# <a name="sp_helplogreader_agent-transact-sql"></a>sp_helplogreader_agent (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt Eigenschaften des Auftrags des Protokolllese-Agents für die Veröffentlichungsdatenbank zurück. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_helplogreader_agent [ [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publisher = ] 'publisher'` Der Name des Verlegers. *Publisher* ist vom **Datentyp vom Datentyp sysname**und hat den Standardwert NULL.  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Die ID des Agents.|  
|**name**|**nvarchar (100)**|Der Name des Agents.|  
|**publisher_security_mode**|**smallint**|Sicherheitsmodus, der vom Agent für die Verbindung mit dem Verleger verwendet wird. Folgende Werte sind möglich:<br /><br /> **0** -  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentifizierung<br /><br /> **1** = Windows-Authentifizierung.|  
|**publisher_login**|**sysname**|Anmeldename, der für die Verbindung mit dem Verleger verwendet wird.|  
|**publisher_password**|**nvarchar (524)**|Aus Sicherheitsgründen wird immer der Wert **\*\*\*\*\*\*\*\*\*\*** zurückgegeben.|  
|**job_id**|**uniqueidentifier**|Eindeutige ID des Agentauftrags.|  
|**job_login**|**nvarchar(512)**|Das Windows-Konto, unter dem die Protokolllese-Agent ausgeführt wird, das im Format *Domäne* \\ *Benutzername*zurückgegeben wird.|  
|**job_password**|**sysname**|Aus Sicherheitsgründen wird immer der Wert **\*\*\*\*\*\*\*\*\*\*** zurückgegeben.|  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 **sp_helplogreader_agent** wird bei der Transaktions Replikation verwendet.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** auf dem Verleger oder Mitglieder der festen Daten Bank Rolle **db_owner** in der Veröffentlichungs Datenbank können **sp_helplogreader_agent**ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Anzeigen und Ändern von Replikationssicherheitseinstellungen](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [sp_addlogreader_agent &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)   
 [sp_changelogreader_agent &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-changelogreader-agent-transact-sql.md)  
  
  
