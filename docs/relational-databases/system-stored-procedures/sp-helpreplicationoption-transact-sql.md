---
description: sp_helpreplicationoption (Transact-SQL)
title: sp_helpreplicationoption (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpreplicationoption
- sp_helpreplicationoption_TSQL
helpviewer_keywords:
- sp_helpreplicationoption
ms.assetid: ef988dbc-dd0b-4132-80ab-81eebec1cffe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 65514021064640439238b595a9178d916b70c286
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210799"
---
# <a name="sp_helpreplicationoption-transact-sql"></a>sp_helpreplicationoption (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Zeigt die Replikationsoptionstypen an, die für einen Server aktiviert sind. Diese gespeicherte Prozedur wird auf jedem Server für jede Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_helpreplicationoption [ [ @optname =] 'option_name' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @optname = ] 'option_name'` Der Name der Replikations Option, die abgefragt werden soll. *option_name* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Transaktions**|Ein Resultset wird zurückgegeben, wenn die Transaktionsreplikation aktiviert ist.|  
|**merge**|Ein Resultset wird zurückgegeben, wenn die Mergereplikation aktiviert ist.|  
|NULL (Standard)|Es wird kein Resultset zurückgegeben.|  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**optname**|**sysname**|Name der Replikationsoption. Die folgenden Werte sind möglich:<br /><br /> **Transaktions**<br /><br /> **merge**|  
|**value**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**major_version**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**minor_version**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**revision**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**install_failures**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_helpreplicationoption** wird verwendet, um Informationen zu Replikations Optionen zu erhalten, die auf einem bestimmten Server aktiviert sind. Verwenden Sie **sp_helpreplicationdboption**, um Informationen zu einer bestimmten Datenbank zu erhalten.  
  
## <a name="permissions"></a>Berechtigungen  
 Ausführungs Berechtigungen werden standardmäßig der **Public** -Rolle erteilt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
