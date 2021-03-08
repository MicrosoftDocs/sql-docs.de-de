---
title: Versionshinweise zu SQL Server-Big Data-Clustern
titleSuffix: SQL Server big data clusters
description: In diesem Artikel werden die neuesten Updates und bekannten Probleme von SQL Server-Big Data-Clustern beschrieben.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c4cb10ac3ba1e0fd8b437e7f0509dc16cc72d854
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837067"
---
# <a name="sql-server-2019-big-data-clusters-release-notes"></a>Versionshinweise zu Big Data-Clustern für SQL Server 2019

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Die folgenden Versionshinweise gelten für [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Dieser Artikel ist in Abschnitte für jedes einzelne Release unterteilt. Jede Version verfügt über einen Link zu einem Supportartikel, in dem die Änderungen der Kapazitätseinheit und Links zu herunterladbaren Linux-Paketen enthalten sind. In diesem Artikel werden zudem [bekannte Probleme](#known-issues) für die neuesten Releases von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] (BDC) aufgeführt.

## <a name="supported-platforms"></a>Unterstützte Plattformen

In diesem Abschnitt werden die Plattformen aufgeführt, die mit BDC unterstützt werden.

### <a name="kubernetes-platforms"></a>Kubernetes-Plattformen

|Plattform|Unterstützte Versionen|
|---------|---------|
|Vanilla (Upstream) Kubernetes|Stellen Sie Big Data-Cluster lokal mithilfe eines Kubernetes-Clusters (mindestens Version 1.13) bereit. Weitere Informationen finden Sie unter [Richtlinie zur Unterstützung der Kubernetes-Version und Versionsabweichung](https://kubernetes.io/docs/setup/release/version-skew-policy/).|
|Red Hat OpenShift|Stellen Sie Big Data-Cluster lokal mithilfe eines OpenShift-Clusters (mindestens Version 4.3) bereit. Weitere Informationen finden Sie unter [Red Hat OpenShift Container Platform Life Cycle Policy (Richtlinie zum Lebenszyklus einer Red Hat OpenShift-Containerplattform)](https://access.redhat.com/support/policy/updates/openshift).<br><br> Die Unterstützung wurde in SQL Server 2019 CU5 eingeführt.|
|Azure Kubernetes Service (AKS)|Stellen Sie Big Data-Cluster für AKS-Cluster (mindestens Version 1.13) bereit.<br/>Informationen zu Versionsunterstützungsrichtlinien finden Sie unter [Unterstützte Kubernetes-Versionen in AKS](/azure/aks/supported-kubernetes-versions).|
|Azure Red Hat OpenShift (ARO)|Stellen Sie Big Data-Cluster für ARO (mindestens Version 4.3) bereit. Weitere Informationen finden Sie unter [Azure Red Hat OpenShift](/azure/openshift/). <br><br> Die Unterstützung wurde in SQL Server 2019 CU5 eingeführt.|

### <a name="host-os-for-kubernetes"></a>Hostbetriebssystem für Kubernetes

|Plattform|Hostbetriebssystem|Unterstützte Versionen|
|---------|---------|---------|
|Kubernetes|Ubuntu|16.04|
|Kubernetes|Red Hat Enterprise Linux|7.3, 7.4, 7.5, 7.6|
|OpenShift|Red Hat Enterprise Linux/CoreOS |Weitere Informationen finden Sie in den [Versionshinweisen zu OpenShift](https://docs.openshift.com/container-platform/4.3/release_notes/ocp-4-3-release-notes.html#ocp-4-3-about-this-release).|

### <a name="sql-server-editions"></a>SQL Server-Editionen

|Edition|Notizen|
|---------|---------|
|Enterprise<br/>Standard<br/>Entwickler| Die Edition von Big Data-Clustern wird von der Edition der SQL Server-Masterinstanz bestimmt. Zum Zeitpunkt der Bereitstellung wird standardmäßig die Developer Edition bereitgestellt. Sie können die Edition nach der Bereitstellung ändern. Informationen dazu finden Sie unter [Konfigurieren der SQL Server-Masterinstanz](./configure-sql-server-master-instance.md). |

## <a name="tools"></a>Tools

|Plattform|Unterstützte Versionen|
|---------|---------|
|[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]|Es wird empfohlen, die neuste verfügbare Version zu verwenden. Ab SQL Server 2019 CU5 verfügt [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] über eine unabhängige semantische Version des Servers. <br/><br/>Führen Sie `azdata –-version` aus, um die Version zu überprüfen.<br/><br/>Die aktuelle Version finden Sie unter [Releaseverlauf](#release-history).|
|Azure Data Studio|Rufen Sie den neuesten Build von [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) ab.|

Eine umfassende Liste finden Sie unter [Welche Tools sind erforderlich?](deploy-big-data-tools.md#which-tools-are-required)

## <a name="release-history"></a>Releaseverlauf

In der folgenden Tabelle wird der Releaseverlauf von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] aufgelistet.

| Release <sup>1</sup> | Big Data-Cluster-Version | [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], Version <sup>2</sup> | Veröffentlichungsdatum |
|--|--|--|--|
| [CU9](#cu9) |  15.0.4102.2 | 20.3.0    | 2021-02-11 |
| [CU8-GDR](#cu8-gdr) | 15.0.4083.2  | 20.2.6    | 2021-01-12 |
| [CU8](#cu8)     | 15.0.4073.23 | 20.2.2    | 19.10.2020 |
| [CU6](#cu6)     | 15.0.4053.23 | 20.0.1    | 4\.8.2020 |
| [CU5](#cu5)     | 15.0.4043.16 | 20.0.0    | 2020-06-22 |
| [CU4](#cu4)     | 15.0.4033.1  | 15.0.4033 | 31.3.2020 |
| [CU3](#cu3)     | 15.0.4023.6  | 15.0.4023 | 12.03.2020 |
| [CU2](#cu2)     | 15.0.4013.40 | 15.0.4013 | 13.2.2020 |
| [CU1](#cu1)     | 15.0.4003.23 | 15.0.4003 | 01.07.2020 |
| [GDR1](#rtm)    | 15.0.2070.34 | 15.0.2070 | 2019-11-04 |

<sup>1</sup> CU7 ist für BDC nicht verfügbar.

<sup>2</sup> Die [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]-Version entspricht der Version des Tools zum Zeitpunkt der CU-Veröffentlichung. `azdata` kann auch unabhängig von der Serververöffentlichung veröffentlicht werden. Daher erhalten Sie möglicherweise neuere Versionen, wenn Sie die neuesten Pakete installieren. Neuere Versionen sind mit zuvor veröffentlichten CUs kompatibel.

## <a name="how-to-install-updates"></a>Installieren von Updates

Informationen zum Installieren von Updates finden Sie unter [Upgraden von Big Data-Clustern für SQL Server [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-upgrade.md).

## <a name="cu9-february-2021"></a><a id="cu9"></a> CU9 (Februar 2021)

Kumulatives Update 9 (CU9) für SQL Server 2019.

|Paketversion | Imagetag |
|-----|-----|
|15.0.4102.2|[2019-CU9-ubuntu-16.04]|

SQL Server 2019 CU9 für SQL Server Big Data-Cluster umfasst wichtige Funktionen:

- Unterstützung für die Konfiguration von BDC nach der Bereitstellung und verbesserte Sichtbarkeit von Systemeinstellungen

   Für Cluster, bei denen `mssql-conf` für SQL Server-Masterinstanz-Konfigurationen verwendet wird, sind nach dem Upgrade auf CU9 zusätzliche Schritte erforderlich. Befolgen Sie [diese](bdc-upgrade-configuration.md) Anweisungen.

- Verbesserte [!INCLUDE[azdata](../includes/azure-data-cli-azdata.md)]-Umgebung bei der Verschlüsselung ruhender Daten.
- Möglichkeit, [Python Spark-Pakete über virtuelle Umgebungen dynamisch zu installieren](spark-install-packages.md).
- Aktualisierte Softwareversionen für die meisten unserer OSS-Komponenten (Grafana, Kibana, FluentBit usw.), um sicherzustellen, dass BDC-Images mit den neuesten Verbesserungen und Korrekturen aktualisiert wurden. Weitere Informationen finden Sie unter [Referenz zu Open-Source-Software](reference-open-source-software.md).
- Verschiedene weitere Verbesserungen und Fehlerbehebungen.

## <a name="cu8-gdrjanuary-2021"></a><a id="cu8-gdr"></a> CU8-DDR (Januar 2021)

Kumulatives Update 8 GDR-Release (CU8-DDR) für SQL Server 2019

|Paketversion | Imagetag |
|-----|-----|
|15.0.4083.2 |[2019-CU8-GDR2-ubuntu-16.04]|

## <a name="cu8-september-2020"></a><a id="cu8"></a> CU8 (September 2020)

Kumulatives Update 8 (CU8) für SQL Server 2019.

|Paketversion | Imagetag |
|-----|-----|
|15.0.4073.23 |[2019-CU8-ubuntu-16.04]

Diese Version enthält mehrere Fehlerkorrekturen und einige Verbesserungen.

### <a name="added-capabilities"></a>Erweiterungen

- [Verschlüsselung ruhender Daten für SQL Server Big Data-Cluster](encryption-at-rest-concepts-and-configuration.md) mithilfe von systemseitig verwalteten Schlüsseln und Zertifikaten.
   > [!CAUTION]
   > Dies ist die erste Version von SQL Server BDC-Verschlüsselung ruhender Daten. Lesen Sie die folgenden Artikel: 
   > - [Sicherheitskonzepte für [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](concept-security.md)
   > - [Konzepte- und Konfigurationsleitfaden für die Verschlüsselung ruhender Daten](encryption-at-rest-concepts-and-configuration.md)
- [Oracle-Proxybenutzer](tutorial-query-oracle.md)-Unterstützung für das Datenvirtualisierungsszenario.

## <a name="cu6-july-2020"></a><a id="cu6"></a> CU6 (Juli 2020)

Kumulatives Update 6 (CU6) für SQL Server 2019.

|Paketversion | Imagetag |
|-----|-----|
|15.0.4053.23 |[2019-CU6-ubuntu-16.04]

Diese Version enthält kleinere Fehlerkorrekturen und Verbesserungen. In den folgenden Artikeln finden Sie Informationen, die sich auf diese Updates beziehen:

- [Verwalten des Zugriffs auf Big Data-Cluster im Active Directory-Modus](manage-user-access.md)
- [Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] im Active Directory-Modus](active-directory-deploy.md)
- [Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in AKS im Active Directory-Modus](active-directory-deployment-aks.md)
- [Bereitstellen von Big Data-Clustern in einem privaten Azure Kubernetes Service-Cluster (AKS-Cluster)](private-deploy.md)
- [Einschränken des ausgehenden Datenverkehrs von Big Data-Clustern (BDC) in einem privaten Azure Kubernetes Service-Cluster (AKS-Cluster)](private-restrict-egress-traffic.md)
- [Bereitstellen von Big Data-Clustern in SQL Server mit Hochverfügbarkeit](deployment-high-availability.md)
- [Konfigurieren eines Big Data-Clusters in SQL Server](./configure-bdc-overview.md)
- [Konfigurieren von Apache Spark und Apache Hadoop in Big Data-Clustern](configure-spark-hdfs.md)
- [Konfigurationseigenschaften der SQL Server-Masterinstanz](reference-config-master-instance.md)
- [Konfigurationseigenschaften von Apache Spark und Apache Hadoop (HDFS)](reference-config-spark-hadoop.md)
- [RBAC-Modell in Kubernetes und Auswirkungen auf Benutzer und Dienstkonten, die Big Data-Cluster verwalten](kubernetes-rbac.md)

## <a name="cu5-june-2020"></a><a id="cu5"></a> CU5 (Juni 2020)

Kumulatives Update 5 (CU5) für SQL Server 2019.

|Paketversion | Imagetag |
|-----|-----|
|15.0.4043.16 |[2019-CU5-ubuntu-16.04]

### <a name="added-capabilities"></a>Erweiterungen

- Unterstützung für die Bereitstellung von Big Data-Cluster in Red Hat OpenShift wurde hinzugefügt. Die Unterstützung umfasst die OpenShift-Containerplattform, die lokal ab Version 4.3 und in Azure Red Hat OpenShift bereitgestellt wird. Weitere Informationen finden Sie unter [Bereitstellen von SQL Server-Big Data-Cluster in OpenShift](deploy-openshift.md).
- Das Sicherheitsmodell für die Big Data-Cluster-Bereitstellung wurde aktualisiert, wodurch privilegierte Container, die als Teil von Big Data-Cluster bereitgestellt werden, nicht mehr *erforderlich* sind. Zusätzlich zu nicht privilegierten Containern werden Container standardmäßig für alle neuen Bereitstellungen unter Verwendung von SQL Server 2019 CU5 nicht als Root-Benutzer ausgeführt. 
- Unterstützung für die Bereitstellung mehrerer Big Data-Cluster für eine Active Directory-Domäne wurde hinzugefügt.
- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] verfügt über eine eigene semantische Version, die unabhängig vom Server ist. Alle Abhängigkeiten zwischen dem Client und der Serverversion von „azdata“ wurden entfernt. Es wird empfohlen, die neuesten Versionen des Clients und des Servers zu verwenden, damit Sie von den neusten Erweiterungen und Fixes profitieren können.
- Es wurden zwei neue gespeicherte Prozeduren („sp_data_source_objects“ und „sp_data_source_table_columns“) eingeführt, um die Introspektion bestimmter externer Datenquellen zu unterstützen. Diese können von Kunden direkt über T-SQL für die Schemaermittlung verwendet werden, um herauszufinden, welche Tabellen für die Virtualisierung verfügbar sind. Wir nutzen diese Änderungen im Assistenten für externe Tabellen der [Datenvirtualisierungserweiterung](../azure-data-studio/extensions/data-virtualization-extension.md) für Azure Data Studio, mit dem Sie über SQL Server, Oracle, MongoDB und Teradata externe Tabellen erstellen können.
- Unterstützung wurde hinzugefügt, um in Grafana vorgenommene Anpassungen dauerhaft zu speichern. In der Vergangenheit haben CU5-Kunden festgestellt, dass Änderungen an Grafana-Konfigurationen bei einem Neustart von Pod `metricsui` (der das Grafana-Dashboard hostet) verloren gingen. Dieses Problem wurde behoben, und alle Konfigurationen werden nun dauerhaft gespeichert. 
- Es wurde ein Sicherheitsproblem behoben, das im Zusammenhang mit der API stand, die zum Sammeln von Pod- und Knotenmetriken mithilfe von Telegraf (in den `metricsdc`-Pods gehostet) verwendet wird. Aufgrund dieser Änderung wird für Telegraf nun ein Dienstkonto, eine Clusterrolle sowie Clusterbindungen benötigt, um die Pod- und Knotenmetriken zu sammeln. Weitere Informationen finden Sie unter [Zum Sammeln von Pod- und Knotenmetriken erforderliche Clusterrolle](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection).
- Es wurden zwei neue Featureparameter eigeführt, um die Sammlung von Pod- und Knotenmetriken zu steuern. Wenn Sie Ihre Kubernetes-Infrastruktur mithilfe anderer Lösungen überwachen, können Sie die integrierte Metriksammlung für Pods und Hostknoten deaktivieren, indem Sie *allowNodeMetricsCollection* und *allowPodMetricsCollection* in der Bereitstellungskonfigurationsdatei control.json auf „False“ festlegen. Bei OpenShift-Umgebungen werden diese Einstellungen in den integrierten Bereitstellungsprofilen standardmäßig auf „False“ festgelegt, da für das Sammeln von Pod- und Knotenmetriken privilegierte Funktionen erforderlich sind.

## <a name="cu4-april-2020"></a><a id="cu4"></a> CU4 (April 2020)

Kumulatives Update 4-Release (CU4) für SQL Server 2019. Die Version der SQL Server-Datenbank-Engine für dieses Release ist 15.0.4033.1.

|Paketversion | Imagetag |
|-----|-----|
|15.0.4033.1 |[2019-CU4-ubuntu-16.04]

## <a name="cu3-march-2020"></a><a id="cu3"></a> CU3 (März 2020)

Kumulatives Update 3-Release (CU3) für SQL Server 2019. Die Version der SQL Server-Datenbank-Engine für dieses Release ist 15.0.4023.6.

|Paketversion | Imagetag |
|-----|-----|
|15.0.4023.6 |[2019-CU3-ubuntu-16.04]

### <a name="resolved-issues"></a>Behobene Probleme

Das kumulative Update 3 (CU3) von SQL Server 2019 behebt die folgenden Probleme früherer Releases.

- [Bereitstellung mit privatem Repository](#deployment-with-private-repository)
- [Beim Upgrade kann aufgrund eines Timeouts ein Fehler auftreten.](#upgrade-may-fail-due-to-timeout)

## <a name="cu2-february-2020"></a><a id="cu2"></a> CU 2 (Februar 2020)

Kumulatives Update 2-Release (CU2) für SQL Server 2019 Die Version der SQL Server-Datenbank-Engine für dieses Release ist 15.0.4013.40.

|Paketversion | Imagetag |
|-----|-----|
|15.0.4013.40 |[2019-CU2-ubuntu-16.04]

## <a name="cu1-january-2020"></a><a id="cu1"></a> Kumulatives Update 1 (Januar 2020)

Dies ist das CU1-Release für SQL Server 2019. Die Version der SQL Server-Datenbank-Engine für dieses Release ist 15.0.4003.23.

|Paketversion | Imagetag |
|-----|-----|
|15.0.4003.23|[2019-CU1-ubuntu-16.04]

## <a name="gdr1-november-2019"></a><a id="rtm"></a> Allgemeine Vertriebsversion 1 (November 2019)

Mit der allgemeinen Vertriebsversion 1 für SQL Server 2019 (GDR1) wird die allgemeine Verfügbarkeit für [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-nover.md)] eingeführt. Die Version der SQL Server-Datenbank-Engine für dieses Release ist 15.0.2070.34.

|Paketversion | Imagetag |
|-----|-----|
|15.0.2070.34|[2019-GDR1-ubuntu-16.04]

[!INCLUDE [sql-server-servicing-updates-version-15](../includes/sql-server-servicing-updates-version-15.md)]

## <a name="known-issues"></a>Bekannte Probleme

### <a name="partial-loss-of-logs-collected-in-elasticsearch-upon-rollback"></a>Partieller Verlust von Protokollen, die in ElasticSearch bei einem Rollback gesammelt wurden

- **Betroffene Releases:** Vorhandene Cluster bei einem fehlgeschlagenen Upgrade auf CU9-Ergebnisse in einem Rollback oder wenn ein Benutzer einen Downgrade auf ein älteres Release ausstellt

- **Problem und Kundenbeeinträchtigung:** Die für die elastische Suche verwendete Softwareversion wurde auf CU9 aktualisiert, und die neue Version ist nicht abwärtskompatibel mit dem vorherigen Protokollformat bzw. den Metadaten. Wenn die ElasticSearch-Komponente erfolgreich aktualisiert wird, aber ein späteres Rollback ausgelöst wird, gehen die zwischen dem ElasticSearch-Upgrade und dem Rollback gesammelten Protokolle dauerhaft verloren. Wenn Sie ein Downgrade auf eine ältere Version von BDC ausgeben (nicht empfohlen), gehen die in Elasticsearch gespeicherten Protokolle verloren. Beachten Sie Folgendes: Wenn der Benutzer ein Upgrade auf CU9 durchführt, werden die Daten wiederhergestellt.

- **Problemumgehung**: Bei Bedarf können Sie die Problembehandlung mithilfe von Protokollen über den Befehl `azdata bdc debug copy-logs` durchführen.

### <a name="missing-pods-and-container-metrics"></a>Fehlende Pods und Containermetriken

- **Betroffene Releases:** Vorhandene und neue Cluster beim Upgrade auf CU9

- **Problem und Kundenbeeinträchtigung:** Wenn Sie ein Upgrade der Version von Telegraf für die BDC-Überwachungselemente in CU9 durchführen, werden Sie beim Upgrade des Clusters auf das CU9-Release bemerken, dass Pods und Containermetriken nicht gesammelt werden. Dies liegt daran, dass eine zusätzliche Ressource in der Definition der Clusterrolle erforderlich ist, die als Ergebnis des Softwareupgrades für Telegraf verwendet wird. Wenn der Benutzer, der den Cluster bereitstellt oder das Upgrade durchführt, nicht die erforderlichen Berechtigungen hat, wird die Bereitstellung bzw. das Upgrade mit einer Warnung fortgesetzt und erfolgreich abgeschlossen. Allerdings werden dabei keine Pod- und Knotenmetriken gesammelt.

- **Problemumgehung**: Sie können einen Administrator bitten, die Rolle und das zugehörige Dienstkonto zu erstellen oder zu aktualisieren (vor oder nach der Bereitstellung bzw. dem Upgrade). Big Data-Cluster verwendet diese dann. [In diesem Artikel wird](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection) beschrieben, wie Sie die erforderlichen Artefakte erstellen.

### <a name="issuing-azdata-bdc-copy-logs-does-not-result-in-logs-being-copied"></a>Das Ausgeben von `azdata bdc copy-logs` führt nicht dazu, dass Protokolle kopiert werden.

- **Betroffene Releases**: [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]-Version *20.0.0*

- **Problem und Kundenbeeinträchtigung:** Bei der Implementierung des Befehls *copy-logs* wird davon ausgegangen, dass das Clienttool `kubectl` (Version 1.15 oder höher) auf dem Clientcomputer installiert ist, von dem der Befehl gesendet wird. Wenn `kubectl` Version 1.14 verwendet wird, wird der Befehl *azdata bdc debug copy-logs* ohne Fehler beendet, aber Protokolle werden nicht kopiert. Wenn Sie die Aktion mit dem *--Debug*-Flag ausführen, wird dieser Fehler in der Ausgabe angezeigt: *source "." is invalid* (Die Quelle "." ist ungültig).

- **Problemumgehung**: Installieren Sie das Tool `kubectl` Version 1.15 oder höher auf demselben Clientcomputer, und geben Sie den `azdata bdc copy-logs`-Befehl erneut aus. Informationen zum Installieren von `kubectl` finden Sie [unter diesem Link](deploy-big-data-tools.md).

### <a name="msdtc-capabilities-can-not-be-enabled-for-sql-server-master-instance-running-within-bdc"></a>MS DTC-Funktionen können nicht für die SQL Server-Masterinstanz aktiviert werden, die in Big Data-Clustern ausgeführt werden.

- **Betroffene Releases:** Alle Bereitstellungskonfigurationen für Big Data-Cluster, unabhängig vom Release

- **Problem und Kundenbeeinträchtigung:** Bei SQL Server-Bereitstellungen in Big Data-Clustern als SQL Server-Masterinstanz kann das MS DTC-Feature nicht aktiviert werden. Für diesen Fall gibt es keine Problemumgehung.

### <a name="ha-sql-server-database-encryption-key-encryptor-rotation"></a>Rotation der Verschlüsselungsschlüssel für SQL Server-Datenbanken mit Hochverfügbarkeit

- **Betroffene Releases:** Alle Versionen bis zu CU8. Behoben für CU9.

- **Problem und Kundenbeeinträchtigung:** Wenn SQL Server mit Hochverfügbarkeit bereitgestellt ist, schlägt die Zertifikatrotation für die verschlüsselte Datenbank fehl. Wenn der folgende Befehl für den Masterpool ausgeführt wird, wird eine Fehlermeldung angezeigt:
    ```
    ALTER DATABASE ENCRYPTION KEY
    ENCRYPTION BY SERVER
    CERTIFICATE <NewCertificateName>
    ```
    Dies hat keine Auswirkung, der Befehl schlägt fehl, und die Verschlüsselung der Zieldatenbank bleibt mithilfe des vorherigen Zertifikats erhalten.

### <a name="enabling-hdfs-encryption-zones-support-on-cu8"></a>Aktivieren der Unterstützung von HDFS-Verschlüsselungszonen bei CU8

- **Betroffene Releases**: Dieses Szenario tritt beim Upgrade von CU6 oder früheren Versionen auf CU8 auf. Bei neuen Bereitstellungen ab CU8 oder beim direkten Upgrade auf CU9 tritt dieses Szenario nicht auf.

- **Probleme und Auswirkungen für Kunden**: Die Unterstützung von HDFS-Verschlüsselungszonen ist in diesem Szenario standardmäßig nicht aktiviert und muss anhand der im [Konfigurationsleitfaden](encryption-at-rest-concepts-and-configuration.md) beschriebenen Schritte konfiguriert werden.

### <a name="empty-livy-jobs-before-you-apply-cumulative-updates"></a>Leere Livy-Aufträge vor dem Anwenden kumulativer Updates

- **Betroffene Releases:** Alle Versionen bis zu CU6. Behoben für CU8.

- **Problem und Kundenbeeinträchtigung:** Während eines Upgrades gibt `sparkhead` den Fehler 404 zurück.

- **Problemumgehung**: Stellen Sie vor dem Durchführen eines Upgrades von BDC sicher, dass keine aktiven Livy-Sitzungen oder -Batchaufträge vorhanden sind. Folgen Sie den unter [Upgrade von einem unterstützten Release](deployment-upgrade.md#upgrade-from-supported-release) angegebenen Anweisungen, um dies zu vermeiden. 

   Wenn Livy während des Upgradevorgangs den Fehler 404 zurückgibt, starten Sie den Livy-Server auf beiden `sparkhead`-Knoten neu. Beispiel:

   ```console
   kubectl -n <clustername> exec -it sparkhead-0/sparkhead-1 -c hadoop-livy-sparkhistory -- exec supervisorctl restart livy
   ```

### <a name="big-data-cluster-generated-service-accounts-passwords-expiration"></a>Ablauf der von Big Data-Clustern generierten Kennwörter für Dienstkonten

- **Betroffene Releases:** Alle Bereitstellungen von Big Data-Clustern mit Active Directory-Integration, unabhängig von der Version

- **Problem und Kundenbeeinträchtigung:** Während der Bereitstellung eines Big Data-Clusters generiert der Workflow eine Reihe von [Dienstkonten](active-directory-objects.md). Abhängig von der auf dem Domänencontroller festgelegten Kennwortablaufrichtlinie können die Kennwörter für diese Konten ablaufen (der Standardwert lautet 42 Tage). Derzeit gibt es keinen Mechanismus zum Rotieren von Anmeldeinformationen für alle Konten im BDC, sodass der Cluster nicht mehr funktionsfähig ist, sobald das Ablaufdatum erreicht ist.

- **Problemumgehung**: Aktualisieren Sie die Ablaufrichtlinie für die BDC-Dienstkonten auf dem Domänencontroller in „Kennwort läuft nie ab“. Eine vollständige Liste dieser Konten finden Sie unter [Automatisch generierte Active Directory-Objekte](active-directory-objects.md). Sie können diese Aktion vor oder nach der Ablaufzeit ausführen. Im letzteren Fall aktiviert Active Directory die abgelaufenen Kennwörter erneut.

### <a name="credentials-for-accessing-services-through-gateway-endpoint"></a>Anmeldeinformationen für den Zugriff auf Dienste über den Gatewayendpunkt

- **Betroffene Releases:** Neue Cluster, die ab CU5 bereitgestellt werden.

- **Problem und Kundenbeeinträchtigung:** Bei neuen Big Data-Clustern, die mithilfe von SQL Server 2019 CU5 bereitgestellt werden, ist der Gatewaybenutzername nicht **Root**. Wenn mithilfe der Anwendung mit den falschen Anmeldeinformationen eine Verbindung mit dem Gatewayendpunkt hergestellt wird, wird ein Authentifizierungsfehler angezeigt. Diese Änderung entsteht durch Ausführung von Anwendungen innerhalb des Big Data-Clusters, bei dem es sich nicht einen Root-Benutzer handelt. Ab SQL Server 2019 CU5 ist ein anderes Standardverhalten festgelegt: Wenn Sie mithilfe von CU5 einen neuen Big Data-Cluster bereitstellen, basiert der Benutzername des Gatewayendpunkts auf dem Wert, der von der Umgebungsvariablen **AZDATA_USERNAME** übergeben wird. Es handelt sich hierbei um denselben Benutzernamen, der für den Controller und die SQL Server-Endpunkte verwendet wird. Dies betrifft nur neue Bereitstellungen. Bestehende Big Data-Cluster mit einem der vorherigen Releases verwenden weiterhin **Root**. Es hat keine Auswirkungen auf die Anmeldeinformationen, wenn der Cluster für die Active Directory-Authentifizierung konfiguriert ist. 

- **Problemumgehung**: Azure Data Studio verarbeitet die Änderung der Anmeldeinformationen transparent für die Verbindung, die über das Gateway hergestellt wurde, um die HDFS-Suche im Objekt-Explorer zu ermöglichen. Sie müssen das [neueste Azure Data Studio-Release](../azure-data-studio/download-azure-data-studio.md) installieren, das die erforderlichen Änderungen enthält, die diesen Anwendungsfall berücksichtigen.
In anderen Szenarios, in denen Sie Anmeldeinformationen für den Zugriff auf den Dienst über das Gateway angeben müssen (z. B. Anmelden mit [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] oder Zugreifen auf Webdashboards für Spark), müssen Sie sicherstellen, dass die richtigen Anmeldeinformationen verwendet werden. Wenn Sie auf einen vorhandenen Cluster abzielen, der vor CU5 bereitgestellt wurde, verwenden Sie weiterhin den **Root**-Benutzernamen, um eine Verbindung mit dem Gateway herzustellen, auch nachdem Sie den Cluster auf CU5 aktualisiert haben. Wenn Sie einen neuen Cluster mithilfe des CU5-Builds bereitstellen, melden Sie sich mithilfe des Benutzernamens an, der zur Umgebungsvariablen **AZDATA_USERNAME** gehört.

### <a name="pods-and-nodes-metrics-not-being-collected"></a>Metriken für Pods und Knoten werden nicht erfasst

- **Betroffene Releases:** Neue und vorhandene Cluster, die CU5-Images verwenden

- **Problem und Kundenbeeinträchtigung:** Aufgrund eines Sicherheitsfixes im Zusammenhang mit der API, die `telegraf` verwendet hat, um Pod- und Hostknotenmetriken zu sammeln, stellen Kunden möglicherweise fest, dass die Metriken nicht erfasst werden. Dies ist sowohl in neuen als auch in bestehenden Bereitstellungen von Big Data-Cluster möglich (nach dem Upgrade auf CU5). Aufgrund des Fixes verlangt Telegraf nun ein Dienstkonto mit Rollenberechtigungen für den gesamten Cluster. Bei der Bereitstellung wird versucht, das erforderliche Dienstkonto und die Clusterrolle zu erstellen, aber wenn der Benutzer, der den Cluster bereitstellt oder das Upgrade durchführt, nicht die erforderlichen Berechtigungen hat, wird die Bereitstellung bzw. das Upgrade mit einer Warnung fortgesetzt und erfolgreich abgeschlossen. Allerdings werden dabei keine Pod- und Knotenmetriken gesammelt.

- **Problemumgehung**: Sie können einen Administrator bitten, die Rolle und das Dienstkonto zu erstellen (vor oder nach der Bereitstellung bzw. dem Upgrade). Big Data-Cluster verwendet diese dann. [In diesem Artikel wird](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection) beschrieben, wie Sie die erforderlichen Artefakte erstellen.

### <a name="azdata-bdc-copy-logs-command-failure"></a>Fehler beim Befehl `azdata bdc copy-logs`

- **Betroffene Releases**: [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]-Version *20.0.0*

- **Problem und Kundenbeeinträchtigung:** Bei der Implementierung des Befehls *copy-logs* wird davon ausgegangen, dass das Clienttool `kubectl` auf dem Clientcomputer installiert ist, von dem der Befehl gesendet wird. Wenn Sie den Befehl für eine auf OpenShift installierte Big Data-Cluster-Instanz ausführen, wird von Clients, auf denen nur das Tool `oc` installiert ist, der folgende Fehler ausgegeben: *An error occurred while collecting the logs: [WinError 2] The system cannot find the file specified* (Beim Sammeln der Protokolle ist ein Fehler aufgetreten: [WinError 2] Die angegebene Datei wurde nicht gefunden).

- **Problemumgehung**: Installieren Sie das Tool `kubectl` auf demselben Clientcomputer, und führen Sie den Befehl `azdata bdc copy-logs` noch mal aus. Informationen zum Installieren von `kubectl` finden Sie [unter diesem Link](deploy-big-data-tools.md).

### <a name="deployment-with-private-repository"></a>Bereitstellung mit privatem Repository

- **Betroffene Releases:** Allgemeine Vertriebsversion 1, Kumulatives Update 1 und 2 (CU1 und 2). Gelöst für CU 3.

- **Problem und Kundenbeeinträchtigung:** Beim Upgrade aus einem privaten Repository müssen bestimmte Anforderungen erfüllt werden.

- **Problemumgehung**: Wenn Sie ein privates Repository verwenden, um die Images für die Bereitstellung oder das Upgrade eines BDC vorab abzurufen, stellen Sie sicher, dass sich die aktuellen Buildimages sowie die Zielbuildimages im privaten Repository befinden. Dadurch kann bei Bedarf ein Rollback durchgeführt werden. Wenn Sie die Anmeldeinformationen des privaten Repositorys seit der ursprünglichen Bereitstellung geändert haben, müssen Sie zudem den entsprechenden geheimen Schlüssel in Kubernetes aktualisieren, bevor Sie das Upgrade durchführen. [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] bietet keine Unterstützung für das Aktualisieren der Anmeldeinformationen über die Umgebungsvariablen `AZDATA_PASSWORD` und `AZDATA_USERNAME`. Aktualisieren Sie den geheimen Schlüssel mithilfe von [`kubectl edit secrets`](https://kubernetes.io/docs/concepts/configuration/secret/#editing-a-secret). 

Ein Upgrade unter Verwendung unterschiedlicher privater Repositorys für den aktuellen Build und den Zielbuild wird nicht unterstützt.

### <a name="upgrade-may-fail-due-to-timeout"></a>Beim Upgrade kann aufgrund eines Timeouts ein Fehler auftreten.

- **Betroffene Releases:** Allgemeine Vertriebsversion 1, Kumulatives Update 1 und 2 (CU1 und 2). Gelöst für CU 3.

- **Problem und Kundenbeeinträchtigung:** Beim Upgrade kann aufgrund eines Timeouts ein Fehler auftreten.

   Der folgende Code zeigt, wie der Fehler aussehen könnte:

   ```
   >azdata.EXE bdc upgrade --name <mssql-cluster>
   Upgrading cluster to version 15.0.4003

   NOTE: Cluster upgrade can take a significant amount of time depending on
   configuration, network speed, and the number of nodes in the cluster.

   Upgrading Control Plane.
   Control plane upgrade failed. Failed to upgrade controller.
   ```

   Dieser Fehler tritt eher auf, wenn Sie das Upgrade des BDC in Azure Kubernetes Service (AKS) durchführen.

- **Problemumgehung**: Erhöhen Sie den Timeoutwert für das Upgrade. 

   Bearbeiten Sie die Konfigurationszuordnung für Upgrades, um die Timeoutwerte für Upgrades zu erhöhen. So bearbeiten Sie die Konfigurationszuordnung für Upgrades:

   1. Führen Sie den folgenden Befehl aus:

      ```bash
      kubectl edit configmap controller-upgrade-configmap
      ```

   2. Bearbeiten Sie die unten aufgeführten Felder:

       **`controllerUpgradeTimeoutInMinutes`** Gibt die Wartezeit in Minuten an, bis der Controller oder die Controllerdatenbank das Upgrade abgeschlossen hat. Der Standardwert ist 5. Aktualisieren Sie diesen Wert auf mindestens 20.

       **`totalUpgradeTimeoutInMinutes`** : Gibt die kombinierte Zeitdauer an, bis der Controller und die Controllerdatenbank das Upgrade abgeschlossen haben (Upgrade für `controller` + `controllerdb`). Der Standardwert ist 10. Aktualisieren Sie diesen Wert auf mindestens 40.

       **`componentUpgradeTimeoutInMinutes`** : Gibt die Zeitdauer an, in der jede nachfolgende Phase des Upgrades abgeschlossen sein muss. Der Standardwert ist 30. Aktualisieren Sie diesen Wert auf 45.

   3. Speichern Sie Ihre Angaben, und schließen Sie die Anwendung.

   Das unten gezeigte Python-Skript zeigt eine weitere Möglichkeit, um die Timeoutwerte festzulegen:

   ```python
   from kubernetes import client, config
   import json

   def set_upgrade_timeouts(namespace, controller_timeout=20, controller_total_timeout=40, component_timeout=45):
       """ Set the timeouts for upgrades

       The timeout settings are as follows

       controllerUpgradeTimeoutInMinutes: sets the max amount of time for the controller
           or controllerdb to finish upgrading

       totalUpgradeTimeoutInMinutes: sets the max amount of time to wait for both the
           controller and controllerdb to complete their upgrade

       componentUpgradeTimeoutInMinutes: sets the max amount of time allowed for
           subsequent phases of the upgrade to complete
       """
       config.load_kube_config()

       upgrade_config_map = client.CoreV1Api().read_namespaced_config_map("controller-upgrade-configmap", namespace)

       upgrade_config = json.loads(upgrade_config_map.data["controller-upgrade"])

       upgrade_config["controllerUpgradeTimeoutInMinutes"] = controller_timeout

       upgrade_config["totalUpgradeTimeoutInMinutes"] = controller_total_timeout

       upgrade_config["componentUpgradeTimeoutInMinutes"] = component_timeout

       upgrade_config_map.data["controller-upgrade"] = json.dumps(upgrade_config)

       client.CoreV1Api().patch_namespaced_config_map("controller-upgrade-configmap", namespace, upgrade_config_map)
   ```

### <a name="livy-job-submission-from-azure-data-studio-ads-or-curl-fail-with-500-error"></a>Die Übermittlung von Livy-Aufträgen von Azure Data Studio oder der curl-Befehl schlägt mit dem Fehler 500 fehl

- **Problem und Kundenbeeinträchtigung:** In einer Hochverfügbarkeitskonfiguration werden freigegebene Spark-Ressourcen (`sparkhead`) mit mehreren Replikaten konfiguriert. In diesem Fall können Fehler bei der Livy-Auftragsübermittlung von Azure Data Studio oder `curl` auftreten. Dies können Sie überprüfen, indem Sie `curl` auf einen beliebigen `sparkhead`-Pod ausführen. Dies resultiert dann in einer abgelehnten Verbindung. Beispielsweise geben `curl https://sparkhead-0:8998/` oder `curl https://sparkhead-1:8998` den Fehler 500 zurück.

   Dies geschieht in den folgenden Szenarios:

   - Zookeeper-Pods oder Prozesse einzelner Zookeeper-Instanzen werden mehrmals neu gestartet.
   - Wenn die Netzwerkkonnektivität zwischen `sparkhead`- und Zookeeper-Pods unzuverlässig ist.

- **Problemumgehung**: Beide Livy-Server müssen neu gestartet werden.

   ```bash
   kubectl -n <clustername> exec sparkhead-0 -c hadoop-livy-sparkhistory supervisorctl restart livy
   ```

   ```bash
   kubectl -n <clustername> exec sparkhead-1 -c hadoop-livy-sparkhistory supervisorctl restart livy
   ```

### <a name="create-memory-optimized-table-when-master-instance-in-an-availability-group"></a>Erstellen einer arbeitsspeicheroptimierten Tabelle, wenn die Masterinstanz sich in einer Verfügbarkeitsgruppe befindet

- **Problem und Kundenbeeinträchtigung:** Sie können den primären Endpunkt, der zum Herstellen einer Verbindung mit Datenbanken in Verfügbarkeitsgruppen (Listener) zur Verfügung gestellt wird, nicht zum Erstellen arbeitsspeicheroptimierter Tabellen verwenden.

- **Problemumgehung**: [Stellen Sie eine Verbindung mit der SQL Server-Instanz her](deployment-high-availability.md#instance-connect), stellen Sie einen Endpunkt zur Verfügung, stellen Sie eine Verbindung mit der SQL Server-Datenbank her, und erstellen Sie die arbeitsspeicheroptimierten Tabellen in der mit der neuen Verbindung erstellten Sitzung, um arbeitsspeicheroptimierte Tabellen zu erstellen, wenn die SQL Server-Masterinstanz sich in einer Verfügbarkeitsgruppe befindet.

### <a name="insert-to-external-tables-active-directory-authentication-mode"></a>Einfügen in externe Tabellen im Active Directory-Authentifizierungsmodus

- **Problem und Kundenbeeinträchtigung:** Wenn die SQL Server-Masterinstanz sich im Active Directory-Authentifizierungsmodus befindet, gibt eine Abfrage, die nur aus externen Tabellen auswählt, von denen sich mindestens eine in einem Speicherpool befindet, und in eine andere externe Tabelle einfügt, Folgendes zurück:

   ```
   Msg 7320, Level 16, State 102, Line 1
   Cannot execute the query "Remote Query" against OLE DB provider "SQLNCLI11" for linked server "SQLNCLI11". Only domain logins can be used to query Kerberized storage pool.
   ```

- **Problemumgehung**: Passen Sie die Abfrage auf einer der folgenden Weisen an. Verknüpfen Sie die Speicherpooltabelle mit einer lokalen Tabelle, oder fügen Sie zunächst in die lokale Tabelle ein, und lesen Sie dann aus der lokalen Tabelle, um etwas in den Datenpool einzufügen.

### <a name="transparent-data-encryption-capabilities-can-not-be-used-with-databases-that-are-part-of-the-availability-group-in-the-sql-server-master-instance"></a>Transparent Data Encryption-Funktionen können nicht mit Datenbanken verwendet werden, die Teil der Verfügbarkeitsgruppe in der SQL Server-Masterinstanz sind.

- **Problem und Kundenbeeinträchtigung:** In Hochverfügbarkeitskonfigurationen können Datenbanken mit aktivierter Verschlüsselung nach einem Failover nicht verwendet werden, da sich die für die Verschlüsselung verwendeten Hauptschlüssel auf den einzelnen Replikaten unterscheiden. 

- **Problemumgehung**: Für dieses Szenario gibt es keine Problemumgehung. Es wird empfohlen, die Verschlüsselung in dieser Konfiguration erst zu aktivieren, wenn ein Fix vorhanden ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).