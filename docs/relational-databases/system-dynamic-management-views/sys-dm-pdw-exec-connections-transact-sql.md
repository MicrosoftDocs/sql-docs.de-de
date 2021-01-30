---
description: sys.dm_pdw_exec_connections (Transact-SQL)
title: sys.dm_pdw_exec_connections (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 2625466b-d0ef-4c71-bedc-6d13491a8351
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 6819fe66d50d839f1a135b33a329d5d9e57dc8e5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99142979"
---
# <a name="sysdm_pdw_exec_connections-transact-sql"></a>sys.dm_pdw_exec_connections (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Gibt Informationen über die zu dieser Instanz von [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] hergestellten Verbindungen zurück, sowie Details zu jeder der Verbindungen.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|session_id|**int**|Identifiziert die Sitzung, die dieser Verbindung zugeordnet ist. Verwenden `SESSION_ID()` Sie, um den `session_id` der aktuellen Verbindung zurückzugeben.|  
|connect_time|**datetime**|Zeitstempel, der angibt, wann die Verbindung eingerichtet wurde. Lässt keine NULL-Werte zu.|  
|encrypt_option|**nvarchar(40)**|Gibt true (die Verbindung ist verschlüsselt) oder false (die Verbindung ist nicht "tctypred").|  
|auth_scheme|**nvarchar(40)**|Gibt das mit dieser Verbindung verwendete [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-/Windows-Authentifizierungsschema an. Lässt keine NULL-Werte zu.|  
|client_id|**varchar(48)**|Die IP-Adresse des Clients, der eine Verbindung mit diesem Server herstellt. Lässt NULL-Werte zu.|  
|sql_spid|**int**|Die Server Prozess-ID der Verbindung. Verwenden `@@SPID` Sie, um den `sql_spid` der aktuellen Verbindung zurückzugeben. Verwenden Sie für den spezifischsten Zweck `session_id` stattdessen den.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die **View Server State** -Berechtigung auf dem Server.  
  
## <a name="relationship-cardinalities"></a>Kardinalität der Beziehungen  
  
| Von | Beschreibung | Beziehung |
| ---- | -- | ------------ |
|dm_pdw_exec_sessions dm_pdw_exec_sessions.session_id|dm_pdw_exec_connections dm_pdw_exec_connections.session_id|1:1|  
|dm_pdw_exec_requests dm_pdw_exec_requests.connection_id|dm_pdw_exec_connections dm_pdw_exec_connections.connection_id|n:1|  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Typische Abfrage zum Sammeln von Informationen über die eigene Verbindung einer Abfrage.  
  
```  
SELECT  
    c.session_id, c.encrypt_option,  
    c.auth_scheme, s.client_id, s.login_name,   
    s.status, s.query_count  
FROM sys.dm_pdw_exec_connections AS c  
JOIN sys.dm_pdw_exec_sessions AS s  
    ON c.session_id = s.session_id  
WHERE c.session_id = SESSION_ID();  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Azure Synapse Analytics und parallele Data Warehouse dynamische Verwaltungs Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  

