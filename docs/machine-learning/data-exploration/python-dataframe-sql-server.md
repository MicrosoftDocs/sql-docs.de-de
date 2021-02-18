---
title: Einfügen eines Python-Datenrahmens in eine SQL-Tabelle
titleSuffix: SQL machine learning
description: So fügen Sie Daten aus einem Datenrahmen in eine SQL-Tabelle ein.
author: dphansen
ms.author: davidph
ms.date: 07/23/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 8d27ae33ab283209725c119ed6d96c4ed511b4ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349110"
---
# <a name="insert-python-dataframe-into-sql-table"></a>Einfügen eines Python-Datenrahmens in eine SQL-Tabelle
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

In diesem Artikel wird beschrieben, wie ein [Pandas](https://pandas.pydata.org/)-Datenrahmen mithilfe des [pyodbc](../../connect/python/pyodbc/python-sql-driver-pyodbc.md)-Pakets in Python in eine SQL-Datenbank-Instanz eingefügt wird.

## <a name="prerequisites"></a>Voraussetzungen

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
* [SQL Server für Windows](../../database-engine/install-windows/install-sql-server.md) oder [für Linux](../../linux/sql-server-linux-overview.md)
::: moniker-end

::: moniker range="=azuresqldb-current"
* [Azure SQL-Datenbank](/azure/sql-database/sql-database-get-started-portal)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
* [Verwaltete Azure SQL-Datenbank-Instanz](/azure/azure-sql/managed-instance/instance-create-quickstart)

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) zum Wiederherstellen der Beispieldatenbank in Azure SQL Managed Instance
::: moniker-end

* Azure Data Studio. Informationen zur Installation finden Sie unter [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

* [Wiederherstellen einer Beispieldatenbank](../../samples/adventureworks-install-configure.md), um die in diesem Artikel verwendeten Beispieldaten zu erhalten.

## <a name="verify-restored-database"></a>Überprüfen der wiederhergestellten Datenbank

Sie können überprüfen, ob die wiederhergestellte Datenbank vorhanden ist, indem Sie die **HumanResources.Department**-Tabelle abfragen:

```sql
USE AdventureWorks;
SELECT * FROM HumanResources.Department;
```

## <a name="install-python-packages"></a>Installieren von Python-Paketen

* [Herunterladen und Installieren von Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md)

Installieren Sie die folgenden Python-Pakete:
  * pyodbc
  * pandas

  Installieren Sie diese Pakete wie folgt:

  1. Klicken Sie in Ihrem Azure Data Studio-Notebook auf die Option **Pakete verwalten**.
  2. Klicken Sie dann im Bereich **Manage Packages** (Pakete verwalten) auf die Registerkarte **Add new** (Neue hinzufügen).
  3. Geben Sie für jedes der folgenden Pakete den jeweiligen Paketnamen ein, klicken Sie auf **Suchen** und dann auf **Installieren**.

## <a name="connect-to-sql-server-using-azure-data-studio"></a>Stellen Sie eine Verbindung zu SQL Server mit Azure Data Studio her.

[Verbindungsherstellung mithilfe von Azure Data Studio](../../azure-data-studio/quickstart-sql-server.md)

1. Stellen Sie eine Verbindung mit der Adventureworks-Datenbank her, um die neue Tabelle „HumanResources.DepartmentTest“ zu erstellen. Die SQL-Tabelle wird für den Einfügevorgang für den Datenrahmen verwendet.

```sql
CREATE TABLE [HumanResources].[DepartmentTest](
[DepartmentID] [smallint] NOT NULL,
[Name] [dbo].[Name] NOT NULL,
[GroupName] [dbo].[Name] NOT NULL
)
GO
```

## <a name="create-csv-file"></a>Erstellen einer CSV-Datei

Kopieren Sie den Text, und speichern Sie die Datei als „department.csv“ für den Datenrahmen.

```text
DepartmentID,Name,GroupName,
1,Engineering,Research and Development,
2,Tool Design,Research and Development,
3,Sales,Sales and Marketing,
4,Marketing,Sales and Marketing,
5,Purchasing,Inventory Management,
6,Research and Development,Research and Development,
7,Production,Manufacturing,
8,Production Control,Manufacturing,
9,Human Resources,Executive General and Administration,
10,Finance,Executive General and Administration,
11,Information Services,Executive General and Administration,
12,Document Control,Quality Assurance,
13,Quality Assurance,Quality Assurance,
14,Facilities and Maintenance,Executive General and Administration,
15,Shipping and Receiving,Inventory Management,
16,Executive,Executive General and Administration
```

## <a name="connect-to-sql-using-python"></a>Herstellen einer Verbindung mit SQL mithilfe von Python

1. Bearbeiten Sie die Variablen der Verbindungszeichenfolge „server“, „database“, „username'“ und „password“, um eine Verbindung mit SQL-Datenbank herzustellen.

2. Bearbeiten Sie den Pfad für die CSV-Datei.

## <a name="load-dataframe-from-csv-file"></a>Laden des Datenrahmens aus der CSV-Datei

Verwenden Sie das Paket `pandas` von Python, um einen Datenrahmen zu erstellen und die CSV-Datei zu laden. Stellen Sie eine Verbindung mit SQL her, um den Datenrahmen in die neue SQL-Tabelle „HumanResources.DepartmentTest“ zu laden.

So erstellen Sie ein neues Notebook

1. Klicken Sie in Azure Data Studio auf **Datei** und dann auf **Neues Notebook**.
2. Wählen Sie im Notebook den Kernel **Python3** aus, und klicken Sie dann auf **+code**.
3. Fügen Sie den Code in das Notebook ein, und klicken Sie auf **Alle ausführen**.

 ```Python
import pyodbc
import pandas as pd
# insert data from csv file into dataframe.
# working directory for csv file: type "pwd" in Azure Data Studio or Linux
# working directory in Windows c:\users\username
df = pd.read_csv("c:\\user\\username\department.csv")
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'yourservername' 
database = 'AdventureWorks' 
username = 'username' 
password = 'yourpassword' 
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
# Insert Dataframe into SQL Server:
for index, row in df.iterrows():
     cursor.execute("INSERT INTO HumanResources.DepartmentTest (DepartmentID,Name,GroupName) values(?,?,?)", row.DepartmentID, row.Name, row.GroupName)
cnxn.commit()
cursor.close()
```

## <a name="confirm-row-count-in-sql"></a>Bestätigen der Zeilenanzahl in SQL

Führen Sie die SQL-Anweisung aus, um zu bestätigen, dass die Tabelle erfolgreich mit Daten aus dem Datenrahmen geladen wurde.

```sql
SELECT count(*) from HumanResources.DepartmentTest;
```

Ergebnisse

```bash
(No column name)
16
```

## <a name="next-steps"></a>Nächste Schritte

+ [Zeichnen eines Histogramms für das Durchsuchen von Daten mit Python](../data-exploration/python-plot-histogram.md)