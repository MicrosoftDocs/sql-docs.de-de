---
description: sys.dm_fts_population_ranges (Transact-SQL)
title: sys.dm_fts_population_ranges (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_population_ranges
- sys.dm_fts_population_ranges_TSQL
- dm_fts_population_ranges_TSQL
- dm_fts_population_ranges
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_population_ranges dynamic management view
ms.assetid: 58d8564b-9c43-4965-a31c-2893890334ef
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6ce4d78571837690ed06a9f2f506e2c19411d17a
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836517"
---
# <a name="sysdm_fts_population_ranges-transact-sql"></a>sys.dm_fts_population_ranges (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt Informationen zu den spezifischen Bereichen im Zusammenhang mit einer zurzeit ausgeführten Volltextindexauffüllung zurück.  
   
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**memory_address**|**varbinary(8)**|Adresse der Speicherpuffer, die für die auf diesen Teilbereich einer Volltextindexauffüllung bezogene Aktivität reserviert sind.|  
|**parent_memory_address**|**varbinary(8)**|Adresse der Speicherpuffer, die das übergeordnete Objekt aller Bereiche einer Auffüllung darstellen, die sich auf einen Volltextindex bezieht.|  
|**is_retry**|**bit**|Falls der Wert 1 ist, ist dieser Teilbereich für die Wiederholung von Zeilen verantwortlich, für die Fehler festgestellt wurden.|  
|**session_id**|**smallint**|ID der Sitzung, die diesen Task zurzeit verarbeitet.|  
|**processed_row_count**|**int**|Anzahl der von diesem Bereich verarbeiteten Zeilen. Die Fortschrittsinformationen werden persistent gespeichert und alle 5 Minuten, anstatt mit jedem Batchcommit, gezählt.|  
|**error_count**|**int**|Anzahl der Zeilen, für die von diesem Bereich Fehler erkannt wurden. Die Fortschrittsinformationen werden persistent gespeichert und alle 5 Minuten, anstatt mit jedem Batchcommit, gezählt.|  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
 
## <a name="physical-joins"></a>Physische Joins  
 ![Wesentliche Joins dieser dynamischen Verwaltungssicht](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-population-ranges-1.gif "Wesentliche Joins dieser dynamischen Verwaltungssicht")  
  
## <a name="relationship-cardinalities"></a>Kardinalität der Beziehungen  
  
|Von|Beschreibung|Relationship|  
|----------|--------|------------------|  
|dm_fts_population_ranges.parent_memory_address|dm_fts_index_population.memory_address|n:1|  
  
## <a name="see-also"></a>Weitere Informationen  
  [Dynamische Verwaltungs Sichten und Funktionen für die voll Text Suche und die semantische Suche &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
