---
title: sys.sp_rda_deauthorize_db (Transact-SQL) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit sys.sp_rda_deauthorize_db authentifizierte Verbindungen zwischen lokalen Stretch-aktivierten Datenbanken und Remote-Azure-Datenbanken entfernen.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_deauthorize_db
- sys.sp_rda_deauthorize_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_deauthorize_db stored procedure
ms.assetid: 2e362e15-2cd5-4856-9f0b-54df56b0866b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9abdf527c434674ccecd655c93c27606b29b940d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186634"
---
# <a name="syssp_rda_deauthorize_db-transact-sql"></a>sys.sp_rda_deauthorize_db (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Entfernt die authentifizierte Verbindung zwischen einer lokalen Stretch-aktivierten Datenbank und der Azure-Remote Datenbank. Führen Sie **sp_rda_deauthorize_db**  aus, wenn die Remote Datenbank nicht erreichbar ist oder sich in einem inkonsistenten Zustand befindet und Sie das Abfrage Verhalten für alle Stretch-aktivierten Tabellen in der Datenbank ändern möchten.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
sp_rda_deauthorize_db   
```  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 0 (Erfolg) oder >0 (Fehler)  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert db_owner-Berechtigungen.  
  
## <a name="remarks"></a>Bemerkungen  
 Nachdem Sie **sp_rda_deauthorize_db** ausgeführt haben, schlagen alle Abfragen für Datenbanken und Tabellen mit aktivierter Stretch-Funktion fehl. Das heißt, der Abfrage Modus ist auf "deaktiviert" festgelegt. Führen Sie eine der folgenden Aktionen aus, um diesen Modus zu beenden.  
  
-   Führen Sie [sys.sp_rda_reauthorize_db &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) aus, um erneut eine Verbindung mit der Azure-Remote Datenbank herzustellen. Durch diesen Vorgang wird der Abfrage Modus automatisch auf LOCAL_AND_REMOTE zurückgesetzt. Dies ist das Standardverhalten für Stretch Database. Das heißt, dass Abfragen Ergebnisse sowohl aus lokalen als auch aus Remote Daten zurückgeben.  
  
-   Führen Sie [sys.sp_rda_set_query_mode &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md) mit dem LOCAL_ONLY Argument aus, damit Abfragen weiterhin nur für lokale Daten ausgeführt werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.sp_rda_set_query_mode &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md)   
 [sys.sp_rda_reauthorize_db &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md)   
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
