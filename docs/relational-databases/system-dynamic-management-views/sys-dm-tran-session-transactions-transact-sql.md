---
description: sys.dm_tran_session_transactions (Transact-SQL)
title: sys. dm_tran_session_transactions (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_tran_session_transactions
- sys.dm_tran_session_transactions
- sys.dm_tran_session_transactions_TSQL
- dm_tran_session_transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_session_transactions dynamic management view
ms.assetid: c7157491-58c2-49fe-87d7-0c9723113adf
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba1077eddc05433ee256aceba3586339a50b36e1
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89546464"
---
# <a name="sysdm_tran_session_transactions-transact-sql"></a>sys.dm_tran_session_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt Korrelationsinformationen für zugehörige Transaktionen und Sitzungen zurück.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys. dm_pdw_nodes_tran_session_transactions**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|session_id|**int**|ID der Sitzung, unter der die Transaktion ausgeführt wird.|  
|transaction_id|**bigint**|ID der Transaktion.|  
|transaction_descriptor|**Binär (8)**|Die Transaktions-ID, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für die Kommunikation mit dem Clienttreiber verwendet wird.|  
|enlist_count|**int**|Anzahl der aktiven Anforderungen in der Sitzung für die Transaktion.|  
|is_user_transaction|**bit**|1 = Die Transaktion wurde von einer Benutzeranforderung initiiert.<br /><br /> 0 = Systemtransaktion.|  
|is_local|**bit**|1 = Lokale Transaktion.<br /><br /> 0 = Verteilte Transaktion oder eine eingetragene gebundene Sitzungstransaktion.|  
|is_enlisted|**bit**|1 = Eingetragene verteilte Transaktion.<br /><br /> 0 = Keine eingetragene verteilte Transaktion.|  
|is_bound|**bit**|1 = Die Transaktion ist in der Sitzung über gebundene Sitzungen aktiv.<br /><br /> 0 = Die Transaktion ist in der Sitzung nicht über gebundene Sitzungen aktiv.|  
|open_transaction_count||Die Anzahl der offenen Transaktionen für jede Sitzung.|  
|pdw_node_id|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] Premium-Tarifen ist die- `VIEW DATABASE STATE` Berechtigung in der Datenbank erforderlich. In [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] den Tarifen "Standard" und "Basic" ist der  **Server Administrator** oder ein **Azure Active Directory Administrator** Konto erforderlich.   

## <a name="remarks"></a>Hinweise  
 Über gebundene Sitzungen und verteilte Transaktionen kann eine Transaktion unter mehreren Sitzungen ausgeführt werden. In diesen Fällen zeigt sys.dm_tran_session_transactions mehrere Zeilen für dieselbe transaction_id an, und zwar eine pro Sitzung, unter der die Transaktion ausgeführt wird.  
  
 Durch Ausführen mehrerer Anforderungen im Autocommitmodus mithilfe mehrerer aktiver Resultsets (MARS) ist mehr als eine aktive Transaktion in einer einzigen Sitzung möglich. In diesen Fällen zeigt sys.dm_tran_session_transactions mehrere Zeilen für dieselbe session_id an, und zwar eine pro Transaktion, die unter dieser Sitzung ausgeführt wird.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungssichten und Funktionen in Verbindung mit Transaktionen &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


