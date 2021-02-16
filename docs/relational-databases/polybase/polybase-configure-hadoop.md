---
title: 'Zugreifen auf externe Daten: Hadoop – PolyBase'
description: In diesem Artikel wird PolyBase im Rahmen einer SQL Server-Instanz mit Hadoop verwendet. PolyBase eignet sich für Ad-hoc-Abfragen externer Tabellen sowie das Importieren und Exportieren von Daten.
ms.date: 12/13/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.custom: seo-dt-2019
ms.openlocfilehash: 1124d05dc57697d9e27757dad2b4fee842a22a5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351804"
---
# <a name="configure-polybase-to-access-external-data-in-hadoop"></a>Konfigurieren von PolyBase für den Zugriff auf externe Daten in Hadoop

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

In diesem Artikel wird erläutert, wie Sie PolyBase in einer SQL Server-Instanz verwenden, um externe Daten in Hadoop abzufragen.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie PolyBase nicht installiert haben, finden Sie weitere Informationen unter [PolyBase installation (Installieren von PolyBase)](polybase-installation.md). Die Voraussetzungen für die Installation werden im entsprechenden Artikel erläutert.

<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

- Ab SQL Server 2019 müssen Sie auch [das PolyBase-Feature aktivieren](polybase-installation.md#enable).

::: moniker-end

- PolyBase unterstützt zwei Hadoop-Anbieter: Hortonworks Data Platform (HDP) und Cloudera Distributed Hadoop (CDH). Hadoop folgt dem Muster „Hauptversion.Nebenversion“ für neue Releases, und alle Versionen, die zu einer unterstützten Haupt- und Nebenversion gehören, werden unterstützt. Folgende Hadoop-Anbieter werden unterstützt:

  - Hortonworks HDP 1.3, 2.1 bis 2.6 und 3.0 unter Linux
  - Hortonworks HDP 1.3 und 2.1 bis 2.3 unter Windows Server
  - Cloudera CDH 4.3, 5.1 bis 5.5 und 5.9 bis 5.13 unter Linux

> [!NOTE]
> PolyBase unterstützt Hadoop-Verschlüsselungszonen ab SQL Server 2016 SP1 CU7 und SQL Server 2017 CU3. Wenn Sie [PolyBase-Erweiterungsgruppen](polybase-scale-out-groups.md) verwenden, müssen alle Computeknoten auch einen Build aufweisen, der Unterstützung für Hadoop-Verschlüsselungszonen enthält.

### <a name="configure-hadoop-connectivity"></a>Konfigurieren der Hadoop-Konnektivität

Zunächst konfigurieren Sie SQL Server PolyBase für die Verwendung Ihres Hadoop-Anbieters.

1. Führen Sie [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) mit „hadoop connectivity“ aus, und legen Sie einen geeigneten Wert für Ihren Anbieter fest. Informationen zum Ermitteln des Werts für Ihren Anbieter finden Sie unter [Konfiguration der PolyBase-Konnektivität](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md). Standardmäßig ist die Hadoop-Konnektivität auf 7 festgelegt.

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure blob storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. Sie müssen SQL Server mithilfe von **services.msc** neu starten. Durch den Neustart von SQL Server werden folgende Dienste neu gestartet:  

   - SQL Server PolyBase-Datenverschiebungsdienst  
   - SQL Server PolyBase-Engine  
  
   ![Beenden und Starten von PolyBase-Diensten in „Services.msc“](../../relational-databases/polybase/media/polybase-stop-start.png "Beenden und Starten von PolyBase-Diensten in „Services.msc“")  
  
## <a name="enable-pushdown-computation"></a><a id="pushdown"></a> Aktivieren der Weitergabeberechnung  

Um die Abfrageleistung zu verbessern, aktivieren Sie die Weitergabeberechnung für Ihren Hadoop-Cluster:  
  
1. Suchen Sie die Datei **yarn-site.xml** im Installationspfad von SQL Server. In der Regel lautet der Pfad:  

   ```xml  
   C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Polybase\Hadoop\conf\  
   ```  

1. Suchen Sie auf dem Hadoop-Computer die analoge Datei im Hadoop-Konfigurationsverzeichnis. Suchen Sie in der Datei den Wert des Konfigurationsschlüssels „yarn.application.classpath.“, und kopieren Sie diesen.  
  
1. Suchen Sie auf dem SQL Server-Computer in der Datei **yarn-site.xml** die Eigenschaft **yarn.application.classpath**. Fügen Sie den Wert vom Hadoop-Computer in das Element „Value“ ein.  
  
1. Für alle CDH 5.X-Versionen müssen Sie die Konfigurationsparameter „mapreduce.application.classpath“ entweder ans Ende der Datei „yarn-site.xml“ oder in die Datei „mapred-site.xml“ einfügen. HortonWorks enthält diese Konfigurationen in den yarn.application.classpath-Konfigurationen. Weitere Beispiele finden Sie unter [PolyBase-Konfiguration](../../relational-databases/polybase/polybase-configuration.md).

## <a name="configure-an-external-table"></a>Konfigurieren einer externen Tabelle

Um die Daten in Ihrer Hadoop-Datenquelle abzufragen, müssen Sie eine externe Tabelle definieren, die in Transact-SQL-Abfragen verwendet werden soll. Die folgenden Schritte beschreiben, wie Sie die externe Tabelle konfigurieren.

1. Erstellen Sie einen Hauptschlüssel für die Datenbank, falls noch keiner vorhanden ist. Dies ist erforderlich, um das Geheimnis für die Anmeldeinformationen zu verschlüsseln.

     ```sql
      CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';  
     ```
    ## <a name="arguments"></a>Argumente
    PASSWORD ='password'

    Das zum Verschlüsseln des Datenbank-Hauptschlüssels verwendete Kennwort. „password“ (Kennwort) muss den Anforderungen der Windows-Kennwortrichtlinien des Computers entsprechen, auf dem die Instanz von SQL Server ausgeführt wird.
1. Erstellen Sie datenbankweite Anmeldeinformationen für Hadoop-Cluster, die mit Kerberos gesichert sind.

   ```sql
   -- IDENTITY: the Kerberos user name.  
   -- SECRET: the Kerberos password  
   CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
   WITH IDENTITY = '<hadoop_user_name>', Secret = '<hadoop_password>';  
   ```

2. Erstellen Sie mit [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) eine externe Datenquelle.

   ```sql
   -- LOCATION (Required) : Hadoop Name Node IP address and port.  
   -- RESOURCE MANAGER LOCATION (Optional): Hadoop Resource Manager location to enable pushdown computation.  
   -- CREDENTIAL (Optional):  the database scoped credential, created above.  
   CREATE EXTERNAL DATA SOURCE MyHadoopCluster WITH (  
         TYPE = HADOOP,
         LOCATION ='hdfs://10.xxx.xx.xxx:xxxx',
         RESOURCE_MANAGER_LOCATION = '10.xxx.xx.xxx:xxxx',
         CREDENTIAL = HadoopUser1
   );  
   ```

3. Erstellen Sie mit [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md) ein externes Dateiformat.

   ```sql
   -- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE))
   ```

4. Erstellen Sie mit [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) eine externe Tabelle, die auf in Hadoop gespeicherte Daten verweist. In diesem Beispiel handelt es sich bei den externen Daten um Kfz-Sensordaten.

   ```sql
   -- LOCATION: path to file or directory that contains the data (relative to HDFS root).  
   CREATE EXTERNAL TABLE [dbo].[CarSensor_Data] (  
         [SensorKey] int NOT NULL,
         [CustomerKey] int NOT NULL,
         [GeographyKey] int NULL,
         [Speed] float NOT NULL,
         [YearMeasured] int NOT NULL  
   )  
   WITH (LOCATION='/Demo/',
         DATA_SOURCE = MyHadoopCluster,  
         FILE_FORMAT = TextFileFormat  
   );  
   ```

5. Erstellen Sie Statistiken für eine externe Tabelle.

   ```sql
   CREATE STATISTICS StatsForSensors on CarSensor_Data(CustomerKey, Speed)  
   ```

## <a name="polybase-queries"></a>PolyBase-Abfragen

Es gibt drei Funktionen, für die PolyBase geeignet ist:  
  
- Ad-hoc-Abfragen von externen Tabellen  
- Importieren von Daten  
- Exportieren von Daten  

Die folgenden Abfragen stellen fiktive Kfz-Sensordaten für das Beispiel bereit.

### <a name="ad-hoc-queries"></a>Ad-hoc-Abfragen  

Die folgende Ad-hoc-Abfrage verknüpft relationale Daten mit Hadoop-Daten. Sie wählt Kunden aus, die schneller als 35 Meilen pro Stunde fahren, und verknüpft in SQL Server gespeicherte strukturierte Daten mit in Hadoop gespeicherten Daten aus Kfz-Sensoren.  

```sql  
SELECT DISTINCT Insured_Customers.FirstName,Insured_Customers.LastName,
       Insured_Customers. YearlyIncome, CarSensor_Data.Speed  
FROM Insured_Customers, CarSensor_Data  
WHERE Insured_Customers.CustomerKey = CarSensor_Data.CustomerKey and CarSensor_Data.Speed > 35
ORDER BY CarSensor_Data.Speed DESC  
OPTION (FORCE EXTERNALPUSHDOWN);   -- or OPTION (DISABLE EXTERNALPUSHDOWN)  
```  

### <a name="importing-data"></a>Importieren von Daten  

Die folgende Abfrage importiert externe Daten in SQL Server. Dieses Beispiel importiert Daten zu schnellen Fahrern in SQL Server zur genaueren Analyse. Zur Verbesserung der Leistung wird die Columnstore-Technologie verwendet.  

```sql
SELECT DISTINCT
      Insured_Customers.FirstName, Insured_Customers.LastName,   
      Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
INTO Fast_Customers from Insured_Customers INNER JOIN   
(  
      SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
ORDER BY YearlyIncome  
  
CREATE CLUSTERED COLUMNSTORE INDEX CCI_FastCustomers ON Fast_Customers;  
```  

### <a name="exporting-data"></a>Exportieren von Daten  

Die folgende Abfrage exportiert Daten aus SQL Server nach Hadoop. Zu diesem Zweck müssen Sie zuerst den PolyBase-Export aktivieren. Erstellen Sie dann eine externe Tabelle für das Ziel, bevor Sie mit dem Datenexport beginnen.

```sql
-- Enable INSERT into external table  
sp_configure 'allow polybase export', 1;  
reconfigure  
  
-- Create an external table.
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] (  
      [FirstName] char(25) NOT NULL,
      [LastName] char(25) NOT NULL,
      [YearlyIncome] float NULL,
      [MaritalStatus] char(1) NOT NULL  
)  
WITH (  
      LOCATION='/old_data/2009/customerdata',  
      DATA_SOURCE = HadoopHDP2,  
      FILE_FORMAT = TextFileFormat,  
      REJECT_TYPE = VALUE,  
      REJECT_VALUE = 0  
);  

-- Export data: Move old data to Hadoop while keeping it query-able via an external table.  
INSERT INTO dbo.FastCustomer2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  

## <a name="view-polybase-objects-in-ssms"></a>Anzeigen von PolyBase-Objekten in SSMS  

In SSMS werden externe Tabellen in einem separaten Ordner **Externe Tabellen** angezeigt. Externe Datenquellen und externe Dateiformate befinden sich in Unterordnern unter **Externe Ressourcen**.  
  
![PolyBase-Objekte in SSMS](media/polybase-management.png)  

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie weitere Möglichkeiten zur Verwendung und Überwachung von PolyBase:

[PolyBase-Erweiterungsgruppen](../../relational-databases/polybase/polybase-scale-out-groups.md)  
[Problembehandlung in PolyBase](polybase-troubleshooting.md)  
