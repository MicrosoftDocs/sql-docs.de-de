---
title: Konfigurieren von RHEL-FCI für SQL Server für Linux
description: Informieren Sie sich, wie Sie eine RHEL-Failoverclusterinstanz auf einem freigegebenen Datenträger für SQL Server unter Linux (Hochverfügbarkeit) konfigurieren.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: dcc0a8d3-9d25-4208-8507-a5e65d2a9a15
ms.openlocfilehash: f635a10d0a1165fb3f657e41147fe023f55d2874
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346278"
---
# <a name="configure-rhel-failover-cluster-instance-fci-cluster-for-sql-server"></a>Konfigurieren eines RHEL-FCI-Clusters (Failoverclusterinstanz) für SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Diese Anleitung enthält Anweisungen zum Erstellen eines freigegebenen Datenträgerfailoverclusters mit zwei Knoten für SQL Server unter Red Hat Enterprise Linux. Die Clusteringebene basiert auf einem [Hochverfügbarkeits-Add-On](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) für Red Hat Enterprise Linux (RHEL), das auf [Pacemaker](https://clusterlabs.org/) aufbaut. Die SQL Server-Instanz ist auf einem der beiden Knoten aktiv.

> [!NOTE] 
> Für den Zugriff auf das Red Hat-Hochverfügbarkeits-Add-On und die Dokumentation ist ein Abonnement erforderlich. 

Wie in der folgenden Abbildung zu sehen, wird der Speicher zwei Servern präsentiert. Clusteringkomponenten – Corosync und Pacemaker – koordinieren die Kommunikation und die Ressourcenverwaltung. Ein Server verfügt über die aktive Verbindung mit den Speicherressourcen und SQL Server. Wenn Pacemaker einen Fehler erkennt, übernehmen die Clusteringkomponenten das Verschieben der Ressourcen auf den anderen Knoten.  

![Freigegebener SQL-Datenträgercluster mit Red Hat Enterprise Linux 7](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 

Weitere Informationen zur Clusterkonfiguration, den Optionen für Ressourcen-Agents und der Verwaltung finden Sie in der [Referenzdokumentation von RHEL](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html).


> [!NOTE] 
> An diesem Punkt ist die Integration von SQL Server in Pacemaker nicht so stark gekoppelt wie in WSFC unter Windows. Aus SQL kann der Cluster nicht erkannt werden. Die gesamte Orchestrierung erfolgt außerhalb durch Pacemaker. Zudem wird der Dienst als eigenständige Instanz von Pacemaker gesteuert. Außerdem sind beispielsweise für die Cluster „dmvs sys.dm_os_cluster_nodes“ und „sys.dm_os_cluster_properties“ keine Datensätze vorhanden.
Wenn Sie eine Verbindungszeichenfolge verwenden möchten, die auf einen Zeichenfolgenservernamen zeigt und nicht die IP-Adresse verwendet, müssen Sie die IP-Adresse, die (wie in den folgenden Abschnitten beschrieben) zum Erstellen der virtuellen IP-Ressource verwendet wird, mit dem ausgewählten Servernamen beim DNS-Server registrieren.

In den folgenden Abschnitten werden die Schritte zum Einrichten einer Failoverclusterlösung erläutert. 

## <a name="prerequisites"></a>Voraussetzungen

Für das folgende End-to-End-Szenario benötigen Sie zwei Computer, um den Cluster mit zwei Knoten und einen weiteren Server zum Konfigurieren des NFS-Servers bereitzustellen. Die folgenden Schritte beschreiben, wie diese Server konfiguriert werden.

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>Einrichten und Konfigurieren des Betriebssystems auf den einzelnen Clusterknoten

Der erste Schritt besteht darin, das Betriebssystem auf den Clusterknoten zu konfigurieren. Verwenden Sie für diese exemplarische Vorgehensweise RHEL mit einem gültigen Abonnement für das Hochverfügbarkeits-Add-On. 

## <a name="install-and-configure-sql-server-on-each-cluster-node"></a>Installieren und Konfigurieren der SQL Server-Instanz auf den einzelnen Clusterknoten

1. Installieren Sie SQL Server auf beiden Knoten, und richten Sie die Anwendung ein.  Ausführliche Anweisungen finden Sie unter [Installieren von SQL Server für Linux](sql-server-linux-setup.md).

1. Legen Sie für die Konfiguration einen Knoten als primär und den anderen als sekundär fest. Verwenden Sie diese Begriffe für den weiteren Verlauf dieses Leitfadens.  

1. Beenden und deaktivieren Sie SQL Server auf dem sekundären Knoten.

   Im folgenden Beispiel wird SQL Server beendet und deaktiviert: 

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl disable mssql-server
   ```
> [!NOTE] 
> Zum Zeitpunkt der Einrichtung wird ein Serverhauptschlüssel für die SQL Server-Instanz generiert und unter `/var/opt/mssql/secrets/machine-key` platziert. Unter Linux wird SQL Server immer als lokales Konto mit dem Namen „mssql“ ausgeführt. Da es sich um ein lokales Konto handelt, wird dessen Identität nicht knotenübergreifend freigegeben. Daher müssen Sie den Verschlüsselungsschlüssel vom primären Knoten auf jeden sekundären Knoten kopieren, damit jedes lokale mssql-Konto darauf zugreifen kann, um den Serverhauptschlüssel zu entschlüsseln. 

1. Erstellen Sie auf dem primären Knoten einen SQL Server-Anmeldenamen für Pacemaker, und erteilen Sie dem Anmeldenamen die Berechtigung zum Ausführen von `sp_server_diagnostics`. Pacemaker verwendet dieses Konto, um zu überprüfen, welcher Knoten auf SQL Server ausgeführt wird. 

   ```bash
   sudo systemctl start mssql-server
   ```

   Stellen Sie mithilfe des sa-Kontos eine Verbindung mit der `master`-Datenbank von SQL Server her, und führen Sie Folgendes aus:

   ```bashsql
   USE [master]
   GO
   CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'

   ALTER SERVER ROLE [sysadmin] ADD MEMBER [<loginName>]
   ```
   Alternativ können Sie die Berechtigungen detaillierter festlegen. Der Pacemaker-Anmeldename benötigt `VIEW SERVER STATE`, um mithilfe von sp_server_diagnostics den Integritätsstatus abzufragen, sowie `setupadmin` und `ALTER ANY LINKED SERVER`, um den Namen der Failoverclusterinstanz mit dem Ressourcennamen zu aktualisieren, indem sp_dropserver und sp_addserver ausgeführt werden. 

1. Beenden und deaktivieren Sie SQL Server auf dem primären Knoten. 

1. Konfigurieren Sie die Hostdatei für jeden Clusterknoten. Die Hostdatei muss die IP-Adresse und den Namen jedes Clusterknotens enthalten. 

    Überprüfen Sie die IP-Adresse jedes Knotens. Das folgende Skript zeigt die IP-Adresse des aktuellen Knotens an. 

   ```bash
   sudo ip addr show
   ```

   Legen Sie den Computernamen auf jedem Knoten fest. Geben Sie jedem Knoten einen eindeutigen Namen, der höchstens 15 Zeichen lang ist. Legen Sie den Computernamen fest, indem Sie diesen zu `/etc/hosts` hinzufügen. Mithilfe des folgenden Skripts können Sie `/etc/hosts` mit `vi` bearbeiten. 

   ```bash
   sudo vi /etc/hosts
   ```
   Das folgende Beispiel zeigt `/etc/hosts` mit Ergänzungen für zwei Knoten mit den Namen `sqlfcivm1` und `sqlfcivm2`.

   ```bash
   127.0.0.1   localhost localhost4 localhost4.localdomain4
   ::1       localhost localhost6 localhost6.localdomain6
   10.128.18.128 sqlfcivm1
   10.128.16.77 sqlfcivm2
   ```

Im nächsten Abschnitt konfigurieren Sie den freigegebenen Speicher und verschieben Ihre Datenbankdateien in diesen Speicher. 

## <a name="configure-shared-storage-and-move-database-files"></a>Konfigurieren von freigegebenem Speicher und Verschieben von Datenbankdateien 

Es gibt eine Reihe von Lösungen für die Bereitstellung von freigegebenem Speicher. Diese exemplarische Vorgehensweise veranschaulicht das Konfigurieren von freigegebenem Speicher mit NFS. Es wird empfohlen, auf die bewährten Methoden zurückzugreifen und Kerberos zum Sichern von NFS zu verwenden (ein Beispiel finden Sie hier: https://www.certdepot.net/rhel7-use-kerberos-control-access-nfs-network-shares/). 

>[!Warning]
>Wenn Sie NFS nicht sichern, kann jeder Benutzer, der auf Ihr Netzwerk zugreifen und die IP-Adresse eines SQL-Knotens spoofen kann, auf Ihre Datendateien zugreifen. Stellen Sie wie immer sicher, dass Sie ein Gefahrenmodell für Ihr System erstellen, bevor Sie es in der Produktion verwenden. Eine andere Speicheroption ist die Verwendung der SMB-Dateifreigabe.

### <a name="configure-shared-storage-with-nfs"></a>Konfigurieren von freigegebenem Speicher mit NFS

> [!IMPORTANT] 
> Das Hosting von Datenbankdateien auf einem NFS-Server mit Version <4 wird in dieser Version nicht unterstützt. Dies schließt die Verwendung von NFS für das Failoverclustering für freigegebene Datenträger sowie für Datenbanken auf nicht gruppierten Instanzen ein. Wir arbeiten daran, in zukünftigen Releases andere Versionen des NFS-Servers zu ermöglichen. 

Gehen Sie auf dem NFS-Server wie folgt vor:

1. Installieren von `nfs-utils`

   ```bash
   sudo yum -y install nfs-utils
   ```

1. Aktivieren und starten Sie `rpcbind`.

   ```bash
   sudo systemctl enable rpcbind && sudo systemctl start rpcbind
   ```

1. Aktivieren und starten Sie `nfs-server`.
 
   ```bash
   sudo systemctl enable nfs-server && sudo systemctl start nfs-server
   ```
 
1.  Bearbeiten Sie `/etc/exports`, um das Verzeichnis zu exportieren, das Sie freigeben möchten. Sie benötigen eine Zeile für jede gewünschte Freigabe. Beispiel: 

   ```bash
   /mnt/nfs  10.8.8.0/24(rw,sync,no_subtree_check,no_root_squash)
   ```

1. Exportieren Sie die Freigaben.

   ```bash
   sudo exportfs -rav
   ```

1. Stellen Sie sicher, dass die Pfade freigegeben/exportiert wurden. Führen Sie dazu Folgendes auf dem NFS-Server aus:

   ```bash
   sudo showmount -e
   ```

1. Fügen Sie in SELinux eine Ausnahme hinzu.

   ```bash
   sudo setsebool -P nfs_export_all_rw 1
   ```
   
1. Öffnen Sie die Firewall des Servers.

   ```bash 
   sudo firewall-cmd --permanent --add-service=nfs
   sudo firewall-cmd --permanent --add-service=mountd
   sudo firewall-cmd --permanent --add-service=rpc-bind
   sudo firewall-cmd --reload
   ```

### <a name="configure-all-cluster-nodes-to-connect-to-the-nfs-shared-storage"></a>Konfigurieren aller Clusterknoten für die Verbindung mit dem freigegebenen NFS-Speicher

Führen Sie die folgenden Schritte auf allen Clusterknoten durch.

1.  Installieren von `nfs-utils`

   ```bash
   sudo yum -y install nfs-utils
   ```

1. Öffnen Sie die Firewall auf Clients und dem NFS-Server.

   ```bash
   sudo firewall-cmd --permanent --add-service=nfs
   sudo firewall-cmd --permanent --add-service=mountd
   sudo firewall-cmd --permanent --add-service=rpc-bind
   sudo firewall-cmd --reload
   ```

1. Vergewissern Sie sich, dass die NFS-Freigaben auf Clientcomputern angezeigt werden.

   ```bash
   sudo showmount -e <IP OF NFS SERVER>
   ```

1. Wiederholen Sie diese Schritte auf allen Clusterknoten.

Weitere Informationen zur Verwendung von NFS finden Sie unter folgenden Quellen:

* [NFS servers and firewalld | Stack Exchange (NFS-Server und firewalld | Stack Exchange)](https://unix.stackexchange.com/questions/243756/nfs-servers-and-firewalld)
* [Mounting an NFS Volume | Linux Network Administrators Guide (Einbinden eines NFS-Volumes | Leitfaden für Linux-Netzwerkadministratoren)](https://www.tldp.org/LDP/nag2/x-087-2-nfs.mountd.html)
* [NFS server configuration | Red Hat Customer Portal (NFS-Serverkonfiguration | Red Hat-Kundenportal)](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/nfs-serverconfig)

### <a name="mount-database-files-directory-to-point-to-the-shared-storage"></a>Einbinden des Datenbankdateiverzeichnisses zum Zeigen auf den freigegebenen Speicher

1.  Speichern Sie die Datenbankdateien **nur auf dem primären Knoten** an einem temporären Speicherort. Das folgende Skript erstellt ein neues temporäres Verzeichnis, kopiert die Datenbankdateien in das neue Verzeichnis und entfernt die alten Datenbankdateien. Da SQL Server als lokaler Benutzer „mssql“ ausgeführt wird, müssen Sie sicherstellen, dass der lokale Benutzer nach der Datenübertragung zur eingebundenen Freigabe Lese- und Schreibzugriff auf die Freigabe hat. 

   ``` 
   $ sudo su mssql
   $ mkdir /var/opt/mssql/tmp
   $ cp /var/opt/mssql/data/* /var/opt/mssql/tmp
   $ rm /var/opt/mssql/data/*
   $ exit
   ``` 

1.  Bearbeiten Sie die Datei `/etc/fstab` auf allen Clusterknoten, um den Befehl zum Einbinden einzuschließen.  

   ```bash
   <IP OF NFS SERVER>:<shared_storage_path> <database_files_directory_path> nfs timeo=14,intr 
   ```
   
   Das folgende Skript ist ein Beispiel hierfür.  

   ``` 
   10.8.8.0:/mnt/nfs /var/opt/mssql/data nfs timeo=14,intr 
   ``` 
> [!NOTE] 
>Wenn Sie wie hier empfohlen eine File System-Ressource (FS) verwenden, muss der Befehl zum Einbinden in „/etc/fstab“ nicht beibehalten werden. Pacemaker übernimmt die Einbindung des Ordners beim Starten der in FS gruppierten Ressource. Mithilfe von Fencing wird sichergestellt, dass FS nie zweimal eingebunden wird. 

1.  Führen Sie den Befehl `mount -a` aus, damit das System die eingebundenen Pfade aktualisiert.  

1.  Kopieren Sie die Datenbank und die Protokolldateien, die Sie in `/var/opt/mssql/tmp` gespeichert haben, in die neu eingebundene Freigabe `/var/opt/mssql/data`. Diesen Schritt müssen Sie nur **auf dem primären Knoten** durchführen. Stellen Sie sicher, dass Sie dem lokalen Benutzer „mssql“ Lese- und Schreibberechtigungen erteilen.

   ``` 
   $ sudo chown mssql /var/opt/mssql/data
   $ sudo chgrp mssql /var/opt/mssql/data
   $ sudo su mssql
   $ cp /var/opt/mssql/tmp/* /var/opt/mssql/data/
   $ rm /var/opt/mssql/tmp/*
   $ exit
   ``` 
 
1.  Überprüfen Sie, ob SQL Server mit dem neuen Dateipfad erfolgreich gestartet wurde. Führen Sie dies auf jedem Knoten durch. An diesem Punkt sollte SQL Server immer nur auf einem Knoten ausgeführt werden. Sie können SQL Server nicht auf beiden Knoten gleichzeitig ausführen, da diese ansonsten versuchen würden, gleichzeitig auf die Datendateien zuzugreifen (verwenden Sie zur Vermeidung eines versehentlichen Starts von SQL Server auf beiden Knoten eine File System-Clusterressource, um sicherzustellen, dass die Freigabe nicht zweimal von verschiedenen Knoten eingebunden wird). Die folgenden Befehle starten SQL Server, überprüfen den Status und beenden SQL Server dann.
 
   ```bash
   sudo systemctl start mssql-server
   sudo systemctl status mssql-server
   sudo systemctl stop mssql-server
   ```
 
An diesem Punkt sind beide Instanzen von SQL Server so konfiguriert, dass sie mit den Datenbankdateien im freigegebenen Speicher ausgeführt werden. Der nächste Schritt besteht darin, SQL Server für Pacemaker zu konfigurieren. 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installieren und Konfigurieren von Pacemaker auf jedem Clusterknoten


2. Erstellen Sie auf beiden Clusterknoten eine Datei zum Speichern von Benutzername und Kennwort für SQL Server für die Pacemaker-Anmeldung. Der folgende Code erstellt und füllt diese Tabelle:

   ```bash
   sudo touch /var/opt/mssql/secrets/passwd
   echo '<loginName>' | sudo tee -a /var/opt/mssql/secrets/passwd
   echo '<loginPassword>' | sudo tee -a /var/opt/mssql/secrets/passwd
   sudo chown root:root /var/opt/mssql/secrets/passwd 
   sudo chmod 600 /var/opt/mssql/secrets/passwd    
   ```

3. Öffnen Sie auf beiden Clusterknoten die Pacemaker-Firewallports. Führen Sie zum Öffnen dieser Ports mit `firewalld` folgenden Befehl aus:

   ```bash
   sudo firewall-cmd --permanent --add-service=high-availability
   sudo firewall-cmd --reload
   ```

   > Wenn Sie eine andere Firewall verwenden, in die keine Konfiguration mit Hochverfügbarkeit integriert ist, müssen die folgenden Ports geöffnet werden, damit Pacemaker mit anderen Knoten im Cluster kommunizieren kann:
   >
   > * TCP: Ports 2224, 3121, 21064
   > * UDP: Port 5405

1. Installieren Sie Pacemaker-Pakete auf jedem Knoten.

   ```bash
   sudo yum install pacemaker pcs fence-agents-all resource-agents
   ```

    

2. Legen Sie das Kennwort für den Standardbenutzer fest, der beim Installieren von Pacemaker und Corosync-Paketen erstellt wird. Verwenden Sie auf beiden Knoten dasselbe Kennwort. 

   ```bash
   sudo passwd hacluster
   ```

    

3. Aktivieren und starten Sie den `pcsd`-Dienst und Pacemaker. So können Knoten dem Cluster nach dem Neustart erneut beitreten. Führen Sie den folgenden Befehl auf beiden Knoten aus.

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

4. Installieren Sie den FCI-Ressourcenagent für SQL Server. Führen Sie die folgenden Befehle auf beiden Knoten aus. 

   ```bash
   sudo yum install mssql-server-ha
   ```

## <a name="configure-fencing-agent"></a>Konfigurieren des Fencing-Agents

Ein STONITH-Gerät stellt einen Fencing-Agent bereit. Ein Beispiel für das Erstellen eines STONITH-Geräts für diesen Cluster in Azure finden Sie unter [Einrichten von Pacemaker unter Red Hat Enterprise Linux in Azure](/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker/#1-create-the-stonith-devices). Ändern Sie die Anweisungen für Ihre Umgebung.

## <a name="create-the-cluster"></a>Erstellen Sie den Cluster. 

1. Erstellen Sie auf einem der Knoten den Cluster.

   ```bash
   sudo pcs cluster auth <nodeName1 nodeName2 ...> -u hacluster
   sudo pcs cluster setup --name <clusterName> <nodeName1 nodeName2 ...>
   sudo pcs cluster start --all
   ```

2. Konfigurieren Sie die Clusterressourcen für SQL Server-, File System- und virtuelle IP-Ressourcen, und pushen Sie die Konfiguration zum Cluster. Sie benötigen die folgenden Informationen:

   - **SQL Server-Ressourcenname**: Ein Name für die gruppierte SQL Server-Ressource 
   - **Floating IP-Ressourcenname**: Ein Name für die virtuelle IP-Adressenressource
   - **IP-Adresse**: Die IP-Adresse, die von Clients zum Herstellen einer Verbindung mit der gruppierten Instanz von SQL Server verwendet wird 
   - **File System-Ressourcenname**: Ein Name für die File System-Ressource
   - **device**: Der NFS-Freigabepfad
   - **device**: Der lokale Pfad, der in die Freigabe eingebunden wird
   - **fstype**: Typ der Dateifreigabe (d. h. NFS)

   Aktualisieren Sie die Werte aus dem folgenden Skript für Ihre Umgebung. Führen Sie es auf einem Knoten aus, um den gruppierten Dienst zu konfigurieren und zu starten.  

   ```bash
   sudo pcs cluster cib cfg 
   sudo pcs -f cfg resource create <sqlServerResourceName> ocf:mssql:fci
   sudo pcs -f cfg resource create <floatingIPResourceName> ocf:heartbeat:IPaddr2 ip=<ip Address>
   sudo pcs -f cfg resource create <fileShareResourceName> Filesystem device=<networkPath> directory=<localPath>         fstype=<fileShareType>
   sudo pcs -f cfg constraint colocation add <virtualIPResourceName> <sqlResourceName>
   sudo pcs -f cfg constraint colocation add <fileShareResourceName> <sqlResourceName> 
   sudo pcs cluster cib-push cfg
   ```

   Das folgende Skript erstellt beispielsweise eine gruppierte SQL Server-Ressource mit dem Namen `mssqlha` sowie eine Floating IP-Ressource mit der IP-Adresse `10.0.0.99`. Außerdem wird eine File System-Ressource erstellt sowie Einschränkungen hinzugefügt, sodass alle Ressourcen auf demselben Knoten als SQL-Ressource verbunden werden. 

   ```bash
   sudo pcs cluster cib cfg
   sudo pcs -f cfg resource create mssqlha ocf:mssql:fci
   sudo pcs -f cfg resource create virtualip ocf:heartbeat:IPaddr2 ip=10.0.0.99
   sudo pcs -f cfg resource create fs Filesystem device="10.8.8.0:/mnt/nfs" directory="/var/opt/mssql/data" fstype="nfs"
   sudo pcs -f cfg constraint colocation add virtualip mssqlha
   sudo pcs -f cfg constraint colocation add fs mssqlha
   sudo pcs cluster cib-push cfg
   ```

   Nach dem Pushen der Konfiguration wird SQL Server auf einem Knoten gestartet. 

3. Stellen Sie sicher, dass SQL Server gestartet wird. 

   ```bash
   sudo pcs status 
   ```

   Die folgenden Beispiele zeigen die Ergebnisse, wenn Pacemaker erfolgreich eine gruppierte Instanz von SQL Server gestartet hat. 

   ```
   fs     (ocf::heartbeat:Filesystem):    Started sqlfcivm1
   virtualip     (ocf::heartbeat:IPaddr2):      Started sqlfcivm1
   mssqlha  (ocf::mssql:fci): Started sqlfcivm1
   
   PCSD Status:
    sqlfcivm1: Online
    sqlfcivm2: Online
   
   Daemon Status:
    corosync: active/disabled
    pacemaker: active/enabled
    pcsd: active/enabled
   ```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Detaillierte Anleitung für das Erstellen von Clustern](https://clusterlabs.org/doc/Cluster_from_Scratch.pdf) von Pacemaker

## <a name="next-steps"></a>Nächste Schritte

[Betreiben von SQL Server auf einem freigegebenen Datenträgercluster mit Red Hat Enterprise Linux](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
