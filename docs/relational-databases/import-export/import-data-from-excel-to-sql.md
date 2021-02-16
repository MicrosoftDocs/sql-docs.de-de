---
title: Importieren von Daten aus Excel in SQL | Microsoft-Dokumentation
description: In diesem Artikel werden die Methoden zum Importieren von Daten aus Excel in SQL Server oder Azure SQL-Datenbank beschrieben. Einige Methoden erfordern nur einen einzigen Schritt, für andere ist als Zwischenschritt eine Textdatei erforderlich.
ms.custom: sqlfreshmay19
ms.date: 09/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 84fce410f5ee03301b5ad8e5fe0aa61698b52e0f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351341"
---
# <a name="import-data-from-excel-to-sql-server-or-azure-sql-database"></a>Importieren von Daten aus Excel in SQL Server oder Azure SQL-Datenbank

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Es gibt verschiedene Möglichkeiten, Daten aus Excel in SQL Server oder Azure SQL-Datenbank zu importieren. Bei einigen Methoden können Sie Daten in einem einzigen Schritt direkt aus Excel-Dateien importieren; bei anderen Methoden müssen Sie Ihre Excel-Daten als Text exportieren (CSV-Datei), bevor Sie diese importieren können. Dieser Artikel fasst die häufig verwendeten Methoden zusammen und enthält Links zu weiterführenden Informationen.

## <a name="list-of-methods"></a>Liste der Methoden

Sie können die folgenden Tools verwenden, um Daten aus Excel zu importieren:

| Export zunächst als Text (SQL Server und SQL-Datenbank) | Direkt aus Excel (nur lokaler SQL Server) |
| :------------------------------------------------- |:------------------------------------------------- |
| [Assistent zum Importieren von Flatfiles](#import-wiz)             |[SQL Server-Import/Export-Assistent](#wiz)        |
| [BULK INSERT](#bulk-insert)-Anweisung              |[SQL Server Integration Services (SSIS)](#ssis)    |
| [BCP](#bcp)                                        |[OPENROWSET](#openrowset)-Funktion <br>            |
| [ Kopier-Assistent (Azure Data Factory)](#adf-wiz)       |                                                   |
| [Azure Data Factory](#adf)                         |                                                   |
| &nbsp; | &nbsp; |

Wenn Sie mehrere Arbeitsblätter aus einer Excel-Arbeitsmappe importieren möchten, müssen Sie normalerweise jedes dieser Tools einmal für jedes Arbeitsblatt ausführen.

Eine vollständige Beschreibung der komplexen Tools wie SSIS oder Azure Data Factory würde den Rahmen dieser Liste sprengen. Um mehr über die Lösung zu erfahren, für die Sie sich interessieren, folgen Sie den bereitgestellten Links.

> [!IMPORTANT]
> Ausführliche Informationen über das Herstellen einer Verbindung mit Excel-Dateien sowie Einschränkungen und bekannte Probleme beim Laden von Daten aus oder in Excel-Dateien finden Sie unter [Load data from or to Excel with SQL Server Integration Services (SSIS) (Laden von Daten aus oder in Excel mit SQL Server Integration Services (SSIS))](../../integration-services/load-data-to-from-excel-with-ssis.md).

Wenn Sie SQL Server nicht installiert haben, bzw. SQL Server installiert haben, jedoch nicht SQL Server Management Studio, lesen Sie [Herunterladen von SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="sql-server-import-and-export-wizard"></a><a name="wiz"></a> SQL Server-Import/Export-Assistent

Importen Sie Daten direkt aus Excel-Dateien, indem Sie den Schritten des Assistenten für SQL Server-Import und -Export folgen. Optional können Sie die Einstellungen als SQL Server Integration Services-Paket (SSIS) speichern, das Sie später anpassen und wiederverwenden können.

1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] her.

2. Erweitern Sie **Datenbanken**.
3. Klicken Sie mit der rechten Maustaste auf eine Datenbank.
4. Zeigen Sie auf **Aufgaben**.
5. Klicken Sie auf eine der folgenden Optionen:

  - **Daten importieren**
  - **Daten exportieren**

    ![Starten des Assistenten in SSMS](../../integration-services/import-export-data/media/start-wizard-ssms.jpg)

![Herstellen einer Verbindung mit einer Excel-Datenquelle](media/excel-connection.png)

Ein Beispiel für die Verwendung des Assistenten zum Importieren von Daten aus Excel in SQL Server finden Sie unter [Erste Schritte mit diesem einfachen Beispiel für den Import/Export-Assistenten](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md).

Weitere Informationen zum Starten des Import/Export-Assistenten finden Sie unter [Starten des SQL Server-Import/Export-Assistenten](../../integration-services/import-export-data/start-the-sql-server-import-and-export-wizard.md).

## <a name="sql-server-integration-services-ssis"></a><a name="ssis"></a> SQL Server Integration Services (SSIS)

Wenn Sie mit SSIS vertraut sind und den SQL Server-Import/Export-Assistenten nicht ausführen möchten, erstellen Sie ein SSIS-Paket, das die Excel-Quelle und das SQL Server-Ziel im Datenfluss verwendet.

Weitere Informationen zu dieser SSIS-Komponente finden Sie in den folgenden Themen:

- [Excel-Quelle](../../integration-services/data-flow/excel-source.md)
- [SQL Server-Ziel](../../integration-services/data-flow/sql-server-destination.md)

Erste Informationen zum Erstellen von SSIS-Paketen finden Sie im Tutorial [Erstellen eines ETL-Pakets](../../integration-services/ssis-how-to-create-an-etl-package.md).

![Komponenten im Datenfluss](media/excel-to-sql-data-flow.png)

## <a name="openrowset-and-linked-servers"></a><a name="openrowset"></a> OPENROWSET und Verbindungsserver

> [!IMPORTANT]
> In Azure SQL-Datenbank können Sie nicht direkt aus Excel importieren. Sie müssen die Daten zunächst als Text (CSV-Datei) exportieren. Beispiele finden Sie [hier](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

> [!NOTE]
> Der ACE-Anbieter (früher der Jet-Anbieter), der die Verbindung mit Excel-Datenquellen herstellt, ist für die interaktive Verwendung auf Clientseite vorgesehen. Wenn Sie den ACE-Anbieter auf SQL Server verwenden – insbesondere in automatisierten oder parallel ausgeführten Prozessen –, kann dies zu unerwarteten Ergebnissen führen.

### <a name="distributed-queries"></a>Verteilte Abfragen

Importieren Sie Daten in SQL Server mithilfe der Transact-SQL-Funktion `OPENROWSET` oder `OPENDATASOURCE` direkt aus Excel-Dateien. Diese Nutzung wird als *verteilte Abfrage* bezeichnet.

> [!IMPORTANT]
> In Azure SQL-Datenbank können Sie nicht direkt aus Excel importieren. Sie müssen die Daten zunächst als Testdatei (CSV) exportieren. Beispiele finden Sie [hier](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

Bevor Sie eine verteilte Abfrage ausführen können, müssen Sie die Serverkonfigurationsoption `ad hoc distributed queries` aktivieren, wie in folgendem Beispiel gezeigt. Weitere Informationen finden Sie unter [Ad Hoc Distributed Queries (Serverkonfigurationsoption)](../../database-engine/configure-windows/ad-hoc-distributed-queries-server-configuration-option.md).

```sql
sp_configure 'show advanced options', 1;
RECONFIGURE;
GO
sp_configure 'Ad Hoc Distributed Queries', 1;
RECONFIGURE;
GO
```

Das folgende Codebeispiel verwendet `OPENROWSET`, um die Daten aus dem Excel-Arbeitsblatt `Sheet1` in eine neue Datenbanktabelle zu importieren.

```sql
USE ImportFromExcel;
GO
SELECT * INTO Data_dq
FROM OPENROWSET('Microsoft.ACE.OLEDB.12.0',
    'Excel 12.0; Database=C:\Temp\Data.xlsx', [Sheet1$]);
GO
```

Hier sehen Sie das Beispiel mit `OPENDATASOURCE`.

```sql
USE ImportFromExcel;
GO
SELECT * INTO Data_dq
FROM OPENDATASOURCE('Microsoft.ACE.OLEDB.12.0',
    'Data Source=C:\Temp\Data.xlsx;Extended Properties=Excel 12.0')...[Sheet1$];
GO
```

Um die importierten Daten an eine *vorhandene* Tabelle *anzufügen*, anstatt eine neue Tabelle zu erstellen, verwenden Sie die Syntax `INSERT INTO ... SELECT ... FROM ...` anstelle der Syntax `SELECT ... INTO ... FROM ...` aus den vorherigen Beispielen.

Um die Excel-Daten abzufragen, ohne sie zu importieren, verwenden Sie einfach die Standardsyntax `SELECT ... FROM ...`.

Weitere Informationen zu verteilten Abfragen finden Sie in den folgenden Themen:

- [Verteilte Abfragen](/previous-versions/sql/sql-server-2008-r2/ms188721(v=sql.105)) (Verteilte Abfragen werden in SQL Server 2016 weiterhin unterstützt, aber die Dokumentation für dieses Feature wurde nicht aktualisiert.)
- [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md)
- [OPENDATASOURCE](../../t-sql/functions/openquery-transact-sql.md)

### <a name="linked-servers"></a>Verbindungsserver

Sie können auch eine dauerhafte Verbindung von SQL Server zur Excel-Datei in Form eines *Verbindungsservers* konfigurieren. Das folgende Beispiel importiert die Daten aus dem Arbeitsblatt `Data` auf dem vorhandenen Excel-Verbindungsserver `EXCELLINK` in eine neue SQL Server-Datenbanktabelle mit der Bezeichnung `Data_ls`.

```sql
USE ImportFromExcel;
GO
SELECT * INTO Data_ls FROM EXCELLINK...[Data$];
GO
```

Sie können einen Verbindungsserver über SQL Server Management Studio oder durch Ausführen der gespeicherten Systemprozedur `sp_addlinkedserver` erstellen, wie im folgenden Beispiel gezeigt.

```sql
DECLARE @RC int

DECLARE @server     nvarchar(128)
DECLARE @srvproduct nvarchar(128)
DECLARE @provider   nvarchar(128)
DECLARE @datasrc    nvarchar(4000)
DECLARE @location   nvarchar(4000)
DECLARE @provstr    nvarchar(4000)
DECLARE @catalog    nvarchar(128)

-- Set parameter values
SET @server =     'EXCELLINK'
SET @srvproduct = 'Excel'
SET @provider =   'Microsoft.ACE.OLEDB.12.0'
SET @datasrc =    'C:\Temp\Data.xlsx'
SET @provstr =    'Excel 12.0'

EXEC @RC = [master].[dbo].[sp_addlinkedserver] @server, @srvproduct, @provider,
@datasrc, @location, @provstr, @catalog
```

Weitere Informationen zu Verbindungsservern finden Sie in den folgenden Themen:

- [Erstellen von Verbindungsservern](../../relational-databases/linked-servers/create-linked-servers-sql-server-database-engine.md)
- [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md)

Weitere Beispiele und Informationen zu Verbindungsservern und verteilten Abfragen finden Sie in den folgenden Themen:

- [Verwenden von Excel mit SQL Server-Verbindungsservern und verteilten Abfragen](https://support.microsoft.com/help/306397/how-to-use-excel-with-sql-server-linked-servers-and-distributed-queries)
- [Importieren von Daten aus Excel in SQL Server](https://support.microsoft.com/help/321686/how-to-import-data-from-excel-to-sql-server)

## <a name="prerequisite---save-excel-data-as-text"></a><a name="prereq"></a> Voraussetzung: Speichern von Excel-Daten im Textformat

Für die restlichen auf dieser Seite beschriebenen Methoden – die BULK INSERT-Anweisung, das BCP-Tool und Azure Data Factory – müssen Sie Ihre Excel-Daten zuerst in eine Textdatei exportieren.

Wählen Sie in Excel **Datei > Speichern unter** und dann als Zieldateityp **Text (durch Tabstopps getrennt) (\*.txt)** oder **CSV (durch Trennzeichen getrennt) (\*.csv)** aus.

Wenn Sie mehrere Arbeitsblätter aus der Arbeitsmappe exportieren möchten, wählen Sie jedes Blatt aus, und wiederholen Sie dann dieses Verfahren. Der Befehl **Speichern unter** exportiert nur das aktive Arbeitsblatt.

> [!TIP]
> Um mit Tools für den Datenimport die besten Ergebnisse zu erzielen, speichern Sie Tabellen, die nur die Spaltenüberschriften und die Datenzeilen enthalten. Wenn die gespeicherten Daten Seitentitel, Leerzeilen und Ähnliches enthalten, führt der spätere Import der Daten möglicherweise zu unerwarteten Ergebnissen.

## <a name="the-import-flat-file-wizard"></a><a name="import-wiz"></a> Der Assistent zum Importieren von Flatfiles

Importen Sie in Textdateien gespeicherte Daten, indem Sie den Schritten des Assistenten zum Importieren von Flatfiles folgen.

Wie zuvor im Abschnitt [Voraussetzung](#prereq) beschrieben, müssen Sie Ihre Excel-Daten als Textdatei exportieren, bevor Sie den Assistenten zum Importieren von Flatfiles verwenden können, um sie zu importieren.

Weitere Informationen über den Assistenten zum Importieren von Flatfiles finden Sie unter [Assistent zum Importieren von Flatfiles in SQL](import-flat-file-wizard.md).

## <a name="bulk-insert-command"></a><a name="bulk-insert"></a> BULK INSERT-Befehl

`BULK INSERT` ist ein Transact-SQL-Befehl, den Sie in SQL Server Management Studio ausführen können. Das folgende Beispiel lädt die Daten aus der durch Trennzeichen getrennten Datei `Data.csv` in eine vorhandene Datenbanktabelle.

Wie zuvor im Abschnitt [Voraussetzung](#prereq) beschrieben, müssen Sie Ihre Excel-Daten als Textdatei exportieren, bevor Sie BULK INSERT verwenden können, um sie zu importieren. BULK INSERT kann Excel-Dateien nicht direkt lesen. Mit dem Befehl BULK INSERT können Sie eine CSV-Datei importieren, die lokal oder in Azure Blob Storage gespeichert ist.

```sql
USE ImportFromExcel;
GO
BULK INSERT Data_bi FROM 'C:\Temp\data.csv'
   WITH (
      FIELDTERMINATOR = ',',
      ROWTERMINATOR = '\n'
);
GO
```

Weitere Informationen und Beispiele für SQL Server und SQL-Datenbank finden Sie in den folgenden Themen:

- [Importieren von Massendaten mithilfe von BULK INSERT oder OPENROWSET(BULK...)](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)
- [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md)

## <a name="bcp-tool"></a><a name="bcp"></a> BCP-Tool

BCP ist ein Programm, das Sie über die Eingabeaufforderung ausführen. Das folgende Beispiel lädt die Daten aus der durch Trennzeichen getrennten Datei `Data.csv` in die vorhandene Datenbanktabelle `Data_bcp`.

Wie zuvor im Abschnitt [Voraussetzung](#prereq) beschrieben, müssen Sie Ihre Excel-Daten als Textdatei exportieren, bevor Sie BCP verwenden können, um sie zu importieren. BCP kann Excel-Dateien nicht direkt lesen. Verwenden Sie es zum Importieren in SQL Server oder SQL-Datenbank aus einer Testdatei (CSV), die lokal gespeichert ist.

> [!IMPORTANT]
> Verwenden Sie BULK INSERT oder OPENROWSET für eine Textdatei (CSV), die in Azure Blob Storage gespeichert ist. Beispiele finden Sie [hier](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

```console
bcp.exe ImportFromExcel..Data_bcp in "C:\Temp\data.csv" -T -c -t ,
```

Weitere Informationen zu BCP finden Sie in folgenden Themen:

- [Importieren und Exportieren von Massendaten mithilfe des Hilfsprogramms bcp](../../relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server.md)
- [bcp (Hilfsprogramm)](../../tools/bcp-utility.md)
- [Vorbereiten von Daten für den Massenexport oder -import](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)

## <a name="copy-wizard-azure-data-factory"></a><a name="adf-wiz"></a> Kopier-Assistent (Azure Data Factory)

Importen Sie in Textdateien gespeicherte Daten, indem Sie den Schritten des Kopier-Assistenten von Azure Data Factory folgen.

Wie zuvor im Abschnitt [Voraussetzung](#prereq) beschrieben, müssen Sie Ihre Excel-Daten als Textdatei exportieren, bevor Sie Azure Data Factory verwenden können, um sie zu importieren. Azure Data Factory kann Excel-Dateien nicht direkt lesen.

Weitere Informationen zum Kopier-Assistenten finden Sie in den folgenden Themen:

- [Assistent zum Kopieren in Data Factory](/azure/data-factory/data-factory-azure-copy-wizard)
- [Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe des Data Factory-Kopier-Assistenten](/azure/data-factory/data-factory-copy-data-wizard-tutorial)

## <a name="azure-data-factory"></a><a name="adf"></a> Azure Data Factory

Wenn Sie mit Azure Data Factory vertraut sind und den Kopier-Assistenten nicht ausführen möchten, erstellen Sie eine Pipeline mit einer Kopieraktivität, die Daten aus der Textdatei nach SQL Server oder Azure SQL-Datenbank kopiert.

Wie zuvor im Abschnitt [Voraussetzung](#prereq) beschrieben, müssen Sie Ihre Excel-Daten als Textdatei exportieren, bevor Sie Azure Data Factory verwenden können, um sie zu importieren. Azure Data Factory kann Excel-Dateien nicht direkt lesen.

Weitere Informationen zu diesen Data Factory-Quellen und -Senken finden Sie in folgenden Themen:

- [Dateisystem](/azure/data-factory/data-factory-onprem-file-system-connector)
- [SQL Server](/azure/data-factory/data-factory-sqlserver-connector)
- [Azure SQL-Datenbank](/azure/data-factory/data-factory-azure-sql-connector)

Informationen zum Kopieren von Daten mit Azure Data Factory finden Sie in folgenden Themen:

- [Verschieben von Daten mit der Kopieraktivität](/azure/data-factory/data-factory-data-movement-activities)
- [Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe des Azure-Portals](/azure/data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database)

## <a name="common-errors"></a>Häufige Fehler

### <a name="microsoftaceoledb120-has-not-been-registered"></a>Microsoft.ACE.OLEDB.12.0 wurde nicht registriert

Dieser Fehler tritt auf, weil der OLE DB-Anbieter nicht installiert ist. Installieren Sie ihn aus [Microsoft Access Database Engine 2010 Redistributable](https://www.microsoft.com/download/details.aspx?id=13255). Achten Sie darauf, die 64-Bit-Version zu installieren, wenn Windows und SQL Server beides 64-Bit-Versionen sind.

Der vollständige Fehler besagt:

```
Msg 7403, Level 16, State 1, Line 3
The OLE DB provider "Microsoft.ACE.OLEDB.12.0" has not been registered.
```

### <a name="cannot-create-an-instance-of-ole-db-provider-microsoftaceoledb120-for-linked-server-null"></a>Eine Instanz des OLE DB-Anbieters „Microsoft.ACE.OLEDB.12.0“ für den Verbindungsserver „(null)“ kann nicht erstellt werden.

Dieser Fehler gibt an, dass Microsoft OLEDB nicht ordnungsgemäß konfiguriert wurde. Führen Sie den folgenden Transact-SQL-Code aus, um das Problem zu beheben:

```sql
EXEC sp_MSset_oledb_prop N'Microsoft.ACE.OLEDB.12.0', N'AllowInProcess', 1
EXEC sp_MSset_oledb_prop N'Microsoft.ACE.OLEDB.12.0', N'DynamicParameters', 1
```

Der vollständige Fehler besagt:

```
Msg 7302, Level 16, State 1, Line 3
Cannot create an instance of OLE DB provider "Microsoft.ACE.OLEDB.12.0" for linked server "(null)".
```

### <a name="the-32-bit-ole-db-provider-microsoftaceoledb120-cannot-be-loaded-in-process-on-a-64-bit-sql-server"></a>Das In-Process-Laden des 32-Bit-OLE DB-Anbieters „Microsoft.ACE.OLEDB.12.0“ auf einem 64-Bit-SQL Server ist nicht möglich.

Dieser Fehler tritt auf, wenn eine 32-Bit-Version des OLE DB-Anbieters mit einem 64-Bit-SQL Server installiert ist. Um dieses Problem zu beheben, deinstallieren Sie die 32-Bit-Version und installieren stattdessen die 64-Bit-Version des OLE DB-Anbieters.

Der vollständige Fehler besagt:

```
Msg 7438, Level 16, State 1, Line 3
The 32-bit OLE DB provider "Microsoft.ACE.OLEDB.12.0" cannot be loaded in-process on a 64-bit SQL Server.
```

### <a name="the-ole-db-provider-microsoftaceoledb120-for-linked-server-null-reported-an-error-the-provider-did-not-give-any-information-about-the-error"></a>Der OLE DB-Anbieter "Microsoft.ACE.OLEDB.12.0" für den Verbindungsserver "(null)" hat einen Fehler gemeldet. Der Anbieter hat keine Informationen zu dem Fehler bereitgestellt.

### <a name="cannot-initialize-the-data-source-object-of-ole-db-provider-microsoftaceoledb120-for-linked-server-null"></a>Das Datenquellenobjekt des OLE DB-Anbieters „Microsoft.ACE.OLEDB.12.0“ für den Verbindungsserver „(null)“ kann nicht initialisiert werden.

Diese beiden Fehler geben normalerweise ein Berechtigungsproblem zwischen dem SQL Server-Prozess und der Datei an. Stellen Sie sicher, dass das Konto, das den SQL Server-Dienst ausführt, Vollzugriff auf die Datei besitzt. Es wird nicht empfohlen, Dateien vom Desktop zu importieren.

Die vollständigen Fehler besagen:

```
Msg 7399, Level 16, State 1, Line 3
The OLE DB provider "Microsoft.ACE.OLEDB.12.0" for linked server "(null)" reported an error. The provider did not give any information about the error.
```

```
Msg 7303, Level 16, State 1, Line 3
Cannot initialize the data source object of OLE DB provider "Microsoft.ACE.OLEDB.12.0" for linked server "(null)".
```

## <a name="see-also"></a>Weitere Informationen

[Laden von Daten aus oder in Excel mit SQL Server Integration Services (SSIS)](../../integration-services/load-data-to-from-excel-with-ssis.md)