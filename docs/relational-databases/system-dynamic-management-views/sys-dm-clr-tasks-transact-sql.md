---
description: sys.dm_clr_tasks (Transact-SQL)
title: sys. dm_clr_tasks (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_clr_tasks
- sys.dm_clr_tasks_TSQL
- dm_clr_tasks
- dm_clr_tasks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_tasks dynamic management view
ms.assetid: 462b9061-09fa-4858-9707-03d6cc19c769
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 22066cd2fedee929a5d4047e20e30531728b45bb
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89537953"
---
# <a name="sysdm_clr_tasks-transact-sql"></a>sys.dm_clr_tasks (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt eine Zeile für alle CLR-Tasks (Common Language Runtime) zurück, die zurzeit ausgeführt werden. Ein [!INCLUDE[tsql](../../includes/tsql-md.md)] -Batch, der einen Verweis auf eine CLR-Routine enthält, erstellt einen separaten Task für die Ausführung des gesamten verwalteten Codes in diesem Batch. Mehrere Anweisungen im Batch, die die Ausführung von verwaltetem Code benötigen, verwenden denselben CLR-Task. Der CLR-Task übernimmt die Verwaltung des Status bzw. der Objekte im Zusammenhang mit der Ausführung von verwaltetem Code sowie der Übergänge zwischen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz und CLR.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**task_address**|**varbinary(8)**|Adresse des CLR-Tasks.|  
|**sos_task_address**|**varbinary(8)**|Adresse des zugrunde liegenden [!INCLUDE[tsql](../../includes/tsql-md.md)] -Batchtasks.|  
|**appdomain_address**|**varbinary(8)**|Adresse der Anwendungsdomäne, in der dieser Task ausgeführt wird.|  
|**state**|**nvarchar(128)**|Aktueller Status des Tasks.|  
|**abort_state**|**nvarchar(128)**|Status, in dem sich der Abbruch zurzeit befindet (falls der Task abgebrochen wurde). Beim Abbrechen eines Tasks durchläuft dieser einen Status nach dem anderen.|  
|**type**|**nvarchar(128)**|Tasktyp.|  
|**affinity_count**|**int**|Affinität des Tasks.|  
|**forced_yield_count**|**int**|Häufigkeit, mit der der Task gezwungen war, seine Position freizugeben.|  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] Premium-Tarifen ist die- `VIEW DATABASE STATE` Berechtigung in der Datenbank erforderlich. In [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] den Tarifen "Standard" und "Basic" ist der  **Server Administrator** oder ein **Azure Active Directory Administrator** Konto erforderlich.   
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten im Zusammenhang mit der Common Language Runtime &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
  

