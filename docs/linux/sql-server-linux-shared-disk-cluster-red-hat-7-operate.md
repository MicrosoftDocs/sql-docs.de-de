---
title: Ausführen von RHEL-FCI für SQL Server für Linux
description: In diesem Artikel erfahren Sie, wie Sie in Red Hat Enterprise Linux (RHEL) eine SQL Server-Failoverclusterinstanz (FCI) für die Hochverfügbarkeit eines freigegebenen Datenträgers ausführen, z. B. um ein Failover für die FCI manuell auszuführen oder um dem Cluster Knoten hinzuzufügen oder sie daraus zu entfernen.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 075ab7d8-8b68-43f3-9303-bbdf00b54db1
ms.openlocfilehash: 272bc33012e37c58f6a3f8ea0ed51f1c216d716f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346277"
---
# <a name="operate-rhel-failover-cluster-instance-fci-for-sql-server"></a>Ausführen einer RHEL-Failoverclusterinstanz für SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel wird beschrieben, wie Sie die folgenden Aufgaben für SQL Server auf einem freigegebenen Datenträger-Failovercluster mit Red Hat Enterprise Linux ausführen.

- Durchführen eines manuellen Clusterfailovers
- Überwachen eines SQL Server-Diensts für Failovercluster
- Hinzufügen eines Clusterknotens
- Entfernen eines Clusterknotens
- Ändern der Häufigkeit der SQL Server-Ressourcenüberwachung

## <a name="architecture-description"></a>Beschreibung der Architektur

Die Clusteringebene basiert auf einem [Hochverfügbarkeits-Add-On](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) für Red Hat Enterprise Linux (RHEL), das auf [Pacemaker](https://clusterlabs.org/) aufbaut. Corosync und Pacemaker koordinieren die Clusterkommunikation und die Ressourcenverwaltung. Die SQL Server-Instanz ist auf einem der beiden Knoten aktiv.

Das folgende Diagramm veranschaulicht die Komponenten in einem Linux-Cluster mit SQL Server. 

![Freigegebener SQL-Datenträgercluster mit Red Hat Enterprise Linux 7](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 

Weitere Informationen zur Clusterkonfiguration, den Optionen für Ressourcen-Agents und der Verwaltung finden Sie in der [Referenzdokumentation von RHEL](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html).

## <a name="failover-cluster-manually"></a><a name = "failManual"></a>Durchführen eines manuellen Clusterfailovers

Der Befehl `resource move` erstellt eine Einschränkung, die die Ressource zwingt, auf dem Zielknoten zu starten.    Nachdem der Befehl `move` ausgeführt wurde, entfernt die ausführende Ressource `clear` die Einschränkung, sodass die Ressource erneut verschoben oder ein automatisches Failover für die Ressource durchgeführt werden kann. 

```bash
sudo pcs resource move <sqlResourceName> <targetNodeName>  
sudo pcs resource clear <sqlResourceName> 
```

Im folgenden Beispiel wird die Ressource **mssqlha** auf den Knoten **sqlfcivm2** verschoben. Dann wird die Einschränkung entfernt, sodass die Ressource später auf einen anderen Knoten verschoben werden kann.  

```bash
sudo pcs resource move mssqlha sqlfcivm2 
sudo pcs resource clear mssqlha 
```

## <a name="monitor-a-failover-cluster-sql-server-service"></a>Überwachen eines SQL Server-Diensts für Failovercluster

Anzeigen des aktuellen Clusterstatus:

```bash
sudo pcs status  
```

Anzeigen des aktuellen Status von Cluster und Ressourcen:

```bash
sudo crm_mon 
```

Die Protokolle des Ressourcen-Agents können Sie unter `/var/log/cluster/corosync.log` anzeigen

## <a name="add-a-node-to-a-cluster"></a>Hinzufügen eines Knotens zu einem Cluster

1. Überprüfen Sie die IP-Adresse jedes Knotens. Das folgende Skript zeigt die IP-Adresse des aktuellen Knotens an. 

   ```bash
   ip addr show
   ```

3. Der neue Knoten benötigt einen eindeutigen Namen, der höchstens 15 Zeichen lang ist. In Red Hat Enterprise Linux ist der Computername standardmäßig `localhost.localdomain`. Dieser Standardname ist möglicherweise nicht eindeutig und außerdem zu lang. Legen Sie den Computernamen für den neuen Knoten fest. Legen Sie den Computernamen fest, indem Sie diesen zu `/etc/hosts` hinzufügen. Mithilfe des folgenden Skripts können Sie `/etc/hosts` mit `vi` bearbeiten. 

   ```bash
   sudo vi /etc/hosts
   ```

   Das folgende Beispiel zeigt `/etc/hosts` mit Ergänzungen für drei Knoten mit den Namen `sqlfcivm1`, `sqlfcivm2` und `sqlfcivm3`.

   ```
   127.0.0.1   localhost localhost4 localhost4.localdomain4
   ::1         localhost localhost6 localhost6.localdomain6
   10.128.18.128 fcivm1
   10.128.16.77 fcivm2
   10.128.14.26 fcivm3
    ```
    
   Die Datei sollte auf jedem Knoten dieselbe sein. 

1. Beenden Sie den SQL Server-Dienst auf dem neuen Knoten.

1. Befolgen Sie die Anweisungen, um das Datenbankdatei-Verzeichnis am freigegebenen Speicherort einzubinden:

   Installieren Sie vom NFS-Server aus `nfs-utils`:

   ```bash
   sudo yum -y install nfs-utils 
   ``` 

   Öffnen Sie die Firewall auf Clients und dem NFS-Server. 

   ```bash
   sudo firewall-cmd --permanent --add-service=nfs
   sudo firewall-cmd --permanent --add-service=mountd
   sudo firewall-cmd --permanent --add-service=rpc-bind
   sudo firewall-cmd --reload
   ```

   Bearbeiten Sie die Datei „/etc/fstab“ so, dass sie den Einbindungsbefehl enthält: 

   ```bash
   <IP OF NFS SERVER>:<shared_storage_path> <database_files_directory_path> nfs timeo=14,intr
   ```

   Führen Sie `mount -a` aus, damit die Änderungen wirksam werden.
   
1. Erstellen Sie auf dem neuen Knoten eine Datei zum Speichern von Benutzername und Kennwort für SQL Server für die Pacemaker-Anmeldung. Der folgende Code erstellt und füllt diese Tabelle:

   ```bash
   sudo touch /var/opt/mssql/passwd
   sudo echo "<loginName>" >> /var/opt/mssql/secrets/passwd
   sudo echo "<loginPassword>" >> /var/opt/mssql/secrets/passwd
   sudo chown root:root /var/opt/mssql/passwd
   sudo chmod 600 /var/opt/mssql/passwd
   ```

3. Öffnen Sie auf dem neuen Knoten die Pacemaker-Firewallports. Führen Sie zum Öffnen dieser Ports mit `firewalld` folgenden Befehl aus:

   ```bash
   sudo firewall-cmd --permanent --add-service=high-availability
   sudo firewall-cmd --reload
   ```

   > [!NOTE]
   > Wenn Sie eine andere Firewall verwenden, in die keine Konfiguration mit Hochverfügbarkeit integriert ist, müssen die folgenden Ports geöffnet werden, damit Pacemaker mit anderen Knoten im Cluster kommunizieren kann:
   >
   > * TCP: Ports 2224, 3121, 21064
   > * UDP: Port 5405

1. Installieren Sie Pacemaker-Pakete auf dem neuen Knoten.

   ```bash
   sudo yum install pacemaker pcs fence-agents-all resource-agents
   ```
 
2. Legen Sie das Kennwort für den Standardbenutzer fest, der beim Installieren von Pacemaker und Corosync-Paketen erstellt wird. Verwenden Sie dasselbe Kennwort wie bei den vorhandenen Knoten. 

   ```bash
   sudo passwd hacluster
   ```
 
3. Aktivieren und starten Sie den `pcsd`-Dienst und Pacemaker. So kann der neue Knoten dem Cluster nach dem Neustart erneut beitreten. Führen Sie den folgenden Befehl auf dem neuen Knoten aus.

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

4. Installieren Sie den FCI-Ressourcenagent für SQL Server. Führen Sie die folgenden Befehle auf dem neuen Knoten aus. 

   ```bash
   sudo yum install mssql-server-ha
   ```

1. Authentifizieren Sie den neuen Knoten vom Cluster aus auf einem vorhandenen Knoten, und fügen Sie ihn dem Cluster hinzu.

    ```bash
    sudo pcs    cluster auth <nodeName3> -u hacluster 
    sudo pcs    cluster node add <nodeName3> 
    ```

    Im folgenden Beispiel wird der Knoten **vm3** dem Cluster hinzugefügt.

    ```bash
    sudo pcs    cluster auth  
    sudo pcs    cluster start 
    ```

## <a name="remove-nodes-from-a-cluster"></a>Entfernen von Knoten aus einem Cluster

Führen Sie den folgenden Befehl aus, um einen Knoten aus einem Cluster zu entfernen:

```bash
sudo pcs    cluster node remove <nodeName>  
```

## <a name="change-the-frequency-of-sqlservr-resource-monitoring-interval"></a>Ändern der Häufigkeit der SQL Server-Ressourcenüberwachung

```bash
sudo pcs    resource op monitor interval=<interval>s <sqlResourceName> 
```

Im folgenden Beispiel wird das Überwachungsintervall für die Ressource „mssql“ auf 2 Sekunden festgelegt:

```bash
sudo pcs    resource op monitor interval=2s mssqlha 
```
## <a name="troubleshoot-red-hat-enterprise-linux-shared-disk-cluster-for-sql-server"></a>Problembehandlung für einen freigegebenen Datenträgercluster mit Red Hat Enterprise Linux für SQL Server

Für die Behandlung von Problemen mit dem Cluster kann es hilfreich sein, zu verstehen, wie die drei Daemons bei der Verwaltung von Clusterressourcen zusammenarbeiten. 

| Daemon | BESCHREIBUNG 
| ----- | -----
| Corosync | Ermöglicht Quorum-Mitgliedschaft und Messaging zwischen Clusterknoten
| Pacemaker | Baut auf Corosync auf und bietet Zustandsautomaten für Ressourcen 
| PCSD | Verwaltet Pacemaker und Corosync über die `pcs`-Tools

PCSD muss ausgeführt werden, damit die `pcs`-Tools verwendet werden können. 

### <a name="current-cluster-status"></a>Aktueller Clusterstatus 

`sudo pcs status` gibt grundlegende Informationen zu Cluster, Quorum, Knoten, Ressourcen und Daemonstatus für jeden Knoten zurück. 

Eine fehlerfreie Pacemaker-Quorumausgabe könnte wie folgt aussehen:

```
Cluster name: MyAppSQL 
Last updated: Wed Oct 31 12:00:00 2016  Last change: Wed Oct 31 11:00:00 2016 by root via crm_resource on sqlvmnode1 
Stack: corosync 
Current DC: sqlvmnode1  (version 1.1.13-10.el7_2.4-44eb2dd) - partition with quorum 
3 nodes and 1 resource configured 

Online: [ sqlvmnode1 sqlvmnode2 sqlvmnode3] 

Full list of resources: 

mssqlha (ocf::sql:fci): Started sqlvmnode1 

PCSD Status: 
sqlvmnode1: Online 
sqlvmnode2: Online 
sqlvmnode3: Online 

Daemon Status: 
corosync: active/disabled 
pacemaker: active/enabled 
```

Im Beispiel bedeutet `partition with quorum`, dass ein Mehrheitsquorum von Knoten online ist. Wenn der Cluster ein Mehrheitsquorum von Knoten verliert, gibt `pcs status``partition WITHOUT quorum` zurück, und alle Ressourcen werden angehalten. 

`online: [sqlvmnode1 sqlvmnode2 sqlvmnode3]` gibt die Namen aller Knoten zurück, die aktuell am Cluster beteiligt sind. Wenn ein oder mehrere Knoten nicht beteiligt sind, gibt `pcs status``OFFLINE: [<nodename>]` zurück.

`PCSD Status` zeigt den Clusterstatus für jeden Knoten an.

### <a name="reasons-why-a-node-may-be-offline"></a>Gründe, warum ein Knoten offline sein kann

Überprüfen Sie Folgendes, wenn ein Knoten offline ist:

- **Firewall**

    Die folgenden Ports müssen auf allen Knoten geöffnet sein, damit Pacemaker kommunizieren kann:
    
    - **TCP: 2224, 3121, 21064

- **Ausführung von Pacemaker oder Corosync**

- **Knotenkommunikation**

- **Zuordnungen von Knotennamen**

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Detaillierte Anleitung für das Erstellen von Clustern](https://clusterlabs.org/doc/Cluster_from_Scratch.pdf) von Pacemaker

## <a name="next-steps"></a>Nächste Schritte

[Configure Red Hat Enterprise Linux shared disk cluster for SQL Server (Konfigurieren eines freigegebenen Datenträgerclusters mit Red Hat Enterprise Linux für SQL Server)](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)

