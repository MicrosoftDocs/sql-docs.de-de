---
description: sys.pdw_loader_backup_runs (Transact-SQL)
title: sys.pdw_loader_backup_runs (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 2b72034c-6a11-46b9-a76c-7a88b2bea360
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: e74c866747b7e8f9c784f43e60ab7fb6ce4cc673
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472911"
---
# <a name="syspdw_loader_backup_runs-transact-sql"></a>sys.pdw_loader_backup_runs (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Enthält Informationen zu laufenden und abgeschlossenen Sicherungs-und Wiederherstellungs Vorgängen in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] sowie zu laufenden und abgeschlossenen Sicherungs-, Wiederherstellungs-und Ladevorgängen in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] . Die Informationen persistieren über Systemneustarts.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|Range|  
|-----------------|---------------|-----------------|-----------|  
|run_id|**int**|Eindeutiger Bezeichner für eine bestimmte Sicherung, Wiederherstellung oder Auslastungs Testlauf.<br /><br /> Der Schlüssel für diese Ansicht.||  
|name|**nvarchar(255)**|Null zum Laden. Der optionale Name für die Sicherung oder Wiederherstellung.||  
|submit_time|**datetime**|Uhrzeit, zu der die Anforderung übermittelt wurde.||  
|start_time|**datetime**|Zeit, zu der der Vorgang gestartet wurde.||  
|end_time|**datetime**|Uhrzeit, zu der der Vorgang abgeschlossen wurde, fehlgeschlagen ist oder abgebrochen wurde||  
|total_elapsed_time|**int**|Die gesamte verstrichene Zeit zwischen start_time und der aktuellen Zeit oder zwischen start_time und end_time für abgeschlossene, abgebrochene oder fehlgeschlagene Ausführungen.|Wenn total_elapsed_time den maximalen Wert für eine ganze Zahl (24,8 Tage in Millisekunden) überschreitet, führt dies zu einem Materialisierungs Fehler aufgrund eines Überlaufs.<br /><br /> Der maximale Wert in Millisekunden entspricht 24,8 Tagen.|  
|operation_type|**nvarchar (16)**|Der Auslastungstyp.|"Backup", "Load", "Restore"|  
|Modus|**nvarchar (16)**|Der Modus innerhalb des Testlaufs.|Für operation_type = **Sicherung**<br />**DIFFERENTIAL**<br />**FULL**<br /><br /> Für operation_type = **Laden**<br />**Anfügen**<br />**Brauchte**<br />**UPSERT**<br /><br /> Für operation_type = **Restore**<br />**DATABASE**<br />**HEADER_ONLY**|  
|database_name|**nvarchar(255)**|Der Name der Datenbank, die den Kontext dieses Vorgangs ist.||  
|table_name|**nvarchar(255)**|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]||  
|Principal_id|**int**|ID des Benutzers, der den Vorgang anfordert.||  
|session_id|**nvarchar(32)**|Die ID der Sitzung, die den Vorgang ausführt.|Weitere Informationen finden Sie unter session_id in [sys.dm_pdw_exec_sessions &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).|  
|request_id|**nvarchar(32)**|Die ID der Anforderung, die den Vorgang ausführt. Bei Lasten ist dies die aktuelle oder letzte Anforderung, die dieser Last zugeordnet ist.|Weitere Informationen finden Sie unter request_id in [sys.dm_pdw_exec_requests &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|status|**nvarchar (16)**|Der Status des Testlaufs.|"abgebrochen", "abgeschlossen", "fehlerhaft", "in Warteschlange", "wird ausgeführt"|  
|Fortschritt|**int**|Prozentsatz abgeschlossen.|0 bis 100|  
|command|**nvarchar(4000)**|Der vollständige Text des Befehls, der vom Benutzer gesendet wurde.|Wird abgeschnitten, wenn mehr als 4000 Zeichen (zählungs Zeichen) enthalten sind.|  
|rows_processed|**bigint**|Anzahl der Zeilen, die im Rahmen dieses Vorgangs verarbeitet werden.||  
|rows_rejected|**bigint**|Anzahl der Zeilen, die im Rahmen dieses Vorgangs abgelehnt wurden.||  
|rows_inserted|**bigint**|Anzahl der Zeilen, die im Rahmen dieses Vorgangs in die Datenbanktabelle (n) eingefügt werden.||  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten von Azure Synapse Analytics und Parallel Data Warehouse Catalog](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
