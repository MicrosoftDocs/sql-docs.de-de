---
description: MSrepl_originators (Transact-SQL)
title: MSrepl_originators (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSrepl_originators_TSQL
- MSrepl_originators
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_originators system table
ms.assetid: a3ac20a6-73f6-4fdc-ad5f-5f72746c9871
author: cawrites
ms.author: chadam
ms.openlocfilehash: b9c5a5690f6349975a2edafd7715d81c769ce31c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210703"
---
# <a name="msrepl_originators-transact-sql"></a>MSrepl_originators (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSrepl_originators** Tabelle enthält eine Zeile für jeden aktualisierbaren Abonnenten, von dem die Transaktion stammt. Diese Tabelle wird in der Verteilungsdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identifiziert den Updateabonnenten.|  
|**publisher_database_id**|**int**|Identifiziert die Veröffentlichungsdatenbank.|  
|**srvname**|**sysname**|Der Name des aktualisierenden Servers.|  
|**dbname**|**sysname**|Der Name der aktualisierenden Datenbank.|  
|**publication_id**|**int**|Identifiziert die Veröffentlichung.|  
|**Eintrag dbversion**|**int**|Identifiziert die Datenbankversion.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
