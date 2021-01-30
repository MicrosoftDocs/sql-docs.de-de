---
description: sp_refresh_log_shipping_monitor (Transact-SQL)
title: sp_refresh_log_shipping_monitor (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_refresh_log_shipping_monitor
- sp_refresh_log_shipping_monitor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_refresh_log_shipping_monitor
ms.assetid: edefb912-31c5-4d99-9aba-06629afd0171
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 16c1fb1de57fb0e1c61410071ec8fde31951f2ab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185718"
---
# <a name="sp_refresh_log_shipping_monitor-transact-sql"></a>sp_refresh_log_shipping_monitor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Diese gespeicherte Prozedur aktualisiert die Remoteüberwachungstabellen mit den neuesten Informationen von einem angegebenen primären oder sekundären Server für den angegebenen Protokollversand-Agent. Die Prozedur wird auf dem primären oder sekundären Server aufgerufen.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_refresh_log_shipping_monitor  
[ @agent_id = ] 'agent_id',  
[ @agent_type = ] 'agent_type'  
[ @database = ] 'database'  
[ @mode ] n  
```  
  
## <a name="arguments"></a>Argumente  
`[ @agent_id = ] 'agent_id'` Die primäre ID für die Sicherung oder die sekundäre ID für kopieren oder wiederherstellen. *agent_id* ist vom Datentyp **uniqueidentifier** und darf nicht NULL sein.  
  
`[ @agent_type = ] 'agent_type'` Der Typ des Protokoll Versand Auftrags.  
  
 0 = Sicherungsauftrag  
  
 1 = Kopierauftrag  
  
 2 = Wiederherstellungsauftrag  
  
 *agent_type* ist vom Datentyp **tinyint** und kann nicht NULL sein.  
  
`[ @database = ] 'database'` Die primäre oder sekundäre Datenbank, die von Sicherungs-oder Wiederherstellungs-Agents protokolliert wird.  
  
`[ @mode ] n` Gibt an, ob die Überwachungsdaten aktualisiert oder bereinigt werden sollen. Der Datentyp von *m* ist tinyint, und die folgenden Werte werden unterstützt:  
  
 1 = aktualisieren (Dies ist der Standardwert.)  
  
 2 = Löschen  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="result-sets"></a>Resultsets  
 Keine.  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_refresh_log_shipping_monitor** aktualisiert die Tabellen " **log_shipping_monitor_primary**", " **log_shipping_monitor_secondary**", " **log_shipping_monitor_history_detail**" und " **log_shipping_monitor_error_detail** " mit Sitzungsinformationen, die noch nicht übertragen wurden. Dies ermöglicht das Synchronisieren des Überwachungsservers mit dem primären oder einem sekundären Server, wenn der Überwachungsserver für einen bestimmten Zeitraum nicht mehr synchronisiert wurde. Zudem können Sie die Überwachungsinformationen auf dem Überwachungsserver bei Bedarf leeren.  
  
 **sp_refresh_log_shipping_monitor** muss von der **Master** -Datenbank auf dem primären oder sekundären Server ausgeführt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Serverrolle **sysadmin** können diese Prozedur ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Informationen zum Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
