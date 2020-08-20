---
description: IHpublishercolumns (Transact-SQL)
title: IHpublishercolumns (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHpublishercolumns
- IHpublishercolumns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublishercolumns system table
ms.assetid: a5347750-224c-40d9-ae12-57e7213b7db9
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: b5d3f4b833d6d5183c888dba9a4a3a39ac42598d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88492775"
---
# <a name="ihpublishercolumns-transact-sql"></a>IHpublishercolumns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **IHpublishercolumns** -Systemtabelle stellt Metadaten dar, die auf dem Verleger gespeichert werden. Diese Tabelle enthält eine Zeile für jede Spalte, die mithilfe des aktuellen Verteilers aus Nicht-SQL Server-Verlegern repliziert wurden. Die Datentyp Informationen in **IHpublishercolumns** sind spezifisch für das nicht SQL Server Database Management System (DBMS), aus dem die Daten veröffentlicht werden. Diese Tabelle wird in der Verteilungsdatenbank gespeichert.  
  
## <a name="definition"></a>Definition  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**publishercolumn_id**|**int**|Identifiziert eine veröffentlichte Spalte.|  
|**table_id**|**int**|Identifiziert die Quell Tabelle aus [IHpublishertables](../../relational-databases/system-tables/ihpublishertables-transact-sql.md) , zu der die Spalte gehört.|  
|**publisher_id**|**smallint**|Identifiziert den Nicht-SQL Server-Verleger, von dem die Spalte veröffentlicht wird.|  
|**name**|**sysname**|Der Name der veröffentlichten Spalte.|  
|**column_ordinal**|**int**|Identifiziert die Spalte nach Reihenfolge.|  
|**type**|**varchar (255)**|Der Spaltendatentyp der Quellspalte auf dem Verleger.|  
|**length**|**bigint**|Die Länge der Quellspalte auf dem Verleger.|  
|**prec**|**int**|Die Genauigkeit der Quellspalte auf dem Verleger.|  
|**scale**|**int**|Die Dezimalstellen der Quellspalte auf dem Verleger.|  
|**IsNullable**|**bit**|Gibt an, ob die Spalte NULL-Werte akzeptiert, wobei **1** bedeutet, dass NULL-Werte akzeptiert werden.|  
|**iscaptured**|**bit**|Zeigt an, ob für die Spalte ein Trigger vorhanden ist. Ein Trigger kann auch dann vorhanden sein, wenn die Spalte nicht in einem Artikel veröffentlicht wird. Der Wert **1** bedeutet, dass der-Wert für die Spalte vorhanden ist.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Heterogene Datenbankreplikation](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikations Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sysarticlecolumns &#40;System Ansicht&#41; &#40;Transact-SQL-&#41;](../../relational-databases/system-views/sysarticlecolumns-system-view-transact-sql.md)   
 [sysarticlecolumns &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/sysarticlecolumns-transact-sql.md)  
  
  
