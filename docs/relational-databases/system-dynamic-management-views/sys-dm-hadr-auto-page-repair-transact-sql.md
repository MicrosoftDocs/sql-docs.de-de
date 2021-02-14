---
description: sys.dm_hadr_auto_page_repair (Transact-SQL)
title: sys.dm_hadr_auto_page_repair (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_hadr_auto_page_repair_TSQL
- sys.dm_hadr_auto_page_repair
- sys.dm_hadr_auto_page_repair_TSQL
- dm_hadr_auto_page_repair
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- automatic page repair
- sys.dm_hadr_auto_page_repair dynamic management view
ms.assetid: d7840adf-4a1b-41ac-bc94-102c07ad1c79
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b7ec1c79c7d3142909750ebbc525729fa75b82c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354180"
---
# <a name="sysdm_hadr_auto_page_repair-transact-sql"></a>sys.dm_hadr_auto_page_repair (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile für jede versuchte automatische Seitenreparatur in einer beliebigen Verfügbarkeitsdatenbank auf einem Verfügbarkeitsreplikat zurück, das von der Serverinstanz für eine beliebige Verfügbarkeitsgruppe gehostet wird. Diese Sicht enthält Zeilen für die letzte automatische Seitenreparatur einer bestimmten primären oder sekundären Datenbank. Pro Datenbank können maximal 100 Zeilen angezeigt werden. Sobald das Maximum in der Datenbank erreicht ist, ersetzt die Zeile bei der nächsten automatischen Seitenreparatur einen der bereits vorhandenen Einträge.
  
  In der folgenden Tabelle wird die Bedeutung der verschiedenen Spalten definiert:  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID der Datenbank, der diese Zeile entspricht.|  
|**file_id**|**int**|ID der Datei, in der sich diese Seite befindet.|  
|**page_id**|**bigint**|ID der Seite in der Datei.|  
|**error_type**|**int**|Typ des Fehlers. Diese Werte sind möglich:<br /><br /> **-** 1 = alle Hardware 823-Fehler<br /><br /> 1 = Fehler vom Typ 824 außer einer fehlerhaften Prüfsumme oder einer zerrissenen Seite (z. B. eine fehlerhafte Seiten-ID)<br /><br /> 2 = Fehlerhafte Prüfsumme<br /><br /> 3 = Zerrissene Seite|  
|**page_status**|**int**|Status einer Seitenreparatur:<br /><br /> 2 = In der Warteschlange für Partneranforderung.<br /><br /> 3 = Anforderung an Partner gesendet.<br /><br /> 4 = die Seite wurde erfolgreich repariert.<br /><br /> 5 = die Seite konnte beim letzten Versuch nicht repariert werden. die automatische Seiten Reparatur versucht erneut, die Seite zu reparieren.|  
|**modification_time**|**datetime**|Zeitpunkt der letzten Seitenstatusänderung.|  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Automatische Seitenreparatur &#40;Verfügbarkeitsgruppen: Datenbankspiegelung&#41;](../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)   
 [suspect_pages &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/suspect-pages-transact-sql.md)   
 [Verwalten der suspect_pages-Tabelle &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
  

