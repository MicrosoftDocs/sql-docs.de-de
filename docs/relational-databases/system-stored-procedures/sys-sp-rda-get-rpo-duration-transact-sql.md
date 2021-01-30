---
title: sys.sp_rda_get_rpo_duration (Transact-SQL) | Microsoft-Dokumentation
description: Verwenden Sie sys.sp_rda_get_rpo_duration, um die Anzahl der Stunden der migrierten Daten zu erhalten, die SQL Server in einer Stagingtabelle speichert, um die Azure-Datenbank vollständig wiederherzustellen.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_get_rpo_duration
- sys.sp_rda_get_rpo_duration_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_get_rpo_duration stored procedure
ms.assetid: 35882067-3072-47ff-9024-ca453c0f49a7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c5c84b580f2a51ad9507db2fbece327a3ca71608
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211799"
---
# <a name="syssp_rda_get_rpo_duration-transact-sql"></a>sys.sp_rda_get_rpo_duration (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Ruft die Anzahl der Stunden der migrierten Daten ab, die SQL Server in einer Stagingtabelle beibehält, um eine vollständige Wiederherstellung der Azure-Remote Datenbank zu gewährleisten, wenn eine Zeit Punkt Wiederherstellung erforderlich ist. 
  
  Weitere Informationen finden Sie unter [Stretch Database verringert das Risiko eines Daten Verlusts für Ihre Azure-Daten, indem migrierte Zeilen vorübergehend beibehalten](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md#stretchRPO)werden.  
    
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
    
## <a name="syntax"></a>Syntax    
    
```    
    
sp_rda_get_rpo_duration @durationinhours output    
    
```    
    
## <a name="output-parameter"></a>Ausgabeparameter    
 *\@durationinhours*    
  Die Anzahl der Stunden (ein ganzzahliger Wert ungleich null) der migrierten Daten, die SQL Server für die aktuelle Stretch-aktivierte Datenbank beibehalten.    
    
## <a name="permissions"></a>Berechtigungen    
 Erfordert db_owner-Berechtigungen.    
    
## <a name="remarks"></a>Bemerkungen    
 Ändern Sie den Wert, indem Sie [sys.sp_rda_set_rpo_duration &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-set-rpo-duration-transact-sql.md)ausführen.    
    
## <a name="see-also"></a>Weitere Informationen    
 [sys.sp_rda_set_rpo_duration &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-set-rpo-duration-transact-sql.md)     
 [Wiederherstellen von Stretch-aktivierten Datenbanken (Stretch Database)](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)    
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)    
    
  
