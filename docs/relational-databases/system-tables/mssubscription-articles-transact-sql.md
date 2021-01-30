---
description: MSsubscription_articles (Transact-SQL)
title: MSsubscription_articles (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSsubscription_articles
- MSsubscription_articles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_articles system table
ms.assetid: dbc1737f-261e-4017-b9cd-703b9fc4ac78
author: cawrites
ms.author: chadam
ms.openlocfilehash: 16d9d91b7c3948f18d90b235c4f853d1220c5380
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211593"
---
# <a name="mssubscription_articles-transact-sql"></a>MSsubscription_articles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSsubscription_articles** Tabelle enthält Informationen zu den Artikeln in einem in der Warteschlange befindlichen Abonnement. Diese Tabelle wird nur für die folgenden Replikationstypen aufgefüllt: Verzögertes Update über eine Warteschlange und sofortiges Update mit verzögertem Update über eine Warteschlange als Failover.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|Die ID der Momentaufnahme, die diesen Artikel bedient.|  
|**artid**|**int**|Die Artikel-ID aus der **sysarticles** -Tabelle.|  
|**Artikel**|**sysname**|Der Name des Artikels in der **sysarticles** -Tabelle.|  
|**dest_table**|**sysname**|Der Name der Ziel Tabelle aus der **sysarticles** -Tabelle.|  
|**owner**|**sysname**|Der Besitzer des Abonnements.|  
|**cft_table**|**sysname**|Der Name der Konflikttabelle dieses Artikels für den Replikationstyp Verzögertes Update über eine Warteschlange.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
