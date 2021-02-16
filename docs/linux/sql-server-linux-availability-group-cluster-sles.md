---
title: 'SUSE: Konfigurieren von Verfügbarkeitsgruppen für SQL Server für Linux'
titleSuffix: SQL Server
description: Hier erfahren Sie, wie Sie Verfügbarkeitsgruppencluster für SQL Server unter SUSE Linux Enterprise Server (SLES) erstellen können.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 04/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 85180155-6726-4f42-ba57-200bf1e15f4d
ms.openlocfilehash: 232235e1e7b8718b83c4826b4a019cb6c52e437c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338410"
---
# <a name="configure-sles-cluster-for-sql-server-availability-group"></a>Konfigurieren von SLES-Clustern für eine SQL Server-Verfügbarkeitsgruppe

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Diese Anleitung enthält Anweisungen zum Erstellen eines Clusters mit drei Knoten für SQL Server unter SuSE Linux Enterprise Server (SLES) 12 SP2. Zur Gewährleistung von Hochverfügbarkeit benötigt eine Verfügbarkeitsgruppe unter Linux drei Knoten. Weitere Informationen hierzu finden Sie unter [Hochverfügbarkeit und Schutz von Daten für Verfügbarkeitsgruppenkonfigurationen](sql-server-linux-availability-group-ha.md). Die Clusteringebene basiert auf der SUSE [High Availability Extension (HAE)](https://www.suse.com/products/highavailability), die auf [Pacemaker](https://clusterlabs.org/) aufbaut. 

Weitere Informationen zu Clusterkonfiguration, Ressourcen-Agent-Optionen, Verwaltung, Best Practices und Empfehlungen finden Sie unter [SUSE Linux Enterprise High Availability Extension 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html).

>[!NOTE]
>An diesem Punkt ist die Integration von SQL Server in Pacemaker unter Linux nicht so stark gekoppelt wie in WSFC unter Windows. SQL Server-Dienst unter Linux ist nicht clusterfähig. Pacemaker steuert die gesamte Orchestrierung der Clusterressourcen, darunter auch die Verfügbarkeitsgruppenressource. Unter Linux sollten Sie sich nicht auf dynamische Verwaltungssichten (Dynamic Management Views, DMVs) von Always On-Verfügbarkeitsgruppen verlassen, die Clusterinformationen wie sys.dm_hadr_cluster bereitstellen. Ferner gilt der virtuelle Netzwerkname für WSFC. Dazu gibt es in Pacemaker keine Entsprechung. Sie können weiterhin einen Listener erstellen, um ihn nach einem Failover für eine transparente erneute Verbindung zu verwenden. Sie müssen jedoch den Listenernamen im DNS-Server manuell mit der IP-Adresse registrieren, die zum Erstellen der virtuellen IP-Ressource verwendet wurde. (Eine Erläuterung hierzu finden Sie in den folgenden Abschnitten.)

[!INCLUDE [bias-sensitive-term-t](../includes/bias-sensitive-term-t.md)]

## <a name="roadmap"></a>Roadmap

Beim Erstellen einer Verfügbarkeitsgruppe zur Gewährleistung von Hochverfügbarkeit müssen Sie bei Linux-Servern anders vorgehen als bei einem Windows Server-Failovercluster. Die allgemeinen Schritte werden in der folgenden Liste beschrieben: 

1. [Konfigurieren Sie SQL Server in den Clusterknoten](sql-server-linux-setup.md).

2. [Erstellen Sie die Verfügbarkeitsgruppe](sql-server-linux-availability-group-failover-ha.md). 

3. Konfigurieren Sie einen Cluster Resource Manager wie Pacemaker. Diese Anweisungen finden Sie in diesem Dokument.
   
   Die Art und Weise des Konfigurierens eines Clusterressourcen-Managers hängt von der jeweiligen Linux-Distribution ab. 

   >[!IMPORTANT]
   >In Produktionsumgebungen wird zur Gewährleistung vonHochverfügbarkeit ein Fencing-Agent wie STONITH benötigt. In den Beispielen in diesem Artikel werden keine Fencing-Agents verwendet. Sie werden lediglich für Tests und Überprüfungen verwendet. 
   
   >In einem Pacemaker-Cluster wird Fencing verwendet, um den Cluster in einen bekannten Zustand zurückzusetzen. Wie das Fencing konfiguriert wird, hängt von der Verteilung und der Umgebung ab. Zurzeit ist das Fencing nicht in allen Cloudumgebungen verfügbar. Informationen hierzu finden Sie unter [SUSE Linux Enterprise High Availability Extension](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.fencing).

5. [Fügen Sie die Verfügbarkeitsgruppe als Ressource im Cluster hinzu](sql-server-linux-availability-group-cluster-sles.md#configure-the-cluster-resources-for-sql-server). 

## <a name="prerequisites"></a>Voraussetzungen

Für das folgende End-to-End-Szenario benötigen Sie drei Computer, um den Cluster mit drei Knoten bereitzustellen. In den folgenden Schritten wird beschrieben, wie Sie diese Server konfigurieren.

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>Einrichten und Konfigurieren des Betriebssystems auf den einzelnen Clusterknoten 

Der erste Schritt besteht darin, das Betriebssystem auf den Clusterknoten zu konfigurieren. Verwenden Sie für diese exemplarische Vorgehensweise SLES 12 SP2 mit einem gültigen Abonnement für das Hochverfügbarkeits-Add-On.

### <a name="install-and-configure-sql-server-service-on-each-cluster-node"></a>Installieren und Konfigurieren des SQL Server-Diensts auf den einzelnen Clusterknoten

1. Installieren Sie den SQL Server-Dienst auf allen Knoten, und richten Sie ihn ein. Ausführliche Anweisungen finden Sie unter [Installieren von SQL Server für Linux](sql-server-linux-setup.md).

1. Legen Sie einen Knoten als primäres Replikat und andere Knoten als sekundäre Replikate fest. Verwenden Sie diese Begriffe in der gesamten Anleitung.

1. Stellen Sie sicher, dass Knoten, die Teil des Clusters sein werden, miteinander kommunizieren können.

   Das folgende Beispiel zeigt `/etc/hosts` mit Ergänzungen für drei Knoten mit den Namen SLES1, SLES2 und SLES3.

   ```
   127.0.0.1   localhost
   10.128.16.33 SLES1
   10.128.16.77 SLES2
   10.128.16.22 SLES3
   ```

   Alle Clusterknoten müssen in der Lage sein, über SSH aufeinander zuzugreifen. Tools wie `hb_report`oder `crm_report` (zur Problembehandlung) und Hawk's History Explorer erfordern kennwortlosen SSH-Zugriff zwischen den Knoten, andernfalls können sie nur Daten vom aktuellen Knoten sammeln. Falls Sie keinen nicht-standardmäßigen SSH-Port verwenden, nutzen Sie die Option „-X“ (siehe Seite `man`). Wenn Ihr SSH-Port z. B. 3479 ist, rufen Sie `crm_report` mit Folgendem auf:

   ```bash
   sudo crm_report -X "-p 3479" [...]
   ```

   Weitere Informationen finden Sie im [SLES Administration Guide (SLES-Administratorhandbuch) im Abschnitt „Miscellaneous“ (Sonstiges)](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.misc).


## <a name="create-a-sql-server-login-for-pacemaker"></a>Erstellen einer SQL Server-Anmeldung für Pacemaker

[!INCLUDE [SLES-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="configure-an-always-on-availability-group"></a>Konfigurieren einer Always On-Verfügbarkeitsgruppe

Konfigurieren auf Linux-Servern zunächst die Verfügbarkeitsgruppe und anschließend die Clusterressourcen. Informationen zum Konfigurieren der Verfügbarkeitsgruppe finden Sie unter [Konfigurieren von Always On-Verfügbarkeitsgruppen für SQL Server für Linux](sql-server-linux-availability-group-configure-ha.md)

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installieren und Konfigurieren von Pacemaker auf jedem Clusterknoten

1. Installieren der High Availability Extension

   Informationen hierzu finden Sie unter [Installing SUSE Linux Enterprise Server and High Availability Extension (Installieren von SUSE Linux Enterprise Server und High Availability Extension)](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.installation)

1. Installieren Sie das SQL Server-Ressourcen-Agent-Paket auf beiden Knoten.

   ```bash
   sudo zypper install mssql-server-ha
   ```

## <a name="set-up-the-first-node"></a>Einrichten des ersten Knotens

   Informationen hierzu finden Sie unter [SLES installation instructions (SLES-Installationsanweisungen)](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.1st-node).

1. Melden Sie sich bei dem physischen oder virtuellen Computer, den Sie als Clusterknoten verwenden möchten, als `root` an.
2. Starten Sie das Bootstrapskript, indem Sie folgenden Befehl ausführen:
   ```bash
   sudo ha-cluster-init
   ```

   Wenn NTP nicht so konfiguriert wurde, das es zur Startzeit startet, wird eine Meldung angezeigt. 

   Wenn Sie den Vorgang dennoch fortsetzen möchten, werden vom Skript automatisch Schlüssel für den SSH-Zugriff und für das Synchronisierungstool Csync2 erstellt, und die dafür erforderlichen Dienste werden gestartet. 

3. So konfigurieren Sie die Clusterkommunikationsebene (Corosync): 

   a. Geben Sie eine Netzwerkadresse für die Bindung ein. Standardmäßig schlägt das Skript die Netzwerkadresse von eth0 vor. Geben Sie alternativ eine andere Netzwerkadresse ein, z. B. die Adresse von bond0. 

   b. Geben Sie eine Multicastadresse ein. Das Skript schlägt eine zufällige Adresse vor, die Sie als Standard verwenden können. 

   c. Geben Sie einen Multicastport ein. Das Skript schlägt 5405 als Standardwert vor. 

   d. Geben Sie zum Konfigurieren von `SBD ()` einen permanenten Pfad zur Partition Ihres Blockgeräts ein, das Sie für die SBD verwenden möchten. Der Pfad muss auf allen Knoten im Cluster einheitlich sein. 
   Schließlich startet das Skript den Dienst Pacemaker, um den Cluster mit einem Knoten online zu schalten und die Webverwaltungsschnittstelle Hawk2 zu aktivieren. Die URL, die für Hawk2 verwendet werden soll, wird auf dem Bildschirm angezeigt. 

4. Weitere Informationen zum Einrichtungsvorgang finden Sie unter `/var/log/sleha-bootstrap.log`. Der Cluster mit einem Knoten wird nun ausgeführt. Überprüfen Sie den Status des Clusters mit „crm status“.

   ```bash
   sudo crm status
   ```

   Die Clusterkonfiguration können Sie auch mit `crm configure show xml` oder `crm configure show` anzeigen.

5. Mit der Bootstrapprozedur wird ein Linux-Benutzer mit dem Namen „hacluster“ und dem Kennwort „linux“ erstellt. Ersetzen Sie das Standardkennwort so bald wie möglich durch ein sicheres Kennwort: 

   ```bash
   sudo passwd hacluster
   ```

## <a name="add-nodes-to-the-existing-cluster"></a>Hinzufügen von Knoten zum vorhandenen Cluster

Wenn Sie über einen Cluster verfügen, der mit einem oder mehreren Knoten ausgeführt wird, fügen Sie mit dem Bootstrapskript „ha-cluster-join“ weitere Clusterknoten hinzu. Das Skript benötigt lediglich Zugriff auf einen vorhandenen Clusterknoten und nimmt die grundlegende Einrichtung auf dem aktuellen Computer automatisch vor. Führen Sie die folgenden Schritte durch:

Wenn Sie die vorhandenen Clusterknoten mit dem Clustermodul `YaST` konfiguriert haben, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie `ha-cluster-join` ausführen:
- Der Root-Benutzer auf den vorhandenen Knoten verfügt über SSH-Schlüssel für die Anmeldung ohne Kennwort. 
- `Csync2` ist auf den vorhandenen Knoten konfiguriert. Weitere Informationen hierzu finden Sie unter „Configuring Csync2 with YaST“ (Konfigurieren von Csync2 mit YaST). 

1. Melden Sie sich bei dem physischen oder virtuellen Computer, der dem Cluster beitreten soll, als Root-Benutzer an. 
2. Starten Sie das Bootstrapskript, indem Sie folgenden Befehl ausführen: 

   ```bash
   sudo ha-cluster-join
   ```

   Wenn NTP nicht so konfiguriert wurde, das es zur Startzeit startet, wird eine Meldung angezeigt. 

3. Wenn Sie den Vorgang dennoch fortsetzen möchten, werden Sie aufgefordert, die IP-Adresse eines vorhandenen Knotens einzugeben. Geben Sie die IP-Adresse ein. 

4. Wenn Sie nicht bereits einen SSH-Zugriff ohne Kennwort zwischen den beiden Computern konfiguriert haben, werden Sie zudem aufgefordert, das Stammkennwort des vorhandenen Knotens einzugeben. 

   Nach der Anmeldung beim angegebenen Knoten kopiert das Skript die Corosync-Konfiguration, konfiguriert SSH und `Csync2` und schaltet den aktuellen Computer als neuen Clusterknoten online. Zudem startet es den für Hawk benötigten Dienst. Wenn Sie den freigegebenen Speicher mit `OCFS2` konfiguriert haben, wird automatisch das Bereitstellungspunktverzeichnis für das Dateisystem `OCFS2` erstellt. 

5. Wiederholen Sie die vorherigen Schritte für alle Computer, die Sie dem Cluster hinzufügen möchten. 

6. Weitere Informationen zu diesem Vorgang finden Sie unter `/var/log/ha-cluster-bootstrap.log`. 

1. Überprüfen Sie den Status des Clusters mit `sudo crm status`. Wenn Sie einen zweiten Knoten erfolgreich hinzugefügt haben, sieht die Ausgabe ähnlich der folgenden aus:

   ```bash
   sudo crm status
   
   3 nodes configured
   1 resource configured
   Online: [ SLES1 SLES2 SLES3]
   Full list of resources:   
   admin_addr     (ocf::heartbeat:IPaddr2):       Started node1
   ```

   >[!NOTE]
   >`admin_addr` ist die virtuelle IP-Clusterressource, die während der anfänglichen Einrichtung des Clusters mit einem Knoten konfiguriert wird.

Nachdem Sie alle Knoten hinzugefügt haben, überprüfen Sie, ob Sie die Option „no-quorum-policy“ in den globalen Clusteroptionen anpassen müssen. Dies ist besonders wichtig für Cluster mit zwei Knoten. Weitere Informationen finden Sie im Abschnitt 4.1.2 „Option no-quorum-policy“ (Option „no-quorum-policy“). 

## <a name="set-cluster-property-cluster-recheck-interval"></a>Festlegen der Clustereigenschaft „cluster-recheck-interval“

`cluster-recheck-interval` gibt das Abrufintervall an, mit dem der Cluster prüft, ob Änderungen in den Ressourcenparametern, Einschränkungen oder anderen Clusteroptionen vorliegen. Wenn ein Replikat ausfällt, versucht der Cluster, das Replikat in einem Intervall neu zu starten, das durch den Wert `failure-timeout` und den Wert `cluster-recheck-interval` gebunden ist. Wenn `failure-timeout` beispielsweise auf 60 Sekunden und `cluster-recheck-interval` auf 120 Sekunden festgelegt ist, wird der Neustart in einem Intervall versucht, das größer als 60 Sekunden, aber kleiner als 120 Sekunden ist. Es wird empfohlen, „failure-timeout“ auf 60 Sekunden und „cluster-recheck-interval“ auf einen Wert größer als 60 Sekunden festzulegen. Es wird nicht empfohlen, „cluster-recheck-interval“ auf einen kleinen Wert festzulegen.

So aktualisieren Sie den Eigenschaftswert auf ein Ausführungsintervall von `2 minutes`:

```bash
crm configure property cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> Wenn Sie bereits über eine Verfügbarkeitsgruppenressource verfügen, die von einem Pacemaker-Cluster verwaltet wird, wird mit allen Verteilungen, für die das aktuellste verfügbare Pacemaker-Pakte 1.1.18-11.el7 verwendet wird, ein Behavior Change für die Clustereinstellung „start-failure-is-fatal“ für den Fall eingeführt, dass deren Wert auf FALSE festgelegt ist. Diese Änderung wirkt sich auf den Failoverworkflow aus. Wenn ein primäres Replikat ausfällt, wird für den Cluster ein Failover auf eines der verfügbaren sekundären Replikate erwartet. Stattdessen werden die Benutzer bemerken, dass der Cluster weiterhin versucht, das ausgefallene primäre Replikat zu starten. Wenn dieses primäre Replikat (aufgrund eines dauerhaften Ausfalls) nicht online geschaltet wird, führt der Cluster kein Failover zu einem anderen verfügbaren sekundären Replikat durch. Aufgrund dieser Änderung ist eine zuvor empfohlene Konfiguration zum Festlegen von „start-failure-is-fatal“ nicht mehr gültig, und für die Einstellung muss der Standardwert `true` wiederhergestellt werden. Darüber hinaus muss die Verfügbarkeitsgruppenressource aktualisiert werden, um die Eigenschaft `failover-timeout` einzubeziehen. 
>
>So aktualisieren Sie den Eigenschaftswert auf ein Ausführungsintervall von `true`:
>
>```bash
>crm configure property start-failure-is-fatal=true
>```
>
>Aktualisieren Sie die Eigenschaft `failure-timeout` Ihrer vorhandenen Verfügbarkeitsgruppenressource auf ein Ausführungsintervall von `60s` (ersetzen Sie hierzu `ag1` durch den Namen Ihrer Verfügbarkeitsgruppenressource): 
>
>```bash
>crm configure edit ag1
># In the text editor, add `meta failure-timeout=60s` after any `param`s and before any `op`s
>```

Weitere Informationen zu Pacemaker-Clustereigenschaften finden Sie unter [Configuring Cluster Resources (Konfigurieren von Clusterressourcen)](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_config_crm_resources.html).

## <a name="configure-fencing-stonith"></a>Konfigurieren des Fencing (STONITH)
Bei Anbietern von Pacemaker-Clustern muss für eine unterstützte Clustereinrichtung STONITH aktiviert und ein Fencinggerät konfiguriert sein. Wenn der Cluster Resource Manager den Status eines Knotens oder einer Ressource auf einem Knoten nicht ermitteln kann, wird das Fencing verwendet, um den Cluster wieder in einen bekannten Status zu versetzen.

Durch das Fencing auf Ressourcenebene wird vor allem sichergestellt, dass während eines Ausfalls durch die Konfiguration einer Ressource keine Daten beschädigt werden. Das Fencing auf Ressourcenebene können Sie beispielsweise mit DRBD (Distributed Replicated Block Device) verwenden, damit die Festplatte auf einem Knoten bei einem Ausfall der Kommunikationsverbindung als veraltet markiert wird.

Mit dem Fencing auf Knotenebene wird sichergestellt, dass ein Knoten keine Ressourcen ausführt. Dies erfolgt durch Zurücksetzen des Knotens. Die zugehörige Pacemaker-Implementierung wird als STONITH bezeichnet (was für „Shoot the Other Node in the Head“ (Schieß dem anderen Knoten in den Kopf) steht). Pacemaker unterstützt eine Vielzahl von Fencinggeräten, z. B. eine unterbrechungsfreie Stromversorgung oder Verwaltungsschnittstellenkarten für Server.

Weitere Informationen finden Sie unter

- [Grundlegendes zu Pacemaker-Clustern](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/)
- [Fencing und STONITH](https://clusterlabs.org/doc/crm_fencing.html)
- [SUSE-Dokumentation zu Hochverfügbarkeit: Fencing und STONITH](https://www.suse.com/documentation/sle_ha/book_sleha/data/cha_ha_fencing.html)

Zum Zeitpunkt der Clusterinitialisierung ist STONITH deaktiviert, wenn keine Konfiguration erkannt wird. STONITH kann später durch Ausführen des folgenden Befehls aktiviert werden:

```bash
sudo crm configure property stonith-enabled=true
```
  
>[!IMPORTANT]
>STONITH wird lediglich zu Testzwecken deaktiviert. Wenn Sie Pacemaker in einer Produktionsumgebung verwenden möchten, sollten Sie je nach Ihrer Umgebung eine STONITH-Implementierung planen und immer aktivieren. SUSE bietet keine Fencing-Agents für Cloudumgebungen (wie Azure) oder Hyper-V. Folglich bietet der Clusteranbieter keine Unterstützung für die Ausführung von Produktionsclustern in diesen Umgebungen. Wir arbeiten an einer in zukünftigen Versionen verfügbaren Lösung zum Schließen dieser Lücke.

## <a name="configure-the-cluster-resources-for-sql-server"></a>Konfigurieren der Clusterressourcen für SQL Server

Informationen finden Sie im [SLES-Administratorhandbuch](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.manual_config).

## <a name="enable-pacemaker"></a>Aktivieren von Pacemaker

Aktivieren Sie Pacemaker so, dass die Software automatisch gestartet wird.

Führen Sie den folgenden Befehl auf allen Knoten im Cluster aus.

```bash
systemctl enable pacemaker
```

### <a name="create-availability-group-resource"></a>Erstellen von Verfügbarkeitsgruppenressourcen

Mit dem folgenden Befehl wird die Verfügbarkeitsgruppenressource für drei Replikate der Verfügbarkeitsgruppe [ag1] erstellt und konfiguriert. Aufgrund der Tatsache, dass Zeitlimits stark von der Arbeitsauslastung abhängen und für jede Bereitstellung sorgfältig angepasst werden müssen, müssen die Überwachungsvorgänge und -zeitlimits in SLES explizit angegeben werden.
Führen Sie den Befehl auf einem Knoten im Cluster aus:

1. Führen `crm configure` Sie aus, um die CRM-Eingabeaufforderung zu öffnen:

   ```bash
   sudo crm configure 
   ```

1. Führen Sie in der CRM-Eingabeaufforderung den folgenden Befehl aus, um die Ressourceneigenschaften zu konfigurieren.

   ```bash
   primitive ag_cluster \
      ocf:mssql:ag \
      params ag_name="ag1" \
      meta failure-timeout=60s \
      op start timeout=60s \
      op stop timeout=60s \
      op promote timeout=60s \
      op demote timeout=10s \
      op monitor timeout=60s interval=10s \
      op monitor timeout=60s interval=11s role="Master" \
      op monitor timeout=60s interval=12s role="Slave" \
      op notify timeout=60s
   ms ms-ag_cluster ag_cluster \
      meta master-max="1" master-node-max="1" clone-max="3" \
     clone-node-max="1" notify="true" \
   commit
      ```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

### <a name="create-virtual-ip-resource"></a>Erstellen einer virtuellen IP-Ressource

Wenn Sie beim Ausführen von `ha-cluster-init` die virtuelle IP-Ressource nicht erstellt haben, können Sie diese Ressource jetzt erstellen. Mit dem folgenden Befehl wird eine virtuelle IP-Ressource erstellt. Ersetzen Sie `<**0.0.0.0**>` durch eine verfügbare Adresse aus Ihrem Netzwerk und `<**24**>` durch die Anzahl der Bits in der CIDR-Subnetzmaske. Führen Sie den Befehl auf einem Knoten aus.

```bash
crm configure \
primitive admin_addr \
   ocf:heartbeat:IPaddr2 \
   params ip=<**0.0.0.0**> \
      cidr_netmask=<**24**>
```

### <a name="add-colocation-constraint"></a>Hinzufügen einer Kollokationseinschränkung
Fast jede Entscheidung in einem Pacemaker-Cluster, wie etwa die Auswahl des Orts, an dem eine Ressource ausgeführt werden soll, erfolgt durch Vergleichen von Bewertungen. Dabei werden Bewertungen pro Ressource berechnet, und der Cluster Resource Manager wählt den Knoten mit der höchsten Bewertung für eine bestimmte Ressource aus. (Wenn ein Knoten eine negative Bewertung für eine Ressource aufweist, kann die Ressource auf diesem Knoten nicht ausgeführt werden.) Wir können die Entscheidungen des Clusters mit Einschränkungen beeinflussen. Einschränkungen weisen eine Bewertung auf. Wenn eine Einschränkung eine Bewertung unter INFINITY aufweist, ist dies lediglich eine Empfehlung. Die Bewertung INFINITY steht für eine obligatorische Einschränkung. Wir möchten sicherstellen, dass das primäre Replikat der Verfügbarkeitsgruppe und die virtuelle IP-Ressource auf demselben Host ausgeführt werden. Daher definieren wir eine Kollokationseinschränkung mit der Bewertung INFINITY. 

Führen Sie den folgenden Befehl auf einem Knoten aus, um die Kollokationseinschränkung für die virtuelle IP-Adresse festzulegen, sodass diese auf demselben Knoten wie der Master ausgeführt wird:

```bash
crm configure
colocation vip_on_master inf: \
    admin_addr ms-ag_cluster:Master
commit
```

### <a name="add-ordering-constraint"></a>Hinzufügen einer Sortierungseinschränkung
Die Kollokationseinschränkung weist eine implizite Sortierungseinschränkung auf. Damit wird die virtuelle IP-Adressressource verschoben, bevor diese die Verfügbarkeitsgruppenressource verschiebt. Die Ereignisse laufen standardmäßig wie folgt ab: 

1. Der Benutzer gibt an den Verfügbarkeitsgruppenmaster eine Ressourcenmigration von „node1“ zu „node2“ aus.
2. Die virtuelle IP-Ressource wird auf Knoten 1 angehalten.
3. Die virtuelle IP-Ressource wird auf Knoten 2 gestartet. Zu diesem Zeitpunkt verweist die IP-Adresse vorübergehend auf Knoten 2, während Knoten 2 noch ein sekundäres Replikat vor dem Failover darstellt. 
4. Der Verfügbarkeitsgruppen-Master auf Knoten 1 wird herabgestuft.
5. Die Verfügbarkeitsgruppe auf Knoten 2 wird zum Master heraufgestuft. 

Fügen Sie eine Sortierungseinschränkung hinzu, um zu vermeiden, dass die IP-Adresse vorübergehend auf den Knoten mit dem sekundären Replikat vor dem Failover verweist. Führen Sie den folgenden Befehl auf einem Knoten aus, um eine Sortierungseinschränkung hinzuzufügen: 

```bash
sudo crm configure \
   order ag_first inf: ms-ag_cluster:promote admin_addr:start
```


>[!IMPORTANT]
>Nachdem Sie den Cluster konfiguriert und die Verfügbarkeitsgruppe als Clusterressource hinzugefügt haben, können Sie ein Failover der Verfügbarkeitsgruppenressourcen nicht mehr mit Transact-SQL durchführen. SQL Server-Clusterressourcen unter Linux sind nicht so eng mit dem Betriebssystem gekoppelt wie in einem Windows Server-Failovercluster (WSFC). Der SQL Server-Dienst kann das Vorhandensein des Clusters nicht erkennen. Die gesamte Orchestrierung erfolgt über die Clusterverwaltungstools. Verwenden Sie in SLES `crm`. 

Führen Sie für die Verfügbarkeitsgruppe ein Failover mit `crm` aus. Initiieren Sie kein Failover mit Transact-SQL. Weitere Informationen finden Sie unter [Failover](sql-server-linux-availability-group-failover-ha.md#failover).


Weitere Informationen finden Sie unter
- [Managing cluster resources (Verwalten von Clusterressourcen)](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.config.crm).   
- [HA Concepts (Hochverfügbarkeitskonzepte)](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.concepts)
- [Pacemaker Quick Reference (Kurzübersicht über Pacemaker)](https://github.com/ClusterLabs/pacemaker/blob/master/doc/pcs-crmsh-quick-ref.md) 

<!---[!INCLUDE [Pacemaker Concepts](..\includes\ss-linux-cluster-pacemaker-concepts.md)]--->

## <a name="next-steps"></a>Nächste Schritte

[Arbeiten mit einer Verfügbarkeitsgruppe mit Hochverfügbarkeit](sql-server-linux-availability-group-failover-ha.md)
