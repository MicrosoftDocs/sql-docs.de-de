---
description: sys.dm_exec_connections (Transact-SQL)
title: sys.dm_exec_connections (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_connections_TSQL
- sys.dm_exec_connections_TSQL
- sys.dm_exec_connections
- dm_exec_connections
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_connections dynamic management view
ms.assetid: 6bd46fe1-417d-452d-a9e6-5375ee8690d8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2f56d5b39cd4c0c303ef271d08c9521c3004a19d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839326"
---
# <a name="sysdm_exec_connections-transact-sql"></a>sys.dm_exec_connections (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt Informationen über die zu dieser Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hergestellten Verbindungen zurück, sowie Details zu jeder der Verbindungen. Gibt Server weite Verbindungsinformationen für SQL Server zurück. Gibt aktuelle Daten bankverbindungs Informationen für die SQL-Datenbank zurück.  
  
> [!NOTE]
> [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] Verwenden Sie [sys.dm_pdw_exec_connections &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-connections-transact-sql.md), um dies von oder aus aufzurufen.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|session_id|**int**|Identifiziert die Sitzung, die dieser Verbindung zugeordnet ist. Lässt NULL-Werte zu.|  
|most_recent_session_id|**int**|Stellt die Sitzungs-ID für die letzte Anforderung dar, die dieser Verbindung zugeordnet ist. (SOAP-Verbindungen können von einer anderen Sitzung wieder verwendet werden.) Lässt NULL-Werte zu.|  
|connect_time|**datetime**|Zeitstempel, der angibt, wann die Verbindung eingerichtet wurde. Lässt keine NULL-Werte zu.|  
|net_transport|**nvarchar(40)**|Gibt immer eine **Sitzung** zurück, wenn für eine Verbindung Multiple Active Result Sets (Mars) aktiviert ist.<br /><br /> **Hinweis:** Beschreibt das physische Transportprotokoll, das von dieser Verbindung verwendet wird. Lässt keine NULL-Werte zu.|  
|protocol_type|**nvarchar(40)**|Gibt den Protokolltyp der Nutzlast an. Zurzeit wird zwischen TDS (TSQL) und SOAP unterschieden. Lässt NULL-Werte zu.|  
|protocol_version|**int**|Die Version des Datenzugriffsprotokolls, das dieser Verbindung zugeordnet ist. Lässt NULL-Werte zu.|  
|endpoint_id|**int**|Ein Bezeichner, der beschreibt, um welchen Verbindungstyp es sich handelt. Dieser endpoint_id-Wert kann zum Abfragen der sys.endpoint-Sicht verwendet werden. Lässt NULL-Werte zu.|  
|encrypt_option|**nvarchar(40)**|Boolescher Wert, der angibt, ob die Verschlüsselung für diese Verbindung aktiviert ist. Lässt keine NULL-Werte zu.|  
|auth_scheme|**nvarchar(40)**|Gibt das mit dieser Verbindung verwendete [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-/Windows-Authentifizierungsschema an. Lässt keine NULL-Werte zu.|  
|node_afinity|**smallint**|Identifiziert den Speicherknoten, zu dem diese Verbindung eine Affinität besitzt. Lässt keine NULL-Werte zu.|  
|num_reads|**int**|Anzahl von Byte Lesevorgängen, die über diese Verbindung aufgetreten sind. Lässt NULL-Werte zu.|  
|num_writes|**int**|Anzahl von Byte Schreibvorgängen, die über diese Verbindung aufgetreten sind. Lässt NULL-Werte zu.|  
|last_read|**datetime**|Zeitstempel für den letzten Lesevorgang, der über diese Verbindung erfolgt ist. Lässt NULL-Werte zu.|  
|last_write|**datetime**|Zeitstempel für den letzten Schreibvorgang, der über diese Verbindung erfolgt ist. Lässt keine NULL-Werte zu.|  
|net_packet_size|**int**|Netzwerkpaketgröße, die für die Informations- und Datenübertragung verwendet wird. Lässt NULL-Werte zu.|  
|client_net_address|**varchar(48)**|Hostadresse des Clients, der die Verbindung mit diesem Server herstellt. Lässt NULL-Werte zu.<br /><br /> In Vorgängerversionen von V12 in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], gibt diese Spalte immer NULL zurück.|  
|client_tcp_port|**int**|Portnummer auf dem Clientcomputer, die dieser Verbindung zugeordnet ist. Lässt NULL-Werte zu.<br /><br /> In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] gibt diese Spalte immer NULL zurück.|  
|local_net_address|**varchar(48)**|Stellt die IP-Adresse auf dem Server dar, die die Zieladresse dieser Verbindung ist. Ist nur für Verbindungen verfügbar, die den TCP-Transportanbieter verwenden. Lässt NULL-Werte zu.<br /><br /> In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] gibt diese Spalte immer NULL zurück.|  
|local_tcp_port|**int**|Stellt den Server-TCP-Port dar, der der Zielport dieser Verbindung ist, falls die Verbindung den TCP-Transport verwendet. Lässt NULL-Werte zu.<br /><br /> In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] gibt diese Spalte immer NULL zurück.|  
|connection_id|**uniqueidentifier**|Dient zur eindeutigen Identifizierung jeder Verbindung. Lässt keine NULL-Werte zu.|  
|parent_connection_id|**uniqueidentifier**|Identifiziert die primäre Verbindung, die von der MARS-Sitzung verwendet wird. Lässt NULL-Werte zu.|  
|most_recent_sql_handle|**varbinary(64)**|Das SQL-Handle der letzten Anforderung, die über diese Verbindung ausgeführt wurde. Die most_recent_sql_handle-Spalte ist immer mit der most_recent_session_id-Spalte synchronisiert. Lässt NULL-Werte zu.|  
|pdw_node_id|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   

## <a name="physical-joins"></a>Physische Joins  
 ![Joins für sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/media/join-dm-exec-connections-1.gif "Joins für sys.dm_exec_connections")  
  
## <a name="relationship-cardinalities"></a>Kardinalität der Beziehungen  
  
| First-Element | Zweites Element | Relationship |
| --------------| -------------- | ------------ |  
|dm_exec_sessions.session_id|dm_exec_connections.session_id|1:1|  
|dm_exec_requests.connection_id|dm_exec_connections.connection_id|n:1|  
|dm_broker_connections.connection_id|dm_exec_connections.connection_id|1:1|  
  
## <a name="examples"></a>Beispiele  
 Typische Abfrage zum Sammeln von Informationen über die eigene Verbindung einer Abfrage.  
  
```sql  
SELECT   
    c.session_id, c.net_transport, c.encrypt_option,   
    c.auth_scheme, s.host_name, s.program_name,   
    s.client_interface_name, s.login_name, s.nt_domain,   
    s.nt_user_name, s.original_login_name, c.connect_time,   
    s.login_time   
FROM sys.dm_exec_connections AS c  
JOIN sys.dm_exec_sessions AS s  
    ON c.session_id = s.session_id  
WHERE c.session_id = @@SPID;  
```  
  
## <a name="see-also"></a>Weitere Informationen  

 [Execution Related Dynamic Management Views and Functions &#40;Transact-SQL&#41; (Dynamische Verwaltungssichten und Funktionen im Zusammenhang mit der Ausführung (Transact-SQL))](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
