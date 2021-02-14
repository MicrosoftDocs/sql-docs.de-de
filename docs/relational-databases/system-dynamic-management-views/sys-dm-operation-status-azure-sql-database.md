---
description: sys.dm_operation_status
title: sys.dm_operation_status | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/05/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- dm_operation_status_TSQL
- dm_operation_status
- sys.dm_operation_status
- sys.dm_operation_status_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_operation_status dynamic management view
- sys.dm_operation_status dynamic management view
ms.assetid: cc847784-7f61-4c69-8b78-5f971bb24d61
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 576e5dd0010b60860d9c37e39c5d28f54ddf4546
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342872"
---
# <a name="sysdm_operation_status"></a>sys.dm_operation_status

[!INCLUDE [asdb-asdbmi-asa](../../includes/applies-to-version/asdb-asdbmi-asa.md)]

  Gibt Informationen zu den Vorgängen zurück, die für Datenbanken auf einem [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]-Server ausgeführt werden.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|session_activity_id|**uniqueidentifier**|ID des Vorgangs. Nicht NULL.|  
|resource_type|**int**|Bezeichnet den Typ der Ressource, für die der Vorgang ausgeführt wird. Nicht NULL. In der aktuellen Version verfolgt diese Sicht nur die Vorgänge nach, die für [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ausgeführt werden. Der entsprechende ganzzahlige Wert ist 0.|  
|resource_type_desc|**nvarchar (2048)**|Beschreibung des Ressourcentyps, für den der Vorgang ausgeführt wird. In der aktuellen Version verfolgt diese Sicht nur die Vorgänge nach, die für [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ausgeführt werden.|  
|major_resource_id|**sql_variant**|Name von [!INCLUDE[ssSDS](../../includes/sssds-md.md)], für die der Vorgang ausgeführt wird. Nicht NULL.|  
|minor_resource_id|**sql_variant**|Nur zur internen Verwendung. Nicht NULL.|  
|operation|**nvarchar(60)**|Für [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ausgeführter Vorgang, z. B. CREATE oder ALTER.|  
|state|**tinyint**|Der Zustand des Vorgangs.<br /><br /> 0 = Ausstehend<br />1 = Vorgang wird ausgeführt<br />2 = Abgeschlossen<br />3 = fehlgeschlagen<br />4 = Abgebrochen|  
|state_desc|**nvarchar(120)**|PENDING = Vorgang wartet auf verfügbare Ressource oder verfügbares Kontingent.<br /><br /> IN_PROGRESS = Vorgang wurde gestartet und wird ausgeführt.<br /><br /> COMPLETED = Vorgang wurde erfolgreich abgeschlossen.<br /><br /> FAILED = Vorgang ist fehlgeschlagen. Weitere Informationen finden Sie in der Spalte **error_desc** .<br /><br /> CANCELLED = Vorgang wurde auf Anforderung des Benutzers beendet.|  
|percent_complete|**int**|Prozentsatz des Vorgangs, der abgeschlossen wurde. Werte sind nicht kontinuierlich, und die gültigen Werte sind unten aufgeführt. Nicht NULL.<br/><br/>0 = Vorgang wurde nicht gestartet.<br/>50 = Vorgang wird ausgeführt<br/>100 = Vorgang wurde beendet.|  
|error_code|**int**|Code, der den Fehler angibt, der während eines fehlgeschlagenen Vorgangs aufgetreten ist. Wenn der Wert 0 ist, bedeutet dies, dass der Vorgang erfolgreich abgeschlossen wurde.|  
|error_desc|**nvarchar (2048)**|Beschreibung des Fehlers, der während eines fehlgeschlagenen Vorgangs aufgetreten ist.|  
|error_severity|**int**|Schweregrad des Fehlers, der während eines fehlgeschlagenen Vorgangs aufgetreten ist. Weitere Informationen zu Schweregraden von Fehlern finden Sie unter [Datenbank-Engine schwere](../errors-events/database-engine-error-severities.md)Grade von Fehlern.|  
|error_state|**int**|Für zukünftige Verwendung reserviert. Zukünftige Kompatibilität wird nicht sichergestellt.|  
|start_time|**datetime**|Zeitstempel, an dem der Vorgang begonnen wurde.|  
|last_modify_time|**datetime**|Zeitstempel, an dem der Datensatz zuletzt für einen länger ausgeführten Vorgang geändert wurde. Im Fall von erfolgreich abgeschlossenen Vorgängen wird in diesem Feld der Zeitstempel angezeigt, an dem der Vorgang abgeschlossen wurde.|  
  
## <a name="permissions"></a>Berechtigungen  
 Diese Ansicht ist nur in der **Master** -Datenbank für den Prinzipal Anmelde Namen auf Serverebene verfügbar.  
  
## <a name="remarks"></a>Bemerkungen  
 Um diese Ansicht verwenden zu können, müssen Sie mit der **Master** -Datenbank verbunden sein. Verwenden Sie die- `sys.dm_operation_status` Sicht in der **Master** -Datenbank des- [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Servers, um den Status der folgenden Vorgänge zu verfolgen, die auf einem ausgeführt werden [!INCLUDE[ssSDS](../../includes/sssds-md.md)] :  
  
-   Erstellen einer Datenbank  
  
-   Kopieren einer Datenbank. Mit Datenbankkopie wird ein Datensatz in dieser Sicht auf den Quell- und Zielservern erstellt.  
  
-   Ändern einer Datenbank  
  
-   Ändern der Leistungsebene einer Dienstebene  
  
-   Ändern der Dienstebene einer Datenbank, z. B. von Basic in Standard.  
  
-   Einrichten einer Georeplikationsbeziehung  
  
-   Beenden einer Georeplikationsbeziehung  
  
-   Datenbank wiederherstellen  
  
-   Datenbank löschen  

Die Informationen in dieser Ansicht werden ungefähr eine Stunde lang aufbewahrt. Verwenden Sie das [Azure-Aktivitätsprotokoll](/azure/azure-monitor/platform/activity-log) , um Details zu den Vorgängen in den letzten 90 Tagen anzuzeigen. Wenn Sie mehr als 90 Tage aufbewahren möchten, sollten Sie [Aktivitätsprotokoll](/azure/azure-monitor/platform/activity-log#send-to-log-analytics-workspace) Einträge an einen Log Analytics Arbeitsbereich senden.

## <a name="example"></a>Beispiel  
 Zeigt die letzten georeplikationsvorgänge an, die der Datenbank "MyDB" zugeordnet sind.  
  
```  
SELECT * FROM sys.dm_operation_status   
   WHERE major_resource_id = 'myddb'   
   ORDER BY start_time DESC;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungs Sichten und-Funktionen für die georeplikation &#40;Azure SQL-Datenbank&#41;](../../relational-databases/system-dynamic-management-views/geo-replication-dynamic-management-views-and-functions-azure-sql-database.md)   
 [sys.dm_geo_replication_link_status &#40;Azure SQL-Datenbank&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md)   
 [sys.geo_replication_links &#40;Azure SQL-Datenbank&#41;](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md)   
 [ALTER DATABASE &#40;Azure SQL-Datenbank&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
  
