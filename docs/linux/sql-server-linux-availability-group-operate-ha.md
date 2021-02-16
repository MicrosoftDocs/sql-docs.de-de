---
title: Betreiben von Verfügbarkeitsgruppen in SQL Server für Linux
description: In diesem Artikel wird erläutert, wie Sie ein paralleles Upgrade mit SQL Server-Instanzen unter Linux mit Verfügbarkeitsgruppen durchführen. Machen Sie sich vor dem Upgrade mit den bewährten Methoden vertraut.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 00e576b6fd2fc92cca591a5ffa3d6e97556bc1ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352561"
---
# <a name="operate-always-on-availability-groups-on-linux"></a>Betreiben von Always On-Verfügbarkeitsgruppen unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

## <a name="upgrade-availability-group"></a>Upgrade einer Verfügbarkeitsgruppe

Bevor Sie ein Upgrade für eine Verfügbarkeitsgruppe durchführen, sehen Sie sich die Muster und Vorgehensweisen unter [Upgrade von Verfügbarkeitsgruppen-Replikatinstanzen](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md) an.

In den folgenden Abschnitten wird erläutert, wie Sie ein paralleles Upgrade mit SQL Server-Instanzen für Linux mit Verfügbarkeitsgruppen durchführen. 

### <a name="upgrade-steps-on-linux"></a>Upgradeschritte unter Linux

Wenn sich Verfügbarkeitsgruppenreplikate auf Instanzen von SQL Server unter Linux befinden, lautet der Clustertyp der Verfügbarkeitsgruppe entweder `EXTERNAL` oder `NONE`. Eine Verfügbarkeitsgruppe, die von einem Cluster-Manager neben dem Windows Server-Failovercluster (WSFC) verwaltet wird, ist `EXTERNAL`. Pacemaker mit Corosync ist ein Beispiel für einen externen Cluster-Manager. Eine Verfügbarkeitsgruppe ohne Cluster-Manager weist den Clustertyp `NONE` auf. Die hier beschriebenen Upgradeschritte gelten speziell Verfügbarkeitsgruppen des Clustertyps `EXTERNAL` oder `NONE`.

Die Reihenfolge, in der Sie ein Upgrade der Instanzen durchführen, richtet sich danach, ob die Rolle der Instanzen sekundär ist und ob sie synchrone oder asynchrone Replikate hosten. Führen Sie zuerst ein Upgrade für SQL Server-Instanzen durch, die asynchrone sekundäre Replikate hosten. Führen Sie danach ein Upgrade für die Instanzen durch, die synchrone sekundäre Replikate hosten. 

   >[!NOTE]
   >Wenn eine Verfügbarkeitsgruppe nur über asynchrone Replikate verfügt, ändern Sie zur Vermeidung von Datenverlusten ein Replikat in ein synchrones Replikat, und warten Sie, bis es synchronisiert ist. Führen Sie dann ein Upgrade dieses Replikats durch.
   
Bevor Sie beginnen, sichern Sie jede Datenbank.

1. Beenden Sie die Ressource auf dem Knoten, auf dem das sekundäre Replikat gehostet wird, für das ein Upgrade durchgeführt werden soll.
   
   Bevor Sie den Upgradebefehl ausführen, beenden Sie die Ressource, sodass der Cluster diese nicht mehr überwacht und unnötigerweise als „fehlerhaft“ kennzeichnet. Das folgende Beispiel fügt eine Ortseinschränkung auf dem Knoten hinzu, die dazu führt, dass die Ressource beendet wird. Aktualisieren Sie `ag_cluster-master` mit dem Ressourcennamen und `nodeName1` mit dem Knoten, auf dem das Replikat gehostet wird, für das ein Upgrade durchgeführt werden soll.

   ```bash
   pcs constraint location ag_cluster-master avoids nodeName1
   ```

1. Führen Sie ein Upgrade von SQL Server auf dem sekundären Replikat durch.

   Das folgende Beispiel führt ein Upgrade der Pakete `mssql-server` und `mssql-server-ha` aus.

   ```bash
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```
1. Entfernen Sie die Ortseinschränkung.

   Bevor Sie den Upgradebefehl ausführen, beenden Sie die Ressource, sodass der Cluster diese nicht mehr überwacht und unnötigerweise als „fehlerhaft“ kennzeichnet. Das folgende Beispiel fügt eine Ortseinschränkung auf dem Knoten hinzu, die dazu führt, dass die Ressource beendet wird. Aktualisieren Sie `ag_cluster-master` mit dem Ressourcennamen und `nodeName1` mit dem Knoten, auf dem das Replikat gehostet wird, für das ein Upgrade durchgeführt werden soll.

   ```bash
   pcs constraint remove location-ag_cluster-master-rhel1--INFINITY
   ```
   Stellen Sie als Best Practice nach dem Upgrade sicher, dass die Ressource gestartet ist (verwenden Sie dafür den Befehl `pcs status`) und das sekundäre Replikat verbunden und in synchronisiertem Zustand ist.

1. Nachdem das Upgrade für alle sekundären Replikate abgeschlossen ist, führen Sie manuell ein Failover auf eins der synchronen sekundären Replikate aus.

   Verwenden Sie bei Verfügbarkeitsgruppen mit dem Clustertyp `EXTERNAL` die Clusterverwaltungstools, um das Failover auszuführen. Bei Verfügbarkeitsgruppen mit dem Clustertyp `NONE` sollte Transact-SQL für das Failover verwendet werden. 
   Das folgende Beispiel führt ein Failover einer Verfügbarkeitsgruppe mit den Clusterverwaltungstools aus. Ersetzen `<targetReplicaName>` Sie durch den Namen des synchronen sekundären Replikats, das zum primären Replikat wird:

   ```bash
   sudo pcs resource move ag_cluster-master <targetReplicaName> --master  
   ``` 
   
   >[!IMPORTANT]
   >Die folgenden Schritte gelten nur für Verfügbarkeitsgruppen ohne Cluster-Manager.

   Wenn der Clustertyp der Verfügbarkeitsgruppe `NONE` lautet, führen Sie ein manuelles Failover aus. Führen Sie die folgenden Schritte wie folgt aus:

      a. Der folgende Befehl legt das primäre Replikat als sekundäres Replikat fest. Ersetzen Sie `AG1` durch den Namen Ihrer Verfügbarkeitsgruppe. Führen Sie den Transact-SQL-Befehl auf der SQL Server-Instanz aus, die das primäre Replikat hostet.

      ```transact-sql
      ALTER AVAILABILITY GROUP [ag1] SET (ROLE = SECONDARY);
      ```

      b. Der folgende Befehl legt ein synchrones sekundäres Replikat als primäres Replikat fest. Führen Sie den folgenden Transact-SQL-Befehl in der Zielinstanz von SQL Server aus, also der Instanz, die das synchrone sekundäre Replikat hostet.

      ```transact-sql
      ALTER AVAILABILITY GROUP [ag1] FAILOVER;
      ```

1. Führen Sie nach dem Failover auf dem alten primären Replikat ein Upgrade für SQL Server durch, indem Sie das vorherige Verfahren wiederholen.

   Das folgende Beispiel führt ein Upgrade der Pakete `mssql-server` und `mssql-server-ha` aus.

   ```bash
   # add constraint for the resource to stop on the upgraded node
   # replace 'nodename2' with the name of the cluster node targeted for upgrade
   pcs constraint location ag_cluster-master avoids nodeName2
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```
   
   ```bash
   # upgrade mssql-server and mssql-server-ha packages
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```

   ```bash
   # remove the constraint; make sure the resource is started and replica is connected and synchronized
   pcs constraint remove location-ag_cluster-master-rhel1--INFINITY
   ```

1. Bei Verfügbarkeitsgruppen mit einem externen Cluster-Manager – der Clustertyp lautet EXTERNAL – löschen Sie die Ortsbeschränkung, die durch das manuelle Failover verursacht wurde. 

   ```bash
   sudo pcs constraint remove cli-prefer-ag_cluster-master  
   ```

1. Setzen Sie die Datenverschiebung für das sekundäre Replikat mit dem neuen Upgrade fort, also das frühere primäre Replikat. Dieser Schritt ist erforderlich, wenn SQL Server-Instanz mit einer höheren Version in einer Verfügbarkeitsgruppe Protokollblöcke an eine Instanz mit einer niedrigeren Version überträgt. Führen Sie den folgenden Befehl auf dem neuen sekundären Replikat aus (dem vorherigen primären Replikat).

   ```transact-sql
   ALTER DATABASE database_name SET HADR RESUME;
   ```

Nachdem Sie das Upgrade für alle Server durchgeführt haben, können Sie ein Failback ausführen. Führen Sie ein Failover zum ursprünglichen primären Replikat durch, sofern erforderlich. 

## <a name="drop-an-availability-group"></a>Löschen einer Verfügbarkeitsgruppe

Führen Sie [DROP AVAILABILITY GROUP](../t-sql/statements/drop-availability-group-transact-sql.md) aus, um eine Verfügbarkeitsgruppe zu löschen. Wenn der Clustertyp `EXTERNAL` oder `NONE` lautet, führen Sie den Befehl in jeder Instanz von SQL Server aus, die ein Replikat hostet. Um beispielsweise eine Verfügbarkeitsgruppe namens `group_name`zu löschen, führen Sie folgenden Befehl aus:

   ```transact-sql
   DROP AVAILABILITY GROUP group_name
   ```
 

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren des Red Hat Enterprise Linux-Clusters für Clusterressourcen von SQL Server-Verfügbarkeitsgruppen](sql-server-linux-availability-group-cluster-rhel.md)

[Konfigurieren des SUSE Linux Enterprise Server-Clusters für Clusterressourcen von SQL Server-Verfügbarkeitsgruppen](sql-server-linux-availability-group-cluster-sles.md)

[Konfigurieren des Ubuntu-Clusters für Clusterressourcen von SQL Server-Verfügbarkeitsgruppen](sql-server-linux-availability-group-cluster-ubuntu.md)
