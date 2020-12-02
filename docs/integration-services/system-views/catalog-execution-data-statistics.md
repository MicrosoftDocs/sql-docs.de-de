---
description: catalog.execution_data_statistics
title: catalog.execution_data_statistics | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 6f51407e-0e4e-4b44-af33-db14c9d40ded
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 24452ceffc1660e57b087395ded3923e47802314
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129481"
---
# <a name="catalogexecution_data_statistics"></a>catalog.execution_data_statistics 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In dieser Sicht wird eine Zeile angezeigt, sobald eine Datenflusskomponente Daten für eine bestimmte Paketausführung an eine Downstreamkomponente sendet. Anhand der Informationen in dieser Sicht kann der Datendurchsatz für eine Komponente berechnet werden.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|data_stats_id|**bigint**|Eindeutiger Bezeichner (ID) der Daten.|  
|execution_id|**bigint**|Eindeutige ID für die Instanz der Ausführung.|  
|package_name|**nvarchar(260)**|Der Name des ersten Pakets, das während der Ausführung gestartet wurde.|  
|task_name|**nvarchar(4000)**|Der Name des Datenflusstask.|  
|dataflow_path_id_string|**nvarchar(4000)**|Die Identifikationszeichenfolge des Datenflusspfads.|  
|dataflow_path_name|**nvarchar(4000)**|Der Name des Datenflusspfads.|  
|source_component_name|**nvarchar(4000)**|Der Namen der Datenflusskomponente, die die Daten gesendet hat.|  
|destination_component_name|**nvarchar(4000)**|Der Namen der Datenflusskomponente, die die Daten empfangen hat.|  
|rows_sent|**bigint**|Die Anzahl der Zeilen, die von der Quellkomponente gesendet wurden.|  
|created_time|**datatimeoffset(7)**|Die Zeit, zu der die Werte abgerufen wurden.|  
|execution_path|**nvarchar(max)**|Der Ausführungspfad der Komponente.|  
  
## <a name="remarks"></a>Hinweise  
  
-   Wenn eine Komponente mehrere Ausgaben hat, wird für jede Ausgabe eine Zeile hinzugefügt.  
  
-   Standardmäßig werden beim Starten einer Ausführung keine Informationen über die Anzahl der gesendeten Zeilen protokolliert.  
  
-   Um diese Daten für eine bestimmte Paketausführung anzuzeigen, legen Sie den Protokolliergrad auf **Ausführlich** fest. Weitere Informationen finden Sie unter [Aktivieren der Protokollierung für die Paketausführung auf dem SSIS-Server](../../integration-services/performance/integration-services-ssis-logging.md#server_logging).  
  
## <a name="permissions"></a>Berechtigungen  
 Diese Sicht erfordert eine der folgenden Berechtigungen:  
  
-   READ-Berechtigung für die Instanz der Ausführung  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
> [!NOTE]  
>  Wenn Sie über die Berechtigung verfügen, einen Vorgang auf dem Server auszuführen, verfügen Sie auch über die Berechtigung, Informationen zu dem Vorgang anzuzeigen. Sicherheit auf Zeilenebene wird erzwungen. Es werden nur Zeilen angezeigt, zu deren Anzeige Sie berechtigt sind.  
  
  
