---
description: PREDICT (Transact-SQL)
title: PREDICT (Transact-SQL)
titleSuffix: SQL machine learning
ms.custom: ''
ms.date: 06/26/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- PREDICT
- PREDICT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PREDICT clause
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current||>=azure-sqldw-latest'
ms.openlocfilehash: 9ccee4b3502dafac331803ba1e8e56c068666bbc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347543"
---
# <a name="predict-transact-sql"></a>PREDICT (Transact-SQL)

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Generiert einen vorhergesagten Wert oder Bewertungen auf Grundlage eines gespeicherten Modells. Weitere Informationen finden Sie unter [Native Bewertung mithilfe der PREDICT T-SQL-Funktion](../../machine-learning/predictions/native-scoring-predict-transact-sql.md).

## <a name="syntax"></a>Syntax

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```syntaxsql
PREDICT  
(  
  MODEL = @model | model_literal,  
  DATA = object AS <table_alias>
  [, RUNTIME = ONNX ]
)  
WITH ( <result_set_definition> )  

<result_set_definition> ::=  
  {  
    { column_name  
      data_type  
      [ COLLATE collation_name ]  
      [ NULL | NOT NULL ]  
    }  
      [,...n ]  
  }  

MODEL = @model | model_literal  
```

::: moniker-end

::: moniker range=">=azure-sqldw-latest"

```syntaxsql
PREDICT  
(  
  MODEL = <model_object>,
  DATA = object AS <table_alias>
  [, RUNTIME = ONNX ]
)  
WITH ( <result_set_definition> )  

<result_set_definition> ::=  
  {  
    { column_name  
      data_type  
      [ COLLATE collation_name ]  
      [ NULL | NOT NULL ]  
    }  
      [,...n ]  
  }  

<model_object> ::=
  {
    model_literal
    | model_variable
    | ( scalar_subquery )
  }
```

::: moniker-end

### <a name="arguments"></a>Argumente

**MODEL**

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017"
Der Parameter `MODEL` wird verwendet, um das Modell anzugeben, das für die Bewertung oder Vorhersage verwendet wird. Das Modell wird als Variable, Literal oder Skalarausdruck angegeben.

`PREDICT` unterstützt Modelle, die mit den Paketen [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) und [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md) trainiert wurden.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
Der Parameter `MODEL` wird verwendet, um das Modell anzugeben, das für die Bewertung oder Vorhersage verwendet wird. Das Modell wird als Variable, Literal oder Skalarausdruck angegeben.

In Azure SQL Managed Instance unterstützt `PREDICT` Modelle im [ONNX-Format (Open Neural Network Exchange)](https://onnx.ai/get-started.html) oder Modelle, die mit den Paketen [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) und [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md) trainiert wurden.
::: moniker-end

::: moniker range=">=azure-sqldw-latest"
Der Parameter `MODEL` wird verwendet, um das Modell anzugeben, das für die Bewertung oder Vorhersage verwendet wird. Das Modell wird als Variable, Literal, Skalarausdruck oder skalare Unterabfrage angegeben.

In Azure Synapse Analytics unterstützt `PREDICT` Modelle im [ONNX-Format (Open Neural Network Exchange)](https://onnx.ai/get-started.html).
::: moniker-end

**DATEN**

Der DATA-Parameter wird verwendet, um die Daten anzugeben, die für die Bewertung oder Vorhersage verwendet werden. Daten werden in Form einer Tabellenquelle in der Abfrage angegeben. Die Tabellenquelle kann eine Tabelle, ein Tabellenalias, CTE-Alias, eine sicht oder Tabellenwertfunktion sein.

**RUNTIME = ONNX**

> [!IMPORTANT]
> Das Argument `RUNTIME = ONNX` ist nur in [Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview), [Azure SQL Edge](/azure/sql-database-edge/onnx-overview) und [Azure Synapse Analytics](/azure/synapse-analytics/overview-what-is) verfügbar.

Dieses Argument gibt die für die Modellausführung verwendete Machine-Learning-Engine an. Der Wert für den Parameter `RUNTIME` ist immer `ONNX`. Dieser Parameter ist für Azure SQL Edge und Azure Synapse Analytics erforderlich. In Azure SQL Managed Instance ist er optional und wird nur bei der Verwendung von ONNX-Modellen verwendet.

**WITH ( <result_set_definition> )**

Die WITH-Klausel wird verwendet, um das Schema der Ausgabe anzugeben, die von der `PREDICT`-Funktion zurückgegeben wird.

Zusätzlich zu den Spalten, die von der `PREDICT`-Funktion selbst zurückgegeben werden, stehen alle Spalten, die Teil der Dateneingabe sind, während der Abfrage zur Verfügung.

### <a name="return-values"></a>Rückgabewerte

Es steht kein vordefiniertes Schema zur Verfügung. Die Inhalte des Modells und die zurückgegebenen Spaltenwerte werden nicht überprüft.

- Die `PREDICT`-Funktion durchläuft die Spalten als Eingabe.
- Die `PREDICT`-Funktion generiert auch neue Spalten, allerdings hängen die Anzahl der Spalten und deren Datentypen vom Typ des Modells ab, das für die Vorhersage verwendet wurde.

Fehlermeldungen im Zusammenhang mit den Daten, dem Modell oder dem Spaltenformat werden von der zugrunde liegenden Vorhersagefunktion zurückgegeben, die dem Modell zugeordnet ist.

::: moniker range=">=sql-server-2017||>=sql-server-linux-2017"
## <a name="remarks"></a>Bemerkungen

Die `PREDICT`-Funktion wird in allen Editionen von SQL Server 2017 oder höher unter Windows und Linux unterstützt. [Machine Learning Services](../../machine-learning/sql-server-machine-learning-services.md) muss nicht aktiviert werden, um `PREDICT` zu verwenden.
::: moniker-end

### <a name="supported-algorithms"></a>Unterstützte Algorithmen

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017"
Das von Ihnen verwendete Modell muss mithilfe eines der unterstützten Algorithmen aus dem [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md)- oder dem [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md)-Paket erstellt worden sein. Eine Liste der derzeit unterstützten Modelle finden Sie unter [Native Bewertung mithilfe der T-SQL-Funktion PREDICT mit SQL Machine Learning](../../machine-learning/predictions/native-scoring-predict-transact-sql.md).
::: moniker-end
::: moniker range="=azure-sqldw-latest"
Algorithmen, die in das Modellformat [ONNX](https://onnx.ai/) konvertiert werden können, werden unterstützt.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Algorithmen, die in das Modellformat [ONNX](https://onnx.ai/) konvertiert werden können, und Modelle, die Sie mithilfe eines der unterstützten Algorithmen aus dem [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md)- oder dem [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md)-Paket erstellt haben, werden unterstützt. Eine Liste der derzeit unterstützten Algorithmen in RevoScaleR und revoscalepy finden Sie unter [Native Bewertung mithilfe der T-SQL-Funktion PREDICT mit SQL Machine Learning](../../machine-learning/predictions/native-scoring-predict-transact-sql.md).
::: moniker-end

### <a name="permissions"></a>Berechtigungen

Es sind zwar keine Berechtigungen für `PREDICT` erforderlich, jedoch benötigt der Benutzer `EXECUTE`-Berechtigungen für die Datenbank und Berechtigungen zum Abfragen von Daten, die als Eingaben verwendet werden. Der Benutzer muss außerdem das Modell aus einer Tabelle heraus lesen können, wenn das Modell in einer Tabelle gespeichert wurde.

## <a name="examples"></a>Beispiele

Die folgenden Beispiele veranschaulichen die Syntax zum Aufrufen von `PREDICT`.

### <a name="using-predict-in-a-from-clause"></a>Verwenden von PREDICT in einer FROM-Klausel

In diesem Beispiel wird auf die `PREDICT`-Funktion in der `FROM`-Klausel einer `SELECT`-Anweisung verwiesen:

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```sql
SELECT d.*, p.Score
FROM PREDICT(MODEL = @model,
    DATA = dbo.mytable AS d) WITH (Score FLOAT) AS p;
```

:::moniker-end

::: moniker range=">=azure-sqldw-latest"

```sql
DECLARE @model VARBINARY(max) = (SELECT test_model FROM scoring_model WHERE model_id = 1);

SELECT d.*, p.Score
FROM PREDICT(MODEL = @model,
    DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH (Score FLOAT) AS p;
```

::: moniker-end

Der Alias **d**, der für die Tabellenquelle im Parameter `DATA` angegeben ist, wird verwendet, um auf die Spalten zu verweisen, die zu `dbo.mytable` gehören. Der Alias **p**, der für die `PREDICT`-Funktion angegeben ist, wird verwendet, um auf die Spalten zu verweisen, die von der `PREDICT`-Funktion zurückgegeben werden.

- Das Modell wird als `varbinary(max)`-Spalte in einer Tabelle namens **Models** gespeichert. Weitere Informationen, z. B. die **ID** und eine **Beschreibung**, werden zur Identifikation des Modells in der Tabelle gespeichert.
- Der Alias **d**, der für die Tabellenquelle im Parameter `DATA` angegeben ist, wird verwendet, um auf die Spalten zu verweisen, die zu `dbo.mytable` gehören. Die Namen der Eingabedatenspalten sollten mit den Namen der Eingaben für das Modell übereinstimmen.
- Der Alias **p**, der für die `PREDICT`-Funktion angegeben ist, wird verwendet, um auf die vorhergesagte Spalte zu verweisen, die von der `PREDICT`-Funktion zurückgegeben wird. Der Spaltenname sollte mit dem Namen der Ausgabe für das Modell übereinstimmen.
- Alle Eingabedatenspalten und die vorhergesagten Spalten können über die SELECT-Anweisung angezeigt werden.

::: moniker range=">=azure-sqldw-latest"

Die vorherige Beispielabfrage kann umgeschrieben werden, um eine Sicht zu erstellen, indem `MODEL` als skalare Unterabfrage angegeben wird:

```sql
CREATE VIEW predictions
AS
SELECT d.*, p.Score
FROM PREDICT(MODEL = (SELECT test_model FROM scoring_model WHERE model_id = 1),
             DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH (Score FLOAT) AS p;
```

:::moniker-end

### <a name="combining-predict-with-an-insert-statement"></a>Kombinieren von PREDICT mit einer INSERT-Anweisung

Einer der gängigsten Anwendungsfälle für die Vorhersage ist das Generieren einer Bewertung für Eingabedaten und das anschließende Einfügen der vorhergesagten Werte in eine Tabelle. Im folgenden Beispiel wird davon ausgegangen, dass die aufrufende Anwendung eine gespeicherte Prozedur verwendet, um eine Zeile mit dem vorhergesagten Wert in eine Tabelle einzufügen:

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```sql
DECLARE @model VARBINARY(max) = (SELECT model FROM scoring_model WHERE model_name = 'ScoringModelV1');

INSERT INTO loan_applications (c1, c2, c3, c4, score)
SELECT d.c1, d.c2, d.c3, d.c4, p.score
FROM PREDICT(MODEL = @model, DATA = dbo.mytable AS d) WITH(score FLOAT) AS p;
```

:::moniker-end

::: moniker range=">=azure-sqldw-latest"

```sql
DECLARE @model VARBINARY(max) = (SELECT model FROM scoring_model WHERE model_name = 'ScoringModelV1');

INSERT INTO loan_applications (c1, c2, c3, c4, score)
SELECT d.c1, d.c2, d.c3, d.c4, p.score
FROM PREDICT(MODEL = @model, DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH(score FLOAT) AS p;
```

:::moniker-end

- Die Ergebnisse von `PREDICT` werden in einer Tabelle namens PredictionResults gespeichert. 
- Das Modell wird als `varbinary(max)`-Spalte in einer Tabelle namens **Models** gespeichert. Weitere Informationen, z. B. die ID und eine Beschreibung, können zur Identifikation des Modells in der Tabelle gespeichert werden.
- Der Alias **d**, der für die Tabellenquelle im Parameter `DATA` angegeben ist, wird verwendet, um auf die Spalten in `dbo.mytable` zu verweisen. Die Namen der Eingabedatenspalten sollten mit den Namen der Eingaben für das Modell übereinstimmen.
- Der Alias **p**, der für die `PREDICT`-Funktion angegeben ist, wird verwendet, um auf die vorhergesagte Spalte zu verweisen, die von der `PREDICT`-Funktion zurückgegeben wird. Der Spaltenname sollte mit dem Namen der Ausgabe für das Modell übereinstimmen.
- Alle Eingabespalten und die vorhergesagten Spalte können über die SELECT-Anweisung angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

- [Native Bewertung mithilfe der PREDICT T-SQL-Funktion](../../machine-learning/predictions/native-scoring-predict-transact-sql.md)
::: moniker range="=azure-sqldw-latest||=azuresqldb-mi-current"
-   [Erfahren Sie mehr über ONNX-Modelle.](/azure/machine-learning/concept-onnx#get-onnx-models)
::: moniker-end
