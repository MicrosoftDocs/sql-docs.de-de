---
description: sys.dm_exec_query_optimizer_info (Transact-SQL)
title: sys.dm_exec_query_optimizer_info (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_query_optimizer_info_TSQL
- dm_exec_query_optimizer_info
- sys.dm_exec_query_optimizer_info_TSQL
- sys.dm_exec_query_optimizer_info
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_optimizer_info dynamic management view
ms.assetid: 1d72cef1-22d8-4ae0-91db-6694fe918c9e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3bd179dd4660ce82e6629619c92e0e0e8e429bc1
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342955"
---
# <a name="sysdm_exec_query_optimizer_info-transact-sql"></a>sys.dm_exec_query_optimizer_info (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt ausführliche Statistiken zur Ausführung des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abfrageoptimierers zurück. Diese Sicht können Sie beim Optimieren einer Arbeitsauslastung verwenden, um Probleme oder Verbesserungen bei der Abfrageoptimierung zu identifizieren. Sie können beispielsweise anhand der Gesamtanzahl der Optimierungen, des Wertes für die verstrichene Zeit und des Endkostenwertes die Abfrageoptimierungen der aktuellen Arbeitsauslastung und sämtliche während des Optimierungsvorgangs beobachteten Änderungen vergleichen. Einige Leistungsindikatoren stellen Daten bereit, die nur für die interne Diagnose von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] relevant sind. Diese Leistungsindikatoren sind als "Internal only" gekennzeichnet.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_exec_query_optimizer_info**.  
  
|Name|Datentyp|Beschreibung|  
|----------|---------------|-----------------|  
|**Indikator**|**nvarchar(4000)**|Name des Statistikereignisses des Abfrageoptimierers.|  
|**occurrence**|**bigint**|Anzahl der Vorkommen von Optimierungsereignissen für diesen Leistungsindikator.|  
|**value**|**float**|Durchschnittlicher Eigenschaftswert pro Ereignisvorkommen.|  
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
    
## <a name="remarks"></a>Bemerkungen  
 **sys.dm_exec_query_optimizer_info** enthält die folgenden Eigenschaften (Indikatoren). Alle Vorkommenwerte sind kumulativ und werden beim Neustarten des Systems auf 0 festgelegt. Alle Werte für Wertfelder werden beim Neustarten des Systems auf NULL festgelegt. Alle Wertspaltenwerte, die einen Durchschnitt angeben, verwenden den Vorkommenwert aus derselben Zeile als Nenner bei der Berechnung des Durchschnitts. Alle Abfrage Optimierungen werden gemessen, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Änderungen an **dm_exec_query_optimizer_info** bestimmt, einschließlich Benutzer-und System generierter Abfragen. Durch die Ausführung eines bereits zwischengespeicherten Plans werden Werte in **dm_exec_query_optimizer_info** nicht geändert, nur Optimierungen sind von Bedeutung.  
  
|Leistungsindikator|Vorkommen|Wert|  
|-------------|----------------|-----------|  
|optimizations|Gesamtzahl der Optimierungen.|Nicht zutreffend|  
|elapsed time|Gesamtzahl der Optimierungen.|Durchschnittlich verstrichene Zeit pro Optimierung einer einzelnen Anweisung (Abfrage), in Sekunden.|  
|final cost|Gesamtzahl der Optimierungen.|Durchschnittliche geschätzte Kosten für einen optimierten Plan in internen Kosteneinheiten.|  
|trivial plan|Nur intern|Nur intern|  
|Tasks|Nur intern|Nur intern|  
|no plan|Nur intern|Nur intern|  
|Suche 0|Nur intern|Nur intern|  
|search 0 time|Nur intern|Nur intern|  
|0 Aufgaben suchen|Nur intern|Nur intern|  
|Suche 1|Nur intern|Nur intern|  
|Suche 1 Zeit|Nur intern|Nur intern|  
|Suche 1 Aufgaben|Nur intern|Nur intern|  
|search 2|Nur intern|Nur intern|  
|search 2 time|Nur intern|Nur intern|  
|search 2 tasks|Nur intern|Nur intern|  
|Stufe 0 auf Stufe 1 erhöhen|Nur intern|Nur intern|  
|gain stage 1 to stage 2|Nur intern|Nur intern|  
|timeout|Nur intern|Nur intern|  
|memory limit exceeded|Nur intern|Nur intern|  
|insert stmt|Anzahl der für INSERT-Anweisungen ausgeführten Optimierungen.|Nicht zutreffend|  
|delete stmt|Anzahl der für DELETE-Anweisungen ausgeführten Optimierungen.|Nicht zutreffend|  
|update stmt|Anzahl der für UPDATE-Anweisungen ausgeführten Optimierungen.|Nicht zutreffend|  
|contains subquery|Anzahl der Optimierungen für eine Abfrage, die mindestens eine Unterabfrage enthält.|Nicht zutreffend|  
|unnest failed|Nur intern|Nur intern|  
|Tabellen|Gesamtzahl der Optimierungen.|Gesamtzahl der Tabellen, auf die pro optimierte Abfrage verwiesen wird.|  
|hints|Häufigkeit, mit der ein Hinweis angegeben wurde. Zu diesen Hinweisen gehören die Abfragehinweise JOIN, GROUP, UNION und FORCE ORDER, die SET-Option FORCE PLAN sowie Joinhinweise.|Nicht zutreffend|  
|order hint|Häufigkeit, mit der ein FORCE ORDER-Hinweis angegeben wurde.|Nicht zutreffend|  
|join hint|Häufigkeit, mit der der Joinalgorithmus von einem Joinhinweis erzwungen wurde.|Nicht zutreffend|  
|view reference|Häufigkeit, mit der in einer Abfrage auf eine Sicht verwiesen wurde.|Nicht zutreffend|  
|remote query|Anzahl der Optimierungen, bei denen die Abfrage auf mindestens eine Remotedatenquelle verwiesen hat, wie z. B. auf eine Tabelle mit einem vierteiligen Namen oder ein OPENROWSET-Ergebnis.|Nicht zutreffend|  
|maximum DOP|Gesamtzahl der Optimierungen.|Durchschnittlicher effektiver MAXDOP-Wert für einen optimierten Plan. Standardmäßig wird das effektive MAXDOP durch die Server Konfigurationsoption **Max. Grad an Parallelität** bestimmt und kann für eine bestimmte Abfrage durch den Wert des MAXDOP-Abfrage Hinweises überschrieben werden.|  
|maximum recursion level|Anzahl der Optimierungen, bei denen mit dem Abfragehinweis eine höhere MAXRECURSION-Ebene als 0 angegeben wurde.|Durchschnittliche MAXRECURSION-Ebene in Optimierungen, bei denen mit dem Abfragehinweis eine maximale Rekursionsebene angegeben wurde.|  
|indexed views loaded|Nur intern|Nur intern|  
|indexed views matched|Anzahl der Optimierungen, bei denen für mindestens eine indizierte Sicht eine Übereinstimmung gefunden wurde.|Durchschnittliche Anzahl der übereinstimmenden Sichten.|  
|indexed views used|Anzahl der Optimierungen, bei denen nach dem Abgleich mindestens eine indizierte Sicht im Ausgabeplan verwendet wird.|Durchschnittliche Anzahl der verwendeten Sichten.|  
|indexed views updated|Anzahl der Optimierungen einer DML-Anweisung, die einen Plan erstellen, von dem mindestens eine indizierte Sicht verwaltet wird.|Durchschnittliche Anzahl der verwalteten Sichten.|  
|dynamic cursor request|Anzahl der Optimierungen, in denen eine Anforderung nach dynamischen Cursorn angegeben wurde.|Nicht zutreffend|  
|fast forward cursor request|Anzahl der Optimierungen, in denen eine Anforderung nach schnellen Vorwärtscursorn angegeben wurde.|Nicht zutreffend|  
|merge stmt|Anzahl der für MERGE-Anweisungen ausgeführten Optimierungen.|Nicht zutreffend|  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-viewing-statistics-on-optimizer-execution"></a>A. Anzeigen von Statistiken zur Ausführung von Optimierern  
 Wie sehen die aktuellen Statistiken zur Ausführung des Abfrageoptimierers für diese [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz aus?  
  
```  
SELECT * FROM sys.dm_exec_query_optimizer_info;  
```  
  
### <a name="b-viewing-the-total-number-of-optimizations"></a>B. Anzeigen der Gesamtzahl von Optimierungen  
 Wie viele Optimierungen werden ausgeführt?  
  
```  
SELECT occurrence AS Optimizations FROM sys.dm_exec_query_optimizer_info  
WHERE counter = 'optimizations';  
```  
  
### <a name="c-average-elapsed-time-per-optimization"></a>C. Durchschnittliche verstrichene Zeit pro Optimierung  
 Wie lange dauert eine Optimierung im Durchschnitt?  
  
```  
SELECT ISNULL(value,0.0) AS ElapsedTimePerOptimization  
FROM sys.dm_exec_query_optimizer_info WHERE counter = 'elapsed time';  
```  
  
### <a name="d-fraction-of-optimizations-that-involve-subqueries"></a>D: Anteil der Optimierungen mit Unterabfragen  
 Wie hoch liegt der Anteil der optimierten Abfragen mit einer Unterabfrage?  
  
```  
SELECT (SELECT CAST (occurrence AS float) FROM sys.dm_exec_query_optimizer_info WHERE counter = 'contains subquery') /  
       (SELECT CAST (occurrence AS float)   
        FROM sys.dm_exec_query_optimizer_info WHERE counter = 'optimizations')  
        AS ContainsSubqueryFraction;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Execution Related Dynamic Management Views and Functions &#40;Transact-SQL&#41; (Dynamische Verwaltungssichten und Funktionen im Zusammenhang mit der Ausführung (Transact-SQL))](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


