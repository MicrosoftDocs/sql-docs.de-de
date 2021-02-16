---
title: Transact-SQL-Anweisungen für Verfügbarkeitsgruppen
description: Führt Transact-SQL-Anweisungen (T-SQL) ein, die das Bereitstellen, Erstellen und Verwalten von Always On-Verfügbarkeitsgruppen unterstützen.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], Transact-SQL statements
ms.assetid: 184d0a81-2259-4db9-9d0d-01aac0b502c8
author: cawrites
ms.author: chadam
ms.openlocfilehash: acbc954f076359f4a0a4517f459876fe3c9ff549
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342366"
---
# <a name="transact-sql-statements-for-always-on-availability-groups"></a>Transact-SQL-Anweisungen für Always On-Verfügbarkeitsgruppen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Dieses Thema bietet eine Einführung in die [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Anweisungen, die das Bereitstellen von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] und das Erstellen und Verwalten einer bestimmten Verfügbarkeitsgruppe, eines Verfügbarkeitsreplikats und einer Verfügbarkeitsdatenbank unterstützen.  
  
 
##  <a name="create-endpoint"></a><a name="CreateEndpoint"></a> CREATE ENDPOINT  
 [CREATE ENDPOINT ... FOR DATABASE_MIRRORING](../../../t-sql/statements/create-endpoint-transact-sql.md) erstellt einen Datenbankspiegelungsendpunkt, wenn keiner auf der Serverinstanz vorhanden ist. Für jede Serverinstanz, auf der Sie [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] oder eine Datenbankspiegelung bereitstellen möchten, ist ein Datenbankspiegelungs-Endpunkt erforderlich.  
  
 Führen Sie diese Anweisung auf der Serverinstanz aus, auf der Sie den Endpunkt erstellen. Sie können auf einer angegebenen Serverinstanz nur einen Datenbankspiegelungs-Endpunkt erstellen. Weitere Informationen finden Sie unter [Der Datenbankspiegelungs-Endpunkt &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md).  
  
##  <a name="create-availability-group"></a><a name="CreateAG"></a> CREATE AVAILABILITY GROUP  
 Mit[CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md) kann eine neue Verfügbarkeitsgruppe und optional ein Verfügbarkeitsgruppenlistener erstellt werden. Sie müssen mindestens die lokale Serverinstanz angeben, die das ursprüngliche primäre Replikat wird. Optional können Sie auch bis zu vier sekundäre Replikate angeben.  
  
 Führen Sie CREATE AVAILABILITY GROUP für die Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aus, die das ursprüngliche primäre Replikat der neuen Verfügbarkeitsgruppe hosten soll. Diese Serverinstanz muss sich auf einem Knoten eines Windows Server Failover Clusters (WSFC) befinden (weitere Informationen finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)).  
  
##  <a name="alter-availability-group"></a><a name="AlterAG"></a> ALTER AVAILABILITY GROUP  
 [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) unterstützt das Ändern einer vorhandenen Verfügbarkeitsgruppe oder eines Verfügbarkeitsgruppenlisteners und das Durchführen eines Failovers für eine Verfügbarkeitsgruppe.  
  
 Führen Sie ALTER AVAILABILITY GROUP für die Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aus, die das aktuelle primäre Replikat hostet.  
  
##  <a name="alter-database--set-hadr-"></a><a name="AlterDb"></a> ALTER DATABASE ... SET HADR ...  
 Die Optionen der [SET HADR](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) -Klausel der ALTER DATABASE-Anweisung ermöglichen es Ihnen, eine sekundäre Datenbank mit der Verfügbarkeitsgruppe der entsprechenden primären Datenbank zu verknüpfen, eine verknüpfte Datenbank zu entfernen und die Datensynchronisierung für eine verknüpfte Datenbank anzuhalten und die Datensynchronisierung fortzusetzen.  
  
##  <a name="drop-availability-group"></a><a name="DropAG"></a> DROP AVAILABILITY GROUP  
 [DROP AVAILABILITY GROUP](../../../t-sql/statements/drop-availability-group-transact-sql.md) entfernt eine bestimmte Verfügbarkeitsgruppe und alle Replikate. DROP AVAILABILITY GROUP kann von einem beliebigen [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Knoten im WSFC-Failovercluster ausgeführt werden.  
  
##  <a name="restrictions-on-the-availability-group-transact-sql-statements"></a><a name="Restrictions"></a> Einschränkungen bei den AVAILABILITY GROUP-Transact-SQL-Anweisungen  
 Die CREATE AVAILABILITY GROUP-, ALTER AVAILABILITY GROUP- und DROP AVAILABILITY GROUP-Anweisungen in [!INCLUDE[tsql](../../../includes/tsql-md.md)] weisen die folgenden Einschränkungen auf:  
  
-   Mit Ausnahme von DROP AVAILABILITY GROUP ist für die Ausführung dieser Anweisungen die Aktivierung des HADR-Diensts auf der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] erforderlich. Weitere Informationen finden Sie unter [Aktivieren und Deaktivieren von Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).  
  
-   Diese Anweisungen können nicht innerhalb von Transaktionen oder Batches ausgeführt werden.  
  
-   Trotz der Bereinigung nach einem Fehler wird mit diesen Anweisungen nicht sichergestellt, dass bei einem Fehler ein Rollback aller Änderungen ausgeführt wird. Systeme sollten Teilfehler jedoch sauber verarbeiten und dann ignorieren können.  
  
-   Diese Anweisungen unterstützen keine Ausdrücke oder Variablen.  
  
-   Wenn eine [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Anweisung während der Ausführung einer anderen Verfügbarkeitsgruppenaktion oder einer Wiederherstellung ausgeführt wird, gibt die Anweisung einen Fehler zurück. Warten Sie, bis die Aktion oder die Wiederherstellung abgeschlossen ist, und wiederholen Sie ggf. die Anweisung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht zu AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
