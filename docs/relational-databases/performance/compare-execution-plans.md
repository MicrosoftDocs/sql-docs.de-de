---
title: Vergleichen von Ausführungsplänen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ähnlichkeiten und Unterschiede zwischen tatsächlichen grafischen Ausführungsplänen mithilfe der Planvergleichsfunktion von SQL Server Management Studio vergleichen können.
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- comparing execution plans
- compare execution plans
- plan comparison
- execution plans [SQL Server], comparing
- Query Store, comparing plans
ms.assetid: 9e583a18-5f4a-4054-bfe1-4b2a76630db6
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 03b6ec22a3eadf7281f0513997cfcc28d40eb1b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344729"
---
# <a name="compare-execution-plans"></a>Vergleichen von Ausführungsplänen
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
In diesem Thema wird beschrieben, wie Sie Ähnlichkeiten und Unterschiede zwischen tatsächlichen grafischen Ausführungsplänen mithilfe der [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]-Planvergleichsfunktion vergleichen können. Dieses Feature ist ab [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], Version 16, verfügbar.
  
> [!NOTE]
> Tatsächliche Ausführungspläne werden nach der Ausführung der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Abfragen oder -Batches generiert. Deshalb enthält ein tatsächlicher Ausführungsplan Laufzeitinformationen wie die tatsächliche Anzahl der Zeilen, Nutzungsmetriken der Ressourcen oder Laufzeitwarnungen (falls vorhanden). Weitere Informationen finden Sie unter [Anzeigen eines tatsächlichen Ausführungsplans](../../relational-databases/performance/display-an-actual-execution-plan.md).
  
Die Möglichkeit, Pläne zu vergleichen, ist eine Aufgabe, die Datenbankexperten aus Gründen der Problembehandlung ggf. übernehmen müssen:
-   Ermitteln, warum eine Abfrage oder ein Batch plötzlich langsam ausgeführt wird.
-   Verstehen der Auswirkungen des Neuschreibens einer Abfrage.
-   Beobachten, wie eine spezifische leistungssteigernde Änderung am Schemaentwurf (z.B. ein neuer Index) den Ausführungsplan effektiv geändert hat.  
 
Die Menüoption **Planvergleich** ermöglicht den Vergleich zweier verschiedener Ausführungspläne nebeneinander, um Ähnlichkeiten und Änderungen, die das unterschiedliche Verhalten aus allen oben genannten Gründen erklären, leichter zu identifizieren. Diese Option kann Folgendes vergleichen:
- Zwei zuvor gespeicherten Ausführungsplandateien (Erweiterung *SQLPLAN*).
- Einen aktiven Ausführungsplan und einen zuvor gespeicherten Abfrageausführungsplan.
- Zwei ausgewählte Abfragepläne im [Abfragespeicher](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).

> [!TIP]
> Der Planvergleich funktioniert mit allen *SQLPLAN*-Dateien, auch mit älteren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Außerdem ermöglicht diese Option einen Offlinevergleich, sodass keine Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz erforderlich ist. 

Wenn zwei Ausführungspläne verglichen werden, werden Bereiche des Plans, die **im Wesentlichen die gleichen Aufgaben ausführen**, in der gleichen Farbe und mit dem gleichen Muster hervorgehoben. Wenn Sie auf einen farbigen Bereich in einem Plan klicken, wird der andere Plan auf dem entsprechenden Knoten in diesem Plan zentriert. Sie können weiterhin Operatoren und Knoten der Ausführungspläne ohne Übereinstimmung vergleichen, aber in diesem Fall müssen Sie die zu vergleichenden Operatoren manuell auswählen.

> [!IMPORTANT]
> Nur Knoten, die die Form des Plans ändern, werden verwendet, um nach Ähnlichkeiten zu suchen. Daher kann es vorkommen, dass ein Knoten, der nicht farbig ist, in der Mitte von zwei Knoten liegt, die sich im gleichen Unterabschnitt des Plans befinden. Das Fehlen von Farbe bedeutet in diesem Fall, dass die Knoten bei der Überprüfung, ob die Abschnitte gleich sind, nicht berücksichtigt wurden.
  
## <a name="to-compare-execution-plans"></a>So vergleichen Sie Ausführungspläne
  
1.  Öffnen Sie eine zuvor gespeicherte Abfrageausführungsplan-Datei (SQLPLAN-Datei) mithilfe des Menüs **Datei**, und klicken Sie auf **Datei öffnen**, oder ziehen Sie eine Plandatei in das [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]-Fenster. Wenn Sie soeben eine Abfrage ausgeführt und sich für die Anzeige ihres Ausführungsplans entschieden haben, navigieren Sie alternativ zur Registerkarte **Ausführungsplan** im Ergebnisbereich. 

2.  Klicken Sie mit der rechten Maustaste in einem leeren Bereich des Ausführungsplans, und klicken Sie dann auf **Showplan vergleichen**. 

    ![Klicken mit der rechten Maustaste auf „Showplan vergleichen“](../../relational-databases/performance/media/plancomparisonmenuoption.png "Klicken mit der rechten Maustaste auf „Showplan vergleichen“")   

3.  Wählen Sie die zweite Abfrageplandatei aus, mit der der Vergleich durchgeführt werden soll. Die zweite Datei wird geöffnet, sodass Sie die Pläne vergleichen können.

4.  Die verglichenen Plänen öffnen ein neues Fenster, standardmäßig mit einem Plan oben und einem Plan unten. Die Standardauswahl ist das erste Auftreten eines Operators oder Knotens, der den verglichenen Plänen gemeinsam ist, aber Unterschiede zwischen den Plänen aufweist. Alle hervorgehobenen Operatoren und Knoten sind in beiden verglichenen Plänen vorhanden. Wenn Sie einen hervorgehobenen Operator im oberen oder linken Plan auswählen, wird automatisch der entsprechende Operator im unteren oder rechten Plan ausgewählt. Durch die Auswahl des Stammknotenoperators in einem der verglichenen Pläne (der SELECT-Knoten in der Abbildung unten) wird auch der jeweilige Stammknotenoperator im anderen verglichenen Plan ausgewählt.

    ![Planen des Vergleichs von zwei gespeicherten Plandateien](../../relational-databases/performance/media/plancomparison-plans.png "Planen des Vergleichs von zwei gespeicherten Plandateien")  

     > [!TIP]
     > Sie können die Anzeige des Ausführungsplanvergleichs nebeneinander umschalten, indem Sie mit der rechten Maustaste auf einen leeren Bereich des Ausführungsplans klicken und **Teilerausrichtung umschalten** auswählen.

     > [!TIP]
     > Alle für Ausführungspläne verfügbaren Zoom- und Navigationsoptionen funktionieren im Planvergleichsmodus. Weitere Einzelheiten finden Sie unter [Anzeigen eines tatsächlichen Ausführungsplans](../../relational-databases/performance/display-an-actual-execution-plan.md).

5.  Auf der rechten Seite öffnet sich ebenfalls ein Fenster mit zwei Eigenschaften im Rahmen der Standardauswahl. Eigenschaften, die in beiden verglichenen Operatoren vorhanden sind, aber Unterschiede aufweisen, wird zur leichteren Identifizierung das *Ungleichheitszeichen* (&ne;) vorangestellt.

    ![Duales Eigenschaftenfenster](../../relational-databases/performance/media/plancomparison-properties.png "Duales Eigenschaftenfenster")  

6.  Das Vergleichsnavigationsfenster **Showplananalyse** wird ebenfalls im unteren Bereich geöffnet. Es stehen drei Registerkarten zur Verfügung:

    1.  Auf der Registerkarte **Anweisungsoptionen** ist die Standardauswahl *Ähnliche Vorgänge hervorheben*, und der gleiche hervorgehobene Operator bzw. Knoten in verglichenen Plänen weist die gleiche Farbe und dasselbe Linienmuster auf. Navigieren Sie zwischen ähnlichen Bereichen in verglichenen Plänen, indem Sie auf ein Linienmuster klicken. Sie können auch Unterschiede in den Plänen anstelle von Ähnlichkeiten hervorheben, indem Sie *Vorgänge hervorheben, die nicht mit ähnlichen Segmenten übereinstimmen* auswählen. 
    
       > [!NOTE]
       > Standardmäßig werden Datenbanknamen beim Vergleichen von Plänen ignoriert, um den Vergleich von Plänen zu ermöglichen, die für Datenbanken erfasst wurden, die unterschiedliche Namen aufweisen, aber das gleiche Schema gemeinsam verwenden. Beispielsweise beim Vergleich von Plänen aus den Datenbanken *ProdDB* und *TestDB*. Dieses Verhalten kann mit der Option *Datenbanknamen beim Vergleichen von Operatoren ignorieren* geändert werden.

       ![Showplananalyse-Fenster](../../relational-databases/performance/media/plancomparison-analysis.png "Showplananalyse-Fenster") 

    2.  Die Registerkarte **Mehrere Anweisungen** ist beim Vergleichen von Plänen mit mehreren Anweisungen nützlich, weil sie erlaubt, dass das richtige Anweisungspaar verglichen wird.

        ![Mehrere Anweisungen im verglichenen Plan](../../relational-databases/performance/media/plancomparison-multiple.png "Mehrere Anweisungen im verglichenen Plan")  

    3.  Auf der Registerkarte **Szenarien** finden Sie eine automatisierte Analyse einiger der wichtigsten Aspekte, die Sie sich in Bezug auf die Unterschiede in den verglichenen Plänen im Zusammenhang mit der [Kardinalitätsschätzung](../../relational-databases/performance/cardinality-estimation-sql-server.md) ansehen können. Für jeden aufgelisteten Operator im linken Bereich zeigt der rechte Bereich Details zum Szenario im Link *Klicken Sie hier, um weitere Informationen zu diesem Szenario zu erhalten* sowie mögliche Gründe an, um dieses Szenario zu erläutern. 

        ![Unterschiedliche geschätzte Zeilen](../../relational-databases/performance/media/plancomparison-scenarios.png "Unterschiedliche geschätzte Zeilen")  

    Wenn dieses Fenster geschlossen ist, klicken Sie mit der rechten Maustaste auf einen leeren Bereich eines verglichenen Plans, und wählen Sie **Optionen für den Showplanvergleich** aus, um es erneut zu öffnen.

    ![Planen von Vergleichsoptionen](../../relational-databases/performance/media/plancomparison-options.png "Planen von Vergleichsoptionen")  

## <a name="to-compare-execution-plans-in-query-store"></a>So vergleichen Sie Ausführungspläne im Abfragespeicher

1.  Identifizieren Sie im Abfragespeicher eine Abfrage mit mehreren Ausführungsplänen. Weitere Informationen zu Abfragespeicherszenarien finden Sie unter [Verwendungsszenarien für den Abfragespeicher](../../relational-databases/performance/query-store-usage-scenarios.md#identify-and-tune-top-resource-consuming-queries).

2.  Verwenden Sie die UMSCHALTTASTE und Ihre Maus, um zwei Pläne für die gleiche Abfrage auszuwählen. 

    ![Auswählen von zwei Plänen im Abfragespeicher](../../relational-databases/performance/media/plancomparison-querystore.png "Auswählen von zwei Plänen im Abfragespeicher")   

3.  Verwenden Sie die Schaltfläche **Compare the plans for the select query in a separate window** (Pläne für die ausgewählte Abfrage in einem separaten Fenster vergleichen), um den Planvergleich zu starten. Führen Sie dann die Schritte 4 bis 6 von *So vergleichen Sie Ausführungspläne* aus. 

    ![Vergleichen des Showplans im Abfragespeicher](../../relational-databases/performance/media/plancomparison-querystoreoption.png "Vergleichen des Showplans im Abfragespeicher") 
