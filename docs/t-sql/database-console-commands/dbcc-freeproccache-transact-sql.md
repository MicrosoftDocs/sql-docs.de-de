---
description: DBCC FREEPROCCACHE (Transact-SQL)
title: DBCC FREEPROCCACHE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/13/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FREEPROCCACHE_TSQL
- FREEPROCCACHE
- DBCC_FREEPROCCACHE_TSQL
- DBCC FREEPROCCACHE
dev_langs:
- TSQL
helpviewer_keywords:
- freeing procedure cache
- removing procedure cache elements
- deleting procedure cache elements
- DBCC FREEPROCCACHE statement
- procedure cache [SQL Server]
- clearing procedure cache
ms.assetid: 0e09d210-6f23-4129-aedb-3d56b2980683
author: pmasl
ms.author: umajay
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c408be9f8b352a0f6fe333be31956b6860da3907
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342634"
---
# <a name="dbcc-freeproccache-transact-sql"></a>DBCC FREEPROCCACHE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Entfernt alle Elemente aus dem Plancache, entfernt einen bestimmten Plan aus dem Plancache durch Angabe eines Planhandles oder SQL-Handles oder entfernt alle einem angegebenen Ressourcenpool zugeordneten Cacheinträge.

> [!NOTE]
> Die Ausführungsstatistik für systemintern kompilierte gespeicherte Prozeduren wird durch DBCC FREEPROCCACHE nicht gelöscht. Der Prozedurcache enthält keine Informationen zu systemintern kompilierten gespeicherten Prozeduren. Jede Ausführungsstatistik, die aus Prozedurausführungen erfasst wird, wird in den DMVs der Ausführungsstatistik angezeigt: [sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md) und [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md).  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
Syntax für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:

```sql
DBCC FREEPROCCACHE [ ( { plan_handle | sql_handle | pool_name } ) ] [ WITH NO_INFOMSGS ]  
```  

Syntax für [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]:
  
```sql
DBCC FREEPROCCACHE [ ( COMPUTE | ALL ) ] 
     [ WITH NO_INFOMSGS ]   
[;]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 ( { *plan_handle* | *sql_handle* | *pool_name* } )  
*plan_handle* führt eine eindeutige Identifizierung eines Abfrageplans für einen ausgeführten Batch aus, dessen Plan sich im Plancache befindet. *plan_handle* ist vom Datentyp **varbinary(64)** und kann von den folgenden dynamischen Verwaltungsobjekten abgerufen werden:  
 -   [sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
 -   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
 -   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  
 -   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  

*sql_handle* das SQL-Handle für den zu löschenden Batch. *sql_handle* ist vom Datentyp **varbinary(64)** und kann von den folgenden dynamischen Verwaltungsobjekten abgerufen werden:  
 -   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
 -   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
 -   [sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)  
 -   [sys.dm_exec_xml_handles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)  
 -   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  

*pool_name* ist der Name eines Ressourcenpools für die Ressourcenkontrolle. *pool_name* ist vom Datentyp **sysname** und kann durch Abfragen der dynamischen Verwaltungssicht [sys.dm_resource_governor_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md) abgerufen werden.  
 Fragen Sie die dynamische Verwaltungssicht [sys.dm_resource_governor_workload_groups](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md) ab, um einem Ressourcenpool eine Arbeitsauslastungsgruppe zur Ressourcenkontrolle zuzuordnen. Informationen über die Arbeitsauslastungsgruppe für eine Sitzung können Sie über die dynamische Verwaltungssicht [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md) abfragen.  

  
 WITH NO_INFOMSGS  
 Alle Informationsmeldungen werden unterdrückt.  
  
 COMPUTE  
 Alle Serverknoten werden aus dem Abfrageplancache gelöscht. Dies ist der Standardwert.  
  
 ALL  
 Alle Computerknoten und Steuerknoten werden aus dem Abfrageplancache gelöscht.  

> [!NOTE]
> Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] ist `ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE` verfügbar, um den Prozedur-/Plancache für die Datenbank im Bereich zu löschen.

## <a name="remarks"></a>Hinweise  
Verwenden Sie DBCC FREEPROCCACHE, um den Plancache sorgfältig zu leeren. Durch das Löschen des Prozedur-/Plancaches werden alle Pläne entfernt, und eingehende Abfrageausführungen werden mit einem neuen Plan kompiliert, statt dass alle zuvor zwischengespeicherten Pläne wieder verwendet werden. 

Bei steigender Anzahl von Kompilierungen kann es zu einer plötzlichen, vorübergehenden Abnahme der Abfrageleistung kommen. Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll enthält für jeden geleerten Cachespeicher im Plancache folgende Infomeldung:

> `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to 'DBCC FREEPROCCACHE' or 'DBCC FREESYSTEMCACHE' operations.` 

Diese Meldung wird alle fünf Minuten protokolliert, solange der Cache innerhalb dieses Zeitintervalls geleert wird.

Der Prozedurcache wird außerdem durch die folgenden Neukonfigurierungsvorgänge gelöscht:
-   access check cache bucket count  
-   access check cache quota  
-   clr enabled  
-   cost threshold for parallelism  
-   cross db ownership chaining  
-   index create memory  
-   max degree of parallelism  
-   Max. Serverarbeitspeicher  
-   max text repl size  
-   max worker threads  
-   Min. Arbeitsspeicher pro Abfrage  
-   Min. Serverarbeitsspeicher  
-   query governor cost limit  
-   query wait  
-   remote query timeout  
-   user options  
  
## <a name="result-sets"></a>Resultsets  
Wenn die WITH NO_INFOMSGS-Klausel nicht angegeben ist, gibt DBCC FREEPROCCACHE folgende Meldung zurück: „Die DBCC-Ausführung wurde abgeschlossen. Falls DBCC Fehlermeldungen ausgegeben hat, wenden Sie sich an den Systemadministrator."
  
## <a name="permissions"></a>Berechtigungen  
Gilt für: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 
- Erfordert die ALTER SERVER STATE-Berechtigung auf dem Server.  

Gilt für: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]
- Erfordert die Mitgliedschaft in der festen Serverrolle DB_OWNER.  

## <a name="general-remarks-for-sssdw-and-sspdw"></a>Allgemeine Hinweise zu [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Viele DBCC FREEPROCCACHE-Befehle können gleichzeitig ausgeführt werden.
In [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] oder [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] kann das Löschen des Plancaches eine vorübergehende Abnahme der Abfrageleistung verursachen, da eingehende Abfragen einen neuen Plan kompilieren, statt einen zuvor zwischengespeicherten Plan zu verwenden. 

DBCC FREEPROCCACHE (COMPUTE) bewirkt nur, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Abfragen neu kompiliert werden, wenn sie auf den Compute-Knoten ausgeführt werden. Es führt nicht dazu, dass [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] oder [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] den parallelen Abfrageplan kompilieren, der auf dem Control-Knoten generiert wird.
DBCC FREEPROCCACHE kann während der Ausführung abgebrochen werden.
  
## <a name="limitations-and-restrictions-for-sssdw-and-sspdw"></a>Einschränkungen für [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
DBCC FREEPROCCACHE kann nicht ohne eine Transaktion ausgeführt werden.
DBCC FREEPROCCACHE wird in EXPLAIN-Anweisungen nicht unterstützt.
  
## <a name="metadata-for-sssdw-and-sspdw"></a>Metadaten für [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Der Systemsicht sys.pdw_exec_requests wird eine neue Zeile hinzugefügt, wenn DBCC FREEPROCCACHE ausgeführt wird.

## <a name="examples-ssnoversion"></a>Beispiele: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
### <a name="a-clearing-a-query-plan-from-the-plan-cache"></a>A. Löschen eines Abfrageplans im Plancache  
Im folgenden Beispiel wird ein Abfrageplan im Plancache gelöscht, indem das Abfrageplanhandle angegeben wird. Um sicherzustellen, dass die Beispielabfrage im Plancache vorhanden ist, wird die Abfrage zuerst ausgeführt. Durch Abfrage der dynamischen Verwaltungssichten `sys.dm_exec_cached_plans` und `sys.dm_exec_sql_text` wird das Planhandle für die Abfrage ermittelt. 

Der Planhandlewert aus dem Resultset wird dann in die `DBCC FREEPROCACHE`-Anweisung eingefügt, sodass nur dieser Plan aus dem Plancache entfernt wird.
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT * FROM Person.Address;  
GO  
SELECT plan_handle, st.text  
FROM sys.dm_exec_cached_plans   
CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS st  
WHERE text LIKE N'SELECT * FROM Person.Address%';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
plan_handle                                         text  
--------------------------------------------------  -----------------------------  
0x060006001ECA270EC0215D05000000000000000000000000  SELECT * FROM Person.Address;  
  
(1 row(s) affected)
 ```
 
```sql  
-- Remove the specific plan from the cache.  
DBCC FREEPROCCACHE (0x060006001ECA270EC0215D05000000000000000000000000);  
GO  
```  
  
### <a name="b-clearing-all-plans-from-the-plan-cache"></a>B. Löschen aller Pläne im Plancache  
Im folgenden Beispiel werden alle Elemente im Plancache gelöscht. Die WITH `NO_INFOMSGS`-Klausel wird angegeben, um die Anzeige der Informationsmeldung zu verhindern.
  
```sql  
DBCC FREEPROCCACHE WITH NO_INFOMSGS;  
```  
  
### <a name="c-clearing-all-cache-entries-associated-with-a-resource-pool"></a>C. Löschen aller einem Ressourcenpool zugeordneten Cacheeinträge  
Im folgenden Beispiel werden alle einem angegebenen Ressourcenpool zugeordneten Cacheeinträge gelöscht. Zunächst wird die `sys.dm_resource_governor_resource_pools`-Sicht abgefragt, um den Wert für *pool_name* zu erhalten.
  
```sql  
SELECT * FROM sys.dm_resource_governor_resource_pools;  
GO  
DBCC FREEPROCCACHE ('default');  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-dbcc-freeproccache-basic-syntax-examples"></a>D: Beispiele für die grundlegende DBCC FREEPROCCACHE-Syntax  
Im folgende Beispiel werden alle vorhandenen Abfrageplancaches aus den Compute-Knoten entfernt. Obwohl der Kontext auf „UserDbSales“ festgelegt ist, werden die Abfrageplan-Caches des Compute-Knotens für alle Datenbanken entfernt. Die WITH NO_INFOMSGS-Klausel verhindert, dass Informationsmeldungen in den Ergebnissen angezeigt werden.  
  
```sql
USE UserDbSales;  
DBCC FREEPROCCACHE (COMPUTE) WITH NO_INFOMSGS;
```  
  
 Das folgende Beispiel ergibt dieselben Ergebnisse wie im vorherigen Beispiel, außer dass die Ergebnisse Informationsmeldungen enthalten.  
  
```sql
USE UserDbSales;  
DBCC FREEPROCCACHE (COMPUTE);  
```  
  
Wenn Informationsmeldungen angefordert werden und die Ausführung erfolgreich ist, enthalten die Abfrageergebnisse eine Zeile pro Compute-Knoten.
  
### <a name="e-granting-permission-to-run-dbcc-freeproccache"></a>E. Erteilen von Berechtigungen zum Ausführen von DBCC FREEPROCCACHE  
Im folgenden Beispiel wird dem Benutzer David die Berechtigung zum Auszuführen von DBCC FREEPROCCACHE erteilt.  
  
```sql
GRANT ALTER SERVER STATE TO David; 
GO
```  
  
## <a name="see-also"></a>Weitere Informationen  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[Ressourcenkontrolle](../../relational-databases/resource-governor/resource-governor.md)  
[ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)
  
  
