---
description: sys.server_resource_stats (Azure SQL-Datenbank)
title: sys.server_resource_stats (Azure SQL-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/28/2018
ms.service: sql-database
ms.topic: reference
f1_keywords:
- resource_stats
- sys.resource_stats
- sys.resource_stats_TSQL
- resource_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_stats
- resource_stats
ms.assetid: ''
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current
ms.openlocfilehash: f8e27f5f4a12c104efc4f770fd121566d4d3c938
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347649"
---
# <a name="sysserver_resource_stats-azure-sql-database"></a>sys.server_resource_stats (Azure SQL-Datenbank)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Gibt CPU-Auslastung, e/a und Speicherdaten für Azure SQL-verwaltete Instanz zurück. Die Daten werden in Intervallen von fünf Minuten gesammelt und aggregiert. Für jede 15-Sekunden-Berichterstattung ist eine Zeile vorhanden. Die zurückgegebenen Daten umfassen CPU-Auslastung, Speichergröße, e/a-Auslastung und SKU. Verlaufsdaten werden ungefähr 14 Tage lang beibehalten.

Die **sys.server_resource_stats** Ansicht hat abhängig von der Version der Azure SQL-verwaltete Instanz, der die Datenbank zugeordnet ist, unterschiedliche Definitionen. Berücksichtigen Sie diese Unterschiede und alle Änderungen, die Ihre Anwendung erfordert, beim Upgrade auf eine neue Serverversion.
 
  
 Die folgende Tabelle beschreibt die verfügbaren Spalten bei einem Server mit der Version 12:  
  
|Spalten|Datentyp|Beschreibung|  
|----------------------------|---------------|-----------------|  
|start_time|**datetime2**|UTC-Zeit, die den Beginn des 15-Sekunden-Berichts Intervalls angibt|  
|end_time|**datetime**|UTC-Zeit, die das Ende des Berichts Intervalls von 15 Sekunden angibt|
|resource_type|Nvarchar(128)|Der Typ der Ressource, für die Metriken bereitgestellt werden.|
|resource_name|nvarchar(128)|Der Name der Ressource.|
|sku|nvarchar(128)|Verwaltete Instanz Dienst Ebene der-Instanz. Folgende Werte sind möglich: <br><ul><li>Universell</li></ul><ul><li>Unternehmenskritisch</li></ul>|
|hardware_generation|nvarchar(128)|Bezeichner der Hardware Generierung: z. b. Gen 4 oder Gen 5|
|virtual_core_count|INT|Stellt die Anzahl virtueller Kerne pro Instanz (8, 16 oder 24 in Public Preview) dar.|
|avg_cpu_percent|Dezimalzahl (5, 2)|Durchschnittliche Compute-Auslastung als Prozentsatz des Limits der verwaltete Instanz Dienst Ebene, die von der Instanz verwendet wird. Sie wird als Summe der CPU-Zeit aller Ressourcenpools für alle Datenbanken in der Instanz und dividiert durch die verfügbare CPU-Zeit für diese Ebene im angegebenen Intervall berechnet.|
|reserved_storage_mb|BIGINT|Reservierter Speicher pro Instanz (Menge an Speicherplatz, den der Kunde für die verwaltete Instanz gekauft hat)|
|storage_space_used_mb|Decimal (18, 2)|Speicher, der von allen Datenbankdateien in einer verwalteten Instanz verwendet wird (einschließlich Benutzer-und System Datenbanken)|
|io_request|BIGINT|Gesamtanzahl der physischen e/a-Vorgänge innerhalb des Intervalls|
|io_bytes_read|BIGINT|Anzahl der innerhalb des Intervalls gelesenen physischen bytes|
|io_bytes_written|BIGINT|Anzahl physischer bytes, die innerhalb des Intervalls geschrieben wurden|

 
> [!TIP]  
>  Weitere Informationen zu diesen Grenzwerten und Dienst Ebenen finden Sie in den Themen [verwaltete Instanz Dienst Ebenen](/azure/sql-database/sql-database-managed-instance#managed-instance-service-tiers).  
    
## <a name="permissions"></a>Berechtigungen  
 Diese Ansicht ist für alle Benutzer Rollen verfügbar, die über Berechtigungen zum Herstellen einer Verbindung mit der **Master** -Datenbank verfügen.  
  
## <a name="remarks"></a>Bemerkungen  
 Die von **sys.server_resource_stats** zurückgegebenen Daten werden als der Gesamtwert ausgedrückt, der in Byte oder Megabyte (in Spaltennamen) verwendet wird, und nicht als avg_cpu, der als Prozentsatz der maximal zulässigen Grenzwerte für die von Ihnen ausgegebene Dienst Ebene/Leistungsstufe angegeben wird.  
 
## <a name="examples"></a>Beispiele  
Im folgenden Beispiel wird die durchschnittliche CPU-Auslastung in den letzten sieben Tagen zurückgegeben.  
  
```sql  
DECLARE @s datetime;  
DECLARE @e datetime;  
SET @s= DateAdd(d,-7,GetUTCDate());  
SET @e= GETUTCDATE();  
SELECT AVG(avg_cpu_percent) AS Average_Compute_Utilization   
FROM sys.server_resource_stats   
WHERE start_time BETWEEN @s AND @e  
GO;
```  
    
## <a name="see-also"></a>Weitere Informationen  
 [Dienst Ebenen verwaltete Instanz](/azure/sql-database/sql-database-managed-instance#managed-instance-service-tiers)
