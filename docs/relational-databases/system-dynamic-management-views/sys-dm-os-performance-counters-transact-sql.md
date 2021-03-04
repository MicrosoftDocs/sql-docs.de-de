---
description: sys.dm_os_performance_counters (Transact-SQL)
title: sys.dm_os_performance_counters (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
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
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e0e080554f8ee1a9a0ba68fe4c4292d2abdb3ee4
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839151"
---
# <a name="sysdm_os_performance_counters-transact-sql"></a>sys.dm_os_performance_counters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile pro Leistungsindikator zurück, der vom Server verwaltet wird. Weitere Informationen zu den einzelnen Leistungs Zählers finden Sie unter [Verwenden von SQL Server Objekten](../../relational-databases/performance-monitor/use-sql-server-objects.md).  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_performance_counters**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**object_name**|**NCHAR (128)**|Kategorie, zu der dieser Leistungsindikator gehört.|  
|**counter_name**|**NCHAR (128)**|Name des Leistungsindikators. Um weitere Informationen zu einem Leistungsindikator zu erhalten, ist dies der Name des Themas, das in der Liste der [SQL Server Objekte verwendeten](../../relational-databases/performance-monitor/use-sql-server-objects.md)Indikatoren ausgewählt werden soll. |  
|**instance_name**|**NCHAR (128)**|Name der spezifischen Instanz des Leistungsindikators. Enthält oft den Datenbanknamen.|  
|**cntr_value**|**bigint**|Aktueller Wert des Leistungsindikators.<br /><br /> **Hinweis:** Für Leistungsindikatoren pro Sekunde ist dieser Wert kumulativ. Der Ratenwert muss durch Stichproben des Werts zu diskreten Zeitintervallen berechnet werden. Der Unterschied zwischen zwei aufeinander folgenden Werten ist gleich der Rate für das verwendete Zeitintervall.|  
|**cntr_type**|**int**|Typ des Leistungsindikators, wie von der Windows-Leistungsarchitektur definiert. Weitere Informationen zu Leistungsdaten Typen finden Sie unter [WMI-Leistungsdaten Typen](/windows/desktop/WmiSdk/wmi-performance-counter-types) in docs oder in Ihrer Windows Server-Dokumentation.|  
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn die Installationsinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Leistungsindikatoren des Windows-Betriebssystems nicht anzeigt, ermitteln Sie mit der folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Abfrage, ob die Leistungsindikatoren deaktiviert wurden.  
  
```sql  
SELECT COUNT(*) FROM sys.dm_os_performance_counters;  
```  
  
Wenn der Rückgabewert 0 Zeilen ist, wurden die Leistungsindikatoren deaktiviert. Sehen Sie sich dann das Setup Protokoll an, und suchen Sie nach Fehler 3409 `Reinstall sqlctr.ini for this instance, and ensure that the instance login account has correct registry permissions.` . Dies deutet darauf hin, dass die Leistungsindikatoren nicht aktiviert wurden. Die Fehlermeldungen unmittelbar vor dem Fehler 3409 sollten die Ursache für das Fehlschlagen der Leistungsindikator-Aktivierung angeben. Weitere Informationen zu Setup Protokolldateien finden Sie unter [anzeigen und lesen SQL Server Setup-Protokolldateien](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  

Leistungsindikatoren, bei denen der `cntr_type` Spaltenwert 65792, 272696320 und 537003264 ist, zeigen einen Leistungsindikator Wert für sofortige Momentaufnahmen an.

Leistungsindikatoren, deren `cntr_type` Spaltenwert 272696576, 1073874176 und 1073939712 ist, zeigen kumulative Indikatorwerte anstelle einer sofortigen Momentaufnahme an. Daher müssen Sie das Delta zwischen zwei Sammelpunkten vergleichen, um einen Momentaufnahme-Lesevorgang zu erhalten.

## <a name="permission"></a>Berechtigung

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
 
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
