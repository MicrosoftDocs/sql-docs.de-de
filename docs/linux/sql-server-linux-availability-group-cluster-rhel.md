---
title: 'RHEL: Konfigurieren von Verfügbarkeitsgruppen für SQL Server unter Linux'
description: Erfahren Sie, wie Sie eine Verfügbarkeitsgruppe konfigurieren, wenn Sie Red Hat Enterprise Linux (RHEL) für SQL Server ausführen.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 01/23/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: b7102919-878b-4c08-a8c3-8500b7b42397
ms.openlocfilehash: 2790bbe9eb648a2d599d1b0bad8f514ab72eda50
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338391"
---
# <a name="configure-rhel-cluster-for-sql-server-availability-group"></a>Konfigurieren von RHEL-Clustern für eine SQL Server-Verfügbarkeitsgruppe

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Dokument wird erläutert, wie Sie einen Verfügbarkeitsgruppencluster mit drei Knoten für SQL Server unter Red Hat Enterprise Linux erstellen. Zur Gewährleistung von Hochverfügbarkeit benötigt eine Verfügbarkeitsgruppe unter Linux drei Knoten. Weitere Informationen hierzu finden Sie unter [Hochverfügbarkeit und Schutz von Daten für Verfügbarkeitsgruppenkonfigurationen](sql-server-linux-availability-group-ha.md). Die Clusteringebene basiert auf einem [Hochverfügbarkeits-Add-On](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) für Red Hat Enterprise Linux (RHEL), das auf [Pacemaker](https://clusterlabs.org/) aufbaut. 

> [!NOTE] 
> Für den Zugriff auf die vollständige Red Hat-Dokumentation ist ein gültiges Abonnement erforderlich. 

Weitere Informationen zur Clusterkonfiguration, den Optionen für Ressourcen-Agents und der Verwaltung finden Sie in der [Referenzdokumentation von RHEL](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html).

> [!NOTE] 
> SQL Server ist unter Linux nicht so eng in Pacemaker integriert wie beim Windows Server-Failoverclustering. Eine SQL Server-Instanz erkennt den Cluster nicht. Pacemaker bietet eine Clusterressourcenorchestrierung. Der virtuelle Netzwerkname ist außerdem spezifisch für das Windows Server-Failoverclustering. In Pacemaker gibt es hierfür keine Entsprechung. Dynamische Verwaltungssichten der Verfügbarkeitsgruppen (DMVs), die Clusterinformationen abfragen, geben in Pacemaker leere Zeilen zurück. Registrieren Sie in DNS den Listenernamen manuell mit der zum Erstellen der virtuellen IP-Ressource verwendeten IP-Adresse, um nach einem Failover einen Listener für eine transparente Neuverbindung zu erstellen. 

In den folgenden Abschnitten werden die Schritte zum Einrichten eines Pacemaker-Clusters und Hinzufügen einer Verfügbarkeitsgruppe als Ressource im Cluster für Hochverfügbarkeit beschrieben.

## <a name="roadmap"></a>Roadmap

Die Schritte zum Erstellen einer Verfügbarkeitsgruppe auf Linux-Servern für Hochverfügbarkeit unterscheiden sich von den Schritten in Windows Server-Failoverclustern. Die allgemeinen Schritte werden in der folgenden Liste beschrieben: 

1. [Konfigurieren Sie SQL Server in den Clusterknoten](sql-server-linux-setup.md).

2. [Erstellen Sie die Verfügbarkeitsgruppe](sql-server-linux-availability-group-configure-ha.md). 

3. Konfigurieren Sie einen Cluster Resource Manager wie Pacemaker. Diese Anweisungen finden Sie in diesem Dokument.
   
   Die Art und Weise des Konfigurierens eines Clusterressourcen-Managers hängt von der jeweiligen Linux-Distribution ab. 

   >[!IMPORTANT]
   >In Produktionsumgebungen wird zur Gewährleistung vonHochverfügbarkeit ein Fencing-Agent wie STONITH benötigt. In den Demos dieser Dokumentation werden keine Fencing-Agents verwendet. Die Demos dienen lediglich zu Testzwecken und Überprüfungen.
   
   >Ein Linux-Cluster verwendet Fencing, um den Cluster in einen bekannten Zustand zurückzusetzen. Wie das Fencing konfiguriert wird, hängt von der Verteilung und der Umgebung ab. Derzeit ist Fencing in einigen Cloudumgebungen nicht verfügbar. Weitere Informationen finden Sie unter [Supportrichtlinien für RHEL-Hochverfügbarkeitscluster – Virtualisierungsplattformen](https://access.redhat.com/articles/29440).

4. [Fügen Sie die Verfügbarkeitsgruppe als Ressource im Cluster hinzu](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource).  

## <a name="configure-high-availability-for-rhel"></a>Konfigurieren der Hochverfügbarkeit für RHEL

Aktivieren Sie das Hochverfügbarkeitsabonnement, und konfigurieren Sie dann Pacemaker, um die Hochverfügbarkeit für RHEL zu konfigurieren.

### <a name="enable-the-high-availability-subscription-for-rhel"></a>Aktivieren des Hochverfügbarkeitsabonnements für RHEL

Jeder Knoten im Cluster muss über ein entsprechendes Abonnement für RHEL und das Hochverfügbarkeits-Add-On verfügen. Überprüfen Sie die Anforderungen unter [Installieren von Hochverfügbarkeitsclusterpaketen in Red Hat Enterprise Linux](https://access.redhat.com/solutions/45930). Führen Sie die folgenden Schritte aus, um das Abonnement un die Repositorys zu konfigurieren:

1. Registrieren Sie das System.

   ```bash
   sudo subscription-manager register
   ```

   Geben Sie Ihren Benutzernamen und Ihr Kennwort ein.   

1. Listen Sie die verfügbaren Pools für die Registrierung auf.

   ```bash
   sudo subscription-manager list --available
   ```

   Notieren Sie sich aus der Liste der verfügbaren Pools die Pool-ID für das Hochverfügbarkeitsabonnement.

1. Aktualisieren Sie das folgende Skript. Ersetzen Sie `<pool id>` durch die Pool-ID für Hochverfügbarkeit aus dem vorherigen Schritt. Führen Sie das Skript aus, um das Abonnement anzufügen.

   ```bash
   sudo subscription-manager attach --pool=<pool id>
   ```

1. Aktivieren Sie das Repository.

   **RHEL 7**

   ```bash
   sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-rpms
   ```

   **RHEL 8**

   ```bash
   sudo subscription-manager repos --enable=rhel-8-for-x86_64-highavailability-rpms
   ```

Weitere Informationen finden Sie unter [Pacemaker – der hochverfügbare Open-Source-Cluster](https://clusterlabs.org/pacemaker/). 

Führen Sie nach dem Konfigurieren des Abonnements die folgenden Schritte aus, um Pacemaker zu konfigurieren:

### <a name="configure-pacemaker"></a>Konfigurieren von Pacemaker

Führen Sie die folgenden Schritte aus, um nach der Registrierung des Abonnements Pacemaker zu konfigurieren:
   
[!INCLUDE [RHEL-Configure-Pacemaker](../includes/ss-linux-cluster-pacemaker-configure-rhel.md)]

Verwenden Sie nach dem Konfigurieren von Pacemaker `pcs`, um mit dem Cluster zu interagieren. Führen Sie alle Befehle auf einem Knoten aus dem Cluster aus. 

## <a name="configure-fencing-stonith"></a>Konfigurieren des Fencing (STONITH)

Bei Anbietern von Pacemaker-Clustern muss für eine unterstützte Clustereinrichtung STONITH aktiviert und ein Fencinggerät konfiguriert sein. STONITH steht für „shoot the other node in the head“ (Schieß dem anderen Knoten in den Kopf). Wenn der Clusterressourcen-Manager den Status eines Knotens oder einer Ressource auf einem Knoten nicht ermitteln kann, wird der Cluster mithilfe von Fencing wieder in einen bekannten Status versetzt.

Ein STONITH-Gerät stellt einen Fencing-Agent bereit. Ein Beispiel für das Erstellen eines STONITH-Geräts für diesen Cluster in Azure finden Sie unter [Einrichten von Pacemaker unter Red Hat Enterprise Linux in Azure](/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker/#1-create-the-stonith-devices). Ändern Sie die Anweisungen für Ihre Umgebung.

Durch Fencing auf Ressourcenebene wird sichergestellt, dass im Falle eines Ausfalls durch die Konfiguration einer Ressource keine Daten beschädigt werden. Sie können Fencing auf Ressourcenebene beispielsweise dazu verwenden, den Datenträger auf einem Knoten bei einem Ausfall der Kommunikationsverbindung als veraltet zu markieren. 

Mit dem Fencing auf Knotenebene wird sichergestellt, dass ein Knoten keine Ressourcen ausführt. Dies erfolgt durch Zurücksetzen des Knotens. Pacemaker unterstützt eine Vielzahl von Fencinggeräten. Beispiele hierfür sind eine unterbrechungsfreie Stromversorgung oder Verwaltungsschnittstellenkarten für Server.

Weitere Informationen zu STONITH und Fencing finden Sie in den folgenden Artikeln:

* [Grundlegendes zu Pacemaker-Clustern](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/index.html)
* [Fencing und STONITH](https://clusterlabs.org/doc/crm_fencing.html)
* [Hochverfügbarkeits-Add-On von Red Hat mit Pacemaker: Fencing](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Configuring_the_Red_Hat_High_Availability_Add-On_with_Pacemaker/ch-fencing-HAAR.html)

>[!NOTE]
>Da die Fencingkonfiguration auf Knotenebene stark von Ihrer Umgebung abhängt, wird sie für dieses Tutorial deaktiviert (sie kann später konfiguriert werden). Mit dem folgenden Skript wird Fencing auf Knotenebene deaktiviert:
>
>```bash
>sudo pcs property set stonith-enabled=false
>``` 
>
>STONITH wird lediglich zu Testzwecken deaktiviert. Wenn Sie Pacemaker in einer Produktionsumgebung verwenden möchten, sollten Sie je nach Ihrer Umgebung eine STONITH-Implementierung planen und immer aktivieren.

## <a name="set-cluster-property-cluster-recheck-interval"></a>Festlegen der Clustereigenschaft „cluster-recheck-interval“

`cluster-recheck-interval` gibt das Abrufintervall an, mit dem der Cluster prüft, ob Änderungen in den Ressourcenparametern, Einschränkungen oder anderen Clusteroptionen vorliegen. Wenn ein Replikat ausfällt, versucht der Cluster, das Replikat in einem Intervall neu zu starten, das durch den Wert `failure-timeout` und den Wert `cluster-recheck-interval` gebunden ist. Wenn `failure-timeout` beispielsweise auf 60 Sekunden und `cluster-recheck-interval` auf 120 Sekunden festgelegt ist, wird der Neustart in einem Intervall versucht, das größer als 60 Sekunden, aber kleiner als 120 Sekunden ist. Es wird empfohlen, „failure-timeout“ auf 60 Sekunden und „cluster-recheck-interval“ auf einen Wert größer als 60 Sekunden festzulegen. Es wird nicht empfohlen, „cluster-recheck-interval“ auf einen kleinen Wert festzulegen.

So aktualisieren Sie den Eigenschaftswert auf ein Ausführungsintervall von `2 minutes`:

```bash
sudo pcs property set cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> Mit allen Distributionen (einschließlich RHEL 7.3 und 7.4), für die das neueste verfügbare Pacemaker-Paket 1.1.18-11.el7 verwendet wird, wird ein Behavior Change für die Clustereinstellung „start-failure-is-fatal“ für den Fall eingeführt, dass deren Wert auf FALSE festgelegt ist. Diese Änderung wirkt sich auf den Failoverworkflow aus. Wenn ein primäres Replikat ausfällt, wird für den Cluster ein Failover auf eines der verfügbaren sekundären Replikate erwartet. Stattdessen werden die Benutzer bemerken, dass der Cluster weiterhin versucht, das ausgefallene primäre Replikat zu starten. Wenn dieses primäre Replikat (aufgrund eines dauerhaften Ausfalls) nicht online geschaltet wird, führt der Cluster kein Failover zu einem anderen verfügbaren sekundären Replikat durch. Aufgrund dieser Änderung ist eine zuvor empfohlene Konfiguration zum Festlegen von „start-failure-is-fatal“ nicht mehr gültig, und für die Einstellung muss der Standardwert `true` wiederhergestellt werden. Darüber hinaus muss die Verfügbarkeitsgruppenressource aktualisiert werden, um die Eigenschaft `failover-timeout` einzubeziehen. 

So aktualisieren Sie den Eigenschaftswert auf ein Ausführungsintervall von `true`:

```bash
sudo pcs property set start-failure-is-fatal=true
```

So aktualisieren Sie die `ag_cluster`-Ressourceneigenschaft `failure-timeout` auf `60s`:

```bash
pcs resource update ag_cluster meta failure-timeout=60s
```


Weitere Informationen zu Pacemaker-Clustereigenschaften finden Sie unter [Pacemaker-Clustereigenschaften](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/ch-clusteropts-HAAR.html).

## <a name="create-a-sql-server-login-for-pacemaker"></a>Erstellen einer SQL Server-Anmeldung für Pacemaker

[!INCLUDE [SQL-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="create-availability-group-resource"></a>Erstellen von Verfügbarkeitsgruppenressourcen

Verwenden Sie den Befehl `pcs resource create`, und legen Sie die Ressourceneigenschaften fest, um die Verfügbarkeitsgruppenressource zu erstellen. Mit dem folgenden Befehl wird eine `ocf:mssql:ag`-Ressource vom Typ Master/untergeordnet für die Verfügbarkeitsgruppe mit dem Namen `ag1` erstellt.

**RHEL 7**

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=60s master notify=true
```

**RHEL 8**

Mit der Verfügbarkeit von **RHEL 8** hat sich die Syntax für das Erstellen geändert. Wenn Sie **RHEL 8** verwenden, ist die Terminologie nun nicht mehr `master`, sondern `promotable`. Verwenden Sie anstelle des obigen Befehls den folgenden Befehl zum Erstellen: 

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=60s promotable notify=true
```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

<a name="createIP"></a>

## <a name="create-virtual-ip-resource"></a>Erstellen einer virtuellen IP-Ressource

Führen Sie den folgenden Befehl auf einem Knoten aus, um die virtuelle IP-Adressressource zu erstellen. Verwenden Sie eine verfügbare statische IP-Adresse aus dem Netzwerk. Ersetzen Sie die IP-Adresse zwischen `<10.128.16.240>` durch eine gültige IP-Adresse.

```bash
sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=<10.128.16.240>
```

In Pacemaker ist der Name eines virtuellen Servers nicht gleichwertig. Registrieren Sie die virtuelle IP-Ressourcenadresse und den gewünschten Namen des virtuellen Servers in DNS, wenn Sie anstelle einer IP-Adresse eine Verbindungszeichenfolge verwenden möchten, die auf einen Zeichenfolgenservernamen verweist. Registrieren Sie für Notfallwiederherstellungskonfigurationen den gewünschten Namen des virtuellen Servers und die IP-Adresse bei den DNS-Servern am primären Standort und dem Standort für die Notfallwiederherstellung.

## <a name="add-colocation-constraint"></a>Hinzufügen einer Kollokationseinschränkung

Fast jede Entscheidung in einem Pacemaker-Cluster, wie etwa die Auswahl des Orts, an dem eine Ressource ausgeführt werden soll, erfolgt durch Vergleichen von Bewertungen. Die Bewertungen werden pro Ressource berechnet. Der Clusterressourcen-Manager wählt den Knoten mit der höchsten Bewertung für eine bestimmte Ressource aus. Wenn ein Knoten eine negative Bewertung für eine Ressource aufweist, kann die Ressource auf diesem Knoten nicht ausgeführt werden.

Auf einem Pacemaker-Cluster können Sie die Entscheidungen des Clusters mit Einschränkungen beeinflussen. Einschränkungen weisen eine Bewertung auf. Wenn eine Einschränkung eine Bewertung unter `INFINITY` aufweist, wird diese von Pacemaker lediglich als Empfehlung angesehen. Eine Bewertung von `INFINITY` ist obligatorisch.

Definieren Sie eine Kollokationseinschränkung mit der Bewertung INFINITY, um sicherzustellen, dass das primäre Replikat und die virtuellen IP-Ressourcen auf demselben Host ausgeführt werden. Führen Sie den folgenden Befehl auf einem Knoten aus, um eine Kollokationseinschränkung hinzuzufügen.

### <a name="rhel-7"></a>RHEL 7

Beim Erstellen der `ag_cluster`-Ressource in RHEL 7 wird die Ressource als `ag_cluster-master` erstellt. Verwenden Sie den folgenden Befehl für RHEL 7:

```bash
sudo pcs constraint colocation add virtualip ag_cluster-master INFINITY with-rsc-role=Master
```

### <a name="rhel-8"></a>RHEL 8

Beim Erstellen der `ag_cluster`-Ressource in RHEL 8 wird die Ressource als `ag_cluster-clone` erstellt. Verwenden Sie den folgenden Befehl für RHEL 8:

```bash
sudo pcs constraint colocation add virtualip with master ag_cluster-clone INFINITY with-rsc-role=Master
```

## <a name="add-ordering-constraint"></a>Hinzufügen einer Sortierungseinschränkung

Die Kollokationseinschränkung weist eine implizite Sortierungseinschränkung auf. Damit wird die virtuelle IP-Adressressource verschoben, bevor diese die Verfügbarkeitsgruppenressource verschiebt. Die Ereignisse laufen standardmäßig wie folgt ab:

1. Der Benutzer gibt den Befehl `pcs resource move` aus, um die primäre Verfügbarkeitsgruppe von node1 zu node2 zu verschieben.
1. Die virtuelle IP-Ressource wird auf Knoten 1 angehalten.
1. Die virtuelle IP-Ressource wird auf Knoten 2 gestartet.
  
   >[!NOTE]
   >Zu diesem Zeitpunkt verweist die IP-Adresse vorübergehend auf Knoten 2, während Knoten 2 noch ein sekundäres Replikat vor dem Failover darstellt. 
   
1. Die primäre Verfügbarkeitsgruppe auf Knoten 1 wird auf sekundär herabgestuft.
1. Die sekundäre Verfügbarkeitsgruppe auf Knoten 2 wird auf primär heraufgestuft. 

Fügen Sie eine Sortierungseinschränkung hinzu, um zu vermeiden, dass die IP-Adresse vorübergehend auf den Knoten mit dem sekundären Replikat vor dem Failover verweist. 

Führen Sie den folgenden Befehl auf einem Knoten aus, um eine Sortierungseinschränkung hinzuzufügen:

### <a name="rhel-7"></a>RHEL 7

```bash
sudo pcs constraint order promote ag_cluster-master then start virtualip
```

### <a name="rhel-8"></a>RHEL 8

```bash
sudo pcs constraint order promote ag_cluster-clone then start virtualip
```

>[!IMPORTANT]
>Nachdem Sie den Cluster konfiguriert und die Verfügbarkeitsgruppe als Clusterressource hinzugefügt haben, können Sie ein Failover der Verfügbarkeitsgruppenressourcen nicht mehr mit Transact-SQL durchführen. SQL Server-Clusterressourcen unter Linux sind nicht so eng mit dem Betriebssystem gekoppelt wie in einem Windows Server-Failovercluster (WSFC). Der SQL Server-Dienst kann das Vorhandensein des Clusters nicht erkennen. Die gesamte Orchestrierung erfolgt über die Clusterverwaltungstools. Verwenden Sie in RHEL oder unter Ubuntu `pcs`-Tools und in SLES `crm`-Tools. 

Führen Sie für die Verfügbarkeitsgruppe ein Failover mit `pcs` aus. Initiieren Sie kein Failover mit Transact-SQL. Anweisungen finden Sie unter [Failover](sql-server-linux-availability-group-failover-ha.md#failover).

## <a name="next-steps"></a>Nächste Schritte

[Arbeiten mit einer Verfügbarkeitsgruppe mit Hochverfügbarkeit](sql-server-linux-availability-group-failover-ha.md)
