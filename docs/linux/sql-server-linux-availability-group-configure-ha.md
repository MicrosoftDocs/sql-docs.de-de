---
title: Konfigurieren von Verfügbarkeitsgruppen für SQL Server für Linux
description: Erfahren Sie mehr über das Erstellen einer SQL Server-Always On-Verfügbarkeitsgruppe (AG) für Hochverfügbarkeit unter Linux.
author: VanMSFT
ms.custom: seo-lt-2019
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/26/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 7313fbc789b37cf326d55566c1f087f80b4929df
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351942"
---
# <a name="configure-sql-server-always-on-availability-group-for-high-availability-on-linux"></a>Konfigurieren von SQL Server-Always On-Verfügbarkeitsgruppen für Hochverfügbarkeit unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel wird beschrieben, wie Sie eine SQL Server-Always On-Verfügbarkeitsgruppe (AG) für Hochverfügbarkeit unter Linux erstellen. Für Verfügbarkeitsgruppen gibt es zwei verschiedene Konfigurationstypen. Eine Konfiguration mit *Hochverfügbarkeit* verwendet einen Cluster-Manager, um Geschäftskontinuität zu gewährleisten. Diese Konfiguration kann auch Replikate mit Leseskalierung enthalten. In diesem Dokument wird erläutert, wie die Verfügbarkeitsgruppe für Hochverfügbarkeit erstellt wird.

Sie können eine Verfügbarkeitsgruppe auch ohne einen Cluster-Manager für die *Leseskalierung* erstellen. Die Verfügbarkeitsgruppe für Leseskalierung bietet nur schreibgeschützte Replikate für die horizontale Skalierung von Leistung. Sie bietet keine Hochverfügbarkeit. Informationen zum Erstellen einer Verfügbarkeitsgruppe für die Leseskalierung finden Sie unter [Configure a SQL Server Availability Group for read-scale on Linux (Konfigurieren einer SQL Server-Verfügbarkeitsgruppe für die Leseskalierung unter Linux)](sql-server-linux-availability-group-configure-rs.md).

Konfigurationen, die Hochverfügbarkeit und Datenschutz garantieren, benötigen entweder zwei oder drei synchrone Commitreplikate. Bei drei synchronen Replikaten kann die Verfügbarkeitsgruppe automatisch wiederhergestellt werden, auch wenn ein Server nicht verfügbar ist. Weitere Informationen finden Sie unter [High availability and data protection for Availability Group configurations (Hochverfügbarkeit und Datenschutz für Verfügbarkeitsgruppenkonfigurationen)](sql-server-linux-availability-group-ha.md). 

Alle Server müssen entweder physisch oder virtuell sein, und virtuelle Server müssen sich auf derselben Virtualisierungsplattform befinden. Diese Anforderung ist notwendig, da die Fencing-Agents plattformspezifisch sind. Informationen finden Sie unter [Richtlinien für Gastcluster](https://access.redhat.com/articles/29440#guest_policies).

## <a name="roadmap"></a>Roadmap

Die Schritte zum Erstellen einer Verfügbarkeitsgruppe auf Linux-Servern für Hochverfügbarkeit unterscheiden sich von den Schritten in Windows Server-Failoverclustern. Die allgemeinen Schritte werden in der folgenden Liste beschrieben: 

1. [Konfigurieren Sie SQL Server auf den drei Clusterservern](sql-server-linux-setup.md).

   >[!IMPORTANT]
   >Alle drei Server in der Verfügbarkeitsgruppe müssen sich auf derselben (physischen oder virtuellen) Plattform befinden, da die Linux-Hochverfügbarkeit Fencing-Agents zum Isolieren von Ressourcen auf Servern verwendet. Die Fencing-Agents sind für jede Plattform spezifisch.

2. Erstellen Sie die Verfügbarkeitsgruppe. Dieser Schritt wird im aktuellen Artikel behandelt. 

3. Konfigurieren Sie einen Cluster Resource Manager wie Pacemaker.
   
   Die Art und Weise des Konfigurierens eines Clusterressourcen-Managers hängt von der jeweiligen Linux-Distribution ab. Unter den folgenden Links finden Sie distributionsspezifische Anweisungen: 

   * [RHEL](sql-server-linux-availability-group-cluster-rhel.md)
   * [SUSE](sql-server-linux-availability-group-cluster-sles.md)
   * [Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md)

   >[!IMPORTANT]
   >In Produktionsumgebungen wird zur Gewährleistung vonHochverfügbarkeit ein Fencing-Agent wie STONITH benötigt. In den Demos dieser Dokumentation werden keine Fencing-Agents verwendet. Die Demos dienen lediglich zu Testzwecken und Überprüfungen. 
   
   >Ein Linux-Cluster verwendet Fencing, um den Cluster in einen bekannten Zustand zurückzusetzen. Wie das Fencing konfiguriert wird, hängt von der Verteilung und der Umgebung ab. Derzeit ist Fencing in einigen Cloudumgebungen nicht verfügbar. Weitere Informationen finden Sie unter [Supportrichtlinien für RHEL-Hochverfügbarkeitscluster – Virtualisierungsplattformen](https://access.redhat.com/articles/29440).
   
   >Informationen zu SLES finden Sie unter [SUSE Linux Enterprise-Hochverfügbarkeitserweiterung](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.fencing).

5. Fügen Sie die Verfügbarkeitsgruppe im Cluster als Ressource hinzu.  

   Die Möglichkeit, die Verfügbarkeitsgruppe im Cluster als Ressource hinzuzufügen, hängt von der Linux-Distribution ab. Unter den folgenden Links finden Sie distributionsspezifische Anweisungen: 

   * [RHEL](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource)
   * [SLES](sql-server-linux-availability-group-cluster-sles.md#configure-the-cluster-resources-for-sql-server)
   * [Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md#create-availability-group-resource)

[!INCLUDE [Create Prerequisites](../includes/ss-linux-cluster-availability-group-create-prereq.md)]

## <a name="create-the-ag"></a>Erstellen der Verfügbarkeitsgruppe

In den Beispielen in diesem Abschnitt wird erläutert, wie die Verfügbarkeitsgruppe mithilfe von Transact-SQL erstellt wird. Sie können auch den SQL Server Management Studio-Assistent für Verfügbarkeitsgruppen verwenden. Beim Erstellen einer Verfügbarkeitsgruppe mit dem Assistenten erhalten Sie eine Fehlermeldung, wenn Sie die Replikate der Verfügbarkeitsgruppe hinzufügen. Erteilen Sie dem Pacemaker auf der Verfügbarkeitsgruppe für alle Replikate die Berechtigungen für `ALTER`, `CONTROL` und `VIEW DEFINITIONS`, um dieses Problem zu beheben. Verknüpfen Sie nach Erteilen der Berechtigungen für das primäre Replikat mithilfe des Assistenten die Knoten mit der Verfügbarkeitsgruppe. Erteilen Sie die Berechtigungen für alle Replikate, damit die Hochverfügbarkeit ordnungsgemäß funktioniert.

Für eine Konfiguration für Hochverfügbarkeit erfordert die Verfügbarkeitsgruppe mindestens drei Replikate, um automatische Failover zu gewährleisten. Diese beiden Konfigurationen unterstützen Hochverfügbarkeit:

- [Drei synchrone Replikate](sql-server-linux-availability-group-ha.md#threeSynch)

- [Zwei synchrone Replikate und ein Konfigurationsreplikat](sql-server-linux-availability-group-ha.md#twoSynch)

Weitere Informationen finden Sie unter [High availability and data protection for Availability Group configurations (Hochverfügbarkeit und Datenschutz für Verfügbarkeitsgruppenkonfigurationen)](sql-server-linux-availability-group-ha.md).

>[!NOTE]
>Die Verfügbarkeitsgruppen können zusätzliche synchrone oder asynchrone Replikate enthalten. 

Erstellen Sie die Verfügbarkeitsgruppe für Hochverfügbarkeit unter Linux. Verwenden Sie [CREATE AVAILABILITY GROUP](../t-sql/statements/create-availability-group-transact-sql.md) mit `CLUSTER_TYPE = EXTERNAL`. 

* Verfügbarkeitsgruppe: `CLUSTER_TYPE = EXTERNAL` gibt an, dass ein eine externe Clusterentität die Verfügbarkeitsgruppe verwaltet. Pacemaker ist ein Beispiel für eine externe Clusterentität. Wenn der Clustertyp der Verfügbarkeitsgruppe extern ist, 

* legen Sie das primäre und das sekundäre Replikat auf `FAILOVER_MODE = EXTERNAL` fest. 
   Dies gibt an, dass das Replikat mit einem externen Cluster-Manager wie Pacemaker interagiert. 

Die folgenden Transact-SQL-Skripte erstellen eine Verfügbarkeitsgruppe für Hochverfügbarkeit namens `ag1`. Das Skript konfiguriert die Verfügbarkeitsgruppenreplikate mit `SEEDING_MODE = AUTOMATIC`. Diese Einstellung bewirkt, dass SQL Server die Datenbank automatisch auf jedem sekundären Server erstellt. Aktualisieren Sie das folgende Skript für Ihre Umgebung. Ersetzen Sie die Werte `<node1>`, `<node2>` oder `<node3>` durch die Namen der SQL Server-Instanzen, auf denen die Replikate gehostet werden. Ersetzen Sie `<5022>` durch den Port, den Sie für den Datenbankspiegelungsendpunkt festgelegt haben. Führen Sie zum Erstellen der Verfügbarkeitsgruppe das folgenden Transact-SQL-Skript auf der SQL Server-Instanz aus, die das primäre Replikat hostet.

Führen Sie **nur eines** der folgenden Skripte aus: 

- [Erstellen einer Verfügbarkeitsgruppe mit drei synchronen Replikaten](#threeSynch)
- [Erstellen einer Verfügbarkeitsgruppe mit zwei synchronen Replikaten und einem Konfigurationsreplikat](#configOnly)
- [Erstellen einer Verfügbarkeitsgruppe mit zwei synchronen Replikaten](#readScale)

<a name="threeSynch"></a>

- Erstellen einer Verfügbarkeitsgruppe mit drei synchronen Replikaten

   ```SQL
   CREATE AVAILABILITY GROUP [ag1]
       WITH (DB_FAILOVER = ON, CLUSTER_TYPE = EXTERNAL)
       FOR REPLICA ON
           N'<node1>' 
            WITH (
               ENDPOINT_URL = N'tcp://<node1>:<5022>',
               AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
               FAILOVER_MODE = EXTERNAL,
               SEEDING_MODE = AUTOMATIC
               ),
           N'<node2>' 
            WITH ( 
               ENDPOINT_URL = N'tcp://<node2>:<5022>', 
               AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
               FAILOVER_MODE = EXTERNAL,
               SEEDING_MODE = AUTOMATIC
               ),
           N'<node3>'
           WITH( 
              ENDPOINT_URL = N'tcp://<node3>:<5022>', 
              AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
              FAILOVER_MODE = EXTERNAL,
              SEEDING_MODE = AUTOMATIC
              );
        
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```

   >[!IMPORTANT]
   >Führen Sie das folgende Skript nicht aus, nachdem Sie das vorherige Skript zum Erstellen einer Verfügbarkeitsgruppe mit drei synchronen Replikaten ausgeführt haben:

<a name="configOnly"></a>
- Erstellen einer Verfügbarkeitsgruppe mit zwei synchronen Replikaten und einem Konfigurationsreplikat:

   >[!IMPORTANT]
   >Mit dieser Architektur kann jede SQL Server-Edition das dritte Replikat hosten. Das dritte Replikat kann beispielsweise auf SQL Server Express Edition gehostet werden. Unter Express Edition ist `WITNESS` der einzige gültige Endpunkttyp. 

   ```SQL
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
          ENDPOINT_URL = N'tcp://<node2>:<5022>',  
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
<a name="readScale"></a>

- Erstellen einer Verfügbarkeitsgruppe mit zwei synchronen Replikaten

   Schließt zwei Replikate mit synchronem Verfügbarkeitsmodus ein. Mit dem folgenden Skript wird beispielsweise eine Verfügbarkeitsgruppe namens `ag1` erstellt. `node1` und `node2` hosten Replikate im synchronen Modus mit automatischem Seeding und automatischem Failover.

   >[!IMPORTANT]
   >Führen Sie das folgende Skript nur zum Erstellen einer Verfügbarkeitsgruppe mit zwei synchronen Replikaten aus. Führen Sie das folgende Skript nicht aus, wenn Sie das vorherige Skript ausgeführt haben. 

   ```SQL
   CREATE AVAILABILITY GROUP [ag1]
      WITH (CLUSTER_TYPE = EXTERNAL)
      FOR REPLICA ON
      N'node1' WITH (
         ENDPOINT_URL = N'tcp://node1:5022',
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
         FAILOVER_MODE = EXTERNAL,
         SEEDING_MODE = AUTOMATIC
      ),
      N'node2' WITH ( 
         ENDPOINT_URL = N'tcp://node2:5022', 
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
         FAILOVER_MODE = EXTERNAL,
         SEEDING_MODE = AUTOMATIC
      );
        
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```


Sie können eine Verfügbarkeitsgruppe auch mit `CLUSTER_TYPE=EXTERNAL` mithilfe von SQL Server Management Studio oder PowerShell konfigurieren. 

### <a name="join-secondary-replicas-to-the-ag"></a>Verknüpfen sekundärer Replikate mit der Verfügbarkeitsgruppe

Der Pacemaker-Benutzer benötigt für die Verfügbarkeitsgruppe auf allen Replikaten Berechtigungen für `ALTER`, `CONTROL` und `VIEW DEFINITION`. Führen Sie zum Erteilen von Berechtigungen das folgende Transact-SQL-Skript aus, nachdem die Verfügbarkeitsgruppe auf dem primären und jedem sekundären Replikat erstellt wurde, sobald diese zur Verfügbarkeitsgruppe hinzugefügt wurden. Ersetzen Sie vor der Ausführung des Skripts `<pacemakerLogin>` durch den Namen des Pacemaker-Benutzerkontos. Wenn Sie nicht über eine Pacemaker-Anmeldung verfügen, [erstellen Sie einen SQL Server-Anmeldung für Pacemaker](sql-server-linux-availability-group-cluster-ubuntu.md#create-a-sql-server-login-for-pacemaker).

```sql
GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::ag1 TO <pacemakerLogin>
GRANT VIEW SERVER STATE TO <pacemakerLogin>
```

Das folgende Transact-SQL-Skript verknüpft eine SQL Server-Instanz mit einer Verfügbarkeitsgruppe namens `ag1`. Aktualisieren Sie das Skript für Ihre Umgebung. Führen Sie auf jeder SQL Server-Instanz, die ein sekundäres Replikat hostet, das folgende Transact-SQL-Skript aus, um die Verfügbarkeitsgruppe zu verknüpfen.

```sql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
         
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create Post](../includes/ss-linux-cluster-availability-group-create-post.md)]

>[!IMPORTANT]
>Nach dem Erstellen der Verfügbarkeitsgruppe müssen Sie für Hochverfügbarkeit die Integration in eine Clustertechnologie wie Pacemaker konfigurieren. Ab [!INCLUDE [SQL Server version](../includes/sssql17-md.md)] ist das Einrichten eines Clusters für eine Leseskalierungskonfiguration mit Verfügbarkeitsgruppen nicht erforderlich.

Wenn Sie die Schritte in diesem Dokument befolgt haben, verfügen Sie über eine Verfügbarkeitsgruppe, die noch nicht gruppiert ist. Der nächste Schritt besteht darin, den Cluster hinzuzufügen. Diese Konfiguration gilt für Leseskalierungs-/Lastenausgleichsszenarios und ist für die Hochverfügbarkeit nicht abgeschlossen. Für Hochverfügbarkeit müssen Sie die Verfügbarkeitsgruppe als Clusterressource hinzufügen. Anweisungen finden Sie unter [Nächste Schritte](#next-steps). 

## <a name="notes"></a>Notizen

>[!IMPORTANT]
>Nachdem Sie den Cluster konfiguriert und die Verfügbarkeitsgruppe als Clusterressource hinzugefügt haben, können Sie ein Failover der Verfügbarkeitsgruppen nicht mehr mit Transact-SQL durchführen. SQL Server-Clusterressourcen unter Linux sind nicht so eng mit dem Betriebssystem gekoppelt wie in einem Windows Server-Failovercluster (WSFC). Der SQL Server-Dienst kann das Vorhandensein des Clusters nicht erkennen. Die gesamte Orchestrierung erfolgt über die Clusterverwaltungstools. Verwenden Sie in RHEL oder Ubuntu `pcs`. Verwenden Sie in SLES `crm`. 

>[!IMPORTANT]
>Wenn die Verfügbarkeitsgruppe eine Clusterressource ist, gibt es in der aktuellen Version ein bekanntes Problem, bei dem ein erzwungenes Failover mit Datenverlust für ein asynchrones Replikat nicht funktioniert. Dies wird in der nächsten Version korrigiert. Ein manuelles oder automatisches Failover zu einem synchronen Replikat ist erfolgreich.


## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren des Red Hat Enterprise Linux-Clusters für Clusterressourcen von SQL Server-Verfügbarkeitsgruppen](sql-server-linux-availability-group-cluster-rhel.md)

[Konfigurieren des SUSE Linux Enterprise Server-Clusters für Clusterressourcen von SQL Server-Verfügbarkeitsgruppen](sql-server-linux-availability-group-cluster-sles.md)

[Konfigurieren des Ubuntu-Clusters für Clusterressourcen von SQL Server-Verfügbarkeitsgruppen](sql-server-linux-availability-group-cluster-ubuntu.md)