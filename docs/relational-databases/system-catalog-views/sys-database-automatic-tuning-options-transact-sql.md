---
title: sys.database_automatic_tuning_options (Transact-SQL) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Optionen zur automatischen Optimierung für eine SQL-Datenbank anzeigen. Weitere Informationen finden Sie unter erforderliche Berechtigungen und Anzeigen zusätzlicher verfügbarer Ressourcen.
ms.custom: ''
ms.date: 07/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- database_automatic_tuning_options_tsql
- database_automatic_tuning_options
- sys.database_automatic_tuning_options_tsql
- sys.database_automatic_tuning_options
dev_langs:
- TSQL
helpviewer_keywords:
- database_automatic_tuning_options catalog view
- sys.database_automatic_tuning_options catalog view
ms.assetid: 16b47d55-8019-41ff-ad34-1e0112178067
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4da712a23dde26d12164957718c3bdfbf89eb487
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475221"
---
# <a name="sysdatabase_automatic_tuning_options-transact-sql"></a>Automatisches tuning_options von sys. Database \_ \_ (Transact-SQL)
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Gibt die Optionen für die automatische Optimierung für diese Datenbank zurück.  

|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar(128)**|Der Name der automatischen Optimierungs Option. Informationen zu den verfügbaren Optionen finden Sie unter [ALTER DATABASE SET AUTOMATIC_TUNING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) .|  
|**desired_state**|**smallint**|Gibt den gewünschten Betriebsmodus für die automatische Optimierungs Option an, der explizit vom Benutzer festgelegt wird.<br />0 = OFF<br />1 = ON|  
|**desired_state_desc**|**nvarchar(60)**|Textbeschreibung des gewünschten Betriebsmodus der automatischen Optimierungs Option.<br />OFF<br />EIN|  
|**actual_state**|**smallint**|Gibt den Betriebsmodus der automatischen Optimierungs Option an.<br />0 = OFF<br />1 = ON|  
|**actual_state_desc**|**nvarchar(60)**|Textbeschreibung des tatsächlichen Betriebsmodus der automatischen Optimierungs Option.<br />OFF<br />EIN|  
|**reason**|**smallint**|Gibt an, warum sich der tatsächliche und der gewünschte Status unterscheiden.<br />2 = deaktiviert<br />11 = QUERY_STORE_OFF<br />12 = QUERY_STORE_READ_ONLY<br />13 = NOT_SUPPORTED|   
|**reason_desc**|**nvarchar(60)**|Textbeschreibung des Grunds, warum sich der tatsächliche und der gewünschte Status unterscheiden.<br />Deaktiviert = Option ist vom System deaktiviert.<br />QUERY_STORE_OFF = Abfragespeicher ist ausgeschaltet.<br />QUERY_STORE_READ_ONLY = Abfragespeicher ist im schreibgeschützten Modus.<br />NOT_SUPPORTED = nur in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition verfügbar| 
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die `VIEW DATABASE STATE`-Berechtigung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Automatische Optimierung](../../relational-databases/automatic-tuning/automatic-tuning.md)   
 [ALTER DATABASE SET AUTOMATIC_TUNING &#40;Transact-SQL-&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.database_query_store_options &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.dm_db_tuning_recommendations &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)   
 
