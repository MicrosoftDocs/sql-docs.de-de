---
title: Konfiguration von SQL Server Big Data-Clustern vor CU9
titleSuffix: SQL Server big data clusters
description: Konfiguration von Big Data-Clustern vor CU9
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b00ed57288d19f08555a00eec8c9e62edc0f8cf6
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343968"
---
# <a name="configure-a-sql-server-big-data-cluster---pre-cu9-release"></a>Konfigurieren eines SQL Server Big Data-Clusters vor CU9

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

> [!NOTE]
> Vor CU9 und bevor Cluster unterstützt wurden, bei denen eine Konfiguration möglich ist, konnten Big Data-Cluster mit Ausnahme der SQL Server-Masterinstanz nur zum Zeitpunkt der Bereitstellung konfiguriert werden. Die SQL Server-Masterinstanz konnte nur mithilfe der Datei „mssql-conf“ nach der Bereitstellung konfiguriert werden. Anleitungen zum Konfigurieren von Big Data-Clustern ab CU9 finden Sie [hier](configure-bdc-overview.md).


Bei Big Data-Clustern bis CU8 können Sie BDC-Einstellungen zum Zeitpunkt der Bereitstellung über die Bereitstellungsdatei `bdc.json` konfigurieren. Die SQL Server-Masterinstanz kann nach der Bereitstellung nur mithilfe der Datei „mssql-conf“ konfiguriert werden.

## <a name="configuration-scopes"></a>Konfigurationsebenen
Big Data-Cluster vor CU9 können auf zwei Ebenen konfiguriert werden: `service` und `resource`. Auch die Hierarchie der Einstellungen entspricht dieser Reihenfolge, von oben nach unten. BDC-Komponenten übernehmen den Wert der Einstellung auf, der auf der niedrigsten Ebene definiert ist. Wenn die Einstellung nicht auf einer bestimmten Ebene definiert ist, wird der Wert der übergeordneten Ebene übernommen.

Angenommen, Sie möchten die Standardanzahl der Kerne definieren, die der Spark-Treiber im Speicherpool und in `Sparkhead`-Ressourcen verwenden soll. Hierzu stehen zwei Möglichkeiten zur Verfügung:

* Standardwert für Kerne auf der Dienstebene `Spark` angeben 
* Standardwert für Kerne auf der Ressourcenebene `storage-0` und `sparkhead` angeben

Im ersten Szenario *übernehmen* alle Ressourcen auf niedrigeren Ebenen des Spark-Diensts (Speicherpool und `Sparkhead`) die Standardanzahl der Kerne vom Standardwert des Spark-Diensts.

Im zweiten Szenario nutzt jede Ressource den auf ihrer jeweiligen Ebene definierten Wert.

Wenn die Standardanzahl von Kernen sowohl auf Dienst- als auch auf Ressourcenebene konfiguriert ist, überschreibt der Wert für die Ressourcenebene den Wert für die Dienstebene, da dies die niedrigste **vom Benutzer konfigurierte** Ebene für die gegebene Einstellung ist.

Spezifische Informationen zur Konfiguration finden Sie in den folgenden entsprechenden Artikeln:

## <a name="configure-the-sql-server-master-instance"></a>Konfigurieren der SQL Server-Masterinstanz
Konfigurieren Sie die Masterinstanz von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].

Die Serverkonfigurationseinstellungen der SQL Server-Masterinstanz können nicht zum Zeitpunkt der Bereitstellung konfiguriert werden. In diesem Artikel wird eine vorübergehende Problemumgehung beschrieben, mit der Einstellungen wie die SQL Server-Edition, das Aktivieren oder Deaktivieren des SQL Server-Agents, das Aktivieren spezifischer Ablaufverfolgungsflags oder das Aktivieren oder Deaktivieren von Kundenfeedback konfiguriert werden können.

Führen Sie die folgenden Schritte aus, um diese Einstellungen zu konfigurieren:

1. Erstellen Sie eine benutzerdefinierte `mssql-custom.conf`-Datei, die die gewünschten Einstellungen enthält. Im folgenden Beispiel werden der SQL Server-Agent, die Telemetrie und das Ablaufverfolgungsflag 1204 aktiviert sowie eine PID für die Enterprise Edition festgelegt:

   ```
   [sqlagent]
   enabled=true
   
   [telemetry]
   customerfeedback=true
   userRequestedLocalAuditDirectory = /tmp/audit

   [DEFAULT]
   pid = Enterprise

   [traceflag]
   traceflag0 = 1204
   ```

1. Kopieren Sie die `mssql-custom.conf`-Datei in `/var/opt/mssql` im `mssql-server`-Container, der sich im `master-0`-Pod befindet. Ersetzen Sie `<namespaceName>` durch den Namen des Big Data-Clusters.

   ```bash
   kubectl cp mssql-custom.conf master-0:/var/opt/mssql/mssql-custom.conf -c mssql-server -n <namespaceName>
   ```

1. Starten Sie die SQL Server-Instanz neu.  Ersetzen Sie `<namespaceName>` durch den Namen des Big Data-Clusters.

   ```bash
   kubectl exec -it master-0  -c mssql-server -n <namespaceName> -- /bin/bash
   supervisorctl restart mssql-server
   exit
   ```

> [!IMPORTANT]
> Wenn die SQL Server-Masterinstanz eine Verfügbarkeitsgruppenkonfiguration aufweist, kopieren Sie die `mssql-custom.conf`-Datei in alle `master`-Pods. Beachten Sie, dass jeder Neustart ein Failover auslöst. Sie müssen also sicherstellen, dass Sie diese Aktivität während Downtime durchführen.

### <a name="known-limitations"></a>Bekannte Einschränkungen

- Für die oben genannten Schritte sind Administratorberechtigungen für den Kubernetes-Cluster erforderlich.
- Sie können die Serversortierung für die SQL Server-Masterinstanz nach der Bereitstellung des Big Data-Clusters nicht ändern.

## <a name="configure-apache-spark-and-apache-hadoop"></a>Konfigurieren von Apache Spark und Apache Hadoop
Zum Konfigurieren von Apache Spark und Apache Hadoop in Big Data-Clustern müssen Sie das Clusterprofil zum Zeitpunkt der Bereitstellung ändern.

Ein Big Data-Cluster verfügt über vier Konfigurationskategorien: 

- `sql` 
- `hdfs` 
- `spark` 
- `gateway` 

`sql`, `hdfs`, `spark` und `sql` sind Dienste. Diese Dienste werden der jeweils gleichen benannten Konfigurationskategorie zugeordnet. Alle Gatewaykonfigurationen werden in Kategorie `gateway` eingeordnet. 

Sämtliche Konfigurationen im Dienst `hdfs` zählen beispielsweise zu Kategorie `hdfs`. Beachten Sie, dass alle Konfigurationen für Hadoop (core-site), HDFS und Zookeeper zur Kategorie `hdfs` zählen und alle Konfigurationen für Livy, Spark, Yarn und Hive Metastore zur Kategorie `spark`. 

Unter [Unterstützte Konfigurationen](reference-config-spark-hadoop.md#supported-configurations) sind die Eigenschaften von Apache Spark und Apache Hadoop aufgeführt, die Sie beim Bereitstellen eines Big Data-Clusters in SQL Server konfigurieren können.

In den folgenden Abschnitten sind die Eigenschaften aufgeführt, die Sie in einem Cluster **nicht** ändern können:

- [Nicht unterstützte `spark`-Konfigurationen](reference-config-spark-hadoop.md#unsupported-spark-configurations)
- [Nicht unterstützte `hdfs`-Konfigurationen](reference-config-spark-hadoop.md#unsupported-hdfs-configurations)
- [Nicht unterstützte `gateway`-Konfigurationen](reference-config-spark-hadoop.md#unsupported-gateway-configurations)

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines Big Data-Clusters in SQL Server](configure-bdc-overview.md)