---
title: Bereitgestellte Ressourcen
titleSuffix: SQL Server Big Data Clusters
description: In diesem Artikel werden die Pods beschrieben, die in der Regel in einem Big Data-Cluster für SQL Server bereitgestellt werden.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 20f31d6cbf4bcdbbdc666b50dea626f79e298db1
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343935"
---
# <a name="resources-deployed-with-big-data-cluster"></a>Mit dem Big Data-Cluster bereitgestellte Ressourcen

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Artikel werden die Ressourcen beschrieben, die von einem Big Data-Cluster für SQL Server bereitgestellt werden.

Mit einem Big Data-Cluster werden Pods basierend auf dem Bereitstellungsprofil bereitgestellt. Weitere Informationen finden Sie unter [Standardkonfigurationen](deployment-guidance.md#configfile). 

In diesem Artikel werden die Pods beschrieben, die mit dem Profil `aks-dev-test-ha` bereitgestellt werden. Zudem erhalten Sie Informationen über einen Spark-Pool. Führen Sie eine Abfrage für Kubernetes durch, um die in Ihrem Cluster bereitgestellten Pods anzuzeigen. Im folgenden Beispiel wird eine Liste von Pods unter einem bestimmten Namespace zurückgegeben.

```bash
kubectl get pods -n <namespace>
```

Ersetzen Sie `<namespace>` durch den Namen des Big Data-Clusters. 

Weitere Informationen finden Sie unter [Vorgehensweise: Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] auf Kubernetes](deployment-guidance.md#configfile).

Das folgende Diagramm zeigt die in einem Big Data-Cluster bereitgestellten Komponenten an:

:::image type="content" source="media/big-data-cluster-overview/architecture-diagram-overview.png" alt-text="Diagramm: Big Data-Cluster":::

Weitere Informationen zur Architektur finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md).

## <a name="deployed-pods"></a>Bereitgestellte Pods

In der folgenden Tabelle sind die in einem Big Data-Cluster bereitgestellten Pods aufgeführt.

|Name  |Bereich|
|---------|---------|
|`control-<nnnn>`        |[Steuerung](#control)|
|`controldb-<#>`         |[Steuerung](#control)|
|`controlwd-<nnnn>`      |[Steuerung](#control)|
|`logsdb-<#>`            |[Steuerung](#control)|
|`logsui-<nnnn>`         |[Steuerung](#control)|
|`metricsdb-<#>`         |[Steuerung](#control)|
|`metricsdc-<nnnn>`      |[Steuerung](#control)|
|`metricsui-<nnnn>`      |[Steuerung](#control)|
|`mgmtproxy-<nnnn>`      |[Steuerung](#control)|
|`zookeeper-<#>`         |[Steuerung](#control)|
|`dns-<nnnn>`            |[Steuerung](#control)|
|`master-<#n>`           |[Masterinstanz](#master-instance)|
|`operator-<nnnn>`       |[Masterinstanz](#master-instance)
|`compute-<#n>-<#m>`     |[Computepool](#compute-pool)|
|`data-<#>-<#>`          |[Datenpool](#data-pool) |
|`storage-<#>-<#>`       |[Speicherpool](#storage-pool)|
|`nmnode-<#>-<#>`        |[Speicherpool](#storage-pool)|
|`sparkhead-<#>`         |[Speicherpool](#storage-pool)|
|`appproxy-<#m>`         |[Anwendungspool](#application-pool)|
|`gateway-<#>`           |[Gatewaydienst](#gateway-service)|

Nicht alle Pods sind in jedem Big Data-Cluster enthalten. Bereitstellungen mit Hochverfügbarkeit oder Active Directory-Integrationen beinhalten spezifische Pods. 

### <a name="high-availability-specific-pods"></a>Spezifische Pods für Hochverfügbarkeit:

- `operator-<nnnn>`
- `zookeeper-<#>`

### <a name="active-directory-specific-pods"></a>Spezifische Pods für Active Directory:

- `dns-<nnnn>`

In den folgenden Abschnitten werden die Pods beschrieben und die Container in jedem Pod aufgelistet.

## <a name="control"></a>Control

Steuerungspods stellen den Steuerungsdienst bereit.

|Podname |Anzahl| Kubernetes-Controllertyp | Container |
|--------|----|------|--------|-------|
|`control-#`|1| ReplicaSet |- `controller`<br><br>- `security-support`<br><br>- `fluentbit`
|`controldb`|1| StatefulSet |- `mssql-server`<br><br>- `fluentbit`
|`controlwd`|1|  ReplicaSet |- `controlwatchdog`
|`logsdb-#` |1| StatefulSet |- `elasticsearch`
|`logsui`   |1| ReplicaSet |- `kibana`
|`metricsdb-#`|1| StatefulSet |- `influxdb`
|`metricsdc`|1 pro Kubernetes-Knoten | DaemonSet |- `telegraf` |
|`metricsui-nnnn`|1| ReplicaSet |- `grafana` |
|`mgmtproxy-nnnn`|1| ReplicaSet |- `service-proxy`<br><br>- `fluentbit`|
|`dns-nnnn`|0 oder 1 für die Azure Active Directory-Integration| ReplicaSet |- `dns`<br><br>- `fluentbit`|

## <a name="master-instance"></a>Master-Instanz

Bei `master-<#n>` handelt es sich um die SQL Server-Masterinstanz.

- Verwaltet den Datenpool über DDL
- Bearbeitet Daten im Daten Pool über DML
- Lagert die analytische Abfrageausführung in den Datenpool aus

|Podname |Anzahl| Kubernetes-Controllertyp | Container |
|--------|----|------|--------|
|`master-<#n>`|Mindestens 1 für Hochverfügbarkeit| StatefulSet|- `mssql-server`<br><br>- `fluentbit`<br><br>- `collectd`<br><br>- `mssql-ha-supervisor` <sup>*</sup>|
|`operator`<sup>*</sup>| 0 oder 1 für Hochverfügbarkeit | ReplicaSet |- `mssql-ha-operator`

<sup>*</sup> Nur für Bereitstellungen mit Hochverfügbarkeit. Der Operator implementiert und registriert die benutzerdefinierte Ressourcendefinition für SQL Server und die Verfügbarkeitsgruppenressourcen. Wenn der Operator bereitgestellt wird, registriert er sich selbst als Listener für Benachrichtigungen über SQL Server-Ressourcen, die im Kubernetes-Cluster bereitgestellt werden. `mssql-ha-supervisor` unterstützt die Verfügbarkeitsgruppe.

Jeder `master`-Pod enthält eine Instanz von SQL Server. Eine Bereitstellung mit Hochverfügbarkeit umfasst 3 Pods. Jeder Pod enthält eine SQL Server-Instanz mit Datenbanken in einer SQL Server-Always On-Verfügbarkeitsgruppe.

Schließen Sie je nach Arbeitsauslastung zum Zeitpunkt der Bereitstellung zusätzliche Pods ein. 

## <a name="compute-pool"></a>Computepool

Der Computepool stellt eine SQL Server-Instanz für die Berechnung bereit.

|Podname |Anzahl| Kubernetes-Controllertyp | Container |
|--------|----|------|--------|
|`compute-<#n>-<#m>`|Mindestens einer| StatefulSet |- `mssql-server`<br><br>- `fluentbit`<br><br>- `collectd`

- `#n` identifiziert den Computepool.
- `#m` identifiziert die Instanz-ID innerhalb des Pools.

Die SQL Server-Instanzen des Computepools sind zustandslos. Sie benötigen lediglich Speicher für `tempdb`.

Schließen Sie je nach Arbeitsauslastung zum Zeitpunkt der Bereitstellung zusätzliche Pods ein. 

## <a name="data-pool"></a>Datenpool

Der Datenpool bietet SQL Server-Instanzen für die Speicherung und das Computing.

|Podname |Anzahl| Kubernetes-Controllertyp | Container |
|--------|----|------|--------|-------|
|`data-<#n>-<#m>` | 0 oder mehr | StatefulSet | -` mssql-server` <br><br>- `fluentbit`<br><br>- `collectd`|

- `#n` identifiziert den Datenpool.
- `#m` identifiziert die Instanz-ID innerhalb des Pools.

Schließen Sie je nach Arbeitsauslastung zum Zeitpunkt der Bereitstellung zusätzliche Pods ein.

## <a name="storage-pool"></a>Speicherpool

Der Speicherpool ermöglicht die Datenerfassung über Spark, den Speicher in HDFS, den Datenzugriff über HDFS und SQL Server-Endpunkte.

|Podname |Anzahl| Kubernetes-Controllertyp | Container |
|--------|----|------|--------|
|`storage-0-#`|Mindestens einer Schließen Sie je nach Arbeitsauslastung zum Zeitpunkt der Bereitstellung zusätzliche Pods ein. | StatefulSet |- `hadoop`<br><br>- `mssql-server`<br><br>- `fluentbit`<br><br>
|`nmnode-0-#`|Mindestens 1 für Hochverfügbarkeit| StatefulSet |- `hadoop`<br><br>- `fluentbit`
|`sparkehead-#`|Mindestens 1 für Hochverfügbarkeit| StatefulSet |- `hadoop-yarn-jobhistory`<br><br>- `hadoop-livy-sparkhistory`<br><br>- `hadoop-hivemetastore`<br><br>-- `fluentbit`
|`zookeeper`|0 oder 3 für Hochverfügbarkeit | StatefulSet |- `zookeeper`<br><br>- `fluentbit`

## <a name="application-pool"></a>Anwendungspool

Der Anwendungspool ist in einigen der Testkonfigurationsprofile enthalten. Der Anwendungspool hostet Anwendungsdienstproxys, die Sie definieren, wenn Sie Ihre Anwendungen für Big Data-Cluster bereitstellen. 

`appproxy` ist eine Web-API, die sich vor den Anwendungen des Anwendungspools befindet. Sie authentifiziert Benutzer und leitet die Anforderungen dann an die Anwendungen weiter.

|Podname | Kubernetes-Controllertyp | Container  |
|--------|----|------|
|`appproxy`| ReplicaSet |- `app-service-proxy`<br><br>- `fluentbit`

Weitere Informationen finden Sie unter [Was hat es mit der Anwendungsbereitstellung in einem Big Data-Cluster auf sich?](concept-application-deployment.md).

Schließen Sie je nach Arbeitsauslastung zum Zeitpunkt der Bereitstellung zusätzliche Pods ein. 

## <a name="gateway-service"></a>Gatewaydienst

Der Gatewaydienst stellt das Knox-Gateway zu Spark, HDFS, [Yarn](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), zur Yarn-Benutzeroberfläche und zur Spark-Benutzeroberfläche bereit.

|Podname | Kubernetes-Controllertyp | Container |
|--------|-----|-----|
|`gateway-<#>`| StatefulSet |- `knox`<br><br>- `fluentbit`

Es wird nur ein Gateway unterstützt.

## <a name="open-source-container-references"></a>Referenzen zu Open-Source-Containern

Informationen zu bestimmten Open-Source-Projekten und -Versionen finden Sie unter [Referenz zu Open-Source-Software](reference-open-source-software.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie in den folgenden Ressourcen:

- [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Workshop: Microsoft [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]-Architektur](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)
- [Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md#configfile)
