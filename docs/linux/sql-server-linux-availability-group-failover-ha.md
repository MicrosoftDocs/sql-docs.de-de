---
title: 'Verwalten des Failovers einer Verfügbarkeitsgruppe: SQL Server für Linux'
description: 'In diesem Artikel werden Failoverarten erläutert: automatisches, geplantes manuelles und erzwungenes manuelles Failover. Beim automatischen und beim geplanten manuellen Failover bleiben alle Daten erhalten.'
author: tejasaks
ms.author: tejasaks
ms.reviewer: vanto
ms.date: 03/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 863278eacebc4b405a4a44e72c4318e950d0cc1d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837669"
---
# <a name="always-on-availability-group-failover-on-linux"></a>Failover der Always On-Verfügbarkeitsgruppe unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Im Kontext einer Verfügbarkeitsgruppe können die primäre und die sekundäre Rolle von Verfügbarkeitsreplikaten normalerweise im Rahmen des sogenannten Failovers ausgetauscht werden. Failover können in drei Formen auftreten: automatisches Failover (ohne Datenverlust), geplantes manuelles Failover (ohne Datenverlust) und erzwungenes manuelles Failover (mit möglichem Datenverlust), welches in der Regel *erzwungenes Failover* genannt wird. Bei automatischen und geplanten manuellen Failover bleiben alle Daten erhalten. Eine Verfügbarkeitsgruppe führt ein Failover auf der Ebene des Verfügbarkeitsreplikats aus. Das heißt, eine Verfügbarkeitsgruppe führt ein Failover auf eines ihrer sekundären Replikate (das aktuelle Failoverziel) aus. 

Hintergrundinformationen zum Failover finden Sie unter [Failover und Failovermodi (Always On-Verfügbarkeitsgruppen)](../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).

## <a name="manual-failover"></a><a name="failover"></a>Manuelles Failover

Verwenden Sie die Clusterverwaltungstools für ein Failover einer Verfügbarkeitsgruppe, die von einem externen Cluster-Manager verwaltet wird. Wenn z. B. eine Lösung einen Linux-Cluster mithilfe von Pacemaker verwaltet, verwenden Sie `pcs`, um manuelle Failover auf RHEL oder Ubuntu durchzuführen. Auf dem SLES verwenden Sie `crm`. 

> [!IMPORTANT]
> Verwenden Sie bei normalen Vorgängen für Failover keine Transact-SQL- oder SQL Server-Verwaltungstools wie SSMS oder PowerShell. Falls `CLUSTER_TYPE = EXTERNAL`, ist `EXTERNAL` der einzige akzeptable Wert für `FAILOVER_MODE`. Mit diesen Einstellungen werden alle manuellen oder automatischen Failoveraktionen vom externen Cluster-Manager ausgeführt. Anweisungen zum Erzwingen eines Failovers mit möglichem Datenverlust finden Sie unter [Force failover (Erzwungenes Failover)](#forceFailover).

### <a name="a-namemanualfailovermanual-failover-steps"></a><a name="manualFailover">Schritte für ein manuelles Failover

Das sekundäre Replikat, das zum primären Replikat wird, muss für das Failover synchron sein. Wenn ein sekundäres Replikat asynchron ist, [ändern Sie den Verfügbarkeitsmodus](../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md).

Das manuelle Failover erfolgt in zwei Schritten.

   Führen Sie zunächst ein [manuelles Failover durch, indem Sie die Verfügbarkeitsgruppenressource vom Clusterknoten, der die Ressource besitzt, zu einem neuen Knoten verschieben](#manualMove).

   Der Cluster führt ein Failover der Verfügbarkeitsgruppenressource aus und fügt eine Speicherorteinschränkung hinzu. Durch diese Einschränkung wird die Ressource so konfiguriert, dass sie auf dem neuen Knoten ausgeführt wird. Entfernen Sie diese Einschränkung, um ein Failover in Zukunft erfolgreich durchführen zu können.

   [Entfernen Sie im nächsten Schritt die Speicherorteinschränkung](#removeLocConstraint).

#### <a name="step-1-manually-fail-over-by-moving-availability-group-resource"></a><a name="manualMove"></a> Schritt 1: Manuelles Failover durch Verschieben der Verfügbarkeitsgruppenressource

Führen Sie den entsprechenden Befehl für Ihre Verteilung aus, um ein Failover von einer Verfügbarkeitsgruppenressource namens *ag_cluster* auf den Clusterknoten namens *nodeName2* durchzuführen:

- **RHEL-/Ubuntu-Beispiel**

   ```bash
   sudo pcs resource move ag_cluster-master nodeName2 --master --lifetime=30S
   ```

- **SLES-Beispiel**

   ```bash
   crm resource migrate ag_cluster nodeName2 --lifetime=30S
   ```

>[!IMPORTANT]
>Wenn Sie die Option „--lifetime“ verwenden, ist die zum Verschieben der Ressource erstellte Speicherorteinschränkung nur temporär und gilt im vorherigen Beispiel 30 Sekunden.
>Beachten Sie, dass die temporäre Einschränkung nicht automatisch aufgehoben wird und möglicherweise in der Einschränkungsliste auftaucht, allerdings als abgelaufene Einschränkung. Abgelaufene Einschränkungen haben keinen Einfluss auf das Failoververhalten des Pacemaker-Clusters. Wenn Sie beim Verschieben der Ressource nicht die Option „--lifetime“ verwenden, sollten Sie eine Speicherorteinschränkung entfernen, die wie unten beschrieben automatisch hinzugefügt wird.

#### <a name="step-2-remove-the-location-constraint"></a><a name="removeLocConstraint"> </a> Schritt 2: Entfernen der Speicherorteinschränkung

Während eines manuellen Failovers fügt der `pcs`-Befehl `move` oder der `crm`-Befehl `migrate` eine Speicherorteinschränkung hinzu, damit die Ressource auf dem neuen Zielknoten platziert wird. Um die neue Einschränkung anzuzeigen, führen Sie den folgenden Befehl nach dem Verschieben der Ressource aus:

- **RHEL-/Ubuntu-Beispiel**

   ```bash
   sudo pcs constraint list --full
   ```

- **SLES-Beispiel**

   ```bash
   crm config show
   ```

Ein Beispiel für die Einschränkung, die aufgrund eines manuellen Failovers erstellt wird. 
 `Enabled on: Node1 (score:INFINITY) (role: Master) (id:cli-prefer-ag_cluster-master)`

   > [!NOTE]
   > Der Ressourcenname der Verfügbarkeitsgruppe in Pacemaker-Clustern in Red Hat Enterprise Linux 8.x und Ubuntu 18.04 ähnelt *ag_cluster-clone*, da sich die Benennung in Bezug auf Ressourcen weiterentwickelt hat, um einen *heraufstufbaren Klon* zu verwenden. 

- **RHEL-/Ubuntu-Beispiel**

   Im folgenden Befehl ist `cli-prefer-ag_cluster-master` die ID der Einschränkung, die entfernt werden muss. `sudo pcs constraint list --full` gibt diese ID zurück. 
   
   ```bash
   sudo pcs resource clear ag_cluster-master  
   ```
   oder
   
   ```bash
   sudo pcs constraint remove cli-prefer-ag_cluster-master  
   ```
  
   Alternativ können Sie die automatische Generierung von Einschränkungen in einer einzelnen Zeile wie folgt verschieben und löschen. Im folgenden Beispiel wird die Terminologie *Klon* gemäß Red Hat Enterprise Linux 8.x verwendet. 
  
   ```bash
   sudo pcs resource move ag_cluster-clone --master nodeName2 && sleep 30 && sudo pcs resource clear ag_cluster-clone

   ```
  
- **SLES-Beispiel**

   Im folgenden Befehl ist `cli-prefer-ms-ag_cluster` die ID der Einschränkung. `crm config show` gibt diese ID zurück. 
   
   ```bash
   crm configure
   delete cli-prefer-ms-ag_cluster 
   commit
   ```

>[!NOTE]
>Das automatische Failover fügt keine Speicherorteinschränkung hinzu, also ist keine Bereinigung notwendig. 

Weitere Informationen finden Sie unter:
- [Red Hat - Managing Cluster Resources (Red Hat – Verwalten von Clusterressourcen)](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Configuring_the_Red_Hat_High_Availability_Add-On_with_Pacemaker/ch-manageresource-HAAR.html)
- [Pacemaker - Move Resources Manually (Pacemaker – Manuelles Verschieben von Ressourcen)](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/_move_resources_manually.html)
 [SLES Administration Guide - Resources (SLES-Verwaltungsleitfaden – Ressourcen)](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.resource) 
 
## <a name="force-failover"></a><a name="forceFailover"></a> Erzwungenes Failover 

Ein erzwungenes Failover ist ausschließlich für die Notfallwiederherstellung gedacht. In diesem Fall können Sie kein Failover mit Clusterverwaltungstools durchführen, da das primäre Rechenzentrum ausgefallen ist. Wenn Sie ein Failover auf ein nicht synchronisiertes sekundäres Replikat erzwingen, ist Datenverlust möglich. Erzwingen Sie ein Failover nur, wenn Sie den Dienst für die Verfügbarkeitsgruppe sofort wiederherstellen müssen und bereit sind, das Risiko des Datenverlustes in Kauf zu nehmen.

Wenn Sie die Clusterverwaltungstools für die Interaktion mit dem Cluster nicht verwenden können, z. B. wenn der Cluster aufgrund eines Notfallereignisses im primären Rechenzentrum nicht erreichbar ist, müssen Sie das Failover möglicherweise erzwingen, um den externen Cluster-Manager zu umgehen. Dieses Verfahren wird für reguläre Vorgänge nicht empfohlen, da das Risiko des Datenverlusts besteht. Greifen Sie auf dieses Verfahren zurück, wenn die Clusterverwaltungstools das Failover nicht durchführen können. Funktionell ähnelt dieses Verfahren der [Durchführung eines erzwungenen manuellen Failovers](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md) einer Verfügbarkeitsgruppe unter Windows.
 
Dieser Prozess zum Erzwingen eines Failovers ist für SQL Server für Linux spezifisch.

1. Vergewissern Sie sich, dass die Verfügbarkeitsgruppenressource nicht mehr vom Cluster verwaltet wird. 

      - Legen Sie für die Ressource auf dem Zielclusterknoten den nicht verwalteten Modus fest. Dieser Befehl signalisiert dem Ressourcen-Agent, dass er die Ressourcenüberwachung und -verwaltung abbrechen soll. Beispiel: 
      
      ```bash
      sudo pcs resource unmanage <resourceName>
      ```

      - Wenn Sie den Modus der Ressource nicht auf „nicht verwaltet“ festlegen können, löschen Sie die Ressource. Beispiel:

      ```bash
      sudo pcs resource delete <resourceName>
      ```

      >[!NOTE]
      >Wenn Sie eine Ressource löschen, werden auch alle zugeordneten Einschränkungen gelöscht. 

1. Legen Sie auf der SQL Server-Instanz, von der das sekundäre Replikat gehostet wird, als Sitzungskontextvariable `external_cluster` fest.

   ```Transact-SQL
   EXEC sp_set_session_context @key = N'external_cluster', @value = N'yes';
   ```

1. Führen Sie mithilfe von Transact-SQL ein Failover der Verfügbarkeitsgruppe durch. Ersetzen Sie im folgenden Beispiel `<MyAg>` durch den Namen Ihrer Verfügbarkeitsgruppe. Stellen Sie eine Verbindung mit der SQL Server-Instanz her, die das sekundäre Zielreplikat hostet, und führen Sie den folgenden Befehl aus:

   ```Transact-SQL
   ALTER AVAILABILITY GROUP <MyAg> FORCE_FAILOVER_ALLOW_DATA_LOSS;
   ```

1.  Versetzen Sie die Verfügbarkeitsgruppe nach einem erzwungenen Failover in einen ordnungsgemäßen Zustand, bevor Sie entweder die Clusterressourcenüberwachung und -verwaltung neu starten oder die Verfügbarkeitsgruppenressource neu erstellen. Lesen Sie sich den Abschnitt [Wichtige Aufgaben nach einem erzwungenen Failover](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md#FollowUp) durch.

1.  Starten Sie die Clusterressourcenüberwachung und -verwaltung neu:

   Führen Sie den folgenden Befehl aus, um die Clusterressourcenüberwachung und -verwaltung neu zu starten:

   ```bash
   sudo pcs resource manage <resourceName>
   sudo pcs resource cleanup <resourceName>
   ```

   Falls Sie die Clusterressource gelöscht haben, erstellen Sie sie neu. Befolgen Sie die Anweisungen unter [Create availability group resource (Verfügbarkeitsgruppenressource erstellen)](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource), um die Clusterressource neu zu erstellen.

>[!Important]
>Verwenden Sie die vorhergehenden Schritte nicht für Übungen zur Notfallwiederherstellung, da dabei das Risiko des Datenverlusts besteht. Ändern Sie stattdessen das asynchrone Replikat in ein synchrones Replikat, und befolgen Sie die Anweisungen für ein [normales manuelles Failover](#manualFailover).

## <a name="database-level-monitoring-and-failover-trigger"></a>Überwachung auf Datenbankebene und Failovertrigger

Für `CLUSTER_TYPE=EXTERNAL` unterscheidet sich die Semantik des Failovertriggers im Vergleich zu WSCF. Wenn sich die Verfügbarkeitsgruppe auf einer SQL Server-Instanz in einem WSFC befindet, führt der Übergang aus dem `ONLINE`-Status für die Datenbank zu einem Fehler bei der Integrität der Verfügbarkeitsgruppe. Als Reaktion löst der Cluster-Manager eine Failoveraktion aus. Unter Linux kann die SQL Server-Instanz nicht mit dem Cluster kommunizieren. Die Überwachung der Datenbankintegrität erfolgt *indirekt*. Wenn der Benutzer die Failoverüberwachung und Failover auf Datenbankebene aktiviert hat (durch Festlegen der Option `DB_FAILOVER=ON` beim Erstellen der Verfügbarkeitsgruppe), überprüft der Cluster bei jeder Ausführung einer Überwachungsaktion, ob der Status der Datenbank `ONLINE` lautet. Der Cluster fragt den Status unter `sys.databases` ab. Wenn der Status nicht `ONLINE` lautet, löst er automatisch ein Failover aus (falls die Bedingungen für das automatische Failover erfüllt sind). Die tatsächliche Dauer des Failovers hängt sowohl von der Frequenz der Überwachungsaktion als auch vom Datenbankstatus ab, der unter „sys.databases“ aktualisiert wird.

Für ein automatisches Failover wird mindestens ein synchrones Replikat benötigt.

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren des Red Hat Enterprise Linux-Clusters für Clusterressourcen von SQL Server-Verfügbarkeitsgruppen](sql-server-linux-availability-group-cluster-rhel.md)

[Konfigurieren des SUSE Linux Enterprise Server-Clusters für Clusterressourcen von SQL Server-Verfügbarkeitsgruppen](sql-server-linux-availability-group-cluster-sles.md)

[Konfigurieren des Ubuntu-Clusters für Clusterressourcen von SQL Server-Verfügbarkeitsgruppen](sql-server-linux-availability-group-cluster-ubuntu.md)
