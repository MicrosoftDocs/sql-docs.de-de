---
description: MStracer_tokens (Transact-SQL)
title: MStracer_tokens (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MStracer_tokens_TSQL
- MStracer_tokens
dev_langs:
- TSQL
helpviewer_keywords:
- MStracer_tokens system table
ms.assetid: b273aa48-8188-4213-8e2c-311543c3236f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2bbd85ea937aee3445d216c35cefe3f61fb494fb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211554"
---
# <a name="mstracer_tokens-transact-sql"></a>MStracer_tokens (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MStracer_tokens** Tabelle verwaltet einen Datensatz von Überwachungs Token-Datensätzen, die in eine Veröffentlichung eingefügt werden. Diese Tabelle wird in der Verteilungsdatenbank gespeichert. Sie wird von der Replikation für die Leistungsüberwachung verwendet.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**tracer_id**|**int**|Identifiziert einen Überwachungstoken-Datensatz.|  
|**publication_id**|**int**|Identifiziert die Veröffentlichung, in die der Überwachungstoken-Datensatz eingefügt wurde.|  
|**publisher_commit**|**datetime**|Das Datum und die Uhrzeit, zu dem ein Commit für den Überwachungstoken-Datensatz auf dem Verleger ausgeführt wurde.|  
|**distributor_commit**|**datetime**|Das Datum und die Uhrzeit, zu dem ein Commit für den Überwachungstoken-Datensatz auf dem Verteiler ausgeführt wurde.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
