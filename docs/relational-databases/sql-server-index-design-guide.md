---
title: Leitfaden zur Architektur und zum Design von SQL Server-Indizes | Microsoft-Dokumentation
description: In diesem Artikel wird das Entwerfen effizienter Indizes in SQL Server behandelt, um eine gute Datenbank- und Anwendungsleistung zu erzielen. Außerdem erfahren Sie mehr über die Indexarchitektur und bewährte Methoden.
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- index design guide
- index design guidance
- guide, index design
- guidance, index design
- index internals
- index architecture
- sql server index internals
- sql server index architecture
- sql server index design guide
- sql server index design guidance
ms.assetid: 11f8017e-5bc3-4bab-8060-c16282cfbac1
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d7a1f83563e3ae103fa683bbe604926f40982d06
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100352709"
---
# <a name="sql-server-index-architecture-and-design-guide"></a>Leitfaden zur Architektur und zum Design von SQL Server-Indizes
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Schlecht entworfene oder fehlende Indizes sind die Hauptquellen für Engpässe der Datenbankanwendung. Ein effizienter Indexentwurf ist zum Erzielen einer guten Datenbank- und Anwendungsleistung unabdinglich. Dieser Leitfaden zum Entwerfen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Indizes enthält Informationen zur Indexarchitektur und bewährte Methoden, die Sie beim Entwerfen effektiver Indizes unterstützen sollen, die den Anforderungen Ihrer Anwendung entsprechen.  
    
In diesem Handbuch wird davon ausgegangen, dass der Leser grundsätzlich mit den in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]verfügbaren Indextypen vertraut ist. Eine allgemeine Beschreibung zu Indextypen finden Sie unter [Indextypen](../relational-databases/indexes/indexes.md).  

Dieses Handbuch behandelt folgende Typen von Indizes:

-   Gruppiert
-   Nicht gruppiert
-   Eindeutig
-   Filtered
-   columnstore
-   Hash
-   Speicheroptimiert, nicht gruppiert

Weitere Informationen zu XML-Indizes finden Sie unter [Übersicht über XML-Indizes](../relational-databases/xml/xml-indexes-sql-server.md) und [Selektive XML-Indizes (SXI)](../relational-databases/xml/selective-xml-indexes-sxi.md).

Weitere Informationen zu räumlichen Indizes finden Sie unter [Übersicht über räumliche Indizes](../relational-databases/spatial/spatial-indexes-overview.md).

Weitere Informationen zu Volltextindizes finden Sie unter [Auffüllen von Volltextindizes](../relational-databases/search/populate-full-text-indexes.md).
  
##  <a name="index-design-basics"></a><a name="Basics"></a> Grundlagen des Indexentwurfs  
 Am Ende eines guten Sachbuchs befindet sich meist ein Index, über den bestimmte Informationen schnell und zielsicher innerhalb des Buchs ausfindig gemacht werden können. Ein Index ist eine sortierte Liste von Schlüsselwörtern, neben denen jeweils eine oder mehrere Seitenzahlen auf die Seiten verweisen, auf denen das jeweilige Schlüsselwort gefunden werden kann. Der SQL Server-Index macht da keinen Unterschied: Er stellt eine sortierte Liste mit Werten dar, bei der jeweils Zeiger auf die [Datenseiten](../relational-databases/pages-and-extents-architecture-guide.md) hinweisen, auf denen sich diese Werte befinden. Der Index selbst wird auf Seiten gespeichert, die die Indexseiten in SQL Server bilden. In einem Buch umfasst der Index meist mehrere Seiten. Wenn Sie nun beispielsweise nach Verweisen auf alle Seiten mit dem Wort „SQL“ suchen, müssen Sie den Index bis zu der Seite durchblättern, die das Schlüsselwort „SQL“ enthält. Von dort gelangen Sie dann über die Verweise zu den angegebenen Buchseiten.  Dies ließe sich durch Hinzufügen einer Seite vor dem Index optimieren, auf der die Seitenzahlen für die einzelnen Buchstaben des Alphabets angegeben werden. Beispiel: „A bis D: Seite 121“, „E bis G: Seite 122“ usw. Mit dieser zusätzlichen Seite entfiele der Schritt, den Index nach dem Ausgangspunkt durchsuchen zu müssen. Eine solche Seite existiert in einem Buch meist nicht, in einem SQL Server-Index jedoch schon. Sie wird als Stammseite des Index bezeichnet. Die Stammseite stellt die Startseite der Baumstruktur dar, die von einem SQL Server-Index verwendet wird. Folgt man der Baumanalogie, so werden die Seiten mit den Zeigern auf die jeweiligen Daten als „Blattseiten“ der Baumstruktur bezeichnet. 

 Ein SQL Server-Index ist eine Struktur auf dem Datenträger oder im Arbeitsspeicher, die einer Tabelle oder einer Sicht zugeordnet ist und durch die das Abrufen von Zeilen aus der Tabelle oder Sicht beschleunigt wird. Ein Index enthält Schlüssel, die aus einer oder mehreren Spalten in der Tabelle oder Sicht erstellt werden. Bei Indizes auf Datenträgern werden diese Schlüssel in einer Baumstruktur (B-Struktur) gespeichert, die es SQL Server ermöglicht, die den Schlüsselwerten zugeordnete(n) Zeile(n) schnell und effizient zu finden.  

 In einem Index werden Daten logisch in Form einer Tabelle mit Zeilen und Spalten gespeichert und physisch in einem zeilenbezogenen Format namens *Rowstore*<sup>1</sup> oder in einem spaltenbezogenen Format namens *[Columnstore](#columnstore_index)* .  
    
 Das Auswählen der richtigen Indizes für eine Datenbank und ihre Arbeitsauslastung ist ein komplexer Vorgang, bei dem ein ausgewogenes Verhältnis zwischen gewünschter Abfragegeschwindigkeit und vertretbaren Updatekosten erzielt werden muss. Schmale Indizes (Indizes mit wenigen Spalten im Indexschlüssel) erfordern weniger Speicherplatz und Wartungsaufwand. Breite Indizes decken jedoch eine größere Anzahl an Abfragen ab. Möglicherweise müssen Sie mit verschiedenen Entwürfen experimentieren, bevor Sie den effizientesten Index ermitteln. Indizes können ohne Auswirkungen auf das Datenbankschema oder den Anwendungsentwurf hinzugefügt, geändert und gelöscht werden. Daher sollten Sie in jedem Fall mit verschiedenen Indizes experimentieren.  
  
 Der Abfrageoptimierer von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] wählt in der Mehrzahl der Fälle zuverlässig den effektivsten Index aus. Indexentwurfsstrategie sollte sein, dem Abfrageoptimierer eine Vielzahl von Indizes zur Auswahl bereitzustellen und sich darauf zu verlassen, dass dieser die richtige Entscheidung trifft. Auf diese Weise wird die Analysezeit verkürzt und ein gutes Leistungsverhalten in vielen unterschiedlichen Situationen erzielt. Wenn Sie anzeigen möchten, welche Indizes der Abfrageoptimierer für eine bestimmte Abfrage verwendet, wählen Sie in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]die Option **Tatsächlichen Ausführungsplan einschließen** aus dem Menü **Abfrage** aus.  
  
 Setzen Sie Indexverwendung aber nicht stets mit gutem Leistungsverhalten bzw. gute Leistung mit effizienter Indexverwendung gleich. Würde durch die Verwendung eines Indexes in jedem Fall die beste Leistung erzielt, so wäre die Arbeit des Abfrageoptimierers sehr einfach. Tatsächlich kann die Auswahl eines falschen Indexes eine Leistung bewirken, die nicht optimal ist. Daher besteht die Aufgabe des Abfrageoptimierers darin, einen Index oder eine Kombination aus Indizes nur dann auszuwählen, wenn die Leistung verbessert wird, und den indizierten Abruf zu vermeiden, wenn die Leistung negativ beeinflusst wird.  

 <sup>1</sup> Rowstore stellte die herkömmliche Vorgehensweise beim Speichern von Daten aus relationalen Tabellen dar. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] bezieht Rowstore sich auf die Tabelle, deren zugrunde liegendes Datenspeicherformat ein Heap, eine B-Struktur (d.h. ein [gruppierter Index](#Clustered)) oder eine speicheroptimierte Tabelle ist.

### <a name="index-design-tasks"></a>Aufgaben beim Indexentwurf  
 Die folgenden Aufgaben stellen die empfohlene Strategie zum Entwerfen von Indizes dar:  
  
1.  Verstehen der Merkmale der Datenbank selbst. 
    * Ob es sich also beispielsweise um eine OLTP-Datenbank (Online Transaction Processing) mit häufigen Datenänderungen handelt, die einen hohen Durchsatz aufrechterhalten muss. Ab [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] sind speicheroptimierte Tabellen und Indizes besonders geeignet für dieses Szenario, da sie ohne Latches auskommen. Weitere Informationen finden Sie unter [Indizes für speicheroptimierte Tabellen](../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md) oder unter [Richtlinien zum Entwerfen von nicht gruppierten Indizes für speicheroptimierte Tabellen](#inmem_nonclustered_index) und [Richtlinien zum Entwerfen von Hashindizes für speicheroptimierte Tabellen](#hash_index) in diesem Handbuch.
    * Weitere Informationen finden Sie ebenfalls in einem Beispiel für ein Entscheidungsunterstützungssystem (Decision Support System, DSS) oder für eine Data Warehousing-Datenbank (OLAP), die große Datasets schnell verarbeiten muss. Ab [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] sind Columnstore-Indizes besonders für typische Data Warehousing-Datasets geeignet. Columnstore-Indizes verbessern die Benutzererfahrung im Bereich Data Warehousing, da sie die schnellere Ausführung allgemeiner Data Warehousing-Abfragen, wie Filter-, Aggregierungs-, Gruppierungs- und Sternjoinabfragen ermöglichen. Weitere Informationen finden Sie unter [Columnstore-Indizes: Übersicht](../relational-databases/indexes/columnstore-indexes-overview.md) oder unter [Richtlinien zum Entwerfen von Columnstore-Indizes](#columnstore_index) in diesem Handbuch.  

2.  Verstehen der Merkmale der am häufigsten verwendeten Abfragen. Wenn Sie z. B. wissen, dass eine häufig verwendete Abfrage zwei oder mehr Tabellen verknüpft, unterstützt Sie dieses Wissen beim Ermitteln der zu verwendenden effizientesten Indextypen.  
  
3.  Verstehen der Merkmale der in den Abfragen verwendeten Spalten. Ein Index eignet sich z. B. ideal für Spalten, die einen ganzzahligen Datentyp besitzen und außerdem eindeutige oder Nicht-NULL-Spalten sind. Für Spalten, die klar definierte Teilmengen von Daten enthalten, können Sie in [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] und höheren Versionen einen gefilterten Index verwenden. Weitere Informationen finden Sie unter [Richtlinien für den Entwurf gefilterter Indizes](#Filtered) in diesem Handbuch.  
  
4.  Ermitteln, welche Indexoptionen die Leistung steigern können, wenn der Index erstellt oder gewartet wird. Für das Erstellen eines gruppierten Indexes für eine vorhandene Tabelle ist z.B. die `ONLINE`-Indexoption nützlich. Die ONLINE-Option ermöglicht, dass gleichzeitige Aktivitäten für die zugrunde liegenden Daten fortgesetzt werden können, während der Index erstellt oder neu erstellt wird. Weitere Informationen finden Sie unter [Festlegen von Indexoptionen](../relational-databases/indexes/set-index-options.md).  
  
5.  Angeben des optimalen Speicherorts für den Index. Ein nicht gruppierter Index kann in derselben Dateigruppe wie die zugrunde liegende Tabelle oder in einer anderen Dateigruppe gespeichert werden. Der Speicherort von Indizes kann die Abfrageleistung durch Optimieren der Datenträger-E/A-Leistung verbessern. Wenn Sie z. B. einen nicht gruppierten Index in einer Dateigruppe speichern, die sich auf einem anderen Datenträger als die Tabellendateigruppe befindet, kann dies die Leistung verbessern, weil mehrere Datenträger gleichzeitig gelesen werden können.  
     Alternativ können gruppierte und nicht gruppierte Indizes ein dateigruppenübergreifendes Partitionsschema verwenden. Durch die Partitionierung sind große Tabellen oder Indizes leichter zu verwalten, denn Sie können dadurch schnell und effizient auf Datenteilmengen zugreifen und sie verwalten, während die Integrität der gesamten Sammlung erhalten bleibt. Weitere Informationen finden Sie unter [partitionierte Tabellen und Indizes](../relational-databases/partitions/partitioned-tables-and-indexes.md). Wenn Sie Partitionierung in Erwägung ziehen, müssen Sie festlegen, ob der Index ausgerichtet sein soll, d. h., ob er weitgehend wie die Tabelle oder unabhängig davon partitioniert werden soll.   

##  <a name="general-index-design-guidelines"></a><a name="General_Design"></a> Allgemeine Richtlinien zum Indexentwurf  
 Erfahrene Datenbankadministratoren sind in der Lage, einen geeigneten Satz an Indizes zu entwerfen. Es handelt sich jedoch selbst bei gemäßigt komplexen Datenbanken und Arbeitsauslastungen um eine sehr komplexe, zeitintensive und fehleranfällige Aufgabe. Das Verständnis der Merkmale der Datenbank, Abfragen und Datenspalten kann Sie beim Entwerfen optimaler Indizes unterstützen.  
  
### <a name="database-considerations"></a>Überlegungen zu Datenbanken  
 Beachten Sie beim Entwerfen eines Indexes die folgenden Datenbankrichtlinien:  
  
-   Eine große Anzahl von Indizes für eine Tabelle beeinträchtigt die Leistung von `INSERT`-, `UPDATE`-, `DELETE`-, und `MERGE`-Anweisungen, da alle Indizes entsprechend angepasst werden müssen, sobald Daten in der Tabelle geändert werden. Wenn eine Spalte beispielsweise in mehreren Indizes verwendet wird und Sie eine `UPDATE`-Anweisung ausführen, durch die Daten in dieser Spalte geändert werden, müssen alle Indizes, die diese Spalte enthalten, sowie die Spalte in der zugrunde liegenden Basistabelle (Heap oder gruppierter Index) ebenfalls aktualisiert werden.  
  
    -   Vermeiden Sie die zu starke Indizierung häufig aktualisierter Tabellen, und halten Sie die Indizes schmal, d. h., verwenden Sie so wenig Spalten wie möglich.  
  
    -   Verwenden Sie viele Indizes, um die Abfrageleistung für Tabellen zu verbessern, die geringe Updateanforderungen und große Datenmengen aufweisen. Eine große Anzahl an Indizes kann die Leistung von Abfragen steigern, durch die keine Daten geändert werden (z. B. SELECT-Anweisungen), da der Abfrageoptimierer aus einer größeren Anzahl an Indizes auswählen kann, um die beste Methode für den schnellstmöglichen Zugriff zu ermitteln.  
  
-   Das Indizieren kleiner Tabellen ist häufig nicht die optimale Methode, da der Abfrageoptimierer in diesem Fall mitunter mehr Zeit benötigt, um die Daten über einen Index zu suchen, als für die Durchführung eines einfachen Tabellenscans erforderlich wäre. Aus diesem Grund werden Indizes für kleine Tabellen möglicherweise niemals verwendet, müssen jedoch trotzdem verwaltet werden, wenn sich Daten in der Tabelle ändern.  
  
-   Indizes für Sichten können zu erheblichen Leistungsverbesserungen führen, wenn die Sicht Aggregationen, Tabellenjoins oder eine Kombination aus Aggregationen und Joins enthält. Es ist nicht erforderlich, dass in der Abfrage explizit auf die jeweilige Sicht verwiesen wird, damit der Abfrageoptimierer die Sicht verwendet.  
  
-   Verwenden Sie den Datenbankoptimierungsratgeber, um die Datenbank zu analysieren und Indexempfehlungen zu erhalten. Weitere Informationen finden Sie unter [Database Engine Tuning Advisor](../relational-databases/performance/database-engine-tuning-advisor.md).  
  
### <a name="query-considerations"></a>Überlegungen zu Abfragen  
 Beachten Sie beim Entwerfen eines Indexes die folgenden Abfragerichtlinien:  
  
-   Erstellen Sie nicht gruppierte Indizes für die Spalten, die häufig in Prädikaten und Joinbedingungen in Abfragen verwendet werden. Dies sind Ihre SARGable<sup>1</sup>-Spalten. Sie sollten jedoch keine unnötigen Spalten hinzufügen. Wenn Sie zu viele Indexspalten hinzufügen, kann sich dies negativ auf den Speicherplatz und die Indexverwaltungsleistung auswirken.  
  
-   Abdeckende Indizes können die Abfrageleistung steigern, weil alle Daten im Index selbst enthalten sind, die die Anforderungen der Abfrage erfüllen. Auf diese Weise muss nur auf die Indexseiten und nicht auf die Datenseiten der Tabelle oder des gruppierten Indexes verwiesen werden, um die abgefragten Daten abzurufen, wodurch der Umfang der E/A-Operationen des Datenträgers verringert wird. Eine Abfrage der Spalten **a** und **b** für eine Tabelle, die einen zusammengesetzten Index besitzt, der für die Spalten **a**, **b** und **c** erstellt wurde, kann die angegebenen Daten ausschließlich aus dem Index abrufen.  

    > [!IMPORTANT]
    > Abdeckende Indizes sind die Bezeichnung für einen [nicht gruppierten Index](#nonclustered-index-architecture), der ein oder mehrere ähnliche Abfrageergebnisse direkt ohne Zugriff auf seine Basistabelle auflöst, und ohne dass Suchen erforderlich sind.
    > Derartige Indizes verfügen über alle erforderlichen Nicht-[SARGable](#sargable)-Spalten auf Blattebene. Das bedeutet, dass die von der SELECT-Klausel zurückgegebenen Spalten und alle WHERE- und JOIN-Argumente durch den Index abgedeckt werden.
    > Es gibt potenziell viel weniger eingehenden und ausgehenden Datenverkehr, um die Abfrage auszuführen, wenn der Index im Vergleich zu den Zeilen und Spalten in der Tabelle selbst nah genug gefasst ist, was bedeutet, dass es sich um eine echte Teilmenge der gesamten Spalten handelt. Ziehen Sie abdeckende Indizes in Betracht, wenn Sie einen kleinen Teil einer großen Tabelle auswählen, und wenn dieser kleine Teil durch ein festes Prädikat definiert ist. Ein Beispiel hierfür sind [Sparsespalten](../relational-databases/tables/use-sparse-columns.md), die nur wenige Werte enthalten, die nicht NULL sind.
    
-   Schreiben Sie Abfragen, die möglichst viele Zeilen in einer einzigen Anweisung einfügen oder ändern, anstatt mehrere Abfragen zum Aktualisieren der gleichen Zeilen zu verwenden. Wenn nur eine Anweisung verwendet wird, kann der Index optimal verwaltet werden.  
  
-   Werten Sie den Abfragetyp sowie die Art der Verwendung von Spalten in der Abfrage aus. Eine Spalte, die in einem Abfragetyp für genaue Übereinstimmung verwendet wird, ist z. B. ein guter Kandidat für einen nicht gruppierten oder gruppierten Index.

<a name="sargable"></a><sup>1</sup> Der Begriff „SARGable“ in relationalen Datenbanken bezieht sich auf ein suchargumentfähiges Prädikat (**S** earch **ARG** ument-**able**), das mithilfe eines Indexes die Ausführung der Abfrage beschleunigen kann.
  
### <a name="column-considerations"></a>Überlegungen zu Spalten  
 Beachten Sie beim Entwerfen eines Indexes die folgenden Spaltenrichtlinien:  
  
-   Wählen Sie für gruppierte Indizes einen kurzen Indexschlüssel aus. Gruppierte Indizes bieten darüber hinaus den Vorteil, dass sie für eindeutige oder Nicht-NULL-Spalten erstellt werden.  
  
-   Die Datentypen **ntext**, **text**, **image**, **varchar(max)** , **nvarchar(max)** und **varbinary(max)** können nicht als Indexschlüsselspalten angegeben werden. **varchar(max)** , **nvarchar(max)** , **varbinary(max)** und **xml** -Datentypen können jedoch als Nichtschlüssel-Indexspalten in einen nicht gruppierten Index aufgenommen werden. Weitere Informationen finden Sie im Abschnitt [Index mit eingeschlossenen Spalten](#Included_Columns)in diesem Handbuch.  
  
-   Ein **xml** -Datentyp kann nur in einem XML-Index eine Schlüsselspalte sein. Weitere Informationen finden Sie unter [XML-Indizes &#40;SQL Server&#41;](../relational-databases/xml/xml-indexes-sql-server.md). Mit SQL Server 2012 SP1 wird ein neuer XML-Indextyp eingeführt, der als selektiver XML-Index bezeichnet wird. Durch diesen neuen Index kann die Abfrageleistung bei Daten verbessert werden, die als XML in SQL Server gespeichert sind. Das sorgt für eine schnellere Indizierung großer XML-Datenmengen und für höhere Skalierbarkeit, indem die Speicherkosten des Indexes gesenkt werden. Weitere Informationen finden Sie unter [Selektive XML-Indizes &#40;SXI&#41;](../relational-databases/xml/selective-xml-indexes-sxi.md).  
  
-   Überprüfen Sie die Eindeutigkeit der Spalten. Ein eindeutiger Index anstelle eines nicht eindeutigen Indexes für dieselbe Kombination von Spalten stellt zusätzliche Informationen für den Abfrageoptimierer bereit, die den Index wertvoller machen. Weitere Informationen finden Sie unter [Richtlinien zum Entwerfen eindeutiger Indizes](#Unique) in diesem Handbuch.  
  
-   Überprüfen Sie die Datenverteilung in der Spalte. Häufig dauert eine Abfrage deshalb sehr lange, weil eine indizierte Spalte mit wenigen eindeutigen Werten verwendet wird oder weil sie einen Join mit einer solchen Spalte durchführt. Hierbei handelt es sich um ein grundlegendes Problem von Daten und Abfragen, das in der Regel nicht gelöst werden kann, ohne die betreffende Situation zu identifizieren. Beispielsweise wird ein physisches Telefonbuch, das alphabetisch nach dem Nachnamen sortiert ist, das Suchen eines Teilnehmers nicht vereinfachen, wenn alle Teilnehmer des Ortsnetzes Smith oder Jones heißen. Weitere Informationen zur Datenverteilung finden Sie unter [Statistics](../relational-databases/statistics/statistics.md).  
  
-   Verwenden Sie für Spalten mit fest definierten Teilmengen, z. B. Sparsespalten, Spalten, die größtenteils NULL-Werte enthalten, Spalten mit Wertekategorien und Spalten mit verschiedenen Wertebereichen, gefilterte Indizes. Ein gut entworfener gefilterter Index kann die Abfrageleistung verbessern, die Indexwartungskosten reduzieren und den Speicheraufwand verringern.  
  
-   Berücksichtigen Sie die Reihenfolge der Spalten, wenn der Index mehrere Spalten enthalten soll. Die Spalte, die in der WHERE-Klausel in einer Gleich- (=), Größer als- (>), Kleiner als- (<) oder BETWEEN-Suchbedingung verwendet oder in einen Join eingeschlossen wird, sollte an erster Stelle stehen. Die Reihenfolge zusätzlicher Spalten sollte basierend auf dem Grad ihrer Eindeutigkeit, d. h. von der eindeutigsten Spalte absteigend zu der am wenigsten eindeutigen Spalte, ausgewählt werden.  
  
     Wenn der Index z. B. als `LastName`, `FirstName` definiert ist, ist der Index hilfreich, wenn das Suchkriterium `WHERE LastName = 'Smith'` oder `WHERE LastName = Smith AND FirstName LIKE 'J%'`lautet. Der Abfrageoptimierer verwendet den Index jedoch nicht für eine Abfrage, die nur nach `FirstName (WHERE FirstName = 'Jane')`sucht.  
  
-   Ziehen Sie das Indizieren berechneter Spalten in Betracht. Weitere Informationen finden Sie unter [Indexes on Computed Columns](../relational-databases/indexes/indexes-on-computed-columns.md).  
  
### <a name="index-characteristics"></a>Indexmerkmale  
 Wenn sich herausgestellt hat, dass ein Index für eine Abfrage geeignet ist, können Sie den Indextyp auswählen, der für die jeweilige Situation am besten geeignet ist. Die Indexmerkmale beziehen sich z. B. auf Folgendes:  
  
-   Gruppiert im Vergleich zu nicht gruppiert  
-   Eindeutig im Vergleich zu nicht eindeutig  
-   Einspaltig im Vergleich zu mehrspaltig  
-   Aufsteigende oder absteigende Reihenfolge in den Spalten des Indexes  
-   Tabellenindizes im Vergleich zu gefilterten für nicht gruppierte Indizes  
-   Vergleich von Columnstore und Rowstore
-   Vergleich von Hashindizes und nicht gruppierten Indizes für speicheroptimierte Tabellen
  
Um die Indexleistung- oder -wartung zu optimieren, können Sie durch das Festlegen einer Option wie z. B. FILLFACTOR auch die Ausgangsspeichermerkmale des Indexes anpassen. Zudem können Sie den Speicherort des Indexes mithilfe von Dateigruppen oder Partitionsschemas festlegen, um die Leistung zu optimieren.  
  
###  <a name="index-placement-on-filegroups-or-partitions-schemes"></a><a name="Index_placement"></a> Indexplatzierung in Dateigruppen oder Partitionsschemas  
 Bei der Entwicklung einer Indexentwurfsstrategie sollten Sie die Platzierung der Indizes in den Dateigruppen berücksichtigen, die der Datenbank zugeordnet sind. Das sorgfältige Auswählen der Dateigruppe oder des Partitionsschemas kann die Abfrageleistung verbessern.  
  
 Standardmäßig werden Indizes in derselben Dateigruppe wie die Basistabelle gespeichert, für die der Index erstellt wird. Ein nicht partitionierter, gruppierter Index und die Basistabelle befinden sich immer in der gleichen Dateigruppe. Sie können jedoch folgende Aktionen ausführen:  
  
-   Erstellen nicht gruppierter Indizes in einer anderen Dateigruppe als der Dateigruppe der Basistabelle oder des gruppierten Indexes.  
-   Partitionieren von gruppierten und nicht gruppierten Indizes, damit diese mehrere Dateigruppen umfassen.  
-   Verschieben einer Tabelle aus einer Dateigruppe in eine andere durch Löschen des gruppierten Index und Angeben einer neuen Dateigruppe oder eines Partitionsschemas in der MOVE TO-Klausel der DROP INDEX-Anweisung oder durch Verwenden der CREATE INDEX-Anweisung mit der DROP_EXISTIN-Klausel.  
  
Wird der nicht gruppierte Index in einer anderen Dateigruppe erstellt, können Leistungsvorteile erzielt werden, wenn die Dateigruppen verschiedene physische Laufwerke mit eigenen Controllern verwenden. Daten und Indexinformationen können dann parallel von mehreren Leseköpfen gelesen werden. Werden beispielsweise `Table_A` in Dateigruppe `f1` und `Index_A` in Dateigruppe `f2` von derselben Abfrage verwendet, können Leistungsvorteile erzielt werden, da beide Dateigruppen konfliktfrei vollständig verwendet werden können. Wenn `Table_A` von der Abfrage durchsucht wird, ohne dass auf `Index_A` verwiesen wird, wird jedoch nur Dateigruppe `f1` verwendet. In diesem Fall ist kein Leistungsgewinn zu verzeichnen.  
  
Da jedoch nicht vorhersehbar ist, welche Zugriffsart wann erfolgt, ist die Entscheidung für das Verteilen der Tabellen und Indizes auf alle Dateigruppen häufig die bessere Lösung. So ist sichergestellt, dass unabhängig von der Art des Datenzugriffs auf alle Datenträger zugegriffen wird, da alle Daten und Indizes gleichmäßig auf alle Datenträger verteilt sind. Diese Lösung ist auch aus Systemadministratorensicht einfacher.  
  
#### <a name="partitions-across-multiple-filegroups"></a>Partitionen über mehrere Dateigruppen  
 Sie können auch das Partitionieren von gruppierten und nicht gruppierten Indizes über mehrere Dateigruppen hinweg in Betracht ziehen. Partitionierte Indizes werden horizontal oder nach Zeile basierend auf einer Partitionsfunktion partitioniert. Die Partitionsfunktion definiert, wie jede einzelne Zeile einer Gruppe von Partitionen auf der Grundlage der Werte bestimmter Spalten zugeordnet wird, die als Partitionierungsspalten bezeichnet werden. Ein Partitionsschema gibt die Zuordnung dieser Partitionen zu einer Sammlung von Dateigruppen an.  
  
 Das Partitionieren eines Indexes kann die folgenden Vorteile bieten:  
  
-   Bereitstellen skalierbarer Systeme, die die Verwaltbarkeit großer Indizes vereinfachen. OLTP-Systeme können z. B. partitionsabhängige Anwendungen implementieren, die große Indizes verwalten können.  
  
-   Schnellere und effizientere Ausführung von Abfragen. Wenn Abfragen auf mehrere Partitionen eines Indexes zugreifen, kann der Abfrageoptimierer einzelne Partitionen gleichzeitig verarbeiten und Partitionen ausschließen, die nicht von der Abfrage betroffen sind.  
  
Weitere Informationen finden Sie unter [partitionierte Tabellen und Indizes](../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
###  <a name="index-sort-order-design-guidelines"></a><a name="Sort_Order"></a> Entwurfsrichtlinien zur Sortierreihenfolge von Indizes  
 Beim Definieren von Indizes sollten Sie berücksichtigen, ob die Daten für die Indexschlüsselspalte in aufsteigender oder absteigender Reihenfolge gespeichert werden sollen. Die Standardreihenfolge ist aufsteigend. Hierbei wird auch die Kompatibilität mit früheren Versionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]beibehalten. Die Syntax der CREATE INDEX-, CREATE TABLE- und ALTER TABLE-Anweisungen unterstützt die Schüsselwörter ASC (aufsteigend) und DESC (absteigend) für einzelne Spalten in Indizes und Einschränkungen:  
  
 Das Angeben der Reihenfolge, in der die Schlüsselwerte in einem Index gespeichert werden, ist sinnvoll, wenn Abfragen, die auf die Tabelle verweisen, über ORDER BY-Klauseln verfügen, die verschiedene Richtungen für die Schlüsselspalten in dem entsprechenden Index angeben. In diesen Fällen kann der Index dafür sorgen, dass kein SORT-Operator mehr im Abfrageplan benötigt wird, wodurch die Abfrage effizienter wird. Die Mitarbeiter der Einkaufsabteilung von [!INCLUDE[ssSampleDBCoFull](../includes/sssampledbcofull-md.md)] müssen beispielsweise die Qualität der Produkte, die sie von den Anbietern kaufen, bewerten. Die Einkäufer sind vor allem interessiert daran, Produkte zu finden, die von diesen Anbietern gesendet wurden und die eine hohe Ablehnungsrate aufweisen. Wie in der folgenden Abfrage gezeigt, muss zum Abrufen der Daten, die diese Anforderung erfüllen, die `RejectedQty` -Spalte in der `Purchasing.PurchaseOrderDetail` -Tabelle in absteigender Reihenfolge (von groß nach klein) sortiert werden, und die `ProductID` -Spalte muss in aufsteigender Reihenfolge (von klein nach groß) sortiert werden.  
  
```sql  
SELECT RejectedQty, ((RejectedQty/OrderQty)*100) AS RejectionRate,  
    ProductID, DueDate  
FROM Purchasing.PurchaseOrderDetail  
ORDER BY RejectedQty DESC, ProductID ASC;  
```  
  
 Der folgende Ausführungsplan für diese Abfrage zeigt, dass der Abfrageoptimierer einen SORT-Operator verwendet hat, um das Resultset in der durch die ORDER BY-Klausel angegebenen Reihenfolge zurückzugeben.  
  
 ![Diagramm für einen Ausführungsplan für diese Abfrage, das zeigt, dass der Abfrageoptimierer einen SORT-Operator verwendet hat, um das Resultset in der durch die ORDER BY-Klausel festgelegten Reihenfolge zurückzugeben](../relational-databases/media/indexsort1.gif)
  
 Falls ein Index mit Schlüsselspalten erstellt wird, die mit jenen in der ORDER BY-Klausel in der Abfrage übereinstimmen, kann der SORT-Operator im Abfrageplan gelöscht werden, wodurch der Abfrageplan effizienter wird.  
  
```sql  
CREATE NONCLUSTERED INDEX IX_PurchaseOrderDetail_RejectedQty  
ON Purchasing.PurchaseOrderDetail  
    (RejectedQty DESC, ProductID ASC, DueDate, OrderQty);  
```  
  
 Nachdem die Abfrage erneut ausgeführt wurde, zeigt folgender Ausführungsplan, dass der SORT-Operator gelöscht wurde und der neu erstellte nicht gruppierte Index verwendet wird.  
  
 ![Diagramm für einen Ausführungsplan, das zeigt, dass der SORT-Operator entfernt wurde und der neu erstellte nicht gruppierte Index verwendet wird](../relational-databases/media/insertsort2.gif)
  
 [!INCLUDE[ssDE](../includes/ssde-md.md)] bewegt sich in beide Richtungen gleichermaßen effizient. Ein als `(RejectedQty DESC, ProductID ASC)` definierter Index kann nach wie vor für eine Abfrage verwendet werden, in der die Sortierreihenfolge der Spalten in der ORDER BY-Klausel reserviert sind. Eine Abfrage mit der ORDER BY-Klausel `ORDER BY RejectedQty ASC, ProductID DESC` kann den Index beispielsweise verwenden.  
  
 Die Sortierreihenfolge kann nur für die Schlüsselspalten im Index angegeben werden. Die Katalogsicht [sys.index_columns](../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md) und die INDEXKEY_PROPERTY-Funktion melden, ob eine Indexspalte in aufsteigender oder absteigender Reihenfolge gespeichert wird.  

## <a name="metadata"></a>Metadaten  
Verwenden Sie diese Metadatenansichten, um die Attribute des Indizes anzuzeigen. Weitere Informationen zur Architektur sind in einigen Ansichten eingebettet.

> [!NOTE]
> Bei Columnstore-Indizes werden alle Spalten in den Metadaten als eingeschlossene Spalten gespeichert. Der Columnstore-Index weist keine Schlüsselspalten auf.  

:::row:::
    :::column:::
        [sys.indexes &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.index_columns &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.partitions &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.internal_partitions &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.column_store_segments &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.column_store_row_groups &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_column_store_row_group_operational_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.dm_db_column_store_row_group_physical_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_column_store_object_pool &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-column-store-object-pool-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_column_store_row_group_operational_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_hash_index_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-hash-index-stats-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.dm_db_xtp_index_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_object_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-object-stats-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.dm_db_xtp_nonclustered_index_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-nonclustered-index-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_table_memory_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-table-memory-stats-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.hash_indexes (Transact-SQL)](../relational-databases/system-catalog-views/sys-hash-indexes-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.memory_optimized_tables_internal_attributes (Transact-SQL)](../relational-databases/system-catalog-views/sys-memory-optimized-tables-internal-attributes-transact-sql.md)
    :::column-end:::
:::row-end:::

##  <a name="clustered-index-design-guidelines"></a><a name="Clustered"></a> Richtlinien für den Entwurf gruppierter Indizes  
 Gruppierte Indizes sortieren und speichern die Datenzeilen in den Tabellen basierend auf ihren Schlüsselwerten. Pro Tabelle kann nur ein gruppierter Index vorhanden sein, da die Datenzeilen nur in einer Reihenfolge sortiert werden können. Mit wenigen Ausnahmen sollte für jede Tabelle ein gruppierter Index für die Spalte(n) definiert werden, auf die Folgendes zutrifft:  
  
-   Sie kann für häufig verwendete Abfragen verwendet werden.  
  
-   Sie stellt einen hohen Grad an Eindeutigkeit bereit.  
  
    > [!NOTE]  
    > Wenn Sie eine PRIMARY KEY-Einschränkung erstellen, wird automatisch ein eindeutiger Index für die Spalte(n) erstellt. Standardmäßig ist dieser Index gruppiert; Sie können jedoch auch einen nicht gruppierten Index angeben, wenn Sie die Einschränkung erstellen.  
  
-   Sie kann in Bereichsabfragen verwendet werden.  
  
Wenn der gruppierte Index nicht mit der `UNIQUE`-Eigenschaft erstellt wird, fügt die [!INCLUDE[ssDE](../includes/ssde-md.md)] der Tabelle automatisch eine 4 Byte große Uniqueifier-Spalte hinzu. Falls erforderlich, fügt das [!INCLUDE[ssDE](../includes/ssde-md.md)] einer Zeile automatisch einen uniqueifier-Wert hinzu, um jeden Schlüssel eindeutig zu machen. Diese Spalte und ihre Werte werden intern verwendet und können durch Benutzer nicht angezeigt werden. Der Zugriff durch Benutzer auf diese ist ebenfalls nicht möglich.  
  
### <a name="clustered-index-architecture"></a>Architektur gruppierter Indizes  
 In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sind Indizes in Form von B-Strukturen aufgebaut. Jede Seite in der B-Struktur eines Indexes wird als Indexknoten bezeichnet. Der oberste Knoten der B-Struktur wird als Stammknoten bezeichnet. Die unteren Knoten im Index werden als Blattknoten bezeichnet. Alle anderen Indexebenen zwischen dem Stamm- und den Blattknoten werden zusammenfassend als Zwischenebenen bezeichnet. In einem gruppierten Index enthalten die Blattknoten die Datenseiten der zugrunde liegenden Tabelle. Die Stamm- und Zwischenebenenknoten enthalten Indexseiten, in denen Indexzeilen enthalten sind. Jede Indexzeile enthält einen Schlüsselwert und einen Zeiger auf eine Seite einer Zwischenebene in der B-Struktur oder auf eine Datenzeile in der Blattebene des Indexes. Die Seiten auf jeder Ebene des Indexes sind durch eine doppelt verknüpfte Liste miteinander verknüpft.  
  
 Gruppierte Indizes besitzen eine Zeile in [sys.partitions](../relational-databases/system-catalog-views/sys-partitions-transact-sql.md), wobei **index_id** = 1 für jede Partition, die vom Index verwendet wird. Standardmäßig besitzt ein gruppierter Index eine Partition. Wenn ein gruppierter Index über mehrere Partitionen verfügt, besitzt jede Partition eine B-Struktur, die die Daten für diese bestimmte Partition enthält. Wenn ein gruppierter Index z. B. vier Partitionen besitzt, sind vier B-Strukturen vorhanden, eine in jeder Partition.  
  
 Abhängig von den Datentypen im gruppierten Index weist jede gruppierte Indexstruktur eine oder mehrere Zuordnungseinheiten auf, in denen die Daten für eine bestimmte Partition gespeichert und verwaltet werden. Jeder gruppierte Index weist mindestens eine IN_ROW_DATA-Zuordnungseinheit pro Partition auf. Der gruppierte Index besitzt außerdem eine *LOB_DATA*-Zuordnungseinheit pro Partition, wenn LOB-Spalten (Large Object) vorhanden sind. Außerdem ist eine *ROW_OVERFLOW_DATA*-Zuordnungseinheit pro Partition vorhanden, wenn der Index Spalten variabler Länge aufweist, die die Zeilengrößenbeschränkung von 8.060 Byte übersteigen.  
  
 Die Seiten in der Datenkette und die darin enthaltenen Zeilen werden anhand des Werts des Schlüssels des gruppierten Indexes angeordnet. Jede Einfügung wird an der Position vorgenommen, die der Schlüsselwert der eingefügten Zeile in der Reihenfolge vorhandener Zeilen einnimmt.  
  
 Die folgende Abbildung veranschaulicht die Struktur eines gruppierten Indexes in einer einzelnen Partition.  
 
 ![Diagramm: Struktur eines gruppierten Index in einer einzelnen Partition](../relational-databases/media/bokind2.gif)  
  
### <a name="query-considerations"></a>Überlegungen zu Abfragen  
 Bevor Sie gruppierte Indizes erstellen, sollten Sie sich überlegen, wie der Zugriff auf die Daten erfolgt. Einen gruppierten Index können Sie für Abfragen verwenden, die die folgenden Aktionen durchführen:  
  
-   Sie geben einen Wertebereich zurück, indem z.B. folgende Operatoren verwendet werden: `BETWEEN`, >, >=, < und <=.  
  
     Sobald mithilfe des gruppierten Indexes die Zeile mit dem ersten Wert gefunden wird, ist sichergestellt, dass Zeilen mit nachfolgenden Indexwerten physisch benachbart sind. Wenn mit einer Abfrage z. B. Datensätze aus einem Bereich von Verkaufsauftragsnummern abgerufen werden, bietet ein gruppierter Index für die `SalesOrderNumber` -Spalte die Möglichkeit, schnell die Zeile zu finden, die die Start-Verkaufsauftragsnummer enthält, und dann alle nachfolgenden Zeilen in der Tabelle abzurufen, bis die letzte Verkaufsauftragsnummer erreicht ist.  
  
-   Zurückgeben umfangreicher Resultsets.  
  
-   Sie verwenden `JOIN`-Klauseln. In der Regel handelt es sich dabei um Fremdschlüsselspalten.  
  
-   Sie verwenden `ORDER BY`- oder `GROUP BY`-Klauseln.  
  
     Durch einen Index für Spalten, die in der ORDER BY- oder GROUP BY-Klausel angegeben werden, entfällt ggf. die Notwendigkeit für [!INCLUDE[ssDE](../includes/ssde-md.md)] , die Daten zu sortieren, da die Zeilen bereits sortiert sind. Die Abfrageleistung wird somit verbessert.  
  
### <a name="column-considerations"></a>Überlegungen zu Spalten  
 Die Definition des gruppierten Indexschlüssels sollte im Allgemeinen so wenig Spalten wie möglich umfassen. Ziehen Sie Spalten in Betracht, auf die ein oder mehrere der folgenden Merkmale zutreffen:  
  
-   Sie sind eindeutig oder enthalten zahlreiche unterschiedliche Werte.  
  
    Eine Mitarbeiter-ID identifiziert einen Mitarbeiter z. B. eindeutig. Ein gruppierter Index oder eine [PRIMARY KEY](../relational-databases/tables/create-primary-keys.md)-Einschränkung für die `EmployeeID`-Spalte verbessert die Leistung von Abfragen, durch die Mitarbeiterinformationen anhand der Mitarbeiter-ID gesucht werden. Alternativ kann ein gruppierter Index für `LastName`, `FirstName`und `MiddleName` erstellt werden, weil Mitarbeiterdatensätze häufig auf diese Weise gruppiert und abgefragt werden und die Kombination dieser Spalten jedoch trotzdem einen hohen Differenzierungsgrad bietet. 

    > [!TIP]
    > Wenn dies nicht anders angegeben ist, erstellt [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] beim Erstellen einer [PRIMARY KEY](../relational-databases/tables/create-primary-keys.md)-Einschränkung einen [gruppierten Index](#Clustered), um diese Einschränkung zu unterstützen.
    > Obwohl ein *[uniqueidentifier](../t-sql/data-types/uniqueidentifier-transact-sql.md)* -Datentyp verwendet werden kann, um Eindeutigkeit als PRIMARY KEY zu erzwingen, handelt es sich dabei nicht um einen effizienten Gruppierungsschlüssel.
    > Wenn Sie einen *uniqueidentifier*-Datentyp als PRIMARY KEY verwenden, wird empfohlen, diesen als nicht gruppierten Index zu erstellen und eine andere Spalte (z.B. `IDENTITY`) zu verwenden, um den gruppierten Index zu erstellen.   
  
-   Der Zugriff auf sie erfolgt sequenziell.  
  
    Durch eine Produkt-ID werden Produkte in der `Production.Product` -Tabelle der [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] -Datenbank beispielsweise eindeutig identifiziert. Abfragen, in denen eine sequenzielle Suche angegeben wird, z. B. `WHERE ProductID BETWEEN 980 and 999`, profitieren von einem gruppierten Index für `ProductID`. Die Ursache liegt darin, dass die Zeilen für diese Schlüsselspalte in sortierter Reihenfolge gespeichert werden.  
  
-   Sie sind als `IDENTITY` definiert.  
  
-   Sie wird häufig verwendet, um die aus einer Tabelle abgerufenen Daten zu sortieren.  
  
    Es kann sinnvoll sein, die Tabelle für die betreffende Spalte zu gruppieren (d. h., physisch zu sortieren), damit nicht bei jeder Abfrage der Spalte Kosten für einen Sortiervorgang entstehen.  
  
 Die Verwendung eines gruppierten Indexes ist nicht empfehlenswert, wenn die Spalten die folgenden Merkmale aufweisen:  
  
-   Spalten, die häufig geändert werden.  
  
    In diesem Fall würde die gesamte Zeile verschoben, weil [!INCLUDE[ssDE](../includes/ssde-md.md)] die physische Reihenfolge der Datenwerte der Zeile aufrechterhalten muss. Dieser Aspekt sollte insbesondere bei Systemen berücksichtigt werden, in denen Transaktionsverarbeitung in großem Umfang erfolgt und Daten nur selten von Dauer sind.  
  
-   Ausführliche Schlüssel.  
  
    Ausführliche Schlüssel sind aus mehreren Spalten oder mehreren großen Spalten zusammengesetzt. Die Schlüsselwerte aus dem gruppierten Index werden von allen nicht gruppierten Indizes als Suchschlüssel verwendet. Alle nicht gruppierten Indizes, die für dieselbe Tabelle definiert werden, sind erheblich größer, da die Einträge des nicht gruppierten Indexes den Gruppierungsschlüssel sowie die Schlüsselspalten enthalten, die für diesen nicht gruppierten Index definiert wurden.  
  
##  <a name="nonclustered-index-design-guidelines"></a><a name="Nonclustered"></a> Entwurfsrichtlinien für einen nicht gruppierten Index  
 Ein nicht gruppierter Index enthält Indexschlüsselwerte sowie Zeilenlokatoren, die auf den Speicherort der Tabellendaten verweisen. Sie können mehrere nicht gruppierte Indizes für eine Tabelle oder eine indizierte Sicht erstellen. Im Allgemeinen sollten nicht gruppierte Indizes so entworfen werden, dass sich die Leistung häufig verwendeter Abfragen verbessert, die nicht vom gruppierten Index abgedeckt werden.  
  
 Vergleichbar mit der Art und Weise wie Sie einen Index in einem Buch verwenden, sucht der Abfrageoptimierer nach einem Datenwert, indem er den nicht gruppierten Index durchsucht, um so den Speicherort des Datenwerts in der Tabelle zu ermitteln. Anschließend werden die Daten direkt von diesem Speicherort abgerufen. Aus diesem Grund sind nicht gruppierte Indizes optimal für Abfragen geeignet, die nach genauen Übereinstimmungen suchen, da der Index Einträge enthält, die in der Tabelle den genauen Speicherort der Datenwerte beschreiben, die in den Abfragen gesucht werden. Wenn beispielsweise die `HumanResources. Employee` -Tabelle nach sämtlichen Mitarbeitern abgefragt werden soll, die einem bestimmten Abteilungsleiter unterstehen, verwendet der Abfrageoptimierer möglicherweise den nicht gruppierten Index `IX_Employee_ManagerID`; hier ist `ManagerID` die Schlüsselspalte. Der Abfrageoptimierer kann schnell alle Einträge im Index finden, die mit der angegebenen `ManagerID`übereinstimmen. Jeder Indexeinträg verweist auf die genaue Seite und Zeile in der Tabelle bzw. im gruppierten Index, in der die entsprechenden Daten zu finden sind. Nachdem der Abfrageoptimierer sämtliche Einträge im Index gefunden hat, kann er sich direkt zu der genauen Seite und Zeile begeben, um die Daten abzurufen.  
  
### <a name="nonclustered-index-architecture"></a>Architektur nicht gruppierter Indizes  
 Nicht gruppierte Indizes weisen dieselbe B-Baumstruktur auf wie gruppierte Indizes, mit Ausnahme der beiden folgenden signifikanten Unterschiede:  
  
-   Die Datenzeilen der zugrunde liegenden Tabelle werden nicht auf der Grundlage ihrer nicht gruppierten Schlüssel sortiert und gespeichert.  
  
-   Die Blattebene eines nicht gruppierten Indexes besteht aus Indexseiten anstelle von Datenseiten.  
  
Zeilenlokatoren in nicht gruppierten Indexzeilen bestehen entweder aus Zeigern, die auf jeweils eine Zeile zeigen, oder aus einem Schlüssel eines gruppierten Indexes für eine Zeile, wie im Folgenden erläutert:  
  
-   Wenn es sich bei der Tabelle um einen Heap handelt (d. h. sie hat keinen gruppierten Index), entspricht der Zeilenlokator einem Zeiger auf die Zeile. Der Zeiger setzt sich aus dem Dateibezeichner (ID), der Seitennummer und der Nummer der Zeile auf der Seite zusammen. Der ganze Zeiger wird als Zeilen-ID (RID) bezeichnet.  
  
-   Wenn die Tabelle einen gruppierten Index besitzt oder der Index für eine indizierte Sicht erstellt wurde, ist der Zeilenlokator der Schlüssel des gruppierten Indexes für die Zeile.  
  
Nicht gruppierte Indizes verfügen in [sys.partitions](../relational-databases/system-catalog-views/sys-partitions-transact-sql.md) für jede vom Index verwendete Partition über eine Zeile, deren **index_id** >1 ist. Standardmäßig besitzen nicht gruppierte Indizes nur eine Partition. Wenn ein nicht gruppierter Index mehrere Partitionen besitzt, weist jede Partition eine B-Baumstruktur auf, die die Indexzeilen der entsprechenden Partition enthält. Wenn ein nicht gruppierter Index beispielsweise vier Partitionen besitzt, gibt es vier B-Baumstrukturen – jeweils eine in jeder Partition.  
  
Abhängig von den Datentypen des nicht gruppierten Indexes erhält jede Struktur mindestens eine Zuordnungseinheit, in der die Daten einer bestimmten Partition gespeichert und verwaltet werden. Jeder nicht gruppierte Index besitzt also eine *IN_ROW_DATA*-Zuordnungseinheit pro Partition, die die B-Strukturseiten des Indexes speichert. Außerdem besitzt der nicht gruppierte Index eine *LOB_DATA*-Zuordnungseinheit, wenn er LOB-Spalten (Large Object) enthält. Darüber hinaus besitzt er eine *ROW_OVERFLOW_DATA*-Zuordnungseinheit pro Partition, wenn er Spalten variabler Länge enthält, die die Zeilengrößenbeschränkung von 8.060 Byte überschreiten.  
  
Die folgende Abbildung veranschaulicht die Struktur eines gruppierten Indexes einer einzigen Partition.  

![Diagramm: Struktur eines nicht gruppierten Index in einer einzelnen Partition](../relational-databases/media/bokind1a.gif)  
  
### <a name="database-considerations"></a>Überlegungen zu Datenbanken  
 Berücksichtigen Sie beim Entwerfen nicht gruppierter Indizes die Merkmale der jeweiligen Datenbank.  
  
-   Datenbanken oder Tabellen mit geringen Updateanforderungen, aber großem Datenvolumen, können von zahlreichen nicht gruppierten Indizes zur Verbesserung der Abfrageleistung profitieren. Mit gefilterten Indizes mit klar definierten Teilmengen von Daten können Sie im Vergleich zu nicht gruppierten Tabellenindizes die Abfrageleistung verbessern und den Aufwand für die Indexverwaltung und die Indexspeicherung reduzieren.  
  
     Entscheidungsunterstützungssystem-Anwendungen und -Datenbanken, die hauptsächlich schreibgeschützte Daten enthalten, können von zahlreichen nicht gruppierten Indizes profitieren. Der Abfrageoptimierer hat hinsichtlich des Ermittelns der schnellsten Zugriffsmethode eine größere Auswahl an Indizes, und da die Datenbankmerkmale auf einen geringen Updateaufwand schließen lassen, wird die Leistung durch die Indexwartung nicht beeinträchtigt.  
  
-   Bei Anwendungen und Datenbanken zur Onlinetransaktionsverarbeitung (OLTP) mit Tabellen, die umfassend aktualisiert werden, sollte ein Übermaß an Indizierung vermieden werden. Zusätzlich sollten die Indizes schmal sein, also so wenig Spalten wie möglich enthalten.  
  
     Eine große Anzahl an Indizes für eine Tabelle beeinträchtigt die Leistung von INSERT-, UPDATE-, DELETE- und MERGE-Anweisungen, da alle Indizes entsprechend angepasst werden müssen, sobald Daten in der Tabelle geändert werden.  
  
### <a name="query-considerations"></a>Überlegungen zu Abfragen  
 Bevor Sie nicht gruppierte Indizes erstellen, sollten Sie sich darüber im Klaren sein, wie der Zugriff auf Ihre Daten erfolgt. Erwägen Sie das Verwenden eines nicht gruppierten Indexes für Abfragen mit folgenden Attributen:  
  
-   Sie verwenden `JOIN`- oder `GROUP BY`-Klauseln.  
  
     Erstellen Sie mehrere nicht gruppierte Indizes für Spalten, die an Join- und Gruppierungsvorgängen beteiligt sind, sowie einen gruppierten Index für eine beliebige Fremdschlüsselspalte.  
  
-   Kein Zurückgeben großer Resultsets.  
  
     Erstellen Sie gefilterte Indizes für Abfragen, die eine klar definierte Zeilenteilmenge aus einer großen Tabelle zurückgeben. 
     
     > [!TIP] 
     > In der Regel stimmt die WHERE-Klausel der CREATE INDEX-Anweisung mit der WHERE-Klausel einer Abfrage überein, die gerade abgedeckt wird.  

-   Enthalten von Spalten, die häufig an Suchbedingungen einer Abfrage beteiligt sind (z. B. WHERE-Klausel), die genaue Übereinstimmungen zurückgeben.  

    > [!TIP]
    > Bedenken Sie die Kosten gegenüber dem Nutzen beim Hinzufügen neuer Indizes. Es kann besser sein, zusätzliche Abfrageanforderungen in einen bestehenden Index zu konsolidieren. Erwägen Sie beispielsweise, eine oder zwei zusätzliche Spalten auf Blattebene zu einem bestehenden Index hinzuzufügen, wenn er die Abdeckung mehrerer kritischer Abfragen erlaubt, anstatt einen genau abdeckenden Index pro kritischer Abfrage zu haben.
    
### <a name="column-considerations"></a>Überlegungen zu Spalten  
 Ziehen Sie Spalten mit einem oder mehrerer dieser Attribute in Betracht:  
  
-   Abdecken der Abfrage.  
  
     Leistungsgewinne werden erzielt, wenn der Index alle Spalten in der Abfrage enthält. Der Abfrageoptimierer kann alle Spaltenwerte im Index finden; da auf Daten der Tabelle oder des gruppierten Indexes nicht zugegriffen wird, kommt es zu weniger Datenträger-E/A-Vorgängen. Verwenden Sie einen Index mit [eingeschlossenen Spalten](#Included_Columns), um mehrere Spalten abzudecken, anstatt einen breiten Indexschlüssel zu erstellen.  
  
     Wenn die Tabelle einen gruppierten Index aufweist, werden die im gruppierten Index definierten Spalten automatisch an das Ende sämtlicher nicht gruppierter Indizes für die Tabelle angefügt. Auf diese Weise kann eine abgedeckte Abfrage erstellt werden, ohne dass die Spalten des gruppierten Indexes in der Definition des nicht gruppierten Indexes angegeben werden. Wenn eine Tabelle beispielsweise einen gruppierten Index für Spalte `C`besitzt, weist ein nicht gruppierter Index für die Spalten `B` und `A` die Schlüsselwertspalten `B`, `A`und `C`auf.  
      
-   Große Anzahl unterschiedlicher Werte, beispielsweise eine Kombination aus Nachname und Vorname, wenn für andere Spalten ein gruppierter Index verwendet wird.  
  
     Wenn nur sehr wenige unterschiedliche Werte vorhanden sind, beispielsweise nur 1 und 0, verwenden die meisten Abfragen den Index nicht, da ein Tabellenscan im Allgemeinen effizienter ist. Für diesen Datentyp sollten Sie einen gefilterten Index anhand eines anderen Werts erstellen, der in weniger Zeilen vorkommt. Beispiel: Wenn die meisten Werte 0 sind, kann der Abfrageoptimierer einen gefilterten Index für die Zeilen verwenden, die den Wert 1 enthalten.  
  
####  <a name="use-included-columns-to-extend-nonclustered-indexes"></a><a name="Included_Columns"></a> Verwenden eingeschlossener Spalten, um nicht gruppierte Indizes zu erweitern  
 Sie können die Funktionen nicht gruppierter Indizes erweitern, indem Sie der Blattebene des nicht gruppierten Indexes Nichtschlüsselspalten hinzufügen. Indem Sie Nichtschlüsselspalten einschließen, erstellen Sie nicht gruppierte Indizes, die eine größere Anzahl von Abfragen abdecken. Dies ist der Fall, weil Nichtschlüsselspalten die folgenden Vorteile aufweisen:  
  
-   Es kann sich um Datentypen handeln, die als Indexschlüsselspalten nicht zulässig sind.  
  
-   Sie werden von [!INCLUDE[ssDE](../includes/ssde-md.md)] beim Berechnen der Indexschlüsselspalten oder Indexschlüsselgröße nicht berücksichtigt.  
  
 Ein Index mit eingeschlossenen Nichtschlüsselspalten kann die Abfrageleistung erheblich steigern, wenn alle Spalten in der Abfrage in den Index als Schlüssel- oder Nichtschlüsselspalten eingeschlossen werden. Leistungsvorteile werden erzielt, weil der Abfrageoptimierer alle Spaltenwerte im Index finden kann; auf Daten der Tabelle oder des gruppierten Indexes wird nicht zugegriffen, sodass als Ergebnis weniger Datenträger-E/A-Vorgänge auftreten.  
  
> [!NOTE]  
> Wenn ein Index alle Spalten enthält, auf die die Abfrage verweist, wird dies normalerweise als Abdecken der Abfrage bezeichnet.  
  
 Die Schlüsselspalten werden auf allen Ebenen des Indexes gespeichert, die Nichtschlüsselspalten nur auf der Blattebene.  
  
##### <a name="using-included-columns-to-avoid-size-limits"></a>Verwenden eingeschlossener Spalten, um Größenbeschränkungen zu umgehen  
 Sie können Nichtschlüsselspalten in einen nicht gruppierten Index einschließen, damit die Größenbegrenzungen des aktuellen Indexes von maximal 16 Schlüsselspalten und einer maximalen Größe des Indexschlüssels von 900 Byte nicht überschritten werden. Nichtschlüsselspalten werden von [!INCLUDE[ssDE](../includes/ssde-md.md)] beim Berechnen der Indexschlüsselspalten oder Indexschlüsselgröße nicht berücksichtigt.   
 Angenommen, Sie möchten die folgenden Spalten in der `Document` -Tabelle indizieren:  
 *  `Title nvarchar(50)`  
 *  `Revision nchar(5)`  
 *  `FileName nvarchar(400)`  
  
 Da für die Datentypen **nchar** und **nvarchar** 2 Bytes für jedes Zeichen erforderlich sind, überschreitet ein Index, der diese drei Spalten enthält, die Größenbeschränkung von 900 Bytes um 10 Bytes (455 * 2). Indem die `INCLUDE` -Klausel der `CREATE INDEX` -Anweisung verwendet wird, kann der Indexschlüssel als (`Title, Revision`) und `FileName` als Nichtschlüsselspalte definiert werden. Auf diese Weise beträgt die Größe des Indexschlüssels 110 Byte (55 \* 2), und der Index enthält dennoch alle erforderlichen Spalten. Die folgende Anweisung erstellt einen solchen Index:  
  
```sql  
CREATE INDEX IX_Document_Title   
ON Production.Document (Title, Revision)   
INCLUDE (FileName);   
```  
  
##### <a name="index-with-included-columns-guidelines"></a>Richtlinien für Indizes mit eingeschlossenen Spalten  
Wenn Sie nicht gruppierte Indizes mit eingeschlossenen Spalten entwerfen, sollten Sie die folgenden Richtlinien beachten:  
  
-   Nichtschlüsselspalten werden in der INCLUDE-Klausel der CREATE INDEX-Anweisung definiert.  
  
-   Nichtschlüsselspalten können nur für nicht gruppierte Indizes für Tabellen oder indizierte Sichten definiert werden.  
  
-   Mit Ausnahme von **text**, **ntext** und **image** sind alle Datentypen zulässig.  
  
-   Bei berechneten Spalten, die deterministisch und präzise oder unpräzise sind, kann es sich um eingeschlossene Spalten handeln. Weitere Informationen finden Sie unter [Indexes on Computed Columns](../relational-databases/indexes/indexes-on-computed-columns.md).  
  
-   Ebenso wie Schlüsselspalten können berechnete Spalten, die aus **image**, **ntext** und **text** -Datentypen abgeleitet werden, Nichtschlüsselspalten (eingeschlossene Spalten) sein, wenn der Datentyp der berechneten Spalte als Nichtschlüssel-Indexspalte zulässig ist.  
  
-   Spaltennamen dürfen nicht sowohl in der INCLUDE-Liste als auch in der Schlüsselspaltenliste angegeben werden.  
  
-   Spaltennamen können nicht in der INCLUDE-Liste wiederholt werden.  

##### <a name="column-size-guidelines"></a>Richtlinien für die Spaltengröße  
  
-   Es muss mindestens eine Schlüsselspalte definiert werden. Die maximal zulässige Anzahl der Nichtschlüsselspalten beträgt 1023 Spalten. Dies ist die maximale Anzahl der Tabellenspalten minus 1.  
  
-   Indexschlüsselspalten (ausschließlich der Nichtschlüsselspalten) unterliegen der Begrenzung der Indexgröße auf maximal 16 Schlüsselspalten und der Gesamtgröße des Indexschlüssels von 900 Byte.  
  
-   Die Gesamtgröße aller Nichtschlüsselspalten wird nur durch die in der INCLUDE-Klausel angegebene Größe der Spalten beschränkt; **varchar(max)** -Spalten sind z. B. auf 2 GB beschränkt.  
  
##### <a name="column-modification-guidelines"></a>Richtlinien für die Spaltenänderung  
 Wenn Sie eine Tabellenspalte ändern, die als eingeschlossene Spalte definiert wurde, gelten die folgenden Einschränkungen:  
  
-   Nichtschlüsselspalten können nur aus der Tabelle gelöscht werden, wenn der Index zuvor gelöscht wird.  
  
-   Nichtschlüsselspalten können nur zum Ausführen der folgenden Aufgaben geändert werden:  
  
    -   Ändern der NULL-Zulässigkeit der Spalte von NOT NULL in NULL.  
  
    -   Vergrößern der Länge von **varchar**-, **nvarchar**- oder **varbinary** -Spalten.  
  
        > [!NOTE]  
        >  Diese Einschränkungen hinsichtlich der Spaltenänderung gelten für Indexschlüsselspalten.  
  
##### <a name="design-recommendations"></a>Entwurfsempfehlungen  
 Überarbeiten Sie nicht gruppierte Indizes mit großen Indexschlüsseln so, dass nur Spalten, die für Suchen und Suchvorgänge verwendet werden, Schlüsselspalten sind. Erklären Sie alle anderen Spalten, die die Abfrage abdecken, zu eingeschlossenen Nichtschlüsselspalten. Auf diese Weise sind alle Spalten vorhanden, die zum Abdecken der Abfrage erforderlich sind, der Indexschlüssel selbst ist jedoch klein und effizient.  
  
 Angenommen, Sie möchten z. B. einen Index entwerfen, der die folgende Abfrage abdeckt:  
  
```sql  
SELECT AddressLine1, AddressLine2, City, StateProvinceID, PostalCode  
FROM Person.Address  
WHERE PostalCode BETWEEN N'98000' and N'99999';  
```  
  
 Damit die Abfrage abgedeckt wird, muss jede Spalte im Index definiert werden. Sie könnten zwar alle Spalten als Schlüsselspalten definieren, die Schlüsselgröße würde dann aber 334 Byte betragen. Da die einzige Spalte, die tatsächlich als Suchkriterium verwendet wird, die `PostalCode` -Spalte mit einer Länge von 30 Byte ist, definiert der bessere Indexentwurf `PostalCode` als Schlüsselspalte und schließt alle anderen Spalten als Nichtschlüsselspalten ein.  
  
 Die folgende Anweisung erstellt einen Index mit eingeschlossenen Spalten, um die Abfrage abzudecken:  
  
```sql  
CREATE INDEX IX_Address_PostalCode  
ON Person.Address (PostalCode)  
INCLUDE (AddressLine1, AddressLine2, City, StateProvinceID);  
```  
  
##### <a name="performance-considerations"></a>Überlegungen zur Leistung  
 Vermeiden Sie es, nicht erforderliche Spalten hinzuzufügen. Das Hinzufügen einer zu großen Anzahl von Schlüssel- oder Nichtschlüssel-Indexspalten kann zu folgenden Auswirkungen auf die Leistung führen:  
  
-   Es passen weniger Indexzeilen auf eine Seite. Dies kann zu einer Zunahme der E/A und verringerter Cacheeffizienz führen.  
  
-   Zum Speichern des Indexes ist eine größere Menge an Speicherplatz erforderlich. Insbesondere das Hinzufügen von **varchar(max)** , **nvarchar(max)** , **varbinary(max)** oder **xml** -Datentypen als Nichtschlüssel-Indexspalten kann die Anforderungen an den Speicherplatz erheblich erhöhen. Der Grund liegt darin, dass die Spaltenwerte in die Blattebene des Indexes kopiert werden. Daher werden sie sowohl im Index als auch in der Basistabelle gespeichert.  
  
-   Die Indexwartung kann zu einem größeren Zeitaufwand für das Ausführen von Änderungen, Einfügungen, Updates oder Löschvorgängen an der zugrunde liegenden Tabelle oder indizierten Sicht führen.  
  
 Sie müssen überprüfen, ob die Steigerungen der Abfrageleistung die negativen Auswirkungen auf die Leistung während der Datenänderung sowie hinsichtlich zusätzlicher Speicherplatzanforderungen aufwiegen.  
  
##  <a name="unique-index-design-guidelines"></a><a name="Unique"></a> Richtlinien zum Entwerfen eindeutiger Indizes  
 Ein eindeutiger Index garantiert, dass der Indexschlüssel keine doppelten Werte enthält und dass deshalb jede Zeile in der Tabelle in gewisser Weise eindeutig ist. Das Angeben eines eindeutigen Indexes ist nur dann sinnvoll, wenn die Eindeutigkeit ein Merkmal der Daten ist. Wenn Sie z. B. sicherstellen möchten, dass die Werte in der `NationalIDNumber` -Spalte der `HumanResources.Employee` -Tabelle eindeutig sind, wenn der Primärschlüssel `EmployeeID`entspricht, erstellen Sie eine UNIQUE-Einschränkung für die `NationalIDNumber` -Spalte. Wenn der Benutzer versucht, denselben Wert für mehrere Mitarbeiter in diese Spalte einzugeben, wird eine Fehlermeldung angezeigt, und der doppelte Wert wird nicht eingegeben.  
  
 Durch eindeutige Indizes für mehrere Spalten stellt der Index sicher, dass jede Kombination der Werte in der indizierten Spalte eindeutig ist. Wenn z. B. ein eindeutiger Index für eine Kombination der Spalten `LastName`, `FirstName`und `MiddleName` erstellt wird, können zwei Zeilen in der Tabelle nicht über dieselbe Wertekombination für diese Spalten verfügen.  
  
 Sowohl gruppierte als auch nicht gruppierte Indizes können eindeutig sein. Unter der Voraussetzung, dass die Daten in der Spalte eindeutig sind, können Sie einen eindeutigen gruppierten Index und mehrere eindeutige nicht gruppierte Indizes für dieselbe Tabelle erstellen.  
  
 Eindeutige Indizes haben folgende Vorteile:  
  
-   Die Datenintegrität der definierten Spalten ist sichergestellt.  
  
-   Es werden zusätzliche, für den Abfrageoptimierer hilfreiche Informationen bereitgestellt.  
  
 Durch das Erstellen einer PRIMARY KEY- oder einer UNIQUE-Einschränkung wird automatisch ein eindeutiger Index für die angegebenen Spalten erstellt. Es gibt keine deutlichen Unterschiede zwischen dem Erstellen einer UNIQUE-Einschränkung und dem Erstellen eines eindeutigen Indexes unabhängig von einer Einschränkung. Die Datenüberprüfung erfolgt auf dieselbe Weise, und der Abfrageoptimierer macht keinen Unterschied zwischen einem durch eine Einschränkung erstellten eindeutigen Index und einem manuell erstellten Index. Allerdings sollten sie eine UNIQUE- oder PRIMARY KEY-Einschränkung für die Spalte erstellen, wenn Datenintegrität das Ziel ist. Dadurch wird das Ziel des Indexes klar.  
  
### <a name="considerations"></a>Überlegungen  
  
-   Ein eindeutiger Index, die UNIQUE-Einschränkung oder die PRIMARY KEY-Einschränkung kann nicht erstellt werden, wenn in den Daten doppelte Schlüsselwerte vorhanden sind.  
  
-   Wenn die Daten eindeutig sind und Sie die Eindeutigkeit erzwingen wollen, werden durch das Erstellen eines eindeutigen Indexes anstelle eines nicht eindeutigen Indexes für dieselbe Spaltenkombination zusätzliche Informationen für den Abfrageoptimierer bereitgestellt, mit deren Hilfe effizientere Ausführungspläne erstellt werden können. Das Erstellen eines eindeutigen Indexes (vorzugsweise durch Erstellen einer UNIQUE-Einschränkung) ist in diesem Fall empfohlen.  
  
-   Ein eindeutiger, nicht gruppierter Index kann eingeschlossene Nichtschlüsselspalten enthalten. Weitere Informationen finden Sie unter [Index mit eingeschlossenen Spalten](#Included_Columns).  
  
  
##  <a name="filtered-index-design-guidelines"></a><a name="Filtered"></a> Richtlinien für den Entwurf gefilterter Indizes  
 Ein gefilterter Index ist ein optimierter nicht gruppierter Index, der sich besonders für Abfragen eignet, bei denen aus einer fest definierten Teilmenge von Daten ausgewählt wird. Dieser verwendet ein Filterprädikat, um einen Teil der Zeilen in der Tabelle zu indizieren. Mit einem sorgfältig entworfenen gefilterten Index können im Gegensatz zu Tabellenindizes die Abfrageleistung verbessert und der Aufwand für die Indexverwaltung und -speicherung reduziert werden.  
  
**Gilt für**: [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] bis [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)].  
  
 Gefilterte Indizes können gegenüber Tabellenindizes folgende Vorteile bieten:  
  
-   **Verbesserte Abfrageleistung und Planqualität**  
  
     Mit einem sorgfältig entworfenen gefilterten Index wird die Abfrageleistung und die Ausführungsplanqualität verbessert, da dieser kleiner ist als ein nicht gruppierter Tabellenindex und mit gefilterten Statistiken arbeitet. Die gefilterten Statistiken sind genauer als Tabellenstatistiken, da diese nur die Zeilen im gefilterten Index umfassen.  
  
-   **Reduzierter Aufwand bei der Indexverwaltung**  
  
     Ein Index wird nur beibehalten, wenn DML-Anweisungen (Data Manipulation Language) die Daten im Index beeinflussen. Ein gefilterter Index reduziert im Vergleich zu einem nicht gruppierten Tabellenindex den Aufwand für die Indexverwaltung, da dieser kleiner ist und nur beibehalten wird, wenn die Daten im Index beeinflusst werden. Eine große Anzahl von gefilterten Indizes ist insbesondere dann von Vorteil, wenn diese Daten enthalten, die nur selten beeinflusst werden. Ebenso reduziert die geringere Indexgröße den Aufwand für die Aktualisierung der Statistiken, wenn ein gefilterter Index nur die häufig beeinflussten Daten enthält.  
  
-   **Reduzierter Aufwand bei der Indexspeicherung**  
  
     Ein gefilterter Index kann den Speicherplatzbedarf von nicht gruppierten Indizes reduzieren, wenn ein Tabellenindex nicht erforderlich ist. Sie können einen nicht gruppierten Tabellenindex durch mehrere gefilterte Indizes ersetzen, ohne damit die Speicherplatzanforderungen wesentlich zu erhöhen.  
  
 Gefilterte Indizes sind nützlich, wenn Spalten klar definierte Teilmengen von Daten enthalten, auf die Abfragen in SELECT-Anweisungen verweisen. Beispiele:  
  
-   Spalten mit geringer Dichte, die nur wenige Werte ungleich NULL enthalten.  
  
-   Heterogene Spalten, die Datenkategorien enthalten.  
  
-   Spalten, die Wertebereiche enthalten, z.&nbsp;B. Dollarmengen, Zeit- und Datumsangaben.  
  
-   Tabellenpartitionen, die durch einfache Vergleichslogik für Spaltenwerte definiert werden.  
  
 Der reduzierte Verwaltungsaufwand für gefilterte Indizes ist am deutlichsten, wenn die Zeilenanzahl im Index verglichen mit der eines Tabellenindex klein ist. Wenn der gefilterte Index die meisten Zeilen in der Tabelle einschließt, ist der Verwaltungsaufwand möglicherweise größer als bei einem Tabellenindex. In diesem Fall sollten Sie anstelle eines gefilterten Index einen Tabellenindex verwenden.  
  
 Gefilterte Indizes werden für eine Tabelle definiert und unterstützen nur einfache Vergleichsoperatoren. Wenn Sie einen Filterausdruck benötigen, der auf mehrere Tabellen verweist oder eine komplexe Logik aufweist, sollten Sie eine Sicht erstellen.  
  
### <a name="design-considerations"></a>Entwurfsaspekte  
 Wenn Sie effektive gefilterte Indizes entwerfen möchten, müssen Sie wissen, welche Abfragen von Ihrer Anwendung verwendet werden und wie diese mit Teilmengen Ihrer Daten in Beziehung stehen. Einige Beispiele für Daten mit fest definierten Teilmengen sind Spalten, die größtenteils nur NULL-Werte enthalten, Spalten mit heterogenen Wertekategorien und Spalten mit verschiedenen Wertebereichen. Die folgenden Entwurfsüberlegungen zeigen, wann ein gefilterter Index Vorteile gegenüber Tabellenindizes hat.  
 
> [!TIP] 
> Die Definition von nicht gruppierten [Columnstore-Indizes](#columnstore_index) unterstützt gefilterte Bedingungen. Um die Auswirkung auf die Leistung beim Hinzufügen eines Columnstore-Indexes in eine OLTP-Tabelle zu verringern, verwenden Sie eine gefilterte Bedingung, um einen nicht gruppierten Columnstore-Index anhand der kalten Daten Ihrer Betriebsworkload zu erstellen. 
  
#### <a name="filtered-indexes-for-subsets-of-data"></a>Gefilterte Indizes für Datenteilmengen  
Wenn eine Spalte nur wenig relevante Werte für Abfragen aufweist, können Sie für die Teilmenge der Werte einen gefilterten Index erstellen. Wenn beispielsweise die Werte in einer Spalte größtenteils NULL sind und die Abfrage nur die Werte ungleich NULL berücksichtigt, können Sie für die Datenzeilen mit den Werten ungleich NULL einen gefilterten Index erstellen. Der resultierende Index ist kleiner und verursacht weniger Verwaltungsaufwand als ein nicht gruppierter Tabellenindex, der für dieselben Schlüsselspalten festgelegt wird.  
  
Die Datenbank `AdventureWorks2012` enthält z. B. eine `Production.BillOfMaterials` -Tabelle mit 2679 Zeilen. Die `EndDate` -Spalte hat nur 199 Zeilen mit einem Wert ungleich NULL. Die anderen 2.480 Zeilen enthalten einen NULL-Wert. Der folgende gefilterte Index würde Abfragen abdecken, die die im Index definierten Spalten zurückgeben und die für `EndDate`nur Zeilen mit einem Wert ungleich NULL auswählen.  
  
```sql  
CREATE NONCLUSTERED INDEX FIBillOfMaterialsWithEndDate  
    ON Production.BillOfMaterials (ComponentID, StartDate)  
    WHERE EndDate IS NOT NULL ;  
GO  
```  
  
Der gefilterte Index `FIBillOfMaterialsWithEndDate` ist für die folgende Abfrage gültig. Sie können den Abfrageausführungsplan anzeigen, um zu bestimmen, ob der Abfrageoptimierer den gefilterten Index verwendet hat.  
  
```sql  
SELECT ProductAssemblyID, ComponentID, StartDate   
FROM Production.BillOfMaterials  
WHERE EndDate IS NOT NULL   
    AND ComponentID = 5   
    AND StartDate > '20080101' ;  
```  
  
Weitere Informationen zum Erstellen von gefilterten Indizes und zum Definieren des Prädikatausdrucks für gefilterte Indizes finden Sie unter [Create Filtered Indexes](../relational-databases/indexes/create-filtered-indexes.md).  
  
#### <a name="filtered-indexes-for-heterogeneous-data"></a>Gefilterte Indizes für heterogene Daten  
 Wenn eine Tabelle heterogene Datenzeilen enthält, können Sie einen gefilterten Index für eine oder mehrere Datenkategorien erstellen.  
  
 Zum Beispiel wird jedes Produkt, das in der `Production.Product` -Tabelle aufgelistet ist, einer `ProductSubcategoryID`zugewiesen, die wiederum den Produktkategorien Fahrräder, Bauteile, Bekleidung oder Zubehör zugeordnet wird. Diese Kategorien sind heterogen, da ihre Spaltenwerte in der `Production.Product` -Tabelle nicht eng zueinander in Beziehung stehen. Beispielsweise besitzen die Spalten `Color`, `ReorderPoint`, `ListPrice`, `Weight`, `Class`und `Style` eindeutige Merkmale für jede Produktkategorie. Angenommen, es werden häufig Abfragen für Zubehör mit Unterkategorien zwischen 27 und 36 einschließlich ausgeführt. Sie können die Abfrageleistung für Zubehör verbessern, indem Sie einen gefilterten Index für die Unterkategorien von Zubehör erstellen, wie im folgenden Beispiel veranschaulicht.  
  
```sql  
CREATE NONCLUSTERED INDEX FIProductAccessories  
    ON Production.Product (ProductSubcategoryID, ListPrice)   
        Include (Name)  
WHERE ProductSubcategoryID >= 27 AND ProductSubcategoryID <= 36;  
```  
  
 Der gefilterte Index `FIProductAccessories` deckt die folgende Abfrage ab, da die Abfrageergebnisse  
  
 im Index enthalten sind und der Abfrageplan keine Basistabellensuche einschließt. Der Abfrageprädikatausdruck `ProductSubcategoryID = 33` ist z. B. eine Teilmenge der gefilterten Indexprädikate `ProductSubcategoryID >= 27` und `ProductSubcategoryID <= 36`, die Spalten `ProductSubcategoryID` und `ListPrice` im Abfrageprädikat sind beides Schlüsselspalten im Index, und der Name wird in der Blattebene des Indexes als einbezogene Spalte gespeichert.  
  
```sql  
SELECT Name, ProductSubcategoryID, ListPrice  
FROM Production.Product  
WHERE ProductSubcategoryID = 33 AND ListPrice > 25.00 ;  
```  
  
#### <a name="key-columns"></a>Schlüsselspalten  
 Die bewährte Methode besteht darin, eine geringe Anzahl von Schlüsselspalten oder eingeschlossenen Spalten in eine Definition des gefilterten Indexes einzuschließen und nur die Spalten einzubeziehen, die der Abfrageoptimierer benötigt, um den gefilterten Index für den Abfrageausführungsplan auszuwählen. Der Abfrageoptimierer kann einen gefilterten Index für die Abfrage auswählen, unabhängig davon, ob dieser die Abfrage abdeckt oder nicht. Der Abfrageoptimierer wählt jedoch eher einen gefilterten Index aus, der die Abfrage abdeckt.  
  
 In einigen Fällen deckt ein gefilterter Index die Abfrage ab, ohne die Spalten im gefilterten Indexausdruck als Schlüsselspalten oder eingeschlossene Spalten in der Definition des gefilterten Indexes einzuschließen. Die folgenden Richtlinien erläutern, wann eine Spalte im gefilterten Indexausdruck eine Schlüsselspalte oder eingeschlossene Spalte in der Definition des gefilterten Indexes sein sollte. Die Beispiele beziehen sich auf den gefilterten Index `FIBillOfMaterialsWithEndDate` , der zuvor erstellt wurde.  
  
 Eine Spalte im gefilterten Indexausdruck muss in der Definition des gefilterten Indexes keine Schlüsselspalte oder eingeschlossene Spalte sein, wenn der gefilterte Indexausdruck dem Abfrageprädikat entspricht und die Abfrage die Spalte im gefilterten Indexausdruck mit den Abfrageergebnissen nicht zurückgibt. Zum Beispiel deckt `FIBillOfMaterialsWithEndDate` die folgende Abfrage ab, da das Abfrageprädikat dem Filterausdruck entspricht und `EndDate` nicht mit den Abfrageergebnissen zurückgegeben wird. `FIBillOfMaterialsWithEndDate` erfordert nicht, dass `EndDate` eine Schlüsselspalte oder eingeschlossene Spalte in der Definition des gefilterten Indexes ist.  
  
```sql  
SELECT ComponentID, StartDate FROM Production.BillOfMaterials  
WHERE EndDate IS NOT NULL;   
```  
  
 Eine Spalte im gefilterten Indexausdruck sollte in der Definition des gefilterten Indexes eine Schlüsselspalte oder eingeschlossene Spalte sein, wenn das Abfrageprädikat die Spalte in einem Vergleich verwendet, der nicht dem gefilterten Indexausdruck entspricht. Zum Beispiel ist `FIBillOfMaterialsWithEndDate` für die folgende Abfrage gültig, da damit aus dem gefilterten Index eine Teilmenge von Zeilen ausgewählt wird. Damit wird jedoch nicht die folgende Abfrage abgedeckt, da `EndDate` im Vergleich `EndDate > '20040101'`verwendet wird, der nicht dem gefilterten Indexausdruck entspricht. Der Abfrageprozessor kann diese Abfrage nicht ausführen, ohne die Werte von `EndDate`abzurufen. Deshalb sollte `EndDate` eine Schlüsselspalte oder eingeschlossene Spalte in der Definition des gefilterten Indexes darstellen.  
  
```sql  
SELECT ComponentID, StartDate FROM Production.BillOfMaterials  
WHERE EndDate > '20040101';   
```  
  
 Eine Spalte im gefilterten Indexausdruck sollte in der Definition des gefilterten Indexes eine Schlüsselspalte oder eingeschlossene Spalte sein, wenn die Spalte im Abfrageresultset enthalten ist. Zum Beispiel deckt `FIBillOfMaterialsWithEndDate` die folgende Abfrage nicht ab, da damit die `EndDate` -Spalte in den Abfrageergebnissen zurückgegeben wird. Deshalb sollte `EndDate` eine Schlüsselspalte oder eingeschlossene Spalte in der Definition des gefilterten Indexes darstellen.  
  
```sql  
SELECT ComponentID, StartDate, EndDate FROM Production.BillOfMaterials  
WHERE EndDate IS NOT NULL;  
```  
  
 Der Schlüssel des gruppierten Indexes für die Tabelle muss in der Definition des gefilterten Indexes keine Schlüsselspalte oder eingeschlossene Spalte sein. Der Schlüssel des gruppierten Indexes ist automatisch in allen nicht gruppierten Indizes enthalten, wozu auch gefilterte Indizes zählen.  
  
#### <a name="data-conversion-operators-in-the-filter-predicate"></a>Datenkonvertierungsoperatoren im Filterprädikat  
 Wenn der im gefilterten Indexausdruck der gefilterten Indexergebnisse angegebene Vergleichsoperator eine implizite oder explizite Datenkonvertierung ergibt, kommt es zu einem Fehler, wenn die Konvertierung auf der linken Seite eines Vergleichsoperators auftritt. Eine mögliche Lösung besteht darin, den gefilterten Indexausdruck mit dem Datenkonvertierungsoperator (CAST oder CONVERT) auf die rechte Seite des Vergleichsoperators zu schreiben.  
  
 Im folgenden Beispiel wird eine Tabelle mit einer Vielzahl von Datentypen erstellt.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE TABLE dbo.TestTable (a int, b varbinary(4));  
```  
  
 In der folgenden Definition des gefilterten Indexes wird die Spalte `b` implizit in einen ganzzahligen Datentyp konvertiert, um sie mit der Konstante 1 vergleichen zu können. Dadurch wird die Fehlermeldung 10611 erzeugt, da die Konvertierung auf der linken Seite des Operators im gefilterten Prädikat auftritt.  
  
```sql  
CREATE NONCLUSTERED INDEX TestTabIndex ON dbo.TestTable(a,b)  
WHERE b = 1;  
```  
  
 Die Lösung besteht darin, die Konstante auf der rechten Seite zu konvertieren, damit diese vom gleichen Typ ist wie Spalte `b`, wie aus dem folgenden Beispiel hervorgeht:  
  
```sql  
CREATE INDEX TestTabIndex ON dbo.TestTable(a,b)  
WHERE b = CONVERT(Varbinary(4), 1);  
```  
  
 Durch das Verschieben der Datenkonvertierung von der linken Seite auf die rechte Seite eines Vergleichsoperators wird möglicherweise die Bedeutung der Konvertierung geändert. Im obigen Beispiel wurde aus einem Integer-Vergleich ein **varbinary** -Vergleich, als der CONVERT-Operator der rechten Seite hinzugefügt wurde.  
  
## <a name="columnstore-index-design-guidelines"></a><a name="columnstore_index"></a> Richtlinien zum Entwerfen von Columnstore-Indizes

Ein *columnstore index* ist eine Technologie zum Speichern, Abrufen und Verwalten von Daten mithilfe eines spaltenbasierten Datenformats, das als Columnstore bezeichnet wird. Weitere Informationen finden Sie unter [Columnstore-Indizes: Übersicht](../relational-databases/indexes/columnstore-indexes-overview.md). 

Versionsinformationen finden Sie unter [Columnstore-Indizes – Neuigkeiten](../relational-databases/indexes/columnstore-indexes-what-s-new.md).

### <a name="columnstore-index-architecture"></a>Columnstore-Indizes: Architektur

Die Kenntnis dieser Grundlagen erleichtert es die anderen Columnstore-Artikel zu verstehen, die erläutern, wie sie effektiv verwendet werden können.

#### <a name="data-storage-uses-columnstore-and-rowstore-compression"></a>Die Datenspeicherung verwendet die Columnstore- und Rowstore-Komprimierung
Im Zusammenhang mit Columnstore-Indizes werden die Begriffe *Rowstore* und *Columnstore* verwendet, um das Format für die Datenspeicherung zu bezeichnen. Columnstore-Indizes verwenden beide Arten der Datenspeicherung.

 ![Gruppierter Columnstore-Index](../relational-databases/indexes/media/sql-server-pdw-columnstore-physicalstorage.gif "Gruppierter Columnstore-Index")

- Ein **Columnstore** enthält Daten, die logisch als Tabelle mit Zeilen und Spalten organisiert und physisch in einem Spaltendatenformat gespeichert sind.
  
  Ein Columnstore-Index speichert die meisten Daten physisch im Columnstore-Format. Im Columnstore-Format werden die Daten als Spalten komprimiert und dekomprimiert. Es ist nicht erforderlich, andere Werte in jeder Zeile zu dekomprimieren, die nicht von der Abfrage angefordert werden. Das schnelle Scannen einer ganzen Spalte einer großen Tabelle wird dadurch erleichtert. 

- Ein **Rowstore** enthält Daten, die logisch als Tabelle mit Zeilen und Spalten organisiert und anschließend physisch in einem Zeilendatenformat gespeichert sind. Dies stellte die herkömmliche Methode zum Speichern von Daten (z.B. Heaps oder gruppierte B-Strukturindizes) aus relationalen Tabellen dar.

  Ein Columnstore-Index speichert einige Zeilen auch physisch in einem Rowstore-Format, ein sogenannter Deltastore. Beim Deltastore, auch Delta-Zeilengruppe genannt, handelt es sich um einen Aufbewahrungsort für Zeilen, die eine zu geringe Anzahl darstellen, um in den Columnstore komprimiert zu werden. Jede Deltazeilengruppe wird als gruppierter B-Strukturindex implementiert. 

- Der **Deltastore** ist ein Aufbewahrungsort für Zeilen, die eine zu geringe Anzahl darstellen, um in den Columnstore komprimiert zu werden. Der Deltastore speichert die Zeilen im Rowstore-Format. 

Weitere Informationen zu Columnstore-Begriffen und -Konzepten finden Sie unter [Columnstore-Indizes: Overview](../relational-databases/indexes/columnstore-indexes-overview.md).
  
#### <a name="operations-are-performed-on-rowgroups-and-column-segments"></a>Vorgänge werden für Zeilengruppen und Spaltensegmente ausgeführt

Der Columnstore-Index gruppiert Zeilen in verwaltbare Einheiten. Diese Einheiten werden jeweils als eine **Zeilengruppe** bezeichnet. Die Anzahl der Zeilen in der Zeilengruppe muss groß genug sein, um die Komprimierungsraten zu verbessern, und klein genug, um von In-Memory-Vorgängen profitieren zu können.

Der Columnstore-Index führt diese Vorgänge z.B. auf Zeilengruppen aus:

* Komprimiert Zeilengruppen in den Columnstore. Die Komprimierung wird für jedes Spaltensegment innerhalb einer Zeilengruppe ausgeführt.
* Führt Zeilengruppen während eines `ALTER INDEX ... REORGANIZE`-Vorgangs zusammen und entfernt dabei gelöschte Daten
* Erstellt neue Zeilengruppen während eines `ALTER INDEX ... REBUILD`-Vorgangs.
* Meldet Zeilengruppen-Integrität und Fragmentierung in dynamischen Verwaltungsansichten (DMVs).

Der Deltastore besteht aus mindestens einer Zeilengruppe, die **Delta-Zeilengruppen** genannt werden. Bei jeder Deltazeilengruppe handelt es sich um einen gruppierten B-Strukturindex, der kleine Massenladevorgänge speichert und Zeilen einfügt, bis die Zeilengruppe 1.048.576 Zeilen enthält. Sobald dies der Fall ist, komprimiert der sogenannte **Tupelverschiebungsvorgang** automatisch die geschlossene Zeilengruppe in den Columnstore. 

Weitere Informationen zu Zeilengruppenzuständen finden Sie unter [sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md). 

> [!TIP]
> Zu viele kleine Zeilengruppen verschlechtern die Qualität des Columnstore-Index. Ein Neuorganisierungsvorgang führt kleinere Zeilengruppen zusammen, wobei eine interne Schwellenwertrichtlinie eingehalten wird, die bestimmt, wie gelöschte Zeilen entfernt und die komprimierten Zeilengruppen kombiniert werden. Nach einem Mergevorgang sollte sich die Indexqualität verbessert haben. 

> [!NOTE]
> Ab [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] wird der Tupelverschiebungsvorgang von einem Mergetask im Hintergrund unterstützt, der automatisch kleinere OPEN-Deltazeilengruppen komprimiert, die für einen bestimmten Zeitraum vorhanden waren (wie durch einen internen Schwellenwert festgelegt), oder COMPRESSED-Zeilengruppen mergt, aus denen eine große Anzahl von Zeilen gelöscht wurde.      

Jede Spalte weist einige ihrer Werte in jeder Zeilengruppe auf. Diese Werte werden als **Spaltensegmente** bezeichnet. Jede Zeilengruppe enthält ein Spaltensegment für jede Spalte in der Tabelle. Jede Spalte besitzt ein Spaltensegment in jeder Zeilengruppe.

![Spaltensegment](../relational-databases/indexes/media/sql-server-pdw-columnstore-columnsegment.gif "Spaltensegment") 
 
Wenn der Columnstore-Index eine Zeilengruppe komprimiert, wird jedes Spaltensegment separat komprimiert. Um eine ganze Spalte zu dekomprimieren, muss der Columnstore-Index nur ein Spaltensegment von jeder Zeilengruppe dekomprimieren. 

Weitere Informationen zu Columnstore-Begriffen und -Konzepten finden Sie unter [Columnstore-Indizes: Overview](../relational-databases/indexes/columnstore-indexes-overview.md). 

#### <a name="small-loads-and-inserts-go-to-the-deltastore"></a>Kleine Ladungen und Einfügungen werden im Deltastore gespeichert
Ein Columnstore-Index verbessert die Columnstore-Komprimierung und Leistung durch das Komprimieren von mindestens 102.400 Zeilen gleichzeitig in den Columnstore-Index. Um Zeilen in einem Sammelvorgang zu komprimieren, sammelt der Columnstore-Index kleine Lasten, und fügt diese im Deltastore ein. Die Deltastore-Vorgänge werden im Hintergrund verarbeitet. Damit die richtigen Abfrageergebnisse zurückgegeben werden, kombiniert der gruppierte Columnstore-Index Abfrageergebnisse aus dem Columnstore und dem Deltastore. 

Zeilen werden im Deltastore aufgenommen wenn sie:
* Mit der `INSERT INTO ... VALUES`-Anweisung eingefügt werden.
* Am Ende eines Massenladevorgangs angelangt sind und ihre Anzahl weniger als 102.400 ist.
* Aktualisiert. Jedes Update wird als Delete und Insert implementiert.

Der Deltastore speichert auch eine Liste von IDs für gelöschte Zeilen, die als gelöscht markiert, aber noch nicht physisch aus dem Columnstore entfernt wurden. 

Weitere Informationen zu Columnstore-Begriffen und -Konzepten finden Sie unter [Columnstore-Indizes: Overview](../relational-databases/indexes/columnstore-indexes-overview.md). 

#### <a name="when-delta-rowgroups-are-full-they-get-compressed-into-the-columnstore"></a>Wenn Delta-Zeilengruppen voll sind, werden sie in den Columnstore komprimiert

Gruppierte Columnstore-Indizes erfassen bis zu 1.048.576 Zeilen in jeder Delta-Zeilengruppe, bevor die Zeilengruppe in den Delta-Store komprimiert wird. Dies verbessert die Komprimierung des Columnstore-Index. Wenn eine Deltazeilengruppe die maximale Zeilenanzahl erreicht, ändert sich ihr Zustand von OPEN in CLOSED. Ein Hintergrundprozess namens Tupelverschiebungsvorgang überprüft auf geschlossene Zeilengruppen. Wenn der Prozess eine geschlossene Zeilengruppe findet, wird diese komprimiert und im Columnstore-Index gespeichert.  

Wenn eine Deltazeilengruppe komprimiert wurde, ändert sich der Zustand der vorhandenen Deltazeilengruppe in TOMBSTONE, damit sie später im Tupelverschiebungsvorgang entfernt wird, wenn nicht auf sie verwiesen wird. Die neue komprimierte Zeilengruppe wird als COMPRESSED gekennzeichnet. 

Weitere Informationen zu Zeilengruppenzuständen finden Sie unter [sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md). 

Mit [ALTER INDEX](../t-sql/statements/alter-index-transact-sql.md) können Delta-Zeilengruppen in den Columnstore gezwungen werden, um den Index neu zu erstellen oder zu organisieren. Beachten Sie, dass bei zu wenig verfügbarem Arbeitsspeicher während des Komprimiervorgangs, der Columnstore-Index die Anzahl der Zeilen in der komprimierten Zeilengruppe möglicherweise reduziert.   

Weitere Informationen zu Columnstore-Begriffen und -Konzepten finden Sie unter [Columnstore-Indizes: Overview](../relational-databases/indexes/columnstore-indexes-overview.md). 

#### <a name="each-table-partition-has-its-own-rowgroups-and-delta-rowgroups"></a>Jede Tabellenpartition verfügt über eigene Zeilengruppen und Delta-Zeilengruppen

Das Konzept der Partitionierung ist in einem gruppierten Index, einem Heap, und einem Columnstore-Index identisch. Das Partitionieren einer Tabelle teilt die Tabelle in kleinere Gruppen von Zeilen, gemäß einer Reihe von Spaltenwerten. Es wird häufig für die Verwaltung der Daten verwendet. Sie könnten z.B. für jedes Jahr und dessen Daten eine Partition erstellen, und anschließend einen Partitionswechsel durchführen, um Daten in einem kostengünstigeren Speicher zu archivieren. Ein Partitionswechsel funktioniert bei Columnstore-Indizes und macht es ganz einfach eine Partition von Daten an einen anderen Speicherort zu verschieben.

Zeilengruppen werden immer in einer Spalten-Partition definiert. Wenn ein Columnstore-Index partitioniert ist, besitzt jede Partition seine eigenen komprimierten Zeilengruppen und Delta-Zeilengruppen.

> [!TIP]
> Sie sollten die Verwendung der Tabellenpartitionierung in Betracht ziehen, wenn Daten aus dem Columnstore entfernt werden müssen. Das Auslagern und Abschneiden von Partitionen, die nicht mehr benötigt werden, ist eine effiziente Strategie zum Löschen von Daten, ohne Fragmentierung zu generieren, die durch kleinere Zeilengruppen eingeführt wird.

##### <a name="each-partition-can-have-multiple-delta-rowgroups"></a>Jede Partition kann über mehrere Delta-Zeilengruppen verfügen
Jede Partition kann über mehrere Delta-Zeilengruppen verfügen. Wenn der Columnstore-Index Daten zu einer Delta-Zeilengruppe hinzufügen muss, und die Delta-Zeilengruppe gesperrt ist, wird der Columnstore-Index versuchen eine Sperre für eine andere Delta-Zeilengruppe zu erhalten. Wenn keine Delta-Zeilengruppen verfügbar sind, wird der Columnstore-Index eine neue Delta-Zeilengruppe erstellen. Beispielsweise könnte eine Tabelle mit 10 Partitionen problemlos über mindestens 20 Delta-Zeilengruppen verfügen. 

#### <a name="you-can-combine-columnstore-and-rowstore-indexes-on-the-same-table"></a>Sie können Columnstore und Rowstore-Indizes für dieselbe Tabelle kombinieren
Der nicht gruppierte Index enthält eine Kopie eines Teils oder aller Zeilen und Spalten der zugrundeliegenden Tabelle. Der Index ist als eine oder mehrere Spalte(n) der Tabelle definiert und weist eine optionale Bedingung auf, die zum Filtern der Zeilen dient. 

Ab [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] können Sie einen aktualisierbaren, **nicht gruppierten Columnstore-Index für eine Rowstore-Tabelle erstellen**. Der Columnstore-Index speichert eine Kopie der Daten, sodass Sie keinen zusätzlichen Speicher benötigen. Allerdings werden die Daten in den Columnstore-Index komprimiert, auf eine kleinere Größe als die Rowstore-Tabelle es erfordert.  Durch dieses Vorgehen können Sie Analysen mit dem Columnstore-Index und Transaktionen mit dem Rowstore-Index zur gleichen Zeit ausführen. Der Spaltenspeicher wird aktualisiert, wenn sich die Daten in der Rowstore-Tabelle ändern, daher arbeiten beide Indizes auf den gleichen Daten.  
  
Ab [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] können **nicht gruppierte Rowstore-Indizes für einen oder mehrere Columnstore-Indizes erstellt werden**. Auf diese Weise können effiziente Tabellensuchvorgänge im zugrundeliegenden Columnstore ausgeführt werden. Auch weitere Optionen werden dadurch verfügbar. Beispielsweise können Sie eine Primärschlüsseleinschränkung durchsetzen, indem Sie eine UNIQUE-Bedingung auf die Rowstore-Tabelle anwenden. Da ein nicht eindeutiger Wert nicht in die Rowstore-Tabelle eingefügt werden kann, kann SQL Server den Wert nicht in den Columnstore einfügen.  
 
### <a name="performance-considerations"></a>Überlegungen zur Leistung 

-   Die Definition des nicht gruppierten Columnstore-Index unterstützt gefilterte Bedingungen. Um die Auswirkung auf die Leistung beim Hinzufügen eines Columnstore-Indexes in eine OLTP-Tabelle zu verringern, verwenden Sie eine gefilterte Bedingung, um einen nicht gruppierten Columnstore-Index anhand der kalten Daten Ihrer Betriebsworkload zu erstellen. 
  
-   Eine In-Memory-Tabelle kann nur über einen Columnstore-Index verfügen. Sie können ihn bei Erstellung der Tabelle generieren oder später mit [ALTER TABLE &#40;Transact-SQL&#41;](../t-sql/statements/alter-table-transact-sql.md) hinzufügen. Vor [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] konnte nur eine datenträgerbasierte Tabelle über einen Columnstore-Index verfügen. 

Weitere Informationen finden Sie unter [Columnstore-Indizes: Abfrageleistung](../relational-databases/indexes/columnstore-indexes-query-performance.md).

### <a name="design-guidance"></a>Leitfaden zum Entwurf 

-   Eine Rowstore-Tabelle kann über einen aktualisierbaren nicht gruppierten Columnstore-Index verfügen. Vor [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] waren nicht gruppierte Columnstore-Indizes schreibgeschützt.  
 
Weitere Informationen finden Sie unter [Columnstore indexes - Design Guidance (Leitfaden zum Entwerfen von Columnstore-Indizes)](../relational-databases/indexes/columnstore-indexes-design-guidance.md).

##  <a name="hash-index-design-guidelines"></a><a name="hash_index"></a> Richtlinien zum Entwerfen von Hashindizes 

Alle speicheroptimierten Tabellen müssen mindestens einen Index enthalten, da die Zeilen durch die Indizes miteinander verbunden werden. Für eine speicheroptimierte Tabelle wird jeder Index auch speicheroptimiert. Hashindizes zählen zu den möglichen Typen von Indizes in einer speicheroptimierten Tabelle. Weitere Informationen zu finden Sie unter [Indizes für speicheroptimierte Tabellen](../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md).

**Gilt für**: [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] bis [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)].  

### <a name="hash-index-architecture"></a>Architektur von Hashindizes
Ein Hashindex besteht aus einem Array von Zeigern. Jedes Element des Arrays wird als Hashbucket bezeichnet.
- Jeder Bucket umfasst 8 Bytes, die zum Speichern der Arbeitsspeicheradresse einer Linkliste von Schlüsseleinträgen verwendet werden.  
- Jeder Eintrag ist ein Wert für einen Indexschlüssel zuzüglich der Adresse für die entsprechende Zeile in der zugrunde liegenden speicheroptimierten Tabelle.  
- Jeder Eintrag verweist in einer Linkliste von Einträgen – alle mit dem aktuellen Bucket verkettet – auf den nächsten Eintrag.  

Die Anzahl von Buckets muss zum Zeitpunkt der Definition des Indexes angegeben werden:
- Je geringer das Verhältnis von Buckets zu Tabellenzeilen oder eindeutigen Werten, desto länger wird die durchschnittliche Bucketlinkliste.  
- Kurze Linklisten können schneller verarbeitet werden als lange Linklisten.
- Die maximale Anzahl von Buckets in einem Hashindex beträgt 1.073.741.824.

> [!TIP]
> Informationen, wie Sie den richtigen `BUCKET_COUNT` -Wert für Ihre Daten ermitteln, finden Sie unter [Konfigurieren der Hashindex-Bucketanzahl](#configuring_bucket_count).

Die Hashfunktion wird auf die Indexschlüsselspalten angewendet. Durch das Ergebnis dieser Funktion wird bestimmt, welchem Bucket dieser Schlüssel zugeordnet wird. Jeder Bucket verfügt über einen Zeiger zu den Zeilen, deren Hashschlüsselwerte diesem Bucket zugeordnet sind.

Die Hashfunktion, die für Hashindizes verwendet wird, weist die folgenden Merkmale auf:
- [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] hat eine Hashfunktion, die für alle Hashindizes verwendet wird.
- Die Hashfunktion ist deterministisch. Der gleiche Eingabeschlüsselwert wird immer demselben Bucket im Hashindex zugeordnet.
- Mehrere Indexschlüssel können dem gleichen Hashbucket zugeordnet werden.
- Die Hashfunktion ist ausgeglichen. Dies bedeutet, dass die Verteilung der Indexschlüsselwerte auf Hashbuckets üblicherweise nicht einer flachen linearen Verteilung entspricht, sondern einer Poisson- oder Glockenkurvenverteilung.
- Eine Poisson-Verteilung ist keine gleichmäßige Verteilung. Indexschlüsselwerte werden in die Hashbuckets nicht gleichmäßig verteilt.
- Wenn zwei Indexschlüssel dem gleichen Hashbucket zugeordnet werden, kommt es zu einem *Hashkonflikt*. Eine große Anzahl von Hashkonflikten kann sich auf die Leistung bei Lesevorgängen auswirken. Ein realistisches Ziel wäre es, wenn 30 % der Buckets zwei verschiedene Schlüsselwerte enthalten.
  
Die Interaktion zwischen dem Hashindex und den Buckets wird im folgenden Bild zusammengefasst.  
  
![hekaton_tables_23d](../relational-databases/in-memory-oltp/media/hekaton-tables-23d.png "Indexschlüssel, als Eingabe in die Hash-Funktion. Die Ausgabe stellt die Adresse eines Hashbuckets dar, der auf den Anfang der Kette verweist.")  

### <a name="configuring-the-hash-index-bucket-count"></a><a name="configuring_bucket_count"></a> Konfigurieren der Bucketanzahl eines Hashindexes
Die Bucketanzahl des Hashindexes wird während der Indexerstellung angegeben und kann mit der Syntax `ALTER TABLE...ALTER INDEX REBUILD` geändert werden.  
  
In den meisten Fällen sollte die Bucketanzahl das Ein- bis Zweifache der Anzahl eindeutiger Werte im Indexschlüssel betragen.   
Möglicherweise können Sie nicht immer prognostizieren, wie viele Werte ein bestimmter Indexschlüssel enthalten kann oder wird. Die Leistung ist in der Regel dennoch gut, falls der **BUCKET_COUNT** -Wert im Rahmen des 10-fachen der tatsächlichen Anzahl der Schlüsselwerte liegt, und die Überschätzung ist im Allgemeinen besser als die Unterschätzung.  
  
Zu **wenige** Buckets hat die folgenden Nachteile:  
  
- Mehr Hashkonflikte von eindeutigen Schlüsselwerten.  
- Jeder eindeutige Wert wird gezwungen, denselben Bucket mit einem anderen eindeutigen Wert zu nutzen.  
- Die durchschnittliche Kettenlänge pro Bucket nimmt zu.  
- Je länger die Bucketkette ist, desto langsamer ist die Gleichheitssuche im Index.  
  
Zu **viele** Buckets hat die folgenden Nachteile:  
  
- Bei einer zu hohen Bucketanzahl können möglicherweise mehr leere Buckets auftreten.  
- Leere Buckets beeinträchtigen die Leistung der vollständigen Indexscans. Wenn diese regelmäßig ausgeführt werden, erwägen Sie eine Bucketanzahl, die annähernd der Anzahl eindeutiger Indexschlüsselwerte entspricht.  
- Leere Buckets belegen Speicher, wobei jeder Bucket allerdings nur 8 Bytes belegt.  
  
> [!NOTE]
> Das Hinzufügen von weiteren Buckets trägt nichts zur Reduzierung der Verkettung von Einträgen bei, die sich einen duplizierten Wert teilen. Die Rate der Wertduplizierung wird verwendet, um zu entscheiden, ob ein Hash den entsprechenden Indextyp hat, nicht um die Bucketanzahl zu berechnen.  

### <a name="performance-considerations"></a>Überlegungen zur Leistung  
  
Die Leistung eines Hash-Indexes ist:  
  
- Ausgezeichnet, wenn das Prädikat der `WHERE`-Klausel einen **genauen** Wert für jede Spalte im Hashindexschlüssel angibt. Ein Hashindex wird mit einem Ungleichheitsprädikat auf einen Scan wiederhergestellt. 
- Schlecht, wenn das Prädikat der `WHERE`-Klausel im Indexschlüssel nach einem **Wertebereich** sucht.  
- Schlecht, wenn das Prädikat der `WHERE`-Klausel einen bestimmten Wert für die **erste** Spalte eines Hashindexschlüssels mit zwei Spalten festlegt, aber keinen Wert für die **anderen** Spalten des Schlüssels.  

> [!TIP]
> Das Prädikat muss alle Spalten im Hashindexschlüssel enthalten. Der Hashindex erfordert einen Schlüssel (für den Hash), um den Index zu durchsuchen. Wenn ein Indexschlüssel aus zwei Spalten besteht und die `WHERE`-Klausel nur die erste Spalte bereitstellt, besitzt [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] keinen vollständigen Schlüssel für den Hash. Dies führt zu einem Indexscan-Abfrageplan.

Wenn ein Hashindex verwendet wird und die Anzahl von eindeutigen Schlüsseln um das Hundertfache (oder mehr) über der Zeilenanzahl liegt, sollten Sie entweder die Bucketanzahl erhöhen, um umfangreiche Zeilenketten zu vermeiden, oder stattdessen einen [nicht gruppierten Index](#inmem_nonclustered_index) verwenden.

### <a name="declaration-considerations"></a><a name="h3-b2-declaration-limitations"></a> Überlegungen zur Deklaration  
Ein Hashindex kann nur für eine speicheroptimierte Tabelle vorhanden sein. Er kann nicht in einer datenträgerbasierten Tabelle vorhanden sein.  
  
Ein Hashindex kann deklariert werden als:  
  
- UNIQUE, oder standardmäßig als nicht eindeutig.  
- NONCLUSTERED. Dies ist die Standardeinstellung.   
  
Im Folgenden finden Sie ein Beispiel der Syntax zum Erstellen eines Hashindexes außerhalb der CREATE TABLE-Anweisung:  
  
```sql
ALTER TABLE MyTable_memop  
ADD INDEX ix_hash_Column2 UNIQUE  
HASH (Column2) WITH (BUCKET_COUNT = 64);
``` 

### <a name="row-versions-and-garbage-collection"></a>Zeilenversionen und Garbage Collection  
Wenn eine Zeile einer speicheroptimierten Tabelle von `UPDATE` betroffen ist, erstellt die Tabelle eine aktualisierte Version dieser Zeile. Während der Updatetransaktion können andere Sitzungen möglicherweise die ältere Version der Zeile lesen und so den Leistungsabfall vermeiden, der mit einer Zeilensperre einhergeht.  
  
Der Hashindex kann möglicherweise auch verschiedene Versionen seiner Einträge haben, um das Update zu berücksichtigen.  
  
Wenn die älteren Versionen später nicht mehr erforderlich sind, durchsucht ein Thread der automatischen Speicherbereinigung (GC, Garbage Collection) die Buckets und Verknüpfung, um alte Einträge zu beseitigen. Der GC-Thread bietet eine bessere Leistung, wenn die Kettenlängen kurz sind. Weitere Informationen finden Sie unter [In-Memory-OLTP-Garbage Collection](../relational-databases/in-memory-oltp/in-memory-oltp-garbage-collection.md). 

##  <a name="memory-optimized-nonclustered-index-design-guidelines"></a><a name="inmem_nonclustered_index"></a> Memory-Optimized Nonclustered Index Design Guideline (Richtlinien zum Entwerfen von speicheroptimierten, nicht gruppierten Indizes) 

Nicht gruppierte Indizes zählen zu den möglichen Typen von Indizes in einer speicheroptimierten Tabelle. Weitere Informationen zu finden Sie unter [Indizes für speicheroptimierte Tabellen](../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md).

**Gilt für**: [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] bis [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)].  

### <a name="in-memory-nonclustered-index-architecture"></a>Architektur nicht gruppierter In-Memory-Indizes

Nicht gruppierte In-Memory-Indizes werden mithilfe einer Datenstruktur implementiert, die als Bw-Struktur bezeichnet wird. Diese wurde 2011 ursprünglich von Microsoft Research entworfen und beschrieben. Bei einer Bw-Struktur handelt es sich um eine Variante der B-Struktur, die keine Sperren oder Latches aufweist. Weitere Informationen finden Sie unter [The Bw-Tree: A B-tree for New Hardware Platforms](https://www.microsoft.com/research/publication/the-bw-tree-a-b-tree-for-new-hardware/). 

Im Allgemeinen kann die Bw-Struktur als Zuordnung von Seiten verstanden werden, die von der Seiten-ID (PidMap) organisiert wird, sowie als Hilfsmittel zum Belegen und Wiederverwenden von Seiten-IDs (PidAlloc) und als Reihe von Seiten, die mit der Seitenzuordnung und miteinander verknüpft sind. Diese drei allgemeinen Unterkomponenten bilden zusammen die grundlegende interne Struktur einer Bw-Struktur.

Die Struktur ähnelt der normalen B-Struktur in folgenden Hinsichten: Jede Seite verfügt über mehrere sortierte Schlüsselwerte, es gibt Ebenen im Index, die jeweils auf eine niedrigere Ebene zeigen, und die Blattebenen zeigen auf eine Datenzeile. Es gibt jedoch einige Unterschiede.

Genau wie bei Hashindizes können mehrere Datenzeilen (Versionen) miteinander verknüpft werden. Bei den Seitenzeigern zwischen den Ebenen handelt es sich um logische Seiten-IDs, die Offsets zu einer Seitenzuordnungstabelle darstellen, die wiederum die physische Adresse für jede Seite enthält.

Es gibt keine direkten Updates für Indexseiten. Deshalb werden neue Änderungsseiten eingeführt.
-  Latches oder Sperren sind für Seitenupdates nicht erforderlich.
-  Indexseiten verfügen nicht über eine festgelegte Größe.

Der Schlüsselwert in jeder dargestellten Seite auf der inneren Knotenebene entspricht dem höchsten Wert, den das untergeordnete Element enthält, auf das gezeigt wird. Jede Zeile enthält ebenfalls die logische Seiten-ID der Seite. Auf den Seiten auf Blattebene ist neben dem Schlüsselwert die physische Adresse der Datenzeile enthalten.

Punktsuchen ähneln den B-Strukturen zwar, der Unterschied ist jedoch, dass die Seiten nur in eine Richtung verknüpft sind und [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] den richtigen Seitenzeigern folgt. Hierbei enthält jede Seite auf der inneren Knotenebene den höchsten Wert des untergeordneten Elements und nicht den niedrigsten wie bei einer B-Struktur.

Wenn eine Seite auf Blattebene geändert werden muss, ändert [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] nicht die Seite selbst. Stattdessen erstellt [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] einen Änderungsdatensatz, der die Änderungen beschreibt, und fügt diesen der vorherigen Seite an. Anschließend wird ebenfalls die Tabellenadresse der Seitenzuordnung für die vorherige Seite aktualisiert. Die aktualisierte Adresse entspricht der des Änderungsdatensatzes, die nun die physische Adresse für diese Seite darstellt.

Es gibt drei verschiedene Vorgänge, die für das Verwalten der Struktur einer Bw-Struktur erforderlich sein können: Konsolidierung, Teilung und Merge.

#### <a name="delta-consolidation"></a>Konsolidierung von Änderungen
Eine lange Kette von Änderungsdatensätzen kann die Suchleistung beeinträchtigen, da lange Ketten durchlaufen werden müssen, wenn ein Index durchsucht wird. Wenn ein neuer Änderungsdatensatz zu einer Kette hinzugefügt wird, die bereits über 16 Elemente verfügt, werden die Änderungen in den Änderungsdatensätzen in die Indexseite konsolidiert, auf die verwiesen wird. Die Seite wird dann neu erstellt und enthält Änderungen, die vom neuen Änderungsdatensatz angegeben werden, der die Konsolidierung ausgelöst hat. Die neu erstellte Seite besitzt die gleiche Seiten-ID, aber eine neue Speicheradresse. 

![hekaton_tables_23e](../relational-databases/in-memory-oltp/media/HKNCI_Delta.gif "Konsolidierung von Änderungsdatensätzen")

#### <a name="split-page"></a>Teilen von Seiten
Eine Indexseite in einer Bw-Struktur wächst je nach Bedarf, beginnend bei der Speicherung einer einzelnen Zeile bis hin zu einer Speicherung von maximal 8 KB. Sobald die Indexseite 8 KB erreicht, bewirkt das Einfügen einer einzelnen Seite die Teilung der Indexseite. Bei einer internen Seite bedeutet dies, dass kein Platz zum Hinzufügen eines weiteren Schlüsselwerts oder Zeigers verfügbar ist. Bei einer Blattseite bedeutet dies, dass die Zeile nach dem Integrieren aller Änderungsdatensätze zu groß ist, um auf die Seite zu passen. Die statistischen Informationen im Seitenkopf einer Blattseite verfolgen, wie viel Speicherplatz erforderlich ist, um die Änderungsdatensätze zu konsolidieren. Diese Informationen werden jeweils beim Hinzufügen neuer Änderungsdatensätze angepasst. 

Ein Teilungsvorgang wird in zwei unteilbaren Schritten ausgeführt. Gehen Sie in der folgenden Abbildung davon aus, dass eine Blattseite eine Teilung erzwingt, da ein Schlüssel mit dem Wert 5 eingefügt wird und eine Seite auf der inneren Knotenebene vorhanden ist, die auf das Ende der aktuellen Seite auf Blattebene zeigt (Schlüsselwert 4).

![hekaton_tables_23f](../relational-databases/in-memory-oltp/media/HKNCI_Split.gif "Teilen von Seiten")

**Schritt 1:** Ordnen Sie zwei neue Seiten namens P1 und P2 zu, und teilen Sie die Zeilen der alten Seite namens P1, einschließlich der neu eingefügten Zeile, auf diese neuen Seiten auf. Ein neuer Slot in der Seitenzuordnungstabelle wird verwendet, um die physische Adresse der Seite P2 zu speichern. Auf die Seiten P1 und P2 kann derzeit noch nicht durch gleichzeitige Vorgänge zugegriffen werden. Darüber hinaus wird der logische Zeiger von P1 auf P2 festgelegt. Aktualisieren Sie die Seitenzuordnungstabelle anschließend in einem unteilbaren Schritt, um den Zeiger von der alten Seite P1 auf die neue Seite P1 zu ändern. 

**Schritt 2:** Die Seite auf der inneren Knotenebene zeigt auf P1, es gibt jedoch keinen direkten Zeiger von einer Seite auf der inneren Knotenebene zu P2. P2 ist nur über P1 erreichbar. Zum Erstellen eines Zeigers von einer Seite auf der inneren Knotenebene zu P2, ordnen Sie eine neue Seite auf der inneren Knotenebene zu (interne Indexseite), kopieren Sie alle Zeilen von der alten Seite auf der inneren Knotenebene, und fügen Sie eine neue Zeile hinzu, um auf P2 zu zeigen. Sobald dieser Vorgang abgeschlossen ist, aktualisieren Sie die Seitenzuordnungstabelle in einem unteilbaren Schritt, um den Zeiger von der alten Seite auf der inneren Knotenebene zur neuen Seite auf der inneren Knotenebene zu ändern.

#### <a name="merge-page"></a>Merge von Seiten
Wenn ein `DELETE`-Vorgang dazu führt, dass eine Seite über weniger als 10 % der maximalen Seitengröße (derzeit 8 KB) verfügt oder nur eine einzelne Zeile enthält, wird diese Seite mit einer zusammenhängenden Seite zusammengeführt.

Wenn eine Zeile aus einer Seite gelöscht wird, wird ein Änderungsdatensatz für den Löschvorgang hinzugefügt. Darüber hinaus wird eine Überprüfung durchgeführt, um zu bestimmen, ob die Indexseite (d.h. die Seite auf der inneren Knotenebene) für einen Merge qualifiziert ist. Dadurch wird überprüft, ob der verbleibende Speicherplatz nach dem Löschen der Zeile weniger als 10 % der maximalen Seitengröße entspricht. Wenn dies zutrifft, wird der Merge in drei unteilbaren Schritten durchgeführt.

Gehen Sie in der folgenden Abbildung davon aus, dass ein `DELETE`-Vorgang den Schlüsselwert 10 löscht. 

![hekaton_tables_23g](../relational-databases/in-memory-oltp/media/HKNCI_Merge.gif "Zusammenführen von Seiten")

**Schritt 1:** Eine Änderungsseite, die den Schlüsselwert 10 darstellt, (blaues Dreieck) wird erstellt, und deren Zeiger auf die Seite Pp1 auf der inneren Knotenebene wird auf die neue Änderungsseite festgelegt. Darüber hinaus wird eine besondere Änderungsseite für den Merge (orangefarbenes Dreieck) erstellt und mit dem Zeiger zur Änderungsseite verknüpft. Zu diesem Zeitpunkt sind beide Seiten (die Änderungsseite und die Änderungsseite für den Merge) nicht für gleichzeitige Transaktionen sichtbar. In einem unteilbaren Schritt wird der Zeiger zur Seite P1 auf Blattebene in der Seitenzuordnungstabelle aktualisiert, damit dieser auf die Änderungsseite für den Merge zeigt. Nach diesem Schritt zeigt der Eintrag für den Schlüsselwert 10 in Pp1 auf die Änderungsseite für den Merge. 

**Schritt 2:** Auf der Seite Pp1 auf der inneren Knotenebene muss die Zeile entfernt werden, die den Schlüsselwert 7 enthält, und der Eintrag für den Schlüsselwert 10 muss aktualisiert werden, damit dieser auf P1 zeigt. Hierfür wird eine neue Seite namens Pp2 auf der inneren Knotenebene zugeordnet, und alle Zeilen von Pp1 (mit Ausnahme der Zeile, die den Schlüsselwert 7 enthält) werden kopiert. Anschließend wird die Zeile für den Schlüsselwert 10 aktualisiert, damit diese auf die Seite P1 zeigt. Sobald dieser Vorgang abgeschlossen ist, wird der Eintrag der Seitenzuordnungstabelle aktualisiert, der auf Pp1 zeigt, damit dieser auf Pp2 zeigt. Pp1 ist nicht mehr erreichbar. 

**Schritt 3:** Die Seiten P2 und P1 auf Blattebene werden zusammengeführt, und die Änderungsseiten werden entfernt. Hierfür wird die neue Seite P3 zugeordnet, die Zeilen von P2 und P1 werden zusammengeführt, und die Änderungen durch die Änderungsseite werden in die neue Seite P3 integriert. Anschließend wird der Eintrag der Seitenzuordnungstabelle, der auf P1 zeigt, in einem unteilbaren Schritt aktualisiert, um auf die Seite P3 zu zeigen. 

### <a name="performance-considerations"></a>Überlegungen zur Leistung

Die Leistung eines nicht gruppierten Indexes ist beim Abfragen von speicheroptimierten Tabellen mit Ungleichheitsprädikaten besser als die eines nicht gruppierten Hashindexes.

> [!NOTE]
> Eine Spalte in einer speicheroptimierten Tabelle kann Teil eines Hashindexes und des nicht gruppierten Indexes sein.

> [!TIP]
> Wenn eine Spalte in einer nicht gruppierten Indexschlüsselspalte viele doppelte Werte aufweist, kann die Leistung bei Updates, Einfügungen und Löschungen abnehmen. Eine Möglichkeit, die Leistung zu verbessern, ist in diesem Fall das Hinzufügen einer anderen Spalte zum nicht gruppierten Index.

##  <a name="additional-reading"></a><a name="Additional_Reading"></a> Zusätzliches Lesematerial  
[CREATE INDEX &#40;Transact-SQL&#41;](../t-sql/statements/create-index-transact-sql.md)    
[ALTER INDEX &#40;Transact-SQL&#41;](../t-sql/statements/alter-index-transact-sql.md)   
[CREATE XML INDEX (Transact-SQL)](../t-sql/statements/create-xml-index-transact-sql.md)  
[CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../t-sql/statements/create-spatial-index-transact-sql.md)     
[Neuorganisieren und Neuerstellen von Indizes](../relational-databases/indexes/reorganize-and-rebuild-indexes.md)         
[Verbessern der Leistung mit indizierten Sichten in SQL Server 2008](/previous-versions/sql/sql-server-2008/dd171921(v=sql.100))  
[Partitionierte Tabellen und Indizes](../relational-databases/partitions/partitioned-tables-and-indexes.md)  
[Erstellen eines Primärschlüssels](../relational-databases/tables/create-primary-keys.md)    
[Indizes für speicheroptimierte Tabellen](../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)  
[Columnstore-Indizes: Übersicht](../relational-databases/indexes/columnstore-indexes-overview.md)  
[Troubleshooting Hash Indexes for Memory-Optimized Tables (Behandlung von Problemen bei Hashindizes für speicheroptimierte Tabellen)](../relational-databases/in-memory-oltp/hash-indexes-for-memory-optimized-tables.md)    
[Dynamische Verwaltungssichten für speicheroptimierte Tabellen (Transact-SQL)](../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)   
[Index Related Dynamic Management Views and Functions (Transact-SQL) (Indexbezogene dynamische Verwaltungssichten und -funktionen (Transact-SQL))](../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)       
[Indizes in berechneten Spalten](../relational-databases/indexes/indexes-on-computed-columns.md)   
[Indizes und ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md#indexes-and-alter-table)      
[Adaptive Indexdefragmentierung](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag)
