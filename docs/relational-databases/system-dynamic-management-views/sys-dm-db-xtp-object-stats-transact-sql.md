---
description: sys.dm_db_xtp_object_stats (Transact-SQL)
title: sys.dm_db_xtp_object_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_object_stats_TSQL
- sys.dm_db_xtp_object_stats
- dm_db_xtp_object_stats
- sys.dm_db_xtp_object_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_object_stats dynamic management view
ms.assetid: 07300b59-3cab-4d3e-8138-5ea8f584f88f
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f6d6c0d191022099e027b29a5be1b34b592da31f
ms.sourcegitcommit: 32135463a8494d9ed1600a58f51819359e3c09dc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2020
ms.locfileid: "91834126"
---
# <a name="sysdm_db_xtp_object_stats-transact-sql"></a>sys.dm_db_xtp_object_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Meldet die Anzahl der Zeilen, die von den Vorgängen betroffen sind, die seit dem letzten Neustart der Datenbank für die einzelnen [!INCLUDE[hek_2](../../includes/hek-2-md.md)] -Objekte ausgeführt wurden. Die Statistiken werden aktualisiert, wenn der Vorgang ausgeführt wird, und zwar unabhängig davon, ob für die Transaktion ein Commit oder Rollback ausgeführt wurde.  
  
 Mithilfe von sys.dm_db_xtp_object_stats können Sie ermitteln, welche speicheroptimierten Tabellen am häufigsten geändert werden. Sie können selten oder nicht verwendete Tabellenindizes entfernen, da jeder Index die Leistung beeinflusst. Bei Verwendung von Hashindizes sollte die Bucketanzahl regelmäßig neu ausgewertet werden. Weitere Informationen finden Sie unter [Determining the Correct Bucket Count for Hash Indexes](/previous-versions/sql/).  
  
 Mithilfe von sys.dm_db_xtp_object_stats können Sie ermitteln, welche speicheroptimierten Tabellen Write-Write-Konflikte verursachen, die möglicherweise die Anwendungsleistung beeinträchtigen. Wenn Sie beispielsweise Wiederholungslogik für Transaktionen implementiert haben, muss ein und dieselbe Anweisung u. U. mehrfach ausgeführt werden. Außerdem können Sie anhand dieser Informationen die Tabellen (und folglich die Geschäftslogik) identifizieren, die eine Behandlung von Write-Write-Fehlern erfordern.  
  
 Diese Sicht enthält eine Zeile für jede speicheroptimierte Tabelle in der Datenbank.  
  
 Weitere Informationen finden Sie unter [In-Memory OLTP &#40;Arbeitsspeicheroptimierung&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|object_id|**bigint**|Die ID des Objekts.|  
|row_insert_attempts|**bigint**|Die Anzahl der Zeilen, die seit dem letzten Neustart der Datenbank von Transaktionen, für die ein Commit oder Abbruch ausgeführt wurde, in die Tabelle eingefügt wurden.|  
|row_update_attempts|**bigint**|Die Anzahl der Zeilen, die seit dem letzten Neustart der Datenbank von Transaktionen, für die ein Commit oder Abbruch ausgeführt wurde, in der Tabelle aktualisiert wurden.|  
|row_delete_attempts|**bigint**|Die Anzahl der Zeilen, die seit dem letzten Neustart der Datenbank von Transaktionen, für die ein Commit oder Abbruch ausgeführt wurde, aus der Tabelle gelöscht wurden.|  
|write_conflicts|**bigint**|Die Anzahl der Schreibkonflikte, die seit dem letzten Neustart der Datenbank aufgetreten sind.|  
|unique_constraint_violations|**bigint**|Die Anzahl der Verletzungen von UNIQUE-Einschränkungen, die seit dem letzten Neustart der Datenbank aufgetreten sind.|  
|object_address|**varbinary(8)**|Nur interne Verwendung.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DATABASE STATE-Berechtigung für die aktuelle Datenbank.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten für speicheroptimierte Tabellen (Transact-SQL)](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
