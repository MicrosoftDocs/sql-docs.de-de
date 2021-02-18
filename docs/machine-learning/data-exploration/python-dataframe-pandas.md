---
title: Einfügen von Daten aus einer SQL-Tabelle in einen Python-Pandas-Datenrahmen
titleSuffix: SQL machine learning
description: In diesem Artikel erfahren Sie, wie Sie Daten aus einer SQL-Tabelle lesen und mithilfe von Python in einen Pandas-Datenrahmen einfügen können.
author: dphansen
ms.author: davidph
ms.date: 07/23/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 7b26838dceee9b933b4f33621af3b32bdb0a0c67
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349088"
---
# <a name="insert-data-from-a-sql-table-into-a-python-pandas-dataframe"></a>Einfügen von Daten aus einer SQL-Tabelle in einen Python-Pandas-Datenrahmen
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

In diesem Artikel wird beschrieben, wie SQL-Daten mithilfe des [pyodbc](../../connect/python/pyodbc/python-sql-driver-pyodbc.md)-Pakets in Python in einen [Pandas](https://pandas.pydata.org/)-Datenrahmen eingefügt werden. Die Zeilen und Spalten der Daten, die im Datenrahmen enthalten sind, können zur weiteren Datenuntersuchung verwendet werden.

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

Sie können überprüfen, ob die wiederhergestellte Datenbank vorhanden ist, indem Sie die Tabelle **Person.CountryRegion** abfragen:

```sql
USE AdventureWorks;
SELECT * FROM Person.CountryRegion;
```

## <a name="install-python-packages"></a>Installieren von Python-Paketen

[Laden Sie Azure Data Studio herunter, und führen Sie die Installation durch.](../../azure-data-studio/download-azure-data-studio.md)

Installieren Sie die folgenden Python-Pakete:
  * pyodbc
  * pandas

  Installieren Sie diese Pakete wie folgt:

  1. Klicken Sie in Ihrem Azure Data Studio-Notebook auf die Option **Pakete verwalten**.
  2. Klicken Sie dann im Bereich **Manage Packages** (Pakete verwalten) auf die Registerkarte **Add new** (Neue hinzufügen).
  3. Geben Sie für jedes der folgenden Pakete den jeweiligen Paketnamen ein, klicken Sie auf **Suchen** und dann auf **Installieren**.

## <a name="insert-data"></a>Einfügen von Daten

Verwenden Sie das folgende Skript, um Daten aus der Tabelle „Person.CountryRegion“ auszuwählen und in einen Dataframe einzufügen. Bearbeiten Sie die Variablen der Verbindungszeichenfolge „server“, „database“, „username“ und „password“, um eine Verbindung mit SQL herzustellen.

So erstellen Sie ein neues Notebook

1. Klicken Sie in Azure Data Studio auf **Datei** und dann auf **Neues Notebook**.
2. Wählen Sie im Notebook den Kernel **Python3** aus, und klicken Sie dann auf **+code**.
3. Fügen Sie den Code in das Notebook ein, und klicken Sie auf **Alle ausführen**.

```python
import pyodbc
import pandas as pd
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'servername' 
database = 'AdventureWorks' 
username = 'yourusername' 
password = 'databasename'  
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
# select 26 rows from SQL table to insert in dataframe.
query = "SELECT [CountryRegionCode], [Name] FROM Person.CountryRegion;"
df = pd.read_sql(query, cnxn)
print(df.head(26))
```

**Ausgabe**

Mit dem `print`-Befehl im obigen Skript werden die Datenzeilen aus dem `pandas`-Datenrahmen `df` angezeigt.

```text
CountryRegionCode                 Name
0                 AF          Afghanistan
1                 AL              Albania
2                 DZ              Algeria
3                 AS       American Samoa
4                 AD              Andorra
5                 AO               Angola
6                 AI             Anguilla
7                 AQ           Antarctica
8                 AG  Antigua and Barbuda
9                 AR            Argentina
10                AM              Armenia
11                AW                Aruba
12                AU            Australia
13                AT              Austria
14                AZ           Azerbaijan
15                BS         Bahamas, The
16                BH              Bahrain
17                BD           Bangladesh
18                BB             Barbados
19                BY              Belarus
20                BE              Belgium
21                BZ               Belize
22                BJ                Benin
23                BM              Bermuda
24                BT               Bhutan
25                BO              Bolivia
```

## <a name="next-steps"></a>Nächste Schritte

+ [Einfügen eines Python-Datenrahmens in SQL](../data-exploration/python-dataframe-sql-server.md)