---
title: Hintergrundinformationen zur Vorschauversion von Azure Synapse Pathway
description: Detaillierte technische Informationen darüber, wie Azure Synapse Pathway Ihren Code übersetzt
author: anshul82-ms
ms.author: anrampal.
ms.prod: sql
ms.technology: Azure Synapse Pathway
ms.topic: conceptual
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-concept
ms.openlocfilehash: dbd362e53b5bfcd916c53e90d6f66c8fb44f0374
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873080"
---
# <a name="azure-synapse-pathway-preview-behind-the-scenes"></a>Hintergrundinformationen zur Vorschauversion von Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Das Ziel von Azure Synapse Pathway ist der Erhalt der funktionalen Absicht des ursprünglichen Codes bei gleichzeitiger Optimierung für Synapse SQL. Die Übersetzung von SQL-Code aus einem Quellsystem für Azure Synapse SQL in Synapse Pathway besteht aus drei Phasen.

In jeder dieser Phasen wird das Wissen aus der Quelle einschließlich quellspezifischer Metadaten beibehalten und erweitert, um bei der Übersetzung die bestmögliche Qualität sicherzustellen.

 ![Azure Synapse Pathway](./media/technical-deep-dive/behind-the-scene.png)

## <a name="stage-1--lexing-and-parsing"></a>Phase 1: Lexing und Analyse

Die Analyse von SQL-Code ist ein Problem, für das es bereits viele Lösungen gibt. Es gibt zahlreiche kommerzielle und quelloffene Parser, die Unterstützung für den zugrunde liegenden Prozess der Aufteilung einer Quellanweisung in logische Token und Ausführung für eine Gruppe von Parserregeln zur Sicherstellung von Sprachkonsistenz bieten. 

Synapse Pathway definiert Quellgrammatiken, die es dem Tool ermöglichen, die SQL-Eingabe zu identifizieren und in einer erweiterten abstrakten Syntaxstruktur (Abstract Syntax Tree, AST) weiterzuverarbeiten. 

## <a name="stage-2---augmented-abstract-syntax-tree-ast"></a>Phase 2: Erweiterte abstrakte Syntaxstruktur (AST)

Synapse Pathway definiert eine allgemeine Darstellung aller Objekte in einer erweiterten abstrakten Syntaxstruktur (AST). Die Synapse Pathway-AST enthält zusätzliche Anweisungen oder fragmentierte Metadaten, die zum Treffen der richtigen Entscheidung bei der Übersetzung von Code zwischen zwei System verwendet werden.

Wenn nicht nur nachverfolgt wird, dass es sich bei einem Token um eine Funktion handelt, sondern auch die Typanforderungen des Quellsystems, können die Skriptgenerierungskomponenten intelligentere Entscheidungen hinsichtlich der Übersetzung für Synapse SQL treffen.

Beispielsweise wird die Quellfunktion für die absolute Funktion wie folgt definiert:

```sql  
ABS( float_expression ) 
```

Azure Synapse SQL definiert die absolute Funktion wie folgt:

```sql  
ABS ( numeric_expression )  
```

In diesem einfachen Fall versteht Synapse Pathway, dass es sich bei der Konvertierung von „float“ in „numeric“ in Synapse SQL um eine implizite [Konvertierung](../../t-sql/functions/cast-and-convert-transact-sql?view=azure-sqldw-latest#implicit-conversions) handelt und keine weitere Typumwandlung erforderlich ist. Die Codeübersetzung ist einfach, sauber und effektiv.

Das Beibehalten dieser Metainformationen zu den Quellanweisungen und Fragmenten ist hilfreich bei strukturellen Unterschieden zwischen Plattformen, z. B. bei Konvertierungen in Deaktivierungslogik für Suchbedingungsprädikate in WHERE-Klauseln.

## <a name="stage-3---syntax-generation"></a>Phase 3: Syntaxgenerierung

Die letzte Phase des Prozesses ist das Generieren von Syntax für Synapse SQL. Synapse Pathway schreibt basierend auf der aus den Quelldateien generierten AST-Struktur jedes DDL-Objekt in eine eigene Datei. Die Syntax-Generatoren verwenden für die Optimierung von Anweisungen detailliertes Wissen über die Zielplattform.

Ein gängiges Muster bei Szenarios, in denen Daten geladen werden, besteht beispielsweise darin, erst den gesamten Inhalt einer Stagingtabelle zu löschen und dann mit INSERT/SELECT die Daten aus einer anderen Stagingtabelle zu laden.

```sql  
DELETE staging.table1 ALL;
INSERT INTO staging.table1…
FROM staging.table2;
```

Synapse SQL verfügt über einen für dieses Szenario optimierten Pfad: [CREATE TABLE AS SELECT](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-develop-ctas). Bei der CTAS-Anweisung handelt es sich um einen batchbasierten, in minimalem Umfang protokollierten Vorgang mit hoher Leistung durch Nutzung der gesamten verfügbaren Computeinfrastruktur. Ohne diese Erkenntnisse zu Synapse SQL erzeugen Tools häufig TRUNCATE- und INSERT/SELECT-Anweisungen.

```sql  
TRUNCATE TABLE staging.table1;
INSERT INTO staging.table1…
FROM staging.table2;
```

Dies ist zwar nicht schlecht, aber der Code kann mit DROP TABLE und CTAS optimiert werden, um eine höhere Leistung zu erzielen.

```sql  
DROP TABLE staging.table1;
CREATE TABLE staging.table1
WITH
(
    -- Derived from the original table definition 
    DISTRIBUTION = HASH(column1),
    -- Derived from the original table definition
    CLUSTERED COLUMNSTORE INDEX
)
AS SELECT  * FROM staging.table2;
```

## <a name="next-steps"></a>Nächste Schritte

- [Azure Synapse Pathway herunterladen](synapse-pathway-download.md)
- [Häufig gestellte Fragen](pathway-faq.md)

