---
description: sys.dm_os_performance_counters (Transact-SQL)
title: sys.dm_os_performance_counters (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_performance_counters
- sys.dm_os_performance_counters_TSQL
- dm_os_performance_counters_TSQL
- sys.dm_os_performance_counters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_performance_counters dynamic management view
ms.assetid: a1c3e892-cd48-40d4-b6be-2a9246e8fbff
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bf8f1d6f2b9ae0e724e23238dea494ad0cb4529f
ms.sourcegitcommit: 32135463a8494d9ed1600a58f51819359e3c09dc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2020
ms.locfileid: "91834241"
---
# <a name="sysdm_os_performance_counters-transact-sql"></a>sys.dm_os_performance_counters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile pro Leistungsindikator zurück, der vom Server verwaltet wird. Weitere Informationen zu den einzelnen Leistungs Zählers finden Sie unter [Verwenden von SQL Server Objekten](../../relational-databases/performance-monitor/use-sql-server-objects.md).  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_performance_counters**.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**object_name**|**NCHAR (128)**|Kategorie, zu der dieser Leistungsindikator gehört.|  
|**counter_name**|**NCHAR (128)**|Name des Leistungsindikators. Um weitere Informationen zu einem Leistungsindikator zu erhalten, ist dies der Name des Themas, das in der Liste der [SQL Server Objekte verwendeten](../../relational-databases/performance-monitor/use-sql-server-objects.md)Indikatoren ausgewählt werden soll. |  
|**instance_name**|**NCHAR (128)**|Name der spezifischen Instanz des Leistungsindikators. Enthält oft den Datenbanknamen.|  
|**cntr_value**|**bigint**|Aktueller Wert des Leistungsindikators.<br /><br /> **Hinweis:** Für Leistungsindikatoren pro Sekunde ist dieser Wert kumulativ. Der Ratenwert muss durch Stichproben des Werts zu diskreten Zeitintervallen berechnet werden. Der Unterschied zwischen zwei aufeinander folgenden Werten ist gleich der Rate für das verwendete Zeitintervall.|  
|**cntr_type**|**int**|Typ des Leistungsindikators, wie von der Windows-Leistungsarchitektur definiert. Weitere Informationen zu Leistungsdaten Typen finden Sie unter [WMI-Leistungsdaten Typen](/windows/desktop/WmiSdk/wmi-performance-counter-types) in docs oder in Ihrer Windows Server-Dokumentation.|  
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="remarks"></a>Hinweise  
 Wenn die Installationsinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Leistungsindikatoren des Windows-Betriebssystems nicht anzeigt, ermitteln Sie mit der folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Abfrage, ob die Leistungsindikatoren deaktiviert wurden.  
  
```sql  
SELECT COUNT(*) FROM sys.dm_os_performance_counters;  
```  
  
Wenn der Rückgabewert 0 Zeilen ist, wurden die Leistungsindikatoren deaktiviert. Sehen Sie sich dann das Setup Protokoll an, und suchen Sie nach Fehler 3409 `Reinstall sqlctr.ini for this instance, and ensure that the instance login account has correct registry permissions.` . Dies deutet darauf hin, dass die Leistungsindikatoren nicht aktiviert wurden. Die Fehlermeldungen unmittelbar vor dem Fehler 3409 sollten die Ursache für das Fehlschlagen der Leistungsindikator-Aktivierung angeben. Weitere Informationen zu Setup Protokolldateien finden Sie unter [anzeigen und lesen SQL Server Setup-Protokolldateien](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  

Leistungsindikatoren, bei denen der `cntr_type` Spaltenwert 65792, 272696320 und 537003264 ist, zeigen einen Leistungsindikator Wert für sofortige Momentaufnahmen an.

Leistungsindikatoren, deren `cntr_type` Spaltenwert 272696576, 1073874176 und 1073939712 ist, zeigen kumulative Indikatorwerte anstelle einer sofortigen Momentaufnahme an. Daher müssen Sie das Delta zwischen zwei Sammelpunkten vergleichen, um einen Momentaufnahme-Lesevorgang zu erhalten.

## <a name="permission"></a>Berechtigung

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] Premium-Tarifen ist die- `VIEW DATABASE STATE` Berechtigung in der Datenbank erforderlich. In [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] den Tarifen "Standard" und "Basic" ist der  **Server Administrator** oder ein **Azure Active Directory Administrator** Konto erforderlich.   
 
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden alle Leistungsindikatoren zurückgegeben, die Momentaufnahme Zählerwerte anzeigen.  
  
```sql  
SELECT object_name, counter_name, instance_name, cntr_value, cntr_type  
FROM sys.dm_os_performance_counters
WHERE cntr_type = 65792 OR cntr_type = 272696320 OR cntr_type = 537003264;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
  [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.sysperfinfo &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)  
  
