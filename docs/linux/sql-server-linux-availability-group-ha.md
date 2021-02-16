---
title: 'Bereitstellungsmuster für Verfügbarkeitsgruppen: SQL Server für Linux'
description: In diesem Artikel werden unterstützte Bereitstellungskonfigurationen für SQL Server-Always On-Verfügbarkeitsgruppen auf Linux-Servern vorgestellt.
ms.custom: seo-lt-2019
ms.date: 04/17/2019
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
ms.assetid: edd75f68-dc62-4479-a596-57ce8ad632e5
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.openlocfilehash: edeb7ab3de19b50f78f2b4b5bab9c0f8afe80780
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347778"
---
# <a name="high-availability-and-data-protection-for-availability-group-configurations"></a>Hochverfügbarkeit und Schutz von Daten für Verfügbarkeitsgruppenkonfigurationen

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel werden unterstützte Bereitstellungskonfigurationen für SQL Server-Always On-Verfügbarkeitsgruppen auf Linux-Servern vorgestellt. Eine Verfügbarkeitsgruppe unterstützt Hochverfügbarkeit und den Schutz von Daten. Ersteres wird durch die automatische Fehlererkennung, das automatische Failover und durch transparente Neuverbindungen nach dem Failover sichergestellt. Letzteres wird durch synchronisierte Replikate gewährleistet. 

Auf einem Windows Server-Failovercluster (WSFC) umfasst eine gängige Hochverfügbarkeitskonfiguration zwei synchrone Replikate und einen dritten Server oder eine Dateifreigabe zur Bereitstellung des Quorums. Der Dateifreigabenzeuge überprüft die Konfiguration der Verfügbarkeitsgruppe, also z. B. den Synchronisierungsstatus und die Rolle des Replikats. Durch diese Konfiguration wird sichergestellt, dass das sekundäre Replikat, das als Failoverziel ausgewählt wurde, über die neuesten Daten verfügt und eine aktuelle Verfügbarkeitsgruppenkonfiguration besitzt. 

Der WSFC synchronisiert die Konfigurationsmetadaten für die Failoververmittlung. Dabei wird entschieden, ob ein Failover für die Verfügbarkeitsgruppenreplikate oder den Dateifreigabezeugen ausgeführt wird. Wenn sich eine Verfügbarkeitsgruppe nicht auf einem WSFC befindet, speichern die SQL Server-Instanzen Konfigurationsmetadaten in der Masterdatenbank.

Angenommen, für eine Verfügbarkeitsgruppe auf einem Linux-Cluster ist die Einstellung `CLUSTER_TYPE = EXTERNAL` festgelegt. Es ist kein WSFC zur Failoververmittlung vorhanden. In diesem Fall werden die Konfigurationsmetadaten von den SQL Server-Instanzen verwaltet. Da es keinen Zeugenserver auf diesem Cluster gibt, ist eine dritte SQL Server-Instanz erforderlich, um Metadaten zum Konfigurationszustand zu speichern. Alle drei SQL Server-Instanzen stellen einen verteilten Metadatenspeicher für den Cluster bereit. 

Der Cluster-Manager kann die SQL Server-Instanzen in der Verfügbarkeitsgruppe abfragen und das Failover orchestrieren, um Hochverfügbarkeit zu gewährleisten. Auf einem Linux-Cluster ist Pacemaker der Cluster-Manager. 

SQL Server 2017 CU1 ermöglicht für eine Verfügbarkeitsgruppe mit der Einstellung `CLUSTER_TYPE = EXTERNAL` Hochverfügbarkeit für zwei synchrone Replikate und für ein Replikat, das ausschließlich zur Konfiguration verwendet wird. Das letztgenannte Replikat kann auf jeder Edition von SQL Server 2017 CU1 oder höher und auch auf einer SQL Server Express Edition gehostet werden. Es verwaltet Konfigurationsinformationen zur Verfügbarkeitsgruppe in der Masterdatenbank, enthält jedoch nicht die Benutzerdatenbank in der Verfügbarkeitsgruppe. 

## <a name="how-the-configuration-affects-default-resource-settings"></a>Auswirkungen der Konfiguration auf die Standardeinstellungen für Ressourcen

Ab SQL Server 2017 wird die Einstellung `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` für Clusterressourcen verwendet. Diese gewährleistet, dass die angegebenen sekundären Replikate die Transaktionsdaten in ein Protokoll schreiben, bevor das primäre Replikat für jede Transaktion einen Commit ausführt. Wenn Sie einen externen Cluster-Manager verwenden, wirkt sich diese Einstellung sowohl auf die Hochverfügbarkeit als auch auf den Schutz von Daten aus. Der Standardwert für die Einstellung hängt von der Architektur ab, die zu dem Zeitpunkt verwendet wird, an dem die Clusterressource erstellt wird. Wenn Sie den SQL Server-Ressourcen-Agent `mssql-server-ha` installieren und eine Clusterressource für die Verfügbarkeitsgruppe erstellen, erkennt der Cluster-Manager die Konfiguration der Verfügbarkeitsgruppe und legt `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` entsprechend fest. 

Wenn der Parameter `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` des Ressourcen-Agents von der Konfiguration unterstützt wird, wird dieser Parameter auf den Wert festgelegt, der Hochverfügbarkeit und den Schutz von Daten bereitstellt. Weitere Informationen finden Sie unter [Grundlegendes zum SQL Server-Ressourcen-Agent für Pacemaker](#pacemakerNotify).

In den folgenden Abschnitten wird das Standardverhalten für die Clusterressource beschrieben. 

Durch die Auswahl eines bestimmten Entwurfs für Verfügbarkeitsgruppen können Sie spezifische Geschäftsanforderungen an die Hochverfügbarkeit, den Schutz von Daten und die Leseskalierung erfüllen.

In den unten aufgeführten Konfigurationen werden die Entwurfsmuster für Verfügbarkeitsgruppen und die Funktionen der einzelnen Muster beschrieben. Die folgenden Entwurfsmuster können für Verfügbarkeitsgruppen mit der Einstellung `CLUSTER_TYPE = EXTERNAL` in Lösungen verwendet werden, in denen Hochverfügbarkeit erforderlich ist: 

- **Drei synchrone Replikate**
- **zwei synchrone Replikate**
- **zwei synchrone Replikate und ein Konfigurationsreplikat**

<a name="threeSynch"></a>

## <a name="three-synchronous-replicas"></a>Drei synchrone Replikate

Diese Konfiguration umfasst drei synchrone Replikate. Die Hochverfügbarkeit und der Schutz von Daten werden standardmäßig gewährleistet. Auch eine Leseskalierung ist möglich.

![Drei Replikate][3]

Eine Verfügbarkeitsgruppe mit drei synchronen Replikaten ermöglicht die Leseskalierung, Hochverfügbarkeit und den Schutz von Daten. In der folgenden Tabelle wird das Verfügbarkeitsverhalten beschrieben. 

|Verfügbarkeitsverhalten |Leseskalierung|Hochverfügbarkeit und </br> Datenschutz | Schutz von Daten|
|:---|---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 |1<sup>\*</sup>|2|
|Ausfall des primären Replikats |Automatisches Failover. Das neue primäre Replikat besitzt Lese-/Schreibberechtigungen. |Automatisches Failover. Das neue primäre Replikat besitzt Lese-/Schreibberechtigungen. |Automatisches Failover. Das neue primäre Replikat steht erst dann für Benutzertransaktionen zur Verfügung, wenn das vorherige Replikat wiederhergestellt wurde und der Verfügbarkeitsgruppe als sekundäres Replikat beigetreten ist. |
|Ausfall eines sekundären Replikats  | Das primäre Replikat besitzt Lese-/Schreibberechtigungen. Es wird kein automatisches Failover ausgeführt, wenn das primäre Replikat ausfällt. |Das primäre Replikat besitzt Lese-/Schreibberechtigungen. Es wird kein automatisches Failover ausgeführt, wenn auch das primäre Replikat ausfällt. | Das primäre Replikat steht nicht für Benutzertransaktionen zur Verfügung. |

<sup>\*</sup> Standardwert

<a name="twoSynch"></a>

## <a name="two-synchronous-replicas"></a>Zwei synchrone Replikate

Diese Konfiguration ermöglicht den Schutz von Daten. Ebenso wie bei anderen Konfigurationen für Verfügbarkeitsgruppen kann auch durch diese Konfiguration die Leseskalierung ermöglicht werden. Die Konfiguration mit zwei synchronen Replikaten gewährleistet nicht automatisch Hochverfügbarkeit und kann nur mit SQL Server 2017 RTM verwendet werden. Neuere Versionen (CU1 und höher) von SQL Server 2017 werden nicht mehr unterstützt.

![Zwei synchrone Replikate][1]

Eine Verfügbarkeitsgruppe mit zwei synchronen Replikaten gewährleistet die Leseskalierung und den Schutz von Daten. In der folgenden Tabelle wird das Verfügbarkeitsverhalten beschrieben. 

|Verfügbarkeitsverhalten |Leseskalierung |Schutz von Daten|
|:---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 <sup>\*</sup>|1|
|Ausfall des primären Replikats | Manuelles Failover. Möglicherweise kommt es zu Datenverlusten. Das neue primäre Replikat besitzt Lese-/Schreibberechtigungen.| Automatisches Failover. Das neue primäre Replikat steht erst dann für Benutzertransaktionen zur Verfügung, wenn das vorherige Replikat wiederhergestellt wurde und der Verfügbarkeitsgruppe als sekundäres Replikat beigetreten ist.|
|Ausfall eines sekundären Replikats  |Das primäre Replikat besitzt Lese-/Schreibberechtigungen. Es wird nicht gespiegelt, weshalb die Gefahr eines Datenverlusts besteht. |Das primäre Replikat steht erst dann für Benutzertransaktionen zur Verfügung, wenn das sekundäre Replikat wiederhergestellt wurde.|

<sup>\*</sup> Standardwert

<a name = "configOnly"></a>

## <a name="two-synchronous-replicas-and-a-configuration-only-replica"></a>Zwei synchrone Replikate und ein Konfigurationsreplikat

Eine Verfügbarkeitsgruppe mit mindestens zwei synchronen Replikaten und einem Konfigurationsreplikat gewährleistet den Schutz von Daten und kann ggf. auch Hochverfügbarkeit sicherstellen. Das folgende Diagramm veranschaulicht diese Architektur:

![Verfügbarkeitsgruppe für Konfiguration][2]

1. Eine synchrone Replikation von Benutzerdaten wird auf einem sekundären Replikat ausgeführt. Dieser Vorgang schließt auch Metadaten zur Konfiguration der Verfügbarkeitsgruppe ein.
2. Eine synchrone Replikation von Metadaten zur Konfiguration der Verfügbarkeitsgruppe wird ausgeführt. Dieser Vorgang schließt keine Benutzerdaten ein.

Im obigen Diagramm überträgt ein primäres Replikat die Konfigurationsdaten per Push sowohl an das sekundäre Replikat als auch an das Konfigurationsreplikat. Das sekundäre Replikat empfängt ebenfalls Benutzerdaten, das Konfigurationsreplikat hingegen nicht. Das sekundäre Replikat befindet sich im synchronen Verfügbarkeitsmodus. Das Konfigurationsreplikat umfasst nicht die Datenbanken in der Verfügbarkeitsgruppe, sondern nur die Metadaten zur Verfügbarkeitsgruppe. Für die Konfigurationsdaten auf dem Konfigurationsreplikat wird synchron ein Commit ausgeführt.

> [!NOTE]
> Ab SQL Server 2017 CU1 wird erstmals eine Verfügbarkeitsgruppe mit einem Konfigurationsreplikat eingesetzt. Alle SQL Server-Instanzen in der Verfügbarkeitsgruppe müssen SQL Server 2017 CU1 oder höher entsprechen. 

Der Standardwert für `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` ist 0. In der folgenden Tabelle wird das Verfügbarkeitsverhalten beschrieben. 

|Verfügbarkeitsverhalten |Hochverfügbarkeit und </br> Datenschutz | Schutz von Daten|
|:---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 <sup>\*</sup>|1|
|Ausfall des primären Replikats | Automatisches Failover. Das neue primäre Replikat besitzt Lese-/Schreibberechtigungen. | Automatisches Failover. Das neue primäre Replikat steht nicht für Benutzertransaktionen zur Verfügung. |
|Ausfall des sekundären Replikats | Das primäre Replikat besitzt Lese-/Schreibberechtigungen. Es wird nicht gespiegelt, weshalb die Gefahr eines Datenverlusts besteht (falls das primäre Replikat ausfällt und nicht wiederhergestellt werden kann). Es wird kein automatisches Failover ausgeführt, wenn auch das primäre Replikat ausfällt. | Das primäre Replikat steht nicht für Benutzertransaktionen zur Verfügung. Es gibt kein Replikat, das als Failoverziel verwendet werden kann, wenn auch das primäre Replikat ausfällt. |
|Ausfall des Konfigurationsreplikats | Das primäre Replikat besitzt Lese-/Schreibberechtigungen. Es wird kein automatisches Failover ausgeführt, wenn auch das primäre Replikat ausfällt. | Das primäre Replikat besitzt Lese-/Schreibberechtigungen. Es wird kein automatisches Failover ausgeführt, wenn auch das primäre Replikat ausfällt. |
|Ausfall des synchronen sekundären Replikats und des Konfigurationsreplikats| Das primäre Replikat steht nicht für Benutzertransaktionen zur Verfügung. Es wird kein automatisches Failover ausgeführt. | Das primäre Replikat steht nicht für Benutzertransaktionen zur Verfügung. Es gibt kein Replikat, das als Failoverziel verwendet werden kann, wenn auch das primäre Replikat ausfällt. |

<sup>\*</sup> Standardwert

> [!NOTE]
> Die SQL Server-Instanz, die das Konfigurationsreplikat hostet, kann auch andere Datenbanken hosten. Sie kann auch als Konfigurationsdatenbank für mehrere Verfügbarkeitsgruppen verwendet werden. 

## <a name="requirements"></a>Requirements (Anforderungen)

- Alle Replikate in einer Verfügbarkeitsgruppe mit einem Konfigurationsreplikat müssen SQL Server 2017 CU 1 oder höher entsprechen.
- Jede Edition von SQL Server kann ein Konfigurationsreplikat hosten. Dies gilt auch für SQL Server Express. 
- Für die Verfügbarkeitsgruppe ist mindestens ein sekundäres Replikat zusätzlich zum primären Replikat erforderlich.
- Die maximale Anzahl der Replikate pro SQL Server-Instanz ist unabhängig von der Anzahl der Konfigurationsreplikate. Für die Standard Edition von SQL Server werden maximal drei Replikate unterstützt, für die Enterprise Edition von SQL Server maximal neun.

## <a name="considerations"></a>Überlegungen

- Eine Verfügbarkeitsgruppe kann höchstens ein Konfigurationsreplikat enthalten. 
- Ein Konfigurationsreplikat kann kein primäres Replikat sein.
- Sie können den Verfügbarkeitsmodus eines Konfigurationsreplikats nicht anpassen. Wenn Sie von einem Konfigurationsreplikat zu einem synchronen oder asynchronen sekundären Replikat wechseln möchten, müssen Sie das Konfigurationsreplikat entfernen und ein sekundäres Replikat mit dem erforderlichen Verfügbarkeitsmodus hinzufügen. 
- Die Daten des Konfigurationsreplikats sind mit den Metadaten der Verfügbarkeitsgruppe synchron. Benutzerdaten sind nicht vorhanden. 
- Eine Verfügbarkeitsgruppe ist ungültig, wenn sie über ein primäres Replikat und ein Konfigurationsreplikat, jedoch nicht über ein sekundäres Replikat verfügt. 
- Sie können keine Verfügbarkeitsgruppe auf einer Instanz erstellen, die der SQL Server Express Edition entspricht. 

<a name="pacemakerNotify"></a>

## <a name="understand-sql-server-resource-agent-for-pacemaker"></a>Grundlegendes zum SQL Server-Ressourcen-Agent für Pacemaker

In SQL Server 2017 CTP 1.4 wurde `sys.availability_groups` die Variable `sequence_number` hinzugefügt. Damit erkennt Pacemaker, wie aktuell sekundäre Replikate im Vergleich zu primären Replikaten sind. `sequence_number` ist eine Variable des Datentyps BIGINT, die fortlaufend erhöht wird und darstellt, wie aktuell das lokale Replikat der Verfügbarkeitsgruppe ist. Pacemaker aktualisiert `sequence_number` bei jeder Änderung der Verfügbarkeitsgruppenkonfiguration. Eine solche Änderung tritt beispielsweise auf, wenn ein Failover ausgeführt oder ein Replikat hinzugefügt oder entfernt wird. Die Zahl wird auf dem primären Replikat aktualisiert und anschließend auf sekundären Replikaten repliziert. Daher verfügt ein sekundäres Replikat mit einer aktuellen Konfiguration über die gleiche Sequenznummer (sequence_number) wie das primäre Replikat. 

Wenn Pacemaker entscheidet, ein Replikat zu einem primären Replikat heraufzustufen, wird zuerst eine *Vorabbenachrichtigung* an alle Replikate gesendet. Die Replikate geben daraufhin die Sequenznummer zurück. Wenn Pacemaker anschließend tatsächlich versucht, ein Replikat zu einem primären Replikat heraufzustufen, stuft das Replikat sich selbst nur herauf, wenn seine Sequenznummer die höchste ist. Wenn das nicht der Fall ist, lehnt das Replikat die Heraufstufung ab. Auf diese Weise kann nur das Replikat mit der höchsten Sequenznummer zu einem primären heraufgestuft werden, sodass es nicht zu Datenverlust kommt. 

Voraussetzung für diesen Prozess ist mindestens ein Replikat, das für die Heraufstufung verfügbar ist und die gleiche Sequenznummer wie das vorherige primäre Replikat besitzt. Der Pacemaker-Ressourcen-Agent legt für `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` einen Wert fest, durch den sichergestellt wird, dass mindestens ein synchrones sekundäres Replikat aktuell ist und als Ziel eines automatischen Failovers standardmäßig verfügbar ist. Bei jeder Überwachungsaktion wird der Wert von `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` berechnet (und bei Bedarf aktualisiert). Der Wert von `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` ergibt sich aus der Anzahl der synchronen Replikate dividiert durch zwei. Zum Zeitpunkt des Failovers benötigt der Ressourcen-Agent den Wert aus `total number of replicas` – `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT`-Replikate, um auf die Vorabbenachrichtigung antworten zu können. Das Replikat mit dem höchsten `sequence_number`-Wert wird zum primären Replikat heraufgestuft. 

Beispiel: Eine Verfügbarkeitsgruppe enthält drei synchrone Replikate: ein primäres Replikat und zwei synchrone sekundäre Replikate.

- `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` ist 1 (3 / 2 -> 1).

- Die erforderliche Anzahl von Replikaten für die Antwort auf die Vorabbenachrichtigung ist 2 (3 – 1 = 2). 

In diesem Szenario müssen zwei Replikate antworten, damit das Failover ausgelöst wird. Nach einem Ausfall eines primären Replikats müssen beide sekundäre Replikate aktuell sein und auf die Vorabbenachrichtigung antworten, damit das automatische Failover erfolgreich ausgeführt wird. Wenn sie online und synchron sind, haben sie die gleiche Sequenznummer. Die Verfügbarkeitsgruppe stuft dann eines der beiden Replikate herauf. Wenn nur eines der sekundären Replikate auf die Vorabbenachrichtigung antwortet, kann der Ressourcen-Agent nicht garantieren, dass genau dieses Replikat über die höchste Sequenznummer verfügt. In diesem Fall wird kein Failover ausgelöst.

> [!IMPORTANT]
> Wenn `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 0 (null) entspricht, besteht das Risiko von Datenverlust. Bei einem Ausfall des primären Replikats löst der Ressourcen-Agent nicht automatisch ein Failover aus. Sie können entweder warten, bis das primäre Replikat wiederhergestellt ist, oder ein manuelles Failover mit `FORCE_FAILOVER_ALLOW_DATA_LOSS` ausführen.

Sie können das Standardverhalten überschreiben und verhindern, dass die Verfügbarkeitsgruppe `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` automatisch festlegt.

Mit dem folgenden Skript wird `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` für eine Verfügbarkeitsgruppe mit dem Namen `<**ag1**>` auf 0 festgelegt. Ersetzen Sie `<**ag1**>` vor der Ausführung durch den Namen der Verfügbarkeitsgruppe.

```bash
sudo pcs resource update <**ag1**> required_synchronized_secondaries_to_commit=0
```

Wenn Sie den Standardwert wiederherstellen möchten, der auf der Konfiguration der Verfügbarkeitsgruppe basiert, führen Sie den folgenden Befehl aus:

```bash
sudo pcs resource update <**ag1**> required_synchronized_secondaries_to_commit=
```

> [!NOTE]
> Wenn Sie die vorangehenden Befehle ausführen, wird das primäre Replikat temporär zu einem sekundären Replikat herabgestuft und dann erneut heraufgestuft. Durch das Ressourcenupdate werden alle Replikate angehalten und anschließend neu gestartet. Der neue Wert für `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` wird nicht sofort, sondern erst nach dem Neustart der Replikate festgelegt.

## <a name="see-also"></a>Weitere Informationen

[Verfügbarkeitsgruppen unter Linux](sql-server-linux-availability-group-overview.md)

<!--Image references-->
[1]: ./media/sql-server-linux-availability-group-ha/1-read-scale-out.png
[2]: ./media/sql-server-linux-availability-group-ha/2-configuration-only.png
[3]: ./media/sql-server-linux-availability-group-ha/3-three-replica.png
[4]: ./media/sql-server-linux-availability-group-ha/configuration-only-example.png
