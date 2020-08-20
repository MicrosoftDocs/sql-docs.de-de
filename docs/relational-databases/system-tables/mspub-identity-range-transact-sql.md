---
description: MSpub_identity_range (Transact-SQL)
title: MSpub_identity_range (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpub_identity_range_TSQL
- MSpub_identity_range
dev_langs:
- TSQL
helpviewer_keywords:
- MSpub_identity_range system table
ms.assetid: 68746eef-32e1-42bc-aff0-9798cd0e88b8
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: f96e19282118a07fb487a64eff2eb8593f4261c0
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88485474"
---
# <a name="mspub_identity_range-transact-sql"></a>MSpub_identity_range (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSpub_identity_range** Tabelle bietet Unterstützung für die Identitäts Bereichs Verwaltung. Diese Tabelle wird in den Datenbanken publication und subscription gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**objid**|**int**|Die ID der Tabelle, die die durch Replikation verwaltete Identitätsspalte enthält.|  
|**range**|**bigint**|Steuert die Bereichsgröße der aufeinander folgenden Identitätswerte, die im Abonnement bei einer Anpassung zugewiesen würden.|  
|**pub_range**|**bigint**|Steuert die Bereichsgröße der aufeinander folgenden Identitätswerte, die in der Veröffentlichung bei einer Anpassung zugewiesen würden.|  
|**current_pub_range**|**bigint**|Der aktuelle Bereich, der von der Veröffentlichung verwendet wird. Sie kann sich von *pub_range* unterscheiden, wenn Sie nach der Änderung durch **sp_changearticle** und vor der nächsten Bereichs Anpassung angezeigt wird.|  
|**threshold**|**int**|Der Prozentwert, der steuert, wann der Verteilungs-Agent einen neuen Identitätsbereich zuweist. Wenn der in *Schwellenwert* angegebene Prozentsatz der Werte verwendet wird, erstellt der Verteilungs-Agent einen neuen Identitäts Bereich.|  
|**last_seed**|**bigint**|Die untere Grenze des aktuellen Bereichs.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
