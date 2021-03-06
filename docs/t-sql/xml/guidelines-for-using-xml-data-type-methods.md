---
description: Richtlinien zum Verwenden von Methoden des xml-Datentyps
title: Richtlinien zum Verwenden von Methoden des xml-Datentyps
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- xml data type [SQL Server], methods
- methods [XML in SQL Server]
ms.assetid: 1a483aa1-42de-4c88-a4b8-c518def3d496
author: MightyPen
ms.author: genemi
ms.openlocfilehash: adf76774e56841978d64661704f91cd6fd04d3fc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193133"
---
# <a name="guidelines-for-using-xml-data-type-methods"></a>Richtlinien zum Verwenden von Methoden des xml-Datentyps

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Thema werden Richtlinien zum Verwenden der **xml**-Datentypmethoden beschrieben.

## <a name="the-print-statement"></a>Die PRINT-Anweisung

Die Methoden des **xml**-Datentyps können nicht in der PRINT-Anweisung verwendet werden, wie das folgende Beispiel zeigt. Die Methoden des **xml**-Datentyps werden als Unterabfragen behandelt, und Unterabfragen sind in der PRINT-Anweisung nicht zulässig. Aus diesem Grund gibt das folgende Beispiel einen Fehler zurück:

```sql
DECLARE @x XML
SET @x = '<root>Hello</root>'
PRINT @x.value('/root[1]', 'varchar(20)') -- will not work because this is treated as a subquery (select top 1 col from table)
```

Eine mögliche Lösung besteht darin, zuerst das Ergebnis der **value()** -Methode einer Variablen vom **xml**-Typ zuzuweisen und dann diese Variable in der Abfrage anzugeben.

```sql
DECLARE @x XML
DECLARE @c VARCHAR(max)
SET @x = '<root>Hello</root>'
SET @c = @x.value('/root[1]', 'VARCHAR(11)')
PRINT @c
```

## <a name="the-group-by-clause"></a>Die GROUP BY-Klausel

Die Methoden des **xml**-Datentyps werden intern als Unterabfragen behandelt. Da GROUP BY einen Skalarwert erfordert und keine Aggregate oder Unterabfragen zulässt, können Sie die Methoden des **xml**-Datentyps nicht in der GROUP BY-Klausel angeben. Eine mögliche Lösung besteht im Aufrufen einer benutzerdefinierten Funktion, innerhalb derer XML-Methoden verwendet werden.

## <a name="reporting-errors"></a>Erstellen von Fehlerberichten

Sollte ein Fehler auftreten, lösen die Methoden des **xml**-Datentyps einen einzelnen Fehler im folgenden Format aus:

```
Msg errorNumber, Level levelNumber, State stateNumber:
XQuery [database.table.method]: description_of_error
```

Beispiel:

```
Msg 2396, Level 16, State 1:
XQuery [xmldb_test.xmlcol.query()]: Attribute may not appear outside of an element
```

## <a name="singleton-checks"></a>Singleton-Überprüfungen

Positionsschritte, Funktionsparameter und Operatoren, die Singleton-Elemente benötigen, geben einen Fehler zurück, wenn der Compiler nicht ermitteln kann, ob zur Laufzeit ein Singleton-Element sichergestellt ist. Dieses Problem tritt häufig bei nicht typisierten Daten auf. So erfordert z. B. die Suche eines Attributs ein übergeordnetes Singleton-Element. Dabei ist eine Ordinalzahl ausreichend, die einen einzelnen übergeordneten Knoten auswählt. Die Auswertung einer **node()** - **-value()** -Kombination zum Extrahieren von Attributwerten erfordert möglicherweise keine Angabe der Ordinalzahl. Dies ist im nächsten Beispiel dargestellt.

### <a name="example-known-singleton"></a>Beispiel: Bekanntes Singleton

In diesem Beispiel generiert die **nodes()** -Methode eine separate Zeile für jedes `<book>`-Element. Die **value()** -Methode, die in einem `<book>`-Knoten ausgewertet wird, extrahiert den Wert von `@genre` und stellt, da es sich um ein Attribut handelt, ein Singleton-Element dar.

```sql
SELECT nref.value('@genre', 'VARCHAR(max)') LastName
FROM T CROSS APPLY xCol.nodes('//book') AS R(nref)
```

Das XML-Schema wird zur Typprüfung von typisiertem XML verwendet. Wenn ein Knoten im XML-Schema als ein Singleton angegeben ist, verwendet der Compiler diese Informationen, und es tritt kein Fehler auf. Ansonsten ist eine Ordinalzahl erforderlich, die einen einzelnen Knoten auswählt. Insbesondere bei Verwendung einer descendant-or-self-Achse (//) wie in `/book//title` geht die Singleton-Kardinalitätsinferenz für das Element `<title>` verloren; dies gilt auch bei einer entsprechenden Angabe durch das XML-Schema. Deshalb sollten Sie stattdessen `(/book//title)[1]` schreiben.

Sie müssen sich des Unterschieds zwischen `//first-name[1]` und `(//first-name)[1]` für die Typüberprüfung bewusst sein. Das erstgenannte Element gibt eine Sequenz von `<first-name>`-Knoten zurück, in denen jeder Knoten den `<first-name>`-Knoten am äußersten linken Rand unter den gleichgeordneten Knoten darstellt. Das letztgenannte Element gibt den ersten `<first-name>`-Singleton-Knoten in der Dokumentreihenfolge der XML-Instanz zurück.

### <a name="example-using-value"></a>Beispiel: Verwenden von value()

Die folgende Abfrage einer nicht typisierten XML-Spalte führt zu einem statischen Kompilierungsfehler. Das liegt daran, dass **value()** einen Singleton-Knoten als erstes Argument erwartet, und der Compiler nicht feststellen kann, ob nur ein `<last-name>`-Knoten zur Laufzeit auftritt:

```sql
SELECT xCol.value('//author/last-name', 'NVARCHAR(50)') LastName
FROM T
```

Es folgt eine Projektmappe, die Sie in Betracht ziehen könnten:

```sql
SELECT xCol.value('//author/last-name[1]', 'NVARCHAR(50)') LastName
FROM T
```

Der Fehler wird auf diese Weise jedoch nicht behoben, da in jeder XML-Instanz mehrere `<author>`-Knoten vorhanden sein können. Die folgende umgeschriebene Version ist jedoch funktionsfähig:

```sql
SELECT xCol.value('(//author/last-name/text())[1]', 'NVARCHAR(50)') LastName
FROM T
```

Diese Abfrage gibt den Wert des ersten `<last-name>`-Elements in jeder XML-Instanz zurück.

## <a name="see-also"></a>Weitere Informationen

- [xml Data Type Methods (xml-Datentypmethoden)](../../t-sql/xml/xml-data-type-methods.md)
