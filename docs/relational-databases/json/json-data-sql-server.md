---
description: JSON-Daten in SQL Server
title: Arbeiten mit JSON-Daten
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: quickstart
helpviewer_keywords:
- JSON
- JSON, built-in support
ms.assetid: c9a4e145-33c3-42b2-a510-79813e67806a
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||= azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 551a0ad762f6cc8fdc0a347c5896a3bbbea27b82
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075992"
---
# <a name="json-data-in-sql-server"></a>JSON-Daten in SQL Server

[!INCLUDE[appliesto-ss2016-asdb-asdw-xxx-md.md](../../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]

JSON ist ein beliebtes Textdatenformat, das zum Austauschen von Daten in modernen Webanwendungen und mobilen Anwendungen verwendet wird. JSON wird auch zum Speichern von unstrukturierten Daten in Protokolldateien oder NoSQL-Datenbanken wie Microsoft Azure Cosmos DB verwendet. Viele REST-Webdienste geben Ergebnisse zurück, die als JSON-Text formatiert sind, oder akzeptieren Daten, die im JSON-Format formatiert sind. Beispielsweise verfügen die meisten Azure-Dienste wie Azure Search, Azure Storage und Azure Cosmos DB über REST-Endpunkte, die JSON zurückgeben oder verarbeiten. JSON ist auch das hauptsächliche Format zum Austauschen von Daten zwischen Webseiten und Webservern über AJAX-Aufrufe. 

JSON-Funktionen in SQL Server ermöglichen Ihnen, NoSQL und relationale Konzepte in der gleichen Datenbank zu kombinieren. Sie können übliche relationale Spalten mit anderen Spalten kombinieren, die Dokumente enthalten, die als JSON-Text in der gleichen Tabelle formatiert sind, JSON-Dokumente analysieren und in relationale Strukturen importieren oder relationale Daten zu JSON-Text formatieren. Im folgenden Video sehen Sie, wie NoSQL- und relationale Konzepte in SQL Server und Azure SQL-Datenbanken durch JSON-Funktionen verbunden werden:

*JSON as a bridge between NoSQL and relational worlds*
> [!VIDEO https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds/player]

Hier ist ein Beispiel des JSON-Texts:

```json
[
  {
    "name": "John",
    "skills": ["SQL", "C#", "Azure"]
  },
  {
    "name": "Jane",
    "surname": "Doe"
  }
]
```

Durch Verwenden von integrierten SQL Server-Funktionen und -Operatoren können Sie die folgenden Aktionen mit JSON-Text ausführen:

- Analysieren Sie JSON-Text und lesen oder ändern Sie Werte.  
- Wandeln Sie Arrays von JSON-Objekten in das Tabellenformat um.  
- Wenden Sie eine beliebige Transact-SQL-Abfrage auf die konvertierte JSON-Objekte an.  
- Formatieren Sie die Ergebnisse der Transact-SQL-Abfragen in das JSON-Format.  
  
![Übersicht über integrierte JSON-Unterstützung](../../relational-databases/json/media/jsonslides1overview.png "Übersicht über integrierte JSON-Unterstützung")  
  
## <a name="key-json-capabilities-of-sql-server-and-sql-database"></a>Wichtige JSON-Funktionen von SQL Server und SQL-Datenbank

In den nächsten Abschnitten werden die wichtigsten Funktionen erläutert, die SQL Server durch die integrierte Unterstützung für JSON bereitstellt. Im folgenden Video sehen Sie, wie JSON-Funktionen und Operatoren verwenden werden:

*SQL Server 2016 and JSON Support*
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support/player?WT.mc_id=dataexposed-c9-niner]

### <a name="extract-values-from-json-text-and-use-them-in-queries"></a>Extrahieren von Werten aus JSON-Text und Verwenden dieser Werte in Abfragen

Wenn Sie JSON-Text verwenden, der in Datenbanktabellen gespeichert ist, können Sie Werte in dem JSON-Text lesen oder ändern, indem Sie die folgenden integrierten Funktionen verwenden:  

- [ISJSON (Transact-SQL)](../../t-sql/functions/isjson-transact-sql.md) testet, ob eine Zeichenfolge gültige JSON-Zeichenfolgen enthält.
- [JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md) extrahiert einen Skalarwert aus einer JSON-Zeichenfolge.
- [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md) extrahiert ein Objekt oder ein Array aus einer JSON-Zeichenfolge.
- [JSON_MODIFY (Transact-SQL)](../../t-sql/functions/json-modify-transact-sql.md) ändert einen Wert in der JSON-Zeichenfolge.

**Beispiel**

Im folgenden Beispiel verwendet die Abfrage sowohl relationale als auch JSON-Daten (gespeichert in der Spalte namens `jsonCol`) aus einer Tabelle:  
  
```sql  
SELECT Name, Surname,
  JSON_VALUE(jsonCol, '$.info.address.PostCode') AS PostCode,
  JSON_VALUE(jsonCol, '$.info.address."Address Line 1"') + ' '
  + JSON_VALUE(jsonCol, '$.info.address."Address Line 2"') AS Address,
  JSON_QUERY(jsonCol, '$.info.skills') AS Skills
FROM People
WHERE ISJSON(jsonCol) > 0
  AND JSON_VALUE(jsonCol, '$.info.address.Town') = 'Belgrade'
  AND Status = 'Active'
ORDER BY JSON_VALUE(jsonCol, '$.info.address.PostCode')
```  
  
Anwendungen und Tools erkennen keinen Unterschied zwischen den Werten aus skalaren Tabellenspalten und denen aus JSON-Spalten. Sie können Werte aus dem JSON-Text in jedem Teil der Transact-SQL-Abfrage verwenden (z.B. WHERE-, ORDER BY-, GROUP BY-Klauseln, Fenster-Aggregate usw.). JSON-Funktionen verwenden eine JavaScript-ähnliche Syntax zum Verweisen auf Werte in JSON-Text.

Weitere Informationen finden Sie unter [Überprüfen, Abfragen und Ändern von JSON-Daten mit integrierten Funktionen (SQL Server)](../../relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server.md), [JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md) und [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md).  
  
### <a name="change-json-values"></a>Ändern der JSON-Werte

Wenn Sie Teile des JSON-Texts ändern müssen, können Sie die [JSON_MODIFY (Transact-SQL)](../../t-sql/functions/json-modify-transact-sql.md)-Funktion verwenden, um den Wert einer Eigenschaft in einer JSON-Zeichenfolge zu aktualisieren und die aktualisierte JSON-Zeichenfolge zurückzugeben. Im folgenden Beispiel wird der Wert einer Eigenschaft in einer Variablen aktualisiert, die JSON enthält:  
  
```sql
DECLARE @json NVARCHAR(MAX);
SET @json = '{"info": {"address": [{"town": "Belgrade"}, {"town": "Paris"}, {"town":"Madrid"}]}}';
SET @json = JSON_MODIFY(@json, '$.info.address[1].town', 'London');
SELECT modifiedJson = @json;
```

**Ergebnisse**

|modifiedJson|  
|--------|  
|{"info":{"address":[{"town":"Belgrade"},{"town":"London"},{"town":"Madrid"}]}|  
  
### <a name="convert-json-collections-to-a-rowset"></a>Konvertieren der JSON-Sammlungen in ein Rowset

Eine benutzerdefinierte Abfragesprache ist zur Abfrage von JSON-Daten in SQL Server nicht erforderlich. Zum Abfragen von JSON-Daten können Sie Standard-T-SQL verwenden. Wenn Sie eine Abfrage erstellen oder JSON-Daten protokollieren müssen, können Sie die JSON-Daten einfach durch Aufrufen der **OPENJSON**-Rowsetfunktion in Zeilen und Spalten konvertieren. Weitere Informationen finden Sie unter [Konvertieren von JSON-Daten in Zeilen und Spalten mit OPENJSON (SQL Server)](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md).  
  
Im folgenden Beispiel wird **OPENJSON** aufgerufen, um ein Array von Objekten, das in einer `@json`-Variablen gespeichert ist, in ein Rowset zu transformieren, das mithilfe der SQL **SELECT**-Standardanweisung abgefragt werden kann:  
  
```sql  
DECLARE @json NVARCHAR(MAX);
SET @json = N'[
  {"id": 2, "info": {"name": "John", "surname": "Smith"}, "age": 25},
  {"id": 5, "info": {"name": "Jane", "surname": "Smith"}, "dob": "2005-11-04T12:00:00"}
]';

SELECT *
FROM OPENJSON(@json)
  WITH (
    id INT 'strict $.id',
    firstName NVARCHAR(50) '$.info.name',
    lastName NVARCHAR(50) '$.info.surname',
    age INT,
    dateOfBirth DATETIME2 '$.dob'
  );
```

**Ergebnisse**

|id|firstName|lastName|age|dateOfBirth|  
|--------|---------------|--------------|---------|-----------------|  
|2|John|Smith|25||  
|5|Jane|Smith||2005-11-04T12:00:00|  
  
**OPENJSON** transformiert das Array von JSON-Objekten in eine Tabelle, wobei jedes Objekt als eine Zeile dargestellt wird. Die Schlüssel/Wert-Paare werden als Zellen zurückgegeben. Die Ausgabe verwendet die folgenden Regeln:

- **OPENJSON** konvertiert JSON-Werte in die Typen, die in der **WITH**-Klausel angegeben sind.
- **OPENJSON** kann sowohl einfache Schlüssel/Wert-Paare als auch geschachtelte, hierarchisch angeordnete Objekte verarbeiten.
- Sie müssen nicht alle Felder zurückgeben, die im JSON-Text enthalten sind.
- **OPENJSON** gibt NULL-Werte zurück, wenn keine JSON-Werte vorhanden sind.
- Sie können optional einen Pfad zu der Typspezifikation angeben, um auf eine geschachtelte Eigenschaft, oder um mit einem anderen Namen auf eine Eigenschaft zu verweisen.
- Das optionale **strenge** im Pfad gibt an, dass die Werte für die angegebenen Eigenschaften im JSON-Text vorhanden sein müssen.

Weitere Informationen finden Sie unter [Konvertieren von JSON-Daten in Zeilen und Spalten mit OPENJSON (SQL Server)](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md) und [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md).  

JSON-Dokumente enthalten möglicherweise untergeordnete Elemente und hierarchische Daten, die relationalen Standardspalten nicht direkt zugeordnet werden können. Sie können in diesem Fall die JSON-Hierarchie vereinfachen, indem Sie die übergeordnete Entität mit untergeordneten Arrays verknüpfen.

Im folgenden Beispiel weist das zweite Objekt des Arrays untergeordnete Arrays auf, die Fähigkeiten von Personen darstellen (Skills-Array). Jedes untergeordnete Objekt kann mithilfe des zusätzlichen Funktionsaufrufs `OPENJSON` analysiert werden:

```sql  
DECLARE @json NVARCHAR(MAX);
SET @json = N'[  
  {"id": 2, "info": {"name": "John", "surname": "Smith"}, "age": 25},
  {"id": 5, "info": {"name": "Jane", "surname": "Smith", "skills": ["SQL", "C#", "Azure"]}, "dob": "2005-11-04T12:00:00"}  
]';

SELECT *  
FROM OPENJSON(@json)  
  WITH (
    id INT 'strict $.id',
    firstName NVARCHAR(50) '$.info.name',
    lastName NVARCHAR(50) '$.info.surname',  
    age INT,
    dateOfBirth DATETIME2 '$.dob',
    skills NVARCHAR(MAX) '$.info.skills' AS JSON
  )
OUTER APPLY OPENJSON(skills)
  WITH (skill NVARCHAR(8) '$');
```

Das **Skills**-Array wird im ersten `OPENJSON`-Element als das ursprüngliche JSON-Textfragment angegeben und mithilfe des `APPLY`-Operators an eine andere `OPENJSON`-Funktion übergeben. Die zweite `OPENJSON`-Funktion analysiert das JSON-Array und gibt Zeichenfolgenwerte als Rowsets mit nur einer Spalte zurück, die mit dem Ergebnis des ersten `OPENJSON`-Elements verknüpft werden.
Das Ergebnis dieser Abfrage wird in der folgenden Tabelle dargestellt:

**Ergebnisse**

|id|firstName|lastName|age|dateOfBirth|skill|  
|--------|---------------|--------------|---------|-----------------|----------|  
|2|John|Smith|25|||  
|5|Jane|Smith||2005-11-04T12:00:00|SQL|
|5|Jane|Smith||2005-11-04T12:00:00|C#|
|5|Jane|Smith||2005-11-04T12:00:00|Azure|

`OUTER APPLY OPENJSON` verknüpft die Entität auf der ersten Ebene mit dem untergeordneten Array und gibt ein vereinfachtes Resultset zurück. Aufgrund der JOIN-Methode wird die zweite Zeile für jedes Skill-Array wiederholt.

### <a name="convert-sql-server-data-to-json-or-export-json"></a>Konvertieren von SQL Server-Daten in JSON oder Exportieren von JSON

>[!NOTE]
>Das Konvertieren von Azure Synapse Analytics-Daten in JSON oder das Exportieren von JSON wird nicht unterstützt.

Formatieren Sie Daten aus SQL Server oder die Ergebnisse von SQL-Abfragen im JSON-Format, indem Sie die **FOR JSON** -Klausel einer **SELECT** -Anweisung hinzufügen. Verwenden Sie **FOR JSON**, um die Formatierung der JSON-Ausgabe von den Clientanwendungen an SQL Server zu delegieren. Weitere Informationen finden Sie unter [Formatieren von Abfrageergebnissen als JSON mit FOR JSON (SQL Server)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md).  
  
Im folgenden Beispiel wird der PATH-Modus mit der **FOR JSON**-Klausel verwendet:  
  
```sql  
SELECT id, firstName AS "info.name", lastName AS "info.surname", age, dateOfBirth AS dob
FROM People
FOR JSON PATH;
```

Am **FOR JSON** -Klausel formatiert SQL-Ergebnisse als JSON-Text, der für alle Apps bereitgestellt werden kann, die JSON verstehen. Die Option PATH verwendet in der SELECT-Klausel durch Punkte getrennte Aliase, um Objekte in den Abfrageergebnissen zu schachteln.  
  
**Ergebnisse**

```json  
[
  {
    "id": 2,
    "info": {
      "name": "John",
      "surname": "Smith"
    },
    "age": 25
  },
  {
    "id": 5,
    "info": {
      "name": "Jane",
      "surname": "Smith"
    },
    "dob": "2005-11-04T12:00:00"
  }
]
```  
  
Weitere Informationen finden Sie unter [Formatieren von Abfrageergebnissen als JSON mit FOR JSON (SQL Server)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md) und [FOR-Klausel (Transact-SQL)](../../t-sql/queries/select-for-clause-transact-sql.md).  

## <a name="use-cases-for-json-data-in-sql-server"></a>Anwendungsfälle für JSON-Daten in SQL Server

Durch die JSON-Unterstützung in SQL Server und Azure SQL-Datenbank können Sie relationale Konzepte mit NoSQL-Konzepten verbinden. Dadurch lassen sich relationale Daten leicht in teilweise strukturierte Daten und umgekehrt transformieren. JSON ist jedoch kein Ersatz für vorhandene relationale Modelle. Im Folgenden werden einige spezifische Anwendungsfälle beschrieben, in denen die JSON-Unterstützung in SQL Server und SQL-Datenbank genutzt wird.

### <a name="simplify-complex-data-models"></a>Vereinfachen komplexer Datenmodelle

Denormalisieren Sie Ihr Datenmodell mit JSON-Feldern anstelle von mehreren untergeordneten Tabellen.

### <a name="store-retail-and-e-commerce-data"></a>Speichern von Einzelhandels- und E-Commerce-Daten

Speichern Sie Informationen über Produkte mit vielen veränderbaren Attributen in einem denormalisierten Modell, um mehr Flexibilität zu erhalten.

### <a name="process-log-and-telemetry-data"></a>Verarbeiten von Protokoll- und Telemetriedaten

Laden Sie als JSON-Dateien gespeicherte Protokolldaten, fragen Sie diese ab und analysieren Sie sie mithilfe der leistungsfähigen Programmiersprache Transact-SQL.

### <a name="store-semi-structured-iot-data"></a>Speichern von teilweise strukturieren IoT-Daten

Laden Sie bei einer Echtzeitanalyse von IoT-Daten eingehende Daten direkt in die Datenbank, anstatt sie durch Staging an einem Speicherort bereitzustellen.

### <a name="simplify-rest-api-development"></a>Vereinfachen der REST-API-Entwicklung

Transformieren Sie relationale Daten aus Ihrer Datenbank problemlos in das JSON-Format, das von den REST-APIs verwendet wird, die Ihre Website unterstützen.

## <a name="combine-relational-and-json-data"></a>Kombinieren von relationalen und JSON-Daten

SQL Server bietet ein Hybridmodell zum Speichern und Verarbeiten von relationalen Daten und JSON-Daten, indem die Transact-SQL-Standardsprache verwendet wird. Sie können Sammlungen Ihrer JSON-Dokumente nach Tabellen anordnen, Beziehungen zwischen ihnen herstellen, in Tabellen gespeicherte, stark typisierte Skalarspalten mit flexiblen Schlüssel-Wert/Paaren kombinieren, die in JSON-Spalten gespeichert werden, und sowohl Skalarwerte als auch JSON-Werte in einer oder mehreren Tabellen abfragen, indem Sie vollständiges Transact-SQL verwenden.

JSON-Text wird in `VARCHAR`- oder `NVARCHAR`-Spalten gespeichert und als Nur-Text indiziert. Jede SQL Server-Funktion oder -Komponente, die Text unterstützt, unterstützt auch JSON, daher gibt es fast keine Einschränkungen hinsichtlich der Interaktion zwischen JSON und anderen SQL Server-Funktionen. Sie können JSON im Arbeitsspeicher oder in temporalen Tabellen speichern, Sicherheitsprädikate auf Zeilenebene auf JSON-Text anwenden usw.

Wenn bei Ihnen reine JSON-Arbeitsauslastungen vorliegen, für die Sie eine Abfragesprache verwenden möchten, die der Verarbeitung von JSON-Dokumenten angepasst ist, sollten Sie Microsoft Azure [Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) in Erwägung ziehen.  
  
Hier folgen einige Anwendungsfälle, die veranschaulichen, wie Sie die integrierte JSON-Unterstützung in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verwenden können.  

## <a name="store-and-index-json-data-in-sql-server"></a>Speichern und Indizieren von JSON-Daten in SQL Server

JSON ist ein Textformat. Deshalb können JSON-Dokumente in einer SQL-Datenbank-Instanz in `NVARCHAR`-Spalten gespeichert werden. Da der Typ `NVARCHAR` in allen untergeordneten Systemen von SQL Server unterstützt wird, können Sie JSON-Dokumente in Tabellen mit **CLUSTERED COLUMNSTORE**-Indizes, **arbeitsspeicheroptimierten** Tabellen oder externen Dateien speichern, die mit OPENROWSET oder PolyBase gelesen werden können.

Weitere Informationen zu den verschiedenen Optionen zum Speichern, Indizieren und Optimieren von JSON-Daten in SQL Server finden Sie unter:

- [Speichern von JSON-Dokumenten in SQL Server oder SQL-Datenbank](store-json-documents-in-sql-tables.md)
- [Indizieren von JSON-Daten](index-json-data.md)
- [Optimieren der JSON-Verarbeitung mit In-Memory-OLTP](optimize-json-processing-with-in-memory-oltp.md)

### <a name="load-json-files-into-sql-server"></a>Laden von JSON-Dateien in SQL Server  

Sie können Informationen, die in Dateien gespeichert sind, als Standard-JSON oder als JSON-Text formatieren, in dem Zeilenumbrüche als Trennzeichen verwendet werden. SQL Server kann die Inhalte aus JSON-Dateien importieren, sie über die **OPENJSON**- oder **JSON_VALUE**-Funktion analysieren und in Tabellen laden.  
  
-   Wenn Ihre JSON-Dokumente in lokalen Dateien, auf freigegebenen Netzlaufwerken oder ein Azure Files-Speicherorten gespeichert sind, auf die SQL Server zugreifen kann, können Sie einen Massenimport durchführen, um die JSON-Daten in SQL Server zu laden.
  
-   Wenn Ihre JSON-Dateien, die Zeilenumbrüche als Trennzeichen verwenden, in Azure-Blobspeicher oder im Hadoop-Dateisystem gespeichert werden, können Sie den JSON-Text mithilfe von PolyBase laden, in Transact-SQL-Code analysieren und in Tabellen laden.  

### <a name="import-json-data-into-sql-server-tables"></a>Importieren von JSON-Daten in SQL Server-Tabellen

Wenn Sie JSON-Daten aus einem externen Dienst in SQL Server laden müssen, können Sie die Daten mit **OPENJSON** in SQL Server importieren, anstatt die Daten in der Anwendungsschicht zu analysieren.  
  
```sql  
DECLARE @jsonVariable NVARCHAR(MAX);

SET @jsonVariable = N'[
  {
    "Order": {  
      "Number":"SO43659",  
      "Date":"2011-05-31T00:00:00"  
    },  
    "AccountNumber":"AW29825",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":1  
    }  
  },  
  {  
    "Order": {  
      "Number":"SO43661",  
      "Date":"2011-06-01T00:00:00"  
    },  
    "AccountNumber":"AW73565",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":3  
    }  
  }
]';

-- INSERT INTO <sampleTable>  
SELECT SalesOrderJsonData.*
FROM OPENJSON (@jsonVariable, N'$')
  WITH (
    Number VARCHAR(200) N'$.Order.Number',
    Date DATETIME N'$.Order.Date',
    Customer VARCHAR(200) N'$.AccountNumber',
    Quantity INT N'$.Item.Quantity'
  ) AS SalesOrderJsonData;
```

Sie können den Inhalt der JSON-Variablen durch eine externen REST-Dienst bereitstellen, als Parameter aus einem clientseitigen JavaScript-Framework senden oder aus externen Dateien laden. Sie können Ergebnisse aus JSON-Text ganz einfach in eine SQL Server-Tabelle einfügen, dort aktualisieren oder zusammenführen.

## <a name="analyze-json-data-with-sql-queries"></a>Analysieren von JSON-Daten mit SQL-Abfragen

Wenn Sie JSON-Daten für Berichtszwecke filtern oder aggregieren müssen, können Sie JSON mithilfe von **OPENJSON** in ein relationales Format transformieren. Sie können dann Standard- oder integrierte Funktionen von [!INCLUDE[tsql](../../includes/tsql-md.md)] verwenden, um die Berichte vorzubereiten.  
  
```sql
SELECT Tab.Id, SalesOrderJsonData.Customer, SalesOrderJsonData.Date
FROM SalesOrderRecord AS Tab
CROSS APPLY OPENJSON (Tab.json, N'$.Orders.OrdersArray')
  WITH (
    Number VARCHAR(200) N'$.Order.Number',
    Date DATETIME N'$.Order.Date',
    Customer VARCHAR(200) N'$.AccountNumber',
    Quantity INT N'$.Item.Quantity'
  ) AS SalesOrderJsonData
WHERE JSON_VALUE(Tab.json, '$.Status') = N'Closed'
ORDER BY JSON_VALUE(Tab.json, '$.Group'), Tab.DateModified;
```  
  
Sie können sowohl Standardtabellenspalten als auch Werte aus JSON-Text in derselben Abfrage verwenden. Sie können Indizes zum `JSON_VALUE(Tab.json, '$.Status')`-Ausdruck hinzufügen, um die Leistung der Abfrage zu verbessern. Weitere Informationen finden Sie unter [Indizieren von JSON-Daten](../../relational-databases/json/index-json-data.md).

## <a name="return-data-from-a-sql-server-table-formatted-as-json"></a>Rückgabe von Daten aus einer SQL Server-Tabelle im JSON-Format

Wenn Sie einen Webdienst verwenden, der Daten von der Datenbankebene übernimmt und sie im JSON-Format zurückgibt, oder wenn Sie JavaScript-Frameworks oder -Bibliotheken haben, die Daten im JSON-Format akzeptieren, können Sie JSON-Ausgabe direkt in einer SQL-Abfrage formatieren. Anstatt Code zu schreiben oder eine Bibliothek einzuschließen, um tabellarische Abfrageergebnisse zu konvertieren und dann Objekte in das JSON-Format zu serialisieren, können Sie auch **FOR JSON** verwenden, um die JSON-Formatierung an SQL Server zu delegieren.  
  
Sie können z.B. eine JSON-Ausgabe generieren, die der OData-Spezifikation entspricht. Der Webdienst erwartet eine Anforderung und Antwort im folgenden Format: 
  
- Anforderung: `/Northwind/Northwind.svc/Products(1)?$select=ProductID,ProductName`  

- Antwort: `{"@odata.context": "https://services.odata.org/V4/Northwind/Northwind.svc/$metadata#Products(ProductID,ProductName)/$entity", "ProductID": 1, "ProductName": "Chai"}`  
  
Diese OData-URL stellt eine Anforderung für die Spalten „ProductID“ und „ProductName“ für das Produkt mit der `ID` 1 dar. Sie können **FOR JSON** verwenden, um die Ausgabe erwartungsgemäß in SQL Server zu formatieren.  
  
```sql  
SELECT 'https://services.odata.org/V4/Northwind/Northwind.svc/$metadata#Products(ProductID,ProductName)/$entity' AS '@odata.context',
  ProductID,
  Name as ProductName
FROM Production.Product
WHERE ProductID = 1
FOR JSON AUTO;
```  
  
Das Ergebnis dieser Abfrage ist ein vollständig mit der OData-Spezifikation konformer JSON-Text. Formatierung und Escapevorgang werden von SQL Server übernommen. SQL Server kann Abfrageergebnisse auch in einem beliebigen Format formatieren, etwa OData JSON oder GeoJSON.  
  
## <a name="test-drive-built-in-json-support-with-the-adventureworks-sample-database"></a>Testen Sie die integrierte JSON-Unterstützung mithilfe der AdventureWorks-Beispieldatenbank.

Laden Sie zum Abrufen der AdventureWorks-Beispieldatenbank zumindest die Datenbankdatei und die Beispiel- und Skriptdatei von [GitHub](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks) herunter.

Nach der Wiederherstellung der Beispieldatenbank in einer Instanz von SQL Server 2016 extrahieren Sie die Beispieldateien, und öffnen Sie dann die Datei *JSON Sample Queries procedures views and indexes.sql* aus dem JSON-Ordner. Führen Sie die Skripts in dieser Datei aus, um einige der vorhandenen Daten als JSON-Daten neu zu formatieren, testen Sie Beispielabfragen und -berichte für die JSON-Daten, indizieren Sie die JSON-Daten, und importieren und exportieren Sie JSON.  
  
Mit den Skripts, die in der Datei enthalten sind, haben Sie die folgenden Möglichkeiten:  
  
- Denormalisieren Sie die vorhandenen Schemas, um Spalten mit JSON-Daten zu erstellen.

  - Speichern Sie Informationen aus den Tabellen „SalesReasons“, „SalesOrderDetails“, „SalesPerson“, „Customer“ sowie aus anderen Tabellen, die auftragsbezogene Daten enthalten, in der Tabelle „SalesOrder_json“ in JSON-Spalten.  
  
  - Speichern Sie Informationen aus den Tabellen „EmailAddresses“ bzw. „PersonPhone“ als Arrays von JSON-Objekten in der Tabelle „Person_json“.  
  
- Erstellen Sie Prozeduren und Sichten, die JSON-Daten abfragen.  
  
- Indizieren Sie die JSON-Daten. Erstellen Sie Indizes zu JSON-Eigenschaften und Volltextindizes.  
  
- Importieren und exportieren Sie JSON. Erstellen Sie Prozeduren, die den Inhalt der Tabellen „Person“ und „SalesOrder“ als JSON-Ergebnisse exportieren, führen Sie diese Prozeduren aus, und importieren und aktualisieren Sie die Tabellen „Person“ und „SalesOrder“, indem Sie JSON-Eingabe verwenden.  
  
- Führen Sie Abfragebeispiele aus. Führen Sie einige Abfragen aus, die die gespeicherten Prozeduren und Sichten aufrufen, die Sie in den Schritten 2 und 4 erstellt haben.  
  
- Bereinigen Sie Skripts. Führen Sie diesen Teil nicht aus, wenn Sie die gespeicherten Prozeduren und Sichten beibehalten möchten, die Sie in den Schritten 2 und 4 erstellt haben.  
  
## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Weitere Informationen zu JSON in SQL Server und Azure SQL-Datenbank  
  
### <a name="microsoft-videos"></a>Microsoft-Videos

Eine visuelle Einführung in die JSON-Unterstützung, die in SQL Server und Azure SQL-Datenbank integriert ist, finden Sie in den folgenden Videos:

*Using JSON in SQL Server 2016 and Azure SQL Database*
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database/player?WT.mc_id=dataexposed-c9-niner]

*Building REST API with SQL Server using JSON functions (Erstellen einer REST-API mithilfe von JSON-Funktionen mit SQL Server)*
> [!VIDEO https://www.youtube.com/embed/0m6GXF3-5WI]

### <a name="reference-articles"></a>Referenzartikel

- [FOR-Klausel (Transact-SQL)](../../t-sql/queries/select-for-clause-transact-sql.md) (FOR JSON)  
- [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md)  
- [JSON-Funktionen (Transact-SQL)](../../t-sql/functions/json-functions-transact-sql.md)  
  - [ISJSON (Transact-SQL)](../../t-sql/functions/isjson-transact-sql.md)  
  - [JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md)  
  - [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md)  
  - [JSON_MODIFY (Transact-SQL)](../../t-sql/functions/json-modify-transact-sql.md)  
