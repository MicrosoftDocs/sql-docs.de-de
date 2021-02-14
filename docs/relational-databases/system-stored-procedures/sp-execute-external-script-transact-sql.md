---
description: sp_execute_external_script (Transact-SQL)
title: sp_execute_external_script (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: reference
f1_keywords:
- sp_execute_external_script_TSQL
- sys.sp_execute_external_script
- sys.sp_execute_external_script_TSQL
- sp_execute_external_script
dev_langs:
- TSQL
helpviewer_keywords:
- sp_execute_external_script
ms.assetid: de4e1fcd-0e1a-4af3-97ee-d1becc7f04df
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: f7c407416b8181864e7cc2fecdc4c95f97ab03cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355254"
---
# <a name="sp_execute_external_script-transact-sql"></a>sp_execute_external_script (Transact-SQL)
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Die gespeicherte Prozedur **sp_execute_external_script** führt ein als Eingabe Argument bereitgestelltes Skript für die Prozedur aus und wird mit [Machine Learning Services](../../machine-learning/sql-server-machine-learning-services.md) -und [Spracherweiterungen](../../language-extensions/language-extensions-overview.md)verwendet. 

Für Machine Learning Services werden [python](../../machine-learning/concepts/extension-python.md) und [R](../../machine-learning/concepts/extension-r.md) unterstützte Sprachen. Für Spracherweiterungen wird Java unterstützt, muss aber mit [Create externe Language](../../t-sql/statements/create-external-language-transact-sql.md)definiert werden.

Zum Ausführen von **sp_execute_external_script** müssen Sie zuerst Machine Learning Services oder Spracherweiterungen installieren. Weitere Informationen finden Sie unter [Installieren von SQL Server Machine Learning Services (python und R) unter Windows](../../machine-learning/install/sql-machine-learning-services-windows-install.md) und [Linux](../../linux/sql-server-linux-setup-machine-learning.md)oder [Installieren von SQL Server Spracherweiterungen unter Windows](../../language-extensions/install/windows-java.md) und [Linux](../../linux/sql-server-linux-setup-language-extensions-java.md).
::: moniker-end

::: moniker range="=sql-server-2017"
Die gespeicherte Prozedur **sp_execute_external_script** führt ein Skript aus, das als Eingabe Argument für die Prozedur bereitgestellt wird, und wird mit [Machine Learning Services](../../machine-learning/sql-server-machine-learning-services.md) auf SQL Server 2017 verwendet.

Für Machine Learning Services werden [python](../../machine-learning/concepts/extension-python.md) und [R](../../machine-learning/concepts/extension-r.md) unterstützte Sprachen.

Zum Ausführen von **sp_execute_external_script** müssen Sie zuerst Machine Learning Services installieren. Weitere Informationen finden Sie unter [Installieren von SQL Server Machine Learning Services (python und R) unter Windows](../../machine-learning/install/sql-machine-learning-services-windows-install.md).
::: moniker-end

::: moniker range="=sql-server-2016"
Die gespeicherte Prozedur **sp_execute_external_script** führt ein als Eingabe Argument bereitgestelltes Skript für die Prozedur aus und wird mit [R Services](../../machine-learning/r/sql-server-r-services.md) auf SQL Server 2016 verwendet.

[R](../../machine-learning/concepts/extension-r.md) Services ist die unterstützte Sprache.

Zum Ausführen von **sp_execute_external_script** müssen Sie zuerst R Services installieren. Weitere Informationen finden Sie unter [Installieren von SQL Server Machine Learning Services (python und R) unter Windows](../../machine-learning/install/sql-r-services-windows-install.md).
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
Die gespeicherte Prozedur **sp_execute_external_script** führt ein als Eingabe Argument bereitgestelltes Skript für die Prozedur aus und wird mit [Machine Learning Services in Azure SQL-verwaltete Instanz](/azure/azure-sql/managed-instance/machine-learning-services-overview)verwendet.

Für Machine Learning Services werden [python](../../machine-learning/concepts/extension-python.md) und [R](../../machine-learning/concepts/extension-r.md) unterstützte Sprachen.

Zum Ausführen **sp_execute_external_script** müssen Sie zuerst Machine Learning Services aktivieren. Weitere Informationen finden Sie in der [Machine Learning Services in der Dokumentation zu Azure SQL verwaltete Instanz](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
## <a name="syntax"></a>Syntax

```
sp_execute_external_script
    @language = N'language',
    @script = N'script'  
    [ , @input_data_1 = N'input_data_1' ]
    [ , @input_data_1_name = N'input_data_1_name' ]  
    [ , @input_data_1_order_by_columns = N'input_data_1_order_by_columns' ]
    [ , @input_data_1_partition_by_columns = N'input_data_1_partition_by_columns' ]  
    [ , @output_data_1_name = N'output_data_1_name' ]  
    [ , @parallel = 0 | 1 ]  
    [ , @params = N'@parameter_name data_type [ OUT | OUTPUT ] [ ,...n ]' ] 
    [ , @parameter1 = 'value1' [ OUT | OUTPUT ] [ ,...n ] ]
```
::: moniker-end
::: moniker range="=sql-server-2016"
## <a name="syntax-for-sql-server-2017-and-earlier"></a>Syntax für SQL Server 2017 und früher

```
sp_execute_external_script   
    @language = N'language',   
    @script = N'script'  
    [ , @input_data_1 = N'input_data_1' ]   
    [ , @input_data_1_name = N'input_data_1_name' ]  
    [ , @output_data_1_name = N'output_data_1_name' ]  
    [ , @parallel = 0 | 1 ]  
    [ , @params = N'@parameter_name data_type [ OUT | OUTPUT ] [ ,...n ]' ] 
    [ , @parameter1 = 'value1' [ OUT | OUTPUT ] [ ,...n ] ]
```
::: moniker-end

## <a name="arguments"></a>Argumente
 **\@ Sprache** = N '*Sprache*'  
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
 Gibt die Skriptsprache an. *Language* ist vom **Datentyp vom Datentyp sysname**. Gültige Werte sind **R**, **python** und jede Sprache, die in [externer Sprache erstellen](../../t-sql/statements/create-external-language-transact-sql.md) (z. b. Java) definiert ist.
::: moniker-end
::: moniker range="=sql-server-2017"
 Gibt die Skriptsprache an. *Language* ist vom **Datentyp vom Datentyp sysname**. Gültige Werte in SQL Server 2017 sind **R** und **python**.
::: moniker-end
::: moniker range="=sql-server-2016"
 Gibt die Skriptsprache an. *Language* ist vom **Datentyp vom Datentyp sysname**. In SQL Server 2016 ist **R** der einzige gültige Wert.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
 Gibt die Skriptsprache an. *Language* ist vom **Datentyp vom Datentyp sysname**. Gültige Werte in Azure SQL verwaltete Instanz sind **R** und **python**.
::: moniker-end

 **\@ Skript** =*N Skript Skript* für externe Sprache als Literale oder Variablen Eingabe angegeben. *Skript* ist vom Datentyp **nvarchar (max)**.  

`[ @input_data_1 =  N'input_data_1' ]` Gibt die Eingabedaten an, die vom externen Skript in Form einer Abfrage verwendet werden [!INCLUDE[tsql](../../includes/tsql-md.md)] . Der Datentyp von *input_data_1* ist vom Datentyp **nvarchar (max)**.

`[ @input_data_1_name = N'input_data_1_name' ]` Gibt den Namen der Variablen an, die für die Darstellung der durch definierten Abfrage verwendet wird @input_data_1 . Der Datentyp der Variablen im externen Skript hängt von der Sprache ab. Im Fall von R ist die Eingabe Variable ein Datenrahmen. Im Fall von python müssen Eingaben tabellarisch sein. *input_data_1_name* ist vom **Datentyp vom Datentyp sysname**.  Der Standardwert ist *Input DataSet*.  

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
`[ @input_data_1_order_by_columns = N'input_data_1_order_by_columns' ]` Wird verwendet, um Modelle pro Partition zu erstellen. Gibt den Namen der Spalte an, die zum Sortieren des Resultsets verwendet wird, z. b. nach Produktname. Der Datentyp der Variablen im externen Skript hängt von der Sprache ab. Im Fall von R ist die Eingabe Variable ein Datenrahmen. Im Fall von python müssen Eingaben tabellarisch sein.

`[ @input_data_1_partition_by_columns = N'input_data_1_partition_by_columns' ]` Wird verwendet, um Modelle pro Partition zu erstellen. Gibt den Namen der Spalte an, die zum Segmentieren von Daten verwendet wird, z. b. geografische Region oder Datum Der Datentyp der Variablen im externen Skript hängt von der Sprache ab. Im Fall von R ist die Eingabe Variable ein Datenrahmen. Im Fall von python müssen Eingaben tabellarisch sein. 
::: moniker-end

`[ @output_data_1_name =  N'output_data_1_name' ]` Gibt den Namen der Variablen im externen Skript an, die die Daten enthält, die nach [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dem Abschluss des Aufrufens einer gespeicherten Prozedur zurückgegeben werden sollen. Der Datentyp der Variablen im externen Skript hängt von der Sprache ab. Bei R muss die Ausgabe ein Datenrahmen sein. Bei python muss die Ausgabe ein Pandas-dataframe sein. *output_data_1_name* ist vom **Datentyp vom Datentyp sysname**.  Der Standardwert ist *outputdataset*.  

`[ @parallel = 0 | 1 ]` Aktivieren Sie die parallele Ausführung von R-Skripts `@parallel` , indem Sie den Parameter auf 1 festlegen. Der Standardwert für diesen Parameter ist 0 (keine Parallelität). Wenn `@parallel = 1` und die Ausgabe direkt an den Client Computer gestreamt werden, ist die- `WITH RESULT SETS` Klausel erforderlich, und es muss ein Ausgabe Schema angegeben werden.  

 + Bei R-Skripts, in denen keine revoscaler-Funktionen verwendet werden, kann die Verwendung des-  `@parallel` Parameters für die Verarbeitung großer Datasets vorteilhaft sein, vorausgesetzt, das Skript kann triviell parallelisiert werden. Wenn Sie z. b. die R- `predict` Funktion mit einem Modell verwenden, um neue Vorhersagen zu generieren, legen Sie `@parallel = 1` als Hinweis auf die Abfrage-Engine fest. Wenn die Abfrage parallelisiert werden kann, werden die Zeilen entsprechend der **MAXDOP** -Einstellung verteilt.  
  
 + Bei R-Skripts, die revoscaler-Funktionen verwenden, wird die parallele Verarbeitung automatisch verarbeitet, und Sie sollten nicht `@parallel = 1` für den **sp_execute_external_script** -Befehl angeben.  
  
`[ @params = N'@parameter_name data_type [ OUT | OUTPUT ] [ ,...n ]' ]` Eine Liste der Eingabeparameter Deklarationen, die im externen Skript verwendet werden.  
  
`[ @parameter1 = 'value1' [ OUT | OUTPUT ] [ ,...n ] ]` Eine Liste von Werten für die Eingabeparameter, die vom externen Skript verwendet werden.  

## <a name="remarks"></a>Bemerkungen

> [!IMPORTANT]
> Die Abfrage Struktur wird von SQL Machine Learning gesteuert, und Benutzer können keine beliebigen Vorgänge für die Abfrage ausführen.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Verwenden Sie **sp_execute_external_script** , um in einer unterstützten Sprache geschriebene Skripts auszuführen. Unterstützte Sprachen sind **python** und **R** , die mit Machine Learning Services verwendet werden, sowie alle Sprachen, die mit Spracherweiterungen für die [Erstellung externer Sprache](../../t-sql/statements/create-external-language-transact-sql.md) (z. b. Java) definiert sind.
::: moniker-end
::: moniker range="=sql-server-2017"
Verwenden Sie **sp_execute_external_script** , um in einer unterstützten Sprache geschriebene Skripts auszuführen. Unterstützte Sprachen sind **python** und **R** in SQL Server 2017 Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
Verwenden Sie **sp_execute_external_script** , um in einer unterstützten Sprache geschriebene Skripts auszuführen. Die einzige unterstützte Sprache ist **r** in SQL Server 2016 r-Diensten.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Verwenden Sie **sp_execute_external_script** , um in einer unterstützten Sprache geschriebene Skripts auszuführen. Unterstützte Sprachen sind **python** und **R** in Azure SQL verwaltete Instanz Machine Learning Services.
::: moniker-end

Standardmäßig werden Resultsets, die von dieser gespeicherten Prozedur zurückgegeben werden, mit unbenannten Spalten ausgegeben. Spaltennamen, die in einem Skript verwendet werden, sind für die Skript Umgebung lokal und werden im Ausgabe Ergebnissatz nicht wiedergegeben. Verwenden Sie die-Klausel von, um Resultsetspalten zu benennen `WITH RESULT SET` [`EXECUTE`](../../t-sql/language-elements/execute-transact-sql.md) .

Zusätzlich zum Zurückgeben eines Resultsets können Sie skalare Werte mithilfe von Output-Parametern an zurückgeben.

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
Sie können die Ressourcen steuern, die von externen Skripts verwendet werden, indem Sie einen externen Ressourcenpool konfigurieren. Weitere Informationen finden Sie unter [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md). Informationen über die Arbeitsauslastung können aus den Katalog Sichten der Ressourcenkontrolle, den DMV-und-Leistungsindikatoren abgerufen werden. Weitere Informationen finden Sie unter [Resource Governor Katalog Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md), [Resource Governor zugehörige dynamische Verwaltungs Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)und [SQL Server externen Scripts-Objekts](../../relational-databases/performance-monitor/sql-server-external-scripts-object.md).  
::: moniker-end

### <a name="monitor-script-execution"></a>Überwachen der Skriptausführung

Überwachen Sie die Skriptausführung mithilfe von [sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md) und [sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
### <a name="parameters-for-partition-modeling"></a>Parameter für die Partitions Modellierung

Sie können zwei zusätzliche Parameter festlegen, die das Modellieren von partitionierten Daten ermöglichen. Partitionen basieren auf einer oder mehreren Spalten, die Sie bereitstellen, die auf natürliche Weise ein Dataset in logische Partitionen segmentieren, die nur während der Skriptausführung erstellt und verwendet werden. Spalten mit sich wiederholenden Werten für Alter, Geschlecht, geografische Region, Datum oder Uhrzeit sind einige Beispiele, die partitionierten DataSets verleihen.

Die beiden Parameter sind **input_data_1_partition_by_columns** und **input_data_1_order_by_columns**, wobei der zweite Parameter verwendet wird, um das Resultset zu sortieren. Die Parameter werden als Eingaben an, `sp_execute_external_script` wobei das externe Skript einmal für jede Partition ausgeführt wird. Weitere Informationen und Beispiele finden Sie unter [Tutorial: Erstellen von Partitions basierten Modellen](../../machine-learning/tutorials/r-tutorial-create-models-per-partition.md).

Sie können das Skript parallel ausführen, indem Sie angeben `@parallel=1` . Wenn die Eingabe Abfrage parallelisiert werden kann, sollten Sie `@parallel=1` als Teil ihrer Argumente auf festlegen `sp_execute_external_script` . Standardmäßig arbeitet der Abfrageoptimierer unter `@parallel=1` Tabellen mit mehr als 256 Zeilen, aber wenn Sie diesen explizit behandeln möchten, enthält dieses Skript den Parameter als Demonstration.

> [!Tip]
> Für Trainingsworkloads können Sie `@parallel` mit einem beliebigen arbiträren Trainingsskript verwenden, sogar solche, die nicht von Microsoft stammende RX-Algorithmen verwenden In der Regel bieten nur RevoScaleR-Algorithmen (mit dem RX-Präfix) Parallelität in Trainingsszenarios in SQL Server. Mit den neuen Parametern in SQL Server 2019 und höher können Sie jedoch ein Skript parallelisieren, das Funktionen aufruft, die nicht speziell mit dieser Funktion entwickelt wurden.
::: moniker-end

### <a name="streaming-execution-for-python-and-r-scripts"></a>Streaming-Ausführung für python-und R-Skripts  

Streaming ermöglicht das Python-oder R-Skript, mit mehr Daten zu arbeiten, als in den Arbeitsspeicher passen. Geben Sie in der-Auflistung einen ganzzahligen Wert für den-Parameter an, um die Anzahl der beim Streaming übergebenen Zeilen zu steuern `@r_rowsPerRead` `@params` .  Wenn Sie z. b. ein Modell trainieren, das sehr große Daten verwendet, können Sie den Wert so anpassen, dass weniger Zeilen gelesen werden, um sicherzustellen, dass alle Zeilen in einem Daten Segment gesendet werden können. Sie können diesen Parameter auch verwenden, um die Anzahl der Zeilen zu verwalten, die gleichzeitig gelesen und verarbeitet werden, um Probleme mit der Serverleistung zu verringern. 
  
Sowohl der `@r_rowsPerRead` Parameter für das Streaming als auch das- `@parallel` Argument sollten als Hinweise angesehen werden. Damit der Hinweis angewendet wird, muss es möglich sein, einen SQL-Abfrageplan zu generieren, der die parallele Verarbeitung einschließt. Wenn dies nicht möglich ist, kann die parallele Verarbeitung nicht aktiviert werden.  
  
> [!NOTE]  
> Streaming und parallele Verarbeitung werden nur in der Enterprise Edition unterstützt. Sie können die Parameter in den Abfragen in der Standard Edition einschließen, ohne einen Fehler zu erhalten, aber die Parameter haben keine Auswirkung, und R-Skripts werden in einem einzelnen Prozess ausgeführt.  
  
## <a name="restrictions"></a>Beschränkungen  

### <a name="data-types"></a>Datentypen

Die folgenden Datentypen werden nicht unterstützt, wenn Sie in der Eingabe Abfrage oder den Parametern **sp_execute_external_script** Prozedur verwendet werden, und es wird ein nicht unterstützter Typfehler zurückgegeben.  

Um dieses Problem zu umgehen, wandeln Sie **die Spalte** oder den Wert in einen unterstützten Typ um, [!INCLUDE[tsql](../../includes/tsql-md.md)] bevor Sie Sie an das externe Skript senden.  
  
+ **Cursor**  
  
+ **timestamp**  
  
+ **datetime2**, **DateTimeOffset**, **Zeit**  
  
+ **sql_variant**  
  
+ **Text**, **Bild**  
  
+ **xml**  
  
+ **hierarchyid**, **Geometrie**, **Geografie**  
  
+ Benutzerdefinierte CLR-Typen

Im Allgemeinen wird jedes Resultset, das einem Datentyp nicht zugeordnet werden kann [!INCLUDE[tsql](../../includes/tsql-md.md)] , als NULL ausgegeben.  

### <a name="restrictions-specific-to-r"></a>Spezifische Einschränkungen für R

Wenn die Eingabe **DateTime** -Werte enthält, die nicht in den zulässigen Wertebereich in R passen, werden die Werte in **na** konvertiert. Dies ist erforderlich, da SQL Machine Learning eine größere Anzahl von Werten zulässt, als in der Sprache R unterstützt werden.

Float-Werte (z. `+Inf` b `-Inf` .,, `NaN` ) werden in SQL Machine Learning nicht unterstützt, obwohl beide Sprachen IEEE 754 verwenden. Aktuelles Verhalten sendet die Werte einfach direkt an SQL. Folglich löst der SQL-Client einen Fehler aus. Daher werden diese Werte in **null** konvertiert.

## <a name="permissions"></a>Berechtigungen

Erfordert die Berechtigung zum **Ausführen beliebiger externer Skript** Datenbanken.  

## <a name="examples"></a>Beispiele

Dieser Abschnitt enthält Beispiele dafür, wie diese gespeicherte Prozedur zum Ausführen von R-oder python-Skripts mit verwendet werden kann [!INCLUDE[tsql](../../includes/tsql-md.md)] .

### <a name="a-return-an-r-data-set-to-sql-server"></a>A. Zurückgeben eines R-Datasets auf SQL Server  

Im folgenden Beispiel wird eine gespeicherte Prozedur erstellt, die **sp_execute_external_script** verwendet, um das in R enthaltene IRIS-DataSet zurückzugeben.  

```sql
DROP PROC IF EXISTS get_iris_dataset;  
go  
CREATE PROC get_iris_dataset
AS  
BEGIN  
 EXEC   sp_execute_external_script  
       @language = N'R'  
     , @script = N'iris_data <- iris;'
     , @input_data_1 = N''  
     , @output_data_1_name = N'iris_data'
     WITH RESULT SETS (("Sepal.Length" float not null,
           "Sepal.Width" float not null,  
        "Petal.Length" float not null,
        "Petal.Width" float not null, "Species" varchar(100)));  
END;
GO
```

### <a name="b-create-a-python-model-and-generate-scores-from-it"></a>B. Erstellen eines Python-Modells und Generieren von Bewertungen daraus

In diesem Beispiel wird veranschaulicht, wie verwendet wird `sp_execute_external_script` , um Ergebnisse für ein einfaches Python-Modell zu generieren.

```sql
CREATE PROCEDURE [dbo].[py_generate_customer_scores]
AS
BEGIN

-- Input query to generate the customer data
DECLARE @input_query NVARCHAR(MAX) = N'SELECT customer, orders, items, cost FROM dbo.Sales.Orders'

EXEC sp_execute_external_script @language = N'Python', @script = N'
import pandas as pd
from sklearn.cluster import KMeans

# Get data from input query
customer_data = my_input_data

# Define the model
n_clusters = 4
est = KMeans(n_clusters=n_clusters, random_state=111).fit(customer_data[["orders","items","cost"]])
clusters = est.labels_
customer_data["cluster"] = clusters

OutputDataSet = customer_data
'
, @input_data_1 = @input_query
, @input_data_1_name = N'my_input_data'
WITH RESULT SETS (("CustomerID" int, "Orders" float,"Items" float,"Cost" float,"ClusterResult" float));
END;
GO
```

Spaltenüberschriften, die im Python-Code verwendet werden, werden nicht in SQL Server ausgegeben. Verwenden Sie daher die with result-Anweisung, um die Spaltennamen und Datentypen anzugeben, die von SQL verwendet werden sollen.

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
### <a name="c-generate-an-r-model-based-on-data-from-sql-server"></a>C. Generieren eines R-Modells auf der Grundlage von Daten aus SQL Server  

Im folgenden Beispiel wird eine gespeicherte Prozedur erstellt, die **sp_execute_external_script** verwendet, um ein Iris-Modell zu generieren und das Modell zurückzugeben.  

> [!NOTE]
> Für dieses Beispiel ist eine vorab Installation des e1071-Pakets erforderlich. Weitere Informationen finden Sie unter [Installieren zusätzlicher R-Pakete auf SQL Server](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md).

```sql
DROP PROC IF EXISTS generate_iris_model;
GO
CREATE PROC generate_iris_model
AS
BEGIN
 EXEC sp_execute_external_script  
      @language = N'R'  
     , @script = N'  
          library(e1071);  
          irismodel <-naiveBayes(iris_data[,1:4], iris_data[,5]);  
          trained_model <- data.frame(payload = as.raw(serialize(irismodel, connection=NULL)));  
'  
     , @input_data_1 = N'select "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species" from iris_data'  
     , @input_data_1_name = N'iris_data'  
     , @output_data_1_name = N'trained_model'  
    WITH RESULT SETS ((model varbinary(max)));  
END;
GO
```

Um ein ähnliches Modell mithilfe von Python zu generieren, ändern Sie die Sprachen-ID von `@language=N'R'` zu `@language = N'Python'` und nehmen die notwendigen Änderungen im `@script`-Argument vor. Alle anderen Parameter funktionieren genauso wie bei R.
::: moniker-end

Zur Bewertung können Sie auch die native [PREDICT](../../t-sql/queries/predict-transact-sql.md)-Funktion verwenden, die in der Regel schneller ist, weil sie die Python- bzw. R-Laufzeit nicht aufrufen muss.

## <a name="see-also"></a>Weitere Informationen

+ [SQL Machine Learning](../../machine-learning/index.yml)
+ [SQL Server Spracherweiterungen](../../language-extensions/language-extensions-overview.md). 
+ [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
+ [Externe Bibliothek &#40;Transact-SQL-&#41;erstellen ](../../t-sql/statements/create-external-library-transact-sql.md)  
+ [sp_prepare &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-prepare-transact-sql.md)   
+ [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
+ [Externe Skripts aktiviert (Serverkonfigurationsoption)](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)   
+ [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)   
+ [SQL Server, Externes Skript-Objekt](../../relational-databases/performance-monitor/sql-server-external-scripts-object.md)  
+ [sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md)  
+ [sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md)