---
title: Konfigurieren einer Verfügbarkeitsgruppe zur Leseskalierung (SQL Server für Linux)
description: Erfahren Sie, wie Sie eine SQL Server-Always On-Verfügbarkeitsgruppe unter Linux für Workloads zur Leseskalierung konfigurieren.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 01/09/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 5cad1c63817bf55231c065123b2456590dbfebb2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352747"
---
# <a name="configure-a-sql-server-availability-group-for-read-scale-on-linux"></a>Konfigurieren einer SQL Server-Verfügbarkeitsgruppe zur Leseskalierung unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Sie können eine SQL Server-Always On-Verfügbarkeitsgruppe unter Linux für Workloads zur Leseskalierung konfigurieren. Für Verfügbarkeitsgruppen gibt es zwei Architekturtypen. In einer Architektur für Hochverfügbarkeit wird ein Cluster-Manager verwendet, um verbesserte Geschäftskontinuität bereitzustellen. Diese Architektur kann auch Replikate mit Leseskalierung enthalten. Informationen dazu, wie die Architektur für Hochverfügbarkeit erstellt wird, finden Sie unter [Konfigurieren von SQL Server-Always On-Verfügbarkeitsgruppen für Hochverfügbarkeit unter Linux](sql-server-linux-availability-group-configure-ha.md). Die andere Architektur unterstützt nur Workloads zur Leseskalierung. In diesem Artikel wird erläutert, wie eine Verfügbarkeitsgruppe ohne Cluster-Manager für Workloads zur Leseskalierung erstellt wird. Diese Architektur bietet nur Leseskalierung. Sie bietet keine Hochverfügbarkeit.

> [!NOTE]
> Eine Verfügbarkeitsgruppe mit `CLUSTER_TYPE = NONE` kann Replikate enthalten, die auf verschiedenen Betriebssystemplattformen gehostet werden. Sie kann keine Unterstützung für Hochverfügbarkeit bieten. 

[!INCLUDE [Create prerequisites](../includes/ss-linux-cluster-availability-group-create-prereq.md)]

## <a name="create-the-ag"></a>Erstellen der Verfügbarkeitsgruppe

Erstellen Sie die Verfügbarkeitsgruppe. Legen Sie `CLUSTER_TYPE = NONE` fest. Darüber hinaus sollten Sie jedes Replikat mit `FAILOVER_MODE = MANUAL` festlegen. Clientanwendungen, die Analysen durchführen oder Berichte zu Workloads erstellen, können eine direkte Verbindung mit der sekundären Datenbank herstellen. Sie können auch eine schreibgeschützte Routingliste erstellen. Anforderungen zum Lesen der Verbindung werden dann über Verbindungen mit dem primären Replikat in Round-Robin-Manier an jedes der in der Routingliste enthaltene sekundäre Replikat weitergeleitet.

Das folgende Transact-SQL-Skript erstellt eine Verfügbarkeitsgruppe mit dem Namen `ag1`. Das Skript konfiguriert die Verfügbarkeitsgruppenreplikate mit `SEEDING_MODE = AUTOMATIC`. Diese Einstellung bewirkt, dass SQL Server die Datenbank automatisch auf jedem sekundären Server erstellt, nachdem diese zur Verfügbarkeitsgruppe hinzugefügt wurde. Aktualisieren Sie das folgende Skript für Ihre Umgebung. Ersetzen Sie die Werte `<node1>` und `<node2>` durch die Namen der SQL Server-Instanzen, auf denen die Replikate gehostet werden. Ersetzen Sie den Wert `<5022>` durch den Port, den Sie für den Endpunkt festgelegt haben. Führen Sie auf dem primären SQL Server-Replikat das folgende Transact-SQL-Skript aus:

```SQL
CREATE AVAILABILITY GROUP [ag1]
    WITH (CLUSTER_TYPE = NONE)
    FOR REPLICA ON
        N'<node1>' WITH (
            ENDPOINT_URL = N'tcp://<node1>:<5022>',
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            FAILOVER_MODE = MANUAL,
            SEEDING_MODE = AUTOMATIC,
                    SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            ),
        N'<node2>' WITH ( 
            ENDPOINT_URL = N'tcp://<node2>:<5022>', 
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            FAILOVER_MODE = MANUAL,
            SEEDING_MODE = AUTOMATIC,
            SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            );
        
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

### <a name="join-secondary-sql-servers-to-the-ag"></a>Verknüpfen sekundärer SQL-Server mit der Verfügbarkeitsgruppe

Das folgende Transact-SQL-Skript verknüpft einen Server mit einer Verfügbarkeitsgruppe mit dem Namen `ag1`. Aktualisieren Sie das Skript für Ihre Umgebung. Führen Sie auf jedem sekundären SQL Server-Replikat das folgende Transact-SQL-Skript aus, um die Verfügbarkeitsgruppe zu verknüpfen:

```SQL
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = NONE);
         
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create post](../includes/ss-linux-cluster-availability-group-create-post.md)]

Bei dieser Verfügbarkeitsgruppe handelt es sich um keine Hochverfügbarkeitskonfiguration. Wenn Sie Hochverfügbarkeit benötigen, befolgen Sie die Anweisungen unter [Konfigurieren einer Always On-Verfügbarkeitsgruppe für SQL Server unter Linux](sql-server-linux-availability-group-configure-ha.md). Insbesondere ist zu beachten, dass die Verfügbarkeitsgruppe mit `CLUSTER_TYPE=WSFC` (unter Windows) oder `CLUSTER_TYPE=EXTERNAL` (unter Linux) erstellt werden muss. Integrieren Sie die Verfügbarkeitsgruppe anschließend in einen Cluster-Manager, indem Sie entweder Windows Server-Failoverclustering unter Windows oder Pacemaker unter Linux verwenden.

## <a name="connect-to-read-only-secondary-replicas"></a>Verbinden mit schreibgeschützten sekundären Replikaten

Es gibt zwei Möglichkeiten für die Verbindung mit schreibgeschützten sekundären Replikaten. Anwendungen können eine direkte Verbindung mit der SQL Server-Instanz herstellen, auf der das sekundäre Replikat gehostet wird, und die Datenbanken abfragen. Sie können auch schreibgeschütztes Routing verwenden, wofür ein Listener erforderlich ist.

* [Lesbare sekundäre Replikate](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)
* [Schreibgeschütztes Routing](../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md#ConnectToSecondary)

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>Ausführen eines Failovers des primären Replikats auf eine schreibgeschützte Verfügbarkeitsgruppe

[!INCLUDE[Force failover](../includes/ss-force-failover-read-scale-out.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren verteilter Verfügbarkeitsgruppen](../database-engine/availability-groups/windows/distributed-availability-groups.md)
* [Weitere Informationen zu Verfügbarkeitsgruppen](../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)
* [Ausführen eines erzwungenen manuellen Failovers](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)