---
title: Apache Spark-Connector für SQL Server
description: Erfahren Sie, wie Sie den Apache Spark-Connector für SQL Server verwenden.
ms.custom: ''
ms.date: 08/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.openlocfilehash: 2cc921b7aa1acaf99da8a632dc8e3996d648682c
ms.sourcegitcommit: 059722ff78a6061b801807416b312ae9f721ec7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100636699"
---
# <a name="apache-spark-connector-sql-server--azure-sql"></a>Apache Spark-Connector: SQL Server und Azure SQL

Der Apache Spark-Connector für SQL Server und Azure SQL ist ein Hochleistungsconnector, der es Ihnen ermöglicht, Transaktionsdaten in Big Data-Analysen zu verwenden und Ergebnisse für Ad-hoc-Abfragen oder Berichte zu speichern. Der Connector ermöglicht Ihnen die Verwendung einer beliebigen SQL-Datenbank (lokal oder in der Cloud) als Eingabedatenquelle oder als Ausgabedatensenke für Spark-Aufträge.

Diese Bibliothek enthält den Quellcode für den Apache Spark-Connector für SQL Server und Azure SQL.

[Apache Spark](https://spark.apache.org/) ist eine vereinheitlichte Engine zur Verarbeitung von umfangreichen Daten.

Über Maven sind zwei Versionen des Connectors verfügbar: eine mit 2.4.5 kompatible Version und eine mit 3.0.0 kompatible Version. Beide Versionen sind [hier](https://search.maven.org/search?q=spark-mssql-connector) erhältlich und können mit den folgenden Koordinaten importiert werden:

| Connector | Maven-Koordinate |
| --------- | ------------------ |
| Mit Spark 2.4.5 kompatibler Connector | `com.microsoft.azure:spark-mssql-connector:1.0.1` |
| Mit Spark 3.0.0 kompatibler Connector | `com.microsoft.azure:spark-mssql-connector_2.12_3.0:1.0.0-alpha` |

Ferner können Sie den Connector unter Verwendung einer Quelle erstellen oder die JAR-Datei aus dem Releaseabschnitt in GitHub herunterladen. Aktuelle Informationen zum Connector finden Sie unter dem [SQL Spark-Connector im GitHub-Repository](https://github.com/microsoft/sql-spark-connector).

## <a name="supported-features"></a>Unterstützte Funktionen

* Unterstützung für alle Spark-Bindungen (Scala, Python, R)
* Unterstützung der Standardauthentifizierung und der Active Directory (AD)-Schlüsseltabelle
* Geänderte Unterstützung für `dataframe`-Schreibvorgänge
* Unterstützung von Schreibvorgängen in SQL Server-Einzelinstanzen und Datenpools in Big Data-Clustern von SQL Server
* Zuverlässige Connectorunterstützung für SQL Server-Einzelinstanzen

| Komponente                            | Unterstützte Versionen              |
|--------------------------------------|---------------------------------|
| Apache Spark                         | 2.4.5, 3.0.0 |
| Scala                                | 2.11, 2.12                            |
| Microsoft JDBC-Treiber für SQL Server | 8,2                             |
| Microsoft SQL Server                 | SQL Server 2008 oder höher        |
| Azure SQL-Datenbank-Instanzen                  | Unterstützt                       |

> [!NOTE]
> Die Verwendung von Azure Synapse Analytics wurde mit diesem Connector nicht getestet. Obwohl die Verwendung möglicherweise funktioniert, kann sie unbeabsichtigte Auswirkungen haben.

### <a name="supported-options"></a>Unterstützte Optionen
Der Apache Spark-Connector für SQL Server und Azure SQL unterstützt die hier definierten Optionen: [SQL DataSource JDBC](https://spark.apache.org/docs/latest/sql-data-sources-jdbc.html)

Darüber hinaus werden die folgenden Optionen unterstützt

| Option | Standard | BESCHREIBUNG |
| --------- | ------------------ | ------------------------------------------ |
| `reliabilityLevel` | `BEST_EFFORT` | `BEST_EFFORT` oder `NO_DUPLICATES`. `NO_DUPLICATES` implementiert eine zuverlässige Einfügung in Neustartszenarios von Executor. |
| `dataPoolDataSource` | `none` | `none` impliziert, dass der Wert nicht festgelegt ist und dass der Connector in eine SQL Server-Einzelinstanz schreiben soll. Legen Sie diesen Wert auf einen Datenquellennamen fest, um eine Datenpooltabelle in Big Data-Clustern zu schreiben.|
| `isolationLevel` | `READ_COMMITTED` | Angeben der Isolationsstufe |
| `tableLock` | `false` | Implementiert eine Einfügung mit der `TABLOCK`-Option, um die Schreibleistung zu verbessern. |
| `schemaCheckEnabled` | `true` | Deaktiviert die strikte Schemaüberprüfung für Datenrahmen und SQL-Tabellen, wenn false festgelegt ist. |

Andere [Optionen zum Massenkopieren](../jdbc/using-bulk-copy-with-the-jdbc-driver.md#sqlserverbulkcopyoptions) können für `dataframe` als Optionen festgelegt werden und werden beim Schreiben an die `bulkcopy`-APIs übergeben.

## <a name="performance-comparison"></a>Leistungsvergleich

Der Apache Spark-Connector für SQL Server und Azure SQL ist beim Schreiben in SQL Server bis zu 15 Mal schneller als der generische JDBC-Connector. Die Leistungsmerkmale variieren abhängig vom Typ, der Datenmenge und den verwendeten Optionen und können von Ausführung zu Ausführung variieren. Die folgenden Leistungsergebnisse entsprechen der Zeit, die zum Überschreiben einer SQL-Tabelle mit 143,9 Mio. Zeilen in einem Spark-`dataframe` benötigt wurde. Der Spark-`dataframe` wird erstellt, indem die mit dem [Spark-TPCDS-Vergleichstest](https://github.com/databricks/spark-sql-perf) generierte `store_sales`-HDFS-Tabelle gelesen wird. Die Zeit zum Lesen von `store_sales` in `dataframe` wird ausgeschlossen. Die Ergebnisse werden über drei Ausführungen gemittelt.

| Connectortyp | Optionen | BESCHREIBUNG |  Schreibdauer |
| --------- | ------------------ | -------------------------------------| ---------- |
| `JDBCConnector` | Standard | Generischer JDBC-Connector mit Standardoptionen |  1\.385 Sekunden |
| `sql-spark-connector` | `BEST_EFFORT` | Bestmögliche `sql-spark-connector`-Leistung mit Standardoptionen |580 Sekunden |
| `sql-spark-connector` | `NO_DUPLICATES ` | Zuverlässige `sql-spark-connector`-Leistung | 709 Sekunden |
| `sql-spark-connector` | `BEST_EFFORT` + tabLock=true | Bestmögliche `sql-spark-connector`-Leistung mit aktivierter Tabellensperre | 72 Sekunden |
| `sql-spark-connector` | `NO_DUPLICATES ` + tabLock=true| Zuverlässige `sql-spark-connector`-Leistung mit aktivierter Tabellensperre| 198 Sekunden |

Konfigurationen

- Spark config: num_executors = 20, executor_memory = '1664 m', executor_cores = 2
- Data Gen config: scale_factor=50, partitioned_tables=true
- Datendatei `store_sales` mit 143.997.590 Zeilen

Umgebung

- [Big Data-Cluster von SQL Server](../../big-data-cluster/release-notes-big-data-cluster.md) CU5
- `master` + 6 Knoten
- Jeder Knoten auf einem Gen 5-Server verfügt über 512 GB RAM, 4 TB NVM pro Knoten und eine 10 GB-NIC

## <a name="commonly-faced-issues"></a>Häufige Probleme

### `java.lang.NoClassDefFoundError: com/microsoft/aad/adal4j/AuthenticationException`

Dieses Problem tritt bei Verwendung einer älteren Version des MSSQL-Treibers (der jetzt in diesem Connector enthalten ist) in einer Hadoop-Umgebung auf. Wenn Sie bisher den vorherigen Azure SQL-Connector verwendet und in diesem Cluster Treiber manuell installiert haben, um Kompatibilität mit Azure Active Directory zu gewährleisten, müssen Sie diese Treiber entfernen.

Schritte zum Beheben des Problems:

1. Wenn Sie eine generische Hadoop-Umgebung verwenden, überprüfen Sie die MSSQL-JAR-Datei `rm $HADOOP_HOME/share/hadoop/yarn/lib/mssql-jdbc-6.2.1.jre7.jar`, und entfernen Sie diese. Wenn Sie Databricks verwenden, fügen Sie ein globales oder Clusterinitialisierungsskript hinzu, um alte Versionen des MSSQL-Treibers aus dem Ordner `/databricks/jars` zu entfernen, oder fügen Sie die folgende Zeile in ein vorhandenes Skript ein: `rm /databricks/jars/*mssql*`
2. Fügen Sie die Pakete `adal4j` und `mssql` hinzu. Sie können z. B. Maven verwenden, allerdings sollten auch andere Tools funktionieren. 

   > [!CAUTION]
   > Der SQL Spark-Connector sollte nicht auf diese Weise installiert werden.

1. Fügen Sie der Verbindungskonfiguration die Treiberklasse hinzu. Beispiel:

   ```csharp
   connectionProperties = {
     `Driver`: `com.microsoft.sqlserver.jdbc.SQLServerDriver`
   }`
   ```

Weitere Informationen und Erläuterungen finden Sie im Abschnitt mit der Lösung zu [https://github.com/microsoft/sql-spark-connector/issues/26](https://github.com/microsoft/sql-spark-connector/issues/26#issuecomment-672006339).

## <a name="get-started"></a>Erste Schritte

Der Apache Spark-Connector für SQL Server und Azure SQL basiert auf der Spark DataSourceV1-API und der SQL Server-Bulk-API und verwendet dieselbe Schnittstelle wie der integrierte JDBC-Spark-SQL-Connector. Mit dieser Integration können Sie den Connector problemlos integrieren und vorhandene Spark-Aufträge migrieren, indem Sie den format-Parameter einfach mit `com.microsoft.sqlserver.jdbc.spark` aktualisieren.

Um den Connector in Ihre Projekte einzubinden, laden Sie dieses Repository herunter, und erstellen Sie die JAR-Datei mithilfe von SBT.

## <a name="write-to-a-new-sql-table"></a>Schreiben in eine neue SQL-Tabelle

> [!WARNING]
> Im `overwrite`-Modus wird standardmäßig zuerst die Tabelle gelöscht, wenn sie bereits in der Datenbank vorhanden ist. Verwenden Sie diese Option mit der gebotenen Sorgfalt, um unerwartete Datenverluste zu vermeiden.

Wenn Sie bei Verwendung des `overwrite`-Modus nicht die Option `truncate` verwenden, gehen bei der erneuten Erstellung der Tabelle Indizes verloren. So wäre eine columnstore-Tabelle nun ein Heap. Wenn Sie die vorhandene Indizierung beibehalten möchten, müssen Sie auch die Option `truncate` mit dem Wert „true“ angeben. Beispiel: `.option("truncate","true")`.

```python
server_name = "jdbc:sqlserver://{SERVER_ADDR}"
database_name = "database_name"
url = server_name + ";" + "databaseName=" + database_name + ";"

table_name = "table_name"
username = "username"
password = "password123!#" # Please specify password here

try:
  df.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .mode("overwrite") \
    .option("url", url) \
    .option("dbtable", table_name) \
    .option("user", username) \
    .option("password", password) \
    .save()
except ValueError as error :
    print("Connector write failed", error)
```

### <a name="append-to-sql-table"></a>Anfügen an eine SQL-Tabelle

```python
try:
  df.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .mode("append") \
    .option("url", url) \
    .option("dbtable", table_name) \
    .option("user", username) \
    .option("password", password) \
    .save()
except ValueError as error :
    print("Connector write failed", error)
```

## <a name="specify-the-isolation-level"></a>Angeben der Isolationsstufe

Beim Durchführen der Masseneinfügung in die Datenbank verwendet dieser Connector standardmäßig die Isolationsstufe `READ_COMMITTED`. Wenn Sie die Isolationsstufe außer Kraft setzen möchten, verwenden Sie die Option `mssqlIsolationLevel`, wie unten gezeigt.

```python
    .option("mssqlIsolationLevel", "READ_UNCOMMITTED") \
```

## <a name="read-from-sql-table"></a>Lesen aus einer SQL-Tabelle

```python
jdbcDF = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("user", username) \
        .option("password", password).load()
```

## <a name="azure-active-directory-authentication"></a>Azure Active Directory-Authentifizierung

### <a name="python-example-with-service-principal"></a>Python-Beispiel mit Dienstprinzipal

```python
context = adal.AuthenticationContext(authority)
token = context.acquire_token_with_client_credentials(resource_app_id_url, service_principal_id, service_principal_secret)
access_token = token["accessToken"]

jdbc_db = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("accessToken", access_token) \
        .option("encrypt", "true") \
        .option("hostNameInCertificate", "*.database.windows.net") \
        .load()
```

### <a name="python-example-with-active-directory-password"></a>Python-Beispiel mit Active Directory-Kennwort

```python
jdbc_df = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("authentication", "ActiveDirectoryPassword") \
        .option("user", user_name) \
        .option("password", password) \
        .option("encrypt", "true") \
        .option("hostNameInCertificate", "*.database.windows.net") \
        .load()
```

Eine erforderliche Abhängigkeit muss installiert werden, um die Authentifizierung mit Active Directory zu unterstützen.

Für **Scala** muss das `_com.microsoft.aad.adal4j_`-Artefakt installiert werden.

Für **Python** muss die `_adal_`-Bibliothek installiert werden.  Diese ist über pip verfügbar.

In den [Beispielnotebooks](https://github.com/microsoft/sql-spark-connector/tree/master/samples) finden Sie Beispiele.

## <a name="support"></a>Support

Der Apache Spark-Connector für Azure SQL und SQL Server ist ein Open-Source-Projekt. Dieser Connector wird nicht durch den Microsoft-Support unterstützt. Wenn Sie Probleme oder Fragen zum Connector haben, erstellen Sie einen Issue in diesem Projektrepository. Die Connectorcommunity ist aktiv und überwacht eingereichte Issues.

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie das [GitHub-Repository für den SQL Spark-Connector](https://github.com/microsoft/sql-spark-connector).

Informationen zu Isolationsstufen in SQL Server finden Sie unter [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).
