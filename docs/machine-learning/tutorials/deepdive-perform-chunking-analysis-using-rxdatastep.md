---
title: Blockweise Analyse in RevoScaleR
description: 'Tutorial 12 zu RevoScaleR: Segmentieren von Daten für die verteilte Analyse mithilfe der R-Programmiersprache in SQL Server'
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: a1dd8b47f7a251918eb87db9a8ec2dfd9d412a10
ms.sourcegitcommit: 9b41725d6db9957dd7928a3620fe4db41eb51c6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88179889"
---
# <a name="perform-chunking-analysis-using-rxdatastep-sql-server-and-revoscaler-tutorial"></a>Durchführen einer blockweisen Analyse mithilfe von rxDataStep (Tutorial für SQL Server und RevoScaleR)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Bei diesem Tutorial handelt es sich um das zwölfte Tutorial von [Lernprogramm: Verwenden von RevoScaleR-Funktionen für R mit SQL Server-Daten](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md). In diesem Lernprogramm erfahren Sie, wie Sie [RevoScaleR-Funktionen](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) mit SQL Server verwenden.

In diesem Tutorial verarbeiten Sie mithilfe der Funktion **rxDataStep** Daten in Blöcken, ohne dass das gesamte Dataset in den Speicher geladen und gleichzeitig verarbeitet werden muss, wie es üblicherweise in R der Fall ist. Die Funktion **rxDataStep** liest die Daten in Blöcken und wendet R-Funktionen nacheinander auf die einzelnen Datenblöcke an. Anschließend werden die Ergebnisse der Zusammenfassung für jeden Block in eine gemeinsame [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenquelle geschrieben. Wenn alle Daten gelesen wurden, werden die Ergebnisse zusammengeführt.

> [!TIP]
> In diesem Tutorial berechnen Sie mithilfe der **Tabellen**-Funktion in R eine Kontingenztabelle. Dieses Beispiel ist nur für Unterrichtszwecke bestimmt. 
> 
> Wenn Sie echte Datasets tabellarisieren möchten, verwenden Sie stattdessen die Funktionen **rxCrossTabs** oder **rxCube** in **RevoScaleR**, die für diese Art von Vorgang optimiert wurden.

## <a name="partition-data-by-values"></a>Partitionieren von Daten nach Werten

1. Erstellen Sie eine benutzerdefinierte R-Funktion, die die **Tabellen**-Funktion in R für jeden Datenblock aufruft, und geben Sie der neuen Funktion den Namen **ProcessChunk**.
  
    ```R
    ProcessChunk <- function( dataList) {
    # Convert the input list to a data frame and compute contingency table
    chunkTable <- table(as.data.frame(dataList))
  
    # Convert table output to a data frame with a single row
    varNames <- names(chunkTable)
    varValues <- as.vector(chunkTable)
    dim(varValues) <- c(1, length(varNames))
    chunkDF <- as.data.frame(varValues)
    names(chunkDF) <- varNames
  
    # Return the data frame, which has a single row
    return( chunkDF )
    }
    ```

2. Legen Sie den Computekontext auf den Server fest.
  
    ```R
    rxSetComputeContext(sqlCompute)
    ```
  
3. Definieren Sie eine SQL Server-Datenquelle für die Daten, die Sie verarbeiten. Beginnen Sie, indem Sie einer Variablen eine SQL-Abfrage zuweisen. Verwenden Sie anschließend diese Variable im Argument *sqlQuery* einer neuen SQL Server-Datenquelle.
  
    ```R
    dayQuery <-  "SELECT DayOfWeek FROM AirDemoSmallTest"
    inDataSource <- RxSqlServerData(sqlQuery = dayQuery,
        connectionString = sqlConnString,
        rowsPerRead = 50000,
        colInfo = list(DayOfWeek = list(type = "factor",
            levels = as.character(1:7))))
    ```

4. Optional können Sie für diese Datenquelle auch **rxGetVarInfo** ausführen. Sie enthält nun eine einzelne Spalte: *Var 1: DayOfWeek, Type: factor, no factor levels available*.
     
5. Erstellen Sie vor dem Anwenden dieser Faktorvariablen auf die Quelldaten eine separate Tabelle für die Zwischenergebnisse. Verwenden Sie zum Definieren der Daten erneut nur die Funktion **RxSqlServerData**, und vergewissern Sie sich, dass alle vorhandenen Tabellen mit dem gleichen Namen gelöscht sind.
  
    ```R
    iroDataSource = RxSqlServerData(table = "iroResults",   connectionString = sqlConnString)
    # Check whether the table already exists.
    if (rxSqlServerTableExists(table = "iroResults",  connectionString = sqlConnString))  { rxSqlServerDropTable( table = "iroResults", connectionString = sqlConnString) }
    ```
  
7.  Rufen Sie die benutzerdefinierte Funktion **ProcessChunk** auf, um die Daten während des Lesens zu transformieren, und verwenden Sie dabei die Funktion als *transformFunc*-Argument für die Funktion **rxDataStep**.
  
    ```R
    rxDataStep( inData = inDataSource, outFile = iroDataSource, transformFunc = ProcessChunk, overwrite = TRUE)
    ```
  
8.  Weisen Sie zum Anzeigen der Zwischenergebnisse von **ProcessChunk** die Ergebnisse von **rxImport** einer Variable zu, und geben Sie die Ergebnisse anschließend in der Konsole aus.
  
    ```R
    iroResults <- rxImport(iroDataSource)
    iroResults
    ```

    **Teilergebnisse**

    | Zeile \# |    1  |   2   |  3   |  4   |  5  |   6   |  7 |
    | --- | ---  | --- | ---  |  ---  | ---  | ---  | --- |
    | 1 | 8228 | 8924 | 6916 | 6932 | 6944 | 5602 | 6454 |
    | 2  | 8321  | 5351 | 7329 | 7411 | 7409 | 6487 | 7692 |

9. Summieren Sie die Spalten, und zeigen Sie die Ergebnisse in der Konsole an, um die Endergebnisse aller Blöcke zu berechnen.

    ```R
    finalResults <- colSums(iroResults)
    finalResults
    ```

    **Ergebnisse**

    1  |   2  |   3  |   4  |   5  |   6  |   7
    ---  |   ---  |   ---  |   ---  |   ---  |   ---  |   ---
    97975 | 77725 | 78875 | 81304 | 82987 | 86159 | 94975 

10. Rufen Sie **rxSqlServerDropTable** auf, um die Tabelle mit den Zwischenergebnissen zu entfernen.
  
    ```R
    rxSqlServerDropTable( table = "iroResults", connectionString = sqlConnString)
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [R-Tutorials für SQL Server](sql-server-r-tutorials.md)