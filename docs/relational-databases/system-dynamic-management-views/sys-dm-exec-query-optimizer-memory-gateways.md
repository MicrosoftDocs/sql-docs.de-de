---
title: sys.dm_exec_query_optimizer_memory_gateways (Transact-SQL)
description: Gibt den aktuellen Status von Ressourcen Semaphoren zurück, die zum Einschränken der gleichzeitigen Abfrageoptimierung verwendet werden.
ms.custom: seo-dt-2019
ms.date: 04/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: reference
f1_keywords:
- dm_exec_query_optimizer_memory_gateways_TSQL
- dm_exec_query_optimizer_memory_gateways
- sys.dm_exec_query_optimizer_memory_gateways_TSQL
- sys.dm_exec_query_optimizer_memory_gateways
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_optimizer_memory_gateways dynamic management view
author: josack
ms.author: josack
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 66e5d3e4509b8593a41a53348e466054ee01b4f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342911"
---
# <a name="sysdm_exec_query_optimizer_memory_gateways-transact-sql"></a>sys.dm_exec_query_optimizer_memory_gateways (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Gibt den aktuellen Status von Ressourcen Semaphoren zurück, die zur Drosselung der gleichzeitigen Abfrageoptimierung verwendet werden.

|Spalte|Typ|Beschreibung|  
|----------|---------------|-----------------|  
|**pool_id**|**int**|Ressourcenpool-ID unter Resource Governor|  
|**name**|**sysname**|Name der Kompilierungs Gate (kleines Gateway, mittleres Gateway, großes Gateway)|
|**max_count**|**int**|Die maximal konfigurierte Anzahl gleichzeitiger Kompilierungen|
|**active_count**|**int**|Die derzeit aktive Anzahl der Kompilierungen in diesem Gate|
|**waiter_count**|**int**|Die Anzahl der Kellner in diesem Gate|
|**threshold_factor**|**bigint**|Schwellenwert, der den maximalen Arbeitsspeicher Bereich definiert, der von der Abfrageoptimierung verwendet wird.  Beim kleinen Gateway gibt threshold_factor die maximale Arbeitsspeicher Auslastung des Optimierers in Bytes für eine Abfrage an, bevor ein Zugriff auf das kleine Gateway benötigt wird.  Für das mittlere und das große Gateway zeigt threshold_factor den Teil des gesamten Server Arbeitsspeichers an, der für dieses Gate verfügbar ist. Sie wird als Divisor verwendet, wenn der Schwellenwert für die Speicherauslastung für das Gate berechnet wird.|
|**threshold**|**bigint**|Nächster Schwellenwert für den Arbeitsspeicher in Bytes.  Die Abfrage ist erforderlich, um einen Zugriff auf dieses Gateway zu erhalten, wenn der Speicherverbrauch diesen Schwellenwert erreicht.  "-1", wenn die Abfrage nicht erforderlich ist, um Zugriff auf dieses Gateway zu erhalten.|
|**is_active**|**bit**|Gibt an, ob die Abfrage erforderlich ist, um das aktuelle Gate zu übergeben.|


## <a name="permissions"></a>Berechtigungen  
SQL Server erfordert die View Server State-Berechtigung auf dem Server.

Azure SQL-Datenbank erfordert die VIEW DATABASE STATE-Berechtigung in der Datenbank.


## <a name="remarks"></a>Bemerkungen  
SQL Server verwendet einen mehrstufigen gatewayansatz, um die Anzahl zulässiger gleichzeitiger Kompilierungen zu drosseln.  Es werden drei Gateways verwendet, einschließlich klein, Mittel und groß. Gateways helfen, die Erschöpfung der Gesamtspeicher Ressourcen durch größere Kompilierungs Arbeitsspeicher-Consumer zu vermeiden.

Wartet auf ein gatewayergebnis bei verzögerter Kompilierung. Zusätzlich zu den Verzögerungen bei der Kompilierung verfügen drosselte Anforderungen über eine zugeordnete RESOURCE_SEMAPHORE_QUERY_COMPILE Wartetyp Akkumulation. Der RESOURCE_SEMAPHORE_QUERY_COMPILE Wartetyp weist möglicherweise darauf hin, dass Abfragen eine große Menge an Arbeitsspeicher für die Kompilierung verwenden und dass der Arbeitsspeicher erschöpft ist, oder ob ausreichend Arbeitsspeicher verfügbar ist, aber die verfügbaren Einheiten in einem bestimmten Gateway sind erschöpft. Die Ausgabe von **sys.dm_exec_query_optimizer_memory_gateways** kann zur Problembehandlung bei Szenarien verwendet werden, in denen nicht genügend Arbeitsspeicher zum Kompilieren eines Abfrage Ausführungs Plans vorhanden war.  

## <a name="examples"></a>Beispiele  

### <a name="a-viewing-statistics-on-resource-semaphores"></a>A. Anzeigen von Statistiken für Ressourcen Semaphoren  
Wie lauten die aktuellen Speicher Gateway-Statistiken des Optimierers für diese Instanz von SQL Server?

```  
SELECT [pool_id], [name], [max_count], [active_count],
       [waiter_count], [threshold_factor], [threshold],
       [is_active]
FROM sys.dm_exec_query_optimizer_memory_gateways;   

```  

## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](./system-dynamic-management-views.md)   
 [Execution Related Dynamic Management Views and Functions &#40;Transact-SQL&#41; (Dynamische Verwaltungssichten und Funktionen im Zusammenhang mit der Ausführung (Transact-SQL))](./execution-related-dynamic-management-views-and-functions-transact-sql.md)  
[Verwenden des Befehls DBCC MEMORYSTATUS zum Überwachen der Speicherauslastung auf SQL Server 2005](https://support.microsoft.com/help/907877/how-to-use-the-dbcc-memorystatus-command-to-monitor-memory-usage-on-sql-server-2005) 
 Die [Kompilierung großer Abfragen wartet auf RESOURCE_SEMAPHORE_QUERY_COMPILE in SQL Server 2014](https://support.microsoft.com/help/3024815/large-query-compilation-waits-on-resource-semaphore-query-compile-in-sql-server-2014)
