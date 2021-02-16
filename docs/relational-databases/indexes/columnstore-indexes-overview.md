---
description: 'Columnstore-Indizes: Übersicht'
title: 'Columnstore-Indizes: Übersicht | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 05/08/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexes creation, columnstore
- indexes [SQL Server], columnstore
- columnstore index
- batch mode execution
- columnstore index, described
- xVelocity, columnstore indexes
ms.assetid: f98af4a5-4523-43b1-be8d-1b03c3217839
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f4ac369f42fccd251cffb5b06f3603f8349f857a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351226"
---
# <a name="columnstore-indexes-overview"></a>Columnstore-Indizes: Übersicht
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Columnstore-Indizes stellen den Standard für das Speichern und Abfragen großer Data Warehousing-Faktentabellen dar. Dieser Index verwendet spaltenbasierte Datenspeicherung und Abfrageverarbeitung, um eine bis zu zehnmal höhere Abfrageleistung im Data Warehouse im Vergleich zur herkömmlichen zeilenorientierten Speicherung zu erzielen. Sie können im Vergleich zur unkomprimierten Datengröße außerdem eine bis zu 10-mal höhere Datenkomprimierung erzielen. Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 ermöglichen Columnstore-Indizes die operative Analyse und bieten damit die Möglichkeit, leistungsfähige Echtzeitanalysen von Transaktionsarbeitsauslastungen durchzuführen.  
  
Erfahren Sie mehr über ähnliche Szenarien:  
  
-   [Columnstore-Indizes: Data Warehouse](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)  
-   [Erste Schritte mit Columnstore für operative Echtzeitanalyse](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)  
  
## <a name="what-is-a-columnstore-index"></a>Was ist ein Columnstore-Index?  
Ein Columnstore-Index ist eine Technologie zum Speichern, Abrufen und Verwalten von Daten mithilfe eines spaltenbasierten Datenformats, das als *Columnstore* bezeichnet wird.  
  
### <a name="key-terms-and-concepts"></a>Hauptbegriffe und -Konzepte  
Die folgenden Hauptbegriffe und -konzepte werden im Zusammenhang mit Columnstore-Indizes verwendet.  
  
#### <a name="columnstore"></a>columnstore
Ein Columnstore enthält Daten, die logisch als Tabelle mit Zeilen und Spalten organisiert und physisch in einem Spaltendatenformat gespeichert sind.  
  
#### <a name="rowstore"></a>Rowstore
Ein Rowstore enthält Daten, die logisch als Tabelle mit Zeilen und Spalten organisiert und physisch in einem Zeilendatenformat gespeichert sind. Dieses Format wird üblicherweise zum Speichern relationaler Tabellendaten verwendet. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bezieht sich Rowstore auf die Tabelle, deren zugrunde liegendes Datenspeicherformat ein Heap, ein gruppierter Index oder eine speicheroptimierte Tabelle ist.  
  
> [!NOTE]  
> In Zusammenhang mit Columnstore-Indizes bezeichnen die Begriffe „Rowstore“ und „Columnstore“ das Format der Datenspeicherung.  
  
#### <a name="rowgroup"></a>Zeilengruppe
Eine Zeilengruppe ist eine Gruppe von Zeilen, die gleichzeitig im Columnstore-Format komprimiert werden. Eine Zeilengruppe enthält in der Regel die maximale Anzahl von Zeilen pro Zeilengruppe (d. h. 1.048.576 Zeilen).  
  
Um eine hohe Leistung und hohe Komprimierungsraten zu erzielen, unterteilt der Columnstore-Index die Tabelle in Zeilengruppen und komprimiert dann jede Zeilengruppe nach Spalten. Die Anzahl der Zeilen in der Zeilengruppe muss groß genug sein, um die Komprimierungsraten zu verbessern, und klein genug, um von In-Memory-Vorgängen profitieren zu können.    

Der Zustand einer Zeilengruppe, aus der alle Daten gelöscht wurden, ändert sich von COMPRESSED in TOMBSTONE. Später wird sie durch einen Hintergrundprozess entfernt, der auch Tupelverschiebungsvorgang genannt wird. Weitere Informationen zu Zeilengruppenzuständen finden Sie unter [sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md).

> [!TIP]
> Zu viele kleine Zeilengruppen verschlechtern die Qualität des Columnstore-Index. Bis [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] ist ein Neuorganisierungsvorgang erforderlich, um kleinere COMPRESSED-Zeilengruppen zusammenzuführen. Dabei wird eine interne Schwellenwertrichtlinie eingehalten, die bestimmt, wie gelöschte Zeilen entfernt und die komprimierten Zeilengruppen kombiniert werden.    
> Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] wird ebenfalls ein Hintergrundvorgang durchgeführt, um COMPRESSED-Zeilengruppen zusammenzuführen, aus denen viele Zeilen gelöscht wurden.     
> Nachdem kleinere Zeilengruppen zusammengeführt wurden, sollte sich die Indexqualität verbessern. 

> [!NOTE]
> Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] wird der Tupelverschiebungsvorgang von einem Mergetask im Hintergrund unterstützt, der automatisch kleinere OPEN-Deltazeilengruppen komprimiert, die für einen bestimmten Zeitraum vorhanden waren (wie durch einen internen Schwellenwert festgelegt), oder COMPRESSED-Zeilengruppen mergt, aus denen eine große Anzahl von Zeilen gelöscht wurde. Dies verbessert die Qualität des Columnstore-Index im Lauf der Zeit.         

#### <a name="column-segment"></a>Spaltensegment
Ein Spaltensegment ist eine Spalte mit Daten aus der Zeilengruppe.  
  
-   Jede Zeilengruppe enthält ein Spaltensegment für jede Spalte in der Tabelle.  
-   Jedes Spaltensegment wird zusammenhängend komprimiert und auf physischen Medien gespeichert.  
  
![Spaltensegment](../../relational-databases/indexes/media/sql-server-pdw-columnstore-columnsegment.gif "Spaltensegment")  
  
#### <a name="clustered-columnstore-index"></a>Gruppierter Columnstore-Index
Ein gruppierter Columnstore-Index ist der physische Speicher für die gesamte Tabelle.    
  
![Gruppierter Columnstore-Index](../../relational-databases/indexes/media/sql-server-pdw-columnstore-physicalstorage.gif "Gruppierter Columnstore-Index")  
  
Um die Fragmentierung der Spaltensegmente zu verringern und die Leistung zu verbessern, können einige Daten im Columnstore-Index vorübergehend in einem gruppierten Index (*Deltastore*) und in einer B-Struktur mit IDs der gelöschten Zeilen gespeichert werden. Die Deltastore-Vorgänge werden im Hintergrund verarbeitet. Damit die richtigen Abfrageergebnisse zurückgegeben werden, kombiniert der gruppierte Columnstore-Index Abfrageergebnisse aus dem Columnstore und dem Deltastore.  
  
#### <a name="delta-rowgroup"></a>Deltazeilengruppe
Eine Deltazeilengruppe ist ein gruppierter B-Strukturindex, der nur mit Columnstore-Indizes verwendet wird. Sie verbessert die Columnstore-Komprimierung und -Leistung, indem sie Zeilen solange speichert, bis bei der Zeilenanzahl ein bestimmter Schwellenwert (1.048.576 Zeilen) erreicht wird und diese dann in den Columnstore verschoben werden.  

Wenn eine Deltazeilengruppe die maximale Zeilenanzahl erreicht, ändert sich ihr Zustand von OPEN in CLOSED. Ein Hintergrundprozess namens Tupelverschiebungsvorgang überprüft auf geschlossene Zeilengruppen. Wenn der Prozess eine geschlossene Zeilengruppe findet, wird die Deltazeilengruppe komprimiert und im Columnstore als COMPRESSED-Zeilengruppe gespeichert. 

Wenn eine Deltazeilengruppe komprimiert wurde, ändert sich der Zustand der vorhandenen Deltazeilengruppe in TOMBSTONE, damit sie später im Tupelverschiebungsvorgang entfernt wird, wenn nicht auf sie verwiesen wird. 

Weitere Informationen zu Zeilengruppenzuständen finden Sie unter [sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md). 

> [!NOTE]
> Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] wird der Tupelverschiebungsvorgang von einem Mergetask im Hintergrund unterstützt, der automatisch kleinere OPEN-Deltazeilengruppen komprimiert, die für einen bestimmten Zeitraum vorhanden waren (wie durch einen internen Schwellenwert festgelegt), oder COMPRESSED-Zeilengruppen mergt, aus denen eine große Anzahl von Zeilen gelöscht wurde. Dies verbessert die Qualität des Columnstore-Index im Lauf der Zeit.         
  
#### <a name="deltastore"></a>Deltastore
Ein Columnstore-Index kann mehr als eine Deltazeilengruppe haben. Alle Deltazeilengruppen zusammen werden als „Deltastore“ bezeichnet.   

Während eines umfassenden Massenladevorgangs werden die meisten Zeilen ohne Umweg über den Deltastore direkt in den Columnstore verschoben. Einige Zeilen am Ende des Massenladevorgangs erreichen möglicherweise nicht die notwendige Anzahl für die minimale Größe einer Zeilengruppe von 102.400 Zeilen. In diesem Fall werden die letzten Zeilen in den Deltastore anstatt in den Columnstore verschoben. Bei kleinen Massenladevorgängen mit weniger als 102.400 Zeilen werden alle Zeilen direkt in den Deltastore verschoben.  
  
#### <a name="nonclustered-columnstore-index"></a>Nicht gruppierter Columnstore-Index
Ein nicht gruppierter Columnstore-Index und ein gruppierter Columnstore-Index haben die gleiche Funktionsweise. Der Unterschied besteht darin, dass ein nicht gruppierter Index ein sekundärer Index ist, der für eine Rowstore-Tabelle erstellt wird. Ein gruppierter Columnstore-Index hingegen ist der primäre Speicher für die gesamte Tabelle.  
  
Der nicht gruppierte Index enthält eine Kopie eines Teils oder aller Zeilen und Spalten der zugrundeliegenden Tabelle. Der Index ist als mindestens eine Spalte der Tabelle definiert und weist eine optionale Bedingung auf, die zum Filtern der Zeilen dient.  
  
Ein nicht gruppierter Columnstore-Index ermöglicht operative Echtzeitanalyse, bei der der OLTP-Workload den zugrunde liegenden gruppierten Index verwendet, während die Analyse parallel auf dem Columnstore-Index ausgeführt wird. Weitere Informationen finden Sie unter [Erste Schritte mit Columnstore für operative Echtzeitanalyse](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
#### <a name="batch-mode-execution"></a>Batchmodusausführung
Die Batchmodusausführung ist eine Methode zur Abfrageverarbeitung, die zum gleichzeitigen Abfragen mehrerer Zeilen verwendet wird. Die Batchmodusausführung ist eng in das Columnstore-Speicherformat integriert und für dieses optimiert. Die Batchmodusausführung wird auch als *vektorbasierte* oder *vektorisierte* Ausführung bezeichnet. Abfragen von Columnstore-Indizes verwenden die Batchmodusausführung, die die Abfrageleistung in der Regel um das Zwei- bis Vierfache steigert. Weitere Informationen finden Sie im [Handbuch zur Architektur der Abfrageverarbeitung](../query-processing-architecture-guide.md#execution-modes). 
  
##  <a name="why-should-i-use-a-columnstore-index"></a><a name="benefits"></a> Warum sollte ich einen Columnstore-Index verwenden?  
Ein Columnstore-Index kann eine sehr hohe Datenkomprimierung bieten, normalerweise etwa zehnfach. Dadurch werden die Speicherkosten für ein Data Warehouse erheblich reduziert. Für Analysezwecke bietet der Columnstore-Index eine weitaus bessere Leistung als ein B-Strukturindex. Columnstore-Indizes stellen das bevorzugte Datenspeicherformat für Data Warehouse- und Analyseworkloads dar. Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]können Sie Columnstore-Indizes für Echtzeitanalysen Ihrer Betriebsarbeitsauslastung verwenden.  
  
Gründe für die hohe Geschwindigkeit von Columnstore-Indizes:  
  
-   Spalten speichern Werte aus der gleichen Domäne und weisen oft ähnliche Werte auf, was zu hohen Komprimierungsraten führt. E/A-Engpässe in Ihrem System werden minimiert oder beseitigt, und der Speicherbedarf wird deutlich reduziert.  
  
-   Hohe Komprimierungsraten verbessern die Abfrageleistung, da der Arbeitsspeicherbedarf geringer ist. Auch die Abfrageleistung kann verbessert werden, da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine größere Anzahl von speicherinternen Abfrage- und Datenvorgängen ausführen kann.  
  
-   Die Batchausführung verbessert die Abfrageleistung, in der Regel um das Zwei- bis Vierfache, indem mehrere Zeilen zusammen verarbeitet werden.  
  
-   Bei Abfragen werden häufig nur wenige Spalten aus einer Tabelle ausgewählt, wodurch das Gesamtaufkommen der E/A-Vorgänge für das physische Medium reduziert wird.  
  
## <a name="when-should-i-use-a-columnstore-index"></a>Wann sollte ein Columnstore-Index verwendet werden?  
Empfohlene Einsatzgebiete:  
  
-   Verwenden Sie einen gruppierten Columnstore-Index zum Speichern von Faktentabellen und umfangreichen Dimensionstabellen für Data Warehouse-Arbeitsauslastungen. Diese Methode verbessert die Abfrageleistung und die Datenkomprimierung um das bis zu Zehnfache. Weitere Informationen finden Sie unter [Columnstore-Indizes: Data Warehouse](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md).  
  
-   Verwenden Sie einen nicht gruppierten Columnstore-Index, um Echtzeitanalysen für OLTP-Workloads auszuführen. Weitere Informationen finden Sie unter [Erste Schritte mit Columnstore für operative Echtzeitanalyse](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
### <a name="how-do-i-choose-between-a-rowstore-index-and-a-columnstore-index"></a>Wie treffe ich die Entscheidung zwischen einem Rowstore-Index und einem Columnstore-Index?  
Rowstore-Indizes eignen sich am besten für Abfragen, die Daten bei der Suche nach einem bestimmten Wert oder mit einem kleinen Wertebereich durchsuchen. Verwenden Sie Rowstore-Indizes für Transaktionsworkloads, da sie tendenziell eher Suchvorgänge in Tabellen als Scans ganzer Tabellen erfordern.  
  
Columnstore-Indizes ermöglichen große Leistungsvorteile bei Analyseabfragen, die große Mengen von Daten durchsuchen, insbesondere bei umfangreichen Tabellen. Verwenden Sie Columnstore-Indizes für Data Warehouse- und Analyseworkloads, insbesondere für Faktentabellen, da diese eher Scans ganzer Tabellen als Suchvorgänge in Tabellen erfordern.  
  
### <a name="can-i-combine-rowstore-and-columnstore-on-the-same-table"></a>Können Rowstore und Columnstore in der gleichen Tabelle kombiniert werden?  
Ja. Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]können Sie einen aktualisierbaren, nicht gruppierten Columnstore-Index für eine Rowstore-Tabelle erstellen. Der Columnstore-Index speichert eine Kopie der ausgewählten Spalten, sodass zusätzlicher Speicherplatz benötigt wird. Allerdings werden die ausgewählten Daten im Durchschnitt um das Zehnfache komprimiert. So können Sie Analysen im Columnstore-Index und Transaktionen im Rowstore-Index zur gleichen Zeit ausführen. Der Columnstore wird aktualisiert, wenn sich Daten in der Rowstore-Tabelle ändern, daher arbeiten beide Indizes mit den gleichen Daten.  
  
Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] können Sie mindestens einen nicht gruppierten Rowstore-Index auf einem Columnstore-Index haben und auf dem zugrunde liegenden Columnstore effiziente Suchen in Tabellen durchführen. Auch weitere Optionen werden dadurch verfügbar. Beispielsweise können Sie eine Primärschlüsseleinschränkung durchsetzen, indem Sie eine UNIQUE-Bedingung auf die Rowstore-Tabelle anwenden. Da ein nicht eindeutiger Wert nicht in die Rowstore-Tabelle eingefügt werden kann, kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] den Wert nicht in den Columnstore einfügen.  
  
## <a name="metadata"></a>Metadaten  
Alle Spalten in einem Columnstore-Index werden in den Metadaten als eingeschlossene Spalten gespeichert. Der Columnstore-Index hat keine Schlüsselspalten.  

:::row:::
    :::column:::
        [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.internal_partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_column_store_row_group_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_column_store_row_group_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_column_store_object_pool &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-column-store-object-pool-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_column_store_row_group_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
 

## <a name="related-tasks"></a>Zugehörige Aufgaben  
Alle relationalen Tabellen, sofern Sie sie nicht als gruppierten Columnstore-Index festlegen, verwenden Rowstore als zugrundeliegendes Datenformat. `CREATE TABLE` erstellt eine Rowstore-Tabelle, es sei denn, Sie geben die Option `WITH CLUSTERED COLUMNSTORE INDEX` an.  
  
Beim Erstellen einer Tabelle mit der `CREATE TABLE`-Anweisung können Sie die Tabelle als Columnstore erstellen, indem Sie die Option `WITH CLUSTERED COLUMNSTORE INDEX` angeben. Wenn Sie bereits über eine Rowstore-Tabelle verfügen, die Sie in einen Columnstore konvertieren möchten, können Sie die Anweisung `CREATE COLUMNSTORE INDEX` verwenden.  
  
|Aufgabe|Referenzthemen|Notizen|  
|----------|----------------------|-----------|  
|Erstellen einer Tabelle als Columnstore.|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)|Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]können Sie die Tabelle als gruppierten Columnstore-Index erstellen. Sie müssen nicht zuerst eine Rowstore-Tabelle erstellen, die Sie dann in Columnstore konvertieren.|  
|Erstellen Sie eine In-Memory-Tabelle mit einem Columnstore-Index.|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)|Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]können Sie eine speicheroptimierte Tabelle mit einem Columnstore-Index erstellen. Der Columnstore-Index kann auch nach dem Erstellen der Tabelle mit der `ALTER TABLE ADD INDEX`-Syntax hinzugefügt werden.|  
|Konvertieren einer Rowstore-Tabelle in eine Columnstore-Tabelle.|[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)|Konvertieren Sie einen vorhandenen Heap oder eine binäre Struktur in einen Columnstore. Aus den Beispielen können Sie ersehen, wie vorhandene Indizes und der Name des Index beim Durchführen der Konvertierung behandelt werden.|  
|Konvertieren einer Columnstore-Tabelle in einen Rowstore.|[CREATE CLUSTERED INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md#d-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index) oder [Konvertieren einer Columnstore-Tabelle in einen Rowstore-Heap](../../t-sql/statements/create-columnstore-index-transact-sql.md#e-convert-a-columnstore-table-back-to-a-rowstore-heap) |In der Regel müssen Sie nicht konvertieren, allerdings kann es vorkommen, dass Sie diese Aktion durchführen müssen. Aus den Beispielen ist zu ersehen, wie ein Columnstore in einen Heap oder einen gruppierten Index konvertiert werden kann.|  
|Erstellen eines Columnstore-Index für eine Rowstore-Tabelle.|[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)|Eine Rowstore-Tabelle kann über einen Columnstore-Index verfügen. Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]kann der Columnstore-Index eine Filterbedingung aufweisen. In den Beispielen wird die grundlegende Syntax verwendet.|  
|Erstellen leistungsfähiger Indizes für Betriebsanalysen.|[Erste Schritte mit Columnstore für operative Echtzeitanalyse](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)|Beschreibt, wie sich ergänzende Columnstore- und B-Strukturindizes erstellt werden, sodass OLTP-Abfragen die B-Strukturindizes und Analyseabfragen die Columnstore-Indizes verwenden.|  
|Erstellen leistungsfähiger Columnstore-Indizes für Data Warehousing|[Columnstore-Indizes: Data Warehouse](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)|Beschreibt, wie B-Strukturindizes für Columnstore-Tabellen verwendet werden können, um leistungsstarke Data Warehousing-Abfragen zu erstellen.|  
|Verwenden eines B-Strukturindex zum Durchsetzen einer Primärschlüsseleinschränkung für einen Columnstore-Index.|[Columnstore-Indizes: Data Warehouse](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)|Zeigt, wie B-Struktur- und Columnstore-Indizes kombiniert werden können, um Primärschlüsseleinschränkungen für den Columnstore-Indizes durchzusetzen.|  
|Löschen eines Columnstore-Indexes.|[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)|Beim Löschen eines Columnstore-Indexes wird die standardmäßige `DROP INDEX`-Syntax verwendet, die auch B-Strukturindizes verwenden. Beim Löschen eines gruppierten Columnstore-Index wird die Columnstore-Tabelle in einen Heap konvertiert.|  
|Löschen einer Zeile aus einem Columnstore-Index.|[DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)|Verwenden Sie [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md) zum Löschen einer Zeile.<br /><br /> **columnstore row**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] markiert die Zeile als logisch gelöscht, der physische Speicherplatz für die Zeile wird jedoch erst wieder freigegeben, wenn der Index neu erstellt wurde.<br /><br /> **deltastore row**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] löscht die Zeile logisch und physisch.|  
|Aktualisieren einer Zeile im Columnstore-Index.|[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)|Verwenden Sie [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md) , um eine Zeile zu aktualisieren.<br /><br /> **columnstore row**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] markiert die Zeile als logisch gelöscht und fügt die aktualisierte Zeile dann in den Deltastore ein.<br /><br /> **deltastore row**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aktualisiert die Zeile im Deltastore.|  
|Laden von Daten in einen Columnstore-Index.|[Columnstore-Indizes: Laden von Daten](~/relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)||  
|Durchsetzen, dass alle Zeilen im Deltastore in den Columnstore wechseln.|[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) ... `REBUILD`<br /><br /> [Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)|`ALTER INDEX` mit der Option `REBUILD` erzwingt das Verschieben aller Zeilen in den Columnstore.|  
|Defragmentieren eines Columnstore-Index.|[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)|`ALTER INDEX ... REORGANIZE` defragmentiert Columnstore-Indizes online.|  
|Zusammenführen von Tabellen mit Columnstore-Indizes.|[MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)||  
  
## <a name="see-also"></a>Weitere Informationen  
 [Columnstore-Indizes: Laden von Daten](~/relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Columnstore-Indizes: Zusammenfassung der Features für Produktversionen](~/relational-databases/indexes/columnstore-indexes-what-s-new.md)   
 [Columnstore-Indizes: Abfrageleistung](~/relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Erste Schritte mit Columnstore für operative Echtzeitanalyse](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)   
 [Columnstore-Indizes: Data Warehouse](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Columnstore-Index-Defragmentierung](~/relational-databases/indexes/columnstore-indexes-defragmentation.md)   
 [Leitfaden zum Design von SQL Server-Indizes](../../relational-databases/sql-server-index-design-guide.md)   
 [Columnstore-Indizes: Architektur](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)   
  
  
