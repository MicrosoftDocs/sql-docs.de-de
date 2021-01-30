---
description: sys.fn_hadr_backup_is_preferred_replica (Transact-SQL)
title: sys.fn_hadr_backup_is_preferred_replica (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_hadr_backup_is_preferred_replica_TSQL
- sys.fn_hadr_backup_is_preferred_replica
- fn_hadr_backup_is_preferred_replica_TSQL
- fn_hadr_backup_is_preferred_replica
dev_langs:
- TSQL
helpviewer_keywords:
- backup on secondary replicas
- active secondary replicas [SQL Server], backup on secondary replicas
- sys.fn_hadr_backup_is_preferred_replica function
ms.assetid: 61b9be77-e2f6-4da1-b2ae-a62cbe226145
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 009e237b8d7ac7d100100969cf9f8a75bbad1195
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206040"
---
# <a name="sysfn_hadr_backup_is_preferred_replica--transact-sql"></a>sys.fn_hadr_backup_is_preferred_replica (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Dient zum Ermitteln, ob das aktuelle Replikat das bevorzugte Sicherungsreplikat ist.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.fn_hadr_backup_is_preferred_replica ( 'dbname' )  
```  
  
## <a name="arguments"></a>Argumente  
 "*dbname*"  
 Der Name der zu sichernden Datenbank. *dbname* ist vom Typ sysname.  
  
## <a name="returns"></a>Gibt zurück  
 Gibt den Datentyp **bool**: 1 zurück, wenn sich die Datenbank auf der aktuellen Instanz auf dem bevorzugten Replikat befindet, andernfalls 0.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie diese Funktion in einem Sicherungsskript, um zu ermitteln, ob sich die aktuelle Datenbank auf dem für Sicherungen bevorzugten Replikat befindet. Sie können ein Skript über jedes Verfügbarkeitsreplikat ausführen. Jeder dieser Aufträge untersucht die gleichen Daten, um zu bestimmen, welcher Auftrag ausgeführt werden soll, sodass nur einer der geplanten Aufträge tatsächlich in die Sicherungs Phase übergeht. Beispielcode kann sich wie folgt zusammensetzen.  
  
```  
If sys.fn_hadr_backup_is_preferred_replica( @dbname ) <> 1   
BEGIN  
-- If this is not the preferred replica, exit (probably without error).  
END  
-- If this is the preferred replica, continue to do the backup.  
  
```  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-sysfn_hadr_backup_is_preferred_replica"></a>A. Verwenden von sys.fn_hadr_backup_is_preferred_replica  
 Im folgenden Beispiel wird 1 zurückgegeben, wenn die aktuelle Datenbank dem bevorzugten Sicherungsreplikat entspricht.  
  
```  
SELECT sys.fn_hadr_backup_is_preferred_replica ('TestDB');  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Konfigurieren der Sicherung auf Verfügbarkeitsreplikaten &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Always on-Verfügbarkeits Gruppenfunktionen &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/always-on-availability-groups-functions-transact-sql.md)   
 [Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [Aktive sekundäre Replikate: Sicherung auf sekundären Replikaten &#40;Always on Verfügbarkeits Gruppen&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md) [Katalog Sichten für Always on Verfügbarkeits Gruppen &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)      
  
  
