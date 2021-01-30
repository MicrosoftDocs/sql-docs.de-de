---
description: MSmerge_sessions (Transact-SQL)
title: MSmerge_sessions (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_sessions
- MSmerge_sessions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_sessions system table
ms.assetid: 09ada8fc-c148-4379-9524-7826b1b0216c
author: cawrites
ms.author: chadam
ms.openlocfilehash: af57f9ae760c09fa14bffe42ceba987734b7c5ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200292"
---
# <a name="msmerge_sessions-transact-sql"></a>MSmerge_sessions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSmerge_sessions** Tabelle enthält Verlaufs Zeilen mit den Ergebnissen vorheriger Merge-Agent Auftrags Sitzungen. Bei jedem Ausführen des Merge-Agents wird der Tabelle eine neue Zeile hinzugefügt. Diese Tabelle wird in der Verteilungsdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|Die ID der Auftragssitzung des Merge-Agents|  
|**agent_id**|**int**|Die ID des Merge-Agents|  
|**start_time**|**datetime**|Der Zeitpunkt, zu dem die Ausführung des Auftrags begonnen hat|  
|**end_time**|**datetime**|Der Zeitpunkt, zu dem die Ausführung des Auftrags beendet wurde|  
|**duration**|**int**|Die kumulierte Dauer dieser Auftragssitzung in Sekunden|  
|**delivery_time**|**int**|Die Anzahl der Sekunden, die für die Anwendung eines Batches von Änderungen benötigt wurden.|  
|**upload_time**|**int**|Die Anzahl der Sekunden, die für das Hochladen von Änderungen auf den Verleger benötigt wurden.|  
|**download_time**|**int**|Die Anzahl der Sekunden, die für das Herunterladen von Änderungen auf den Abonnenten benötigt wurden.|  
|**delivery_rate**|**float**|Die durchschnittliche Anzahl der übermittelten Befehle pro Sekunde.|  
|**time_remaining**|**int**|Die geschätzte Anzahl der in einer aktiven Sitzung verbleibenden Sekunden.|  
|**percent_complete**|**decimal**|Der geschätzte Prozentsatz der gesamten Änderungen, die in einer aktiven Sitzung bereits übermittelt wurden|  
|**upload_inserts**|**int**|Die Anzahl der auf dem Verleger angewendeten Einfügungen.|  
|**upload_updates**|**int**|Die Anzahl der auf dem Verleger angewendeten Updates.|  
|**upload_deletes**|**int**|Die Anzahl der auf dem Verleger angewendeten Löschungen.|  
|**upload_conflicts**|**int**|Die Anzahl der Konflikte, die während der Anwendung von Änderungen auf dem Verleger aufgetreten sind.|  
|**upload_conflicts_resolved**|**int**|Die Anzahl der Konflikte, die während der Anwendung von Änderungen auf dem Verleger gelöst wurden.|  
|**upload_rows_retried**|**int**|Die Anzahl der auf den Verleger hochgeladenen Zeilen, deren Übermittlung wiederholt werden musste.|  
|**download_inserts**|**int**|Die Anzahl der auf dem Abonnenten angewendeten Einfügungen.|  
|**download_updates**|**int**|Die Anzahl der auf dem Abonnenten angewendeten Updates.|  
|**download_deletes**|**int**|Die Anzahl der auf dem Abonnenten angewendeten Löschungen.|  
|**download_conflicts**|**int**|Die Anzahl der Konflikte, die während der Anwendung von Änderungen auf dem Abonnenten aufgetreten sind.|  
|**download_conflicts_resolved**|**int**|Die Anzahl der Konflikte, die während der Anwendung von Änderungen auf dem Abonnenten gelöst wurden.|  
|**download_rows_retried**|**int**|Die Anzahl der auf den Abonnenten heruntergeladenen Zeilen, deren Übermittlung wiederholt werden musste.|  
|**schema_changes**|**int**|Die Anzahl der während der Sitzung angewendeten Schemaänderungen.|  
|**metadata_rows_cleanedup**|**int**|Die Anzahl der Zeilen mit Metadaten, für die während der Sitzung ein Cleanup ausgeführt wurde.|  
|**RunStatus**|**int**|Der Ausführungsstatus:<br /><br /> **1** = Start.<br /><br /> **2** = erfolgreich.<br /><br /> **3** = wird ausgeführt.<br /><br /> **4** = im Leerlauf.<br /><br /> **5** = Wiederholungsversuch.<br /><br /> **6** = Fehler.|  
|**estimated_upload_changes**|**int**|Die geschätzte Anzahl von Änderungen, die auf dem Verleger angewendet werden mussten|  
|**estimated_download_changes**|**int**|Die geschätzte Anzahl von Änderungen, die auf dem Abonnenten angewendet werden mussten|  
|**connection_type**|**int**|Die während des Hochladens verwendete Verbindung:<br /><br /> **1** = LAN (Local Area Network).<br /><br /> **2** = DFÜ-Netzwerkverbindung.<br /><br /> **3** = Websynchronisierung.|  
|**timestamp**|**timestamp**|Die Timestampspalte dieser Tabelle.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
