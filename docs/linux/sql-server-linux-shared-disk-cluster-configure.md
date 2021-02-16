---
title: Konfigurieren von FCI – SQL Server für Linux (RHEL)
description: Hier erfahren Sie, wie Sie eine Failoverclusterinstanz (FCI) unter Red Hat Enterprise Linux (RHEL) für SQL Server konfigurieren.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 31c8c92e-12fe-4728-9b95-4bc028250d85
ms.openlocfilehash: b2b97fa250b5af1a0030b51b0ea29a89324db9c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346298"
---
# <a name="configure-failover-cluster-instance---sql-server-on-linux-rhel"></a>Konfigurieren einer Failoverclusterinstanz: SQL Server für Linux (RHEL)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Eine SQL Server-Failoverclusterinstanz mit zwei Knoten auf einem freigegebenen Datenträger bietet Redundanz für Hochverfügbarkeit auf Serverebene. In diesem Tutorial lernen Sie, wie Sie eine Failoverclusterinstanz von SQL Server für Linux mit zwei Knoten erstellen. Sie führen dabei die folgenden Schritte durch:

> [!div class="checklist"]
> * Einrichten und Konfigurieren von Linux
> * Installieren und Konfigurieren von SQL Server
> * Konfigurieren der Datei „hosts“
> * Konfigurieren von freigegebenem Speicher und Verschieben der Datenbankdateien
> * Installieren und Konfigurieren von Pacemaker auf jedem Clusterknoten
> * Konfigurieren der Failoverclusterinstanz

In diesem Artikel wird beschrieben, wie Sie für SQL Server eine Failoverclusterinstanz mit zwei Knoten auf einem freigegebenen Datenträger erstellen. Der Artikel enthält Anweisungen und Skriptbeispiele für Red Hat Enterprise Linux (RHEL). Ubuntu-Verteilungen ähneln RHEL, sodass die Skriptbeispiele normalerweise auch unter Ubuntu funktionieren. 

Konzeptionelle Informationen finden Sie unter [SQL Server Failover Cluster Instance (FCI) on Linux (SQL Server-Failoverclusterinstanzen unter Linux)](sql-server-linux-shared-disk-cluster-concepts.md).

## <a name="prerequisites"></a>Voraussetzungen

Für das folgende End-to-End-Szenario benötigen Sie zwei Computer, um den Cluster mit zwei Knoten und einen weiteren Server zum Speichern bereitzustellen. Die folgenden Schritte beschreiben, wie diese Server konfiguriert werden.

## <a name="set-up-and-configure-linux"></a>Einrichten und Konfigurieren von Linux

Der erste Schritt besteht darin, das Betriebssystem auf den Clusterknoten zu konfigurieren. Konfigurieren Sie auf jedem Knoten im Cluster eine Linux-Verteilung. Verwenden Sie auf beiden Knoten die gleiche Verteilung und Version. Verwenden Sie eine der folgenden Verteilungen:
    
* RHEL mit einem gültigen Abonnement für das Add-On für Hochverfügbarkeit

## <a name="install-and-configure-sql-server"></a>Installieren und Konfigurieren von SQL Server

1. Installieren Sie SQL Server auf beiden Knoten, und richten Sie ihn ein.  Ausführliche Anweisungen finden Sie unter [Installieren von SQL Server für Linux](sql-server-linux-setup.md).
1. Legen Sie für die Konfiguration einen Knoten als primär und den anderen als sekundär fest. Verwenden Sie diese Begriffe für den weiteren Verlauf dieses Leitfadens.  
1. Beenden und deaktivieren Sie SQL Server auf dem sekundären Knoten.
    Im folgenden Beispiel wird SQL Server beendet und deaktiviert: 
    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl disable mssql-server
    ```

    > [!NOTE] 
    > Zum Zeitpunkt der Einrichtung wird ein Serverhauptschlüssel für die SQL Server-Instanz generiert und unter `var/opt/mssql/secrets/machine-key` platziert. Unter Linux wird SQL Server immer als lokales Konto mit dem Namen „mssql“ ausgeführt. Da es sich um ein lokales Konto handelt, wird dessen Identität nicht knotenübergreifend freigegeben. Daher müssen Sie den Verschlüsselungsschlüssel vom primären Knoten auf jeden sekundären Knoten kopieren, damit jedes lokale mssql-Konto darauf zugreifen kann, um den Serverhauptschlüssel zu entschlüsseln. 

1.  Erstellen Sie auf dem primären Knoten einen SQL Server-Anmeldenamen für Pacemaker, und erteilen Sie dem Anmeldenamen die Berechtigung zum Ausführen von `sp_server_diagnostics`. Pacemaker verwendet dieses Konto, um zu überprüfen, welcher Knoten auf SQL Server ausgeführt wird. 

    ```bash
    sudo systemctl start mssql-server
    ```
   
   Stellen Sie mithilfe des sa-Kontos eine Verbindung mit der `master`-Datenbank von SQL Server her, und führen Sie Folgendes aus:

   ```sql
   USE [master]
   GO
   CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'
   ALTER SERVER ROLE [sysadmin] ADD MEMBER [<loginName>]
   ```

   Alternativ können Sie die Berechtigungen detaillierter festlegen. Der Pacemaker-Anmeldename benötigt `VIEW SERVER STATE`, um mithilfe von sp_server_diagnostics den Integritätsstatus abzufragen, sowie `setupadmin` und `ALTER ANY LINKED SERVER`, um den Namen der Failoverclusterinstanz mit dem Ressourcennamen zu aktualisieren, indem sp_dropserver und sp_addserver ausgeführt werden. 

1. Beenden und deaktivieren Sie SQL Server auf dem primären Knoten. 

## <a name="configure-the-hosts-file"></a>Konfigurieren der Datei „hosts“

Konfigurieren Sie auf jedem Clusterknoten die Hostdatei. Die Hostdatei muss die IP-Adresse und den Namen jedes Clusterknotens enthalten.

1. Überprüfen Sie die IP-Adresse jedes Knotens. Das folgende Skript zeigt die IP-Adresse des aktuellen Knotens an. 

    ```bash
    sudo ip addr show
    ```

1. Legen Sie den Computernamen auf jedem Knoten fest. Geben Sie jedem Knoten einen eindeutigen Namen, der höchstens 15 Zeichen lang ist. Legen Sie den Computernamen fest, indem Sie diesen zu `/etc/hosts` hinzufügen. Mithilfe des folgenden Skripts können Sie `/etc/hosts` mit `vi` bearbeiten. 

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

## <a name="configure-storage--move-database-files"></a>Konfigurieren des Speichers & Verschieben von Datenbankdateien  

Sie müssen einen Speicher bereitstellen, auf den beide Knoten zugreifen können. Sie können iSCSI, NFS oder SMB verwenden. Konfigurieren Sie den Speicher, stellen Sie ihn für die Clusterknoten bereit, und verschieben Sie die Datenbankdateien dann in den neuen Speicher. In den folgenden Artikeln werden die Schritte für jeden Speichertyp erläutert:

- [Configure failover cluster instance - iSCSI - SQL Server on Linux (Konfigurieren einer Failoverclusterinstanz (iSCSI): SQL Server für Linux)](sql-server-linux-shared-disk-cluster-configure-iscsi.md)
- [Configure failover cluster instance - NFS - SQL Server on Linux (Konfigurieren einer Failoverclusterinstanz (NFS): SQL Server für Linux)](sql-server-linux-shared-disk-cluster-configure-nfs.md)
- [Configure failover cluster instance - SMB - SQL Server on Linux (Konfigurieren einer Failoverclusterinstanz (SMB): SQL Server für Linux)](sql-server-linux-shared-disk-cluster-configure-smb.md)

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installieren und Konfigurieren von Pacemaker auf jedem Clusterknoten

1. Erstellen Sie auf beiden Clusterknoten eine Datei zum Speichern von Benutzername und Kennwort für SQL Server für die Pacemaker-Anmeldung. 

    Der folgende Code erstellt und füllt diese Tabelle:

    ```bash
    sudo touch /var/opt/mssql/secrets/passwd
    sudo echo '<loginName>' >> /var/opt/mssql/secrets/passwd
    sudo echo '<loginPassword>' >> /var/opt/mssql/secrets/passwd
    sudo chown root:root /var/opt/mssql/secrets/passwd 
    sudo chmod 600 /var/opt/mssql/secrets/passwd    
    ```

1. Öffnen Sie auf beiden Clusterknoten die Pacemaker-Firewallports. Führen Sie zum Öffnen dieser Ports mit `firewalld` folgenden Befehl aus:

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
1. Legen Sie das Kennwort für den Standardbenutzer fest, der beim Installieren von Pacemaker und Corosync-Paketen erstellt wird. Verwenden Sie auf beiden Knoten dasselbe Kennwort. 

   ```bash
   sudo passwd hacluster
   ```
1. Aktivieren und starten Sie den `pcsd`-Dienst und Pacemaker. So können Knoten dem Cluster nach dem Neustart erneut beitreten. Führen Sie den folgenden Befehl auf beiden Knoten aus.

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

1. Installieren Sie den FCI-Ressourcenagent für SQL Server. Führen Sie die folgenden Befehle auf beiden Knoten aus. 

   ```bash
   sudo yum install mssql-server-ha
   ```

## <a name="configure-the-failover-cluster-instance"></a>Konfigurieren der Failoverclusterinstanz

Die FCI wird in einer Ressourcengruppe erstellt. Dies ist ein bisschen einfacher, da durch die Ressourcengruppe weniger Einschränkungen nötig sind. Fügen Sie die Ressourcen der Ressourcengruppe jedoch in der Reihenfolge hinzu, in der sie gestartet werden sollen. Die Startreihenfolge sieht wie folgt aus: 

1. Speicherressource
2. Netzwerkressource
3. Anwendungsressource

In diesem Beispiel wird eine FCI in der Gruppe NewLinFCIGrp erstellt. Der Name der Ressourcengruppe muss für jede Ressource eindeutig sein, die in Pacemaker erstellt wurde.

1.  Erstellen Sie die Datenträgerressource. Wenn kein Problem vorliegt, wird keine Antwort zurückgegeben. Die Art und Weise der Erstellung der Datenträgerressource hängt vom Speichertyp ab. Im Folgenden finden Sie ein Beispiel für jeden Speichertyp. Verwenden Sie das Beispiel, das für den Speichertyp für Ihren Clusterspeicher gilt.

    **iSCSI**

    ```bash
    sudo pcs resource create <iSCSIDiskResourceName> Filesystem device="/dev/<VolumeGroupName>/<LogicalVolumeName>" directory="<FolderToMountiSCSIDisk>" fstype="<FileSystemType>" --group RGName
    ```

    \<iSCSIDIskResourceName> ist der Name der Ressource, die dem iSCSI-Datenträger zugeordnet ist.

    \<VolumeGroupName> ist der Name der Volumegruppe.  

    \<LogicalVolumeName> ist der Name des erstellten logischen Volumes.  

    \<FolderToMountiSCSIDIsk> ist der Ordner für die Einbindung des Datenträgers (bei Systemdatenbanken und dem Standardspeicherort wäre das „/var/opt/mssql/data“).

    \<FileSystemType> wäre EXT4 oder XFS, je nachdem, wie formatiert wurde und was die Verteilung unterstützt. 

    **NFS**

    ```bash
    sudo pcs resource create <NFSDiskResourceName> Filesystem device="<IPAddressOfNFSServer>:<FolderOnNFSServer>" directory="<FolderToMountNFSShare>" fstype=nfs4 options=" nfsvers=4.2,timeo=14,intr" --group RGName
    mount -t nfs4 IPAddressOfNFSServer:FolderOnNFSServer /var/opt/mssql/data -o 
    ```

    \<NFSDIskResourceName> ist der Name der Ressource, die der NFS-Freigabe zugeordnet ist.

    \<IPAddressOfNFSServer> ist die IP-Adresse des NFS-Servers, den Sie verwenden werden.

    \<FolderOnNFSServer> ist der Name der NFS-Freigabe.

    \<FolderToMountNFSShare> ist der Ordner für die Einbindung des Datenträgers (bei Systemdatenbanken und dem Standardspeicherort wäre das „/var/opt/mssql/data“).

    Das folgende Beispiel soll dies erläutern:

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci1 /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

    **SMB**

    ```bash
    sudo pcs resource create SMBDiskResourceName Filesystem device="//<ServerName>/<ShareName>" directory="<FolderName>" fstype=cifs options="vers=3.0,username=<UserName>,password=<Password>,domain=<ADDomain>,uid=<mssqlUID>,gid=<mssqlGID>,file_mode=0777,dir_mode=0777" --group <RGName>
    ```

    \<ServerName> ist der Name des Servers mit der SMB-Freigabe.

    \<ShareName> ist der Name der Freigabe.

    \<FolderName> ist der Name des Ordners, der im letzten Schritt erstellt wurde.
    
    \<UserName> ist der Name des Benutzers, der auf die Freigabe zugreifen soll.

    \<Password> ist das Kennwort für den Benutzer.

    \<ADDomain> ist die AD DS-Domäne (falls zutreffend, wenn eine Windows Server-basierte SMB-Freigabe verwendet wird).

    \<mssqlUID> ist die Benutzer-ID des mssql-Benutzers.

    \<mssqlGID> ist die Gruppen-ID des mssql-Benutzers.

    \<RGName> ist der Name der Ressourcengruppe.
 
2.  Erstellen Sie die IP-Adresse, die von der FCI verwendet wird. Wenn kein Problem vorliegt, wird keine Antwort zurückgegeben.

    ```bash
    sudo pcs resource create <IPResourceName> ocf:heartbeat:IPaddr2 ip=<IPAddress> nic=<NetworkCard> cidr_netmask=<NetMask> --group <RGName>
    ```

    \<IPResourceName> ist der Name der Ressource, die der IP-Adresse zugeordnet ist.

    \<IPAddress> ist die IP-Adresse für die FCI.

    \<NetworkCard> ist die Netzwerkkarte, die dem Subnetz zugeordnet ist (d. h. eth0).

    \<NetMask> ist die Netzmaske des Subnetzes (d. h. 24).

    \<RGName> ist der Name der Ressourcengruppe.
 
3.  Erstellen Sie die FCI-Ressource. Wenn kein Problem vorliegt, wird keine Antwort zurückgegeben.

    ```bash
    sudo pcs resource create FCIResourceName ocf:mssql:fci op defaults timeout=60s --group RGName
    ```

    \<FCIResourceName> ist nicht nur der Name der Ressource, sondern auch der Anzeigename, der der FCI zugeordnet ist. Damit stellen Benutzer und Anwendungen eine Verbindung her. 

    \<RGName> ist der Name der Ressourcengruppe.
 
4.  Führen Sie den Befehl `sudo pcs resource` aus. Die FCI muss online sein.
 
5.  Stellen Sie mithilfe des DNS-/Ressourcennamens der FCI eine Verbindung mit SSMS oder sqlcmd her.

6.  Geben Sie die Anweisung `SELECT @@SERVERNAME` aus. Sie sollte den Namen der FCI zurückgeben.

7.  Geben Sie die Anweisung `SELECT SERVERPROPERTY('ComputerNamePhysicalNetBIOS')` aus. Sie sollte den Namen des Knotens zurückgeben, auf dem die FCI ausgeführt wird.

8.  Führen Sie ein manuelles Failover der FCI auf den oder die anderen Knoten durch. Weitere Informationen finden Sie in den Anweisungen unter [Operate failover cluster instance - SQL Server on Linux (Betreiben einer Failoverclusterinstanz: SQL Server für Linux)](sql-server-linux-shared-disk-cluster-operate.md).

9.  Führen Sie schließlich ein Failback auf den ursprünglichen Knoten durch, und entfernen Sie die Verbindungseinschränkung.

<!---

|Distribution |Topic 
|----- |-----
|**Red Hat Enterprise Linux with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)<br/>[Operate](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
|**SUSE Linux Enterprise Server with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-sles-configure.md)

-->
## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie die folgenden Aufgaben abgeschlossen.

> [!div class="checklist"]
> * Einrichten und Konfigurieren von Linux
> * Installieren und Konfigurieren von SQL Server
> * Konfigurieren der Datei „hosts“
> * Konfigurieren von freigegebenem Speicher und Verschieben der Datenbankdateien
> * Installieren und Konfigurieren von Pacemaker auf jedem Clusterknoten
> * Konfigurieren der Failoverclusterinstanz

## <a name="next-steps"></a>Nächste Schritte

- [Operate failover cluster instance - SQL Server on Linux (Betreiben einer Failoverclusterinstanz: SQL Server für Linux)](sql-server-linux-shared-disk-cluster-operate.md)

<!--Image references-->
