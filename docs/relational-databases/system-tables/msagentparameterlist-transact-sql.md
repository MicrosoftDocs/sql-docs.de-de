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
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 111916053ec35d742994b81796efba1bb502a161
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88488799"
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
  
  
