---
description: sys.availability_groups_cluster (Transact-SQL)
title: sys.availability_groups_cluster (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.availability_groups_cluster
- availability_groups_cluster
- availability_groups_cluster_TSQL
- sys.availability_groups_cluster_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_groups_cluster catalog view
- Availability Groups [SQL Server], WSFC clusters
ms.assetid: d0f4683f-cdf0-4227-8b68-720ffe58f158
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 22e1b7354ff2e343b86530766e6fe040383a2920
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341368"
---
# <a name="sysavailability_groups_cluster-transact-sql"></a>sys.availability_groups_cluster (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile für jede Always On-Verfügbarkeitsgruppe im WSFC-Cluster (Windows Server-Failoverclustering) zurück. Jede Zeile enthält die Metadaten der Verfügbarkeitsgruppe aus dem WSFC-Cluster.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Eindeutiger Bezeichner (GUID) der Verfügbarkeitsgruppe.|  
|**name**|**sysname**|Der Name der Verfügbarkeitsgruppe. Dies ist ein vom Benutzer angegebener Name, der innerhalb des Windows Server-Failoverclusters (WSFC) eindeutig sein muss.|  
|**resource_id**|**nvarchar(40)**|Ressourcen-ID für die WSFC-Clusterressource.|  
|**resource_group_id**|**nvarchar(40)**|Ressourcengruppen-ID für die WSFC-Clusterressourcengruppe der Verfügbarkeitsgruppe.|  
|**failure_condition_level**|**int**|Benutzerdefinierte Fehlerbedingungsebene, unter der ein automatisches Failover ausgelöst werden muss, einer der folgenden ganzzahligen Werte:<br /><br /> 1: gibt an, dass ein automatisches Failover initiiert werden soll, wenn eine der folgenden Aktionen auftritt: <br />-Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Dienst ist herunter.<br />-Die Lease der Verfügbarkeits Gruppe für die Verbindung mit dem wsfc-Failovercluster läuft ab, da keine ACK von der Serverinstanz empfangen wird. Weitere Informationen finden Sie unter [How It Works: SQL Server Always On Lease Timeout (Funktionsweise: SQL Server Always On-Leasetimeout)](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268).<br /><br /> 2: gibt an, dass ein automatisches Failover initiiert werden soll, wenn eine der folgenden Aktionen auftritt:  <br />-Die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt keine Verbindung mit dem Cluster her, und der vom Benutzer angegebene **health_check_timeout** Schwellenwert der Verfügbarkeits Gruppe wurde überschritten. <br />-Das Verfügbarkeits Replikat weist einen fehlerhaften Status auf. <br />3: gibt an, dass ein automatisches Failover bei kritischen internen Fehlern initiiert werden soll [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , z. b. verwaisten Spinlocks, ernsthaften Schreibzugriffs Verletzungen oder zu viel Absicherungen. Dies ist der Standardwert. <br />4: gibt an, dass ein automatisches Failover bei mittleren internen Fehlern initiiert werden soll [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , z. b. bei einem permanenten Speichermangel im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] internen Ressourcenpool.<br />5: gibt an, dass ein automatisches Failover für alle qualifizierten Fehlerbedingungen initiiert werden soll, einschließlich:<br />-Erschöpfung der SQL-Engine-Arbeitsthreads. <br />-Erkennung eines nicht lösbaren Deadlocks.<br /><br /> Die Fehlerbedingungsebenen (1–5) reichen von der Ebene 1 mit den wenigsten Einschränkungen bis zur Ebene 5 mit den meisten Einschränkungen. Jede Bedingungsebene umfasst stets auch sämtliche weniger restriktiven Ebenen. Daher schließt die strengste Bedingungsebene 5 die vier Bedingungsebenen mit weniger Einschränkungen (1-4) ein, Ebene 4 schließt die Ebenen 1-3 ein usw.<br /><br /> Um diesen Wert zu ändern, verwenden Sie die FAILURE_CONDITION_LEVEL-Option der [Alter Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) - [!INCLUDE[tsql](../../includes/tsql-md.md)] Anweisung.|  
|**health_check_timeout**|**int**|Die Wartezeit (in Millisekunden), mit der die gespeicherte Prozedur des [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) Servers Server Zustandsinformationen zurückgibt, bevor davon ausgegangen wird, dass die Serverinstanz langsam ist oder nicht antwortet. Der Standardwert ist 30000 Millisekunden (oder 30 Sekunden).<br /><br /> Um diesen Wert zu ändern, verwenden Sie die HEALTH_CHECK_TIMEOUT-Option der [Alter Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) - [!INCLUDE[tsql](../../includes/tsql-md.md)] Anweisung.|  
|**automated_backup_preference**|**tinyint**|Bevorzugter Speicherort zum Durchführen von Sicherungen auf den Verfügbarkeitsdatenbanken in dieser Verfügbarkeitsgruppe. Einer der folgenden Werte:<br /><br /> 0: primär. Sicherungen sollten immer auf dem primären Replikat erfolgen.<br />1: nur sekundär. Die Durchführung von Sicherungen auf einem sekundären Replikat wird bevorzugt.<br />2: Sekundär bevorzugen. Die Durchführung von Sicherungen auf einem sekundären Replikat wird bevorzugt, aber die Durchführung von Sicherungen auf dem primären Replikat wird akzeptiert, wenn kein sekundäres Replikat für Sicherungsvorgänge verfügbar ist. Dies ist das Standardverhalten.<br />3: beliebiges Replikat. Keine Präferenz, ob Sicherungen auf dem primären Replikat oder einem sekundären Replikat durchgeführt werden.<br /><br /> Weitere Informationen finden Sie unter [Aktive sekundäre Replikate: Sicherung auf sekundären Replikaten &#40;Always On-Verfügbarkeitsgruppen&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).|  
|**automated_backup_preference_desc**|**nvarchar(60)**|Beschreibung der **automated_backup_preference**, eine der folgenden:<br /><br /> PRIMARY<br /><br /> SECONDARY_ONLY<br /><br /> SECONDARY<br /><br /> Keine|  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW ANY DEFINITION-Berechtigung für die Serverinstanz.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Überwachen von Verfügbarkeits Gruppen &#40;Transact-SQL-&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Überwachen von Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  
