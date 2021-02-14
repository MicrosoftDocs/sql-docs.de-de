---
description: sys.availability_groups (Transact-SQL)
title: sys.availability_groups (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.availability_groups_TSQL
- availability_groups_TSQL
- sys.availability_groups
- availability_groups
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_groups catalog view
ms.assetid: da7fa55f-c008-45d9-bcfc-3513b02d9e71
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cc784f8bbd811ad50f2fcb1b64bd3f5bdf8cd2d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338914"
---
# <a name="sysavailability_groups-transact-sql"></a>sys.availability_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile für jede Verfügbarkeitsgruppe zurück, für die die lokale SQL Server-Instanz ein Verfügbarkeitsreplikat hostet. Jede Zeile enthält eine zwischengespeicherte Kopie der Metadaten der Verfügbarkeitsgruppe.  
   
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Eindeutiger Bezeichner (GUID) der Verfügbarkeitsgruppe.|  
|**name**|**sysname**|Der Name der Verfügbarkeitsgruppe. Dies ist ein vom Benutzer angegebener Name, der innerhalb des Windows Server-Failoverclusters (WSFC) eindeutig sein muss.|  
|**resource_id**|**nvarchar(40)**|Ressourcen-ID für die WSFC-Clusterressource.|  
|**resource_group_id**|**nvarchar(40)**|Ressourcengruppen-ID für die WSFC-Clusterressourcengruppe der Verfügbarkeitsgruppe.|  
|**failure_condition_level**|**int**|Eine benutzerdefinierte Fehler Bedingungs Ebene, unter der ein automatisches Failover ausgelöst werden muss, einer der ganzzahligen Werte, die in der Tabelle direkt unterhalb dieser Tabelle angezeigt werden.<br /><br /> Die Fehlerbedingungsebenen (1–5) reichen von der Ebene 1 mit den wenigsten Einschränkungen bis zur Ebene 5 mit den meisten Einschränkungen. Jede Bedingungsebene umfasst stets auch sämtliche weniger restriktiven Ebenen. Daher schließt die strengste Bedingungsebene 5 die vier Bedingungsebenen mit weniger Einschränkungen (1-4) ein, Ebene 4 schließt die Ebenen 1-3 ein usw.<br /><br /> Um diesen Wert zu ändern, verwenden Sie die FAILURE_CONDITION_LEVEL-Option der [Alter Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) - [!INCLUDE[tsql](../../includes/tsql-md.md)] Anweisung.|  
|**health_check_timeout**|**int**|Die Wartezeit (in Millisekunden), mit der die gespeicherte Prozedur des [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) Servers Server Zustandsinformationen zurückgibt, bevor davon ausgegangen wird, dass die Serverinstanz langsam ist oder nicht antwortet. Der Standardwert ist 30000 Millisekunden (oder 30 Sekunden).<br /><br /> Um diesen Wert zu ändern, verwenden Sie die HEALTH_CHECK_TIMEOUT-Option der [Alter Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) - [!INCLUDE[tsql](../../includes/tsql-md.md)] Anweisung.|  
|**automated_backup_preference**|**tinyint**|Bevorzugter Speicherort zum Durchführen von Sicherungen auf den Verfügbarkeitsdatenbanken in dieser Verfügbarkeitsgruppe. Im folgenden sind die möglichen Werte und ihre Beschreibungen aufgeführt.<br /><br /> <br /><br /> 0: primär. Sicherungen sollten immer auf dem primären Replikat erfolgen.<br /><br /> 1: nur sekundär. Die Durchführung von Sicherungen auf einem sekundären Replikat wird bevorzugt.<br /><br /> 2: Sekundär bevorzugen. Die Durchführung von Sicherungen auf einem sekundären Replikat wird bevorzugt, aber die Durchführung von Sicherungen auf dem primären Replikat wird akzeptiert, wenn kein sekundäres Replikat für Sicherungsvorgänge verfügbar ist. Dies ist das Standardverhalten.<br /><br /> 3: beliebiges Replikat. Keine Präferenz, ob Sicherungen auf dem primären Replikat oder einem sekundären Replikat durchgeführt werden.<br /><br /> <br /><br /> Weitere Informationen finden Sie unter [Aktive sekundäre Replikate: Sicherung auf sekundären Replikaten &#40;Always On-Verfügbarkeitsgruppen&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).|  
|**automated_backup_preference_desc**|**nvarchar(60)**|Beschreibung der **automated_backup_preference**, eine der folgenden:<br /><br /> PRIMARY<br /><br /> SECONDARY_ONLY<br /><br /> SECONDARY<br /><br /> Keine|  
|**version**|**smallint**|Die Version der Verfügbarkeits Gruppen Metadaten, die im Windows-Failovercluster gespeichert sind. Diese Versionsnummer wird erhöht, wenn neue Funktionen hinzugefügt werden.|  
|**basic_features**|**bit**|Gibt an, ob dies eine Basis-Verfügbarkeits Gruppe ist. Weitere Informationen finden Sie unter [Basis-Verfügbarkeitsgruppen &#40;Always On-Verfügbarkeitsgruppen&#41;](../../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).|  
|**dtc_support**|**bit**|Gibt an, ob die DTC-Unterstützung für diese Verfügbarkeits Gruppe aktiviert wurde. Die Option **DTC_SUPPORT** von **Create Availability Group** steuert diese Einstellung.|  
|**db_failover**|**bit**|Gibt an, ob die Verfügbarkeits Gruppe Failover für Daten Bank Integritäts Bedingungen unterstützt. Die Option **DB_FAILOVER** von **Create Availability Group** steuert diese Einstellung.|  
|**is_distributed**|**bit**|Gibt an, ob es sich um eine verteilte Verfügbarkeits Gruppe handelt. Weitere Informationen finden Sie unter [Verteilte Verfügbarkeitsgruppen &#40;AlwaysOn-Verfügbarkeitsgruppen&#41;](../../database-engine/availability-groups/windows/distributed-availability-groups.md).|
|**cluster_type**|**tinyint**|0: Windows Server-Failovercluster <br/><br/>1: externer Cluster (z. b. Linux-Pacemaker)<br/><br/>2: keine|
|**cluster_type_desc**|**nvarchar(60)**|Text Beschreibung des Cluster Typs|
|**required_synchronized_secondaries_to_commit**|**int**| Die Anzahl der sekundären Replikate, die sich im synchronisierten Zustand befinden müssen, damit ein Commit ausgeführt wird.|
|**sequence_number**|**bigint**|Gibt die Konfigurations Sequenz der Verfügbarkeits Gruppe an. Nimmt jedes Mal zu, wenn das primäre Replikat der Verfügbarkeits Gruppe die Konfiguration der Gruppe aktualisiert.|
|**is_contained**|**bit**|1: die Master Instanz für Big Data-Cluster wurde für hohe Verfügbarkeit konfiguriert. <br/><br/> 0: alle anderen.|
  
## <a name="failure-condition-level--values"></a>Werte für Fehler Bedingungs Ebene  
 In der folgenden Tabelle werden die möglichen Fehler Bedingungs Ebenen für die **failure_condition_level** Spalte beschrieben.  
  
|Wert|Fehlerbedingung|  
|-----------|-----------------------|  
|1|Gibt an, dass in einem der folgenden Fälle ein automatisches Failover initiiert werden muss:<br /><br /> <br /><br /> -Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Dienst ist herunter.<br /><br /> -Die Lease der Verfügbarkeits Gruppe für die Verbindung mit dem wsfc-Failovercluster läuft ab, da keine ACK von der Serverinstanz empfangen wird. Weitere Informationen finden Sie unter [How It Works: SQL Server Always On Lease Timeout (Funktionsweise: SQL Server Always On-Leasetimeout)](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268).|  
|2|Gibt an, dass in einem der folgenden Fälle ein automatisches Failover initiiert werden muss:<br /><br /> <br /><br /> -Die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt keine Verbindung mit dem Cluster her, und der vom Benutzer angegebene **health_check_timeout** Schwellenwert der Verfügbarkeits Gruppe wurde überschritten.<br /><br /> -Das Verfügbarkeits Replikat weist einen fehlerhaften Status auf.|  
|3|Gibt an, dass ein automatisches Failover bei kritischen internen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlern initiiert werden soll, z. B. verwaisten Spinlocks, schwerwiegenden Schreibzugriffsverletzungen oder zu vielen Sicherungen.<br /><br /> Dies ist der Standardwert.|  
|4|Gibt an, dass ein automatisches Failover bei mittelschweren internen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlern initiiert werden soll, z. B. bei dauerhaft unzureichendem Arbeitsspeicher im internen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ressourcenpool.|  
|5|Gibt an, dass ein automatisches Failover bei sämtlichen qualifizierten Fehlerbedingungen initiiert werden soll, einschließlich:<br /><br /> <br /><br /> -Erschöpfung der SQL-Engine-Arbeitsthreads.<br /><br /> -Erkennung eines nicht lösbaren Deadlocks.|  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW ANY DEFINITION-Berechtigung für die Serverinstanz.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Überwachen von Verfügbarkeits Gruppen &#40;Transact-SQL-&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Überwachen von Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
