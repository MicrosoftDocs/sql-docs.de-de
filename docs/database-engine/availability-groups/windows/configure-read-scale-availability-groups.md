---
title: Konfigurieren der Leseskalierung für eine Verfügbarkeitsgruppe
description: Hier erfahren Sie, wie Sie Ihre AlwaysOn-Verfügbarkeitsgruppe von SQL Server für Workloads zur Leseskalierung unter Windows konfigurieren.
ms.custom: seodec18
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.date: 05/24/2018
ms.topic: how-to
ms.prod: sql
ms.technology: availability-groups
ms.openlocfilehash: 995d8d2238ef9ded767544423093c8352e02bb1c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343385"
---
# <a name="configure-read-scale-for-an-always-on-availability-group"></a>Konfigurieren der Leseskalierung für eine Always On-Verfügbarkeitsgruppe

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Sie können eine SQL Server-Always On-Verfügbarkeitsgruppe unter Windows für Workloads zur Leseskalierung konfigurieren. Es gibt zwei Architekturtypen für Verfügbarkeitsgruppen:
* Eine Architektur für Hochverfügbarkeit, die einen Cluster-Manager zur Gewährleistung einer verbesserten Geschäftskontinuität verwendet und lesbare sekundäre Replikate enthalten kann. Informationen zur Erstellung dieser Architektur für Hochverfügbarkeit finden Sie unter [Erstellung und Konfiguration von Verfügbarkeitsgruppen unter Windows](creation-and-configuration-of-availability-groups-sql-server.md). 
* Eine Architektur, die nur Workloads zur Leseskalierung unterstützt. 

In diesem Artikel wird erläutert, wie eine Verfügbarkeitsgruppe ohne Cluster-Manager für Workloads zur Leseskalierung erstellt wird. Diese Architektur bietet nur Leseskalierung. Sie bietet keine Hochverfügbarkeit.

>[!NOTE]
>Eine Verfügbarkeitsgruppe mit `CLUSTER_TYPE = NONE` kann Replikate enthalten, die auf verschiedenen Betriebssystemplattformen gehostet werden. Sie kann keine Unterstützung für Hochverfügbarkeit bieten. Weitere Informationen für Linux-Betriebssysteme finden Sie unter [Konfigurieren einer SQL Server-Verfügbarkeitsgruppe zur Leseskalierung unter Linux](../../../linux/sql-server-linux-availability-group-configure-rs.md).

[!INCLUDE [Create prerequisites](../../../includes/ss-availability-group-rs-prereq.md)]

## <a name="create-an-availability-group"></a>Erstellen einer Verfügbarkeitsgruppe

Erstellen einer Verfügbarkeitsgruppe Legen Sie `CLUSTER_TYPE = NONE` fest. Darüber hinaus sollten Sie jedes Replikat mit `FAILOVER_MODE = NONE` festlegen. Clientanwendungen, die Analysen durchführen oder Berichte zu Workloads erstellen, können eine direkte Verbindung mit sekundären Datenbanken herstellen. Sie können ebenfalls eine schreibgeschützte Routingliste erstellen. Anforderungen zum Lesen der Verbindung werden dann über Verbindungen mit dem primären Replikat in Round-Robin-Manier an jedes der in der Routingliste enthaltene sekundäre Replikat weitergeleitet.

Im folgenden Transact-SQL-Skript wird eine Verfügbarkeitsgruppe namens `ag1` erstellt. Das Skript konfiguriert die Verfügbarkeitsgruppenreplikate mit `SEEDING_MODE = AUTOMATIC`. Diese Einstellung bewirkt, dass SQL Server die Datenbank automatisch auf jedem sekundären Server erstellt, nachdem diese zur Verfügbarkeitsgruppe hinzugefügt wurde. 

Aktualisieren Sie das folgende Skript für Ihre Umgebung. Ersetzen Sie die Werte `<node1>` und `<node2>` durch die Namen der SQL Server-Instanzen, auf denen die Replikate gehostet werden. Ersetzen Sie den Wert `<5022>` durch den Port, den Sie für den Endpunkt festgelegt haben. Führen Sie auf dem primären SQL Server-Replikat das folgende Transact-SQL-Skript aus:

```sql
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

### <a name="join-secondary-sql-server-instances-to-the-availability-group"></a>Verknüpfen von sekundären SQL Server-Instanzen mit der Verfügbarkeitsgruppe

Das folgende Transact-SQL-Skript verknüpft einen Server mit einer Verfügbarkeitsgruppe namens `ag1`. Aktualisieren Sie das Skript für Ihre Umgebung. Führen Sie folgendes Transact-SQL-Skript auf jedem sekundären SQL Server-Replikat aus, um die Verfügbarkeitsgruppe zu verknüpfen:

```sql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = NONE);

ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create post](../../../includes/ss-availability-group-rs-postactivity.md)]

Bei dieser Verfügbarkeitsgruppe handelt es sich nicht um eine Hochverfügbarkeitskonfiguration. Wenn Sie Hochverfügbarkeit benötigen, befolgen Sie die Anweisungen unter [Konfigurieren einer Always On-Verfügbarkeitsgruppe für SQL Server unter Linux](../../../linux/sql-server-linux-availability-group-configure-ha.md) oder unter [Erstellung und Konfiguration von Verfügbarkeitsgruppen unter Windows](creation-and-configuration-of-availability-groups-sql-server.md).

## <a name="connect-to-read-only-secondary-replicas"></a>Verbinden mit schreibgeschützten sekundären Replikaten

Es gibt zwei Möglichkeiten für die Verbindung mit schreibgeschützten sekundären Replikaten:
* Anwendungen können eine direkte Verbindung mit der SQL Server-Instanz herstellen, auf der das sekundäre Replikat gehostet wird, und die Datenbanken abfragen. Weitere Informationen finden Sie unter [Lesbare sekundäre Replikate](active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).
* Anwendungen können ebenfalls schreibgeschütztes Routing verwenden. Hierfür ist ein Listener erforderlich. Wenn Sie ein Szenario mit Leseskalierung ohne einen Cluster-Manager bereitstellen, können Sie dennoch einen Listener erstellen, der auf die IP-Adresse des aktuellen primären Replikats und des Ports verweist, die sich von jener unterscheidet, die von SQL Server überwacht wird. Nach einem Failover müssen Sie den Listener neu erstellen, um auf die neue primäre IP-Adresse zu verweisen. Weitere Informationen finden Sie unter [Schreibgeschütztes Routing](listeners-client-connectivity-application-failover.md#ConnectToSecondary).

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>Ausführen eines Failovers des primären Replikats auf eine schreibgeschützte Verfügbarkeitsgruppe

[!INCLUDE[Force failover](../../../includes/ss-force-failover-read-scale-out.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren verteilter Verfügbarkeitsgruppen](./distributed-availability-groups.md)
* [Weitere Informationen zu Verfügbarkeitsgruppen](overview-of-always-on-availability-groups-sql-server.md)
* [Ausführen eines erzwungenen manuellen Failovers](perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)