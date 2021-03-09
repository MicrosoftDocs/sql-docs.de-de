---
description: CREATE EXTERNAL DATA SOURCE (Transact-SQL)
title: CREATE EXTERNAL DATA SOURCE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/05/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE EXTERNAL DATA SOURCE
- CREATE_EXTERNAL_DATA_SOURCE
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, data source
- PolyBase, create data source
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4f503a3382f0ae4ec8ea7fb8f43e91254551e73c
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247367"
---
# <a name="create-external-data-source-transact-sql"></a>CREATE EXTERNAL DATA SOURCE (Transact-SQL)

Erstellt eine externe Datenquelle für Abfragen mithilfe von SQL Server, SQL-Datenbank, Azure Synapse Analytics oder Analytics Platform System (Parallel Data Warehouse oder PDW).

Dieser Artikel stellt die Syntax, Argumente, Anweisungen, Berechtigungen und Beispiele für das SQL-Produkt Ihrer Wahl bereit.

Weitere Informationen zu Syntaxkonventionen finden Sie unter [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL-Datenbank](create-external-data-source-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-data-source-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-data-source-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-sql-server"></a>Übersicht: SQL Server

Erstellt eine externe Datenquelle für PolyBase-Abfragen. Externe Datenquellen werden zum Herstellen von Verbindungen verwendet und unterstützen diese primären Anwendungsfälle:

- Datenvirtualisierung und Laden von Dateien mithilfe von [PolyBase][intro_pb]
- Massenladevorgänge mit `BULK INSERT` oder `OPENROWSET`

**Gilt für**: Seit [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]

## <a name="syntax"></a>Syntax

```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( [ LOCATION = '<prefix>://<path>[:<port>]' ]
    [ [ , ] CONNECTION_OPTIONS = '<key_value_pairs>'[,...]]
    [ [ , ] CREDENTIAL = <credential_name> ]
    [ [ , ] PUSHDOWN = { ON | OFF } ]
    [ [ , ] TYPE = { HADOOP | BLOB_STORAGE } ]
    [ [ , ] RESOURCE_MANAGER_LOCATION = '<resource_manager>[:<port>]' )
[ ; ]
```

## <a name="arguments"></a>Argumente

### <a name="data_source_name"></a>data_source_name

Gibt den benutzerdefinierten Namen für die Datenquelle an. Der Name muss innerhalb der Datenbank in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eindeutig sein.

### <a name="location--prefixpathport"></a>LOCATION = *`'<prefix>://<path[:port]>'`*

Stellt das Konnektivitätsprotokoll und den Pfad zur externe Datenquelle bereit.

| Externe Datenquelle    | Speicherort-Präfix | Location path (Pfad zum Speicherort)                                         | Unterstützte Standorte nach Produkt/Dienst |
| ----------------------- | --------------- | ----------------------------------------------------- | ---------------------------------------- |
| Cloudera oder Hortonworks | `hdfs`          | `<Namenode>[:port]`                                   | Seit [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]                       |
| Azure Storage-Konto (V2) | `wasb[s]`       | `<container>@<storage_account>.blob.core.windows.net` | Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] werden hierarchische Namespaces **nicht** unterstützt. |
| [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]              | `sqlserver`     | `<server_name>[\<instance_name>][:port]`              | Seit [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]                       |
| Oracle                  | `oracle`        | `<server_name>[:port]`                                | Seit [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]                       |
| Teradata                | `teradata`      | `<server_name>[:port]`                                | Seit [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]                       |
| MongoDB oder CosmosDB     | `mongodb`       | `<server_name>[:port]`                                | Seit [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]                       |
| ODBC                    | `odbc`          | `<server_name>[:port]`                                | Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] (nur unter Windows)        |
| Massenvorgänge         | `https`         | `<storage_account>.blob.core.windows.net/<container>` | Seit [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]                        |
| Edge Hub         | `edgehub`         | Nicht zutreffend | EdgeHub befindet sich immer lokal bei der [Azure SQL Edge](/azure/azure-sql-edge/overview/)-Instanz. Daher ist es nicht erforderlich, einen Pfad- oder Portwert anzugeben. Nur in Azure SQL Edge verfügbar.                      |
| Kafka        | `kafka`         | `<Kafka IP Address>[:port]` | Nur in Azure SQL Edge verfügbar.                      |

Speicherortpfad:

- `<`Namenode`>` = Der Name des Computers, der Namensdienst-URI oder die IP-Adresse von `Namenode` im Hadoop-Cluster. PolyBase muss DNS-Namen auflösen, die vom Hadoop-Cluster verwendet werden. <!-- For highly available Hadoop configurations, provide the Nameservice ID as the `LOCATION`. -->
- `port` = Der Port, an dem die externe Datenquelle lauscht. In Hadoop verwendet der Port den Konfigurationsparameter `fs.defaultFS`. Der Standardwert ist 8020.
- `<container>` = Der Container des Speicherkontos, der die Daten speichert. Stammcontainer sind schreibgeschützt und Daten können nicht zurück in den Container geschrieben werden.
- `<storage_account>` = Name des Speicherkontos der Azure-Ressource
- `<server_name>` = Hostname.
- `<instance_name>` = Der Name der von SQL Server benannten Instanz. Wird verwendet, wenn Sie den SQL Server-Browserdienst auf der Zielinstanz ausführen.

Zusätzliche Hinweise und Anweisungen für das Festlegen des Speicherorts:

- [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] überprüft die Existenz der externen Datenquelle nicht, wenn das Objekt erstellt wird. Erstellen Sie zum Überprüfen mithilfe der externe Datenquelle eine externe Tabelle.
- Verwenden Sie beim Abfragen von Hadoop für alle Tabellen die gleiche externe Datenquelle, um eine konsistente Abfragesemantik zu ermöglichen.
- Sie können das Speicherort-Präfix `sqlserver` verwenden, um eine Verbindung zwischen einer [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]-Instanz und einer anderen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz, einer [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]-Instanz oder mit Azure Synapse Analytics herzustellen.
- Geben Sie `Driver={<Name of Driver>}` an, wenn Sie sich über `ODBC` verbinden.
- `wasbs` ist optional, wird jedoch für den Zugriff auf Azure Storage-Konten empfohlen, da Daten mithilfe einer sicheren TLS/SSL-Verbindung gesendet werden.
- `abfs`- oder `abfss`-APIs werden beim Zugriff auf Azure Storage-Konten nicht unterstützt.
- Die Option „Hierarchischer Namespace“ für Azure Storage-Konten (V2) wird nicht unterstützt. Stellen Sie sicher, dass diese Option weiterhin **deaktiviert** ist.
- Für erfolgreiche PolyBase-Abfragen während eines Hadoop-`Namenode`-Failovers sollten Sie in Betracht ziehen, eine virtuelle IP-Adresse für den `Namenode` des Hadoop-Clusters zu verwenden. Falls nicht, führen Sie den Befehl [ALTER EXTERNAL DATA SOURCE][alter_eds] aus, um auf den neuen Speicherort zu verweisen.

### <a name="connection_options--key_value_pair"></a>CONNECTION_OPTIONS = *key_value_pair*

Gibt zusätzliche Optionen bei einer Verbindung über `ODBC` mit einer externen Datenquelle an. Wenn Sie mehrere Verbindungsoptionen verwenden möchten, trennen Sie diese durch ein Semikolon.


Diese Struktur gilt sowohl für generische `ODBC`-Verbindungen als auch integrierte `ODBC`-Connectors für SQL Server, Oracle, Teradata, MongoDB und CosmosDB.

`key_value_pair` ist das Schlüsselwort sowie der Wert für eine bestimmte Verbindungsoption. Die verfügbaren Schlüsselwörter und -werte sind vom externen Datenquellentyp abhängig. Der Name des Treibers ist mindestens erforderlich, es gibt jedoch auch andere Optionen wie `APP='<your_application_name>'` oder `ApplicationIntent= ReadOnly|ReadWrite`, die für die Angabe nützlich sind und bei der Problembehandlung hilfreich sind.

Weitere Informationen finden Sie hier:

- [Verwenden von Schlüsselwörtern für Verbindungszeichenfolgen][connection_options]
- [Verbindungszeichenfolgen-Schlüsselwörter für den ODBC-Treiber][connection_option_keyword]

### <a name="pushdown--on--off"></a>PUSHDOWN = *ON | OFF*

Gibt an, ob die Berechnung an die externe Datenquelle weitergegeben werden kann. Diese Option ist standardmäßig aktiviert.

`PUSHDOWN` wird unterstützt, wenn eine Verbindung zu SQL Server, Oracle, Teradata, MongoDB oder ODBC auf der Ebene der externen Datenquelle hergestellt wird.

Das Aktivieren oder Deaktivieren der Weitergabe auf Abfrageebene erfolgt über einen [Hinweis][hint_pb].

### <a name="credential--credential_name"></a>CREDENTIAL = *credential_name*

Gibt die datenbankbezogenen Anmeldeinformationen für die Authentifizierung mit der externen Datenquelle an.

Zusätzliche Hinweise und Anweisungen für das Erstellen der Anmeldeinformationen:

- `CREDENTIAL` ist nur erforderlich, wenn die Daten gesichert wurden. `CREDENTIAL` ist für Datasets, die den anonymen Zugriff zulassen, nicht erforderlich.
- Wenn `TYPE` = `BLOB_STORAGE`, müssen die Anmeldeinformationen mithilfe einer `SHARED ACCESS SIGNATURE` als Identität erstellt werden. Darüber hinaus sollte das SAS-Token wie folgt konfiguriert werden:
  - Schließen Sie bei der Konfiguration als Geheimnis das führende `?` aus.
  - Für die zu ladende Datei (z. B. `srt=o&sp=r`) benötigen Sie mindestens eine Leseberechtigung.
  - Verwenden Sie einen gültigen Ablaufzeitraum (alle Daten in UTC-Zeit).

Ein Beispiel für die Verwendung von `CREDENTIAL` mit `SHARED ACCESS SIGNATURE` und `TYPE` = `BLOB_STORAGE` finden Sie unter [Erstellen einer externen Datenquelle für Massenvorgänge, die Daten aus Azure Storage abrufen](#i-create-an-external-data-source-for-bulk-operations-retrieving-data-from-azure-storage).

Weitere Informationen zum Erstellen datenbankweit gültiger Anmeldeinformationen finden Sie unter [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc].

### <a name="type---hadoop--blob_storage-"></a>TYP = *[HADOOP | BLOB_STORAGE]*

Gibt den Typ der externe Datenquelle an, die konfiguriert wird. Dieser Parameter ist nicht immer erforderlich.

- Verwenden Sie HADOOP, wenn die externe Datenquelle Cloudera, Hortonworks oder ein Azure Storage-Konto ist.
- Verwenden Sie BLOB_STORAGE, wenn Sie mithilfe von [BULK INSERT][bulk_insert] oder [OPENROWSET][openrowset] mit [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] Massenvorgänge aus Azure Storage-Konten ausführen.

> [!IMPORTANT]
> Legen Sie `TYPE` nicht fest, wenn Sie eine andere externe Datenquelle verwenden.

Ein Beispiel für die Verwendung von `TYPE` = `HADOOP` zum Laden von Daten aus einem Azure Storage-Konto finden Sie unter [Erstellen einer externen Datenquelle für den Zugriff auf Daten in Azure Storage mithilfe der Schnittstelle „wasb://“](#e-create-external-data-source-to-access-data-in-azure-storage-using-the-wasb-interface). <!--[Create external data source to reference Azure Storage](#e-create-external-data-source-to-reference-azure-storage).-->

### <a name="resource_manager_location--resourcemanager_uriport"></a>RESOURCE_MANAGER_LOCATION = *'ResourceManager_URI[:port]'*

Konfigurieren Sie diesen optionalen Wert beim Herstellen einer Verbindung mit Hortonworks oder Cloudera.

Wenn `RESOURCE_MANAGER_LOCATION` definiert ist, trifft der Abfrageoptimierer zur Verbesserung der Leistung eine kostenorientierte Entscheidung. Ein MapReduce-Auftrag kann zum Übertragen der Berechnung an Hadoop verwendet werden. Das Angeben von `RESOURCE_MANAGER_LOCATION` kann das Volume der zwischen Hadoop und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] transferierten Daten erheblich reduzieren, was wiederum zu einer verbesserten Abfrageleistung führen kann.

Wenn der Ressourcen-Manager nicht angegeben ist, wird die Weitergabe der Berechnung an Hadoop für PolyBase-Abfragen deaktiviert.

Wenn kein Port angegeben ist, wird der Standardwert mithilfe der aktuellen Einstellung für die Konfiguration von „Hadoop Connectivity“ ausgewählt.

| Hadoop Connectivity | Standardport des Ressourcen-Managers |
| ------------------- | ----------------------------- |
| 1                   | 50300                         |
| 2                   | 50300                         |
| 3                   | 8021                          |
| 4                   | 8032                          |
| 5                   | 8050                          |
| 6                   | 8032                          |
| 7                   | 8050                          |

Eine vollständige Liste der unterstützten Hadoop-Versionen finden Sie unter [PolyBase Connectivity Configuration (Transact-SQL) (Konfiguration der PolyBase-Netzwerkkonnektivität (Transact-SQL))][connectivity_pb].

> [!IMPORTANT]  
> Der RESOURCE_MANAGER_LOCATION-Wert wird nicht überprüft, wenn Sie die externe Datenquelle erstellen. Das Eingeben eines falschen Werts verursacht zum Zeitpunkt der Ausführung einer Weitergabe gegebenenfalls einen Abfragefehler, da sich der bereitgestellte Wert nicht auflösen kann.

[Erstellen einer externen Datenquelle zum Verweisen auf Hadoop mit aktivierter Weitergabe](#c-create-external-data-source-to-reference-hadoop-with-push-down-enabled) stellt ein konkretes Beispiel und weitere Anleitungen bereit.

## <a name="permissions"></a>Berechtigungen

Die `CONTROL`-Berechtigung für die Datenbank in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist erforderlich.

## <a name="locking"></a>Sperren

Eine gemeinsame Sperre für das `EXTERNAL DATA SOURCE`-Objekt wird zugelassen.

## <a name="security"></a>Sicherheit

PolyBase unterstützt die proxybasierte Authentifizierung für die meisten externen Datenquellen. Erstellen Sie datenbankweit gültige Anmeldeinformationen, um das Proxykonto zu erstellen.

Wenn Sie eine Verbindung mit dem Speicher oder Datenpool in einem Big Data-Cluster in SQL Server herstellen, werden die Anmeldeinformationen des Benutzers an das Back-End-System übergeben. Erstellen Sie Anmeldenamen im Datenpool selbst, um die Pass-Through-Authentifizierung zu aktivieren.

Derzeit wird ein SAS-Token des Typs `HADOOP` nicht unterstützt. Dieses Token wird nur mit einem Speicherkonto-Zugriffsschlüssel unterstützt. Beim Erstellen einer externen Datenquelle mit dem Typ `HADOOP` und SAS-Anmeldeinformationen tritt folgender Fehler auf:

`Msg 105019, Level 16, State 1 - EXTERNAL TABLE access failed due to internal error: 'Java exception raised on call to HdfsBridge_Connect. Java exception message: Parameters provided to connect to the Azure storage account are not valid.: Error [Parameters provided to connect to the Azure storage account are not valid.] occurred while accessing external file.'`

## <a name="examples-starting-with-sssql16-md"></a>Beispiele (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])

> [!IMPORTANT]
> Informationen zum Installieren und Aktivieren von PolyBase finden Sie unter [Installieren von PolyBase unter Windows](../../relational-databases/polybase/polybase-installation.md).

### <a name="a-create-external-data-source-in-sql-server-2019-to-reference-oracle"></a>A. Erstellen einer externe Datenquelle in SQL Server 2019 für den Verweis auf Oracle

Stellen Sie sicher, dass Sie über datenbankweit gültige Anmeldeinformationen verfügen, um eine externe Datenquelle zu erstellen, die auf Oracle verweist. Optional können Sie auch die Weitergabe der Berechnung dieser Datenquelle aktivieren oder deaktivieren.

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '!MyC0mpl3xP@ssw0rd!' ;

-- Create a database scoped credential with Azure storage account key as the secret.
CREATE DATABASE SCOPED CREDENTIAL OracleProxyAccount
WITH
     IDENTITY = 'oracle_username',
     SECRET = 'oracle_password' ;

CREATE EXTERNAL DATA SOURCE MyOracleServer
WITH
  ( LOCATION = 'oracle://145.145.145.145:1521',
    CREDENTIAL = OracleProxyAccount,
    PUSHDOWN = ON
  ) ;
```

Weitere Beispiele für andere Datenquellen wie MongoDB finden Sie unter [Configure PolyBase to access external data in MongoDB (Konfigurieren von PolyBase für den Zugriff auf externe Daten in MongoDB)][mongodb_pb].

### <a name="b-create-external-data-source-to-reference-hadoop"></a>B. Erstellen einer externen Datenquelle, um auf Hadoop zu verweisen

Geben Sie den Computernamen oder die IP-Adresse von Hadoop-`Namenode` und des Ports an, um eine externe Datenquelle zu erstellen, die auf Ihre Hortonworks- oder Cloudera-Hadoop-Cluster verweist. <!-- Provide the Nameservice ID as the `LOCATION` for highly available configurations. -->

```sql
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8050' ,
    TYPE = HADOOP
  ) ;
```

### <a name="c-create-external-data-source-to-reference-hadoop-with-push-down-enabled"></a>C. Erstellen einer externen Datenquelle, um mit der aktivierten Weitergabe auf Hadoop zu verweisen

Geben Sie die Option `RESOURCE_MANAGER_LOCATION` an, um die Berechnung für PolyBase-Abfragen an Hadoop weiterzugeben. Nach der Aktivierung trifft PolyBase eine kostenorientierte Entscheidung, um zu bestimmen, ob die Abfrageberechnung an Hadoop weitergegeben werden soll.

```sql
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8020' ,
    TYPE = HADOOP ,
    RESOURCE_MANAGER_LOCATION = '10.10.10.10:8050'
  ) ;
```

### <a name="d-create-external-data-source-to-reference-kerberos-secured-hadoop"></a>D: Erstellen einer externen Datenquelle, um auf Kerberos-gesicherte Hadoop-Software zu verweisen

Um sicherzustellen, dass das Hadoop-Cluster mit Kerberos gesichert ist, können Sie den Wert der hadoop.security.authentication-Eigenschaft in „Hadoop-Core-site.xml“ überprüfen. Um auf ein Kerberos-gesichertes Hadoop-Cluster zu verweisen, müssen Sie datenbankweit gültige Anmeldeinformationen angeben, die Ihren Kerberos-Benutzernamen und Ihr Kennwort enthalten. Der Hauptschlüssel der Datenbank wird verwendet, um datenbankspezifische Anmeldeinformationen zu verschlüsseln.

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Kerberos user name and password.
CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
WITH
     IDENTITY = '<hadoop_user_name>',
     SECRET = '<hadoop_password>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8050' ,
    CREDENTIAL = HadoopUser1 ,
    TYPE = HADOOP ,
    RESOURCE_MANAGER_LOCATION = '10.10.10.10:8050'
  );
```

### <a name="e-create-external-data-source-to-access-data-in-azure-storage-using-the-wasb-interface"></a>E. Erstellen einer externen Datenquelle für den Zugriff auf Daten in Azure Storage mithilfe der Schnittstelle „wasb://“
In diesem Beispiel ist die externe Datenquelle ein Azure Storage-Konto (V2) namens `logs`. Der Container heißt `daily`. Die externe Azure Storage-Datenquelle dient nur der Datenübertragung. Die Prädikatweitergabe wird nicht unterstützt. Hierarchische Namespaces werden nicht unterstützt, wenn der Datenzugriff über die Schnittstelle `wasb://` erfolgt.

In diesem Beispiel wird gezeigt, wie Sie die datenbankweit gültigen Anmeldeinformationen für die Authentifizierung bei einem Azure Storage-Konto (V2) erstellen. Geben Sie den Azure Storage-Kontoschlüssel im Anmeldeinformationsgeheimnis der Datenbank an. Sie können eine beliebige Zeichenfolge in der datenbankweit gültigen Identität der Anmeldeinformationen angeben, da sie nicht für die Authentifizierung bei Azure Storage verwendet wird.

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Azure storage account key as the secret.
CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
  IDENTITY = '<my_account>' ,
  SECRET = '<azure_storage_account_key>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyAzureStorage
WITH
  ( LOCATION = 'wasbs://daily@logs.blob.core.windows.net/' ,
    CREDENTIAL = AzureStorageCredential ,
    TYPE = HADOOP
  ) ;
```

### <a name="f-create-external-data-source-to-reference-a-sql-server-named-instance-via-polybase-connectivity-sql-server-2019"></a>F. Erstellen einer externen Datenquelle für einen Verweis auf eine benannte SQL Server-Instanz über die PolyBase-Konnektivität ([!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Sie können eine externe Datenquelle erstellen, die auf eine benannte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz verweist, indem Sie `CONNECTION_OPTIONS` verwenden, um den Instanznamen anzugeben. 

Im folgenden Beispiel ist `WINSQL2019` der Hostname und `SQL2019` der Instanzname. `'Server=%s\SQL2019'` ist das Schlüssel-Wert-Paar.

```sql
CREATE EXTERNAL DATA SOURCE SQLServerInstance2
WITH (
  LOCATION = 'sqlserver://WINSQL2019' ,
  CONNECTION_OPTIONS = 'Server=%s\SQL2019' ,
  CREDENTIAL = SQLServerCredentials
) ;
```

Alternativ können Sie einen Port verwenden, um eine Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz herzustellen.

```sql
CREATE EXTERNAL DATA SOURCE SQLServerInstance2
WITH (
  LOCATION = 'sqlserver://WINSQL2019:58137' ,
  CREDENTIAL = SQLServerCredentials
) ;
```

### <a name="g-create-external-data-source-to-reference-kafka"></a>G. Erstellen einer externen Datenquelle für einen Verweis auf Kafka

In diesem Beispiel ist die externe Datenquelle ein Kafka-Server mit der IP-Adresse xxx.xxx.xxx.xxx, der an Port 1900 lauscht. Die externe Kafka-Datenquelle dient nur zum Streamen von Daten und unterstützt keinen Prädikatpushdown.

```sql
-- Create an External Data Source for Kafka
CREATE EXTERNAL DATA SOURCE MyKafkaServer WITH (
    LOCATION = 'kafka://xxx.xxx.xxx.xxx:1900'
)
go
```

### <a name="h-create-external-data-source-to-reference-edgehub"></a>H. Erstellen einer externen Datenquelle für einen Verweis auf EdgeHub

In diesem Beispiel ist die externe Datenquelle eine EdgeHub-Instanz, die auf demselben Edgegerät wie Azure SQL Edge ausgeführt wird. Die externe EdgeHub-Datenquelle dient nur zum Streamen von Daten und unterstützt keinen Prädikatpushdown.

```sql
-- Create an External Data Source for Kafka
CREATE EXTERNAL DATA SOURCE MyEdgeHub WITH (
    LOCATION = 'edgehub://'
)
go
```

## <a name="examples-bulk-operations"></a>Beispiele: Massenvorgänge

> [!IMPORTANT]
> An das Ende der `LOCATION`-URL darf kein **/** , Dateiname oder Shared Access Signature-Parameter hinzugefügt werden, wenn Sie eine externe Datenquelle für Massenvorgänge konfigurieren.

### <a name="i-create-an-external-data-source-for-bulk-operations-retrieving-data-from-azure-storage"></a>I. Erstellen einer externen Datenquelle für Massenvorgänge, die Daten aus Azure Storage abrufen

**Gilt für:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]
Verwenden Sie die folgende Datenquelle für Massenvorgänge mit [BULK INSERT][bulk_insert] oder [OPENROWSET][openrowset]. Die Anmeldeinformationen müssen `SHARED ACCESS SIGNATURE` als Identität festgelegt haben, dürfen kein führendes `?` im SAS-Token aufweisen, müssen mindestens Leseberechtigung für die zu ladende Datei besitzen (z. B. `srt=o&sp=r`), und ihr Ablaufdatum muss gültig sein (alle Datumsangaben sind in UTC-Zeit). Weitere Informationen zu SAS finden Sie unter [Verwenden von Shared Access Signatures (SAS)][sas_token].

```sql
CREATE DATABASE SCOPED CREDENTIAL AccessAzureInvoices
WITH
  IDENTITY = 'SHARED ACCESS SIGNATURE',
  -- Remove ? from the beginning of the SAS token
  SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************' ;

CREATE EXTERNAL DATA SOURCE MyAzureInvoices
WITH
  ( LOCATION = 'https://newinvoices.blob.core.windows.net/week3' ,
    CREDENTIAL = AccessAzureInvoices ,
    TYPE = BLOB_STORAGE
  ) ;
```

Dies wird beim [BULK INSERT][bulk_insert_example]-Beispiel praktisch veranschaulicht.

## <a name="see-also"></a>Weitere Informationen

- [ALTER EXTERNAL DATA SOURCE (Transact-SQL)][alter_eds]
- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]
- [CREATE EXTERNAL FILE FORMAT (Transact-SQL)][create_eff]
- [CREATE EXTERNAL TABLE (Transact-SQL)][create_etb]
- [sys.external_data_sources (Transact-SQL)][cat_eds]
- [Verwenden von Shared Access Signatures (SAS)][sas_token]

<!-- links to external pages -->
<!-- SQL Docs -->
[bulk_insert]: ./bulk-insert-transact-sql.md
[bulk_insert_example]: ./bulk-insert-transact-sql.md#f-importing-data-from-a-file-in-azure-blob-storage
[openrowset]: ../functions/openrowset-transact-sql.md

[create_dsc]: ./create-database-scoped-credential-transact-sql.md
[create_eff]: ./create-external-file-format-transact-sql.md
[create_etb]: ./create-external-table-transact-sql.md
[create_etb_as_sel]: ./create-external-table-as-select-transact-sql.md?view=azure-sqldw-latest&preserve-view=true
[create_tbl_as_sel]: ./create-table-as-select-azure-sql-data-warehouse.md?view=azure-sqldw-latest&preserve-view=true

[alter_eds]: ./alter-external-data-source-transact-sql.md

[cat_eds]: ../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md
<!-- PolyBase docs -->
[intro_pb]: ../../relational-databases/polybase/polybase-guide.md
[mongodb_pb]: ../../relational-databases/polybase/polybase-configure-mongodb.md
[connectivity_pb]:https://docs.microsoft.com/sql/database-engine/configure-windows/polybase-connectivity-configuration-transact-sql
[connection_options]: ../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md
[hint_pb]: ../../relational-databases/polybase/polybase-pushdown-computation.md#force-pushdown

<!-- Azure Docs -->
[sas_token]: /azure/storage/storage-dotnet-shared-access-signature-part-1

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-external-data-source-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL-Datenbank \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-data-source-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-data-source-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-sql-database"></a>Übersicht: Azure SQL-Datenbank

Erstellt eine externe Datenquelle für elastische Abfragen. Externe Datenquellen werden zum Herstellen von Verbindungen verwendet und unterstützen diese primären Anwendungsfälle:

- Massenladevorgänge mit `BULK INSERT` oder `OPENROWSET`
- Abfragen von SQL-Datenbank- oder Azure Synapse-Remoteinstanzen mithilfe von SQL-Datenbank mit [elastischen Abfragen][remote_eq]
- Abfragen von Shard-Azure SQL-Datenbank mithilfe einer [elastischen Abfrage][sharded_eq]

## <a name="syntax"></a>Syntax

```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( [ LOCATION = '<prefix>://<path>[:<port>]' ]
    [ [ , ] CREDENTIAL = <credential_name> ]
    [ [ , ] TYPE = { BLOB_STORAGE | RDBMS | SHARD_MAP_MANAGER } ]
    [ [ , ] DATABASE_NAME = '<database_name>' ]
    [ [ , ] SHARD_MAP_NAME = '<shard_map_manager>' ] )
[ ; ]
```

## <a name="arguments"></a>Argumente

### <a name="data_source_name"></a>data_source_name

Gibt den benutzerdefinierten Namen für die Datenquelle an. Dieser Name muss innerhalb der Datenbank in SQL-Datenbank eindeutig sein.

### <a name="location--prefixpathport"></a>LOCATION = *`'<prefix>://<path[:port]>'`*

Stellt das Konnektivitätsprotokoll und den Pfad zur externe Datenquelle bereit.

| Externe Datenquelle   | Speicherort-Präfix | Location path (Pfad zum Speicherort)                                         |
| ---------------------- | --------------- | ----------------------------------------------------- |
| Massenvorgänge        | `https`         | `<storage_account>.blob.core.windows.net/<container>` |
| Elastische Abfrage (Shard)  | Nicht erforderlich    | `<shard_map_server_name>.database.windows.net`        |
| Elastische Abfrage (Remote) | Nicht erforderlich    | `<remote_server_name>.database.windows.net`           |

Speicherortpfad:

- `<shard_map_server_name>` = Der logische Servername in Azure, der den Shardzuordnungs-Manager hostet. Das `DATABASE_NAME`-Argument stellt die Datenbank zum Hosten der Shardzuordnung bereit, und `SHARD_MAP_NAME` wird für die Shardzuordnung selbst verwendet.
- `<remote_server_name>` = Der Name des logischen Zielservers für die elastische Abfrage. Der Name der Datenbank wird mithilfe des `DATABASE_NAME`-Arguments angegeben.

Zusätzliche Hinweise und Anweisungen für das Festlegen des Speicherorts:

- [!INCLUDE[ssde_md](../../includes/ssde_md.md)] überprüft die Existenz der externen Datenquelle nicht, wenn das Objekt erstellt wird. Erstellen Sie zum Überprüfen mithilfe der externe Datenquelle eine externe Tabelle.

### <a name="credential--credential_name"></a>CREDENTIAL = *credential_name*

Gibt die datenbankbezogenen Anmeldeinformationen für die Authentifizierung mit der externen Datenquelle an.

Zusätzliche Hinweise und Anweisungen für das Erstellen der Anmeldeinformationen:

- Verwenden Sie zum Laden von Daten aus Azure Storage in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] eine Shared Access Signature (ein SAS-Token).
- `CREDENTIAL` ist nur erforderlich, wenn die Daten gesichert wurden. `CREDENTIAL` ist für Datasets, die den anonymen Zugriff zulassen, nicht erforderlich.
- Wenn `TYPE` = `BLOB_STORAGE`, müssen die Anmeldeinformationen mithilfe von `SHARED ACCESS SIGNATURE` als Identität erstellt werden. Darüber hinaus sollte das SAS-Token wie folgt konfiguriert werden:
  - Schließen Sie bei der Konfiguration als Geheimnis das führende `?` aus.
  - Für die zu ladende Datei (z. B. `srt=o&sp=r`) benötigen Sie mindestens eine Leseberechtigung.
  - Verwenden Sie einen gültigen Ablaufzeitraum (alle Daten in UTC-Zeit).

Ein Beispiel für die Verwendung von `CREDENTIAL` mit `SHARED ACCESS SIGNATURE` und `TYPE` = `BLOB_STORAGE` finden Sie unter [Erstellen einer externen Datenquelle für Massenvorgänge, die Daten aus Azure Storage abrufen](#c-create-an-external-data-source-for-bulk-operations-retrieving-data-from-azure-storage).

Weitere Informationen zum Erstellen datenbankweit gültiger Anmeldeinformationen finden Sie unter [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc].

### <a name="type---blob_storage--rdbms--shard_map_manager"></a>TYPE = *[ BLOB_STORAGE | RDBMS | SHARD_MAP_MANAGER]*

Gibt den Typ der externe Datenquelle an, die konfiguriert wird. Dieser Parameter ist nicht immer erforderlich.

- Verwenden Sie `RDBMS` für datenbankübergreifende Abfragen mit elastischen Abfragen in SQL-Datenbank.
- Verwenden Sie beim Erstellen einer externen Datenquelle `SHARD_MAP_MANAGER`, wenn Sie eine Verbindung mit einer Shard-SQL-Datenbank herstellen.
- Verwenden Sie `BLOB_STORAGE` beim Ausführen von Massenvorgängen mit [BULK INSERT][bulk_insert] oder [OPENROWSET][openrowset].

> [!IMPORTANT]
> Legen Sie `TYPE` nicht fest, wenn Sie eine andere externe Datenquelle verwenden.

### <a name="database_name--database_name"></a>DATABASE_NAME = *database_name*

Konfigurieren Sie dieses Argument, wenn der `TYPE` auf `RDBMS` oder `SHARD_MAP_MANAGER` festgelegt ist.

| TYPE              | Wert von DATABASE_NAME                                       |
| ----------------- | ------------------------------------------------------------ |
| RDBMS             | Der Name der Remotedatenbank auf dem mit `LOCATION` bereitgestellten Server. |
| SHARD_MAP_MANAGER | Der Name der Datenbank, die als Shardzuordnungs-Manager fungiert.      |

Ein Beispiel zum Erstellen einer externe Datenquelle, bei der `TYPE` = `RDBMS` ist, finden Sie unter [Erstellen einer externen RDBMS-Datenquelle](#b-create-an-rdbms-external-data-source).

### <a name="shard_map_name--shard_map_name"></a>SHARD_MAP_NAME = *shard_map_name*

Wird verwendet, wenn das `TYPE`-Argument auf `SHARD_MAP_MANAGER` festgelegt wird, nur um den Namen der Shardzuordnung festzulegen.

Ein Beispiel zum Erstellen einer externe Datenquelle, bei der `TYPE` = `SHARD_MAP_MANAGER` ist, finden Sie unter [Erstellen einer externen Datenquelle mithilfe des Shardzuordnungs-Managers](#a-create-a-shard-map-manager-external-data-source).

## <a name="permissions"></a>Berechtigungen

Die `CONTROL`-Berechtigung für die Datenbank in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ist erforderlich.

## <a name="locking"></a>Sperren

Eine gemeinsame Sperre für das `EXTERNAL DATA SOURCE`-Objekt wird zugelassen.

## <a name="examples"></a>Beispiele:

### <a name="a-create-a-shard-map-manager-external-data-source"></a>A. Erstellen einer externen Datenquelle für einen Shardzuordnungs-Manager

Zur Erstellung einer externen Datenquelle, die auf SHARD_MAP_MANAGER verweist, wird der Name des SQL-Datenbankservers angegeben, der den Shardzuordnungs-Manager in SQL-Datenbank oder der SQL Server-Datenbank auf einer VM hostet.

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
WITH
  IDENTITY = '<username>',
  SECRET = '<password>' ;

CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc
WITH
  ( TYPE = SHARD_MAP_MANAGER ,
    LOCATION = '<server_name>.database.windows.net' ,
    DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb' ,
    CREDENTIAL = ElasticDBQueryCred ,
    SHARD_MAP_NAME = 'CustomerIDShardMap'
  ) ;
```

Sie finden ein ausführliches Tutorial unter [Getting started with elastic queries for sharding (horizontal partitioning) (Erste Schritte mit elastischen Abfragen für Sharding (horizontales Partitionieren))][sharded_eq_tutorial].

### <a name="b-create-an-rdbms-external-data-source"></a>B. Erstellen einer externen RDBMS-Datenquelle

Zur Erstellung einer externen Datenquelle, die auf RDBMS verweist, wird der Name des SQL-Datenbankservers der Remotedatenbank in SQL-Datenbank angegeben.

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

CREATE DATABASE SCOPED CREDENTIAL SQL_Credential
WITH
  IDENTITY = '<username>' ,
  SECRET = '<password>' ;

CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc
WITH
  ( TYPE = RDBMS ,
    LOCATION = '<server_name>.database.windows.net' ,
    DATABASE_NAME = 'Customers' ,
    CREDENTIAL = SQL_Credential
  ) ;
```

Sie finden ein ausführliches Tutorial für RDBMS unter [Getting started with cross-database queries (vertical partitioning) (Erste Schritte mit datenbankübergreifenden Abfragen (vertikale Partitionierung))][remote_eq_tutorial].

## <a name="examples-bulk-operations"></a>Beispiele: Massenvorgänge

> [!IMPORTANT]
> An das Ende der `LOCATION`-URL darf kein **/** , Dateiname oder Shared Access Signature-Parameter hinzugefügt werden, wenn Sie eine externe Datenquelle für Massenvorgänge konfigurieren.

### <a name="c-create-an-external-data-source-for-bulk-operations-retrieving-data-from-azure-storage"></a>C. Erstellen einer externen Datenquelle für Massenvorgänge, die Daten aus Azure Storage abrufen

Verwenden Sie die folgende Datenquelle für Massenvorgänge mit [BULK INSERT][bulk_insert] oder [OPENROWSET][openrowset]. Die Anmeldeinformationen müssen `SHARED ACCESS SIGNATURE` als Identität festgelegt haben, dürfen kein führendes `?` im SAS-Token aufweisen, müssen mindestens Leseberechtigung für die zu ladende Datei besitzen (z. B. `srt=o&sp=r`), und ihr Ablaufdatum muss gültig sein (alle Datumsangaben sind in UTC-Zeit). Weitere Informationen zu SAS finden Sie unter [Verwenden von Shared Access Signatures (SAS)][sas_token].

```sql
CREATE DATABASE SCOPED CREDENTIAL AccessAzureInvoices
WITH
  IDENTITY = 'SHARED ACCESS SIGNATURE',
  -- Remove ? from the beginning of the SAS token
  SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************' ;

CREATE EXTERNAL DATA SOURCE MyAzureInvoices
WITH
  ( LOCATION = 'https://newinvoices.blob.core.windows.net/week3' ,
    CREDENTIAL = AccessAzureInvoices ,
    TYPE = BLOB_STORAGE
  ) ;
```

Sie finden dieses Beispiel unter [BULK INSERT][bulk_insert_example].

## <a name="see-also"></a>Weitere Informationen

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]
- [CREATE EXTERNAL TABLE (Transact-SQL)][create_etb]
- [sys.external_data_sources (Transact-SQL)][cat_eds]
- [Verwenden von Shared Access Signatures (SAS)][sas_token]
- [Einführung in elastische Abfragen][intro_eq]

<!-- links to external pages -->
<!-- SQL Docs -->
[bulk_insert]: ./bulk-insert-transact-sql.md
[bulk_insert_example]: ./bulk-insert-transact-sql.md#f-importing-data-from-a-file-in-azure-blob-storage
[openrowset]: ../functions/openrowset-transact-sql.md
[create_dsc]: ./create-database-scoped-credential-transact-sql.md
[create_etb]: https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source
[alter_eds]: ./alter-external-data-source-transact-sql.md
[cat_eds]: ../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md
<!-- PolyBase docs -->
[intro_pb]: ../../relational-databases/polybase/polybase-guide.md
[mongodb_pb]: ../../relational-databases/polybase/polybase-configure-mongodb.md
[connectivity_pb]:https://docs.microsoft.com/sql/database-engine/configure-windows/polybase-connectivity-configuration-transact-sql
[connection_options]: ../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md
[hint_pb]: ../../relational-databases/polybase/polybase-pushdown-computation.md#force-pushdown
<!-- Elastic Query Docs -->
[intro_eq]: /azure/azure-sql/database/elastic-query-overview
[remote_eq]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[remote_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[sharded_eq]: /azure/azure-sql/database/elastic-query-getting-started
[sharded_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started

<!-- Azure Docs -->
[azure_ad]: /azure/data-lake-store/data-lake-store-authenticate-using-active-directory
[sas_token]: /azure/storage/storage-dotnet-shared-access-signature-part-1

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-external-data-source-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](create-external-data-source-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-data-source-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-synapse-analytics"></a>Übersicht: Azure Synapse Analytics

Erstellt eine externe Datenquelle für PolyBase. Externe Datenquellen werden zum Herstellen von Verbindungen verwendet und unterstützen die folgenden primären Anwendungsfälle: Datenvirtualisierung und Laden von Dateien mithilfe von [PolyBase][intro_pb]

> [!IMPORTANT]  
> Informationen zum Erstellen einer externen Datenquelle zum Abfragen von SQL-Analyse-Ressourcen mithilfe von Azure SQL-Datenbank mit einer [elastischen Abfrage][remote_eq] finden Sie unter [SQL-Datenbank](create-external-data-source-transact-sql.md?view=azuresqldb-current&preserve-view=true).

## <a name="syntax"></a>Syntax

### [[!INCLUDE[sss-dedicated-pool-md.md](../../includes/sss-dedicated-pool-md.md)]](#tab/dedicated)
```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( [ LOCATION = '<prefix>://<path>[:<port>]' ]
    [ [ , ] CREDENTIAL = <credential_name> ]
    [ [ , ] TYPE = HADOOP ]
[ ; ]
```
### [[!INCLUDE[sssod-md.md](../../includes/sssod-md.md)]](#tab/serverless)
```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>  
WITH
(    LOCATION = '<prefix>://<path>[:<port>]'
) 
[;]
```
---

## <a name="arguments"></a>Argumente

### <a name="data_source_name"></a>data_source_name

Gibt den benutzerdefinierten Namen für die Datenquelle an. Der Name innerhalb von [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] muss eindeutig sein.

### <a name="location--prefixpathport"></a>LOCATION = *`'<prefix>://<path[:port]>'`*

Stellt das Konnektivitätsprotokoll und den Pfad zur externe Datenquelle bereit.

| Externe Datenquelle        | Speicherort-Präfix | Location path (Pfad zum Speicherort)                                         |
| --------------------------- | --------------- | ----------------------------------------------------- |
| Azure Data Lake Storage Gen 1 | `adl`           | `<storage_account>.azuredatalake.net`                 |
| Azure Data Lake Storage Gen 2 | `abfs[s]`       | `<container>@<storage_account>.dfs.core.windows.net`  |
| Azure Storage-Konto (V2)    | `wasb[s]`       | `<container>@<storage_account>.blob.core.windows.net` |

Speicherortpfad:

- `<container>` = Der Container des Speicherkontos, der die Daten speichert. Stammcontainer sind schreibgeschützt und Daten können nicht zurück in den Container geschrieben werden.
- `<storage_account>` = Name des Speicherkontos der Azure-Ressource

Zusätzliche Hinweise und Anweisungen für das Festlegen des Speicherorts:

- Bei der Bereitstellung von Azure Data Lake Storage Gen2 ist die Standardoption `enable secure SSL connections`. Wenn diese Option aktiviert ist, müssen Sie `abfss` verwenden, wenn eine sichere TLS/SSL-Verbindung ausgewählt ist. Beachten Sie, dass `abfss` auch für unsichere TLS-Verbindungen funktioniert.
- Azure Synapse überprüft die Existenz der externen Datenquelle nicht, wenn das Objekt erstellt wird. . Erstellen Sie zum Überprüfen mithilfe der externe Datenquelle eine externe Tabelle.
- Verwenden Sie beim Abfragen von Hadoop für alle Tabellen die gleiche externe Datenquelle, um eine konsistente Abfragesemantik zu ermöglichen.
- `wasbs` wird empfohlen, da Daten mithilfe einer sicheren TLS-Verbindung gesendet werden.
- Hierarchische Namespaces werden für Azure Storage-Konten (V2) nicht unterstützt, wenn der Datenzugriff über PolyBase mithilfe der Schnittstelle „wasb://“ erfolgt.

### <a name="credential--credential_name"></a>CREDENTIAL = *credential_name*

Gibt die datenbankbezogenen Anmeldeinformationen für die Authentifizierung mit der externen Datenquelle an.

Zusätzliche Hinweise und Anweisungen für das Erstellen der Anmeldeinformationen:

- Verwenden Sie einen Azure Storage-Schlüssel, um Daten aus Azure Storage oder Azure Data Lake Storage Gen2 (ADLS) in Azure Synapse Analytics zu laden.
- `CREDENTIAL` ist nur erforderlich, wenn die Daten gesichert wurden. `CREDENTIAL` ist für Datasets, die den anonymen Zugriff zulassen, nicht erforderlich.

Weitere Informationen zum Erstellen datenbankweit gültiger Anmeldeinformationen finden Sie unter [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc].

### <a name="type--hadoop"></a>TYPE = *HADOOP*

Gibt den Typ der externe Datenquelle an, die konfiguriert wird. Dieser Parameter ist nicht immer erforderlich.

- Verwenden Sie HADOOP, wenn die externe Datenquelle Azure Storage, ADLS Gen1 oder ADLS Gen2 ist.

Ein Beispiel für die Verwendung von `TYPE` = `HADOOP` zum Laden von Daten aus Azure Storage finden Sie unter [Erstellen einer externen Datenquelle für Verweise auf Azure Data Lake Storage Gen1 oder Gen2 mithilfe eines Dienstprinzipals](#b-create-external-data-source-to-reference-azure-data-lake-store-gen-1-or-2-using-a-service-principal).

## <a name="permissions"></a>Berechtigungen

Erfordert die `CONTROL`-Berechtigung für die Datenbank.

## <a name="locking"></a>Sperren

Eine gemeinsame Sperre für das `EXTERNAL DATA SOURCE`-Objekt wird zugelassen.

## <a name="security"></a>Sicherheit

PolyBase unterstützt die proxybasierte Authentifizierung für die meisten externen Datenquellen. Erstellen Sie datenbankweit gültige Anmeldeinformationen, um das Proxykonto zu erstellen.

Wenn Sie eine Verbindung mit dem Speicher oder Datenpool in einem Big Data-Cluster in SQL Server herstellen, werden die Anmeldeinformationen des Benutzers an das Back-End-System übergeben. Erstellen Sie Anmeldenamen im Datenpool selbst, um die Pass-Through-Authentifizierung zu aktivieren.

Derzeit wird ein SAS-Token des Typs `HADOOP` nicht unterstützt. Dieses Token wird nur mit einem Speicherkonto-Zugriffsschlüssel unterstützt. Beim Erstellen einer externen Datenquelle mit dem Typ `HADOOP` und SAS-Anmeldeinformationen tritt folgender Fehler auf:

`Msg 105019, Level 16, State 1 - EXTERNAL TABLE access failed due to internal error: 'Java exception raised on call to HdfsBridge_Connect. Java exception message: Parameters provided to connect to the Azure storage account are not valid.: Error [Parameters provided to connect to the Azure storage account are not valid.] occurred while accessing external file.'`

## <a name="examples"></a>Beispiele:

### <a name="a-create-external-data-source-to-access-data-in-azure-storage-using-the-wasb-interface"></a>A. Erstellen einer externen Datenquelle für den Zugriff auf Daten in Azure Storage mithilfe der Schnittstelle „wasb://“
In diesem Beispiel ist die externe Datenquelle ein Azure Storage-Konto (V2) namens `logs`. Der Container heißt `daily`. Die externe Azure Storage-Datenquelle dient nur der Datenübertragung. Die Prädikatweitergabe wird nicht unterstützt. Hierarchische Namespaces werden nicht unterstützt, wenn der Datenzugriff über die Schnittstelle `wasb://` erfolgt.

In diesem Beispiel wird gezeigt, wie Sie die datenbankweit gültigen Anmeldeinformationen für die Authentifizierung bei Azure Storage erstellen. Geben Sie den Azure Storage-Kontoschlüssel im Anmeldeinformationsgeheimnis der Datenbank an. Sie können eine beliebige Zeichenfolge in der datenbankweit gültigen Identität der Anmeldeinformationen angeben, da sie nicht für die Authentifizierung bei Azure Storage verwendet wird.

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Azure storage account key as the secret.
CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
  IDENTITY = '<my_account>',
  SECRET = '<azure_storage_account_key>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyAzureStorage
WITH
  ( LOCATION = 'wasbs://daily@logs.blob.core.windows.net/' ,
    CREDENTIAL = AzureStorageCredential ,
    TYPE = HADOOP
  ) ;
```

### <a name="b-create-external-data-source-to-reference-azure-data-lake-store-gen-1-or-2-using-a-service-principal"></a>B. Erstellen einer externen Datenquelle für Verweise auf Azure Data Lake Storage Gen1 oder Gen2 mithilfe eines Dienstprinzipals

Die Azure Data Lake Storage-Konnektivität kann auf Ihrem ADLS-URI und dem Dienstprinzipal Ihrer Azure Active Directory-Anwendung basieren. Die Dokumentation zum Erstellen dieser Anwendung finden Sie unter [Authentifizierung bei Data Lake Storage mit Active Directory][azure_ad].

```sql
-- If you do not have a Master Key on your DW you will need to create one.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

-- These values come from your Azure Active Directory Application used to authenticate to ADLS
CREATE DATABASE SCOPED CREDENTIAL ADLS_credential
WITH
  -- IDENTITY = '<clientID>@<OAuth2.0TokenEndPoint>' ,
  IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token' ,
  -- SECRET = '<KEY>'
  SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI=' 
;

-- For Gen 1 - Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen 1 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step
CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH
  ( LOCATION = 'adl://newyorktaxidataset.azuredatalakestore.net' ,
    CREDENTIAL = ADLS_credential ,
    TYPE = HADOOP
  ) ;

-- For Gen 2 - Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen 2 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step
CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH
  -- Please note the abfss endpoint when your account has secure transfer enabled
  ( LOCATION = 'abfss://data@newyorktaxidataset.dfs.core.windows.net' , 
    CREDENTIAL = ADLS_credential ,
    TYPE = HADOOP
  ) ;
```

### <a name="c-create-external-data-source-to-reference-azure-data-lake-store-gen-2-using-the-storage-account-key"></a>C. Erstellen einer externen Datenquelle für Verweise auf Azure Data Lake Storage Gen2 mithilfe des Speicherkontoschlüssels

```sql
-- If you do not have a Master Key on your DW you will need to create one.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

CREATE DATABASE SCOPED CREDENTIAL ADLS_credential
WITH
-- IDENTITY = '<storage_account_name>' ,
  IDENTITY = 'newyorktaxidata' ,
-- SECRET = '<storage_account_key>'
  SECRET = 'yz5N4+bxSb89McdiysJAzo+9hgEHcJRJuXbF/uC3mhbezES/oe00vXnZEl14U0lN3vxrFKsphKov16C0w6aiTQ=='
;

-- Note this example uses a Gen 2 secured endpoint (abfss)
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( LOCATION = 'abfss://2013@newyorktaxidataset.dfs.core.windows.net' ,
    CREDENTIAL = ADLS_credential ,
    TYPE = HADOOP
  ) ;
```

### <a name="d-create-external-data-source-to-reference-polybase-connectivity-to-azure-data-lake-store-gen-2-using-abfs"></a>D: Erstellen einer externen Datenquelle für einen Verweis auf PolyBase-Verbindungen mit Azure Data Lake Storage Gen2 mithilfe von „abfs://“

Wenn eine Verbindung mit einem Konto in Azure Data Lake Storage Gen 2 mithilfe einer [verwalteten Identität](/azure/active-directory/managed-identities-azure-resources/overview
) hergestellt wird, muss SECRET nicht angegeben werden.

```sql
-- If you do not have a Master Key on your DW you will need to create one
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

--Create database scoped credential with **IDENTITY = 'Managed Service Identity'**

CREATE DATABASE SCOPED CREDENTIAL msi_cred 
WITH IDENTITY = 'Managed Service Identity' ;

--Create external data source with abfss:// scheme for connecting to your Azure Data Lake Store Gen2 account

CREATE EXTERNAL DATA SOURCE ext_datasource_with_abfss 
WITH 
  ( TYPE = HADOOP , 
    LOCATION = 'abfss://myfile@mystorageaccount.dfs.core.windows.net' , 
    CREDENTIAL = msi_cred
  ) ;
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]
- [CREATE EXTERNAL FILE FORMAT (Transact-SQL)][create_eff]
- [CREATE EXTERNAL TABLE (Transact-SQL)][create_etb]
- [CREATE EXTERNAL TABLE AS SELECT (Azure Synapse Analytics)][create_etb_as_sel]
- [CREATE TABLE AS SELECT (Azure Synapse Analytics)][create_tbl_as_sel]
- [sys.external_data_sources (Transact-SQL)][cat_eds]
- [Verwenden von Shared Access Signatures (SAS)][sas_token]

<!-- links to external pages -->
<!-- SQL Docs -->
[bulk_insert]: ./bulk-insert-transact-sql.md
[bulk_insert_example]: ./bulk-insert-transact-sql.md#f-importing-data-from-a-file-in-azure-blob-storage
[openrowset]: ../functions/openrowset-transact-sql.md

[create_dsc]: ./create-database-scoped-credential-transact-sql.md
[create_eff]: ./create-external-file-format-transact-sql.md
[create_etb]: https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source
[create_etb_as_sel]: ./create-external-table-as-select-transact-sql.md?view=azure-sqldw-latest&preserve-view=true
[create_tbl_as_sel]: ./create-table-as-select-azure-sql-data-warehouse.md?view=azure-sqldw-latest&preserve-view=true

[alter_eds]: ./alter-external-data-source-transact-sql.md

[cat_eds]: ../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md
<!-- PolyBase docs -->
[intro_pb]: ../../relational-databases/polybase/polybase-guide.md
[mongodb_pb]: ../../relational-databases/polybase/polybase-configure-mongodb.md
[connectivity_pb]:https://docs.microsoft.com/sql/database-engine/configure-windows/polybase-connectivity-configuration-transact-sql
[connection_options]: ../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md
[hint_pb]: ../../relational-databases/polybase/polybase-pushdown-computation.md#force-pushdown
<!-- Elastic Query Docs -->
[intro_eq]: /azure/azure-sql/database/elastic-query-overview
[remote_eq]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[remote_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[sharded_eq]: /azure/azure-sql/database/elastic-query-getting-started
[sharded_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started

<!-- Azure Docs -->
[azure_ad]: /azure/data-lake-store/data-lake-store-authenticate-using-active-directory
[sas_token]: /azure/storage/storage-dotnet-shared-access-signature-part-1

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-external-data-source-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](create-external-data-source-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-data-source-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-analytics-platform-system"></a>Übersicht: Analyseplattformsystem

Erstellt eine externe Datenquelle für PolyBase-Abfragen. Externe Datenquellen werden zum Herstellen von Verbindungen verwendet und unterstützen den folgenden Anwendungsfall: Datenvirtualisierung und Laden von Dateien mithilfe von [PolyBase][intro_pb]

## <a name="syntax"></a>Syntax

```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( [ LOCATION = '<prefix>://<path>[:<port>]' ]
    [ [ , ] CREDENTIAL = <credential_name> ]
    [ [ , ] TYPE = HADOOP ]
    [ [ , ] RESOURCE_MANAGER_LOCATION = '<resource_manager>[:<port>]' )
[ ; ]
```

## <a name="arguments"></a>Argumente

### <a name="data_source_name"></a>data_source_name

Gibt den benutzerdefinierten Namen für die Datenquelle an. Der Name muss innerhalb des Servers in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] eindeutig.

### <a name="location--prefixpathport"></a>LOCATION = *`'<prefix>://<path[:port]>'`*

Stellt das Konnektivitätsprotokoll und den Pfad zur externe Datenquelle bereit.

| Externe Datenquelle    | Speicherort-Präfix | Location path (Pfad zum Speicherort)                                         |
| ----------------------- | --------------- | ----------------------------------------------------- |
| Cloudera oder Hortonworks | `hdfs`          | `<Namenode>[:port]`                                   |
| Azure Storage-Konto   | `wasb[s]`       | `<container>@<storage_account>.blob.core.windows.net` |

Speicherortpfad:

- `<Namenode>`: Der Name des Computers, der Namensdienst-URI oder die IP-Adresse von `Namenode` im Hadoop-Cluster. PolyBase muss DNS-Namen auflösen, die vom Hadoop-Cluster verwendet werden. <!-- For highly available Hadoop configurations, provide the Nameservice ID as the `LOCATION`. -->
- `port` = Der Port, an dem die externe Datenquelle lauscht. In Hadoop verwendet der Port den Konfigurationsparameter `fs.defaultFS`. Der Standardwert ist 8020.
- `<container>` = Der Container des Speicherkontos, der die Daten speichert. Stammcontainer sind schreibgeschützt und Daten können nicht zurück in den Container geschrieben werden.
- `<storage_account>` = Name des Speicherkontos der Azure-Ressource

Zusätzliche Hinweise und Anweisungen für das Festlegen des Speicherorts:

- Die PDW-Engine überprüft die Existenz der externen Datenquelle nicht, wenn das Objekt erstellt wird. Erstellen Sie zum Überprüfen mithilfe der externe Datenquelle eine externe Tabelle.
- Verwenden Sie beim Abfragen von Hadoop für alle Tabellen die gleiche externe Datenquelle, um eine konsistente Abfragesemantik zu ermöglichen.
- `wasbs` wird empfohlen, da Daten mithilfe einer sicheren TLS-Verbindung gesendet werden.
- Hierarchische Namespaces können nicht zusammen mit Azure Storage-Konten in „wasb://“ verwendet werden.
- Für erfolgreiche PolyBase-Abfragen während eines Hadoop-`Namenode`-Failovers sollten Sie in Betracht ziehen, eine virtuelle IP-Adresse für den `Namenode` des Hadoop-Clusters zu verwenden. Falls nicht, führen Sie den Befehl [ALTER EXTERNAL DATA SOURCE][alter_eds] aus, um auf den neuen Speicherort zu verweisen.

### <a name="credential--credential_name"></a>CREDENTIAL = *credential_name*

Gibt die datenbankbezogenen Anmeldeinformationen für die Authentifizierung mit der externen Datenquelle an.

Zusätzliche Hinweise und Anweisungen für das Erstellen der Anmeldeinformationen:

- Wenn Sie Daten aus Azure Storage in Azure Synapse oder PDW laden möchten, verwenden Sie einen Azure Storage-Schlüssel.
- `CREDENTIAL` ist nur erforderlich, wenn die Daten gesichert wurden. `CREDENTIAL` ist für Datasets, die den anonymen Zugriff zulassen, nicht erforderlich.

### <a name="type---hadoop-"></a>TYPE = *[ HADOOP ]*

Gibt den Typ der externe Datenquelle an, die konfiguriert wird. Dieser Parameter ist nicht immer erforderlich.

- Verwenden Sie HADOOP, wenn die externe Datenquelle Cloudera, Hortonworks oder Azure Storage ist.

Ein Beispiel für die Verwendung von `TYPE` = `HADOOP` zum Laden von Daten aus Azure Storage finden Sie unter [Erstellen einer externen Datenquelle, um auf Hadoop zu verweisen](#a-create-external-data-source-to-reference-hadoop).

### <a name="resource_manager_location--resourcemanager_uriport"></a>RESOURCE_MANAGER_LOCATION = *'ResourceManager_URI[:port]'*

Konfigurieren Sie diesen optionalen Wert beim Herstellen einer Verbindung mit Hortonworks oder Cloudera.

Wenn `RESOURCE_MANAGER_LOCATION` definiert ist, trifft der Abfrageoptimierer zur Verbesserung der Leistung eine kostenorientierte Entscheidung. Ein MapReduce-Auftrag kann zum Übertragen der Berechnung an Hadoop verwendet werden. Das Angeben von `RESOURCE_MANAGER_LOCATION` kann das Volume der zwischen Hadoop und SQL transferierten Daten erheblich reduzieren, was wiederum zu einer verbesserten Abfrageleistung führen kann.

Wenn der Ressourcen-Manager nicht angegeben ist, wird die Weitergabe der Berechnung an Hadoop für PolyBase-Abfragen deaktiviert.

Wenn kein Port angegeben ist, wird der Standardwert mithilfe der aktuellen Einstellung für die Konfiguration von „Hadoop Connectivity“ ausgewählt.

| Hadoop Connectivity | Standardport des Ressourcen-Managers |
| ------------------- | ----------------------------- |
| 1                   | 50300                         |
| 2                   | 50300                         |
| 3                   | 8021                          |
| 4                   | 8032                          |
| 5                   | 8050                          |
| 6                   | 8032                          |
| 7                   | 8050                          |

Eine vollständige Liste der unterstützten Hadoop-Versionen finden Sie unter [PolyBase Connectivity Configuration (Transact-SQL) (Konfiguration der PolyBase-Netzwerkkonnektivität (Transact-SQL))][connectivity_pb].

> [!IMPORTANT]
> Der `RESOURCE_MANAGER_LOCATION`-Wert wird nicht überprüft, wenn Sie die externe Datenquelle erstellen. Das Eingeben eines falschen Werts verursacht zum Zeitpunkt der Ausführung einer Weitergabe gegebenenfalls einen Abfragefehler, da sich der bereitgestellte Wert nicht auflösen kann.

[Erstellen einer externen Datenquelle zum Verweisen auf Hadoop mit aktivierter Weitergabe](#b-create-external-data-source-to-reference-hadoop-with-push-down-enabled) stellt ein konkretes Beispiel und weitere Anleitungen bereit.

## <a name="permissions"></a>Berechtigungen

Die `CONTROL`-Berechtigung für die Datenbank in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ist erforderlich.

> [!NOTE]
> In vorherigen Releases von PDW waren `ALTER ANY EXTERNAL DATA SOURCE`-Berechtigungen beim Erstellen von externen Datenquellen erforderlich.

## <a name="locking"></a>Sperren

Eine gemeinsame Sperre für das `EXTERNAL DATA SOURCE`-Objekt wird zugelassen.

## <a name="security"></a>Sicherheit

PolyBase unterstützt die proxybasierte Authentifizierung für die meisten externen Datenquellen. Erstellen Sie datenbankweit gültige Anmeldeinformationen, um das Proxykonto zu erstellen.

Derzeit wird ein SAS-Token des Typs `HADOOP` nicht unterstützt. Dieses Token wird nur mit einem Speicherkonto-Zugriffsschlüssel unterstützt. Beim Erstellen einer externen Datenquelle mit dem Typ `HADOOP` und SAS-Anmeldeinformationen tritt folgender Fehler auf:

`Msg 105019, Level 16, State 1 - EXTERNAL TABLE access failed due to internal error: 'Java exception raised on call to HdfsBridge_Connect. Java exception message: Parameters provided to connect to the Azure storage account are not valid.: Error [Parameters provided to connect to the Azure storage account are not valid.] occurred while accessing external file.'`

## <a name="examples"></a>Beispiele:

### <a name="a-create-external-data-source-to-reference-hadoop"></a>A. Erstellen einer externen Datenquelle, um auf Hadoop zu verweisen

Geben Sie den Computernamen oder die IP-Adresse von Hadoop-`Namenode` und des Ports an, um eine externe Datenquelle zu erstellen, die auf Ihre Hortonworks- oder Cloudera-Hadoop-Cluster verweist. <!-- Provide the Nameservice ID as the `LOCATION` for highly available configurations. -->

```sql
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8050' ,
    TYPE = HADOOP
  ) ;
```

### <a name="b-create-external-data-source-to-reference-hadoop-with-push-down-enabled"></a>B. Erstellen einer externen Datenquelle, um mit der aktivierten Weitergabe auf Hadoop zu verweisen

Geben Sie die Option `RESOURCE_MANAGER_LOCATION` an, um die Berechnung für PolyBase-Abfragen an Hadoop weiterzugeben. Nach der Aktivierung trifft PolyBase eine kostenorientierte Entscheidung, um zu bestimmen, ob die Abfrageberechnung an Hadoop weitergegeben werden soll.

```sql
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8020'
    TYPE = HADOOP
    RESOURCE_MANAGER_LOCATION = '10.10.10.10:8050'
) ;
```

### <a name="c-create-external-data-source-to-reference-kerberos-secured-hadoop"></a>C. Erstellen einer externen Datenquelle, um auf Kerberos-gesicherte Hadoop-Software zu verweisen

Um sicherzustellen, dass das Hadoop-Cluster mit Kerberos gesichert ist, können Sie den Wert der hadoop.security.authentication-Eigenschaft in „Hadoop-Core-site.xml“ überprüfen. Um auf ein Kerberos-gesichertes Hadoop-Cluster zu verweisen, müssen Sie datenbankweit gültige Anmeldeinformationen angeben, die Ihren Kerberos-Benutzernamen und Ihr Kennwort enthalten. Der Hauptschlüssel der Datenbank wird verwendet, um datenbankspezifische Anmeldeinformationen zu verschlüsseln.

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Kerberos user name and password.
CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
WITH
  IDENTITY = '<hadoop_user_name>' ,
  SECRET = '<hadoop_password>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8050' ,
    CREDENTIAL = HadoopUser1 ,
    TYPE = HADOOP ,
    RESOURCE_MANAGER_LOCATION = '10.10.10.10:8050'
  ) ;
```

### <a name="d-create-external-data-source-to-access-data-in-azure-storage-using-the-wasb-interface"></a>D: Erstellen einer externen Datenquelle für den Zugriff auf Daten in Azure Storage mithilfe der Schnittstelle „wasb://“

In diesem Beispiel ist die externe Datenquelle ein Azure Storage-Konto (V2) namens `logs`. Der Container heißt `daily`. Die externe Azure Storage-Datenquelle dient nur der Datenübertragung. Die Prädikatweitergabe wird nicht unterstützt. Hierarchische Namespaces werden nicht unterstützt, wenn der Datenzugriff über die Schnittstelle `wasb://` erfolgt.

Dieses Beispiel zeigt, wie Sie die datenbankweit gültigen Anmeldeinformationen für die Authentifizierung für Azure Storage erstellen. Geben Sie den Azure-Speicherkontoschlüssel in den Anmeldeinformation für die Datenbank an. Sie können eine beliebige Zeichenfolge in der datenbankweit gültigen Identität der Anmeldeinformationen angeben, da sie nicht für die Authentifizierung bei Azure Storage verwendet wird.

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Azure storage account key as the secret.
CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
  IDENTITY = '<my_account>' ,
  SECRET = '<azure_storage_account_key>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyAzureStorage
WITH
  ( LOCATION = 'wasbs://daily@logs.blob.core.windows.net/'
    CREDENTIAL = AzureStorageCredential
    TYPE = HADOOP
  ) ;
```


## <a name="see-also"></a>Weitere Informationen

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]
- [CREATE EXTERNAL FILE FORMAT (Transact-SQL)][create_eff]
- [CREATE EXTERNAL TABLE (Transact-SQL)][create_etb]
- [sys.external_data_sources (Transact-SQL)][cat_eds]
- [Verwenden von Shared Access Signatures (SAS)][sas_token]

<!-- links to external pages -->
<!-- SQL Docs -->
[bulk_insert]: ./bulk-insert-transact-sql.md
[bulk_insert_example]: ./bulk-insert-transact-sql.md#f-importing-data-from-a-file-in-azure-blob-storage
[openrowset]: ../functions/openrowset-transact-sql.md

[create_dsc]: ./create-database-scoped-credential-transact-sql.md
[create_eff]: ./create-external-file-format-transact-sql.md
[create_etb]: https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source
[create_etb_as_sel]: ./create-external-table-as-select-transact-sql.md?view=azure-sqldw-latest&preserve-view=true
[create_tbl_as_sel]: ./create-table-as-select-azure-sql-data-warehouse.md?view=azure-sqldw-latest&preserve-view=true

[alter_eds]: ./alter-external-data-source-transact-sql.md

[cat_eds]: ../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md
<!-- PolyBase docs -->
[intro_pb]: ../../relational-databases/polybase/polybase-guide.md
[mongodb_pb]: ../../relational-databases/polybase/polybase-configure-mongodb.md
[connectivity_pb]:https://docs.microsoft.com/sql/database-engine/configure-windows/polybase-connectivity-configuration-transact-sql
[connection_options]: ../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md
[connection_option_keyword]: ../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md#odbc-driver-connection-string-keywords
[hint_pb]: ../../relational-databases/polybase/polybase-pushdown-computation.md#force-pushdown
<!-- Elastic Query Docs -->
[intro_eq]: /azure/azure-sql/database/elastic-query-overview
[remote_eq]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[remote_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[sharded_eq]: /azure/azure-sql/database/elastic-query-getting-started
[sharded_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started

<!-- Azure Docs -->
[azure_ad]: /azure/data-lake-store/data-lake-store-authenticate-using-active-directory
[sas_token]: /azure/storage/storage-dotnet-shared-access-signature-part-1

::: moniker-end
