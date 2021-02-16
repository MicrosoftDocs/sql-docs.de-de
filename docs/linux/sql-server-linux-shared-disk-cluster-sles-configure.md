---
title: Konfigurieren eines freigegebenen SLES-Datenträgerclusters für SQL Server
description: Implementieren Sie Hochverfügbarkeit, indem Sie einen freigegebenen Datenträgercluster mit SUSE Linux Enterprise Server (SLES) für SQL Server konfigurieren.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: e5ad1bdd-c054-4999-a5aa-00e74770b481
ms.openlocfilehash: 87a4fd9edf52c3590fb19781fc5bf76c9bb0168b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346265"
---
# <a name="configure-sles-shared-disk-cluster-for-sql-server"></a>Konfigurieren eines freigegebenen SLES-Datenträgerclusters für SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Diese Anleitung enthält Anweisungen zum Erstellen eines freigegebenen Datenträgerclusters mit zwei Knoten für SQL Server unter SUSE Linux Enterprise Server (SLES). Die Clusteringebene basiert auf der SUSE [High Availability Extension (HAE)](https://www.suse.com/products/highavailability), die auf [Pacemaker](https://clusterlabs.org/) aufbaut. 

Weitere Informationen zu Clusterkonfiguration, Ressourcen-Agent-Optionen, Verwaltung, Best Practices und Empfehlungen finden Sie unter [SUSE Linux Enterprise High Availability Extension 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html).

## <a name="prerequisites"></a>Voraussetzungen

Für das folgende End-to-End-Szenario benötigen Sie zwei Computer, um den Cluster mit zwei Knoten und einen weiteren Server zum Konfigurieren der NFS-Freigabe bereitzustellen. Die folgenden Schritte beschreiben, wie diese Server konfiguriert werden.

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>Einrichten und Konfigurieren des Betriebssystems auf den einzelnen Clusterknoten

Der erste Schritt besteht darin, das Betriebssystem auf den Clusterknoten zu konfigurieren. Verwenden Sie für diese exemplarische Vorgehensweise SLES mit einem gültigen Abonnement für das Hochverfügbarkeits-Add-On.

## <a name="install-and-configure-sql-server-on-each-cluster-node"></a>Installieren und Konfigurieren der SQL Server-Instanz auf den einzelnen Clusterknoten

1. Installieren Sie SQL Server auf beiden Knoten, und richten Sie die Anwendung ein. Ausführliche Anweisungen finden Sie unter [Installieren von SQL Server für Linux](sql-server-linux-setup.md).
2. Legen Sie für die Konfiguration einen Knoten als primär und den anderen als sekundär fest. Verwenden Sie diese Begriffe für den weiteren Verlauf dieses Leitfadens. 
3. Beenden und deaktivieren Sie SQL Server auf dem sekundären Knoten. Im folgenden Beispiel wird SQL Server beendet und deaktiviert:

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl disable mssql-server
    ```

    > [!NOTE]
    > Zum Zeitpunkt der Einrichtung wird ein Serverhauptschlüssel für die SQL Server-Instanz generiert und unter `/var/opt/mssql/secrets/machine-key` platziert. Unter Linux wird SQL Server immer als lokales Konto mit dem Namen „mssql“ ausgeführt. Da es sich um ein lokales Konto handelt, wird dessen Identität nicht knotenübergreifend freigegeben. Daher müssen Sie den Verschlüsselungsschlüssel vom primären Knoten auf jeden sekundären Knoten kopieren, damit jedes lokale mssql-Konto darauf zugreifen kann, um den Serverhauptschlüssel zu entschlüsseln.
4. Erstellen Sie auf dem primären Knoten einen SQL Server-Anmeldenamen für Pacemaker, und erteilen Sie dem Anmeldenamen die Berechtigung zum Ausführen von `sp_server_diagnostics`. Pacemaker verwendet dieses Konto, um zu überprüfen, welcher Knoten auf SQL Server ausgeführt wird.

    ```bash
    sudo systemctl start mssql-server
    ```
    Stellen Sie mithilfe des sa-Kontos eine Verbindung mit der Masterdatenbank von SQL Server her, und führen Sie Folgendes aus:

    ```sql
    USE [master]
    GO
    CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'
    GRANT VIEW SERVER STATE TO <loginName>
    ```
5. Beenden und deaktivieren Sie SQL Server auf dem primären Knoten.
6. Befolgen Sie die Anweisungen [in der SUSE-Dokumentation](https://www.suse.com/documentation/sles11/book_sle_admin/data/sec_basicnet_yast.html), um die hosts-Datei für die einzelnen Clusterknoten zu konfigurieren und zu aktualisieren. Die hosts-Datei muss die IP-Adresse und den Namen jedes Clusterknotens enthalten.

    So überprüfen Sie die IP-Adresse des aktuellen Knotens:

    ```bash
    sudo ip addr show
    ```

    Legen Sie den Computernamen auf jedem Knoten fest. Geben Sie jedem Knoten einen eindeutigen Namen, der höchstens 15 Zeichen lang ist. Legen Sie den Computernamen fest, indem Sie ihn `/etc/hostname` mithilfe von [yast](https://www.suse.com/documentation/sles11/book_sle_admin/data/sec_basicnet_yast.html) oder [manuell](https://www.suse.com/documentation/sled11/book_sle_admin/data/sec_basicnet_manconf.html) hinzufügen.

    Das folgende Beispiel zeigt `/etc/hosts` mit Ergänzungen für zwei Knoten mit den Namen `SLES1` und `SLES2`.

    ```
    127.0.0.1   localhost
    10.128.18.128 SLES1
    10.128.16.77 SLES2
    ```

    > [!NOTE]
    > Alle Clusterknoten müssen in der Lage sein, über SSH aufeinander zuzugreifen. Tools wie „hb_report“ oder „crm_report“ (zur Problembehandlung) und Hawk's History Explorer erfordern kennwortlosen SSH-Zugriff zwischen den Knoten, andernfalls können sie nur Daten vom aktuellen Knoten sammeln. Falls Sie keinen nicht-standardmäßigen SSH-Port verwenden, nutzen Sie die Option „-X“ ([siehe Hauptseite](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_requirements_other.html)). Wenn Ihr SSH-Port z.B. 3479 ist, rufen Sie crm_report mit Folgendem auf:
    >
    >```bash
    >crm_report -X "-p 3479" [...]
    >```
    >Weitere Informationen finden Sie im [Administratorhandbuch].(https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.misc)

Im nächsten Abschnitt konfigurieren Sie den freigegebenen Speicher und verschieben Ihre Datenbankdateien in diesen Speicher.  

## <a name="configure-shared-storage-and-move-database-files"></a>Konfigurieren von freigegebenem Speicher und Verschieben von Datenbankdateien

Es gibt eine Reihe von Lösungen für die Bereitstellung von freigegebenem Speicher. Diese exemplarische Vorgehensweise veranschaulicht das Konfigurieren von freigegebenem Speicher mit NFS. Sie sollten auf die bewährten Methoden zurückzugreifen und Kerberos zum Sichern von NFS verwenden: 

- [Sharing File Systems with NFS](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#cha.nfs) (Freigeben von Dateisystemen mit NFS)

Wenn Sie diese Anleitung nicht befolgen, kann jeder Benutzer, der auf Ihr Netzwerk zugreifen und die IP-Adresse eines SQL-Knotens spoofen kann, auf Ihre Datendateien zugreifen. Stellen Sie wie immer sicher, dass Sie ein Gefahrenmodell für Ihr System erstellen, bevor Sie es in der Produktion verwenden. 

Eine andere Speicheroption ist die Verwendung der SMB-Dateifreigabe:

- [Samba-Abschnitt der SUSE-Dokumentation](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#cha.samba)

### <a name="configure-an-nfs-server"></a>Konfigurieren eines NFS-Servers

Informationen zum Konfigurieren eines NFS-Servers finden Sie in den folgenden Schritten in der SUSE-Dokumentation: [Configuring NFS Server](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#sec.nfs.configuring-nfs-server) (Konfigurieren des NFS-Servers).

### <a name="configure-all-cluster-nodes-to-connect-to-the-nfs-shared-storage"></a>Konfigurieren aller Clusterknoten für die Verbindung mit dem freigegebenen NFS-Speicher

Bevor Sie das Client-NFS zum Einbinden des SQL Server-Datenbankdateien-Pfads zum Verweisen auf den freigegebenen Speicherort konfigurieren, stellen Sie sicher, dass Sie die Datenbankdateien an einem temporären Speicherort speichern, um Sie später auf die Freigabe kopieren zu können:

1. Speichern Sie die Datenbankdateien **nur auf dem primären Knoten** an einem temporären Speicherort. Das folgende Skript erstellt ein neues temporäres Verzeichnis, kopiert die Datenbankdateien in das neue Verzeichnis und entfernt die alten Datenbankdateien. Da SQL Server als lokaler Benutzer „mssql“ ausgeführt wird, müssen Sie sicherstellen, dass der lokale Benutzer nach der Datenübertragung zur eingebundenen Freigabe Lese- und Schreibzugriff auf die Freigabe hat. 

    ```bash
    su mssql
    mkdir /var/opt/mssql/tmp
    cp /var/opt/mssql/data/* /var/opt/mssql/tmp
    rm /var/opt/mssql/data/*
    exit
    ```

    Konfigurieren Sie den NFS-Client auf allen Clusterknoten:

    - [Configuring Clients](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#sec.nfs.configuring-nfs-clients) (Konfigurieren von Clients)

    > [!NOTE]
    > Sie sollten die bewährten Methoden und Empfehlungen von SUSE in Bezug auf den hoch verfügbaren NFS-Speicher befolgen: [Highly Available NFS Storage with DRBD and Pacemaker](https://www.suse.com/documentation/sle-ha-12/book_sleha_techguides/data/art_ha_quick_nfs.html) (Hoch verfügbarer NFS-Speicher mit DRBD und Pacemaker).

2. Überprüfen Sie, ob SQL Server mit dem neuen Dateipfad erfolgreich gestartet wurde. Führen Sie dies auf jedem Knoten durch. An diesem Punkt sollte SQL Server immer nur auf einem Knoten ausgeführt werden. Sie können SQL Server nicht auf beiden Knoten gleichzeitig ausführen, da diese ansonsten versuchen würden, gleichzeitig auf die Datendateien zuzugreifen (verwenden Sie zur Vermeidung eines versehentlichen Starts von SQL Server auf beiden Knoten eine File System-Clusterressource, um sicherzustellen, dass die Freigabe nicht zweimal von verschiedenen Knoten eingebunden wird). Die folgenden Befehle starten SQL Server, überprüfen den Status und beenden SQL Server dann.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    sudo systemctl stop mssql-server
    ```

An diesem Punkt sind beide Instanzen von SQL Server so konfiguriert, dass sie mit den Datenbankdateien im freigegebenen Speicher ausgeführt werden. Der nächste Schritt besteht darin, SQL Server für Pacemaker zu konfigurieren. 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installieren und Konfigurieren von Pacemaker auf jedem Clusterknoten

1. **Erstellen Sie auf beiden Clusterknoten eine Datei zum Speichern von Benutzername und Kennwort für SQL Server für die Pacemaker-Anmeldung**. Der folgende Code erstellt und füllt diese Tabelle:

    ```bash
    sudo touch /var/opt/mssql/secrets/passwd
    sudo echo '<loginName>' >> /var/opt/mssql/secrets/passwd
    sudo echo '<loginPassword>' >> /var/opt/mssql/secrets/passwd
    sudo chown root:root /var/opt/mssql/secrets/passwd 
    sudo chmod 600 /var/opt/mssql/secrets/passwd
    ```
2. **Alle Clusterknoten müssen in der Lage sein, über SSH aufeinander zugreifen**. Tools wie „hb_report“ oder „crm_report“ (zur Problembehandlung) und Hawk's History Explorer erfordern kennwortlosen SSH-Zugriff zwischen den Knoten, andernfalls können sie nur Daten vom aktuellen Knoten sammeln. Falls Sie keinen nicht-standardmäßigen SSH-Port verwenden, nutzen Sie die Option „-X“ (siehe Hauptseite). Wenn Ihr SSH-Port z.B. 3479 ist, rufen Sie hb_report mit Folgendem auf:

    ```bash
    crm_report -X "-p 3479" [...]
    ```

    Weitere Informationen finden Sie unter [System Requirements and Recommendations in the SUSE documentation (Systemanforderungen und Empfehlungen in der SUSE-Dokumentation)](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_requirements_other.html).

3. **Installieren Sie die Erweiterung für hohe Verfügbarkeit**. Um die Erweiterung zu installieren, befolgen Sie die Schritte im folgenden SUSE-Thema:
    
    [Installation and Setup Quick Start (Installation und Setup – Schnellstart)](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html)

4. **Installieren Sie den FCI-Ressourcenagent für SQL Server**. Führen Sie die folgenden Befehle auf beiden Knoten aus:

    ```bash
    sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo
    sudo zypper --gpg-auto-import-keys refresh
    sudo zypper install mssql-server-ha
    ```

5. **Richten Sie den ersten Knoten automatisch ein**. Im nächsten Schritt wird ein ausgeführter Cluster mit einem Noten durch Konfigurieren des ersten Knotens, SLES1, eingerichtet. Befolgen Sie den Anweisungen im SUSE-Thema [Setting Up the First Node (Einrichten des ersten Knotens)](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.1st-node).

    Abschließend überprüfen Sie den Status des Clusters mit `crm status`:
    ```bash
    crm status
    ```

    Es sollte angezeigt werden, dass ein Knoten, SLES1, konfiguriert wurde.

6. **Hinzufügen von Knoten zu einem vorhandenen Cluster**. Als Nächstes fügen Sie den SLES2-Knoten dem Cluster hinzu. Befolgen Sie den Anweisungen im SUSE-Thema [Adding the Second Node (Hinzufügen des zweiten Knotens)](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.2nd-node).
    
    Abschließend überprüfen Sie den Status des Clusters mit **crm status**. Wenn Sie einen zweiten Knoten erfolgreich hinzugefügt haben, sieht die Ausgabe ähnlich der folgenden aus:
        
    ```
    2 nodes configured
    1 resource configured
    Online: [ SLES1 SLES2 ]
    Full list of resources:
    admin_addr     (ocf::heartbeat:IPaddr2):       Started SLES1
    ```

    > [!NOTE]
    > **admin_addr** ist die virtuelle IP-Clusterressource, die während der anfänglichen Einrichtung des Clusters mit einem Knoten konfiguriert wird.

7.  **Vorgehensweisen zum Entfernen**. Wenn Sie einen Knoten aus dem Cluster entfernen möchten, verwenden Sie das Bootstrap-Skript **ha-cluster-remove**. Weitere Informationen finden Sie unter [Overview of the Bootstrap Scripts (Übersicht über die Bootstrap-Skripts)](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.bootstrap).  

## <a name="configure-the-cluster-resources-for-sql-server"></a>Konfigurieren der Clusterressourcen für SQL Server

In den folgenden Schritten wird erläutert, wie Sie die Clusterressource für SQL Server konfigurieren. Es gibt zwei Einstellungen, die Sie anpassen müssen.

- **SQL Server-Ressourcenname**: Ein Name für die gruppierte SQL Server-Ressource 
- **Timeoutwert**: Der Timeoutwert ist die Zeitspanne, die der Cluster wartet, während eine Ressource online geschaltet wird. In SQL Server ist dies die Zeit, die SQL Server Ihrer Erwartung zufolge benötigt, um die `master`-Datenbank online zu schalten. 

Aktualisieren Sie die Werte aus dem folgenden Skript für Ihre Umgebung. Führen Sie es auf einem Knoten aus, um den gruppierten Dienst zu konfigurieren und zu starten.

```bash
sudo crm configure
primitive <sqlServerResourceName> ocf:mssql:fci op start timeout=<timeout_in_seconds>
colocation <constraintName> inf: <virtualIPResourceName> <sqlServerResourceName>
show
commit
exit
```

Das folgende Skript erstellt beispielsweise eine gruppierte SQL Server-Ressource mit dem Namen „mssqlha“. 

```bash
sudo crm configure
primitive mssqlha ocf:mssql:fci op start timeout=60s
colocation admin_addr_mssqlha inf: admin_addr mssqlha
show
commit
exit
```

Nachdem ein Commit für die Konfiguration ausgeführt wurde, wird SQL Server auf demselben Knoten gestartet wie die virtuelle IP-Ressource. 

Weitere Informationen finden Sie unter [Configuring and Managing Cluster Resources (Command Line)](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.manual_config) (Konfigurieren und Verwalten von Clusterressourcen (Befehlszeile)). 

### <a name="verify-that-sql-server-is-started"></a>Stellen Sie sicher, dass SQL Server gestartet wird. 

Führen Sie den Befehl **crm status** aus, um zu überprüfen, ob SQL Server gestartet wurde:

```bash
crm status
```

Die folgenden Beispiele zeigen die Ergebnisse, wenn Pacemaker erfolgreich als gruppierte Ressource gestartet wurde. 
```
2 nodes configured
2 resources configured

Online: [ SLES1 SLES2 ]

Full list of resources:

 admin_addr     (ocf::heartbeat:IPaddr2):       Started SLES1
 mssqlha        (ocf::mssql:fci):       Started SLES1
```

## <a name="managing-cluster-resources"></a>Verwalten von Clusterressourcen

Informationen zum Verwalten von Clusterressourcen finden Sie im folgenden SUSE-Thema: [Managing Cluster Resources](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.config.crm ) (Verwalten von Clusterressourcen)

### <a name="manual-failover"></a>Manuelles Failover

Obwohl Ressourcen so konfiguriert sind, dass bei einem Hardware- oder Softwarefehler automatisch ein Failover (oder eine Migration) zu anderen Knoten des Clusters ausgeführt wird, können Sie eine Ressource auch über die Pacemaker-GUI oder die Befehlszeile manuell auf einen anderen Knoten im Cluster verschieben. 

Verwenden Sie für diese Aufgabe den Migrationsbefehl. Um z.B. die SQL-Ressource zu einem Clusterknoten mit dem Namen SLES2 zu migrieren, führen Sie Folgendes aus: 

```bash
crm resource
migrate mssqlha SLES2
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[SUSE Linux Enterprise High Availability Extension – Administration Guide](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html) (SUSE Linux Enterprise High Availability Extension – Administratorhandbuch). 
