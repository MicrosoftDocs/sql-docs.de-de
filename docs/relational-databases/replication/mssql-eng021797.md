---
description: MSSQL_ENG021797
title: MSSQL_ENG021797 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG021797 error
ms.assetid: 54d83a1e-43fd-449c-a2b2-fdda2609a534
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 1c7a625832a9cbe43dfc01969c7a2cce6f434e0f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193973"
---
# <a name="mssql_eng021797"></a>MSSQL_ENG021797
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Meldungsdetails  
  
|attribute|Wert|  
|-|-|  
|Produktname|SQL Server|  
|Ereignis-ID|21797|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Symbolischer Name||  
|Meldungstext|„%s“ muss eine gültige Windows-Anmeldung der folgenden Form sein: "MACHINE Login" oder "DOMAIN Login". Lesen Sie die Dokumentation zu '%3!s!'.|  
  
## <a name="explanation"></a>Erklärung  
 Dieser Fehler wird von folgenden gespeicherten Replikationsprozeduren ausgelöst, wenn der für `@job_login` angegebene Parameter Null oder ungültig ist. Dieser Fehler kann auftreten, wenn ein Mitglied der festen Datenbankrolle **db_owner** Skripts aus vorherigen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ausführt. Das Sicherheitsmodell wurde in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]geändert, daher müssen die Skripts aktualisiert werden.  
  
-   [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)  
  
-   [sp_addqreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md)  
  
-   [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)  
  
-   [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)  
  
-   [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)  
  
-   [sp_addmergepushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md)  
  
-   [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)  
  
 Diese gespeicherten Prozeduren können von einem Mitglied der festen Serverrolle **sysadmin** auf dem entsprechenden Server bzw. einem Mitglied der festen Datenbankrolle **db_owner** in der entsprechenden Datenbank ausgeführt werden. Jede gespeicherte Prozedur erstellt einen Agentauftrag, für den Sie das [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Konto angeben können, unter dem der Agent ausgeführt werden soll. Für Benutzer, die die Rolle **sysadmin** besitzen, werden Agentaufträge implizit erstellt, auch wenn kein Windows-Konto angegeben wird – sollte ein Konto angegeben werden, muss es jedoch gültig sein. Agents werden im Kontext des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Dienstkontos auf dem entsprechenden Server ausgeführt. Das Festlegen eines Kontos ist zwar nicht erforderlich, aus Sicherheitsgründen empfiehlt es sich jedoch, ein separates Konto für jeden Agent anzugeben. Weitere Informationen finden Sie unter [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="user-action"></a>Benutzeraktion  
 Stellen Sie sicher, dass Sie ein gültiges Windows-Konto für den `@job_login`-Parameter der einzelnen Prozeduren angeben. Wenn Sie Replikationsskripts aus vorherigen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]übernehmen, aktualisieren Sie diese Skripts, sodass sie die für [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]erforderlichen gespeicherten Prozeduren und Parameter enthalten. Weitere Informationen finden Sie unter [Aktualisieren von Replikationsskripts &#40;Replikationsprogrammierung mit Transact-SQL&#41;](../../relational-databases/replication/administration/upgrade-replication-scripts-replication-transact-sql-programming.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Fehler- und Ereignisreferenz &#40;Replikation&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
