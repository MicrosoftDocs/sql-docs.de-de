---
title: Indizes für speicheroptimierte Tabellen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie sich ein Index einer speicheroptimierten Tabelle von einem herkömmlichen Index einer datenträgerbasierten Tabelle in SQL Server und Azure SQL-Datenbank unterscheidet.
ms.custom: ''
ms.date: 09/16/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: eecc5821-152b-4ed5-888f-7c0e6beffed9
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 15cabb6877e249a6f0be9d38449c85c13bbda78b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351239"
---
# <a name="indexes-on-memory-optimized-tables"></a>Indizes für speicheroptimierte Tabellen

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Alle speicheroptimierten Tabellen müssen mindestens einen Index enthalten, da die Zeilen durch die Indizes miteinander verbunden werden. Für eine speicheroptimierte Tabelle wird jeder Index auch speicheroptimiert. Ein Index für eine speicheroptimierte Tabelle unterscheidet sich auf vielfältige Weise von einem herkömmlichen Index für eine datenträgerbasierte Tabelle:  

- Datenzeilen werden nicht in Seiten gespeichert, sodass es keine Sammlung von Seiten bzw. Erweiterungen, Partitionen oder Zuordnungseinheiten gibt, auf die zum Abrufen aller Seiten einer Tabelle verwiesen werden kann. Für einen der verfügbaren Indextypen gibt es Indexseiten. Diese Indextypen werden jedoch anders gespeichert als Indizes für datenträgerbasierte Tabellen. Sie lassen nicht den herkömmlichen Fragmentierungstyp innerhalb einer Seite anwachsen, sodass sie keinen Füllfaktor haben.
- Änderungen, die bei der Datenbearbeitung an Indizes in speicheroptimierten Tabellen vorgenommen werden, werden niemals auf den Datenträger geschrieben. Nur die Datenzeilen und Änderungen an den Daten werden in das Transaktionsprotokoll geschrieben. 
- Speicheroptimierte Indizes werden neu erstellt, wenn die Datenbank wieder online geschaltet wird. 

Alle Indizes in speicheroptimierten Tabellen werden basierend auf den Indexdefinitionen bei der Datenbankwiederherstellung erstellt.

Bei dem Index muss es sich um einen der folgenden handeln:  
  
- Hashindex  
- Nicht gruppierter speicheroptimierter Index (d.h. die interne Standardstruktur einer B-Struktur) 
  
*Hashindizes* werden unter [Hashindizes für speicheroptimierte Tabellen](../../relational-databases/sql-server-index-design-guide.md#hash_index) ausführlicher erläutert.  
*Nicht gruppierte Indizes* werden unter [Nicht gruppierte Indizes für speicheroptimierte Tabellen](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index) ausführlicher behandelt.  
*Columnstore* -Indizes werden in einem [anderen Artikel](../../relational-databases/indexes/columnstore-indexes-overview.md)behandelt.  

## <a name="syntax-for-memory-optimized-indexes"></a>Syntax für speicheroptimierte Indizes  
  
Jede CREATE TABLE-Anweisung für eine speicheroptimierte Tabelle muss einen Index enthalten, entweder explizit über einen INDEX oder implizit über eine PRIMAY KEY- oder UNIQUE-Einschränkung.
  
Für eine Deklaration mit dem Standard DURABILITY = SCHEMA\_AND_DATA muss die speicheroptimierte Tabelle einen Primärschlüssel enthalten. Die PRIMARY KEY NONCLUSTERED-Klausel in der folgenden CREATE TABLE-Anweisung erfüllt zwei Anforderungen:  
  
- Sie stellt einen Index bereit, um die Mindestanforderung von einem Index in der CREATE TABLE-Anweisung zu erfüllen.  
- Sie stellt den Primärschlüssel bereit, der für die SCHEMA\_AND_DATA-Klausel erforderlich ist.  

    ```sql
    CREATE TABLE SupportEvent  
    (  
        SupportEventId   int NOT NULL  
            PRIMARY KEY NONCLUSTERED,  
        ...  
    )  
        WITH (  
            MEMORY_OPTIMIZED = ON,  
            DURABILITY = SCHEMA_AND_DATA);  
    ```

> [!NOTE]  
> Für [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] besteht ein Limit von 8 Indizes pro speicheroptimierte Tabelle oder Tabellentyp. Ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] und in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] gibt es keine Begrenzung mehr für die spezifische Anzahl von Indizes für speicheroptimierte Tabellen und Tabellentypen.
  
### <a name="code-sample-for-syntax"></a>Codebeispiel für die Syntax  
  
Dieser Unterabschnitt enthält einen Transact-SQL-Codeblock, der die Syntax zum Erstellen von verschiedenen Indizes für eine speicheroptimierte Tabelle darstellt. Der Code veranschaulicht Folgendes:  
  
1. Erstellen Sie eine speicheroptimierte Tabelle.  
2. Verwenden Sie ALTER TABLE-Anweisungen, um zwei Indizes hinzuzufügen.  
3. Fügen Sie einige Zeilen Daten ein (INSERT).  
   
    ```sql
    DROP TABLE IF EXISTS SupportEvent;  
    go  

    CREATE TABLE SupportEvent  
    (  
        SupportEventId   int               not null   identity(1,1)  
        PRIMARY KEY NONCLUSTERED,  

        StartDateTime        datetime2     not null,  
        CustomerName         nvarchar(16)  not null,  
        SupportEngineerName  nvarchar(16)      null,  
        Priority             int               null,  
        Description          nvarchar(64)      null  
    )  
        WITH (  
        MEMORY_OPTIMIZED = ON,  
        DURABILITY = SCHEMA_AND_DATA);  
    go  
        
        --------------------  
        
    ALTER TABLE SupportEvent  
        ADD CONSTRAINT constraintUnique_SDT_CN  
        UNIQUE NONCLUSTERED (StartDateTime DESC, CustomerName);  
    go  

    ALTER TABLE SupportEvent  
        ADD INDEX idx_hash_SupportEngineerName  
        HASH (SupportEngineerName) WITH (BUCKET_COUNT = 64);  -- Nonunique.  
    go  
        
        --------------------  
        
    INSERT INTO SupportEvent  
        (StartDateTime, CustomerName, SupportEngineerName, Priority, Description)  
        VALUES  
        ('2016-02-23 13:40:41:123', 'Abby', 'Zeke', 2, 'Display problem.'     ),  
        ('2016-02-24 13:40:41:323', 'Ben' , null  , 1, 'Cannot find help.'    ),  
        ('2016-02-25 13:40:41:523', 'Carl', 'Liz' , 2, 'Button is gray.'      ),  
        ('2016-02-26 13:40:41:723', 'Dave', 'Zeke', 2, 'Cannot unhide column.');  
    go 
    ``` 
  
## <a name="duplicate-index-key-values"></a>Doppelte Indexschlüsselwerte

Durch doppelte Werte für einen Indexschlüssel kann die Leistung speicheroptimierter Tabellen reduziert werden. Duplikate für das System durchlaufen Eingangsketten für die meisten Lese- und Schreibvorgänge für Indizes. Wenn eine Kette mehr als 100 doppelte Einträge umfasst, kann die Leistungsminderung messbar werden.

### <a name="duplicate-hash-values"></a>Doppelte Hashwerte

Dieses Problem ist bei Hashindizes stärker sichtbar. Hashindizes sind aufgrund der folgenden Aspekte stärker beeinträchtigt:

- Die niedrigeren Kosten pro Vorgang für Hashindizes
- Die Interferenz großer doppelter Ketten mit der Hashkollisionskette

Um die Duplizierung in einem Index zu reduzieren, können Sie folgende Anpassungen vornehmen:

- Verwenden Sie einen nicht gruppierten Index.
- Fügen Sie am Ende des Indexschlüssels zusätzliche Spalten hinzu, um die Anzahl von Duplikaten zu verringern.
  - Sie können beispielsweise Spalten hinzufügen, die auch im Primärschlüssel enthalten sind.

Weitere Informationen zu Hashkollisionen finden Sie unter [Hashindizes für speicheroptimierte Tabellen](../../relational-databases/sql-server-index-design-guide.md#hash_index).

### <a name="example-improvement"></a>Verbesserungsbeispiel

Es folgt ein Beispiel dafür, wie Sie ineffiziente Leistung in Ihrem Index vermeiden können.

Angenommen, Sie haben eine Tabelle `Customers` mit einem Primärschlüssel für `CustomerId` und einem Index in der Spalte `CustomerCategoryID`. In der Regel sind in einer bestimmten Kategorie viele Kunden enthalten. Daher gibt es viele doppelte Werte für „CustomerCategoryID“ in einem bestimmten Schlüssel des Indexes.

In diesem Szenario hat sich die Verwendung eines nicht gruppierten Index für `(CustomerCategoryID, CustomerId)` bewährt. Dieser Index kann für Abfragen verwendet werden, die ein Prädikat unter Einbeziehung von `CustomerCategoryID` verwenden, doch enthält der Indexschlüssel keine Duplikate. Daher wird weder durch die doppelten „CustomerCategoryID“-Werte noch durch die zusätzliche Spalte im Index eine ineffiziente Indexwartung verursacht.

Die folgende Abfrage zeigt die durchschnittliche Anzahl doppelter Indexschlüsselwerte für `CustomerCategoryID` in der Tabelle `Sales.Customers`in der Beispieldatenbank [WideWorldImporters](../../samples/wide-world-importers-what-is.md).

```sql
SELECT AVG(row_count) FROM
    (SELECT COUNT(*) AS row_count 
        FROM Sales.Customers
        GROUP BY CustomerCategoryID) a
```

Um die durchschnittliche Anzahl der Indexschlüsselduplikate für Ihre eigene Tabelle und den Index zu evaluieren, ersetzen Sie `Sales.Customers` durch Ihren Tabellennamen und `CustomerCategoryID` durch die Liste der Indexschlüsselspalten.

## <a name="comparing-when-to-use-each-index-type"></a>Vergleichen des Verwendungszeitpunkts für jeden Indextyp  
  
Die Art Ihrer spezifischen Abfragen bestimmt, welche Art von Index die beste Wahl ist.  

Beim Implementieren speicheroptimierter Tabellen in einer vorhandenen Anwendung gilt die allgemeine Empfehlung, mit nicht gruppierten Indizes zu beginnen, da ihre Funktionen mehr den Funktionen herkömmlicher gruppierter und nicht gruppierter Indizes für datenträgerbasierte Tabellen ähneln. 
  
### <a name="recommendations-for-nonclustered-index-use"></a>Empfehlungen für die Verwendung nicht gruppierter Indizes  
  
Ein nicht gruppierter Index ist gegenüber einem Hashindex zu bevorzugen, wenn:  
  
- Abfragen eine `ORDER BY`-Klausel für die indizierte Spalte enthalten.  
- Bei Abfragen nur die führenden Spalten eines mehrspaltigen Indexes getestet werden.  
- Abfragen die indizierte Spalte testen mithilfe einer `WHERE`-Klausel mit:  
  - Einer Ungleichheit: `WHERE StatusCode != 'Done'`  
  - Einem Wertebereichsscan: `WHERE Quantity >= 100`  
  
In allen folgenden SELECT-Anweisungen wird ein nicht gruppierter Index gegenüber einem Hashindex bevorzugt:  

```sql
SELECT CustomerName, Priority, Description 
FROM SupportEvent  
WHERE StartDateTime > DateAdd(day, -7, GetUtcDate());  

SELECT StartDateTime, CustomerName  
FROM SupportEvent  
ORDER BY StartDateTime DESC; -- ASC would cause a scan.

SELECT CustomerName  
FROM SupportEvent  
WHERE StartDateTime = '2016-02-26';  
```
  
### <a name="recommendations-for-hash-index-use"></a>Empfehlungen für die Verwendung von Hashindizes   
  
[Hashindizes](../../relational-databases/sql-server-index-design-guide.md#hash_index) werden in erster Linie für gezielte Suchvorgänge und nicht für Bereichsscans verwendet.

Ein Hashindex ist gegenüber einem nicht gruppierten Index vorzuziehen, wenn Abfragen Gleichheitsprädikate, verwenden, und die `WHERE`-Klausel wird allen Indexschlüsselspalten zugeordnet, wie im folgenden Beispiel gezeigt wird:  
  
```sql
SELECT CustomerName 
FROM SupportEvent  
WHERE SupportEngineerName = 'Liz';
```  

### <a name="multi-column-index"></a>Mehrspaltiger Index  
  
Ein mehrspaltiger Index könnte ein nicht gruppierter Index oder ein Hashindex sein. Nehmen wir an, die Indexspalten sind „col1“ und „col2“. Gemäß der folgenden `SELECT`-Anweisung wäre nur der nicht gruppierte Index für den Abfrageoptimierer nützlich:  
  
```sql
SELECT col1, col3  
FROM MyTable_memop  
WHERE col1 = 'dn';  
```

Der Hashindex benötigt die `WHERE`-Klausel, um einen Gleichheitstest für die einzelnen Spalten im Schlüssel anzugeben. Ansonsten ist der Hashindex für den Abfrageoptimierer nicht sinnvoll.  
  
Genauso wenig ist der Indextyp nützlich, wenn die `WHERE`-Klausel nur die zweite Spalte im Indexschlüssel angibt.  

## <a name="summary-table-to-compare-index-use-scenarios"></a>Zusammenfassungstabelle für den Vergleich der Indexverwendungsszenarien  
  
Die folgende Tabelle enthält alle Vorgänge, die von den verschiedenen Indextypen unterstützt werden. *Ja* bedeutet, dass der Index die Anforderung effizient verarbeiten kann, und *Nein* bedeutet, dass der Index die Anforderung nicht effizient erfüllen kann. 
  
| Vorgang | Speicheroptimiert, <br/> hash | Speicheroptimiert, <br/> Nicht gruppiert | Datenträgerbasiert, <br/> (nicht) gruppiert |  
| :-------- | :--------------------------- | :----------------------------------- | :------------------------------------ |  
| Indexscan, alle Tabellenzeilen abrufen. | Ja | Ja | Ja |  
| Indexsuche nach Gleichheitsprädikaten (=). | Ja <br/> (Vollständiger Schlüssel ist erforderlich.) | Ja  | Ja |  
| Indexsuche nach Ungleichheits- und Bereichsprädikaten <br/> (>, <, <=, >=, `BETWEEN`). | Nein <br/> (Führt zu einem Indexscan.) | Ja<sup>1</sup> | Ja |  
| Abrufen von Zeilen in einer Sortierreihenfolge, die der Indexdefinition entspricht. | Nein | Ja | Ja |  
| Abrufen von Zeilen in einer Sortierreihenfolge, die der umgekehrten Indexdefinition entspricht. | Nein | Nein | Ja |  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<sup>1</sup> Für einen nicht gruppierten speicheroptimierten Index ist der vollständige Schlüssel nicht erforderlich, um eine Indexsuche auszuführen.  

## <a name="automatic-index-and-statistics-management"></a>Automatische Verwaltung von Index und Statistiken

Nutzen Sie Lösungen wie [Adaptive Index Defrag](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag), um die Indexdefragmentierung und das Aktualisieren der Statistiken für eine oder mehrere Datenbanken automatisch zu verwalten. Dieser Vorgang entscheidet unter anderem anhand des Fragmentierungsgrads automatisch, ob ein Index neu organisiert oder neu erstellt wird und aktualisiert Statistiken mit einem linearen Schwellenwert.

## <a name="see-also"></a><a name="Additional_Reading"></a> Weitere Ressourcen   
 [Handbuch zum SQL Server Indexentwurf](../../relational-databases/sql-server-index-design-guide.md)   
 [Hashindizes für speicheroptimierte Tabellen](../../relational-databases/sql-server-index-design-guide.md#hash_index)   
 [Nicht gruppierte Indizes für speicheroptimierte Tabellen](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index)    
 [Adaptive Indexdefragmentierung](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag)