---
description: sys.dm_os_buffer_descriptors (Transact-SQL)
title: sys.dm_os_buffer_descriptors (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_buffer_descriptors_TSQL
- dm_os_buffer_descriptors_TSQL
- sys.dm_os_buffer_descriptors
- dm_os_buffer_descriptors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_buffer_descriptors dynamic management view
ms.assetid: 012aab95-8888-4f35-9ea3-b5dff6e3f60f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 35439ff7ac5806225969378c88eaa6fde92df896
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839519"
---
# <a name="sysdm_os_buffer_descriptors-transact-sql"></a>sys.dm_os_buffer_descriptors (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt Informationen zu allen Datenseiten zurück, die derzeit im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Pufferpool sind. Die Ausgabe dieser Sicht kann verwendet werden, um die Verteilung der Datenbankseiten im Pufferpool gemäß der Datenbank, des Objekts oder des Typs zu bestimmen. In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] gibt diese dynamische Verwaltungssicht auch Informationen über die Datenseiten in der Pufferpoolerweiterungsdatei zurück. Weitere Informationen finden Sie unter [Puffer Pool Erweiterung](../../database-engine/configure-windows/buffer-pool-extension.md).  
  
 Beim Lesen einer Datenseite vom Datenträger wird die Seite in den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Pufferpool kopiert und für die Wiederverwendung zwischengespeichert. Jede zwischengespeicherte Datenseite verfügt über einen Pufferdeskriptor. Pufferdeskriptoren identifizieren jede Datenseite eindeutig, die derzeit in einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zwischengespeichert ist. sys.dm_os_buffer_descriptors gibt zwischengespeicherte Seiten für alle Benutzer- und Systemdatenbanken zurück. Dazu zählen auch Seiten, die der Ressourcendatenbank zugeordnet sind.  
  
> **Hinweis:** Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_buffer_descriptors**.  

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID der Datenbank, die der Seite im Pufferpool zugeordnet ist. Lässt NULL-Werte zu.|  
|file_id|**int**|ID der Datei, die das persistente Image der Seite speichert. Lässt NULL-Werte zu.|  
|page_id|**int**|ID der Seite innerhalb der Datei. Lässt NULL-Werte zu.|  
|page_level|**int**|Indexebene der Seite. Lässt NULL-Werte zu.|  
|allocation_unit_id|**bigint**|ID der Zuordnungseinheit der Seite. Dieser Wert kann für den Join mit sys.allocation_units verwendet werden. Lässt NULL-Werte zu.|  
|page_type|**nvarchar(60)**|Typ der Seite, z. B. Datenseite oder Indexseite. Lässt NULL-Werte zu.|  
|row_count|**int**|Anzahl der Zeilen auf der Seite. Lässt NULL-Werte zu.|  
|free_space_in_bytes|**int**|Umfang des verfügbaren Speicherplatzes auf der Seite (in Byte). Lässt NULL-Werte zu.|  
|is_modified|**bit**|1 = Seite wurde nach dem Lesen vom Datenträger geändert. Lässt NULL-Werte zu.|  
|numa_node|**int**|NUMA-Knoten (Non-Uniform Memory Access) für den Puffer. Lässt NULL-Werte zu.|  
|read_microsec|**bigint**|Die tatsächliche Zeit (in Mikrosekunden), die erforderlich ist, um die Seite in den Puffer einzulesen. Diese Zahl wird zurückgesetzt, wenn der Puffer wiederverwendet wird. Lässt NULL-Werte zu.|  
|is_in_bpool_extension|**bit**|1 = die Seite befindet sich in der Pufferpool Erweiterung. Lässt NULL-Werte zu.|  
|pdw_node_id|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
   
## <a name="remarks"></a>Bemerkungen  
 sys.dm_os_buffer_descriptors gibt Seiten zurück, die von der Ressourcendatenbank verwendet werden. sys.dm_os_buffer_descriptors gibt keine Informationen zu freien oder gestohlenen Seiten zurück oder zu Seiten, die beim Lesen Fehler enthielten.  
  
|Von|Beschreibung|Andererseits|Relationship|  
|----------|--------|--------|------------------|  
|sys.dm_os_buffer_descriptors|sys.databases|database_id|n:1|  
|sys.dm_os_buffer_descriptors|\<userdb>.sys.allocation_units|allocation_unit_id|n:1|  
|sys.dm_os_buffer_descriptors|\<userdb>.sys.database_files|file_id|n:1|  
|sys.dm_os_buffer_descriptors|sys.dm_os_buffer_pool_extension_configuration|file_id|n:1|  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-returning-cached-page-count-for-each-database"></a>A. Zurückgeben der zwischengespeicherten Seitenanzahl für jede Datenbank  
 Im folgenden Beispiel wird die für jede Datenbank geladene Seitenanzahl zurückgegeben.  
  
```  
SELECT COUNT(*)AS cached_pages_count  
    ,CASE database_id   
        WHEN 32767 THEN 'ResourceDb'   
        ELSE db_name(database_id)   
        END AS database_name  
FROM sys.dm_os_buffer_descriptors  
GROUP BY DB_NAME(database_id) ,database_id  
ORDER BY cached_pages_count DESC;  
```  
  
### <a name="b-returning-cached-page-count-for-each-object-in-the-current-database"></a>B. Zurückgeben der zwischengespeicherten Seitenanzahl für jedes Objekt in der aktuellen Datenbank  
 Im folgenden Beispiel wird die für jedes Objekt in der aktuellen Datenbank geladene Seitenanzahl zurückgegeben.  
  
```  
SELECT COUNT(*)AS cached_pages_count   
    ,name ,index_id   
FROM sys.dm_os_buffer_descriptors AS bd   
    INNER JOIN   
    (  
        SELECT object_name(object_id) AS name   
            ,index_id ,allocation_unit_id  
        FROM sys.allocation_units AS au  
            INNER JOIN sys.partitions AS p   
                ON au.container_id = p.hobt_id   
                    AND (au.type = 1 OR au.type = 3)  
        UNION ALL  
        SELECT object_name(object_id) AS name     
            ,index_id, allocation_unit_id  
        FROM sys.allocation_units AS au  
            INNER JOIN sys.partitions AS p   
                ON au.container_id = p.partition_id   
                    AND au.type = 2  
    ) AS obj   
        ON bd.allocation_unit_id = obj.allocation_unit_id  
WHERE database_id = DB_ID()  
GROUP BY name, index_id   
ORDER BY cached_pages_count DESC;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 
 [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [Ressourcendatenbank](../../relational-databases/databases/resource-database.md)   
 [sys.dm_os_buffer_pool_extension_configuration &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-pool-extension-configuration-transact-sql.md)  
  
