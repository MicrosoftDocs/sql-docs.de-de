---
title: Konfigurieren von Ubuntu-Clustern für eine Verfügbarkeitsgruppe
titleSuffix: SQL Server on Linux
description: Erfahren Sie, wie Sie ein Cluster mit drei Knoten unter Ubuntu erstellen und eine zuvor erstellte Verfügbarkeitsgruppenressource im Cluster hinzufügen.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 04/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: dd0d6fb9-df0a-41b9-9f22-9b558b2b2233
ms.openlocfilehash: 13b719848e9cbced670e58ae3a747967a8901ada
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338314"
---
# <a name="configure-ubuntu-cluster-and-availability-group-resource"></a>Konfigurieren von Ubuntu-Clustern und Verfügbarkeitsgruppenressource

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Dokument wird erläutert, wie ein Cluster mit drei Knoten unter Ubuntu erstellt und eine zuvor erstellte Verfügbarkeitsgruppe als Ressource im Cluster hinzugefügt wird. Zur Gewährleistung von Hochverfügbarkeit benötigt eine Verfügbarkeitsgruppe unter Linux drei Knoten. Weitere Informationen hierzu finden Sie unter [Hochverfügbarkeit und Schutz von Daten für Verfügbarkeitsgruppenkonfigurationen](sql-server-linux-availability-group-ha.md).

> [!NOTE] 
> An diesem Punkt ist die Integration von SQL Server in Pacemaker unter Linux nicht so stark gekoppelt wie in WSFC unter Windows. Aus SQL kann der Cluster nicht erkannt werden. Die gesamte Orchestrierung erfolgt außerhalb durch Pacemaker. Zudem wird der Dienst als eigenständige Instanz von Pacemaker gesteuert. Ferner gilt der virtuelle Netzwerkname für WSFC. Dazu gibt es in Pacemaker keine Entsprechung. Dynamische Always On-Verwaltungssichten, die Clusterinformationen abfragen, geben leere Zeilen zurück. Sie können weiterhin einen Listener erstellen, um ihn nach einem Failover für eine transparente erneute Verbindung zu verwenden. Sie müssen jedoch den Listenernamen im DNS-Server manuell mit der IP-Adresse registrieren, die zum Erstellen der virtuellen IP-Ressource verwendet wurde (eine Erläuterung hierzu finden Sie in den folgenden Abschnitten).

In den folgenden Abschnitten werden die Schritte zum Einrichten einer Failoverclusterlösung erläutert. 

## <a name="roadmap"></a>Roadmap

Die Schritte zum Erstellen einer Verfügbarkeitsgruppe auf Linux-Servern für Hochverfügbarkeit unterscheiden sich von den Schritten in Windows Server-Failoverclustern. Die allgemeinen Schritte werden in der folgenden Liste beschrieben: 

1. [Konfigurieren Sie SQL Server in den Clusterknoten](sql-server-linux-setup.md).

2. [Erstellen Sie die Verfügbarkeitsgruppe](sql-server-linux-availability-group-configure-ha.md). 

3. Konfigurieren Sie einen Cluster Resource Manager wie Pacemaker. Diese Anweisungen finden Sie in diesem Dokument.
   
   Die Art und Weise des Konfigurierens eines Clusterressourcen-Managers hängt von der jeweiligen Linux-Distribution ab. 

   >[!IMPORTANT]
   >In Produktionsumgebungen wird zur Gewährleistung vonHochverfügbarkeit ein Fencing-Agent wie STONITH benötigt. In den Demos dieser Dokumentation werden keine Fencing-Agents verwendet. Die Demos dienen lediglich zu Testzwecken und Überprüfungen. 
   >
   >Ein Linux-Cluster verwendet Fencing, um den Cluster in einen bekannten Zustand zurückzusetzen. Wie das Fencing konfiguriert wird, hängt von der Verteilung und der Umgebung ab. Zurzeit ist das Fencing nicht in allen Cloudumgebungen verfügbar. Weitere Informationen finden Sie unter [Supportrichtlinien für RHEL-Hochverfügbarkeitscluster – Virtualisierungsplattformen](https://access.redhat.com/articles/29440).
   >
   >Das Fencing wird in der Regel im Betriebssystem implementiert und ist von der Umgebung abhängig. Anweisungen für das Fencing finden Sie in der Verteilerdokumentation des Betriebssystems.

5.  [Fügen Sie die Verfügbarkeitsgruppe als Ressource im Cluster hinzu](sql-server-linux-availability-group-cluster-ubuntu.md#create-availability-group-resource). 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installieren und Konfigurieren von Pacemaker auf jedem Clusterknoten

1. Öffnen Sie auf allen Knoten die Firewallports. Öffnen Sie den Port für den Pacemaker-Dienst für hohe Verfügbarkeit, die SQL Server-Instanz und den Endpunkt der Verfügbarkeitsgruppe. Der Standard-TCP-Port für den Server, auf dem SQL Server ausgeführt wird, ist 1433.  

   ```bash
   sudo ufw allow 2224/tcp
   sudo ufw allow 3121/tcp
   sudo ufw allow 21064/tcp
   sudo ufw allow 5405/udp
        
   sudo ufw allow 1433/tcp # Replace with TDS endpoint
   sudo ufw allow 5022/tcp # Replace with DATA_MIRRORING endpoint
        
   sudo ufw reload
   ```
   
   Alternativ können Sie einfach die Firewall deaktivieren:
        
   ```bash
   sudo ufw disable
   ```

1. Installieren Sie Pacemaker-Pakete. Führen Sie die folgenden Befehle auf allen Knoten aus:

   ```bash
   sudo apt-get install pacemaker pcs fence-agents resource-agents
   ```

2. Legen Sie das Kennwort für den Standardbenutzer fest, der beim Installieren von Pacemaker und Corosync-Paketen erstellt wird. Verwenden Sie auf allen Knoten dasselbe Kennwort. 

   ```bash
   sudo passwd hacluster
   ```

## <a name="enable-and-start-pcsd-service-and-pacemaker"></a>Aktivieren und starten Sie den pcsd-Dienst und Pacemaker.

Mit dem folgenden Befehl werden pcsd-Dienst und Pacemaker aktiviert und gestartet. Führen Sie dies auf allen Knoten aus. So können die Knoten dem Cluster nach dem Neustart erneut beitreten. 

```bash
sudo systemctl enable pcsd
sudo systemctl start pcsd
sudo systemctl enable pacemaker
```
>[!NOTE]
>Der Befehl zum Aktivieren von Pacemaker könnte mit der Fehlermeldung „pacemaker Default-Start contains no runlevels, aborting“ (Pacemaker-Standardstart enthält keine Ausführungsebenen, Abbruch) beendet werden. Dies ist harmlos, die Clusterkonfiguration kann fortgesetzt werden. 

## <a name="create-the-cluster"></a>Erstellen des Clusters

1. Entfernen Sie alle vorhandenen Clusterkonfigurationen von allen Knoten. 

   Wenn Sie „sudo apt-get install pcs“ ausführen, werden Pacemaker, Corosync und pcs gleichzeitig installiert, und alle drei Dienste werden gestartet.  Beim Starten von Corosync wird eine „/etc/cluster/corosync.conf“-Vorlagendatei generiert.  Damit die nächsten Schritte erfolgreich ausgeführt werden, sollte diese Datei nicht vorhanden sein. Zur Problemumgehung wird Pacemaker/Corosync beendet und „/etc/cluster/corosync.conf“ gelöscht. Anschließend werden die nächsten Schritte erfolgreich abgeschlossen. „pcs cluster destroy“ erfüllt denselben Zweck, und Sie können diesen Befehl als einmaligen ersten Clustersetupschritt verwenden.
   
   Mit dem folgenden Befehl werden alle vorhandenen Clusterkonfigurationsdateien entfernt und alle Clusterdienste angehalten. Dadurch wird der Cluster dauerhaft zerstört. Führen Sie dies als ersten Schritt in einer Präproduktionsumgebung aus. Beachten Sie, dass der Pacemaker-Dienst von „pcs cluster destroy“ deaktiviert wurde und erneut aktiviert werden muss. Führen Sie den folgenden Befehl auf allen Knoten aus.
   
   >[!WARNING]
   >Der Befehl zerstört alle vorhandenen Clusterressourcen.

   ```bash
   sudo pcs cluster destroy 
   sudo systemctl enable pacemaker
   ```

1. Erstellen Sie den Cluster. 

   >[!WARNING]
   >Aufgrund eines bekannten Problems, das der Clusteringanbieter untersucht, tritt beim Starten des Clusters („pcs cluster start“) der folgende Fehler auf. Dies liegt daran, dass die in „/etc/corosync/corosync.conf“ konfigurierte Protokolldatei, die beim Ausführen des Befehls zum Einrichten des Clusters erstellt wird, falsch ist. Um dieses Problem zu umgehen, ändern Sie die Protokolldatei in „/var/log/corosync/corosync.log“ um. Alternativ könnten Sie die Datei „/var/log/cluster/corosync.log“ erstellen.
 
   ```Error
   Job for corosync.service failed because the control process exited with error code. 
   See "systemctl status corosync.service" and "journalctl -xe" for details.
   ```
  
Mit dem folgenden Befehl wird ein Cluster mit drei Knoten erstellt. Bevor Sie das Skript ausführen, ersetzen Sie die Werte zwischen `< ... >`. Führen Sie den folgenden Befehl auf dem primären Knoten aus. 

   ```bash
   sudo pcs cluster auth <node1> <node2> <node3> -u hacluster -p <password for hacluster>
   sudo pcs cluster setup --name <clusterName> <node1> <node2...> <node3>
   sudo pcs cluster start --all
   sudo pcs cluster enable --all
   ```
   
   >[!NOTE]
   >Wenn Sie vorher einen Cluster auf denselben Knoten konfiguriert haben, müssen Sie die Option „--force“ verwenden, wenn Sie „pcs cluster setup“ ausführen. Beachten Sie, dass dies der Ausführung von „pcs cluster destroy“ entspricht, und der Peacemaker-Dienst muss erneut mithilfe von „sudo systemctl enable pacemaker“ aktiviert werden.


## <a name="configure-fencing-stonith"></a>Konfigurieren des Fencing (STONITH)

Bei Anbietern von Pacemaker-Clustern muss für eine unterstützte Clustereinrichtung STONITH aktiviert und ein Fencinggerät konfiguriert sein. Wenn der Cluster Resource Manager den Status eines Knotens oder einer Ressource auf einem Knoten nicht ermitteln kann, wird das Fencing verwendet, um den Cluster wieder in einen bekannten Status zu versetzen. Durch die Umgrenzung auf Ressourcenebene wird vor allem sichergestellt, dass bei einem Ausfall durch die Konfiguration einer Ressource keine Daten beschädigt werden. Das Fencing auf Ressourcenebene können Sie beispielsweise mit DRBD (Distributed Replicated Block Device) verwenden, damit die Festplatte auf einem Knoten bei einem Ausfall der Kommunikationsverbindung als veraltet markiert wird. Mit dem Fencing auf Knotenebene wird sichergestellt, dass ein Knoten keine Ressourcen ausführt. Dies erfolgt durch Zurücksetzen des Knotens. Die zugehörige Pacemaker-Implementierung wird als STONITH bezeichnet (was für „Shoot the Other Node in the Head“ (Schieß dem anderen Knoten in den Kopf) steht). Pacemaker unterstützt eine Vielzahl von Umgrenzungsgeräten, z.B. eine unterbrechungsfreie Stromversorgung oder Verwaltungsschnittstellenkarten für Server. Weitere Informationen finden Sie unter [Pacemaker Clusters from Scratch](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/) (Pacemaker-Cluster völlig neu erstellen) und [Fencing and Stonith](https://clusterlabs.org/doc/crm_fencing.html) (Umgrenzung und STONITH). 

Da die Umgrenzungskonfiguration auf Knotenebene stark von Ihrer Umgebung abhängt, wird sie für dieses Tutorial deaktiviert (sie kann zu einem späteren Zeitpunkt konfiguriert werden). Führen Sie das folgende Befehlsskript auf dem primären Knoten aus: 

```bash
sudo pcs property set stonith-enabled=false
```

>[!IMPORTANT]
>STONITH wird lediglich zu Testzwecken deaktiviert. Wenn Sie Pacemaker in einer Produktionsumgebung verwenden möchten, sollten Sie je nach Ihrer Umgebung eine STONITH-Implementierung planen und immer aktivieren. Informationen zu Fencing-Agents für eine bestimmte Verteilung erhalten Sie beim Hersteller des Betriebssystems. 

## <a name="set-cluster-property-cluster-recheck-interval"></a>Festlegen der Clustereigenschaft „cluster-recheck-interval“

`cluster-recheck-interval` gibt das Abrufintervall an, mit dem der Cluster prüft, ob Änderungen in den Ressourcenparametern, Einschränkungen oder anderen Clusteroptionen vorliegen. Wenn ein Replikat ausfällt, versucht der Cluster, das Replikat in einem Intervall neu zu starten, das durch den Wert `failure-timeout` und den Wert `cluster-recheck-interval` gebunden ist. Wenn `failure-timeout` beispielsweise auf 60 Sekunden und `cluster-recheck-interval` auf 120 Sekunden festgelegt ist, wird der Neustart in einem Intervall versucht, das größer als 60 Sekunden, aber kleiner als 120 Sekunden ist. Es wird empfohlen, „failure-timeout“ auf 60 Sekunden und „cluster-recheck-interval“ auf einen Wert größer als 60 Sekunden festzulegen. Es wird nicht empfohlen, „cluster-recheck-interval“ auf einen kleinen Wert festzulegen.

So aktualisieren Sie den Eigenschaftswert auf ein Ausführungsintervall von `2 minutes`:

```bash
sudo pcs property set cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> Wenn Sie bereits über eine Verfügbarkeitsgruppenressource verfügen, die von einem Pacemaker-Cluster verwaltet wird, wird mit allen Verteilungen, für die das aktuellste verfügbare Pacemaker-Pakte 1.1.18-11.el7 verwendet wird, ein Behavior Change für die Clustereinstellung „start-failure-is-fatal“ für den Fall eingeführt, dass deren Wert auf FALSE festgelegt ist. Diese Änderung wirkt sich auf den Failoverworkflow aus. Wenn ein primäres Replikat ausfällt, wird für den Cluster ein Failover auf eines der verfügbaren sekundären Replikate erwartet. Stattdessen werden die Benutzer bemerken, dass der Cluster weiterhin versucht, das ausgefallene primäre Replikat zu starten. Wenn dieses primäre Replikat (aufgrund eines dauerhaften Ausfalls) nicht online geschaltet wird, führt der Cluster kein Failover zu einem anderen verfügbaren sekundären Replikat durch. Aufgrund dieser Änderung ist eine zuvor empfohlene Konfiguration zum Festlegen von „start-failure-is-fatal“ nicht mehr gültig, und für die Einstellung muss der Standardwert `true` wiederhergestellt werden. Darüber hinaus muss die Verfügbarkeitsgruppenressource aktualisiert werden, um die Eigenschaft `failover-timeout` einzubeziehen. 
>
>So aktualisieren Sie den Eigenschaftswert auf ein Ausführungsintervall von `true`:
>
>```bash
>sudo pcs property set start-failure-is-fatal=true
>```
>
>Aktualisieren Sie die Eigenschaft `failure-timeout` Ihrer vorhandenen Verfügbarkeitsgruppenressource auf ein Ausführungsintervall von `60s` (ersetzen Sie hierzu `ag1` durch den Namen Ihrer Verfügbarkeitsgruppenressource):
>
>```bash
>pcs resource update ag1 meta failure-timeout=60s
>```

## <a name="install-sql-server-resource-agent-for-integration-with-pacemaker"></a>Installieren des SQL Server-Ressourcen-Agent für die Integration in Pacemaker

Führen Sie die folgenden Befehle auf allen Knoten aus. 

```bash
sudo apt-get install mssql-server-ha
```

## <a name="create-a-sql-server-login-for-pacemaker"></a>Erstellen einer SQL Server-Anmeldung für Pacemaker

[!INCLUDE [SLES-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="create-availability-group-resource"></a>Erstellen von Verfügbarkeitsgruppenressourcen

Verwenden Sie den Befehl `pcs resource create`, und legen Sie die Ressourceneigenschaften fest, um die Verfügbarkeitsgruppenressource zu erstellen. Der folgende Befehl erstellt eine `ocf:mssql:ag`-Ressource vom Typ Master/untergeordnet für die Verfügbarkeitsgruppe mit dem Namen `ag1`. 

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=30s master meta notify=true

```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

## <a name="create-virtual-ip-resource"></a>Erstellen einer virtuellen IP-Ressource

Führen Sie den folgenden Befehl auf einem Knoten aus, um die virtuelle IP-Adressressource zu erstellen. Verwenden Sie eine verfügbare statische IP-Adresse aus dem Netzwerk. Bevor Sie das Skript ausführen, ersetzen Sie die Werte zwischen `< ... >` durch eine gültige IP-Adresse.

```bash
sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=<10.128.16.240>
```

In Pacemaker ist der Name eines virtuellen Servers nicht gleichwertig. Wenn Sie eine Verbindungszeichenfolge verwenden möchten, die auf einen Zeichenfolgen-Servernamen verweist, und nicht die IP-Adresse verwenden, registrieren Sie die IP-Ressourcenadresse und den gewünschten Namen des virtuellen Servers in DNS. Registrieren Sie für Notfallwiederherstellungskonfigurationen den gewünschten Namen des virtuellen Servers und die IP-Adresse bei den DNS-Servern am primären Standort und dem Standort für die Notfallwiederherstellung.

## <a name="add-colocation-constraint"></a>Hinzufügen einer Kollokationseinschränkung

Fast jede Entscheidung in einem Pacemaker-Cluster, wie etwa die Auswahl des Orts, an dem eine Ressource ausgeführt werden soll, erfolgt durch Vergleichen von Bewertungen. Dabei werden Bewertungen pro Ressource berechnet, und der Cluster Resource Manager wählt den Knoten mit der höchsten Bewertung für eine bestimmte Ressource aus. (Wenn ein Knoten eine negative Bewertung für eine Ressource aufweist, kann die Ressource auf diesem Knoten nicht ausgeführt werden.) Verwenden Sie Einschränkungen, um die Entscheidungen des Clusters zu konfigurieren. Einschränkungen weisen eine Bewertung auf. Wenn eine Einschränkung eine Bewertung unter INFINITY aufweist, ist dies lediglich eine Empfehlung. Die Bewertung INFINITY steht für eine obligatorische Einschränkung. Um sicherzustellen, dass das primäre Replikat und die virtuelle IP-Ressource sich auf demselben Host befinden, definieren Sie eine Kollokationseinschränkung mit der Bewertung INFINITY. Führen Sie den folgenden Befehl auf einem Knoten aus, um eine Kollokationseinschränkung hinzuzufügen. 

```bash
sudo pcs constraint colocation add virtualip ag_cluster-master INFINITY with-rsc-role=Master
```

## <a name="add-ordering-constraint"></a>Hinzufügen einer Sortierungseinschränkung

Die Kollokationseinschränkung weist eine implizite Sortierungseinschränkung auf. Damit wird die virtuelle IP-Adressressource verschoben, bevor diese die Verfügbarkeitsgruppenressource verschiebt. Die Ereignisse laufen standardmäßig wie folgt ab:

1. Der Benutzer gibt den Befehl `pcs resource move` aus, um die primäre Verfügbarkeitsgruppe von node1 zu node2 zu verschieben.
1. Die virtuelle IP-Ressource wird auf node1 angehalten.
1. Die virtuelle IP-Ressource wird auf node2 gestartet.

   >[!NOTE]
   >Zu diesem Zeitpunkt verweist die IP-Adresse vorübergehend auf node2, während node2 noch ein sekundäres Replikat vor dem Failover darstellt. 
   
1. Die primäre Verfügbarkeitsgruppe auf node1 wird auf sekundär herabgestuft.
1. Die sekundäre Verfügbarkeitsgruppe auf node2 wird auf primär heraufgestuft. 

Fügen Sie eine Sortierungseinschränkung hinzu, um zu vermeiden, dass die IP-Adresse vorübergehend auf den Knoten mit dem sekundären Replikat vor dem Failover verweist. 

Führen Sie den folgenden Befehl auf einem Knoten aus, um eine Sortierungseinschränkung hinzuzufügen:

```bash
sudo pcs constraint order promote ag_cluster-master then start virtualip
```

>[!IMPORTANT]
>Nachdem Sie den Cluster konfiguriert und die Verfügbarkeitsgruppe als Clusterressource hinzugefügt haben, können Sie ein Failover der Verfügbarkeitsgruppenressourcen nicht mehr mit Transact-SQL durchführen. SQL Server-Clusterressourcen unter Linux sind nicht so eng mit dem Betriebssystem gekoppelt wie in einem Windows Server-Failovercluster (WSFC). Der SQL Server-Dienst kann das Vorhandensein des Clusters nicht erkennen. Die gesamte Orchestrierung erfolgt über die Clusterverwaltungstools. Verwenden Sie in RHEL oder Ubuntu `pcs`. 

<!---[!INCLUDE [Pacemaker Concepts](..\includes\ss-linux-cluster-pacemaker-concepts.md)]--->

## <a name="next-steps"></a>Nächste Schritte

[Arbeiten mit einer Verfügbarkeitsgruppe mit Hochverfügbarkeit](sql-server-linux-availability-group-failover-ha.md)

