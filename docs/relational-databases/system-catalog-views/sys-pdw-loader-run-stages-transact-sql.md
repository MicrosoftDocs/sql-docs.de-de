---
description: sys.pdw_loader_run_stages (Transact-SQL)
title: sys.pdw_loader_run_stages (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 255681e9-323c-42c0-a63c-1f05536efdd5
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 288810d6143bcdb98e7bf3257f5958b2cf5f91ee
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92036733"
---
# <a name="syspdw_loader_run_stages-transact-sql"></a>sys.pdw_loader_run_stages (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Enthält Informationen zu laufenden und abgeschlossenen Ladevorgängen in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] . Die Informationen persistieren über Systemneustarts.  
  
| Spaltenname | Datentyp | BESCHREIBUNG | Range |
| ----------- | --------- | ----------- | ----- |
|run_id|**int**|Eindeutiger Bezeichner einer loadertestlauf.||  
|Stufe|**nvarchar(30)**|Die aktuelle Phase für den Testlauf.|' CREATE_STAGING ', ' DMS_LOAD ', ' LOAD_INSERT ', ' LOAD_CLEANUP '|  
|request_id|**nvarchar(32)**|ID der Anforderung, die diese Phase ausgeführt hat.||  
|status|**nvarchar (16)**|Status dieser Phase.||  
|start_time|**datetime**|Der Zeitpunkt, zu dem die Phase gestartet wurde.||  
|end_time|**datetime**|Der Zeitpunkt, zu dem die Phase beendet wurde (sofern vorhanden).|NULL, wenn nicht gestartet oder in Bearbeitung.|  
|total_elapsed_time|**int**|Gesamte Zeit, die diese Phase ausgeführt hat (oder bisher aufgewendet).|Wenn total_elapsed_time den maximalen Wert für eine ganze Zahl (24,8 Tage in Millisekunden) überschreitet, führt dies zu einem Materialisierungs Fehler aufgrund eines Überlaufs.<br /><br /> Der maximale Wert in Millisekunden entspricht 24,8 Tagen.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten von Azure Synapse Analytics und Parallel Data Warehouse Catalog](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
