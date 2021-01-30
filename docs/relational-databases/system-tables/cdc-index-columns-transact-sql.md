---
description: cdc.index_columns (Transact-SQL)
title: CDC.index_columns (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- cdc.index_columns_TSQL
- cdc.index_columns
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.index_columns
ms.assetid: 256ec8a5-3031-40a8-9fdb-99db42ea453d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0eeac23e94fa1927e5b5efd573fd19fb8935d6f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183303"
---
# <a name="cdcindex_columns-transact-sql"></a>cdc.index_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile für jede Indexspalte zurück, die einer Änderungstabelle zugeordnet ist. Die Indexspalten werden von Change Data Capture verwendet, um die Zeilen in der Quelltabelle eindeutig zu identifizieren. Standardmäßig werden die Spalten des Primärschlüssels der Quelltabelle eingeschlossen. Wenn jedoch in der Quelltabelle ein eindeutiger Index angegeben und Change Data Capture in der Quelltabelle aktiviert ist, werden stattdessen die in diesem Index enthaltenen Spalten verwendet. Bei aktivierter Nachverfolgung von Nettoänderungen ist ein Primärschlüssel oder ein eindeutiger Index erforderlich. Weitere Informationen finden Sie unter [sys.sp_cdc_enable_table &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).  
  
 Es wird empfohlen, die Systemtabellen nicht direkt abzufragen. Führen Sie stattdessen die gespeicherte Prozedur [sys.sp_cdc_help_change_data_capture](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md) aus.  

  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID der Änderungstabelle.|  
|**column_name**|**sysname**|Name der Indexspalte.|  
|**index_ordinal**|**tinyint**|Ordnungszahl (1-basiert) der im Index enthaltenen Spalte.|  
|**column_id**|**int**|ID der Spalte in der Quelltabelle.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [CDC.change_tables &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/cdc-change-tables-transact-sql.md)  
  
  
