---
description: MSagentparameterlist (Transact-SQL)
title: MSagentparameterlist (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSagentparameterlist_TSQL
- MSagentparameterlist
dev_langs:
- TSQL
helpviewer_keywords:
- Msagentparameterlist system table
ms.assetid: 4ea571a0-078d-4e13-95ee-f3d4bbd4dfb2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b5fcded0e1ffe97578832f773e65d2d08e1c41d8
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89551045"
---
# <a name="msagentparameterlist-transact-sql"></a>MSagentparameterlist (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSagentparameterlist** -Tabelle enthält Informationen zum Replikations-Agent-Parameter und wird verwendet, um die Parameter anzugeben, die für einen bestimmten Agenttyp festgelegt werden können. Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**agent_type**|**tinyint**|Der Agenttyp:<br /><br /> **1** = Momentaufnahmen-Agent.<br /><br /> **2** = Protokolllese-Agent.<br /><br /> **3** = Verteilungs-Agent.<br /><br /> **4** = Merge-Agent.<br /><br /> **9** = Warteschlangenlese-Agent.|  
|**parameter_name**|**sysname**|Der Name eines gültigen Agentparameters.|  
|**default_value**|**nvarchar(4000)**|Der Standardwert des Agentparameters, wobei NULL darauf hinweist, dass kein Standardwert vorhanden ist.|  
|**min_value**|**int**|Legt einen unteren Grenzwert für den Agentparameter fest, wobei NULL darauf hinweist, dass kein unterer Grenzwert vorhanden ist.|  
|**max_value**|**int**|Legt einen oberen Grenzwert für den Agentparameter fest, wobei NULL darauf hinweist, dass kein oberer Grenzwert vorhanden ist.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
