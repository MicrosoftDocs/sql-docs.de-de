---
title: Erfassen von Daten mit Spark-Aufträgen
titleSuffix: SQL Server Big Data Clusters
description: In diesem Tutorial wird veranschaulicht, wie Daten mithilfe von Spark-Aufträgen in Azure Data Studio im Datenpool eines Big-Data-Clusters für SQL Server erfasst werden.
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b580ef9f94ef4a677d67518b15a740fc2df75e2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045651"
---
# <a name="tutorial-ingest-data-into-a-sql-server-data-pool-with-spark-jobs"></a>Tutorial: Erfassen von Daten in einem SQL Server-Datenpool mithilfe von Spark-Aufträgen

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Tutorial wird erläutert, wie Daten über Spark-Aufträge in den [Datenpool](concept-data-pool.md) eines [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] geladen werden. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer externen Tabelle im Datenpool
> * Erstellen eines Spark-Auftrags zum Laden von Daten aus HDFS
> * Abfragen der Ergebnisse in der externen Tabelle

> [!TIP]
> Wenn Sie möchten, können Sie ein Skript für die Befehle in diesem Tutorial herunterladen und ausführen. Anweisungen finden Sie in den [Beispielen zu Datenpools](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/data-pool) auf GitHub.

## <a name="prerequisites"></a><a id="prereqs"></a> Voraussetzungen

- [Big-Data-Tools](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **Erweiterung von SQL Server 2019**
- [Laden von Beispieldaten in einen Big Data-Cluster von SQL Server](tutorial-load-sample-data.md)

## <a name="create-an-external-table-in-the-data-pool"></a>Erstellen einer externen Tabelle im Datenpool

Mit den folgenden Schritten wird eine externe Tabelle mit dem Namen **web_clickstreams_spark_results** im Datenpool erstellt. Diese Tabelle kann dann als Speicherort für die Erfassung von Daten im Big-Data-Cluster verwendet werden.

1. Stellen Sie in Azure Data Studio eine Verbindung mit der SQL Server-Masterinstanz Ihres Big Data-Clusters her. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit der SQL Server-Masterinstanz](connect-to-big-data-cluster.md#master).

1. Doppelklicken Sie im Fenster **Server** auf die Verbindung, um das Serverdashboard der SQL Server-Masterinstanz anzuzeigen. Wählen Sie **Neue Abfrage** aus.

   ![Abfrage der SQL Server-Masterinstanz](./media/tutorial-data-pool-ingest-spark/sql-server-master-instance-query.png)

1. Erstellen Sie Berechtigungen für den MSSQL-Spark-Connector.
   ```sql
   USE Sales
   CREATE LOGIN sample_user  WITH PASSWORD ='password123!#' 
   CREATE USER sample_user FROM LOGIN sample_user

   -- To create external tables in data pools
   GRANT ALTER ANY EXTERNAL DATA SOURCE TO sample_user;

   -- To create external tables
   GRANT CREATE TABLE TO sample_user;
   GRANT ALTER ANY SCHEMA TO sample_user;

   -- To view database state for Sales
   GRANT VIEW DATABASE STATE ON DATABASE::Sales TO sample_user;

   ALTER ROLE [db_datareader] ADD MEMBER sample_user
   ALTER ROLE [db_datawriter] ADD MEMBER sample_user
   ```

1. Erstellen Sie eine externe Datenquelle für den Datenpool, wenn diese nicht bereits vorhanden ist.

   ```sql
   USE Sales
   GO
   IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlDataPool')
     CREATE EXTERNAL DATA SOURCE SqlDataPool
     WITH (LOCATION = 'sqldatapool://controller-svc/default');
   ```

1. Erstellen Sie eine externe Tabelle mit dem Namen **web_clickstreams_spark_results** im Datenpool.

   ```sql
   USE Sales
   GO
   IF NOT EXISTS(SELECT * FROM sys.external_tables WHERE name = 'web_clickstreams_spark_results')
      CREATE EXTERNAL TABLE [web_clickstreams_spark_results]
      ("wcs_click_date_sk" BIGINT , "wcs_click_time_sk" BIGINT , "wcs_sales_sk" BIGINT , "wcs_item_sk" BIGINT , "wcs_web_page_sk" BIGINT , "wcs_user_sk" BIGINT)
      WITH
      (
         DATA_SOURCE = SqlDataPool,
         DISTRIBUTION = ROUND_ROBIN
      );
   ```
   
1. Erstellen Sie Anmeldeinformationen für Datenpools und erteilen Sie dem Benutzer Berechtigungen.
   ```sql 
   EXECUTE( ' Use Sales; CREATE LOGIN sample_user  WITH PASSWORD = ''password123!#'' ;') AT  DATA_SOURCE SqlDataPool;

   EXECUTE('Use Sales; CREATE USER sample_user; ALTER ROLE [db_datareader] ADD MEMBER sample_user;  ALTER ROLE [db_datawriter] ADD MEMBER sample_user;') AT DATA_SOURCE SqlDataPool;
   ```
   
Die Erstellung einer externen Datenpooltabelle ist ein blockierender Vorgang. Sie können erst dann wieder Aktionen durchführen, wenn die angegebene Tabelle auf allen Back-End-Knoten des Datenpools erstellt wurde. Wenn während des Erstellens ein Fehler auftritt, wird dem Aufrufer eine Fehlermeldung zurückgegeben.

## <a name="start-a-spark-streaming-job"></a>Starten eines Spark-Streamingauftrags

Im nächsten Schritt erstellen Sie einen Spark-Streamingauftrag, der Webclickstreamdaten aus dem Speicherpool (HDFS) in die externe Tabelle lädt, die Sie im Datenpool erstellt haben. Diese Daten wurden zu /clickstream_data in [Laden von Beispieldaten in einen Big Data Cluster für SQL Server](tutorial-load-sample-data.md) hinzugefügt.

1. Stellen Sie in Azure Data Studio eine Verbindung mit der Masterinstanz Ihres Big-Data-Clusters her. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einem Big-Data-Cluster](connect-to-big-data-cluster.md).

2. Erstellen Sie ein neues Notebook, und wählen Sie Spark (Scala) als Ihren Kernel aus.

3. Führen Sie den Spark-Auftrag zur Erfassung aus.

   1. Konfigurieren Sie die Parameter des Spark-SQL-Connectors.

    > [!NOTE]
    > Wenn Ihr Big Data-Cluster mit Active Directory-Integration bereitgestellt wird, ersetzen Sie den Wert für **Hostname** unten, sodass der an den Dienstnamen angefügte vollqualifizierte Domänenname eingeschlossen wird. Beispiel: *hostname=master-p-svc.\<domainName>* .

      ```
      import org.apache.spark.sql.types._
      import org.apache.spark.sql.{SparkSession, SaveMode, Row, DataFrame}

      // Change per your installation
      val user= "username"
      val password= "****"
      val database =  "MyTestDatabase"
      val sourceDir = "/clickstream_data"
      val datapool_table = "web_clickstreams_spark_results"
      val datasource_name = "SqlDataPool"
      val schema = StructType(Seq(
      StructField("wcs_click_date_sk",LongType,true), StructField("wcs_click_time_sk",LongType,true), 
      StructField("wcs_sales_sk",LongType,true), StructField("wcs_item_sk",LongType,true),
      StructField("wcs_web_page_sk",LongType,true), StructField("wcs_user_sk",LongType,true)
      ))

      val hostname = "master-p-svc"
      val port = 1433
      val url = s"jdbc:sqlserver://${hostname}:${port};database=${database};user=${user};password=${password};"
      ```
   2. Definieren und führen Sie den Spark-Auftrag aus.
      * Jeder Auftrag besteht aus zwei Teilen: readStream und writeStream. Im Folgenden wird ein Datenrahmen mit dem oben definierten Schema erstellt. Dann wird dieser in die externe Tabelle im Datenpool geschrieben.
      ```
      import org.apache.spark.sql.{SparkSession, SaveMode, Row, DataFrame}
      
      val df = spark.readStream.format("csv").schema(schema).option("header", true).load(sourceDir)
      val query = df.writeStream.outputMode("append").foreachBatch{ (batchDF: DataFrame, batchId: Long) => 
                batchDF.write
                 .format("com.microsoft.sqlserver.jdbc.spark")
                 .mode("append")
                  .option("url", url)
                  .option("dbtable", datapool_table)
                  .option("user", user)
                  .option("password", password)
                  .option("dataPoolDataSource",datasource_name).save()
               }.start()

      query.awaitTermination(40000)
      query.stop()
      ```
## <a name="query-the-data"></a>Abfragen der Daten

Die folgenden Schritte zeigen, dass der Spark-Streamingauftrag die Daten aus HDFS in den Datenpool geladen hat.

1. Bevor Sie die erfassten Daten abfragen, sehen Sie sich den Spark-Ausführungsstatus an, einschließlich der Yarn-App-ID, der Spark-Benutzeroberfläche und der Treiberprotokolle. Diese Informationen werden im Notebook angezeigt, wenn Sie die Spark-Anwendung zum ersten Mal starten.

   ![Details zur Ausführung von Spark](./media/tutorial-data-pool-ingest-spark/Spark-Joblog-sparkui-yarn.png)

1. Wechseln Sie zurück zum Abfragefenster der SQL Server-Masterinstanz, das Sie zu Beginn des Tutorials geöffnet haben.

1. Führen Sie die folgende Abfrage aus, um die erfassten Daten zu überprüfen.

   ```sql
   USE Sales
   GO
   SELECT count(*) FROM [web_clickstreams_spark_results];
   SELECT TOP 10 * FROM [web_clickstreams_spark_results];
   ```
1. Die Daten können auch in Spark abgefragt werden. Der folgende Code gibt beispielsweise die Anzahl der Datensätze in der Tabelle aus:
   ```
   def df_read(dbtable: String,
                url: String,
                dataPoolDataSource: String=""): DataFrame = {
        spark.read
             .format("com.microsoft.sqlserver.jdbc.spark")
             .option("url", url)
             .option("dbtable", dbtable)
             .option("user", user)
             .option("password", password)
             .option("dataPoolDataSource", dataPoolDataSource)
             .load()
             }

   val new_df = df_read(datapool_table, url, dataPoolDataSource=datasource_name)
   println("Number of rows is " +  new_df.count)
   ```
## <a name="clean-up"></a>Bereinigung

Verwenden Sie den folgenden Befehl, um die in diesem Tutorial erstellten Datenbankobjekte zu entfernen.

```sql
DROP EXTERNAL TABLE [dbo].[web_clickstreams_spark_results];
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie in Azure Data Studio ein Beispielnotebook ausführen:
> [!div class="nextstepaction"]
> [Run a sample notebook (Ausführen eines Beispielnotebooks)](notebooks-tutorial-spark.md)
