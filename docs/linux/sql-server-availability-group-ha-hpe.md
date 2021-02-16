---
title: Bereitstellen einer Verfügbarkeitsgruppe mit HPE Serviceguard (SQL Server für Linux)
description: Verwenden Sie HPE Serviceguard als Cluster-Manager, um Hochverfügbarkeit mit einer Verfügbarkeitsgruppe auf SQL Server für Linux zu erzielen.
ms.date: 01/15/2021
ms.prod: sql
ms.technology: linux
ms.topic: tutorial
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.openlocfilehash: 528d9550f79eea50db554dec11ead53039348cdb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051931"
---
# <a name="tutorial---setup-a-three-node-always-on-availability-group-with-hpe-serviceguard-for-linux"></a>Tutorial: Einrichten einer Always On-Verfügbarkeitsgruppe mit drei Knoten mit HPE Serviceguard für Linux 

In diesem Tutorial wird erläutert, wie Sie eine Always On-Verfügbarkeitsgruppe von SQL Server mit HPE Serviceguard für Linux konfigurieren, die auf lokalen VMs ausgeführt wird.

Eine Übersicht über die HPE Serviceguard-Cluster finden Sie unter [HPE Serviceguard-Cluster](https://h20195.www2.hpe.com/v2/GetPDF.aspx/c04154488.pdf).

> [!NOTE]
> Microsoft unterstützt die Datenverschiebung, die Verfügbarkeitsgruppe und die SQL Server-Komponente. Wenden Sie sich an HPE, um Unterstützung im Zusammenhang mit der Dokumentation der HPE Serviceguard-Clusterverwaltung und -Quorumverwaltung zu erhalten

Dieses Tutorial umfasst die folgenden Aufgaben:

> [!div class="checklist"]
> * SQL Server auf allen drei VMs installieren, die Teil der Verfügbarkeitsgruppe sind
> * HPE Serviceguard auf den VMs installieren
> * HPE-Serviceguard-Cluster erstellen
> * Verfügbarkeitsgruppe erstellen und eine Beispieldatenbank zur Verfügbarkeitsgruppe hinzufügen
> * Die SQL Server-Arbeitsauslastung in der Verfügbarkeitsgruppe über den Serviceguard-Cluster-Manager bereitstellen
> * Ein automatisches Failover ausführen und den Knoten mit dem Cluster verknüpfen

## <a name="prerequisites"></a>Voraussetzungen

* Drei VMs in einer lokalen Umgebung, die HPE Serviceguard auf einer unterstützten Linux-Distribution ausführen.

   Ein Beispiel für eine unterstützte Distribution finden Sie unter [HPE Serviceguard für Linux](https://h20195.www2.hpe.com/v2/gethtml.aspx?docname=c04154488).

   Weitere Informationen zur Unterstützung für öffentliche Cloud-Umgebungen erhalten Sie von HPE.

   Die Anweisungen in diesem Tutorial werden auf der Seite „HPE Serviceguard für Linux“ überprüft. Eine Testversion kann über [HPE](https://www.hpe.com/us/en/resources/servers/serviceguard-linux-trial.html) heruntergeladen werden.

* SQL Server-Datenbankdateien zum Einbinden des Logical Volume (LVM) für alle VMs. Weitere Informationen finden Sie unter [Quick start guide for Serviceguard Linux (HPE) (Schnellstartleitfaden für Serviceguard für Linux (HPE))](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us).

* Stellen Sie sicher, dass auf den VMs eine OpenJDK-Java Runtime installiert ist.

   > [!NOTE]
   > Das IBM Java SDK wird nicht unterstützt.

## <a name="install-sql-server"></a>Installieren von SQL Server

Führen Sie auf allen drei VMs einen der unten aufgeführten Schritte basierend auf der Linux-Distribution aus, die Sie für dieses Tutorial ausgewählt haben, um SQL Server und Tools zu installieren.

### <a name="rhel"></a>RHEL

* [Installieren von SQL Server unter Red Hat](quickstart-install-connect-red-hat.md#install)
* [Extras](quickstart-install-connect-red-hat.md#tools)

### <a name="sles"></a>SLES

* [Installieren von SQL Server unter SLES](quickstart-install-connect-suse.md#install)
* [Extras](quickstart-install-connect-suse.md#tools)

Nachdem Sie diesen Schritt ausgeführt haben, sollten Sie den SQL Server-Dienst und die Tools auf allen drei VMs installiert haben, die an der Verfügbarkeitsgruppe teilnehmen sollen.

## <a name="install-hpe-serviceguard-on-the-vms"></a>Installieren von HPE Serviceguard auf den VMs

In diesem Schritt installieren Sie HPE Serviceguard für Linux auf allen drei VMs. In der folgenden Tabelle werden die Rollen beschrieben, die jeder Server im Cluster hat.

|Number of VMs (Anzahl von VMs) | HPE Servicguard-Rolle | Replikatrolle der Microsoft SQL Server-Verfügbarkeitsgruppe|
|--------------|-----------------|------------|
|1 | HPE Serviceguard-Clusterknoten | Primäres Replikat |
|Mindestens 1 | HPE Serviceguard-Clusterknoten | Sekundäres Replikat |
|1 | HPE Serviceguard-Quorumserver | Konfigurationsreplikat |

Verwenden Sie die `cminstaller`-Methode, um Serviceguard zu installieren. Bestimmte Anweisungen finden Sie unter den folgenden Links:

Serviceguard-Cluster und Serviceguard-Quorumserver

* [Install Serviceguard for Linux on two nodes (Installieren von HPE Serviceguard für Linux auf zwei Knoten)](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Install_serviceguard_using_cminstaller). Weitere Informationen finden Sie im Abschnitt **Install_serviceguard_using_cminstaller**.
* [Install Serviceguard quorum server on the third node (Installieren des HPE Serviceguard-Quorumservers auf dem dritten Knoten)](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Install_QS_from_the_ISO). Weitere Informationen finden Sie im Abschnitt **Install_QS_from_the_ISO**.

Nach Abschluss der Installation des HPE Serviceguard-Clusters können Sie das Clusterverwaltungsportal auf dem TCP-Port 5522 auf dem primären Replikatknoten aktivieren. Mit den folgenden Schritten wird der Firewall eine Regel hinzugefügt, um den TCP-Port 5522 zuzulassen. Der folgende Befehl ist für eine RHEL-Distribution vorgesehen. Sie müssen ähnliche Befehle für andere Distributionen ausführen:

```console
sudo firewall-cmd --zone=public --add-port=5522/tcp --permanent

sudo firewall-cmd --reload 
```

## <a name="create-hpe-serviceguard-cluster"></a>Erstellen eines HPE Serviceguard-Clusters

Befolgen Sie die nachstehenden Anweisungen, um den HPE Serviceguard-Cluster zu konfigurieren und zu erstellen. In diesem Schritt wird auch der Quorumserver konfiguriert.

1. [Configure the Serviceguard quorum server on the third node (Konfigurieren des Serviceguard-Quorumservers auf dem dritten Knoten)](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Configure_QS). Weitere Informationen finden Sie im Abschnitt **Configure_QS**.
2. [Configure and create Serviceguard cluster on the other two nodes (Konfigurieren und Erstellen des Serviceguard-Clusters auf den beiden anderen Knoten)](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Configure_and_create_cluster). Weitere Informationen finden Sie im Abschnitt **Configure_and_create_Cluster**.

## <a name="create-the-availability-group-and-add-a-sample-database"></a>Erstellen der Verfügbarkeitsgruppe und Hinzufügen einer Beispieldatenbank

Erstellen Sie in diesem Schritt eine Verfügbarkeitsgruppe mit mindestens zwei synchronen Replikaten und einem Konfigurationsreplikat. Dadurch kann der Schutz von Daten gewährleistet und ggf. auch Hochverfügbarkeit bereitgestellt werden. Das folgende Diagramm veranschaulicht diese Architektur:

:::image type="content" source="media/sql-server-linux-availability-group-ha/2-configuration-only.png" alt-text="Das primäre Replikat synchronisiert Benutzerdaten und Konfigurationsdaten mit dem sekundären Replikat. Das primäre Replikat synchronisiert nur Konfigurationsdaten mit dem Konfigurationsreplikat. Das Konfigurationsreplikat weist keine Benutzerdatenreplikate auf.":::

1. Eine synchrone Replikation von Benutzerdaten wird auf einem sekundären Replikat ausgeführt. Dieser Vorgang schließt auch Metadaten zur Konfiguration der Verfügbarkeitsgruppe ein.

2. Eine synchrone Replikation von Metadaten zur Konfiguration der Verfügbarkeitsgruppe wird ausgeführt. Dieser Vorgang schließt keine Benutzerdaten ein.

Weitere Informationen finden Sie unter [Zwei synchrone Replikate und ein Konfigurationsreplikat](sql-server-linux-availability-group-ha.md).

Führen Sie die folgenden Schritte aus, um die Verfügbarkeitsgruppe zu erstellen:

1. [Aktivieren von Always On-Verfügbarkeitsgruppen und Neu starten von mssql-server](#enable-always-on-availability-groups-and-restart-mssql-server) auf allen VMs, einschließlich des Konfigurationsreplikats
2. [Aktivieren einer `AlwaysOn_health`-Ereignissitzung (optional)](#enable-an-alwayson_health-event-session---optional)
3. [Erstellen eines Zertifikats auf der primären VM](#create-a-certificate-on-the-primary-vm)
4. [Erstellen des Zertifikats auf sekundären Servern](#create-the-certificate-on-secondary-servers)
5. [Erstellen der Datenbankspiegelungsendpunkte auf allen Replikaten](#create-the-database-mirroring-endpoints-on-the-replicas)
6. [Erstellen einer Verfügbarkeitsgruppe](#create-availability-group)
7. [Verknüpfen der sekundären Replikate](#join-the-secondary-replicas)
8. [Hinzufügen einer Datenbank zur oben erstellten Verfügbarkeitsgruppe](#add-a-database-to-the-availability-group-created-above)

### <a name="enable-always-on-availability-groups-and-restart-mssql-server"></a>Aktivieren von Always On-Verfügbarkeitsgruppen und Neustarten von mssql-server

Aktivieren Sie das Always On-Feature auf allen Knoten, die eine SQL Server-Instanz hosten. Starten Sie dann „mssql-server“ neu. Führen Sie das folgende Skript auf allen drei Knoten aus:

```console
sudo /opt/mssql/bin/mssql-conf
set hadr.hadrenabled 1 sudo systemctl restart mssql-serve
```

### <a name="enable-an-alwayson_health-event-session---optional"></a>Aktivieren einer `AlwaysOn_health`-Ereignissitzung (optional)

Optional können Sie erweiterte Ereignisse der Always On-Verfügbarkeitsgruppen aktivieren, die Ihnen bei der Ursachendiagnose helfen, wenn Sie Probleme in einer Verfügbarkeitsgruppe beheben. Führen Sie auf jeder SQL Server-Instanz den folgenden Befehl aus:

```tsql
ALTER EVENT SESSION AlwaysOn_health ON SERVER WITH (STARTUP_STATE=ON);
GO
```

### <a name="create-a-certificate-on-the-primary-vm"></a>Erstellen eines Zertifikats auf der primären VM

Das folgende Transact-SQL-Skript erstellt einen Hauptschlüssel und ein Zertifikat. Anschließend werden das Zertifikat und die Datei mit einem privaten Schlüssel gesichert. Aktualisieren Sie das Skript durch sichere Kennwörter. Stellen Sie eine Verbindung mit der primären SQL Server-Instanz her, und führen Sie das folgende Transact-SQL-Skript aus:

```tsql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Master_Key_Password';

CREATE CERTIFICATE dbm_certificate WITH SUBJECT = 'dbm';

BACKUP CERTIFICATE dbm_certificate TO FILE = '/var/opt/mssql/data/dbm_certificate.cer'
WITH PRIVATE KEY 
    ( FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
      ENCRYPTION BY PASSWORD = '<Private_Key_Password>' );

```

Zu diesem Zeitpunkt weist das primäre SQL Server-Replikat ein Zertifikat unter `/var/opt/mssql/data/dbm_certificate.cer` und einen privaten Schlüssel unter `var/opt/mssql/data/dbm_certificate.pvk` auf. Kopieren Sie diese beiden Dateien auf allen Servern, die Verfügbarkeitsreplikate hosten werden, an den gleichen Speicherort. Verwenden Sie den mssql-Benutzer, oder erteilen Sie dem mssql-Benutzer Berechtigungen, um auf diese Dateien zuzugreifen.

Der folgende Befehl kopiert z.B. auf dem Quellserver die Dateien auf den Zielcomputer. Ersetzen Sie die `'node2'`-Werte durch den Namen des Hosts, auf dem die sekundäre SQL Server-Instanz ausgeführt wird. Kopieren Sie das Zertifikat auch für das Konfigurationsreplikat, und führen Sie die unten aufgeführten Befehle auch auf diesem Knoten aus.

```console
cd /var/opt/mssql/data
scp dbm_certificate.* root@<node2>:/var/opt/mssql/data/
```

Führen Sie jetzt auf den sekundären VMs, auf denen die sekundäre Instanz und das Konfigurationsreplikat von SQL Server ausgeführt wird, die folgenden Befehle aus, sodass der mssql-Benutzer das kopierte Zertifikat besitzen kann:

```console
cd /var/opt/mssql/data

chown mssql:mssql dbm_certificate.*
```

### <a name="create-the-certificate-on-secondary-servers"></a>Erstellen des Zertifikats auf sekundären Servern

Das folgende Transact-SQL-Skript erstellt einen Hauptschlüssel und ein Zertifikat aus der Sicherung, die Sie auf dem primären SQL Server-Replikat erstellt haben. Aktualisieren Sie das Skript durch sichere Kennwörter. Das Entschlüsselungskennwort ist das gleiche Kennwort, mit dem Sie die PVK-Datei in einem vorherigen Schritt erstellt haben. Führen Sie das folgende Skript auf allen sekundären Servern mit Ausnahme des Konfigurationsreplikats aus, um das Zertifikat zu erstellen:

```tsql
CREATE MASTER KEY ENCRYPTION BY PASSWORD =
'<Master_Key_Password>';

CREATE CERTIFICATE dbm_certificate FROM FILE =
'/var/opt/mssql/data/dbm_certificate.cer'
WITH PRIVATE KEY ( FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
DECRYPTION BY PASSWORD = '<Private_Key_Password>' );
```

### <a name="create-the-database-mirroring-endpoints-on-the-replicas"></a>Erstellen der Datenbankspiegelungsendpunkte auf allen Replikaten

Führen Sie auf dem primären und dem sekundären Replikat die folgenden Befehle zum Erstellen der Datenbankspiegelungsendpunkte aus:

```tsql
CREATE ENDPOINT [hadr_endpoint] AS TCP (LISTENER_PORT = 5022)
    FOR DATABASE_MIRRORING 
        (
        ROLE = WITNESS,
        AUTHENTICATION = CERTIFICATE dbm_certificate,
        ENCRYPTION = REQUIRED ALGORITHM AES
        );

ALTER ENDPOINT [hadr_endpoint] STATE = STARTED;
```

> [!NOTE]
> 5022 ist der Standardport, der für den Datenbankspiegelungsendpunkt verwendet wird. Sie können ihn in jeden verfügbaren Port ändern.

Erstellen Sie auf dem Konfigurationsreplikat den Datenbankspiegelungsendpunkt mit dem unten angegebenen Befehl. Beachten Sie, dass der Wert für die Rolle hier auf `WITNESS` festgelegt ist, so wie für das Konfigurationsreplikat erforderlich.

```tsql
CREATE ENDPOINT [hadr_endpoint] AS TCP (LISTENER_PORT = 5022)
    FOR DATABASE_MIRRORING (
        ROLE = WITNESS,
        AUTHENTICATION = CERTIFICATE dbm_certificate,
        ENCRYPTION = REQUIRED ALGORITHM AES
        );

ALTER ENDPOINT [hadr_endpoint] STATE = STARTED;
```

### <a name="create-availability-group"></a>Erstellen der Verfügbarkeitsgruppe

Führen Sie die folgenden Befehle auf der primären Replikatinstanz aus. Diese Befehle erstellen eine Verfügbarkeitsgruppe mit dem Namen **ag1**, die über einen **External** `cluster_type` verfügt und der Verfügbarkeitsgruppe die Berechtigung zum Erstellen einer Datenbank gewährt.

Ersetzen Sie vor dem Ausführen der folgenden Skripts die Platzhalter `<node1>`, `<node2>` und `<node3>` (Konfigurationsreplikat) durch den Namen der VMs, die Sie in den vorherigen Schritten erstellt haben.

```tsql
CREATE AVAILABILITY GROUP [ag1]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR REPLICA ON
    N'<node1>' WITH (
        ENDPOINT_URL = N'tcp://<node1>:<5022>',
        AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
        FAILOVER_MODE = EXTERNAL,
        SEEDING_MODE = AUTOMATIC
        ),

    N'<node2>' WITH (
        ENDPOINT_URL = N'tcp://<node2>:\<5022>',
        AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
        FAILOVER_MODE = EXTERNAL,
        SEEDING_MODE = AUTOMATIC
        ),
    
    N'<node3>' WITH (
        ENDPOINT_URL = N'tcp://<node3>:<5022>',
        AVAILABILITY_MODE = CONFIGURATION_ONLY
        );
          
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

### <a name="join-the-secondary-replicas"></a>Verknüpfen der sekundären Replikate

Führen Sie die nachfolgenden Befehle auf allen sekundären Replikaten aus. Diese Befehle verknüpfen die sekundären Replikate mit der Verfügbarkeitsgruppe „ag1“ mit dem primären Replikat und stellen der Verfügbarkeitsgruppe den Zugriff auf das Erstellen von Datenbanken bereit.

```tsql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
GO
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
GO
```

### <a name="add-a-database-to-the-availability-group-created-above"></a>Hinzufügen einer Datenbank zur oben erstellten Verfügbarkeitsgruppe

Stellen Sie eine Verbindung mit dem primären Replikat her, und führen Sie die folgenden T-SQL-Befehle aus, um folgende Schritte auszuführen:

1. Erstellen Sie eine Beispieldatenbank mit dem Namen **db1**, die zur Verfügbarkeitsgruppe hinzugefügt wird.
2. Legen Sie das Wiederherstellungsmodell der Datenbank auf „Vollständig“ fest. Alle Datenbanken in einer Verfügbarkeitsgruppe erfordern das vollständige Wiederherstellungsmodell.
3. Sichern Sie die Datenbank. Eine Datenbank erfordert mindestens eine vollständige Sicherung, bevor Sie diese zu einer Verfügbarkeitsgruppe hinzufügen können.

```tsql
CREATE DATABASE [db1]; -- creates a database named db1
GO
ALTER DATABASE [db1] SET RECOVERY FULL; -- set the database in full recovery mode
GO
BACKUP DATABASE [db1] -- backs up the database to disk TO DISK = N'/var/opt/mssql/data/db1.bak';
GO
ALTER AVAILABILITY GROUP [ag1] ADD DATABASE [db1]; -- adds the database db1 to the AG
GO
```

Nachdem Sie die vorherigen Schritte erfolgreich ausgeführt haben, sehen Sie, dass eine Verfügbarkeitsgruppe **ag1** erstellt wurde und die drei VMs als Replikat mit einem primären Replikat, einem sekundären Replikat und einem Konfigurationsreplikat hinzugefügt werden. **ag1** enthält eine Datenbank.

## <a name="deploy-the-sql-server-availability-group-workload-hpe-cluster-manager"></a>Bereitstellen der Arbeitsauslastung für die SQL Server-Verfügbarkeitsgruppe (HPE Cluster-Manager)

Stellen Sie in HPE Serviceguard die SQL Server-Arbeitsauslastung in der Verfügbarkeitsgruppe über die Benutzeroberfläche des Serviceguard-Cluster-Managers bereit.
   
Stellen Sie die Arbeitsauslastung der Verfügbarkeitsgruppe bereit, und aktivieren Sie die Hochverfügbarkeit und die Notfallwiederherstellung (DR) über den Serviceguard-Cluster mithilfe der [Serviceguard manager graphical user interface (grafischen Benutzeroberfläche des Serviceguard-Managers)](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Protect_your_alwayson_availability_group). Weitere Informationen finden Sie im Abschnitt **Schützen von Microsoft SQL Server für Linux für Always On-Verfügbarkeitsgruppen**.

## <a name="perform-automatic-failover-and-join-the-node-back-to-cluster"></a>Ein automatisches Failover ausführen und den Knoten mit dem Cluster verknüpfen

Für den automatischen Failovertest können Sie das primäre Replikat herunterfahren (ausschalten). Dadurch wird die plötzliche Nichtverfügbarkeit des primären Knotens repliziert. Das erwartete Verhalten sieht wie folgt aus:

1. Der Cluster-Manager stuft eines der sekundären Replikate in der Verfügbarkeitsgruppe auf „Primär“ hoch.
2. Das fehlgeschlagene primäre Replikat wird automatisch dem Cluster beitreten, nachdem es gesichert wurde. Der Cluster-Manager stuft ihn auf das sekundäre Replikat hoch.

Informationen zu HPE Serviceguard finden Sie im Abschnitt [**Testing the setup for failover readiness (Testen des Setups auf Failoverbereitschaft)**](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Test_the_setup_preparedness).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über [Always On-Verfügbarkeitsgruppen unter Linux](sql-server-linux-availability-group-overview.md)
