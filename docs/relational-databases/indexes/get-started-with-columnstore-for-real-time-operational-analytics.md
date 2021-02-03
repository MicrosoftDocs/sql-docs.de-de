---
description: Erste Schritte mit Columnstore für die operative Echtzeitanalyse
title: Erste Schritte mit Columnstore für die operative Echtzeitanalyse | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/08/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: quickstart
ms.assetid: e1328615-6b59-4473-8a8d-4f360f73187d
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dbc0867b71cadb33b1eb9174132b2031134a72f0
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236935"
---
# <a name="get-started-with-columnstore-for-real-time-operational-analytics"></a>Erste Schritte mit Columnstore für die operative Echtzeitanalyse
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Mit [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] wird die operative Echtzeitanalyse eingeführt, die Möglichkeit, Analyse- und OLTP-Workloads zugleich auf den gleichen Datenbanktabellen auszuführen. Damit können Sie nicht nur Analysen in Echtzeit ausführen, sondern benötigen auch keine ETL-Aufträge und kein Data Warehouse mehr.  
  
## <a name="real-time-operational-analytics-explained"></a>Grundlagen der operativen Echtzeitanalyse  
 Bisher verwendeten Unternehmen separate Systeme für operative (d. h. OLTP) und Analysearbeitsauslastungen. Bei derartigen Systemen verschieben ETL-Aufträge (Extrahieren, Transformieren, Laden) die Daten aus dem Betriebs- in den Analysespeicher. Die Analysedaten sind normalerweise in einem Data Warehouse oder Data Mart gespeichert, die dediziert für die Ausführung von Analyseabfragen verwendet werden. Diese Lösung hat sich zwar als Standard etabliert, sie sieht sich jedoch diesen drei Herausforderungen gegenüber:  
  
-   **Komplexität.** Für das Implementieren von ETL kann Codeerstellung in erheblichem Umfang erforderlich werden, insbesondere, um nur geänderte Zeilen zu laden. Es kann schwierig sein, die geänderten Zeilen zu bestimmen.  
  
-   **Kosten:** Die Implementierung von ETL verursacht Kosten durch den Erwerb von Hardware und zusätzlicher Softwarelizenzen.  
  
-   **Datenlatenz.** Die Implementierung von ETL bringt eine zeitliche Verzögerung mit sich, die durch die Ausführung der Analyse bedingt ist. Wenn der ETL-Auftrag beispielsweise am Ende jedes Geschäftstags ausgeführt wird, werden die Analyseabfragen auf Daten ausgeführt, die mindestens einen Tag alt sind. Für viele Unternehmen ist diese Verzögerung nicht akzeptabel, da das Unternehmen von einer Analyse der Daten in Echtzeit abhängig ist. Beispielsweise ist für die Erkennung von Betrugsversuchen eine Echtzeitanalyse der Betriebsdaten erforderlich.  
  
 ![Übersicht über Real-Time Operational Analytics](../../relational-databases/indexes/media/real-time-operational-analytics-overview.png "Übersicht über Real-Time Operational Analytics")  
  
Die operative Echtzeitanalyse stellt eine Lösung für diese Herausforderungen bereit.   
Wenn Analyse- und OLTP-Arbeitsauslastungen auf der gleichen zugrundeliegenden Tabelle ausgeführt werden, tritt keine Zeitverzögerung ein.   In Szenarien, die Echtzeitanalyse verwenden können, lassen sich Kosten und Komplexität stark verringern, da die Notwendigkeit von ETL sowie von Erwerb und Wartung eines separaten Data Warehouses entfallen.  
  
> [!NOTE]  
>  Die operative Echtzeitanalyse zielt auf das Szenario einer einzelnen Datenquelle ab, etwa einer ERP-Anwendung (Enterprise Resource Planning), auf der sowohl die betriebs- als auch die analysebedingte Arbeitsauslastung ausgeführt werden kann. Die Notwendigkeit eines separaten Data Warehouses entfällt dadurch nicht, wenn vor der Ausführung der Analysearbeitsauslastung Daten aus mehreren Quellen integriert werden müssen, oder für die Analyse extreme Leistungsfähigkeit mithilfe zuvor aggregierter Daten, wie etwa Cubes, erforderlich ist.  
  
 Echtzeitanalysen verwenden einen aktualisierbaren Columnstore-Index in einer Rowstore-Tabelle. Der Columnstore-Index unterhält eine Kopie der Daten, sodass die OLTP- und Analysearbeitsauslastungen auf separaten Kopien der Daten ausgeführt werden. Dadurch wird die Leistungseinbuße durch die gleichzeitige Ausführung beider Arbeitsauslastungen minimiert. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwaltet Indexänderungen automatisch, sodass OLTP-Änderungen jederzeit für die Analyse auf dem aktuellen Stand verfügbar sind. Mit diesem Entwurf ist es möglich und praktikabel, die Analyse in Echtzeit auf aktuellen Daten auszuführen. Dies funktioniert sowohl für datenträgerbasierte als auch für speicheroptimierte Tabellen.  
  
## <a name="get-started-example"></a>Beispiel für den Einstieg  
 So steigen Sie in die Echtzeitanalyse ein:  
  
1.  Bestimmen Sie die Tabellen in Ihrem operationalen Schema, die Daten enthalten, die für die Analyse benötigt werden.  
  
2.  Löschen Sie für jede Tabelle alle B-Strukturindizes, deren Hauptzweck im Beschleunigen der vorhandenen Analyse für Ihre OLTP-Arbeitsauslastung besteht. Ersetzen Sie sie durch einen einzelnen Columnstore-Index.  Dadurch wird möglicherweise die Gesamtleistung Ihrer OLTP-Arbeitsauslastung verbessert, da weniger Indizes gewartet werden müssen.  
  
    ```sql  
    --This example creates a nonclustered columnstore index on an existing OLTP table.  
    --Create the table  
    CREATE TABLE t_account (  
        accountkey int PRIMARY KEY,  
        accountdescription nvarchar (50),  
        accounttype nvarchar(50),  
        unitsold int  
    );  
  
    --Create the columnstore index with a filtered condition  
    CREATE NONCLUSTERED COLUMNSTORE INDEX account_NCCI   
    ON t_account (accountkey, accountdescription, unitsold)   
    ;  
    ```  
  
     Der Columnstore-Index für eine Tabelle im Arbeitsspeicher ermöglicht die operative Echtzeitanalyse durch die Integration von In-Memory-OLTP- und In-Memory-Columnstore-Technologien, durch die hohe Leistung sowohl für die OLTP- als auch für die analysebedingten Workloads möglich werden. Der Columnstore-Index für eine Tabelle im Arbeitsspeicher muss alle Spalten beinhalten.  
  
    ```sql  
    -- This example creates a memory-optimized table with a columnstore index.  
    CREATE TABLE t_account (  
        accountkey int NOT NULL PRIMARY KEY NONCLUSTERED,  
        Accountdescription nvarchar (50),  
        accounttype nvarchar(50),  
        unitsold int,  
        INDEX t_account_cci CLUSTERED COLUMNSTORE  
        )  
        WITH (MEMORY_OPTIMIZED = ON );  
    GO  
  
    ```  
  
3.  Das ist alles, was Sie tun müssen!  

 Sie können jetzt operative Echtzeitanalyse ausführen, ohne Änderungen an Ihrer Anwendung vornehmen zu müssen.  Die Analyseabfragen werden auf dem Columnstore-Index ausgeführt, während die OLTP-Operationen auch weiterhin auf den OLTP-B-Strukturindizes ausgeführt werden. Die OLTP-Arbeitsauslastungen werden auch weiterhin mit hoher Leistung ausgeführt, bringen jedoch einen gewissen Mehraufwand für die Wartung des Columnstore-Index mit sich. Informationen über Leistungsoptimierungen finden Sie im nächsten Abschnitt.  
  
## <a name="blog-posts"></a>Blogbeiträge  
 In den folgenden Blogbeiträgen erfahren Sie mehr über die operative Echtzeitanalyse. Möglicherweise sind die Abschnitte zu Leistungstipps leichter zu verstehen, wenn Sie sich zuerst die Blogbeiträge ansehen.  
  
-   [Business Case für operative Echtzeitanalyse (Business case for real-time operational analytics)](https://blogs.technet.microsoft.com/dataplatforminsider/2015/12/09/real-time-operational-analytics-using-in-memory-technology/)  
  
-   [Verwendung eines nicht gruppierten Columnstore-Index für operative Echtzeitanalyse (Using a nonclustered columnstore index for real-time operational analytics)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-using-nonclustered-columnstore-index)  
  
-   [Ein einfaches Beispiel, das einen nicht gruppierten Columnstore-Index verwendet (A simple example using a nonclustered columnstore index)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-simple-example-using-nonclustered-clustered-columnstore-index-ncci)  
  
-   [Wie SQL Server einen nicht gruppierten Columnstore-Index für eine transaktionale Arbeitsauslastung aufrecht erhält (How SQL Server maintains a nonclustered columnstore index on a transactional workload)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-dml-operations-and-nonclustered-columnstore-index-ncci-in-sql-server-2016)  
  
-   [Minimieren des Einflusses der Wartung eines nicht gruppierten Columnstore-Index durch Verwendung eines gefilterten Index (Minimizing the impact of nonclustered columnstore index maintenance by using a filtered index)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-filtered-nonclustered-columnstore-index-ncci)  
  
-   [Minimieren des Einflusses der Wartung eines nicht gruppierten Columnstore-Index durch Einsatz von verzögerter Komprimierung (Minimizing the impact of nonclustered columnstore index maintenance by using compression delay)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-for-nonclustered-columnstore-index-ncci)  
  
-   [Minimieren des Einflusses der Wartung eines nicht gruppierten Columnstore-Index durch Einsatz von verzögerter Komprimierung – Zahlen zur Leistung (Minimizing impact of a nonclustered columnstore index maintenance by using compression delay - performance numbers)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-with-ncci-and-the-performance)  
  
-   [Operative Echtzeitanalyse mit speicheroptimierten Tabellen (Real time operational analytics with memory-optimized tables)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-memory-optimized-table-and-columnstore-index)  
  
-   [Columnstore-Index und die Zusammenführungsrichtlinie für Zeilengruppen (Columnstore index and the merge policy for rowgroups)](/archive/blogs/sqlserverstorageengine/columnstore-index-merge-policy-for-reorganize)  
  
## <a name="performance-tip-1-use-filtered-indexes-to-improve-query-performance"></a>Leistungstipp Nr. 1: Verwenden von gefilterten Indizes zum Verbessern der Abfrageleistung  
 Das Ausführen der operativen Echtzeitanalyse kann die Leistung der OLTP-Arbeitsauslastung beeinträchtigen. Dieser Einfluss sollte so klein wie möglich sein. Dieses Beispiel zeigt, wie gefilterte Indizes verwendet werden können, um den Einfluss eines nicht gruppierten Columnstore-Indexes auf die transaktionale Arbeitsauslastung zu minimieren, während zugleich eine Echtzeitanalyse bereitgestellt wird.  
  
 Um den Mehraufwand für die Wartung eines nicht gruppierten Columnstore-Index für eine Betriebsarbeitsauslastung zu minimieren, können Sie eine gefilterte Bedingung verwenden, um einen nicht gruppierten Columnstore-Index nur für die *warmen* oder sich langsam ändernden Daten zu erstellen. Beispielsweise können Sie in einer Anwendung zur Bestellungsverwaltung einen nicht gruppierten Columnstore-Index für die bereits versendeten Bestellungen erstellen. Nach dem Versand ändert sich eine Bestellung in der Regel nicht mehr, daher können diese Daten als „warm“ angesehen werden. Bei einem gefilterten Index erfordern die Daten im nicht gruppierten Columnstore-Index weniger Aktualisierungen, wodurch sich der Einfluss auf die Transaktionsarbeitsauslastung verringert.  
  
 Analyseabfragen greifen bei Bedarf transparent sowohl auf „warme“ als auch auf „heiße“ Daten zu, um Echtzeitanalyse bereitzustellen. Wenn ein erheblicher Anteil der Betriebsworkload die „heißen“ Daten betrifft, ist für diese Vorgänge keine zusätzliche Wartung des Columnstore-Index erforderlich. Eine bewährte Methode ist die Erstellung eines gruppierten Rowstore-Index für die in der gefilterten Indexdefinition verwendeten Spalten. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet den gruppierten Index, um schnell die Zeilen zu durchsuchen, die der Filterbedingung nicht erfüllt haben. Ohne diesen gruppierten Index ist ein vollständiger Scan der Rowstore-Tabelle erforderlich, um diese Zeilen zu finden, was sich deutlich negativ auf die Leistung der Analyseabfrage auswirken kann. Ohne Einsatz eines gruppierten Index lässt sich auch ein komplementär gefilterter, nicht gruppierter B-Strukturindex zur Bestimmung dieser Zeilen verwenden, jedoch ist das nicht zu empfehlen, da der Zugriff auf einen großen Zeilenbereich über nicht gruppierte B-Strukturindizes aufwändig ist.  
  
> [!NOTE]  
>  Ein gefilterter nicht gruppierter Columnstore-Index wird nur für datenträgerbasierte Tabellen unterstützt. Für speicheroptimierte Tabellen wird er nicht unterstützt  
  
### <a name="example-a-access-hot-data-from-btree-index-warm-data-from-columnstore-index"></a>Beispiel A: Zugriff auf „heiße“ Daten über einen B-Strukturindex, auf „warme“ Daten über einen Columnstore-Index  
 In diesem Beispiel wird eine gefilterte Bedingung (accountkey > 0) verwendet, um festzulegen, welche Zeilen im Columnstore-Index enthalten sind. Dies hat den Zweck, die Filterbedingung und die nachfolgenden Abfragen so zu gestalten, dass auf sich häufig ändernde „heiße“ Daten über den B-Strukturindex, auf die stabileren „warmen“ Daten über den Columnstore-Index zugegriffen wird.  
  
 ![Kombinierte Indizes für warme und heiße Daten](../../relational-databases/indexes/media/de-columnstore-warmhotdata.png "Kombinierte Indizes für warme und heiße Daten")  
  
> [!NOTE]  
>  Der Abfrageoptimierer zieht den Columnstore-Index für den Abfrageplan in Betracht, wählt ihn aber nicht in jedem Fall aus. Wenn der Abfrageoptimierer den gefilterten Columnstore-Index wählt, kombiniert er transparent die Zeilen aus dem Columnstore-Index mit den Zeilen, die der Filterbedingung nicht entsprechen, um Echtzeitanalyse zu ermöglichen. Dies unterscheidet sich von einem gewöhnlichen nicht gruppierten gefilterten Index, der nur in Abfragen verwendet werden kann, die sich auf die im Index vorhandenen Zeilen beschränken.  
  
```sql  
--Use a filtered condition to separate hot data in a rowstore table  
-- from "warm" data in a columnstore index.  
  
-- create the table  
CREATE TABLE  orders (  
         AccountKey         int not null,  
         Customername       nvarchar (50),  
        OrderNumber         bigint,  
        PurchasePrice       decimal (9,2),  
        OrderStatus         smallint not null,  
        OrderStatusDesc     nvarchar (50))  
  
-- OrderStatusDesc  
-- 0 => 'Order Started'  
-- 1 => 'Order Closed'  
-- 2 => 'Order Paid'  
-- 3 => 'Order Fullfillment Wait'  
-- 4 => 'Order Shipped'  
-- 5 => 'Order Received'  
  
CREATE CLUSTERED INDEX  orders_ci ON orders(OrderStatus)  
  
--Create the columnstore index with a filtered condition  
CREATE NONCLUSTERED COLUMNSTORE INDEX orders_ncci ON orders  (accountkey, customername, purchaseprice, orderstatus)  
where orderstatus = 5  
;  
  
-- The following query returns the total purchase done by customers for items > $100 .00  
-- This query will pick  rows both from NCCI and from 'hot' rows that are not part of NCCI  
SELECT top 5 customername, sum (PurchasePrice)  
FROM orders  
WHERE purchaseprice > 100.0   
Group By customername  
```  
  
 Die Analyseabfrage wird mit dem folgenden Abfrageplan ausgeführt. Es ist zu sehen, dass der Zugriff auf die Zeilen, die der Filterbedingung nicht entsprechen, über den gruppierten B-Strukturindex erfolgt.  
  
 ![Abfrageplan](../../relational-databases/indexes/media/query-plan-columnstore.png "Abfrageplan")  
  
 Weitere Informationen zu [gefilterten, nicht gruppierten Columnstore-Indizes](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-filtered-nonclustered-columnstore-index-ncci)finden Sie im Blog.  
  
## <a name="performance-tip-2-offload-analytics-to-always-on-readable-secondary"></a>Leistungstipp Nr. 2: Auslagern der Analyse auf eine schreibgeschützte sekundäre Always On-Datenbank  
 Zwar kann der Wartungsaufwand für den Columnstore-Index durch Verwendung eines gefilterten Columnstore-Index minimiert werden, die Analyseabfragen können jedoch trotzdem erhebliche Computerressoucen (CPU, E/A, Arbeitsspeicher) in Anspruch nehmen, was sich negativ auf die für die Betriebsworkload verfügbare Leistung auswirkt. Für die meisten unternehmenswichtigen Arbeitsauslastungen ergibt sich als unsere Empfehlung die Always On-Konfiguration. In dieser Konfiguration kann der Einfluss der Ausführung der Analyse beseitigt werden, indem sie in eine schreibgeschützte sekundäre Datenbank ausgelagert wird.  
  
## <a name="performance-tip-3-reducing-index-fragmentation-by-keeping-hot-data-in-delta-rowgroups"></a>Leistungstipp Nr. 3: Reduzierung der Indexfragmentierung durch Speicherung der „heißen“ Daten in Deltazeilengruppen  
 Tabellen mit Columnstore-Index können stark fragmentiert werden (in Form von gelöschten Zeilen), wenn durch die Arbeitsauslastung Zeilen aktualisiert/gelöscht werden, die komprimiert wurden. Ein fragmentierter Columnstore-Index führt zu einer ineffizienten Auslastung von Arbeitsspeicher/Speicherplatz. Neben dem ineffizienten Ressourceneinsatz wirkt er sich auch negativ auf die Analyseabfrageleistung aus, da zusätzliche E/A-Vorgänge anfallen und es erforderlich ist, die gelöschten Zeilen aus dem Resultset zu filtern.  
  
 Die gelöschten Zeilen werden physisch erst beim Ausführen der Indexdefragmentierung mit dem Befehl `REORGANIZE` oder durch Neuerstellung des Columnstore-Index für die gesamte Tabelle oder die betroffene(n) Partition(en) entfernt. Sowohl `REORGANIZE` als auch `REBUILD` sind aufwändige Indexvorgänge, die Ressourcen beanspruchen, die andernfalls für die Workload zur Verfügung stünden. Ferner kann ein zu frühes Komprimieren von Zeilen dazu führen, dass sie aufgrund von Aktualisierungen mehrfach erneut komprimiert werden müssen, was einen unnützen Mehraufwand für die Komprimierung verursacht.  
Die Indexfragmentierung kann mithilfe der Option `COMPRESSION_DELAY` minimiert werden.  
  
```sql  
-- Create a sample table  
CREATE TABLE t_colstor (  
               accountkey                      int not null,  
               accountdescription              nvarchar (50) not null,  
               accounttype                     nvarchar(50),  
               accountCodeAlternatekey         int)  
  
-- Creating nonclustered columnstore index with COMPRESSION_DELAY. The columnstore index will keep the rows in closed delta rowgroup for 100 minutes   
-- after it has been marked closed  
CREATE NONCLUSTERED COLUMNSTORE index t_colstor_cci on t_colstor (accountkey, accountdescription, accounttype)   
                       WITH (DATA_COMPRESSION= COLUMNSTORE, COMPRESSION_DELAY = 100);  
  
;  
```  
  
 Im Blog finden Sie Details zur [verzögerten Komprimierung](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-for-nonclustered-columnstore-index-ncci).  
  
 Im Folgenden werden die empfohlenen bewährten Methoden aufgeführt:  
  
-   **Einfüge-/Abfrageworkload:** Wenn Ihre Workload in erster Linie Daten einfügt und sie abfragt, stellt der Standardwert „0“ (null) für COMPRESSION_DELAY die empfohlene Option dar. Die neu eingefügten Zeilen werden komprimiert, sobald eine Million Zeilen in eine einzelne Deltazeilengruppe eingefügt wurden.  
    Beispiele für eine solche Arbeitsauslastung sind (a) eine traditionelle DW-Arbeitsauslastung und (b) eine Klickdatenstrom-Analyse, bei der das Klickmuster in einer Webanwendung analysiert werden soll.  
  
-   **OLTP-Workload:** Wenn die Arbeitsauslastung sehr DML-intensiv ist (also eine komplexe Mischung aus UPDATE-, DELETE- und INSERT-Vorgängen), lässt sich die Fragmentierung des Columnstore-Index durch Untersuchen von DMV „sys. dm_db_column_store_row_group_physical_stats“ bestimmen. Wenn Sie dabei sehen, dass mehr als 10 % der Zeilen in kürzlich komprimierten Zeilengruppen als gelöscht markiert wurden, können Sie die Option COMPRESSION_DELAY verwenden, um eine Zeitverzögerung bis zur Qualifikation der Zeilen zur Komprimierung hinzuzufügen. Wenn bei Ihrer Arbeitsauslastung neu eingefügte Datensätze normalerweise für etwa 60 Minuten „heiß“ bleiben (d. h. in dieser Zeit mehrfach aktualisiert werden), sollten Sie COMPRESSION_DELAY auf 60 festlegen.  
  
 Wir gehen davon aus, dass die meisten Kunden keine Anpassungen vornehmen müssen. Der Standardwert der Option COMPRESSION_DELAY sollte für ihren Fall funktionieren.  
Fortgeschrittenen Benutzern empfehlen wir, die Abfrage unten auszuführen und den Prozentsatz der gelöschten Zeilen im Lauf der letzten 7 Tage zu bestimmen.  
  
```sql  
SELECT row_group_id,cast(deleted_rows as float)/cast(total_rows as float)*100 as [% fragmented], created_time  
FROM sys. dm_db_column_store_row_group_physical_stats  
WHERE object_id = object_id('FactOnlineSales2')   
             AND  state_desc='COMPRESSED'   
             AND deleted_rows>0   
             AND created_time > GETDATE() - 7  
ORDER BY created_time DESC;  
```  
  
 Wenn die Anzahl der gelöschten Zeilen in komprimierten Zeilengruppen mehr als 20 % beträgt, mit einer Häufung in älteren Zeilengruppen mit weniger als 5 % Variation (die als „kalte“ Zeilengruppen bezeichnet werden), legen Sie COMPRESSION_DELAY = (Erstellungszeit_der_jüngsten_Zeilengruppe – aktuelle_Zeit) fest. Beachten Sie, dass dieser Ansatz sich besonders für eine stabile und relativ homogene Arbeitslast eignet.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beschreibung von Columnstore-Indizes](../../relational-databases/indexes/columnstore-indexes-overview.md)   
 [Laden von Daten für Columnstore-Indizes](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Abfrageleistung für Columnstore-Indizes](../../relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Columnstore-Indizes für Data Warehousing](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)
