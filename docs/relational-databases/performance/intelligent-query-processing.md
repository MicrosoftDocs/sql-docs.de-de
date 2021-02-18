---
title: Intelligente Abfrageverarbeitung
description: Features zur intelligenten Abfrageverarbeitung, die die Abfrageleistung in SQL Server und in Azure SQL-Datenbank verbessern
ms.custom: seo-dt-2019
ms.date: 11/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords: ''
author: joesackmsft
ms.author: josack
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 40a2ba2ff9e3955c09a8d8529a67ec3275e17222
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338965"
---
# <a name="intelligent-query-processing-in-sql-databases"></a>Intelligente Abfrageverarbeitung in SQL-Datenbanken

[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

Die Featurefamilie „Intelligente Abfrageverarbeitung“ (Intelligent Query Processing, IQP) umfasst Features mit weitreichenden Auswirkungen, die die Leistung vorhandener Workloads mit minimalem Implementierungsaufwand verbessern. 

![Intelligente Abfrageverarbeitung](./media/iqp-feature-family.png)

In diesem 6-minütigen Video erhalten Sie einen Überblick über die intelligente Abfrageverarbeitung:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Overview-Intelligent-Query-processing-in-SQL-Server-2019/player?WT.mc_id=dataexposed-c9-niner]


Sie können Workloads automatisch für die intelligente Abfrageverarbeitung anpassen, indem Sie den geeigneten Datenbank-Kompatibilitätsgrad für die Datenbank aktivieren. Diesen können Sie mit [!INCLUDE[tsql](../../includes/tsql-md.md)] festlegen. Beispiel:  

```sql
ALTER DATABASE [WideWorldImportersDW] SET COMPATIBILITY_LEVEL = 150;
```

In der folgenden Tabelle sind Details zu allen Features der intelligenten Abfrageverarbeitung dargestellt, sowie deren jeweiligen Anforderungen für den Datenbank-Kompatibilitätsgrad.

| **IQP-Feature** | **Unterstützt in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]** | **Unterstützt in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** |**Beschreibung** |
| ---------------- | ------- | ------- | ---------------- |
| [Adaptive Joins (Batchmodus)](#batch-mode-adaptive-joins) | Ja, unter Kompatibilitätsgrad 140| Ja, ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] unter Kompatibilitätsgrad 140|Adaptive Joins wählen je nach tatsächlichen Eingabezeilen während der Laufzeit dynamisch einen Jointyp aus.|
| [Approximate Count Distinct](#approximate-query-processing) | Ja| Ja, ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]|Stellt die geschätzte Abfrageverarbeitung für Big Data-Szenarios mit dem Vorteil einer hohen Leistung und einem niedrigen Speicherbedarf bereit. |
| [Batchmodus bei Rowstore](#batch-mode-on-rowstore) | Ja, unter Kompatibilitätsgrad 150| Ja, ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] unter Kompatibilitätsgrad 150|Stellt den Batchmodus für CPU-gebundene relationale Data Warehouse-Workloads bereit, ohne Columnstore-Indizes zu benötigen.  | 
| [Verschachtelte Ausführung](#interleaved-execution-for-mstvfs) | Ja, unter Kompatibilitätsgrad 140| Ja, ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] unter Kompatibilitätsgrad 140|Verwendet die tatsächliche Kardinalität der Tabellenwertfunktion mit mehreren Anweisungen, die bei der ersten Kompilierung aufgetreten ist, anstatt einer festgelegten Schätzung.|
| [Feedback zur Speicherzuweisung (Batchmodus)](#batch-mode-memory-grant-feedback) | Ja, unter Kompatibilitätsgrad 140| Ja, ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] unter Kompatibilitätsgrad 140|Wenn es für eine Abfrage im Batchmodus Operationen gibt, die sich auf den Datenträger auswirken, wird für anschließende Ausführungen mehr Speicher hinzugefügt. Wenn eine Abfrage unnötigerweise mehr als die Hälfte des zugewiesenen Speichers belegt, wird die Speicherzuweisungsseite für anschließende Ausführungen reduziert.|
| [Feedback zur Speicherzuweisung (Zeilenmodus)](#row-mode-memory-grant-feedback) | Ja, unter Kompatibilitätsgrad 150| Ja, ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] unter Kompatibilitätsgrad 150|Wenn es für eine Abfrage im Zeilenmodus Operationen gibt, die sich auf den Datenträger auswirken, wird für anschließende Ausführungen mehr Speicher hinzugefügt. Wenn eine Abfrage unnötigerweise mehr als die Hälfte des zugewiesenen Speichers belegt, wird die Speicherzuweisungsseite für anschließende Ausführungen reduziert.|
| [Inlining benutzerdefinierter Skalarfunktionen](#scalar-udf-inlining) | Nein | Ja, ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] unter Kompatibilitätsgrad 150|Benutzerdefinierte Skalarfunktionen werden in äquivalente relationale Ausdrücke transformiert, für die „Inlining“ in die aufrufende Abfrage ausgeführt wird, was häufig zu erheblichen Leistungssteigerungen führt.|
| [Verzögerte Kompilierung von Tabellenvariablen](#table-variable-deferred-compilation) | Ja, unter Kompatibilitätsgrad 150| Ja, ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] unter Kompatibilitätsgrad 150|Verwendet die tatsächliche Kardinalität der Tabellenvariable, die bei der ersten Kompilierung aufgetreten ist, anstatt einer festgelegten Schätzung.|

## <a name="batch-mode-adaptive-joins"></a>Adaptive Joins im Batchmodus
Mit dem Feature „Adaptive Joins im Batchmodus“ wird es ermöglicht, die Wahl der Join-Methode ([Hashjoin oder Join geschachtelter Schleifen](../../relational-databases/performance/joins.md)) auf den Zeitpunkt **nach** der Überprüfung der ersten Eingabe zu verzögern, indem ein einzelner zwischengespeicherter Plan verwendet wird. Der Operator für adaptive Joins definiert einen Schwellenwert, der bestimmt, wann zu einem Plan geschachtelter Schleifen gewechselt wird. Daher kann Ihr Plan während der Ausführung dynamisch zu einer passenderen Joinstrategie wechseln.

Weitere Informationen, z. B. zum Deaktivieren adaptiver Joins ohne Änderung des Kompatibilitätsgrads, finden Sie unter [Grundlegendes zu adaptiven Joins](../../relational-databases/performance/joins.md#adaptive).

## <a name="batch-mode-memory-grant-feedback"></a>Feedback zur Speicherzuweisung im Batchmodus
Der Plan nach der Ausführung einer Abfrage in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] enthält den für die Ausführung mindestens erforderlichen Speicherplatz und die ideale Speicherzuweisungsgröße, sodass alle Zeilen in den Speicher passen. Es gibt Leistungseinbußen, wenn die Speicherzuweisungsgrößen falsch sind. Zu große Zuweisungen führen zu verschwendetem Speicherplatz und geringerer Parallelität. Nicht ausreichende Speicherzuweisungen führen zu teuren Überläufen auf den Datenträger. Für wiederholte Workloads berechnet das Feedback zur Speicherzuweisung im Batchmodus den tatsächlich erforderlichen Speicherplatz für eine Abfrage neu und aktualisiert anschließend den Zuweisungswert des zwischengespeicherten Plans. Wenn eine identische Abfrageanweisung ausgeführt wird, verwendet die Abfrage die angepasste Speicherzuweisungsgröße. Dadurch werden zu hohe Speicherzuweisungen verringert, die die Parallelität beeinträchtigen, und Probleme bei zu gering geschätzten Speicherzuweisungen behoben, die teuren Überläufe auf den Datenträger verursachen.
Der folgende Graph veranschaulicht ein Beispiel für den Gebrauch des Feedbacks zur adaptiven Speicherzuweisung im Batchmodus. Die erste Ausführung der Abfrage hat aufgrund von einer hoher Zahl von Überlaufen **88 Sekunden** in Anspruch genommen:   

```sql
DECLARE @EndTime datetime = '2016-09-22 00:00:00.000';
DECLARE @StartTime datetime = '2016-09-15 00:00:00.000';
SELECT TOP 10 hash_unique_bigint_id
FROM dbo.TelemetryDS
WHERE Timestamp BETWEEN @StartTime and @EndTime
GROUP BY hash_unique_bigint_id
ORDER BY MAX(max_elapsed_time_microsec) DESC;
```

![Hohe Zahl von Überläufen](./media/2_AQPGraphHighSpills.png)

Wenn das Feedback zur Speicherzuweisung aktiviert ist, dauert die zweite Ausführung **1 Sekunde** (vorher 88 Sekunden), Überläufe treten nicht mehr auf, und die Zuweisung ist höher: 

![Keine Überläufe](./media/3_AQPGraphNoSpills.png)

### <a name="memory-grant-feedback-sizing"></a>Größenanpassung des Feedbacks zur Speicherzuweisung im Batchmodus
Wenn bei einer Bedingung mit einer zu großen Speicherzuweisung der zugewiesene Speicher mehr als doppelt so groß ist als der tatsächlich verwendete Speicher, berechnet das Feedback zur Speicherzuweisung diese neu und aktualisiert den zwischengespeicherten Plan. Pläne mit Speicherzuweisungen unter 1 MB werden bei Überschreitungen nicht neu berechnet.
Bei einer Bedingung mit zu geringen Speicherzuweisungen, die bei Batchmodusoperatoren zu einem Überlauf auf einen Datenträger führen, löst das Feedback zur Speicherzuweisung eine Neuberechnung der Speicherzuweisung aus. Überlaufereignisse werden an das Feedback zur Speicherzuweisung gemeldet und können als XEvent-Ereignis *spilling_report_to_memory_grant_feedback* angegeben werden. Dieses Ereignis gibt die Knoten-ID aus dem Plan und der Größe der übergelaufenen Daten dieses Knotens zurück.

### <a name="memory-grant-feedback-and-parameter-sensitive-scenarios"></a>Feedback zur Speicherzuweisung und parameterempfindliche Szenarios
Verschiedene Parameterwerte können auch unterschiedliche Abfragepläne erfordern, um optimale Ergebnisse zu bieten. Diese Art von Abfrage wird als „parameterempfindlich“ bezeichnet. Bei parameterempfindlichen Plänen deaktiviert sich das Feedback zur Speicherzuweisung in Abfragen selbst, wenn es nicht stabile Speicheranforderungen aufweist. Der Plan wird nach mehreren wiederholten Abfrageausführungen deaktiviert. Dies können Sie beobachten, indem Sie das XEvent *memory_grant_feedback_loop_disabled* überwachen. Weitere Informationen zur Parameterermittlung und -empfindlichkeit finden Sie im [Leitfaden zur Architektur der Abfrageverarbeitung](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing).

### <a name="memory-grant-feedback-caching"></a>Caching des Feedbacks zur Speicherzuweisung
Das Feedback kann für eine einzelne Ausführung in einem zwischengespeicherten Plan gespeichert werden. Allerdings sind es aufeinanderfolgende Ausführungen dieser Anweisung, die von Anpassungen des Feedbacks zur Speicherzuweisung profitieren. Diese Funktion wird bei wiederholten Ausführungen von Anweisungen angewendet. Das Feedback zur Speicherzuweisung ändert nur den zwischengespeicherten Plan. Änderungen werden aktuell nicht im Abfragespeicher erfasst.
Das Feedback wird nicht übernommen, wenn der Plan aus dem Cache entfernt wird. Das Feedback geht ebenso verloren, wenn es zu einem Failover kommt. Eine Anweisung mit `OPTION (RECOMPILE)` erstellt einen neuen Plan und speichert diesen nicht zwischen. Da er nicht zwischengespeichert wird, wird auch kein Feedback zur Speicherzuweisung erzeugt und für diese Kompilierung und Ausführung auch nicht gespeichert. Wenn allerdings eine äquivalente Anweisung (d.h. eine Anweisung mit dem gleichen Abfragehash), die **nicht**`OPTION (RECOMPILE)` verwendet hat, zwischengespeichert und dann erneut ausgeführt wird, kann die darauffolgende Anweisung vom Feedback zur Speicherzuweisung profitieren.

### <a name="tracking-memory-grant-feedback-activity"></a>Nachverfolgen der Aktivität des Feedbacks zur Speicherzuweisung
Sie können Ereignisse des Feedbacks zur Speicherzuweisung mit dem XEvent-Ereignis *memory_grant_updated_by_feedback* nachverfolgen. Dieses Ereignis verfolgt den Verlauf der Ausführungsanzahl, die Zahl von Aktualisierungen des Plans durch das Feedback zur Speicherzuweisung und die optimale zusätzliche Speicherzuweisung vor der Anpassung und nach der Anpassung des zwischengespeicherten Plans durch das Feedback zur Speicherzuweisung.

### <a name="memory-grant-feedback-resource-governor-and-query-hints"></a>Feedback zur Speicherzuweisung, Ressourcenkontrolle und Abfragehinweise
Der tatsächlich zugewiesene Speicher berücksichtigt die Abfragespeichereinschränkung, die von der Ressourcenkontrolle oder dem Abfragehinweis bestimmt wird.

### <a name="disabling-batch-mode-memory-grant-feedback-without-changing-the-compatibility-level"></a>Deaktivieren des Feedbacks zur Speicherzuweisung im Batchmodus ohne Änderung des Kompatibilitätsgrads
Das Feedback zur Speicherzuweisung kann im Datenbank- oder Anweisungsbereich deaktiviert werden, während der Datenbankkompatibilitätsgrad weiterhin bei 140 und höher bleibt. Um das Feedback zur Speicherzuweisung im Batchmodus für alle Abfrageausführungen, die aus der Datenbank stammen, zu deaktivieren, führen Sie die folgende Anweisung im Kontext der betroffenen Datenbank aus:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK = ON;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_MEMORY_GRANT_FEEDBACK = OFF;
```

Ist diese Einstellung aktiviert, wird sie in [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) als aktiviert aufgeführt.

Um das Feedback zur Speicherzuweisung im Batchmodus für alle Abfrageausführungen, die aus der Datenbank stammen, wieder zu aktivieren, führen Sie die folgende Anweisung im Kontext der betroffenen Datenbank aus:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK = OFF;

-- Azure SQL Database, SQL Server 2019 and higher
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_MEMORY_GRANT_FEEDBACK = ON;
```

Sie können das Feedback zur Speicherzuweisung im Batchmodus auch für eine bestimmte Abfrage deaktivieren, indem Sie `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK` als [USE HINT-Abfragehinweis](../../t-sql/queries/hints-transact-sql-query.md#use_hint) festlegen. Beispiel:

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (USE HINT ('DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK')); 
```

Ein USE HINT-Abfragehinweis hat Vorrang vor einer datenbankweit gültigen Konfiguration oder einer Ablaufverfolgungsflageinstellung.

## <a name="row-mode-memory-grant-feedback"></a>Feedback zur Speicherzuweisung im Zeilenmodus

**Anwendungsbereich:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Das Feedback zur Speicherzuweisung im Zeilenmodus erweitert die Feedbackfunktion zur Speicherzuweisung im Batchmodus, indem die Größe der Speicherzuweisung sowohl für Batch- als auch für Zeilenmodusoperatoren angepasst wird.  

Um Feedback zur Speicherzuweisung im Zeilenmodus in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] zu aktivieren, aktivieren Sie den Datenbank-Kompatibilitätsgrad 150 für die Datenbank, mit der Sie beim Ausführen der Abfrage verbunden sind.

Die Feedbackaktivität zur Speicherzuweisung im Zeilenmodus wird über das XEvent-Ereignis **memory_grant_updated_by_feedback** angezeigt. 

Seitdem Feedback zur Speicherzuweisung im Zeilenmodus verfügbar ist, werden zwei neue Abfrageplanattribute für die tatsächlichen Pläne nach der Ausführung angezeigt: ***IsMemoryGrantFeedbackAdjusted** _ and _*_LastRequestedMemory_*_. Diese werden dem XML-Element des _MemoryGrantInfo*-Abfrageplans hinzugefügt. 

*LastRequestedMemory* zeigt den zugewiesenen Speicher in KB von der vorherigen Abfrageausführung an. Mit dem Attribut *IsMemoryGrantFeedbackAdjusted* können Sie den Feedbackstatus einer Speicherzuweisung für die Anweisung in einem Abfrageausführungsplan überprüfen. Folgende Werte werden in diesem Attribut angezeigt:

| Wert IsMemoryGrantFeedbackAdjusted | BESCHREIBUNG |
|---|---|
| No: First Execution | Das Feedback zur Speicherzuweisung passt den Speicher für die erste Kompilierung und die zugeordnete Ausführung nicht an.  |
| No: Accurate Grant | Wenn es keinen Überlauf auf dem Datenträger gibt und die Anweisung mindestens 50 % des zugewiesenen Speichers nutzt, wird Feedback zur Speicherzuweisung nicht ausgelöst. |
| No: Feedback disabled | Wenn das Feedback zur Speicherzuweisung kontinuierlich ausgelöst wird und zwischen Speichererhöhung und -verkleinerung schwankt, deaktivieren Sie das Feedback zur Speicherzuweisung für die Anweisung. |
| Yes: Adjusting | Das Feedback zur Speicherzuweisung wurde angewendet und kann für die nächste Ausführung weiter angepasst werden. |
| Yes: Stable | Das Feedback zur Speicherzuweisung wurde angewendet und der zugewiesene Speicher ist jetzt stabil. Das bedeutet: Der für die vorherige Ausführung zuletzt zugewiesene Speicher entspricht dem für die aktuelle Ausführung. |

### <a name="disabling-row-mode-memory-grant-feedback-without-changing-the-compatibility-level"></a>Deaktivieren des Feedbacks zur Speicherzuweisung im Zeilenmodus ohne Änderung des Kompatibilitätsgrads
Das Feedback zur Speicherzuweisung im Zeilenmodus kann im Datenbank- oder Anweisungsbereich deaktiviert werden, während der Datenbankkompatibilitätsgrad weiterhin bei 150 und höher bleibt. Um das Feedback zur Speicherzuweisung im Zeilenmodus für alle Abfrageausführungen, die aus der Datenbank stammen, zu deaktivieren, führen Sie die folgende Anweisung im Kontext der betroffenen Datenbank aus:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ROW_MODE_MEMORY_GRANT_FEEDBACK = OFF;
```

Um das Feedback zur Speicherzuweisung im Zeilenmodus für alle Abfrageausführungen, die aus der Datenbank stammen, erneut zu aktivieren, führen Sie die folgende Anweisung im Kontext der betroffenen Datenbank aus:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ROW_MODE_MEMORY_GRANT_FEEDBACK = ON;
```

Sie können das Feedback zur Speicherzuweisung im Zeilenmodus auch für eine bestimmte Abfrage deaktivieren, indem Sie `DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK` als [USE HINT-Abfragehinweis](../../t-sql/queries/hints-transact-sql-query.md#use_hint) festlegen. Beispiel:

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (USE HINT ('DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK')); 
```

Ein USE HINT-Abfragehinweis hat Vorrang vor einer datenbankweit gültigen Konfiguration oder einer Ablaufverfolgungsflageinstellung.

## <a name="interleaved-execution-for-mstvfs"></a>Verschachtelte Ausführung für MSTVFs
Bei der verschachtelten Ausführung verwenden Sie die tatsächliche Zeilenanzahl aus der Funktion, um besser informierte Entscheidungen zum Downstream-Abfrageplan zu treffen. Weitere Informationen zu Tabellenwertfunktionen mit mehreren Anweisungen (MSTVFs) finden Sie unter [Tabellenwertfunktionen](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF).

Eine verschachtelte Ausführung ändert die unidirektionale Grenze zwischen der Optimierungs- und der Ausführungsphase für eine Ausführung mit einer Abfrage. Zudem können Pläne damit auf Grundlage der überarbeiteten Kardinalitätsschätzungen angepasst werden. Wenn Sie bei der Optimierung auf einen möglichen Kandidaten für eine verschachtelte Ausführung (bei der es sich aktuell um **Tabellenfunktionen mit mehreren Anweisungen (MSTVFs)** handelt) stoßen, wird die Optimierung unterbrochen, die entsprechende Unterstruktur ausgeführt, die genauen Kardinalitätsschätzungen erfasst, und anschließend wird die Optimierung für Downstreamvorgänge wiederaufgenommen.   

Die festgelegte Kardinalitätsschätzung von Tabellenwertfunktionen mit mehreren Anweisungen beträgt ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] „100“ und in früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] „1“. Die verschachtelte Ausführung löst Probleme bei der Leistung von Workloads, die auf die festgelegten Kardinalitätsschätzungen von MSTVFs zurückzuführen sind. Weitere Informationen zu MSTVFs finden Sie unter [Erstellen benutzerdefinierter Funktionen (Datenbank-Engine)](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF).

Die folgende Abbildung zeigt die Ausgabe einer [Liveabfragestatistik](../../relational-databases/performance/live-query-statistics.md), eine Teilmenge eines allgemeinen Ausführungsplans, der die Auswirkung festgelegter Kardinalitätsschätzungen von MSTVFs veranschaulicht. Sie sehen den tatsächlichen Zeilenfluss gegenüber dem geschätzten. Es gibt drei erwähnenswerte Bereiche des Plans (von rechts nach links):
1. Der MSTVF-Table Scan hat eine festgelegte Schätzung von 100 Zeilen. In diesem Beispiel gibt es allerdings 527.597 Zeilen, die den MSTVF Table Scan durchlaufen. Dies ist an der Liveabfragestatistik *527597 von 100* der tatsächlichen Elemente von geschätzten Elemente zu erkennen – die festgelegte Schätzung liegt also deutlich zu weit unten.
1. Für den Nested Loop-Vorgang wird davon ausgegangen, dass nur 100 Zeilen von der äußeren Seite des Joins zurückgegebene werden. Aufgrund der hohen Zahl an tatsächlich von MSTVF zurückgegebenen Zeilen sollten Sie sich aber für einen komplett anderen Joinalgorithmus entscheiden.
1. Beachten Sie, dass beim Hash Match-Vorgang ein kleines Warnsymbol angezeigt wird, das in diesem Fall darauf hinweist, dass es einen Überlauf auf den Datenträger hab.

![Zeilenfluss vs. geschätzte Zeilen](./media/7_AQPFlowThreeAreas.png)

Vergleichen Sie den vorherigen Plan mit dem tatsächlich generierten Plan mit aktivierter verschachtelter Ausführung:

![Verschachtelter Plan](./media/8_AQPInterleavedEnabledPlan.png)

1. Beachten Sie, dass der MSTVF-Table Scan jetzt die genaue Kardinalitätsschätzung widerspiegelt. Beachten Sie auch die Neuanordnung des Table Scans und der anderen Vorgänge.
1. Bei Joinalgorithmen haben Sie von einem Nested Loop-Vorgang zu einem Hash Match-Vorgang gewechselt, was für die vorhandene Zahl an Zeilen optimaler ist.
1. Beachten Sie auch, dass es keine Überlaufwarnungen mehr gibt, da mehr Speicherplatz auf Grundlage der tatsächlichen Zeilenzahl, die den MSTVF-Table Scan durchläuft, zugewiesen wird.

### <a name="interleaved-execution-eligible-statements"></a>Zulässige Anweisungen für verschachtelte Ausführungen
Verweisanweisungen von MSTVF in verschachtelten Ausführungen müssen aktuell schreibgeschützt sein und dürfen nicht Teil eines Datenmodifizierungsvorgangs sein. MSTVFs eignen sich nur für die verschachtelte Ausführung, wenn Sie Laufzeitkonstanten verwenden.

### <a name="interleaved-execution-benefits"></a>Vorteile der verschachtelten Ausführung
Allgemein gilt: Je höher der Unterschied zwischen der geschätzten und tatsächlichen Zeilenzahl in Verbindung mit der Zahl von Downstreamplanvorgängen ist, desto mehr wird die Leistung beeinträchtigt.
Allgemein profitieren Abfragen von der verschachtelten Ausführung, die:
1. eine große Diskrepanz zwischen der geschätzten und tatsächlichen Zeilenzahl für das temporäre Resultset aufweisen (in diesem Fall MSTVF);
1. und in denen die Abfrage empfindlich ist, was Änderungen der Größe des temporären Ergebnisses angeht. Dies geschieht typischerweise, wenn es eine komplexe Struktur über der Unterstruktur im Abfrageplan gibt.
Ein einfaches `SELECT *` einer MSTVF profitiert nicht von einer verschachtelten Ausführung.

### <a name="interleaved-execution-overhead"></a>Aufwand der verschachtelten Ausführung
Der Aufwand sollte sehr gering oder nicht vorhanden sein. MSTVFs wurden bereits vor der Einführung der verschachtelten Ausführung materialisiert. Der Unterschied besteht jedoch darin, dass jetzt eine verzögerte Optimierung möglich ist sowie die anschließende Nutzung der Kardinalitätsschätzung des materialisierten Rowsets.
Genauso wie mit allen Änderungen, die sich auf Pläne auswirken, können sich einige Pläne so ändern, dass Sie einen schlechteren Plan für die Abfrage erhalten, auch wenn Sie eine bessere Kardinalität der Unterstruktur haben. Dies können Sie z.B. verhindern, indem Sie den Kompatibilitätsgrad wiederherstellen oder den Abfragespeicher verwenden, um das Verwenden der nicht rückläufigen Version des Plans zu erzwingen.

### <a name="interleaved-execution-and-consecutive-executions"></a>Verschachtelte Ausführung und nachfolgende Ausführungen
Sobald ein verschachtelter Ausführungsplan zwischengespeichert wurde, wird der Plan mit den überarbeiteten Schätzungen der ersten Ausführung für nachfolgende Ausführungen verwendet, ohne dass die verschachtelte Ausführung neu instantiiert werden muss.

### <a name="tracking-interleaved-execution-activity"></a>Nachverfolgen der Aktivität von verschachtelten Ausführungen
Sie können sich Verwendungsattribute im Ausführungsplan der Abfrage anschauen:

| Ausführungsplanattribut | BESCHREIBUNG |
| --- | --- |
| ContainsInterleavedExecutionCandidates | Gilt für den Knoten *QueryPlan*. Wenn dieser *true* lautet, gibt dieser an, dass der Plan mögliche Kandidaten für die überlappende Ausführung enthält. |
| IsInterleavedExecuted | Das Attribut des Elements *RuntimeInformation* befindet sich für den Knoten „TVF“ unter „RelOp“. Wenn es *true* entspricht, wurde der Vorgang im Zuge einer überlappenden Ausführung materialisiert. |

Sie können überlappende Ausführungen auch mit den folgenden XEvents nachverfolgen:

| XEvent | BESCHREIBUNG |
| ---- | --- |
| interleaved_exec_status | Dieses Ereignis wird ausgelöst, wenn eine verschachtelte Ausführung durchgeführt wird. |
| interleaved_exec_stats_update | Dieses Ereignis beschreibt die von der verschachtelten Ausführung aktualisierten Kardinalitätsschätzungen. |
| Interleaved_exec_disabled_reason | Dieses Ereignis wird ausgelöst, wenn eine Abfrage mit einem möglichen Kandidaten für eine verschachtelte Ausführung nicht verschachtelt ausgeführt wird. |

Eine Abfrage muss ausgeführt werden, damit die verschachtelte Ausführung die Kardinalitätsschätzungen für MSTVF überarbeiten kann. Allerdings zeigt der geschätzte Ausführungsplan immer noch an, wenn es Kandidaten für eine überlappende Ausführung gibt. Dies macht er mithilfe des showplan-Attributs `ContainsInterleavedExecutionCandidates`.    

### <a name="interleaved-execution-caching"></a>Zwischenspeichern der verschachtelte Ausführung
Wenn ein Plan aus dem Cache gelöscht oder entfernt wird, kommt es bei der Abfrageausführung zu einer neuen Kompilierung mit der verschachtelten Ausführung.
Eine Anweisung mit `OPTION (RECOMPILE)` erstellt einen neuen Plan mit der überlappenden Ausführung und speichert diesen nicht zwischen.     

### <a name="interleaved-execution-and-query-store-interoperability"></a>Geschachtelte Ausführung und Interoperabilität des Abfragespeichers
Pläne mit der verschachtelten Ausführung können erzwungen werden. Der Plan ist die Version mit angepassten Kardinalitätsschätzungen auf Grundlage der ersten Ausführung.    

### <a name="disabling-interleaved-execution-without-changing-the-compatibility-level"></a>Deaktivieren von geschachtelte Ausführung ohne Änderung des Kompatibilitätsgrads
Geschachtelte Ausführung kann im Datenbank- oder Anweisungsbereich deaktiviert werden, während der Datenbankkompatibilitätsgrad weiterhin bei 140 und höher bleibt.  Um geschachtelte Ausführung für alle Abfrageausführungen zu deaktivieren, die aus der Datenbank stammen, führen Sie die folgende Anweisung im Kontext der betroffenen Datenbank aus:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_INTERLEAVED_EXECUTION_TVF = ON;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET INTERLEAVED_EXECUTION_TVF = OFF;
```

Ist diese Einstellung aktiviert, wird sie in [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) als aktiviert aufgeführt.
Um geschachtelte Ausführung für alle Abfrageausführungen wieder zu aktivieren, die aus der Datenbank stammen, führen Sie die folgende Anweisung im Kontext der betroffenen Datenbank aus:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_INTERLEAVED_EXECUTION_TVF = OFF;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET INTERLEAVED_EXECUTION_TVF = ON;
```

Sie können adaptive Joins auch für eine verschachtelte Ausführung für eine bestimmte Abfrage deaktivieren, indem Sie `DISABLE_INTERLEAVED_EXECUTION_TVF` als [USE HINT-Abfragehinweis](../../t-sql/queries/hints-transact-sql-query.md#use_hint) festlegen. Beispiel:

```sql
SELECT [fo].[Order Key], [fo].[Quantity], [foo].[OutlierEventQuantity]
FROM [Fact].[Order] AS [fo]
INNER JOIN [Fact].[WhatIfOutlierEventQuantity]('Mild Recession',
                            '1-01-2013',
                            '10-15-2014') AS [foo] ON [fo].[Order Key] = [foo].[Order Key]
                            AND [fo].[City Key] = [foo].[City Key]
                            AND [fo].[Customer Key] = [foo].[Customer Key]
                            AND [fo].[Stock Item Key] = [foo].[Stock Item Key]
                            AND [fo].[Order Date Key] = [foo].[Order Date Key]
                            AND [fo].[Picked Date Key] = [foo].[Picked Date Key]
                            AND [fo].[Salesperson Key] = [foo].[Salesperson Key]
                            AND [fo].[Picker Key] = [foo].[Picker Key]
OPTION (USE HINT('DISABLE_INTERLEAVED_EXECUTION_TVF'));
```

Ein USE HINT-Abfragehinweis hat Vorrang vor einer datenbankweit gültigen Konfiguration oder einer Ablaufverfolgungsflageinstellung.

## <a name="table-variable-deferred-compilation"></a>Verzögerte Kompilierung von Tabellenvariablen

**Anwendungsbereich:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Die **verzögerte Kompilierung von Tabellenvariablen** verbessert die Qualität des Abfrageplans und die Gesamtleistung für Abfragen mit Verweisen auf Tabellenvariablen. Während der Optimierung und der ersten Kompilierung des Plans verteilt diese Funktion Kardinalitätsschätzungen, die auf tatsächlichen Tabellenvariablen-Zeilenzahlen basieren. Diese Informationen zur genauen Zeilenzahl werden dann zur Optimierung der nachgelagerten Planvorgänge verwendet.

Bei der verzögerten Kompilierung von Tabellenvariablen wird die Kompilierung einer Anweisung, die auf eine Tabellenvariable verweist, bis zur ersten tatsächlichen Ausführung der Anweisung verzögert. Dieses Verhalten der verzögerten Kompilierung ist identisch mit dem von temporären Tabellen. Diese Änderung führt dazu, dass anstelle des ursprünglichen einreihigen Schätzwertes die tatsächliche Kardinalität verwendet wird. 

Aktivieren Sie den Datenbank-Kompatibilitätsgrad 150 für die Datenbank, mit der Sie beim Ausführen der Abfrage verbunden sind, um die verzögerten Kompilierung von Tabellenvariablen zu aktivieren.

Die verzögerte Kompilierung von Tabellenvariablen führt **nicht** zu Änderungen an anderen Merkmalen von Tabellenvariablen. Beispielsweise wird durch dieses Feature keine Spaltenstatistik zu Tabellenvariablen hinzugefügt.

Die verzögerte Kompilierung von Tabellenvariablen **führt nicht zu einer häufigeren Neukompilierung**. Stattdessen wird der Zeitpunkt der ersten Kompilierung verschoben. Der resultierende zwischengespeicherte Plan wird basierend auf der anfänglichen Zeilenanzahl für die verzögerte Kompilierung von Tabellenvariablen generiert. Der zwischengespeicherte Plan wird von nachfolgenden Abfragen wiederverwendet. Es wird solange wiederverwendet, bis der Plan entfernt oder erneut kompiliert wird. 

Die Zeilenanzahl für Tabellenvariablen, die für die anfängliche Plankompilierung verwendet wird, stellt einen typischen Wert dar, der von einem geschätzten Festwert für die Zeilenanzahl abweichen kann. Bei Abweichungen profitieren Downstreamvorgänge. Weicht die Zeilenanzahl für Tabellenvariablen für alle durchgeführten Ausführungen erheblich ab, wird die Leistung durch dieses Feature möglicherweise nicht verbessert.

### <a name="disabling-table-variable-deferred-compilation-without-changing-the-compatibility-level"></a>Deaktivieren der verzögerten Kompilierung von Tabellenvariablen ohne Änderung des Kompatibilitätsgrads
Deaktivieren Sie die verzögerte Kompilierung von Tabellenvariablen im Datenbank- oder Anweisungsbereich, während Sie den Datenbankkompatibilitätsgrad bei 150 und höher beibehalten. Um die verzögerte Kompilierung von Tabellenvariablen für alle Abfrageausführungen zu deaktivieren, die aus der Datenbank stammen, führen Sie das folgende Beispiel im Kontext der betroffenen Datenbank aus:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET DEFERRED_COMPILATION_TV = OFF;
```

Um die verzögerte Kompilierung von Tabellenvariablen für alle Abfrageausführungen, die aus der Datenbank stammen, erneut zu aktivieren, führen Sie das folgende Beispiel im Kontext der betroffenen Datenbank aus:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET DEFERRED_COMPILATION_TV = ON;
```

Sie können die verzögerte Kompilierung von Tabellenvariablen auch für eine bestimmte Abfrage deaktivieren, indem Sie DISABLE_DEFERRED_COMPILATION_TV als USE HINT-Abfragehinweis zuweisen.  Beispiel:

```sql
DECLARE @LINEITEMS TABLE 
    (L_OrderKey INT NOT NULL,
     L_Quantity INT NOT NULL
    );

INSERT @LINEITEMS
SELECT L_OrderKey, L_Quantity
FROM dbo.lineitem
WHERE L_Quantity = 5;

SELECT O_OrderKey,
    O_CustKey,
    O_OrderStatus,
    L_QUANTITY
FROM    
    ORDERS,
    @LINEITEMS
WHERE    O_ORDERKEY    =    L_ORDERKEY
    AND O_OrderStatus = 'O'
OPTION (USE HINT('DISABLE_DEFERRED_COMPILATION_TV'));
```

## <a name="scalar-udf-inlining"></a>Inlining benutzerdefinierter Skalarfunktionen

**Anwendungsbereich:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Das skalare UDF-Inlining wandelt [skalare UDFs](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#Scalar) automatisch in relationale Ausdrücke um. Diese werden in die aufrufende SQL-Abfrage eingebettet. Diese Transformation verbessert die Leistung von Workloads, die skalare UDFs nutzen. Skalares UDF-Inlining ermöglicht eine kostenbasierte Optimierung der Vorgänge innerhalb von UDFs. Die Ergebnisse sind effiziente, mengenorientierte und parallele statt ineffiziente, iterative, serielle Ausführungspläne. Dieses Feature ist standardmäßig unter dem Datenbank-Kompatibilitätsgrad 150 aktiviert.

Weitere Informationen finden Sie unter [Inlining benutzerdefinierter Skalarfunktionen](../user-defined-functions/scalar-udf-inlining.md).

## <a name="approximate-query-processing"></a>Geschätzte Abfrageverarbeitung

**Anwendungsbereich:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Die geschätzte Abfrageverarbeitung ist eine neue Featurefamilie. Sie stellt Aggregationen über große Datasets hinweg bereit, bei denen die Reaktionsfähigkeit wichtiger ist als die absolute Präzision. Ein Beispiel ist die Berechnung eines **COUNT(DISTINCT())** über 10 Milliarden Zeilen für die Anzeige auf einem Dashboard. In diesem Fall ist absolute Genauigkeit nicht wichtig, aber die Reaktionsfähigkeit ist es jedoch. Diese neue **APPROX_COUNT_DISTINCT**-Aggregatfunktion gibt die ungefähre Anzahl von eindeutigen Ungleich-Null-Werten in einer Gruppe zurück.

Weitere Informationen finden Sie unter [APPROX_COUNT_DISTINCT (Transact-SQL)](../../t-sql/functions/approx-count-distinct-transact-sql.md).

## <a name="batch-mode-on-rowstore"></a>Batchmodus bei Rowstore 

**Anwendungsbereich:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Batchmodus bei Rowstore ermöglicht die Ausführung im Batchmodus für Analyseworkloads, die keine Columnstore-Indizes erfordern.  Dieses Feature unterstützt die Ausführung im Batchmodus und Bitmapfilter für On-Disk-Heaps und B-Struktur-Indizes. Batchmodus bei Rowstore ermöglicht die Unterstützung aller vorhandenen batchmodusfähigen Operatoren.

### <a name="background"></a>Hintergrund
Mit [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] wurde ein neues Feature zur Beschleunigung analytischer Workloads eingeführt: Columnstore-Indizes. Wir haben die Anwendungsfälle erweitert und die Leistung von Columnstore-Indizes in allen nachfolgenden Releases verbessert. Bisher wurden diese Funktionen so dargestellt und dokumentiert, als ob es sich um genau ein Feature handeln würde. Sie erstellen die Columnstore-Indizes in Ihren Tabellen. Und Ihre analytische Workload wird schneller ausgeführt. Es gibt jedoch zwei miteinander zusammenhängende, aber unterschiedliche Gruppen von Technologien:
- **Columnstore**-Indizes erlauben Analyseabfragen nur den Zugriff auf die Daten in den Spalten, die sie benötigen. Die Seitenkomprimierung im Spaltenspeicherformat ist ebenfalls effektiver als die Komprimierung in traditionellen **Rowstore**-Indizes. 
- Mit der Verarbeitung im **Batchmodus** verarbeiten Abfrageoperatoren Daten effizienter. Die Verarbeitung erfolgt für ein Batch an Zeilen und nicht für jede Zeile einzeln. Zahlreiche weitere Verbesserungen der Skalierbarkeit sind an die Batchmodusverarbeitung gebunden. Weitere Informationen zum Batchmodus finden Sie unter [Ausführungsmodi](../../relational-databases/query-processing-architecture-guide.md#execution-modes).

Die zwei Featuregruppen verbessern zusammen die E/A- und CPU-Auslastung:
- Durch Columnstore-Indizes passen mehr Daten in den Speicher. Dadurch wird die E/A-Arbeitsauslastung reduziert.
- Die Batchmodusverarbeitung nutzt die CPU effizienter.

Wann immer möglich, machen sich die beiden Technologien gegenseitig zu Nutze. So können Batchmodusaggregate z.B. als Teil eines Columnstore-Indexes ausgewertet werden. Außerdem werden komprimierte Columnstore-Daten mit der Lauflängencodierung wesentlich effizienter mit Joins im Batchmodus und Batchmodusaggregaten verarbeitet. 
 
Es ist jedoch wichtig, zu berücksichtigen, dass die beiden Merkmale unabhängig voneinander sind:
* Sie können Zeilenmoduspläne erhalten, die Columnstore-Indizes verwenden.
* Sie können Batchmoduspläne erhalten, die nur Rowstore-Indizes verwenden. 

Sie erhalten in der Regel die besten Ergebnisse, wenn Sie die beiden Features zusammen verwenden. Bisher betrachtete der SQL Server-Abfrageoptimierer die Verarbeitung im Batchmodus nur für Abfragen, die mindestens eine Tabelle mit einem Columnstore-Index betreffen.

Columnstore-Indizes sind für einige Anwendungen möglicherweise nicht geeignet. Eine Anwendung kann ein anderes Feature verwenden, das nicht von Columnstore-Indizes unterstützt wird. Direkte Änderungen sind beispielsweise nicht mit der Columnstore-Komprimierung kompatibel. Daher werden Auslöser in Tabellen mit gruppierten Columnstore-Indizes nicht unterstützt. Noch wichtiger ist, dass **DELETE**- und **UPDATE**-Anweisungen durch Columnstore-Indizes aufwändiger werden. 

Für einige hybride Transaktions-/Analyseworkloads überwiegt der Mehraufwand bei Transaktionsworkloads die Vorteile der Verwendung von Columnstore-Indizes für Analyseabfragen. Solche Szenarios können von einer verbesserten CPU-Auslastung profitieren, indem nur die Batchmodusverarbeitung genutzt wird. Aus diesem Grund berücksichtigt das Feature „Batchmodus bei Rowstore“ den Batchmodus für alle Abfragen, unabhängig davon, welche Art von Indizes beteiligt sind.

### <a name="workloads-that-might-benefit-from-batch-mode-on-rowstore"></a>Workloads, die vom Batchmodus auf Rowstore profitieren können
Die folgenden Workloads können vom Batchmodus auf Rowstore profitieren:
* Ein signifikanten Teil der Workload besteht aus analytischen Abfragen. Normalerweise verwenden diese Abfragen Operatoren wie Joins oder Aggregate, die Hunderttausende von Zeilen oder mehr verarbeiten.
* Die Workload ist CPU-gebunden. Wenn der Engpass E/A ist, wird weiterhin empfohlen, dass Sie nach Möglichkeit einen Columnstore-Index in Betracht ziehen.
* Das Erstellen eines Columnstore-Index fügt dem transaktionalen Teil Ihrer Workload zu viel Mehraufwand hinzu. Oder es kann kein Columnstore-Index erstellt werden, da Ihre Anwendung von einem Feature abhängt, das bei Columnstore-Indizes noch nicht unterstützt wird.


> [!NOTE]
> Batchmodus bei Rowstore kann nur bei der Verringerung des CPU-Verbrauchs helfen. Wenn Ihr Engpass E/A-bezogen ist und Daten nicht bereits zwischengespeichert werden („kalter“ Cache), verbessert Batchmodus bei Rowstore die verstrichene Abfragezeit nicht. Ähnlich gilt, dass eine Leistungsverbesserung unwahrscheinlich ist, wenn auf dem Computer nicht genügend Arbeitsspeicher zum Zwischenspeichern aller Daten vorhanden ist.

### <a name="what-changes-with-batch-mode-on-rowstore"></a>Welche Änderungen mit dem Batchmodus auf Rowstore verbunden sind

Legen Sie für die Datenbank den Kompatibilitätsgrad 150 fest. Es sind keine weiteren Änderungen erforderlich.

Auch wenn eine Abfrage nicht auf Tabellen mit Columnstore-Indizes zugreift, verwendet der Abfrageprozessor Heuristik, um zu entscheiden, ob der Batchmodus berücksichtigt wird. Die Heuristik umfasst diese Überprüfungen:
1. Ein erstes Überprüfen der Tabellengrößen, verwendeten Operatoren und geschätzten Kardinalitäten in der Eingabeabfrage.
2. Weitere Prüfpunkte kommen hinzu, wenn der Abfrageoptimierer neue, kostengünstigere Pläne für die Abfrage entdeckt. Wenn diese alternativen Pläne keine signifikante Verwendung des Batchmodus aufweisen, beendet der Optimierer die Untersuchung von Batchmodusalternativen.

Wenn der Batch-modus bei Rowstow verwendet wird, sehen Sie den tatsächlichen Ausführungsmodus als **Batchmodus** im Abfrageplan. Der Scan-Operator verwendet den Batchmodus für On-Disk-Heaps und B-Struktur-Indizes. Diese Überprüfung im Batchmodus kann Batchmodus-Bitmapfilter auswerten. Vielleicht finden Sie auch andere Batchmodusoperatoren im Plan. Beispielsweise Hashjoins, hashbasierte Aggregate, Sortierungen, Fensteraggregate, Filter, Verkettung und Skalarwertberechnungs-Operatoren.

### <a name="remarks"></a>Bemerkungen

Abfragepläne verwenden nicht immer den Batchmodus. Der Abfrageoptimierer entscheidet möglicherweise, dass der Batchmodus für die Abfrage nicht sinnvoll ist. 

Der Suchbereich des Abfrageoptimierers ändert sich. Wenn Sie also einen Zeilenmodusplan erhalten, ist er möglicherweise nicht derselbe wie der Plan, den Sie in einem niedrigeren Kompatibilitätsgrad erhalten. Und wenn Sie einen Batchmodusplan erhalten, ist er möglicherweise nicht derselbe wie der Plan, den Sie mit einem Columnstore-Index erhalten. 

Aufgrund des neuen Batchmodus-Rowstore-Scans können Pläne sich auch für Abfragen ändern, die Columnstore- und Rowstore-Indizes mischen.

Aktuell bestehen folgende Einschränkungen für den neuen Batchmodus bei Rowstorescans: 
- Er funktioniert nicht bei In-Memory-OLTP-Tabellen und kann ausschließlich für Indizes verwendet werden, die sich auf Datenträgerheaps oder in B-Strukturen befinden. 
- Er funktioniert auch nicht, wenn eine LOB-Spalte abgerufen oder gefiltert wird. Diese Einschränkung betrifft Spaltensätze mit geringer Dichte und XML-Spalten.

Es gibt Abfragen, für die der Batchmodus auch bei Columnstore-Indizes nicht verwendet wird. Beispiele sind Abfragen, die Cursor enthalten. Dieselben Ausschlüsse gelten auch für den Batchmodus bei Rowstore.

### <a name="configure-batch-mode-on-rowstore"></a>Konfigurieren des Batchmodus bei Rowstore
Die datenbankweite **BATCH_MODE_ON_ROWSTORE**-Konfiguration ist standardmäßig aktiviert. Sie deaktiviert den Batchmodus bei Rowstore, ohne dass eine Änderung des Datenbank-Kompatibilitätgrads erforderlich ist:

```sql
-- Disabling batch mode on rowstore
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_ON_ROWSTORE = OFF;

-- Enabling batch mode on rowstore
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_ON_ROWSTORE = ON;
```

Sie können den Batchmodus bei Rowstore über die datenbankweite Konfiguration deaktivieren. Sie können die Einstellung auf Abfrageebene jedoch immer noch überschreiben, indem Sie den Abfragehinweis **ALLOW_BATCH_MODE** verwenden. Im folgenden Beispiel wird der Batchmodus bei Rowstore aktiviert, auch wenn die Funktion über die datenbankweite Konfiguration deaktiviert ist:

```sql
SELECT [Tax Rate], [Lineage Key], [Salesperson Key], SUM(Quantity) AS SUM_QTY, SUM([Unit Price]) AS SUM_BASE_PRICE, COUNT(*) AS COUNT_ORDER
FROM Fact.OrderHistoryExtended
WHERE [Order Date Key]<=DATEADD(dd, -73, '2015-11-13')
GROUP BY [Tax Rate], [Lineage Key], [Salesperson Key]
ORDER BY [Tax Rate], [Lineage Key], [Salesperson Key]
OPTION(RECOMPILE, USE HINT('ALLOW_BATCH_MODE'));
```

Sie können den Batchmodus bei Rowstore auch mit dem Abfragehinweis **DISALLOW_BATCH_MODE** für eine bestimmte Abfrage deaktivieren. Sehen Sie sich folgendes Beispiel an:

```sql
SELECT [Tax Rate], [Lineage Key], [Salesperson Key], SUM(Quantity) AS SUM_QTY, SUM([Unit Price]) AS SUM_BASE_PRICE, COUNT(*) AS COUNT_ORDER
FROM Fact.OrderHistoryExtended
WHERE [Order Date Key]<=DATEADD(dd, -73, '2015-11-13')
GROUP BY [Tax Rate], [Lineage Key], [Salesperson Key]
ORDER BY [Tax Rate], [Lineage Key], [Salesperson Key]
OPTION(RECOMPILE, USE HINT('DISALLOW_BATCH_MODE'));
```

## <a name="see-also"></a>Weitere Informationen
[Leistungscenter für SQL Server-Datenbankmodul und Azure SQL-Datenbank](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md)     
[Leitfaden zur Architektur der Abfrageverarbeitung](../../relational-databases/query-processing-architecture-guide.md)    
[Referenz zu logischen und physischen Showplanoperatoren](../../relational-databases/showplan-logical-and-physical-operators-reference.md)    
[Joins](../../relational-databases/performance/joins.md)       
[Demo zur intelligenten Abfrageverarbeitung](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/intelligent-query-processing)   
