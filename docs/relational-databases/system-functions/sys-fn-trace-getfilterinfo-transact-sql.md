---
description: sys.fn_trace_getfilterinfo (Transact-SQL)
title: sys.fn_trace_getfilterinfo (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_trace_getfilterinfo
- fn_trace_getfilterinfo_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server], filters
- sys.fn_trace_getfilterinfo function
- filters [SQL Server], traces
- fn_trace_getfilterinfo function
ms.assetid: 09fe4a28-ff8a-4655-9da1-4654d5bc514d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7e20ee696ab4d371c6b12dfb24bd5433170080ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200366"
---
# <a name="sysfn_trace_getfilterinfo-transact-sql"></a>sys.fn_trace_getfilterinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt Informationen über die auf eine angegebene Ablaufverfolgung angewendeten Filter zurück.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen erweiterte Ereignisse.  
  
 
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn_trace_getfilterinfo ( trace_id )  
```  
  
## <a name="arguments"></a>Argumente  
 *trace_id*  
 Die ID der Ablaufverfolgung. *trace_id* ist vom Datentyp **int** und hat keinen Standardwert.  
  
## <a name="tables-returned"></a>Zurückgegebene Tabellen  
 Gibt die folgenden Informationen zurück. Weitere Informationen zu den Spalten finden Sie unter [sp_trace_setfilter &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md).  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**ColumnID**|**int**|Die ID der Spalte, auf die der Filter angewendet wird|  
|**logical_operator**|**int**|Gibt an, ob der Operator AND oder OR angewendet wird|  
|**comparison_operator**|**int**|Gibt die Art des Vergleichs an:<br /><br /> 0 = Gleich<br /><br /> 1 = Ungleich<br /><br /> 2 = Größer als<br /><br /> 3 = Kleiner als<br /><br /> 4 = Größer als oder gleich<br /><br /> 5 = Kleiner als oder gleich<br /><br /> 6 = Wie<br /><br /> 7 = Nicht wie|  
|**value**|**sql_variant**|Gibt den Wert an, auf den der Filter angewendet wird|  
  
## <a name="remarks"></a>Bemerkungen  
 Der Benutzer legt *trace_id* Wert fest, um die Ablauf Verfolgung zu identifizieren, zu ändern und zu steuern. Wenn die ID einer bestimmten Ablauf Verfolgung überschritten wird, gibt **fn_trace_getfilterinfo** Informationen zu allen Filtern für diese Ablauf Verfolgung zurück. Wenn die angegebene Ablaufverfolgung keinen Filter aufweist, gibt diese Funktion ein leeres Rowset zurück. Wird eine ungültige ID übergeben, gibt die Funktion ein leeres Rowset zurück. Ähnliche Informationen zu Ablauf Verfolgungen finden Sie unter [sys.fn_trace_getinfo &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER TRACE-Berechtigung auf dem Server.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel gibt Informationen zu allen Filtern der Ablaufverfolgung Nummer 2 zurück.  
  
```  
SELECT * FROM fn_trace_getfilterinfo(2) ;  
GO  
  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen einer Ablaufverfolgung &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [sp_trace_create &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)   
 [sp_trace_generateevent &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)   
 [sys.fn_trace_geteventinfo &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [sys.fn_trace_gettable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-gettable-transact-sql.md)  
  
  
