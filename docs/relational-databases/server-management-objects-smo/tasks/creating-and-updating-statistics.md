---
description: Erstellen und Aktualisieren von Statistiken
title: Erstellen und Aktualisieren von Statistiken
ms.prod: sql
ms.prod_service: database-engine
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- statistical information [SMO]
ms.assetid: 47a0a172-a969-4deb-bca9-dd04401a0fe1
author: markingmyname
ms.author: maghan
ms.reviewer: matteot
ms.custom: seo-dt-2019
ms.date: 06/04/2020
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 64b631219afcd37bbc6883ef7e1c89d6335b7095
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475531"
---
# <a name="create-and-update-statistics"></a>Erstellen und Aktualisieren von Statistiken

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

In SMO können statistische Informationen über die Verarbeitung von Abfragen in der Datenbank mithilfe des  <xref:Microsoft.SqlServer.Management.Smo.Statistic>-Objekts gesammelt werden.

Es ist möglich, Statistiken für jede Spalte mit dem <xref:Microsoft.SqlServer.Management.Smo.Statistic> -Objekt und dem-Objekt zu erstellen <xref:Microsoft.SqlServer.Management.Smo.StatisticColumn> . Die <xref:Microsoft.SqlServer.Management.Smo.Statistic.Update%2A>-Methode kann ausgeführt werden, um die Statistik im <xref:Microsoft.SqlServer.Management.Smo.Statistic>-Objekt zu aktualisieren. Die Ergebnisse können im Abfrageoptimierer angezeigt werden.

## <a name="example"></a>Beispiel

Wenn Sie ein beliebiges Codebeispiel verwenden möchten, können Sie die Programmierumgebung, die Programmier Vorlage und die Programmiersprache auswählen, in der die Anwendung erstellt werden soll. Weitere Informationen finden Sie unter [Erstellen eines Visual C-&#35; SMO-Projekts in Visual Studio .net](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).

## <a name="create-and-update-statistics-in-visual-basic"></a>Erstellen und Aktualisieren von Statistiken in Visual Basic

In diesem Codebeispiel wird eine neue Tabelle für eine vorhandene Datenbank erstellt, für die das <xref:Microsoft.SqlServer.Management.Smo.Statistic>-Objekt und das <xref:Microsoft.SqlServer.Management.Smo.StatisticColumn>-Objekt erstellt werden.

```vb
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Reference the AdventureWorks2012 database.
Dim db As Database
db = srv.Databases("AdventureWorks2012")
'Reference the CreditCard table.
Dim tb As Table
tb = db.Tables("CreditCard", "Sales")
'Define a Statistic object by supplying the parent table and name arguments in the constructor.
Dim stat As Statistic
stat = New Statistic(tb, "Test_Statistics")
'Define a StatisticColumn object variable for the CardType column and add to the Statistic object variable.
Dim statcol As StatisticColumn
statcol = New StatisticColumn(stat, "CardType")
stat.StatisticColumns.Add(statcol)
'Create the statistic counter on the instance of SQL Server.
stat.Create()
```

## <a name="create-and-update-statistics-in-c"></a>Erstellen und Aktualisieren von Statistiken in C #

In diesem Codebeispiel wird eine neue Tabelle für eine vorhandene Datenbank erstellt, für die das <xref:Microsoft.SqlServer.Management.Smo.Statistic>-Objekt und das <xref:Microsoft.SqlServer.Management.Smo.StatisticColumn>-Objekt erstellt werden.

```csharp
public static void CreatingAndUpdatingStatistics()
{
    // Connect to the local, default instance of SQL Server.
    var srv = new Server();

    // Reference the AdventureWorks2012 database.
    var db = srv.Databases["AdventureWorks"];

    // Reference the CreditCard table.
    var tb = db.Tables["CreditCard", "Sales"];

    // Define a Statistic object by supplying the parent table and name
    // arguments in the constructor.
    var stat = new Statistic(tb, "Test_Statistics");

    // Define a StatisticColumn object variable for the CardType column
    // and add to the Statistic object variable.
    var statcol = new StatisticColumn(stat, "CardType");
    stat.StatisticColumns.Add(statcol);

    //Create the statistic counter on the instance of SQL Server.
    stat.Create();

    // List all the statistics object on the table (you will see the newly created one)
    foreach (var s in tb.Statistics.Cast<Statistic>())
        Console.WriteLine($"{s.ID}\t{s.Name}");

    // Output:
    //  2       AK_CreditCard_CardNumber
    //  1       PK_CreditCard_CreditCardID
    //  3       Test_Statistics
 }
```

## <a name="create-and-update-statistics-in-powershell"></a>Erstellen und Aktualisieren von Statistiken in PowerShell

In diesem Codebeispiel wird eine neue Tabelle für eine vorhandene Datenbank erstellt, für die das <xref:Microsoft.SqlServer.Management.Smo.Statistic>-Objekt und das <xref:Microsoft.SqlServer.Management.Smo.StatisticColumn>-Objekt erstellt werden.

```powershell
Import-Module SQLServer

# Connect to the local, default instance of SQL Server.  
$srv = Get-Item SQLSERVER:\SQL\localhost\DEFAULT

# Reference the AdventureWorks database.
$db = $srv.Databases["AdventureWorks"]

# Reference the CreditCard table.
$tb = $db.Tables["CreditCard", "Sales"]

# Define a Statistic object by supplying the parent table and name
# arguments in the constructor.
$stat = New-Object Microsoft.SqlServer.Management.Smo.Statistic($tb, "Test_Statistics")

# Define a StatisticColumn object variable for the CardType column
# and add to the Statistic object variable.
$statcol = New-Object Microsoft.SqlServer.Management.Smo.StatisticColumn($stat, "CardType")
$stat.StatisticColumns.Add($statcol)

# Create the statistic counter on the instance of SQL Server.
$stat.Create()

# Finally dump all the statistics (you can see the newly created one at the bottom)
$tb.Statistics

# Output:
# Name                                Last Updated Is From Index  Statistic Columns
#                                                  Creation
# ----                                ------------ -------------- -----------------
# AK_CreditCard_CardNumber      10/27/2017 2:33 PM True           {CardNumber}
# PK_CreditCard_CreditCardID    10/27/2017 2:33 PM True           {CreditCardID}
# Test_Statistics                 6/4/2020 8:11 PM False          {CardType}
```
