---
title: PolyBase-Konfiguration und -Sicherheit für Hadoop | Microsoft-Dokumentation
description: Verwenden Sie diese Einstellungen für die PolyBase-Konnektivität mit Hadoop, einschließlich Hadoop.RPC.Protection, XML-Beispieldateien für CDH 5.X-Cluster und Kerberos-Konfiguration.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 455c1de1f11b58d4d12cf982d035846b73e60a43
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351854"
---
# <a name="polybase-configuration-and-security-for-hadoop"></a>PolyBase-Konfiguration und -Sicherheit für Hadoop

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Dieser Artikel bietet eine Referenz für verschiedene Konfigurationseinstellungen, die die PolyBase-Konnektivität mit Hadoop beeinflussen. Eine exemplarische Vorgehensweise zur Verwendung von PolyBase mit Hadoop finden Sie unter [Konfigurieren von PolyBase für den Zugriff auf externe Daten in Hadoop](polybase-configure-hadoop.md).

## <a name="hadooprpcprotection-setting"></a><a id="rpcprotection"></a> Hadoop.RPC.Protection-Einstellung

Eine gängige Methode zum Sichern der Kommunikation in einem Hadoop-Cluster ist das Ändern der Konfiguration „hadoop.rpc.protection“ zu „Datenschutz“ oder „Integrität“. Standardmäßig geht PolyBase davon aus, dass die Konfiguration auf „Authentifizieren“ festgelegt ist. Um diese Standardeinstellung außer Kraft zu setzen, müssen Sie folgende Eigenschaft zur Datei „core-site.xml“ hinzufügen. Das Ändern dieser Konfiguration ermöglicht eine sichere Datenübertragung zwischen den Hadoop-Knoten und eine TLS-Verbindung mit SQL Server.

```xml
<!-- RPC Encryption information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG -->
   <property>
     <name>hadoop.rpc.protection</name>
     <value></value>
   </property> 
```

Zur Verwendung von „Datenschutz“ oder „Integrität“ für „for hadoop.rpc.protection“ ist mindestens SQL Server-Version SQL Server 2016 SP1 CU7, SQL Server 2016 SP2 oder SQL Server 2017 CU3 erforderlich.

## <a name="example-xml-files-for-cdh-5x-cluster"></a>XML-Beispieldateien für CDH 5.X-Cluster

„Yarn-site.xml“ mit der Konfiguration „yarn.application.classpath“ und „mapreduce.application.classpath“

```xml
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
   <property>
      <name>yarn.resourcemanager.connect.max-wait.ms</name>
      <value>40000</value>
   </property>
   <property>
      <name>yarn.resourcemanager.connect.retry-interval.ms</name>
      <value>30000</value>
   </property>
<!-- Applications' Configuration-->
   <property>
     <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
      <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
      <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
      <name>yarn.application.classpath</name>
      <value>$HADOOP_CLIENT_CONF_DIR,$HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,$HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,$HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/,$HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,$MR2_CLASSPATH*</value>
   </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
      <name>yarn.resourcemanager.principal</name>
      <value></value>
   </property>
-->
</configuration>
```

Wenn Sie die beiden Konfigurationseinstellungen in mapred-site.xml und yarn-site.xml aufteilen, erhalten Sie die folgenden beiden Dateien:

**yarn-site.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
   <property>
      <name>yarn.resourcemanager.connect.max-wait.ms</name>
      <value>40000</value>
   </property>
   <property>
      <name>yarn.resourcemanager.connect.retry-interval.ms</name>
      <value>30000</value>
   </property>
<!-- Applications' Configuration-->
   <property>
     <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
      <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
      <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
      <name>yarn.application.classpath</name>
      <value>$HADOOP_CLIENT_CONF_DIR,$HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,$HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,$HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/*</value>
   </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
      <name>yarn.resourcemanager.principal</name>
      <value></value>
   </property>
-->
</configuration>
```

**mapred-site.xml**

Beachten Sie, dass die Eigenschaft mapreduce.application.classpath hinzugefügt wurde. In CDH 5.x finden Sie die Konfigurationswerte unter derselben Namenskonvention in Ambari.

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
<configuration xmlns:xi="http://www.w3.org/2001/XInclude">
   <property>
     <name>mapred.min.split.size</name>
       <value>1073741824</value>
   </property>
   <property>
     <name>mapreduce.app-submission.cross-platform</name>
     <value>true</value>
   </property>
<property>
     <name>mapreduce.application.classpath</name>
     <value>$HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,$MR2_CLASSPATH</value>
   </property>


<!--kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
     <name>mapreduce.jobhistory.principal</name>
     <value></value>
   </property>
   <property>
     <name>mapreduce.jobhistory.address</name>
     <value></value>
   </property>
-->
</configuration>
```

## <a name="kerberos-configuration"></a>Kerberos-Konfiguration  

Beachten Sie, dass es zur Authentifizierung von PolyBase bei einem gesicherten Kerberos-Cluster erforderlich ist, die Einstellung „hadoop.rpc.protection“ auf „Authentifizieren“ festzulegen. Dadurch bleibt die Datenkommunikation zwischen den Hadoop-Knoten unverschlüsselt. Aktualisieren Sie Datei „core-site.xml“ auf dem PolyBase-Server, um die Einstellung „Datenschutz“ oder „Integrität“ für „hadoop.rpc.protection“ zu verwenden. Weitere Informationen finden Sie im vorherigen Abschnitt ([Herstellen einer Verbindung mit einem Hadoop-Cluster mit der Einstellung „hadoop.rpc.protection“](#rpcprotection)).

So stellen Sie mithilfe von MIT KDC eine Verbindung mit einem mit Kerberos geschützten Hadoop-Cluster her:

1. Suchen Sie das Hadoop-Konfigurationsverzeichnis im Installationspfad von SQL Server. In der Regel lautet der Pfad:  

   ```  
   C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase\Hadoop\conf  
   ```  

2. Suchen Sie den hadoopseitigen Konfigurationswert der in dieser Tabelle aufgelisteten Konfigurationsschlüssel. (Suchen Sie die Dateien auf dem Hadoop-Computer im Hadoop-Konfigurationsverzeichnis.)  
   
3. Kopieren Sie die Konfigurationswerte in die Eigenschaft „Value“ in den entsprechenden Dateien auf dem SQL Server-Computer.  
   
   |**#**|**Konfigurationsdatei**|**Konfigurationsschlüssel**|**Aktion**|  
   |------------|----------------|---------------------|----------|   
   |1|core-site.xml|polybase.kerberos.kdchost|Geben Sie den KDC-Hostnamen an. Zum Beispiel: kerberos.ihr-bereich.de.|  
   |2|core-site.xml|polybase.kerberos.realm|Geben Sie den Kerberos-Bereich an. Beispiel: YOUR-REALM.COM <br><br>**Konfigurationshinweis**: Der Bereichsname muss in Großbuchstaben geschrieben werden.|  
   |3|core-site.xml|hadoop.security.authentication|Suchen Sie die hadoopseitige Konfiguration, und kopieren Sie diese auf den SQL Server-Computer. Beispiel: KERBEROS<br></br>**Sicherheitshinweis:** KERBEROS muss in Großbuchstaben geschrieben werden. Bei Kleinschreibung ist die Funktionalität nicht gewährleistet.|   
   |4|hdfs-site.xml|dfs.namenode.kerberos.principal|Suchen Sie die hadoopseitige Konfiguration, und kopieren Sie diese auf den SQL Server-Computer. Beispiel: hdfs/_HOST@YOUR-REALM.COM|  
   |5|mapred-site.xml|mapreduce.jobhistory.principal|Suchen Sie die hadoopseitige Konfiguration, und kopieren Sie diese auf den SQL Server-Computer. Beispiel: mapred/_HOST@YOUR-REALM.COM|  
   |6|mapred-site.xml|mapreduce.jobhistory.address|Suchen Sie die hadoopseitige Konfiguration, und kopieren Sie diese auf den SQL Server-Computer. Beispiel: 10.193.26.174:10020|  
   |7|yarn-site.xml yarn.|yarn.resourcemanager.principal|Suchen Sie die hadoopseitige Konfiguration, und kopieren Sie diese auf den SQL Server-Computer. Beispiel: yarn/_HOST@YOUR-REALM.COM|  

4. Erstellen Sie ein datenbankweites Anmeldeinformationsobjekt, um die Authentifizierungsinformationen für jeden Hadoop-Benutzer anzugeben. Weitere Informationen finden Sie unter [PolyBase T-SQL-Objekte](../../relational-databases/polybase/polybase-t-sql-objects.md).  

## <a name="next-steps"></a>Nächste Schritte  

Weitere Informationen finden Sie in den folgenden Artikeln:

[Konfigurieren von PolyBase für den Zugriff auf externe Daten in Hadoop](polybase-configure-hadoop.md)
[PolyBase-Leitfaden](../../relational-databases/polybase/polybase-guide.md)
