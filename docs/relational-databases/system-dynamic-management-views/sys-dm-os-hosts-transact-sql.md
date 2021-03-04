---
description: sys.dm_os_hosts (Transact-SQL)
title: sys.dm_os_hosts (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_hosts_TSQL
- dm_os_hosts
- dm_os_hosts_TSQL
- sys.dm_os_hosts
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_hosts dynamic management view
ms.assetid: a313ff3b-1fe9-421e-b94b-cea19c43b0e5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d82b85a23c67979e8f94a9358d0175d21193dc15
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837594"
---
# <a name="sysdm_os_hosts-transact-sql"></a>sys.dm_os_hosts (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt alle Hosts zurück, die zurzeit in einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] registriert sind. Diese Sicht gibt auch die von diesen Hosts verwendeten Ressourcen zurück.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_hosts**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**host_address**|**varbinary(8)**|Die interne Speicheradresse des Hostobjekts.|  
|**type**|**nvarchar(60)**|Der Typ der gehosteten Komponente. Beispiel:<br /><br /> SOSHOST_CLIENTID_SERVERSNI= SQL Server Native Interface<br /><br /> SOSHOST_CLIENTID_SQLOLEDB = SQL Server Native Client OLE DB Provider<br /><br /> SOSHOST_CLIENTID_MSDART = Microsoft Data Access Run Time|  
|**name**|**nvarchar(32)**|Der Name des Hosts.|  
|**enqueued_tasks_count**|**int**|Gesamtanzahl der Tasks, die von diesem Host in Warteschlangen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] platziert wurden.|  
|**active_tasks_count**|**int**|Gesamtanzahl der aktuell ausgeführten Tasks, die von diesem Host in Warteschlangen platziert wurden.|  
|**completed_ios_count**|**int**|Gesamtanzahl der E/A-Vorgänge, die über diesen Host ausgegeben und abgeschlossen wurden.|  
|**completed_ios_in_bytes**|**bigint**|Gesamtanzahl von Bytes der E/A-Vorgänge, die über diesen Host abgeschlossen wurden.|  
|**active_ios_count**|**int**|Gesamtanzahl von E/A-Anforderungen in Verbindung mit diesem Host, die zurzeit auf Beendigung warten.|  
|**default_memory_clerk_address**|**varbinary(8)**|Speicheradresse des diesem Host zugeordneten Arbeitsspeicherclerk-Objekts. Weitere Informationen finden Sie unter [sys.dm_os_memory_clerks &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).|  
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   

## <a name="remarks"></a>Bemerkungen  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lässt Komponenten, z. B. einen OLE DB-Anbieter, die nicht Teil der ausführbaren Datei von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sind, Arbeitsspeicher belegen und an nicht präemptiven Planungen teilnehmen. Diese Komponenten werden von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gehostet, und alle von diesen Komponenten zugeordneten Ressourcen werden nachverfolgt. Als Host kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Ressourcen besser berücksichtigen, die von Komponenten außerhalb der ausführbaren Datei von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden.  
  
## <a name="relationship-cardinalities"></a>Kardinalität der Beziehungen  
  
|Von|Beschreibung|Relationship|  
|----------|--------|------------------|  
|sys.dm_os_hosts. default_memory_clerk_address|sys.dm_os_memory_clerks. memory_clerk_address|1:1|  
|sys.dm_os_hosts. host_address|sys.dm_os_memory_clerks. host_address|1:1|  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der Gesamtumfang des Arbeitsspeichers, für den von einer gehosteten Komponente ein Commit ausgeführt wurde, bestimmt.  
  
||  
|-|  
|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.|  
  
```  
SELECT h.type, SUM(mc.pages_kb) AS commited_memory  
FROM sys.dm_os_memory_clerks AS mc   
INNER JOIN sys.dm_os_hosts AS h   
    ON mc.memory_clerk_address = h.default_memory_clerk_address  
GROUP BY h.type;  
```  
  
## <a name="see-also"></a>Weitere Informationen  

 [sys.dm_os_memory_clerks &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)   
 [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
