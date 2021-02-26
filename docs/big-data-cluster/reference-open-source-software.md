---
title: Referenz zu Open-Source-Software
titleSuffix: SQL Server Big Data Clusters
description: Ermitteln der in SQL Server Big Data-Clustern verwendeten Open-Source-Software und der entsprechenden.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 210787512ea3ded5aaab660c8973482bb13c2ff4
ms.sourcegitcommit: 6c93282cce1216dac327cb28848a3ab4d51b776e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2021
ms.locfileid: "100646362"
---
# <a name="open-source-software-reference"></a>Referenz zu Open-Source-Software

[!INCLUDE [sqlserver2019](../includes/applies-to-version/sqlserver2019.md)]

Ein SQL Server Big Data-Cluster enthält einige im Rahmen von Open-Source-Projekten entwickelte Container. In diesem Artikel werden bestimmte Projekte und Versionen dieser Container beschrieben.

## <a name="project-list"></a>Projektliste

In der folgenden Tabelle sind die Open-Source-Projekte aufgeführt, die bis [!INCLUDE [sssql19-md](../includes/sssql19-md.md)] CU8 und früher und ab CU9 und später verwendet werden. 

| Project | CU8 und früher | Ab CU9 |
|--|--|--|
| [collectd](https://collectd.org/) | 5.8.1 | 5.12 |
| [InfluxDB](https://www.influxdata.com) | 1.7.6 | 1.8.3 |
| [Elasticsearch](https://www.elastic.co/) | 7.0.1 | 7.9.1 |
| [Fluent Bit](https://docs.fluentbit.io/manual/about/what-is-fluent-bit) | 1.1.1 | 1.6.3 |
| [Grafana](https://grafana.com/) | 6.3.6 | 7.3.1 |
| Hadoop <br/>[HDFS DataNode](concept-storage-pool.md)<br/>[HDFS NameNode](https://cwiki.apache.org/confluence/display/HADOOP2/NameNode) |3.1.3+|3.3.0|
| [Hive (Metastore)](https://hive.apache.org/) |2.3.7|2.3.7<br/>3.0.0 (eigenständig)<br/>3.1.2 (Struktur)|
| [Kibana](https://www.elastic.co/kibana) | 7.0.1 | 7.9.1 |
| [Knox](https://knox.apache.org/) |1.2.0|1.4.0|
| [Livy](https://livy.apache.org/) |0.6.0|0.7.0|
| [opendistro-elasticsearch-security](https://www.elastic.co/what-is/elastic-stack-security) | 1.0.0.1 | 1.10.1.0 |
| [Openresty (Nginx)](https://openresty.org/) | 1.15.8 | 1.17.8.2 |
| [Spark](configure-spark-hdfs.md) |2.4.6 und höher|2.4.10|
| [Telegraf-](https://docs.influxdata.com/telegraf/) | 1.10.3 | 1.16.1 |
| [ZooKeeper](https://cwiki.apache.org/confluence/display/zookeeper) |3.5.8|3.6.2

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Mit dem Big Data-Cluster bereitgestellte Ressourcen](concept-architecture-pods.md).