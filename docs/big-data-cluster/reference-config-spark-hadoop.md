---
title: Konfigurationseigenschaften von Apache Spark und Apache Hadoop
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu den Konfigurationseigenschaften von Apache Spark und Apache Hadoop (HDFS)
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 08/04/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c6a73d8cf4a110990260db4917d565c33bd59766
ms.sourcegitcommit: 765262cdc6352a5325148afc22fa4f1499fe1aa3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514887"
---
# <a name="apache-spark--apache-hadoop-hdfs-configuration-properties"></a>Konfigurationseigenschaften von Apache Spark und Apache Hadoop (HDFS)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Big Data-Cluster unterstützt während und nach der Bereitstellung die Konfiguration von Apache Spark- und Hadoop-Komponenten im Dienst- und Ressourcenbereich. Big Data-Cluster verwendet für die meisten Einstellungen dieselben Standardkonfigurationswerte wie das jeweilige Open-Source-Projekt. Die Einstellungen, die geändert werden, sind unten zusammen mit einer Beschreibung und dem zugehörigen Standardwert aufgeführt. Abgesehen von der Gatewayressource gibt es keinen Unterschied zwischen den Einstellungen, die im Dienstbereich und im Ressourcenbereich konfigurierbar sind.

Alle möglichen Konfigurationen und die entsprechenden Standardwerte finden Sie auf der zugehörigen Apache-Dokumentationswebsite:
- Apache Spark: https://spark.apache.org/docs/latest/configuration.html
- Apache Hadoop:
  - HDFS HDFS-Site: https://hadoop.apache.org/docs/r2.7.1/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml
  - HDFS Core-Site: https://hadoop.apache.org/docs/r2.8.0/hadoop-project-dist/hadoop-common/core-default.xml  
  - Yarn: https://hadoop.apache.org/docs/r3.1.1/hadoop-yarn/hadoop-yarn-site/ResourceModel.html
- Hive: https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties#ConfigurationProperties-MetaStore
- Livy: https://github.com/cloudera/livy/blob/master/conf/livy.conf.template
- Apache Knox Gateway: https://knox.apache.org/books/knox-0-14-0/user-guide.html#Gateway+Details

Die Einstellungen, die nicht konfiguriert werden können, sind ebenfalls unten aufgeführt.

> [!NOTE]
> Wenn Sie Spark in den Speicherpool einschließen möchten, legen Sie in der Konfigurationsdatei `bdc.json` den booleschen Wert `includeSpark` auf `spec.resources.storage-0.spec.settings.spark` fest. Die entsprechenden Anweisungen finden Sie unter [Konfigurieren von Apache Spark und Apache Hadoop in Big Data-Clustern](configure-spark-hdfs.md).


##  <a name="big-data-clusters-specific-default-spark-settings"></a>Spezifische Spark-Standardeinstellungen für Big Data-Cluster
Die folgenden Spark-Einstellungen weisen BDC-spezifische Standardwerte auf, können aber vom Benutzer konfiguriert werden. Vom System verwaltete Einstellungen sind nicht aufgeführt.

| Einstellungsname                                                                                 | Beschreibung                                                                                                                                                           | Typ   | Standardwert                                                                                                                              |
| ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| capacity-scheduler.yarn.scheduler.capacity.maximum-applications                      | Die maximale Anzahl von Anwendungen im System, die gleichzeitig aktiv sein können (sowohl in Ausführung als auch ausstehend).                                                               | INT    | 10000                                                                                                                                      |
| capacity-scheduler.yarn.scheduler.capacity.resource-calculator                       | Die ResourceCalculator-Implementierung, die zum Vergleichen von Ressourcen im Scheduler verwendet werden soll.                                                                               | Zeichenfolge | org.apache.hadoop.yarn.util.resource.DominantResourceCalculator                                                                            |
| capacity-scheduler.yarn.scheduler.capacity.root.queues                               | Der Kapazitätsplaner mit vordefinierter Warteschlange namens „root“.                                                                                                              | Zeichenfolge | default                                                                                                                                    |
| capacity-scheduler.yarn.scheduler.capacity.root.default.capacity                     | Warteschlangenkapazität in Prozent (%) als absolute Mindestkapazität für Ressourcenwarteschlangen für die Warteschlange „root“.                                                                          | INT    | 100                                                                                                                                        |
| spark-defaults-conf.spark.driver.cores                                               | Anzahl der Kerne, die für den Treiberprozess verwendet werden sollen (nur im Clustermodus).                                                                                                  | INT    | 1                                                                                                                                          |
| spark-defaults-conf.spark.driver.memoryOverhead                                      | Die Menge an Speicher außerhalb des Heaps, die pro Treiber im Clustermodus zugewiesen werden soll.                                                                                             | INT    | 384                                                                                                                                        |
| spark-defaults-conf.spark.executor.instances                                         | Die Anzahl von Executors für die statische Zuordnung.                                                                                                                        | INT    | 1                                                                                                                                          |
| spark-defaults-conf.spark.executor.cores                                             | Die Anzahl von Kernen, die für jeden Executor verwendet werden soll.                                                                                                                          | INT    | 1                                                                                                                                          |
| spark-defaults-conf.spark.driver.memory                                              | Die Menge an Arbeitsspeicher, die für den Treiberprozess verwendet werden soll.                                                                                                                       | Zeichenfolge | 1g                                                                                                                                         |
| spark-defaults-conf.spark.executor.memory                                            | Die Menge an Arbeitsspeicher, die pro Executorprozess verwendet werden soll.                                                                                                                         | Zeichenfolge | 1g                                                                                                                                         |
| spark-defaults-conf.spark.executor.memoryOverhead                                    | Die Menge an Speicher außerhalb des Heaps, die pro Executor zugewiesen werden soll.                                                                                                           | INT    | 384                                                                                                                                        |
| yarn-site.yarn.nodemanager.resource.memory-mb                                        | Die Menge an physischem Arbeitsspeicher (in MB), die für Container zugeordnet werden kann.                                                                                               | INT    | 8192                                                                                                                                       |
| yarn-site.yarn.scheduler.maximum-allocation-mb                                       | Die maximale Zuordnung für jede Containeranforderung in Resource Manager.                                                                                           | INT    | 8192                                                                                                                                       |
| yarn-site.yarn.nodemanager.resource.cpu-vcores                                       | Die Anzahl der CPU-Kerne, die für Container zugeordnet werden kann.                                                                                                             | INT    | 32                                                                                                                                         |
| yarn-site.yarn.scheduler.maximum-allocation-vcores                                   | Die maximale Zuordnung für jede Containeranforderung in Resource Manager (in vCPU-Kernen).                                                            | INT    | 8                                                                                                                                          |
| yarn-site.yarn.nodemanager.linux-container-executor.secure-mode.pool-user-count      | Die Anzahl von Poolbenutzern für den Linux-Containerexecutor im sicheren Modus.                                                                                             | INT    | 6                                                                                                                                          |
| yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent                        | Maximaler Prozentsatz der Ressourcen im Cluster, die zum Ausführen von Anwendungsmastern verwendet werden können.                                                                              | float  | 0,1                                                                                                                                        |
| yarn-site.yarn.nodemanager.container-executor.class                                  | Containerexecutors für ein bestimmtes Betriebssystem.                                                                                                               | Zeichenfolge | org.apache.hadoop.yarn.server.nodemanager.LinuxContainerExecutor                                                                           |
| capacity-scheduler.yarn.scheduler.capacity.root.default.user-limit-factor            | Das Vielfache der Warteschlangenkapazität, das konfiguriert werden kann, um einem einzelnen Benutzer den Erhalt weiterer Ressourcen zu gestatten.                                                          | INT    | 1                                                                                                                                          |
| capacity-scheduler.yarn.scheduler.capacity.root.default.maximum-capacity             | Maximale Warteschlangenkapazität in Prozent (%) als Gleitkommawert ODER als absolute Höchstkapazität für Ressourcenwarteschlangen. Wenn Sie diesen Wert auf -1 festlegen, wird die maximale Kapazität auf 100 % festgelegt.           | INT    | 100                                                                                                                                        |
| capacity-scheduler.yarn.scheduler.capacity.root.default.state                        | Der Status der Warteschlange kann entweder „Wird ausgeführt“ oder „Beendet“ lauten.                                                                                                                      | Zeichenfolge | RUNNING                                                                                                                                    |
| capacity-scheduler.yarn.scheduler.capacity.root.default.maximum-application-lifetime | Maximale Lebensdauer einer Anwendung, die in eine Warteschlange eingereiht wird (in Sekunden). Bei einem Wert, der kleiner oder gleich null ist, wird die Einstellung deaktiviert.                     | INT    | -1                                                                                                                                         |
| capacity-scheduler.yarn.scheduler.capacity.root.default.default-application-lifetime | Standardlebensdauer einer Anwendung, die in eine Warteschlange eingereiht wird (in Sekunden). Bei einem Wert, der kleiner oder gleich null ist, wird die Einstellung deaktiviert.                     | INT    | -1                                                                                                                                         |
| capacity-scheduler.yarn.scheduler.capacity.node-locality-delay                       | Anzahl der verpassten Planungsmöglichkeiten, nach der der CapacityScheduler versucht, rack-local-Container zu planen.                                               | INT    | 40                                                                                                                                         |
| capacity-scheduler.yarn.scheduler.capacity.rack-locality-additional-delay            | Anzahl der zusätzlich zu „node-locality-delay“ verpassten Planungsmöglichkeiten, nach der der CapacityScheduler versucht, off-switch-Container zu planen. | INT    | -1                                                                                                                                         |
| hadoop-env.HADOOP_HEAPSIZE_MAX                                                       | Standardmäßige maximale Heapgröße aller Hadoop-JVM-Prozesse.                                                                                                                 | INT    | 2048                                                                                                                                       |
| yarn-env.YARN_RESOURCEMANAGER_HEAPSIZE                                               | Heapgröße von YARN ResourceManager.                                                                                                                                     | INT    | 2048                                                                                                                                       |
| yarn-env.YARN_NODEMANAGER_HEAPSIZE                                                   | Heapgröße von YARN NodeManager.                                                                                                                                         | INT    | 2048                                                                                                                                       |
| mapred-env.HADOOP_JOB_HISTORYSERVER_HEAPSIZE                                         | Heapgröße von Hadoop JobHistoryServer.                                                                                                                                 | INT    | 2048                                                                                                                                       |
| hive-env.HADOOP_HEAPSIZE                                                             | Heapgröße von Hadoop für Hive.                                                                                                                                          | INT    | 2048                                                                                                                                       |
| livy-conf.livy.server.session.timeout-check                                          | Überprüft das Sitzungszeitlimit für den Livy-Server.                                                                                                                                | bool   | true                                                                                                                                       |
| livy-conf.livy.server.session.timeout-check.skip-busy                                | Skip-busy für die Überprüfung des Sitzungszeitlimits für den Livy-Server.                                                                                                                  | bool   | true                                                                                                                                       |
| livy-conf.livy.server.session.timeout                                                | Zeitlimit für Livy-Serversitzung in (ms/s/m | min/h/d/y).                                                                                                              | Zeichenfolge | 2h                                                                                                                                         |
| livy-conf.livy.server.yarn.poll-interval                                             | Abrufintervall für YARN in Livy-Server in (ms/s/m | min/h/d/y).                                                                                                     | Zeichenfolge | 500ms                                                                                                                                      |
| livy-conf.livy.rsc.jars                                                              | Livy-RSC-JARs.                                                                                                                                                        | Zeichenfolge | local:/opt/livy/rsc-jars/livy-api.jar,local:/opt/livy/rsc-jars/livy-rsc.jar,local:/opt/livy/rsc-jars/netty-all.jar                         |
| livy-conf.livy.repl.jars                                                             | Livy-REPL-JARs.                                                                                                                                                       | Zeichenfolge | local:/opt/livy/repl_2.11-jars/livy-core.jar,local:/opt/livy/repl_2.11-jars/livy-repl.jar,local:/opt/livy/repl_2.11-jars/commons-codec.jar |
| livy-conf.livy.rsc.sparkr.package                                                    | Livy-RSC-SparkR-Paket.                                                                                                                                              | Zeichenfolge | hdfs:///system/livy/sparkr.zip                                                                                                             |
| livy-env.LIVY_SERVER_JAVA_OPTS                                                       | Java-Optionen für Livy-Server.                                                                                                                                             | Zeichenfolge | -Xmx2g                                                                                                                                     |
| spark-defaults-conf.spark.r.backendConnectionTimeout                                 | Verbindungszeitlimit in Sekunden, das vom R-Prozess bei Verbindung mit RBackend festgelegt wurde.                                                                                         | INT    | 86.400                                                                                                                                      |
| spark-defaults-conf.spark.pyspark.python                                             | Python-Option für Spark.                                                                                                                                              | Zeichenfolge | /opt/bin/python3                                                                                                                           |
| spark-defaults-conf.spark.yarn.jars                                                  | YARN-JARs.                                                                                                                                                            | Zeichenfolge | local:/opt/spark/jars/*                                                                                                                    |
| spark-history-server-conf.spark.history.fs.cleaner.maxAge                            | Maximales Alter von Auftragsverlaufsdateien, bevor diese durch die Verlaufsbereinigung des Dateisystems gelöscht werden, in (ms/s/m | min/h/d/y).                                                   | Zeichenfolge | 7d                                                                                                                                         |
| spark-history-server-conf.spark.history.fs.cleaner.interval                          | Bereinigungsintervall für Spark-Verlauf in (ms/s/m | min/h/d/y).                                                                                                        | Zeichenfolge | 12h                                                                                                                                        |
| hadoop-env.HADOOP_CLASSPATH                                                          | Legt den zusätzlichen Hadoop-Klassenpfad fest.                                                                                                                                 | Zeichenfolge |                                                                                                                                            |
| spark-env.SPARK_DAEMON_MEMORY                                                        | Arbeitsspeicher für Spark-Daemon.                                                                                                                                                  | Zeichenfolge | 2g                                                                                                                                         |
| yarn-site.yarn.log-aggregation.retain-seconds                                        | Wenn die Protokollaggregation aktiviert ist, legt diese Eigenschaft den Aufbewahrungszeitraum für Protokolle fest (in Sekunden).                                                                       | INT    | 604800                                                                                                                                     |
| yarn-site.yarn.nodemanager.log-aggregation.compression-type                          | Komprimierungstyp für die Protokollaggregation für YARN NodeManager.                                                                                                            | Zeichenfolge | gz                                                                                                                                         |
| yarn-site.yarn.nodemanager.log-aggregation.roll-monitoring-interval-seconds          | Intervall in Sekunden für die Rollenüberwachung bei der NodeManager-Protokollaggregation.                                                                                                  | INT    | 3600                                                                                                                                       |
| yarn-site.yarn.scheduler.minimum-allocation-mb                                       | Die Mindestzuordnung für jede Containeranforderung in Resource Manager (in MB).                                                                                   | INT    | 512                                                                                                                                        |
| yarn-site.yarn.scheduler.minimum-allocation-vcores                                   | Die Mindestzuordnung für jede Containeranforderung in Resource Manager (in vCPU-Kernen).                                                             | INT    | 1                                                                                                                                          |
| yarn-site.yarn.nm.liveness-monitor.expiry-interval-ms                                | Gibt an, wie lange gewartet werden soll, bis ein Knoten-Manager als inaktiv erachtet wird.                                                                                                             | INT    | 180.000                                                                                                                                     |
| yarn-site.yarn.resourcemanager.zk-timeout-ms                                         | Zeitlimit für Zookeeper-Sitzung in Millisekunden.                                                                                                                          | INT    | 40.000                                                                                                                                      |
| capacity-scheduler.yarn.scheduler.capacity.root.default.acl_application_max_priority | Die ACL mit Personen, die Anwendungen mit konfigurierter Priorität übermitteln können. Beispiel: [user={name} group={name} max_priority={priorität} default_priority={priorität}].             | Zeichenfolge | *                                                                                                                                          |
| includeSpark                                                                         | Boolescher Wert, der konfiguriert, ob Spark-Aufträge im Speicherpool ausgeführt werden können oder nicht.                                                                                           | bool   | true                                                                                                                                       |
| enableSparkOnK8s                                                                     | Boolescher Wert, der konfiguriert, ob Spark für K8s aktiviert werden soll. In diesem Fall werden Container für K8s im Spark-Kopfteil hinzugefügt.                                                               | bool   | false                                                                                                                                      |
| sparkVersion                                                                         | Die Version von Spark                                                                                                                                                  | Zeichenfolge | 2.4                                                                                                                                        |
| spark-env.PYSPARK_ARCHIVES_PATH                                                      | Pfad zu den in Spark-Aufträgen verwendeten pyspark-Archiv-JARs.                                                                                                                      | Zeichenfolge | local:/opt/spark/python/lib/pyspark.zip,local:/opt/spark/python/lib/py4j-0.10.7-src.zip                                                    |

In den folgenden Abschnitten werden die nicht unterstützten Konfigurationen aufgeführt.

##  <a name="big-data-clusters-specific-default-hdfs-settings"></a>Spezifische HDFS-Standardeinstellungen für Big Data-Cluster
Die folgenden HDFS-Einstellungen weisen BDC-spezifische Standardwerte auf, können aber vom Benutzer konfiguriert werden. Vom System verwaltete Einstellungen sind nicht aufgeführt.

| Einstellungsname                                                                       | Beschreibung                                                                                         | Typ   | Standardwert                                                                    |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | ------ | -------------------------------------------------------------------------------------- |
| hdfs-site.dfs.replication                                                  | Standardmäßige Blockreplikation.                                                                          | INT    | 2                                                                                      |
| hdfs-site.dfs.namenode.provided.enabled                                    | Ermöglicht dem Namensknoten die Verarbeitung bereitgestellter Speicher.                                                   | bool   | true                                                                                   |
| hdfs-site.dfs.datanode.provided.enabled                                    | Ermöglicht dem Datenknoten die Verarbeitung bereitgestellter Speicher.                                                   | bool   | true                                                                                   |
| hdfs-site.dfs.datanode.provided.volume.lazy.load                           | Ermöglicht LazyLoading im Datenknoten für die bereitgestellten Speicher.                                                 | bool   | true                                                                                   |
| hdfs-site.dfs.provided.aliasmap.inmemory.enabled                           | Ermöglicht die In-Memory-Aliaszuordnung für bereitgestellte Speicher.                                                     | bool   | true                                                                                   |
| hdfs-site.dfs.provided.aliasmap.class                                      | Die Klasse, die verwendet wird, um das Eingabeformat der Blöcke für bereitgestellte Speicher anzugeben.              | Zeichenfolge | org.apache.hadoop.hdfs.server.common.blockaliasmap.impl.InMemoryLevelDBAliasMapClient  |
| hdfs-site.dfs.namenode.provided.aliasmap.class                             | Die Klasse, die verwendet wird, um das Eingabeformat der Blöcke für bereitgestellte Speicher für den Namensknoten anzugeben. | Zeichenfolge | org.apache.hadoop.hdfs.server.common.blockaliasmap.impl.NamenodeInMemoryAliasMapClient |
| hdfs-site.dfs.provided.aliasmap.load.retries                               | Anzahl von Wiederholungsversuchen für den Datenknoten zum Laden der bereitgestellten Aliaszuordnung.                                    | INT    | 0                                                                                      |
| hdfs-site.dfs.provided.aliasmap.inmemory.batch-size                        | Die Batchgröße beim Durchlaufen der Datenbank, die die Aliaszuordnung unterstützt.                               | INT    | 500                                                                                    |
| hdfs-site.dfs.datanode.provided.volume.readthrough                         | Aktiviert Readthrough für bereitgestellte Speicher im Datenknoten.                                               | bool   | true                                                                                   |
| hdfs-site.dfs.provided.cache.capacity.mount                                | Ermöglicht die Einbindung von Cachekapazität für bereitgestellte Speicher.                                                  | bool   | true                                                                                   |
| hdfs-site.dfs.provided.overreplication.factor                              | Faktor für übermäßige Replikation für bereitgestellte Speicher.                                                       | float  | 1                                                                                      |
| hdfs-site.dfs.provided.cache.capacity.fraction                             | Anteil der Cachekapazität für den bereitgestellten Speicher.                                                       | float  | 0,01                                                                                   |
| hdfs-site.dfs.ls.limit                                                     | Begrenzt die Anzahl von über ls gedruckten Dateien.                                                            | INT    | 500                                                                                    |
| hdfs-env.HDFS_NAMENODE_OPTS                                                | Optionen für HDFS-Namensknoten.                                                                              | Zeichenfolge | -Dhadoop.security.logger=INFO,RFAS -Xmx2g                                              |
| hdfs-env.HDFS_DATANODE_OPTS                                                | Optionen für HDFS-Datenknoten.                                                                              | Zeichenfolge | -Dhadoop.security.logger=ERROR,RFAS -Xmx2g                                             |
| hdfs-env.HDFS_ZKFC_OPTS                                                    | HDFS-ZKFC-Optionen.                                                                                  | Zeichenfolge | -Xmx1g                                                                                 |
| hdfs-env.HDFS_JOURNALNODE_OPTS                                             | HDFS-JournalNode-Optionen.                                                                           | Zeichenfolge | -Xmx2g                                                                                 |
| hdfs-env.HDFS_AUDIT_LOGGER                                                 | Optionen für die HDFS-Überwachungsprotokollierung.                                                                          | Zeichenfolge | INFO,RFAAUDIT                                                                          |
| core-site.hadoop.security.group.mapping.ldap.search.group.hierarchy.levels | Hierarchieebenen für die Hadoop-LDAP-Suchgruppe der Hauptsite.                                            | INT    | 10                                                                                     |
| core-site.fs.permissions.umask-mode                                        | umask-Berechtigungsmodus.                                                                              | Zeichenfolge | 077                                                                                    |
| core-site.hadoop.security.kms.client.failover.max.retries                  | Maximale Anzahl von Wiederholungsversuchen für Clientfailover.                                                                    | INT    | 20                                                                                     || zoo-cfg.tickTime                                       | Taktzeit für die Zookeeper-Konfiguration.                         | INT    | 2000                |
| zoo-cfg.initLimit                                      | Initialisierungszeit für die Zookeeper-Konfiguration.                         | INT    | 10                  |
| zoo-cfg.syncLimit                                      | Synchronisierungszeit für die Zookeeper-Konfiguration.                         | INT    | 5                   |
| zoo-cfg.maxClientCnxns                                 | Maximale Anzahl von Clientverbindungen für die Zookeeper-Konfiguration.            | INT    | 60                  |
| zoo-cfg.minSessionTimeout                              | Mindestzeitlimit von Sitzungen für die Zookeeper-Konfiguration.           | INT    | 4000                |
| zoo-cfg.maxSessionTimeout                              | Maximales Zeitlimit von Sitzungen für die Zookeeper-Konfiguration.           | INT    | 40.000               |
| zoo-cfg.autopurge.snapRetainCount                      | Anzahl beizubehaltender Momentaufnahmen für die Zookeeper-Konfiguration der automatischen Bereinigung.       | INT    | 3                   |
| zoo-cfg.autopurge.purgeInterval                        | Löschintervall für die Zookeeper-Konfiguration der automatischen Bereinigung.          | INT    | 0                   |
| zookeeper-java-env.JVMFLAGS                            | JVM-Flags für die Java-Umgebung in Zookeeper.            | Zeichenfolge | -Xmx1G -Xms1G       |
| zookeeper-log4j-properties.zookeeper.console.threshold | Schwellenwert für die log4j-Konsole in Zookeeper.               | Zeichenfolge | INFO                |
| zoo-cfg.zookeeper.request.timeout                      | Steuert das Zookeeper-Zeitlimit für Anforderungen in Millisekunden. | INT    | 40.000               |
| kms-site.hadoop.security.kms.encrypted.key.cache.size | Cachegröße für verschlüsselten Schlüssel in Hadoop KMS. | INT  | 500 |
    

##  <a name="big-data-clusters-specific-default-gateway-settings"></a>Spezifische Gateway-Standardeinstellungen für Big Data-Cluster
Die folgenden Gatewayeinstellungen weisen BDC-spezifische Standardwerte auf, können aber vom Benutzer konfiguriert werden. Vom System verwaltete Einstellungen sind nicht aufgeführt. Gatewayeinstellungen können nur im Bereich **Ressourcen** konfiguriert werden.

| Einstellungsname                                          | Beschreibung                                            | Typ   | Standardwert |
| --------------------------------------------- | ------------------------------------------------------ | ------ | ------------------- |
| gateway-site.gateway.httpclient.socketTimeout | Socketzeitlimit für HTTP-Client im Gateway in (ms/s/m). | Zeichenfolge | 90s                 |
| gateway-site.sun.security.krb5.debug          | Debuggen für Kerberos-Sicherheit.                           | bool   | true                |
| knox-env.KNOX_GATEWAY_MEM_OPTS                | Arbeitsspeicheroptionen für Knox-Gateway.                           | Zeichenfolge | -Xmx2g              |

## <a name="unsupported-spark-configurations"></a>Nicht unterstützte Spark-Konfigurationen

Die folgenden `spark`-Konfigurationen werden nicht unterstützt und können im Kontext des Big Data-Clusters nicht geändert werden.

| Category  | Unterkategorie               | Datei                       | Nicht unterstützte Konfigurationen                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|           | yarn-site                  | yarn-site.xml              | yarn.log-aggregation-enable                                             |
|           |                            |                            | yarn.log.server.url                                                     |
|           |                            |                            | yarn.nodemanager.pmem-check-enabled                                     |
|           |                            |                            | yarn.nodemanager.vmem-check-enabled                                     |
|           |                            |                            | yarn.nodemanager.aux-services                                           |
|           |                            |                            | yarn.resourcemanager.address                                            |
|           |                            |                            | yarn.nodemanager.address                                                |
|           |                            |                            | yarn.client.failover-no-ha-proxy-provider                               |
|           |                            |                            | yarn.client.failover-proxy-provider                                     |
|           |                            |                            | yarn.http.policy                                                        |
|           |                            |                            | yarn.nodemanager.linux-container-executor.secure-mode.use-pool-user     |
|           |                            |                            | yarn.nodemanager.linux-container-executor.secure-mode.pool-user-prefix  |
|           |                            |                            | yarn.nodemanager.linux-container-executor.nonsecure-mode.local-user     |
|           |                            |                            | yarn.acl.enable                                                         |
|           |                            |                            | yarn.admin.acl                                                          |
|           |                            |                            | yarn.resourcemanager.hostname                                           |
|           |                            |                            | yarn.resourcemanager.principal                                          |
|           |                            |                            | yarn.resourcemanager.keytab                                             |
|           |                            |                            | yarn.resourcemanager.webapp.spnego-keytab-file                          |
|           |                            |                            | yarn.resourcemanager.webapp.spnego-principal                            |
|           |                            |                            | yarn.nodemanager.principal                                              |
|           |                            |                            | yarn.nodemanager.keytab                                                 |
|           |                            |                            | yarn.nodemanager.webapp.spnego-keytab-file                              |
|           |                            |                            | yarn.nodemanager.webapp.spnego-principal                                |
|           |                            |                            | yarn.resourcemanager.ha.enabled                                         |
|           |                            |                            | yarn.resourcemanager.cluster-id                                         |
|           |                            |                            | yarn.resourcemanager.zk-address                                         |
|           |                            |                            | yarn.resourcemanager.ha.rm-ids                                          |
|           |                            |                            | yarn.resourcemanager.hostname.*                                         |
|           | capacity-scheduler         | capacity-scheduler.xml     | yarn.scheduler.capacity.root.acl_submit_applications                    |
|           |                            |                            | yarn.scheduler.capacity.root.acl_administer_queue                       |
|           |                            |                            | yarn.scheduler.capacity.root.default.acl_application_max_priority       |
|           | yarn-env                   | yarn-env.sh                |                                                                         |
|           | spark-defaults-conf        | spark-defaults.conf        | spark.yarn.archive                                                      |
|           |                            |                            | spark.yarn.historyServer.address                                        |
|           |                            |                            | spark.eventLog.enabled                                                  |
|           |                            |                            | spark.eventLog.dir                                                      |
|           |                            |                            | spark.sql.warehouse.dir                                                 |
|           |                            |                            | spark.sql.hive.metastore.version                                        |
|           |                            |                            | spark.sql.hive.metastore.jars                                           |
|           |                            |                            | spark.extraListeners                                                    |
|           |                            |                            | spark.metrics.conf                                                      |
|           |                            |                            | spark.ssl.enabled                                                       |
|           |                            |                            | spark.authenticate                                                      |
|           |                            |                            | spark.network.crypto.enabled                                            |
|           |                            |                            | spark.ssl.keyStore                                                      |
|           |                            |                            | spark.ssl.keyStorePassword  
|           |                            |                            | spark.ui.enabled                                                        |
|           | spark-env                  | spark-env.sh               | SPARK_NO_DAEMONIZE                                                      |
|           |                            |                            | SPARK_DIST_CLASSPATH                                                    |
|           | spark-history-server-conf  | spark-history-server.conf  | spark.history.fs.logDirectory                                           |
|           |                            |                            | spark.ui.proxyBase                                                      |
|           |                            |                            | spark.history.fs.cleaner.enabled                                        |
|           |                            |                            | spark.ssl.enabled                                                       |
|           |                            |                            | spark.authenticate                                                      |
|           |                            |                            | spark.network.crypto.enabled                                            |
|           |                            |                            | spark.ssl.keyStore                                                      |
|           |                            |                            | spark.ssl.keyStorePassword                                              |
|           |                            |                            | spark.history.kerberos.enabled                                          |
|           |                            |                            | spark.history.kerberos.principal                                        |
|           |                            |                            | spark.history.kerberos.keytab                                           |
|           |                            |                            | spark.ui.filters                                                        |
|           |                            |                            | spark.acls.enable                                                       |
|           |                            |                            | spark.history.ui.acls.enable                                            |
|           |                            |                            | spark.history.ui.admin.acls                                             |
|           |                            |                            | spark.history.ui.admin.acls.groups                                      |
|           | livy-conf                  | livy.conf                  | livy.keystore                                                           |
|           |                            |                            | livy.keystore.password                                                  |
|           |                            |                            | livy.spark.master                                                       |
|           |                            |                            | livy.spark.deploy-mode                                                  |
|           |                            |                            | livy.rsc.jars                                                           |
|           |                            |                            | livy.repl.jars                                                          |
|           |                            |                            | livy.rsc.pyspark.archives                                               |
|           |                            |                            | livy.rsc.sparkr.package                                                 |
|           |                            |                            | livy.repl.enable-hive-context                                           |
|           |                            |                            | livy.superusers                                                         |
|           |                            |                            | livy.server.auth.type                                                   |
|           |                            |                            | livy.server.launch.kerberos.keytab                                      |
|           |                            |                            | livy.server.launch.kerberos.principal                                   |
|           |                            |                            | livy.server.auth.kerberos.principal                                     |
|           |                            |                            | livy.server.auth.kerberos.keytab                                        |
|           |                            |                            | livy.impersonation.enabled                                              |
|           |                            |                            | livy.server.access-control.enabled                                      |
|           |                            |                            | livy.server.access-control.*                                            |
|           | livy-env                   | livy-env.sh                |                                                                         |
|           | hive-site                  | hive-site.xml              | javax.jdo.option.ConnectionURL                                          |
|           |                            |                            | javax.jdo.option.ConnectionDriverName                                   |
|           |                            |                            | javax.jdo.option.ConnectionUserName                                     |
|           |                            |                            | javax.jdo.option.ConnectionPassword                                     |
|           |                            |                            | hive.metastore.uris                                                     |
|           |                            |                            | hive.metastore.pre.event.listeners                                      |
|           |                            |                            | hive.security.authorization.enabled                                     |
|           |                            |                            | hive.security.metastore.authenticator.manager                           |
|           |                            |                            | hive.security.metastore.authorization.manager                           |
|           |                            |                            | hive.metastore.use.SSL                                                  |
|           |                            |                            | hive.metastore.keystore.path                                            |
|           |                            |                            | hive.metastore.keystore.password                                        |
|           |                            |                            | hive.metastore.truststore.path                                          |
|           |                            |                            | hive.metastore.truststore.password                                      |
|           |                            |                            | hive.metastore.kerberos.keytab.file                                     |
|           |                            |                            | hive.metastore.kerberos.principal                                       |
|           |                            |                            | hive.metastore.sasl.enabled                                             |
|           |                            |                            | hive.metastore.execute.setugi                                           |
|           |                            |                            | hive.cluster.delegation.token.store.class                               |
|           | hive-env                   | hive-env.sh                

## <a name="unsupported-hdfs-configurations"></a>Nicht unterstützte HDFS-Konfigurationen

Die folgenden `hdfs`-Konfigurationen werden nicht unterstützt und können im Kontext des Big Data-Clusters nicht geändert werden.

| Category  | Unterkategorie               | Datei                       | Nicht unterstützte Konfigurationen                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|          | core-site                   | core-site.xml                 | fs.defaultFS                                          |
|          |                             |                               | ha.zookeeper.quorum                                   |
|          |                             |                               | hadoop.tmp.dir                                        |
|          |                             |                               | hadoop.rpc.protection                                 |
|          |                             |                               | hadoop.security.auth_to_local                         |
|          |                             |                               | hadoop.security.authentication                        |
|          |                             |                               | hadoop.security.authorization                         |
|          |                             |                               | hadoop.http.authentication.simple.anonymous.allowed   |
|          |                             |                               | hadoop.http.authentication.type                       |
|          |                             |                               | hadoop.http.authentication.kerberos.principal         |
|          |                             |                               | hadoop.http.authentication.kerberos.keytab            |
|          |                             |                               | hadoop.http.filter.initializers                       |
|          |                             |                               | hadoop.security.group.mapping.*                       |                               |
|          |                             |                               | hadoop.security.key.provider.path                     |                               |
|          | mapred-env                  | mapred-env.sh                 |                                                       |
|          | hdfs-site                   | hdfs-site.xml                 | dfs.namenode.name.dir                                 |
|          |                             |                               | dfs.datanode.data.dir                                 |
|          |                             |                               | dfs.namenode.acls.enabled                             |
|          |                             |                               | dfs.namenode.datanode.registration.ip-hostname-check  |
|          |                             |                               | dfs.client.retry.policy.enabled                       |
|          |                             |                               | dfs.permissions.enabled                               |
|          |                             |                               | dfs.nameservices                                      |
|          |                             |                               | dfs.ha.namenodes.nmnode-0                             |
|          |                             |                               | dfs.namenode.rpc-address.nmnode-0.*                   |
|          |                             |                               | dfs.namenode.shared.edits.dir                         |
|          |                             |                               | dfs.ha.automatic-failover.enabled                     |
|          |                             |                               | dfs.ha.fencing.methods                                |
|          |                             |                               | dfs.journalnode.edits.dir                             |
|          |                             |                               | dfs.client.failover.proxy.provider.nmnode-0           |
|          |                             |                               | dfs.namenode.http-address                             |
|          |                             |                               | dfs.namenode.httpS-address                            |
|          |                             |                               | dfs.http.policy                                       |
|          |                             |                               | dfs.encrypt.data.transfer                             |
|          |                             |                               | dfs.block.access.token.enable                         |
|          |                             |                               | dfs.data.transfer.protection                          |
|          |                             |                               | dfs.encrypt.data.transfer.cipher.suites               |
|          |                             |                               | dfs.https.port                                        |
|          |                             |                               | dfs.namenode.keytab.file                              |
|          |                             |                               | dfs.namenode.kerberos.principal                       |
|          |                             |                               | dfs.namenode.kerberos.internal.spnego.principal       |
|          |                             |                               | dfs.datanode.data.dir.perm                            |
|          |                             |                               | dfs.datanode.address                                  |
|          |                             |                               | dfs.datanode.http.address                             |
|          |                             |                               | dfs.datanode.ipc.address                              |
|          |                             |                               | dfs.datanode.https.address                            |
|          |                             |                               | dfs.datanode.keytab.file                              |
|          |                             |                               | dfs.datanode.kerberos.principal                       |
|          |                             |                               | dfs.journalnode.keytab.file                           |
|          |                             |                               | dfs.journalnode.kerberos.principal                    |
|          |                             |                               | dfs.journalnode.kerberos.internal.spnego.principal    |
|          |                             |                               | dfs.web.authentication.kerberos.keytab                |
|          |                             |                               | dfs.web.authentication.kerberos.principal             |
|          |                             |                               | dfs.webhdfs.enabled                                   |
|          |                             |                               | dfs.permissions.superusergroup                        |
|          | hdfs-env                    | hdfs-env.sh                   | HADOOP_HEAPSIZE_MAX                                   |
|          | zoo-cfg                     | zoo.cfg                       | secureClientPort                                      |
|          |                             |                               | clientPort                                            |
|          |                             |                               | dataDir                                               |
|          |                             |                               | dataLogDir                                            |
|          |                             |                               | 4lw.commands.whitelist                                |
|          | zookeeper-java-env          | java.env                      | ZK_LOG_DIR                                            |
|          |                             |                               | SERVER_JVMFLAGS                                       |
|          | zookeeper-log4j-properties  | log4j.properties (zookeeper)  | log4j.rootLogger                                      |
|          |                             |                               | log4j.appender.CONSOLE.*                              |

> [!NOTE]
> In diesem Artikel wird der Begriff *Whitelist* verwendet, der in diesem Kontext von Microsoft als unangemessen eingestuft wird. Der Begriff wird in diesem Artikel verwendet, weil er derzeit in der Software verwendet wird. Sobald der Begriff aus der Software entfernt wird, wird er auch aus dem Artikel entfernt.

## <a name="unsupported-gateway-configurations"></a>Nicht unterstützte `gateway`-Konfigurationen

Die folgenden `gateway`-Konfigurationen werden nicht unterstützt und können im Kontext des Big Data-Clusters nicht geändert werden.

| Category  | Unterkategorie               | Datei                       | Nicht unterstützte Konfigurationen                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|          | gateway-site                | gateway-site.xml              | gateway.port                                          |
|          |                             |                               | gateway.path                                          |
|          |                             |                               | gateway.gateway.conf.dir                              |
|          |                             |                               | gateway.hadoop.kerberos.secured                       |
|          |                             |                               | java.security.krb5.conf                               |
|          |                             |                               | java.security.auth.login.config                       |
|          |                             |                               | gateway.websocket.feature.enabled                     |
|          |                             |                               | gateway.scope.cookies.feature.enabled                 |
|          |                             |                               | ssl.exclude.protocols                                 |
|          |                             |                               | ssl.include.ciphers                                   |

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines SQL Server Big Data-Clusters](configure-bdc-overview.md)
