---
description: sys.database_connection_stats (Azure SQL-Datenbank)
title: sys.database_connection_stats
titleSuffix: Azure SQL Database
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.database_connection_stats
- database_connection_stats
- database_connection_stats_TSQL
- sys.database_connection_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_connection_stats
- database_connection_stats
ms.assetid: 5c8cece0-63b0-4dee-8db7-6b43d94027ec
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current
ms.openlocfilehash: 9aaca2dee46593d637b11cde9b4e9e2a55664f6b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350891"
---
# <a name="sysdatabase_connection_stats-azure-sql-database"></a>sys.database_connection_stats (Azure SQL-Datenbank)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Enthält Statistiken für [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Datenbankverbindungs-Ereignisse und bietet einen Überblick über erfolgreiche und fehlgeschlagene Datenbankverbindungen.  Weitere Informationen zu konnektivitätsereignissen finden Sie unter Ereignis Typen in [sys.event_log &#40;Azure SQL-Datenbank&#41;](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md).  
  
|Statistik|type|BESCHREIBUNG|  
|---------------|----------|-----------------|  
|**database_name**|**sysname**|Der Name der Datenbank.|  
|**start_time**|**datetime2**|UTC-Datum und -Zeit des Beginns des Aggregationsintervalls. Die Uhrzeit ist immer ein Vielfaches von 5 Minuten. Beispiel:<br /><br /> '2011-09-28 16:00:00'<br />'2011-09-28 16:05:00'<br />'2011-09-28 16:10:00'|  
|**end_time**|**datetime2**|UTC-Datum und -Zeit des Endes des Aggregationsintervalls. **End_time** ist immer genau 5 Minuten später als die entsprechende **start_time** in derselben Zeile.|  
|**success_count**|**int**|Anzahl erfolgreicher Verbindungen.|  
|**total_failure_count**|**int**|Gesamtzahl fehlerhafter Verbindungen. Dies ist die Summe aus **connection_failure_count**, **terminated_connection_count** und **throttled_connection_count** und umfasst keine Deadlockereignisse.|  
|**connection_failure_count**|**int**|Anzahl der Anmeldefehler.|  
|**terminated_connection_count**|**int**|**_Gilt nur für [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] v11._**<br /><br /> Anzahl beendeter Verbindungen.|  
|**throttled_connection_count**|**int**|**_Gilt nur für [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] v11._**<br /><br /> Anzahl gedrosselter Verbindungen.|  
  
## <a name="remarks"></a>Bemerkungen  
  
### <a name="event-aggregation"></a>Ereignisaggregation

 Die Ereignisinformationen für diese Sicht werden gesammelt und innerhalb von 5-minütigen Intervallen aggregiert. Die Anzahlspalten stellen die Häufigkeit dar, mit der ein bestimmtes Konnektivitätsereignis für eine bestimmte Datenbank innerhalb eines angegebenen Zeitintervalls aufgetreten ist.  
  
 Wenn ein Benutzer beispielsweise am 05.02.2012 zwischen 11:00 und 11:05 Uhr (UTC) sieben Mal eine Verbindung mit der Datenbank „Database1“ herstellt, sind diese Informationen in dieser Sicht in einer einzelnen Zeile verfügbar:  
  
|**database_name**|**start_time**|**end_time**|**success_count**|**total_failure_count**|**connection_failure_count**|**terminated_connection_count**|**throttled_connection_count**|  
|------------------------|---------------------|-------------------|------------------------|-------------------------------|------------------------------------|---------------------------------------|--------------------------------------|  
|`Database1`|`2012-02-05 11:00:00`|`2012-02-05 11:05:00`|`0`|`7`|`7`|`0`|`0`|  
  
### <a name="interval-start_time-and-end_time"></a>start_time und end_time des Intervalls

 Ein Ereignis ist in einem Aggregations Intervall enthalten, wenn das Ereignis *am* oder _nach_**start_time** und _vor_**end_time** für dieses Intervall auftritt. Beispielsweise würde ein Ereignis, das genau zum Zeitpunkt `2012-10-30 19:25:00.0000000` eintritt, nur im zweiten unten gezeigten Intervall aufgenommen werden:  
  
```  
  
start_time                    end_time  
2012-10-30 19:20:00.0000000   2012-10-30 19:25:00.0000000  
2012-10-30 19:25:00.0000000   2012-10-30 19:30:00.0000000  
```  
  
### <a name="data-updates"></a>Datenupdates

 Die Daten in dieser Sicht werden im Zeitverlauf gesammelt. Normalerweise werden die Daten innerhalb einer Stunde nach Beginn des Aggregationsintervalls gesammelt, es kann aber maximal 24 Stunden dauern, bis alle Daten in der Sicht angezeigt werden. Während dieser Zeit werden die Informationen in einer einzelnen Zeile möglicherweise gelegentlich aktualisiert.  
  
### <a name="data-retention"></a>Datenaufbewahrung

 Die Daten in dieser Sicht werden maximal 30 Tage lang aufbewahrt, abhängig von der Anzahl der Datenbanken und der Anzahl der von jeder Datenbank generierten eindeutigen Ereignisse. Um diese Informationen für einen längeren Zeitraum beizubehalten, kopieren Sie die Daten in eine separate Datenbank. Nachdem Sie eine erste Kopie der Sicht erstellt haben, werden die Zeilen in der Sicht möglicherweise aktualisiert, während die Daten gesammelt werden. Damit die Kopie der Daten aktuell bleibt, führen Sie regelmäßig einen Tabellenscan der Zeilen aus, um nach einer Erhöhung der Ereignisanzahl für vorhandene Zeilen zu suchen und um neue Zeilen zu ermitteln (eindeutige Zeilen bestimmen Sie anhand der Start- und Endzeiten). Aktualisieren Sie dann die Kopie der Daten mit diesen Änderungen.  
  
### <a name="errors-not-included"></a>Fehler nicht enthalten

 Diese Sicht enthält möglicherweise nicht alle Verbindungs- und Fehlerinformationen:  
  
- Diese Ansicht enthält nicht alle [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Datenbankfehler, die auftreten können, sondern nur die in Ereignis Typen in [sys.event_log &#40;Azure SQL-Datenbank&#41;](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md).  
  
- Wenn ein Computerfehler innerhalb des [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Rechenzentrums auftritt, fehlt in der Ereignis Tabelle möglicherweise eine kleine Menge von Daten.  
  
- Wenn eine IP-Adresse von DoSGuard blockiert wurde, können Verbindungsversuchsereignisse von dieser IP-Adresse nicht gesammelt werden. Diese werden in dieser Sicht nicht angezeigt.  
  
## <a name="permissions"></a>Berechtigungen

 Benutzer, die über die Berechtigung zum Zugriff auf die **Master** -Datenbank verfügen, haben schreibgeschützten Zugriff auf diese Sicht.  
  
## <a name="example"></a>Beispiel

 Das folgende Beispiel zeigt eine Abfrage von **sys.database_connection_stats** , um eine Zusammenfassung der Datenbankverbindungen zurückzugeben, die zwischen 12 Uhr 9/25/2011 mittags und 12 9/28/2011 Uhr UTC (UTC) aufgetreten sind. Standardmäßig werden die Abfrageergebnisse nach **start_time** (aufsteigender Reihenfolge) sortiert.  
  
```sql
SELECT *  
FROM sys.database_connection_stats
WHERE start_time>='2011-09-25 12:00:00' and end_time<='2011-09-28 12:00:00';  
```  

## <a name="see-also"></a>Weitere Informationen

 [Beheben von Verbindungsproblemen mit der Azure SQL-Datenbank](/azure/sql-database/sql-database-troubleshoot-common-connection-issues)  
  
  
