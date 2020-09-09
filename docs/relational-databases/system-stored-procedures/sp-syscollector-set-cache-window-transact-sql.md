---
description: sp_syscollector_set_cache_window (Transact-SQL)
title: sp_syscollector_set_cache_window (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_syscollector_set_cache_window
- sp_syscollector_set_cache_window_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_set_cache_window stored procedure
- data collector [SQL Server], stored procedures
ms.assetid: 660f2749-392f-46bf-89f3-27764d848507
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 81d1be542b93a1ceb7bd699e2dbc07a7f3884fa4
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89541493"
---
# <a name="sp_syscollector_set_cache_window-transact-sql"></a>sp_syscollector_set_cache_window (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Legt fest, wie häufig bei Fehlern versucht wird, Daten hochzuladen. Das Wiederholen des Uploadversuchs im Fall eines Fehlers verringert das Risiko, gesammelte Daten zu verlieren.  

  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_syscollector_set_cache_window [ @cache_window = ] cache_window   
```  
  
## <a name="arguments"></a>Argumente  
 [ @cache_window =] *cache_window*  
 Gibt an, wie häufig im Fall eines Fehlers erneut versucht wird, Daten in das Verwaltungs-Data Warehouse hochzuladen, ohne dass dabei Daten verloren gehen. *cache_window* ist vom Datentyp **int** und hat den Standardwert 1. *cache_window* kann einen der folgenden Werte aufweisen:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|-1|Zwischenspeicherung aller hochzuladenden Daten aus den vorherigen fehlgeschlagenen Uploadversuchen.|  
|0|Keine Zwischenspeicherung von Daten aus einem fehlgeschlagenen Uploadversuch.|  
|*n*|Zwischenspeichern von Daten aus n früheren Uploadfehlern, wobei *n* >= 1 ist.|  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="remarks"></a>Bemerkungen  
 Sie müssen den Datensammler deaktivieren, bevor Sie die Cachefensterkonfiguration ändern. Bei dieser gespeicherten Prozedur tritt ein Fehler auf, wenn der Datensammler aktiviert ist. Weitere Informationen finden Sie unter [Aktivieren oder Deaktivieren der Datensammlung](../../relational-databases/data-collection/enable-or-disable-data-collection.md)und [Verwalten der Datensammlung](../../relational-databases/data-collection/manage-data-collection.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Damit diese Prozedur ausgeführt werden kann, ist die Mitgliedschaft in der festen Datenbankrolle dc_admin (mit EXECUTE-Berechtigung) erforderlich.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der Datensammler deaktiviert, das Cachefenster so konfiguriert, dass Daten für bis zu drei fehlgeschlagene Uploadversuche beibehalten werden, und der Datensammler anschließend erneut aktiviert.  
  
```sql  
USE msdb;  
GO  
EXECUTE dbo.sp_syscollector_disable_collector;  
GO  
EXECUTE dbo.sp_syscollector_set_cache_window 3;  
GO  
EXECUTE dbo.sp_syscollector_enable_collector;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_syscollector_set_cache_directory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-set-cache-directory-transact-sql.md)  
  
  
