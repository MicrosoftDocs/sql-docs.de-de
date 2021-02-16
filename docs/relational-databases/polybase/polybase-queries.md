---
title: PolyBase-Abfrageszenarien | Microsoft-Dokumentation
description: In diesem Artikel finden Sie Beispiele für Abfragen, in denen das PolyBase-Feature von SQL Server verwendet wird. In diesen Abfragen werden z. B. SELECT-Anweisungen, JOIN-Klauseln zum Verknüpfen von externen Tabellen mit lokalen, Import-/Exportvorgänge von Daten und neue Katalogsichten verwendet.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
keywords:
- PolyBase
helpviewer_keywords:
- PolyBase, import and export
- Hadoop, import with PolyBase
- Hadoop, export with PolyBase
- Azure blob storage, import with PolyBase
- Azure blob storage, export with PolyBase
ms.assetid: 2c5aa2bd-af7d-4f57-9a28-9673c2a4c07e
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: cdcd5d06395d8216fdf3e9c61b8434ab226c9837
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351834"
---
# <a name="polybase-query-scenarios"></a>PolyBase-Abfrageszenarien

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Dieser Artikel bietet Beispiele für Abfragen mit dem [PolyBase](../../relational-databases/polybase/polybase-guide.md)-Feature von SQL Server (ab 2016). Bevor Sie diese Beispiele verwenden können, müssen Sie PolyBase installieren und konfigurieren. Weitere Informationen finden Sie im [PolyBase-Leitfaden](polybase-guide.md).
  
Führen Sie Transact-SQL-Anweisungen für externe Tabellen aus, oder verwenden Sie BI-Tools, um externe Tabellen abzufragen.
  
## <a name="select-from-external-table"></a>Auswählen aus einer externen Tabelle mit der SELECT-Anweisung  

Eine einfache Abfrage, die Daten aus einer definierten externen Tabelle zurückgibt.  

```sql  
SELECT TOP 10 * FROM [dbo].[SensorData];   
```

Eine einfache Abfrage, die ein Prädikat enthält.

```sql
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65;
```

## <a name="join-external-tables-with-local-tables"></a>Externe Tabellen mit dem Befehl JOIN mit lokalen Tabellen verknüpfen

```sql
SELECT InsuranceCustomers.FirstName,   
   InsuranceCustomers.LastName,   
   SensorData.Speed  
FROM InsuranceCustomers INNER JOIN SensorData    
ON InsuranceCustomers.CustomerKey = SensorData.CustomerKey   
WHERE SensorData.Speed > 65   
ORDER BY SensorData.Speed DESC  
  
```  

## <a name="import-data"></a>Daten importieren

Importieren Sie Daten aus Hadoop oder Azure Storage in SQL Server für den beständigen Speicher. Verwenden Sie SELECT INTO, um Daten, auf die eine externe Tabelle verweist, zu importieren und dauerhaft in SQL Server zu speichern. Erstellen Sie dynamisch eine relationale Tabelle, und erstellen Sie dann in einem zweiten Schritt einen Columnstore-Index am oberen Rand der Tabelle.

```sql
-- PolyBase scenario - import external data into SQL Server
-- Import data for fast drivers into SQL Server to do more in-depth analysis
-- Leverage columnstore technology
  
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

## <a name="export-data"></a>Exportieren von Daten

Exportieren von Daten aus SQL Server in Hadoop oder Azure Storage 

Aktivieren Sie zunächst die Exportfunktion, indem Sie den Wert `sp_configure` von „allow polybase export“ auf 1 festlegen. Erstellen Sie anschließend eine externe Tabelle, die auf das Zielverzeichnis verweist. Die Anweisung CREATE EXTERNAL TABLE erstellt das Zielverzeichnis, wenn dieses noch nicht vorhanden ist. Verwenden Sie dann INSERT INTO zum Exportieren von Daten aus einer lokalen SQL Server-Tabelle in die externe Datenquelle. 

Die Ergebnisse der SELECT-Anweisung werden an die angegebene Position im angegebenen Dateiformat exportiert. Die externen Dateien heißen *QueryID_date_time_ID.format*, wobei *ID* ein inkrementeller Bezeichner ist und *Format* das Format der exportierten Daten. Beispielsweise kann ein Dateiname „QID776_20160130_182739_0.orc“ lauten.

> [!NOTE]
> Beim Exportieren von Daten nach Hadoop oder Azure Blob Storage über PolyBase werden nur die Daten exportiert, nicht die Spaltennamen (Metadaten), wie im CREATE EXTERNAL TABLE-Befehl definiert.

```sql  
-- PolyBase scenario  - export data from SQL Server to Hadoop
-- Create an external table
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
INSERT INTO dbo.FastCustomers2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```

## <a name="new-catalog-views"></a>Neue Katalogsichten

Die folgenden neuen Katalogsichten zeigen externe Ressourcen an.

```sql
SELECT * FROM sys.external_data_sources;   
SELECT * FROM sys.external_file_formats;  
SELECT * FROM sys.external_tables;  
```

 Bestimmen Sie unter Verwendung von, ob eine Tabelle eine externe Tabelle ist. `is_external`  

```sql  
SELECT name, type, is_external FROM sys.tables WHERE name='myTableName'   
```  

## <a name="next-steps"></a>Nächste Schritte  

Weitere Informationen zur Problembehandlung finden Sie unter [PolyBase troubleshooting](../../relational-databases/polybase/polybase-troubleshooting.md)(PolyBase-Problembehandlung).
