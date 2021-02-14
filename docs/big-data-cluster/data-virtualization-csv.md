---
title: Virtualisieren von CSV-Daten aus einem Speicherpool
subtitle: Big Data-Cluster für SQL Server
description: Hier finden Sie die Schritte, die für das Erstellen einer externen Tabelle für die Virtualisierung einer CSV-Datei in einem Big Data-Cluster erforderlich sind.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 04/24/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: polybase
monikerRange: '>= sql-server-ver15'
ms.metadata: seo-lt-2019
ms.openlocfilehash: 7b78e44ee36e9320688a8a6815fbb267f1663d3d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038670"
---
# <a name="virtualize-csv-data-from-storage-pool-big-data-clusters"></a>Virtualisieren von CSV-Daten aus einem Speicherpool (Big Data-Cluster)

Big Data-Cluster in SQL Server können Daten aus CSV-Dateien in HDFS virtualisieren. Dieser Prozess ermöglicht es, dass die Daten am ursprünglichen Speicherort verbleiben und dennoch wie jede andere Tabelle von einer SQL Server-Instanz abgefragt werden können. Für dieses Feature werden PolyBase-Connectors verwendet. Der Bedarf für ETL-Prozesse wird dabei minimiert. Weitere Informationen zur Datenvirtualisierung finden Sie unter [Was ist PolyBase?](../relational-databases/polybase/polybase-guide.md).

## <a name="prerequisites"></a>Voraussetzungen

- [Ein bereitgestellter Big Data-Cluster](deployment-guidance.md)
- [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)

## <a name="select-or-upload-a-csv-file-for-data-virtualization"></a>Auswählen oder Hochladen einer CSV-Datei zur Datenvirtualisierung 

Stellen Sie in Azure Data Studio (ADS) [eine Verbindung mit der SQL Server-Masterinstanz](connect-to-big-data-cluster.md#master) Ihres Big Data-Clusters her. Sobald die Verbindung hergestellt wurde, erweitern Sie die HDFS-Elemente im Objekt-Explorer, um nach den CSV-Dateien zu suchen, deren Daten Sie virtualisieren möchten. 

Erstellen Sie für dieses Tutorial ein Verzeichnis namens **Daten**.

1. Klicken Sie mit der rechten Maustaste auf das Kontextmenü des HDFS-Stammverzeichnisses.
2. Klicken Sie auf **Neues Verzeichnis**.
3. Benennen Sie das neue Verzeichnis mit *Daten*.

Laden Sie Beispieldaten hoch. Für eine einfache exemplarische Vorgehensweise können Sie eine Beispieldatei mit CSV-Daten verwenden. Für diesen Artikel werden Daten über die Gründe von Flugverspätungen des [Verkehrsministeriums der Vereinigten Staaten](https://www.transtats.bts.gov/OT_Delay/OT_DelayCause1.asp?pn=1) verwendet. Laden Sie die Rohdaten herunter, und extrahieren Sie die Daten auf Ihrem Computer. Benennen Sie die Datei mit *airline_delay_causes.csv*.

So laden Sie die Beispieldatei nach dem Extrahieren hoch:

1. *Klicken Sie in Azure Data Studio mit der rechten Maustaste* auf das Verzeichnis, das Sie neu erstellt haben. 
2. Klicken Sie auf **Dateien hochladen**.

![Beispieldatei mit CSV-Daten in HDFS](media/data-virtualization/100-csv-sample-file-hdfs.png)

Azure Data Studio lädt die Dateien in HDFS in den Big Data-Cluster hoch.

## <a name="create-the-storage-pool-external-data-source-in-your-target-database"></a>Erstellen der externen Speicherpooldatenquelle in Ihrer Zieldatenbank

Die externe Speicherpooldatenquelle wird in einer Datenbank nicht standardmäßig in Ihrem Big Data-Cluster erstellt. Bevor Sie die externe Tabelle erstellen können, müssen Sie die externe Standarddatenquelle **SqlStoragePool** in Ihrer Zieldatenbank mit der folgenden Transact-SQL-Abfrage erstellen. Stellen Sie sicher, dass Sie zunächst den Kontext der Abfrage gemäß Ihrer Zieldatenbank ändern.

```sql
-- Create the default storage pool source for SQL Big Data Cluster
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
    CREATE EXTERNAL DATA SOURCE SqlStoragePool
    WITH (LOCATION = 'sqlhdfs://controller-svc/default');
```

## <a name="create-the-external-table"></a>Erstellen der externen Tabelle

Klicken Sie in ADS mit der rechten Maustaste auf die CSV-Datei, und wählen Sie im Kontextmenü **Create External Table From CSV File** (Externe Tabelle aus CSV-Datei erstellen) aus. Sie können externe Tabellen auch aus CSV-Dateien aus einem Verzeichnis in HDFS erstellen, wenn die Daten im Verzeichnis dem gleichen Schema folgen. Dies würde die Virtualisierung der Daten auf einer Verzeichnisebene ermöglichen, ohne dass einzelne Dateien verarbeitet werden müssten und ein zusammengeführtes Resultset für die kombinierten Daten abgerufen werden müsste. Azure Data Studio führt Sie durch die Schritte, die für das Erstellen der externen Tabelle erforderlich sind.

Geben Sie die Datenbank, die Datenquelle, einen Tabellennamen, das Schema und den Namen für das externe Dateiformat der Tabelle an.

Klicken Sie auf **Weiter**.

## <a name="preview-data"></a>Datenvorschau

Azure Data Studio zeigt eine Vorschau der importierten Daten.

![Screenshot des Fensters „Externe Tabelle aus CSV erstellen“ mit einer Vorschau der importierten Daten](media/data-virtualization/130-csv-preview-data.png)

Klicken Sie auf **Weiter**, um fortzufahren, sobald Sie die Vorschau überprüft haben.

## <a name="modify-columns"></a>Bearbeiten von Spalten

Im daraufhin angezeigten Fenster können Sie die Spalten der externen Tabelle, die erstellt werden soll, bearbeiten. Sie können Spaltennamen ändern, den Datentyp ändern und Zeilen mit NULL-Werten zulassen. 

![Screenshot des Fensters „Externe Tabelle aus CSV erstellen“ mit Schritt 3 „Spalten ändern“](media/data-virtualization/140-csv-modify-columns.png)

Klicken Sie auf **Weiter**, wenn Sie die Zielspalten überprüft haben.

## <a name="summary"></a>Zusammenfassung

Dieser Schritt bietet eine Zusammenfassung Ihrer Auswahl. Der SQL Server-Name, der Name der Datenbank, der Name der Tabelle, das Tabellenschema und weitere Informationen zur externen Tabelle werden angezeigt. Bei diesem Schritt haben Sie die Möglichkeit, ein Skript zu generieren oder eine Tabelle zu erstellen. **Skript generieren** erstellt ein Skript in T-SQL, um die externe Datenquelle zu erstellen. **Tabelle erstellen** erstellt die externe Datenquelle.

![Bildschirm „Zusammenfassung“](media/data-virtualization/150-csv-virtualize-data-summary.png)

Wenn Sie auf **Tabelle erstellen** klicken, erstellt SQL Server die externe Tabelle in der Zieldatenbank.

Wenn Sie auf **Skript generieren** klicken, erstellt Ihre Azure Data Studio-Instanz die T-SQL-Abfrage für das Erstellen der externen Tabelle.

Sobald die Tabelle erstellt wurde, kann sie direkt mithilfe von T-SQL über die SQL Server-Instanz abgefragt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum SQL Server-Cluster für Big Data und die entsprechenden Szenarien finden Sie unter [Was ist der SQL Server-Cluster für Big Data?](big-data-cluster-overview.md).
