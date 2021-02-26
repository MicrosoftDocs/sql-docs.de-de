---
title: Konfigurationseigenschaften für SQL Server Big Data-Cluster
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu Konfigurationseigenschaften für Big Data-Cluster
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ecaba704d9c08619f42c5cdf8d726917ccc61b9c
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343539"
---
# <a name="sql-server-big-data-clusters-configuration-properties"></a>Konfigurationseigenschaften für SQL Server Big Data-Cluster

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Die Konfigurationseinstellungen für Big Data-Cluster können auf den folgenden Ebenen definiert werden: `cluster`, `service` und `resource`. Auch die Hierarchie der Einstellungen entspricht dieser Reihenfolge, von oben nach unten. BDC-Komponenten übernehmen den Wert der Einstellung auf, der auf der niedrigsten Ebene definiert ist. Wenn die Einstellung nicht auf einer bestimmten Ebene definiert ist, wird der Wert der übergeordneten Ebene übernommen. Im Folgenden sind die Einstellungen aufgeführt, die für die einzelnen Komponenten von BDCs auf den verschiedenen Ebenen verfügbar sind. Die Konfigurationseinstellungen für Ihren BDC können Sie auch mithilfe von azdata anzeigen.

## <a name="bdc-cluster-scope-settings"></a>BDC-Einstellungen auf Clusterebene
Folgende Einstellungen können auf Clusterebene konfiguriert werden.

|Eigenschaft|Optionen|
| --- | --- |
|`mssql.telemetry`|`customerfeedback = { true | false }` |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="sql-service-scope-settings"></a>Einstellungen auf der Dienstebene von SQL
Folgende Einstellungen können auf der Dienstebene von SQL konfiguriert werden.

|Eigenschaft|Optionen|
| --- | --- |
|`mssql.language`|`lcid = <language_identifier>` |

## <a name="spark-service-scope-settings"></a>Einstellungen auf der Dienstebene von Spark
Im Artikel [Konfigurationseigenschaften von Apache Spark und Apache Hadoop (HDFS)](reference-config-spark-hadoop.md) sind alle unterstützten und nicht unterstützten Einstellungen aufgeführt.

## <a name="hdfs-service-scope-settings"></a>Einstellungen auf der Dienstebene von HDFS
Im Artikel [Konfigurationseigenschaften von Apache Spark und Apache Hadoop (HDFS)](reference-config-spark-hadoop.md) sind alle unterstützten und nicht unterstützten Einstellungen aufgeführt.

## <a name="gateway-service-scope-settings"></a>Einstellungen auf der Dienstebene des Gateways
Auf der Dienstebene des Gateways sind keine Einstellungen konfigurierbar. Konfigurieren Sie die Einstellungen auf der Ressourcenebene des Gateways.

## <a name="app-service-scope-settings"></a>Einstellungen auf der Dienstebene der App
Keine verfügbar

## <a name="master-pool-resource-scope-settings"></a>Einstellungen auf der Ressourcenebene des Masterpools
|Eigenschaft|Optionen|
| --- | --- |
|`mssql.sqlagent`|`enabled = { true | false }` |
|`mssql.licensing`|`pid = { Enterprise | Developer }` |
<!-- |`mssql.collation`|`x = <language_identifier>` | -->

> [!NOTE]
> Die Änderung der Standardsortierung für eine Instanz von SQL Server ist ein komplexer Vorgang. Dabei muss nicht nur die `mssql.collation`-Einstellung geändert werden. Möglicherweise müssen auch die Benutzerdatenbanken und alle darin enthaltenen Objekte neu erstellt werden. Eine Anleitung hierzu finden Sie unter [Festlegen oder Ändern der Serversortierung](../relational-databases/collations/set-or-change-the-server-collation.md#changing-the-server-collation-in-sql-server)

## <a name="storage-pool-resource-scope-settings"></a>Einstellungen auf der Ressourcenebene des Speicherpools
Der Speicherpool besteht aus SQL-, Spark- und HDFS-Komponenten.

### <a name="available-sql-configurations"></a>Verfügbare SQL-Konfigurationen
|Eigenschaft|Optionen|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.storagePoolCacheSize`| |
|`mssql.storagePoolMaxCacheSize`| |
|`mssql.storagePoolCacheAutogrowth`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |


### <a name="available-apache-spark-and-hadoop-configurations"></a>Verfügbare Apache Spark- und Hadoop-Konfigurationen
Im Artikel [Konfigurationseigenschaften von Apache Spark und Apache Hadoop (HDFS)](reference-config-spark-hadoop.md) sind alle unterstützten und nicht unterstützten Einstellungen aufgeführt.

## <a name="data-pool-resource-scope-settings"></a>Einstellungen auf der Ressourcenebene des Datenpools
|Eigenschaft|Optionen|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="compute-pool-resource-scope-settings"></a>Einstellungen auf der Ressourcenebene des Computepools
|Eigenschaft|Optionen|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="spark-pool-resource-scope-settings"></a>Einstellungen auf der Ressourcenebene des Spark-Pools
Im Artikel [Konfigurationseigenschaften von Apache Spark und Apache Hadoop (HDFS)](reference-config-spark-hadoop.md) sind alle unterstützten und nicht unterstützten Einstellungen aufgeführt.

## <a name="gateway-resource-scope-settings"></a>Einstellung auf der Ressourcenebene des Gateways
Im Artikel [Konfigurationseigenschaften von Apache Spark und Apache Hadoop (HDFS)](reference-config-spark-hadoop.md) sind alle unterstützten und nicht unterstützten Einstellungen aufgeführt.

## <a name="sparkhead-resource-scope-settings"></a>Einstellung auf der Ressourcenebene von `Sparkhead`
Im Artikel [Konfigurationseigenschaften von Apache Spark und Apache Hadoop (HDFS)](reference-config-spark-hadoop.md) sind alle unterstützten und nicht unterstützten Einstellungen aufgeführt.

## <a name="zookeeper-resource-scope-settings"></a>Einstellungen auf der Ressourcenebene von Zookeeper
Im Artikel [Konfigurationseigenschaften von Apache Spark und Apache Hadoop (HDFS)](reference-config-spark-hadoop.md) sind alle unterstützten und nicht unterstützten Einstellungen aufgeführt.

## <a name="namenode-resource-scope-settings"></a>Einstellung auf der Ressourcenebene von Namenode
Im Artikel [Konfigurationseigenschaften von Apache Spark und Apache Hadoop (HDFS)](reference-config-spark-hadoop.md) sind alle unterstützten und nicht unterstützten Einstellungen aufgeführt.

## <a name="app-proxy-resource-scope-settings"></a>Einstellung auf der Ressourcenebene eines App-Proxys
Keine verfügbar

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines SQL Server Big Data-Clusters](configure-bdc-overview.md)