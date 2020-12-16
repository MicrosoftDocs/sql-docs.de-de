---
title: Feststellen, welche Abfragen Sperren enthalten
description: In diesem Artikel wird eine Methode veranschaulicht, mit der Sie ermitteln können, welche Abfrage eine Sperre enthält. Datenbankadministratoren müssen möglicherweise die Quelle der Sperren ermitteln, die die Datenbankleistung beeinträchtigen.
ms.date: 10/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- queries [SQL Server], extended events
- queries [SQL Server], holding locks
- xe
- extended events [SQL Server], locks
- extended events [SQL Server], holding locks
ms.assetid: bdfce092-3cf1-4b5e-99d5-fd8c6f9ad560
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e1e24e408df936fc2a651263218896e4c96826e7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465611"
---
# <a name="determine-which-queries-are-holding-locks"></a>Feststellen, welche Abfragen Sperren enthalten

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Datenbankadministratoren müssen oft die Quelle von Sperren identifizieren, die die Datenbankleistung beeinträchtigen.  
  
Sie vermuten z. B., dass eine Blockierung ein Leistungsproblem am Server verursacht. Bei der Abfrage von sys.dm_exec_requests befinden sich mehrere Sitzungen im angehaltenen Modus, wobei der jeweilige Wartetyp darauf hinweist, dass es sich bei der Ressource, auf die gewartet wird, um eine Sperre handelt.  
  
Bei der Abfrage von sys.dm_tran_locks zeigen die Ergebnisse, dass viele Sperren ausstehen. Für die Sitzungen, denen die Sperren erteilt wurden, werden jedoch keine aktiven Anforderungen in sys.dm_exec_requests angezeigt.  
  
In diesem Beispiel wird eine Möglichkeit gezeigt, wie Sie die Abfrage, die die Sperre aufgenommen hat, den Abfrageplan und den [!INCLUDE[tsql](../../includes/tsql-md.md)] -Stapel zum Zeitpunkt, als die Sperre bewirkt wurde, bestimmen können. Darüber hinaus veranschaulicht dieses Beispiel, wie das Paarbildungsziel in einer Extended Events-Sitzung verwendet wird.  
  
Um diese Aufgabe auszuführen, müssen Sie mit dem Abfrage-Editor in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] den folgenden Vorgang durchführen.  
  
> [!NOTE]  
>  In diesem Beispiel wird die AdventureWorks-Datenbank verwendet.  
  
### <a name="to-determine-which-queries-are-holding-locks"></a>So stellen Sie fest, welche Abfragen Sperren enthalten  
  
1.  Führen Sie im Abfrage-Editor die folgenden Anweisungen aus.  
  
    ```sql
    -- Perform cleanup.   
    IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='FindBlockers')  
        DROP EVENT SESSION FindBlockers ON SERVER  
    GO  
    -- Use dynamic SQL to create the event session and allow creating a -- predicate on the AdventureWorks database id.  
    --  
    DECLARE @dbid int  
  
    SELECT @dbid = db_id('AdventureWorks')  
  
    IF @dbid IS NULL  
    BEGIN  
        RAISERROR('AdventureWorks is not installed. Install AdventureWorks before proceeding', 17, 1)  
        RETURN  
    END  
  
    DECLARE @sql nvarchar(1024)  
    SET @sql = '  
    CREATE EVENT SESSION FindBlockers ON SERVER  
    ADD EVENT sqlserver.lock_acquired   
        (action   
            ( sqlserver.sql_text, sqlserver.database_id, sqlserver.tsql_stack,  
             sqlserver.plan_handle, sqlserver.session_id)  
        WHERE ( database_id=' + cast(@dbid as nvarchar) + ' AND resource_0!=0)   
        ),  
    ADD EVENT sqlserver.lock_released   
        (WHERE ( database_id=' + cast(@dbid as nvarchar) + ' AND resource_0!=0 ))  
    ADD TARGET package0.pair_matching   
        ( SET begin_event=''sqlserver.lock_acquired'',   
                begin_matching_columns=''database_id, resource_0, resource_1, resource_2, transaction_id, mode'',   
                end_event=''sqlserver.lock_released'',   
                end_matching_columns=''database_id, resource_0, resource_1, resource_2, transaction_id, mode'',  
        respond_to_memory_pressure=1)  
    WITH (max_dispatch_latency = 1 seconds)'  
  
    EXEC (@sql)  
    --   
    -- Create the metadata for the event session  
    -- Start the event session  
    --  
    ALTER EVENT SESSION FindBlockers ON SERVER  
    STATE = START  
    ```  
  
2.  Nach Ausführung einer Arbeitsauslastung auf dem Server geben Sie die folgenden Anweisungen im Abfrage-Editor aus, um die Abfragen zu ermitteln, die noch Sperren enthalten.  
  
    ```sql
    --  
    -- The pair matching targets report current unpaired events using   
    -- the sys.dm_xe_session_targets dynamic management view (DMV)  
    -- in XML format.  
    -- The following query retrieves the data from the DMV and stores  
    -- key data in a temporary table to speed subsequent access and  
    -- retrieval.  
    --  
    SELECT   
    objlocks.value('(action[@name="session_id"]/value)[1]', 'int')  
            AS session_id,  
        objlocks.value('(data[@name="database_id"]/value)[1]', 'int')   
            AS database_id,  
        objlocks.value('(data[@name="resource_type"]/text)[1]', 'nvarchar(50)' )   
            AS resource_type,  
        objlocks.value('(data[@name="resource_0"]/value)[1]', 'bigint')   
            AS resource_0,  
        objlocks.value('(data[@name="resource_1"]/value)[1]', 'bigint')   
            AS resource_1,  
        objlocks.value('(data[@name="resource_2"]/value)[1]', 'bigint')   
            AS resource_2,  
        objlocks.value('(data[@name="mode"]/text)[1]', 'nvarchar(50)')   
            AS mode,  
        objlocks.value('(action[@name="sql_text"]/value)[1]', 'varchar(MAX)')   
            AS sql_text,  
        CAST(objlocks.value('(action[@name="plan_handle"]/value)[1]', 'varchar(MAX)') AS xml)   
            AS plan_handle,      
        CAST(objlocks.value('(action[@name="tsql_stack"]/value)[1]', 'varchar(MAX)') AS xml)   
            AS tsql_stack  
    INTO #unmatched_locks  
    FROM (  
        SELECT CAST(xest.target_data as xml)   
            lockinfo  
        FROM sys.dm_xe_session_targets xest  
        JOIN sys.dm_xe_sessions xes ON xes.address = xest.event_session_address  
        WHERE xest.target_name = 'pair_matching' AND xes.name = 'FindBlockers'  
    ) heldlocks  
    CROSS APPLY lockinfo.nodes('//event[@name="lock_acquired"]') AS T(objlocks)  
  
    --  
    -- Join the data acquired from the pairing target with other   
    -- DMVs to return provide additional information about blockers  
    --  
    SELECT ul.*  
        FROM #unmatched_locks ul  
        INNER JOIN sys.dm_tran_locks tl ON ul.database_id = tl.resource_database_id AND ul.resource_type = tl.resource_type  
        WHERE resource_0 IS NOT NULL  
        AND session_id IN   
            (SELECT blocking_session_id FROM sys.dm_exec_requests WHERE blocking_session_id != 0)  
        AND tl.request_status='wait'  
        AND REPLACE(ul.mode, 'LCK_M_', '' ) = tl.request_mode  
  
    ```  
  
3.  Löschen Sie nach dem Identifizieren der Probleme alle temporären Tabellen und die Ereignissitzung.  
  
    ```sql
    DROP TABLE #unmatched_locks  
    DROP EVENT SESSION FindBlockers ON SERVER  
    ```  

> [!NOTE]
> Die vorangehenden Transact-SQL-Codebeispiele werden lokal auf SQL Server ausgeführt, können jedoch auf _Azure SQL-Datenbank nicht ordnungsgemäß ausgeführt werden._ Die Kernteile des Beispiels, die direkt Ereignisse wie `ADD EVENT sqlserver.lock_acquired` einbeziehen, funktionieren ebenfalls auf Azure SQL-Datenbank. Vorläufige Elemente wie `sys.server_event_sessions` müssen jedoch für die Azure SQL-Datenbank-Pendats wie `sys.database_event_sessions` angepasst werden, damit das Beispiel ausgeführt werden kann.
> Weitere Informationen zu diesen kleinen Unterschieden zwischen einer lokalen SQL Server-Instanz und Azure SQL-Datenbank finden Sie in den folgenden Artikeln:
> - [Erweiterte Ereignisse in Azure SQL-Datenbank](/azure/sql-database/sql-database-xevent-db-diff-from-svr#transact-sql-differences)
> - [Systemobjekte, die erweiterte Ereignisse unterstützen](xevents-references-system-objects.md)

## <a name="see-also"></a>Weitere Informationen  
 [CREATE EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md)   
 [ALTER EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-event-session-transact-sql.md)   
 [DROP EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-event-session-transact-sql.md)   
 [sys.dm_xe_session_targets &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-targets-transact-sql.md)   
 [sys.dm_xe_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-sessions-transact-sql.md)  
  
  
