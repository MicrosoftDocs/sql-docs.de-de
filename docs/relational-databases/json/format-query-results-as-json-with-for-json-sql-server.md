---
description: Formatieren von Abfrageergebnisse als JSON mit FOR JSON (SQL Server)
title: Formatieren von Abfrageergebnissen als JSON mit FOR JSON
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON
- JSON, exporting
- exporting JSON
ms.assetid: 15b56365-58c2-496c-9d4b-aa2600eab09a
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 916ff3bf29640807314e139ceeaf82283133d73b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478131"
---
# <a name="format-query-results-as-json-with-for-json-sql-server"></a>Formatieren von Abfrageergebnisse als JSON mit FOR JSON (SQL Server)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

Formatieren Sie Abfrageergebnisse als JSON, oder exportieren Sie Daten aus SQL Server als JSON, indem Sie die **FOR JSON**-Klausel einer **SELECT**-Anweisung hinzufügen. Verwenden der **FOR JSON**-Klausel, um Clientanwendungen zu vereinfachen, indem Sie die Formatierung der JSON-Ausgabe von der App zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] delegieren. [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) ist der empfohlene Abfrage-Editor für JSON-Abfragen, da hierbei die JSON-Ergebnisse (wie in diesem Artikel gezeigt) automatisch formatiert werden, anstatt dass eine flache Zeichenfolge angezeigt wird.
  
 Bei Verwendung der **FOR JSON** -Klausel, können Sie die Struktur der JSON-Ausgabe explizit angeben, oder lassen Sie die Struktur der SELECT-Anweisung die Ausgabe bestimmen.  
  
-   Verwenden Sie **FOR JSON PATH**, um die vollständige Kontrolle über das Format der JSON-Ausgabe zu behalten. Sie können Wrapper-Objekte erstellen und komplexe Eigenschaften schachteln.  
  
-   Verwenden Sie **FOR JSON AUTO**, um die JSON-Ausgabe automatisch auf Grundlage der SELECT-Anweisung zu formatieren.  
  
Hier finden Sie ein Beispiel für eine **SELECT**-Anweisung mit der **FOR JSON**-Klausel und deren Ausgabe.
  
 ![FOR JSON](../../relational-databases/json/media/jsonslides2forjson.png)
  
## <a name="option-1---you-control-output-with-for-json-path"></a>Option 1: Sie können die Ausgabe mit FOR JSON PATH steuern
Im **PATH** -Modus können Sie die Punktsyntax verwenden, z.B. `'Item.UnitPrice'`, um geschachtelte Ausgaben zu formatieren.  

Hier ist eine Beispielabfrage, die den **PATH** -Modus mit der **FOR JSON** -Klausel verwendet. Im folgenden Beispiel wird auch die Option **ROOT** verwendet, um ein benanntes Stammelement anzugeben. 
  
 ![Flussdiagramm für FOR JSON-Ausgabe](../../relational-databases/json/media/forjson-example1.png) 

### <a name="more-info-about-for-json-path"></a>Weitere Informationen zu FOR JSON PATH
Ausführlichere Informationen und Beispiele finden Sie unter [Format Nested JSON Output with PATH Mode (SQL Server) (Formatieren einer geschachtelten JSON-Ausgabe im PATH-Modus (SQL Server))](../../relational-databases/json/format-nested-json-output-with-path-mode-sql-server.md).

Weitere Informationen zu Syntax und Verwendung finden Sie unter [FOR-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-for-clause-transact-sql.md).  

## <a name="option-2---select-statement-controls-output-with-for-json-auto"></a>Option 2: Die SELECT-Anweisung steuert die Ausgabe mit FOR JSON AUTO
Im **AUTO** -Modus bestimmt die Struktur der SELECT-Anweisung das Format der JSON-Ausgabe.

**Nullwerte** sind standardmäßig nicht in der Ausgabe enthalten. Sie können **INCLUDE_NULL_VALUES** dazu verwenden, dieses Verhalten zu ändern.  

Hier ist eine Beispielabfrage, die den **AUTO** -Modus mit der **FOR JSON** -Klausel verwendet.

```sql  
SELECT name, surname  
FROM emp  
FOR JSON AUTO;
```  

Hier sehen Sie den zurückgegebenen JSON-Code.

```json  
[{
    "name": "John"
}, {
    "name": "Jane",
    "surname": "Doe"
}]
```

### <a name="2b---example-with-join-and-null"></a>2.b: Beispiel mit JOIN und NULL

Im folgenden Beispiel von `SELECT...FOR JSON AUTO` wird gezeigt, wie die JSON-Ergebnisse im Falle einer 1:Viele-Beziehung zwischen Daten aus mit `JOIN` verknüpften Tabellen aussehen.

Das Fehlen des NULL-Werts aus dem zurückgegebenen JSON-Code wird ebenfalls veranschaulicht. Sie können dieses Standardverhalten jedoch mithilfe des Schlüsselworts `INCLUDE_NULL_VALUES` in der `FOR`-Klausel außer Kraft setzen.

```sql
go

DROP TABLE IF EXISTS #tabStudent;
DROP TABLE IF EXISTS #tabClass;

go

CREATE TABLE #tabClass
(
   ClassGuid   uniqueIdentifier  not null  default newid(),
   ClassName   nvarchar(32)      not null
);

CREATE TABLE #tabStudent
(
   StudentGuid   uniqueIdentifier  not null  default newid(),
   StudentName   nvarchar(32)      not null,
   ClassGuid     uniqueIdentifier      null   -- Foreign key.
);

go

INSERT INTO #tabClass
      (ClassGuid, ClassName)
   VALUES
      ('DE807673-ECFC-4850-930D-A86F921DE438', 'Algebra Math'),
      ('C55C6819-E744-4797-AC56-FF8A729A7F5C', 'Calculus Math'),
      ('98509D36-A2C8-4A65-A310-E744F5621C83', 'Art Painting')
;

INSERT INTO #tabStudent
      (StudentName, ClassGuid)
   VALUES
      ('Alice Apple', 'DE807673-ECFC-4850-930D-A86F921DE438'),
      ('Alice Apple', 'C55C6819-E744-4797-AC56-FF8A729A7F5C'),
      ('Betty Boot' , 'C55C6819-E744-4797-AC56-FF8A729A7F5C'),
      ('Betty Boot' , '98509D36-A2C8-4A65-A310-E744F5621C83'),
      ('Carla Cap'  , null)
;

go

SELECT
      c.ClassName,
      s.StudentName
   from
                       #tabClass   as c
      RIGHT OUTER JOIN #tabStudent as s ON s.ClassGuid = c.ClassGuid
   --where
   --   c.ClassName LIKE '%Math%'
   order by
      c.ClassName,
      s.StudentName
   FOR
      JSON AUTO
      --, INCLUDE_NULL_VALUES
;

go

DROP TABLE IF EXISTS #tabStudent;
DROP TABLE IF EXISTS #tabClass;

go
```

Als Nächstes folgt der JSON-Code, der von der vorherigen SELECT-Anweisung ausgegeben wird.

```json
JSON_F52E2B61-18A1-11d1-B105-00805F49916B

[
   {"s":[{"StudentName":"Carla Cap"}]},
   {"ClassName":"Algebra Math","s":[{"StudentName":"Alice Apple"}]},
   {"ClassName":"Art Painting","s":[{"StudentName":"Betty Boot"}]},
   {"ClassName":"Calculus Math","s":[{"StudentName":"Alice Apple"},{"StudentName":"Betty Boot"}]}
]
```

### <a name="more-info-about-for-json-auto"></a>Weitere Informationen zu FOR JSON AUTO

Ausführlichere Informationen und Beispiele finden Sie unter [Format JSON Output Automatically with AUTO Mode (SQL Server) (Automatisches Formatieren von JSON-Ausgaben (SQL Server))](../../relational-databases/json/format-json-output-automatically-with-auto-mode-sql-server.md).

Weitere Informationen zu Syntax und Verwendung finden Sie unter [FOR-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-for-clause-transact-sql.md).  
  
## <a name="control-other-json-output-options"></a>Steuern der Ausgabe anderer JSON-Ausgabeoptionen  
Steuern Sie die Ausgabe der **FOR JSON** -Klausel, indem Sie die folgenden zusätzlichen Optionen verwenden.  
  
-   **ROOT** Geben Sie die Option **ROOT** an, um der JSON-Ausgabe ein einzelnes Element der obersten Ebene hinzuzufügen. Wenn Sie diese Option nicht angeben, besitzt die JSON-Ausgabe kein Stammelement. Weitere Informationen finden Sie unter [Add a Root Node to JSON Output with the ROOT Option &#40;SQL Server&#41; (Hinzufügen eines Stammknotens zur JSON-Ausgabe mithilfe der ROOT-Option (SQL Server))](../../relational-databases/json/add-a-root-node-to-json-output-with-the-root-option-sql-server.md).  
  
-   **INCLUDE_NULL_VALUES** Geben Sie die Option **INCLUDE_NULL_VALUES** an, um NULL-Werte in die JSON-Ausgabe einzuschließen. Wenn Sie diese Option nicht angeben, enthält die Ausgabe in den Abfrageergebnissen keine JSON-Eigenschaften für NULL-Werte. Weitere Informationen finden Sie unter[Include Null Values in JSON Output with the INCLUDE_NULL_VALUES Option (SQL Server) (Einschließen von Nullwerten in die JSON-Ausgabe mit der Option INCLUDE_NULL_VALUES (SQL Server))](../../relational-databases/json/include-null-values-in-json-include-null-values-option.md).   

-   **WITHOUT_ARRAY_WRAPPER** Geben Sie zum standardmäßigen Entfernen der rechteckigen Klammern, die die JSON-Ausgabe der **FOR JSON** -Klausel umgeben, die Option **WITHOUT_ARRAY_WRAPPER** an. Verwenden Sie diese Option, um ein einzelnes JSON-Objekt als Ausgabe eines einzeiligen Ergebnisses zu generieren. Wenn Sie diese Option nicht angeben, wird die JSON-Ausgabe als Array formatiert, das in eckige Klammern eingeschlossen ist. Weitere Informationen finden Sie unter [Remove Square Brackets from JSON Output with the WITHOUT_ARRAY_WRAPPER Option &#40;SQL Server&#41; (Entfernen von rechteckigen Klammern von JSON-Ausgabe mit der Option WITHOUT_ARRAY_WRAPPER (SQL Server))](../../relational-databases/json/remove-square-brackets-from-json-without-array-wrapper-option.md). 
   
## <a name="output-of-the-for-json-clause"></a>Ausgabe der FOR JSON-Klausel  
Die Ausgabe der **FOR JSON**-Klausel besitzt die folgenden Eigenschaften:  
  
1.  Das Resultset enthält eine einzelne Spalte.
    -   Ein kleines Resultset kann eine einzelne Zeile enthalten.
    -   Ein großes Resultset teilt den langen JSON-String über mehrere Zeilen auf.
        -   Standardmäßig verkettet SQL Server Management Studio (SSMS) die Ergebnisse in einer einzelnen Zeile, wenn die Ausgabeeinstellung **Results to Grid (Ergebnisse im Raster)** ist. Die SSMS-Statusleiste zeigt die tatsächliche Zeilenanzahl an.
        -   Möglicherweise erfordern andere Clientanwendungen Code, um längere Ergebnislisten neu zu einer einzelnen gültigen JSON-Zeichenfolge zu kombinieren, indem die Inhalte mehrerer Zeilen verkettet werden. Ein Beispiel für diesen Code in einer C#-Anwendung finden Sie unter [Use FOR JSON output in a C# client app (Verwenden der FOR JSON-Ausgabe in einer C#-Client-App)](../../relational-databases/json/use-for-json-output-in-sql-server-and-in-client-apps-sql-server.md#use-for-json-output-in-a-c-client-app).
  
     ![Beispiel einer FOR JSON-Ausgabe](../../relational-databases/json/media/forjson-example2.png)  
  
2.  Die Ergebnisse werden wie ein JSON-Objekt-Array formatiert.  
  
    -   Die Anzahl der Elemente im JSON-Array entspricht der Anzahl der Zeilen in den Ergebnissen der SELECT-Anweisung (bevor die FOR JSON-Klausel angewendet wird). 
  
    -   Jede Zeile in den Ergebnissen der SELECT-Anweisung (bevor die FOR JSON-Klausel angewendet wird) wird ein separates JSON-Objekt im Array.  
  
    -   Jede Spalte in den Ergebnissen der SELECT-Anweisung (bevor die FOR JSON-Klausel angewendet wird) wird zu einer Eigenschaft des JSON-Objekts.  
  
3.  Sowohl die Namen der Spalten als auch deren Werte werden entsprechend der JSON-Syntax geschützt. Weitere Informationen finden Sie unter [How FOR JSON escapes special characters and control characters &#40;SQL Server&#41; (So maskiert FOR JSON Sonderzeichen und Steuerzeichen (SQL Server))](../../relational-databases/json/how-for-json-escapes-special-characters-and-control-characters-sql-server.md).

### <a name="example"></a>Beispiel
Hier ist ein Beispiel, das veranschaulicht, wie die **FOR JSON**-Klausel die JSON-Ausgabe formatiert.  
  
**Abfrageergebnisse**  

|A|B|C|D|
|-|-|-|-|
|10|11|12|X|  
|20|21|22|J|  
|30|31|32|Z|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |

 **JSON-Ausgabe**  
  
```json  
[{
    "A": 10,
    "B": 11,
    "C": 12,
    "D": "X"
}, {
    "A": 20,
    "B": 21,
    "C": 22,
    "D": "Y"
}, {
    "A": 30,
    "B": 31,
    "C": 32,
    "D": "Z"
}] 
```  

 Weitere Informationen darüber, was Sie in der Ausgabe der **FOR JSON**-Klausel sehen, finden Sie in den folgenden Themen.  

-   [How FOR JSON converts SQL Server data types to JSON data types &#40;SQL Server&#41; (So konvertiert FOR JSON SQL Server-Datentypen in JSON-Datentypen (SQL Server))](../../relational-databases/json/how-for-json-converts-sql-server-data-types-to-json-data-types-sql-server.md)  
    Die **FOR JSON** -Klausel verwendet die in diesem Thema beschriebenen Regeln, um SQL-Datentypen in JSON-Typen in der JSON-Ausgabe zu konvertieren.  

-   [How FOR JSON escapes special characters and control characters &#40;SQL Server&#41; (So maskiert FOR JSON Sonderzeichen und Steuerzeichen (SQL Server))](../../relational-databases/json/how-for-json-escapes-special-characters-and-control-characters-sql-server.md)  
    Die **FOR JSON** -Klausel maskiert Sonderzeichen und stellt die Steuerzeichen in der JSON-Ausgabe dar, wie in diesem Thema beschrieben.  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Weitere Informationen zu JSON in SQL Server und Azure SQL-Datenbank  
  
### <a name="microsoft-videos"></a>Microsoft-Videos

Eine visuelle Einführung in die JSON-Unterstützung, die in SQL Server und Azure SQL-Datenbank integriert ist, finden Sie in den folgenden Videos:

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a>Weitere Informationen  
 [FOR-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-for-clause-transact-sql.md)   
 [Use FOR JSON output in SQL Server and in client apps (SQL Server) (Verwenden der FOR JSON-Ausgabe in SQL Server und Client-Apps (SQL Server))](../../relational-databases/json/use-for-json-output-in-sql-server-and-in-client-apps-sql-server.md)  
  
  
