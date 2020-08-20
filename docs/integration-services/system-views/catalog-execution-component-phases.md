---
description: catalog.execution_component_phases
title: catalog.execution_component_phases | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 07a9a163-4787-40f7-b371-ac5c6cb4b095
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e307babfd703b83758a6d1d3c7de3e6f62b00119
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88495233"
---
# <a name="catalogexecution_component_phases"></a>catalog.execution_component_phases 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Zeigt die von einer Datenflusskomponente in jeder Ausführungsphase benötigte Zeit an.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|phase_stats_id|**bigint**|Eindeutiger Bezeichner (ID) der Phase.|  
|execution_id|**bigint**|Eindeutige ID für die Instanz der Ausführung.|  
|package_name|**nvarchar(260)**|Der Name des ersten Pakets, das während der Ausführung gestartet wurde.|  
|task_name|**nvarchar(4000)**|Der Name des Datenflusstask.|  
|subcomponent_name|**nvarchar(4000)**|Der Name der Datenflusskomponente.|  
|phase|**nvarchar(128)**|Der Name der Ausführungsphase.|  
|start_time|**datetimeoffset(7)**|Der Zeitpunkt, zu dem die Phase gestartet wurde.|  
|end_time|**datetimeoffset(7)**|Der Zeitpunkt, zu dem die Phase beendet wurde.|  
|execution_path|**nvarchar(max)**|Der Ausführungspfad der Datenflusstask.|  
  
## <a name="remarks"></a>Bemerkungen  
 In dieser Sicht wird für jede Ausführungsphase einer Datenflusskomponente eine Zeile angezeigt, z. B. Überprüfen, Vor der Ausführung, Nach der Ausführung, PrimeOutput und ProcessInput. Jede Zeile zeigt die Start- und Endzeit einer bestimmten Ausführungsphase an.  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird die catalog.execution_component_phases-Sicht verwendet, um die Gesamtzeit zu suchen, die für die Ausführung eines bestimmten Pakets in allen Phasen benötigt wurde (**active_time**), sowie die insgesamt verstrichene Zeit für das Paket (**total_time**).  
  
> [!WARNING]  
>  Die catalog.execution_component_phases-Sicht enthält diese Informationen, wenn der Protokolliergrad der Paketausführung auf "Leistung" oder "Ausführlich" festgelegt wird. Weitere Informationen finden Sie unter [Aktivieren der Protokollierung für die Paketausführung auf dem SSIS-Server](../../integration-services/performance/integration-services-ssis-logging.md#server_logging).  
  
```sql
use SSISDB  
select package_name, task_name, subcomponent_name, execution_path,  
    SUM(DATEDIFF(ms,start_time,end_time)) as active_time,  
    DATEDIFF(ms,min(start_time), max(end_time)) as total_time  
from catalog.execution_component_phases  
where execution_id = 1841  
group by package_name, task_name, subcomponent_name, execution_path  
order by package_name, task_name, subcomponent_name, execution_path  
```  
  
## <a name="permissions"></a>Berechtigungen  
 Diese Sicht erfordert eine der folgenden Berechtigungen:  
  
-   READ-Berechtigung für die Instanz der Ausführung  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
> [!NOTE]  
>  Wenn Sie über die Berechtigung verfügen, einen Vorgang auf dem Server auszuführen, verfügen Sie auch über die Berechtigung, Informationen zu dem Vorgang anzuzeigen. Sicherheit auf Zeilenebene wird erzwungen. Es werden nur Zeilen angezeigt, zu deren Anzeige Sie berechtigt sind.  
  
  
