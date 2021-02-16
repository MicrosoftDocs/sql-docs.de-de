---
description: CREATE STATISTICS (Transact-SQL)
title: CREATE STATISTICS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/04/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STATISTICS
- STATISTICS_TSQL
- CREATE STATISTICS
- CREATE_STATISTICS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- query optimization statistics [SQL Server], creating
- indexed views [SQL Server], statistics
- FULLSCAN option
- CREATE STATISTICS statement
- filtered statistics [SQL Server]
- creating statistics [SQL Server]
- NORECOMPUTE clause
ms.assetid: b23e2f6b-076c-4e6d-9281-764bdb616ad2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7a86a2cfbe236aad6e37e0f8895f2be29ec1c5ae
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100344024"
---
# <a name="create-statistics-transact-sql"></a>CREATE STATISTICS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Erstellt Abfrageoptimierungsstatistiken in einer oder mehreren Spalten einer Tabelle, einer indizierten Sicht oder einer externen Tabelle. Bei den meisten Abfragen generiert der Abfrageoptimierer automatisch die notwendigen Statistiken für einen hochwertigen Abfrageplan; in einigen Fällen müssen Sie weitere Statistiken mithilfe von CREATE STATISTICS erstellen oder den Abfrageentwurf ändern, um die Abfrageleistung zu verbessern.  
  
 Weitere Informationen finden Sie unter [Statistiken](../../relational-databases/statistics/statistics.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Create statistics on an external table  
CREATE STATISTICS statistics_name   
ON { table_or_indexed_view_name } ( column [ ,...n ] )   
    [ WITH FULLSCAN ] ;  
  
-- Create statistics on a regular table or indexed view  
CREATE STATISTICS statistics_name   
ON { table_or_indexed_view_name } ( column [ ,...n ] )   
    [ WHERE <filter_predicate> ]  
    [ WITH   
        [ [ FULLSCAN   
            [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
          | SAMPLE number { PERCENT | ROWS }   
            [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
          | <update_stats_stream_option> [ ,...n ]    
        [ [ , ] NORECOMPUTE ]   
        [ [ , ] INCREMENTAL = { ON | OFF } ] 
        [ [ , ] MAXDOP = max_degree_of_parallelism ]
    ] ;  
  
<filter_predicate> ::=   
    <conjunct> [AND <conjunct>]  
  
<conjunct> ::=  
    <disjunct> | <comparison>  
  
<disjunct> ::=  
        column_name IN (constant ,...)  
  
<comparison> ::=  
        column_name <comparison_op> constant  
  
<comparison_op> ::=  
    IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !<  
    
<update_stats_stream_option> ::=  
    [ STATS_STREAM = stats_stream ]  
    [ ROWCOUNT = numeric_constant ]  
    [ PAGECOUNT = numeric_contant ] 
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE STATISTICS statistics_name   
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( column_name  [ ,...n ] )   
    [ WHERE <filter_predicate> ]  
    [ WITH {  
           FULLSCAN   
           | SAMPLE number PERCENT   
      }  
    ]  
[;]  
  
<filter_predicate> ::=   
    <conjunct> [AND <conjunct>]  
  
<conjunct> ::=  
    <disjunct> | <comparison>  
  
<disjunct> ::=  
        column_name IN (constant ,...)  
  
<comparison> ::=  
        column_name <comparison_op> constant  
  
<comparison_op> ::=  
    IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !<  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *statistics_name*  
 Der Name der zu erstellenden Statistik.  
  
 *table_or_indexed_view_name*  
 Der Name der Tabelle, der indizierten Sicht oder der externen Tabelle, für die die Statistik erstellt werden soll. Legen Sie einen qualifizierten Tabellennamen fest, um Statistiken für eine andere Datenbank zu erstellen.  
  
 *Spalte [ ,...n]*  
 Mindestens eine Spalte, die in den Statistiken enthalten sein soll. Die Spalten sollten von links nach rechts nach Priorität geordnet sein. Nur die erste Spalte wird zum Erstellen des Histrogramms verwendet. Alle Spalten werden für spaltenübergreifende Statistiken verwendet, die als „Dichten“ bezeichnet werden.  
  
 Sie können beliebige Spalten angeben, die von folgenden Ausnahmen abgesehen als Indexschlüsselspalte angegeben werden können:  
  
-   **XML**-, Volltext- und FILESTREAM-Spalten können nicht angegeben werden.  
  
-   Berechnete Spalten können nur angegeben werden, wenn die DARITHABORT-Datenbankeinstellung und die QUOTED_IDENTIFIER-Datenbankeinstellung auf ON festgelegt sind.  
  
-   Spalten des CLR-benutzerdefiniertne Typs können angegeben werden, wenn der Typ die binäre Reihenfolge unterstützt. Berechnete Spalten, die als Methodenaufrufe einer Spalte eines benutzerdefinierten Typs definiert sind, können angegeben werden, wenn die Methoden als deterministisch gekennzeichnet sind.  
  
 WHERE \<filter_predicate> Gibt einen Ausdruck zum Auswählen einer Teilmenge von Zeilen an, die beim Erstellen des Statistikobjekts eingeschlossen werden sollen. Statistiken, die mit einem Filterprädikat erstellt werden, werden als gefilterte Statistiken bezeichnet. Im Filterprädikat werden einfache Vergleichsoperatoren verwendet. Es darf darin nicht auf eine berechnete Spalte, eine UDT-Spalte, eine Spalte mit einem räumlichen Datentyp oder eine Spalten mit dem **hierarchyID**-Datentyp verwiesen werden. Vergleiche mit NULL-Literalen sind mit den Vergleichsoperatoren nicht zulässig. Verwenden Sie stattdessen den IS NULL-Operator und den IS NOT NULL-Operator.  
  
 Es folgen einige Beispiele für Filterprädikate für die Production.BillOfMaterials-Tabelle:  
  
 * `WHERE StartDate > '20000101' AND EndDate <= '20000630'`  
  
 * `WHERE ComponentID IN (533, 324, 753)`  
  
 * `WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL`  
  
 Weitere Informationen zu Filterprädikaten finden Sie unter [Create Filtered Indexes (Erstellen gefilterter Indizes)](../../relational-databases/indexes/create-filtered-indexes.md).  
  
 FULLSCAN  
 Berechnet die Statistiken, indem alle Zeilen überprüft werden. FULLSCAN und SAMPLE 100 PERCENT führen zu gleichen Ergebnissen. FULLSCAN kann nicht in Verbindung mit der SAMPLE-Option verwendet werden.  
  
 Wenn diese Option ausgelassen wird, verwendet SQL Server Stichproben, um die Statistiken zu erstellen. Zudem wird die Größe der Stichprobe bestimmt, die erforderlich ist, um einen hochwertigen Abfrageplan zu erstellen.  
  
 SAMPLE *Zahl* { PERCENT | ROWS }  
 Gibt den ungefähren Prozentsatz oder die ungefähre Anzahl von Zeilen in der Tabelle oder indizierten Sicht an, die vom Abfrageoptimierer beim Erstellen von Statistiken verwendet werden sollen. Für PERCENT kann *Zahlenwerte* von 0 bis 100 annehmen, für ROWS kann *Zahlenwerte* von 0 bis zur Gesamtanzahl der Zeilen annehmen. Der tatsächliche Prozentsatz oder die tatsächliche Anzahl von Zeilen, die vom Abfrageoptimierer als Stichprobe entnommen werden, stimmt möglicherweise nicht mit dem angegebenen Prozentsatz oder der angegebenen Anzahl überein. Der Abfrageoptimierer scannt z. B. alle Zeilen auf einer Datenseite.  
  
 SAMPLE eignet sich für Sonderfälle, in denen der auf Standardstichproben beruhende Abfrageplan nicht optimal ist. In den meisten Situationen muss SAMPLE nicht angegeben werden, da der Abfrageoptimierer automatisch Stichproben verwendet und die statistisch signifikante Stichprobengröße ermittelt, wie zum Erstellen hochwertiger Abfragepläne erforderlich.  
  
 SAMPLE kann nicht in Verbindung mit der Option FULLSCAN verwendet werden. Wenn weder SAMPLE noch FULLSCAN angegeben wurde, verwendet der Abfrageoptimierer Stichprobendaten und berechnet die Stichprobengröße anhand der Standardeinstellungen.  
  
 Es wird davon abgeraten, 0 PERCENT oder 0 ROWS anzugeben. Wenn 0 PERCENT oder ROWS angegeben ist, wird das Statistikobjekt erstellt, es enthält jedoch keine Statistikdaten.  
 
 PERSIST_SAMPLE_PERCENT = { ON | OFF }  
 Bei **ON** behalten die Statistiken den bei der Erstellung angegebenen Prozentsatz für die Stichprobenentnahme für nachfolgende Updates bei, die keinen expliziten Prozentsatz für die Stichprobenentnahme angeben. Bei **OFF** wird der Prozentsatz für die Stichprobenentnahme in nachfolgenden Updates auf den Standardwert zurückgesetzt, sofern diese keinen expliziten Prozentsatz für die Stichprobenentnahme angeben. Der Standardwert ist **OFF**. 
 
 > [!NOTE]
 > Wenn die Tabelle abgeschnitten wird, übernehmen alle Statistiken, die basierend auf dem abgeschnittenen HoBT erstellt wurden, wieder den Standardstichproben-Prozentsatz.

 **Gilt für**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU4) und höher (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU1).    
  
 STATS_STREAM **=** _stats_stream_  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 NORECOMPUTE  
 Deaktiviert die AUTO_UPDATE_STATISTICS-Option zur automatischen Statistikaktualisierung für *statistics_name*. Wenn diese Option angegeben wird, schließt der Abfrageoptimierer alle laufenden Statistikupdates für *statistics_name* ab und deaktiviert zukünftige Updates.  
  
 Entfernen Sie die Statistiken mit [DROP STATISTICS](../../t-sql/statements/drop-statistics-transact-sql.md), und führen Sie dann CREATE STATISTICS ohne die NORECOMPUTE-Option aus, um Statistikupdates wieder zu aktivieren.  
  
> [!WARNING]  
> Bei Verwendung dieser Option können suboptimale Abfragepläne entstehen. Es wird empfohlen, diese Option nur in Einzelfällen von einem qualifizierten Systemadministrator vornehmen zu lassen.  
  
 Weitere Informationen über die Option AUTO_STATISTICS_UPDATE finden Sie unter [ALTER DATABASE SET-Optionen &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md). Weitere Informationen zum Deaktivieren und erneuten Aktivieren von Statistikupdates finden Sie unter [Statistiken](../../relational-databases/statistics/statistics.md).  
  
 INCREMENTAL = { ON | OFF }  
 Bei **ON** wird die Statistik pro Partition erstellt. Bei **OFF** werden Statistiken für alle Partitionen kombiniert. Der Standardwert ist **OFF**.  
  
 Wenn Statistiken pro Partition nicht unterstützt werden, wird ein Fehler generiert. Inkrementelle Statistiken werden für folgende Statistiktypen nicht unterstützt:  
  
-   Statistiken, die mit Indizes erstellt wurden, die über keine Partitionsausrichtung mit der Basistabelle verfügen.  
-   Statistiken, die für lesbare sekundäre Always On-Datenbanken erstellt wurden.  
-   Statistiken, die für schreibgeschützte Datenbanken erstellt wurden.  
-   Statistiken, die für gefilterte Indizes erstellt wurden.  
-   Statistiken, die für Sichten erstellt wurden.  
-   Statistiken, die für interne Tabellen erstellt wurden.  
-   Statistiken, die mit räumlichen Indizes oder XML-Indizes erstellt wurden.  
  
**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.  
  
MAXDOP = *max_degree_of_parallelism*  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3).  
  
 Überschreibt die Konfigurationsoption **max degree of parallelism** (Max. Grad an Parallelität) für die Dauer des Statistikvorgangs. Weitere Informationen finden Sie unter [Konfigurieren der Serverkonfigurationsoption Max. Grad an Parallelität](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Sie können mit MAXDOP die Anzahl der Prozessoren begrenzen, die bei der Ausführung paralleler Pläne verwendet werden. Maximal sind 64 Prozessoren zulässig.  
  
 *max_degree_of_parallelism* kann folgende Werte haben:  
  
 1  
 Unterdrückt das Generieren paralleler Pläne.  
  
 \>1  
 Beschränkt die maximale Anzahl der Prozessoren, die bei einem parallelen Statistikvorgang verwendet werden, je nach aktueller Systemauslastung auf die angegebene Zahl oder einen niedrigeren Wert.  
  
 0 (Standard)  
 Verwendet abhängig von der aktuellen Systemarbeitsauslastung die tatsächliche Anzahl von Prozessoren oder weniger Prozessoren.  
  
 \<update_stats_stream_option> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  

## <a name="permissions"></a>Berechtigungen  
 Erfordert eine der folgenden Berechtigungen:  
  
-   ALTER TABLE  
-   Der Benutzer ist der Tabellenbesitzer.  
-   Mitgliedschaft in der festen Datenbankrolle **db_ddladmin**.  
  
## <a name="general-remarks"></a>Allgemeine Hinweise  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann tempdb verwenden, um die als Stichprobe entnommenen Zeilen vor dem Erstellen der Statistiken zu sortieren.  
  
### <a name="statistics-for-external-tables"></a>Statistiken für externe Tabellen  
 Beim Erstellen von Statistiken für externe Tabellen importiert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die externe Tabelle in eine temporäre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Tabelle und erstellt anschließend die Statistiken. Bei Statistiken für Stichproben werden nur die als Stichprobe entnommenen Zeilen importiert. Bei einer großen externen Tabelle ist es wesentlich schneller, die Standardstichprobenentnahme statt der FULL SCAN-Option zu verwenden.  
  
### <a name="statistics-with-a-filtered-condition"></a>Statistiken mit einer gefilterten Bedingung  
 Gefilterte Statistiken können die Abfrageleistung für Abfragen verbessern, bei denen aus klar definierten Teilmengen von Daten ausgewählt wird. Gefilterte Statistiken verwenden ein Filterprädikat in der WHERE-Klausel, um die Teilmenge von Daten auszuwählen, die in den Statistiken enthalten ist.  
  
### <a name="when-to-use-create-statistics"></a>Verwendung von CREATE STATISTICS  
 Weitere Informationen dazu, wann UPDATE STATISTICS verwendet werden sollte, finden Sie unter [Statistiken](../../relational-databases/statistics/statistics.md).  
  
### <a name="referencing-dependencies-for-filtered-statistics"></a>Verweisen auf Abhängigkeiten für gefilterte Statistiken  
 Die [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)-Katalogsicht kennzeichnet jede Spalte im gefilterten Statistikprädikat als eine verweisende Abhängigkeit. Überlegen Sie genau, welche Vorgänge Sie für Tabellenspalten ausführen, bevor Sie eine gefilterte Statistik erstellen, da Sie die Definition einer Tabellenspalte, die für ein Prädikat einer gefilterten Statistik definiert wurde, nicht löschen, umbenennen oder ändern können.  
  
## <a name="limitations-and-restrictions"></a>Einschränkungen  
* Das Aktualisieren von Statistiken bei externen Tabellen wird nicht unterstützt. Zum Aktualisieren einer Statistik müssen Sie die Statistik löschen und neu erstellen.  
* Sie können bis zu 64 Spalten pro Statistikobjekt auflisten.
* Die Option MAXDOP ist nicht mit den Optionen STATS_STREAM, ROWCOUNT und PAGECOUNT kompatibel.
* Die Option MAXDOP ist, falls verwendet, durch die Einstellung „MAX_DOP“ der Resource Governor-Arbeitsauslastungsgruppe eingeschränkt.
* CREATE und DROP STATISTICS werden nicht auf externen Tabellen in Azure SQL-Datenbank unterstützt.
  
## <a name="examples"></a>Beispiele  

### <a name="examples-use-the-adventureworks-database"></a>In den Beispielen wird die Datenbank „AdventureWorks“ verwendet.  

### <a name="a-using-create-statistics-with-sample-number-percent"></a>A. Verwenden von CREATE STATISTICS mit SAMPLE number PERCENT  
 Im folgenden Beispiel wird die `ContactMail1`-Statistik erstellt. Dabei wird eine zufällige Stichprobe von 5 Prozent der Spalten `BusinessEntityID` und `EmailPromotion` aus der Tabelle `Person` der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank verwendet.  
  
```sql  
CREATE STATISTICS ContactMail1  
    ON Person.Person (BusinessEntityID, EmailPromotion)  
    WITH SAMPLE 5 PERCENT;  
```  
  
### <a name="b-using-create-statistics-with-fullscan-and-norecompute"></a>B. Verwenden von CREATE STATISTICS mit FULLSCAN und NORECOMPUTE  
 Im folgenden Beispiel werden die `NamePurchase`-Statistiken für alle Zeilen in der `BusinessEntityID`-Spalte und der `EmailPromotion`-Spalte der `Person`-Tabelle erstellt. Dabei wird die automatische Neuberechnung von Statistiken deaktiviert.  
  
```sql  
CREATE STATISTICS NamePurchase  
    ON AdventureWorks2012.Person.Person (BusinessEntityID, EmailPromotion)  
    WITH FULLSCAN, NORECOMPUTE;  
```  
  
### <a name="c-using-create-statistics-to-create-filtered-statistics"></a>C. Erstellen gefilterter Statistiken mithilfe von CREATE STATISTICS  
 Im folgenden Beispiel wird die gefilterte Statistik `ContactPromotion1` erstellt. Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] nimmt 50 Prozent der Daten in die Stichprobe auf und wählt dann die Zeilen aus, in denen `EmailPromotion` gleich 2 ist.  
  
```sql  
CREATE STATISTICS ContactPromotion1  
    ON Person.Person (BusinessEntityID, LastName, EmailPromotion)  
WHERE EmailPromotion = 2  
WITH SAMPLE 50 PERCENT;  
GO  
```  
  
### <a name="d-create-statistics-on-an-external-table"></a>D: Erstellen von Statistiken für eine externe Tabelle  
 Sie müssen beim Erstellen von Statistiken für eine externe Tabelle abgesehen von der Bereitstellung einer Liste der Spalten lediglich entscheiden, ob die Statistiken durch Stichprobenentnahme aus den Zeilen oder durch einen Scan aller Zeilen erstellt werden soll. CREATE und DROP STATISTICS werden nicht auf externen Tabellen in Azure SQL-Datenbank unterstützt.
  
 Da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Daten aus der externen Tabelle in eine temporäre Tabelle importiert, um Statistiken zu erstellen, nimmt die FULL SCAN-Option wesentlich mehr Zeit in Anspruch. Bei einer großen Tabelle ist die Standardmethode für die Stichprobenentnahme in der Regel ausreichend.  
  
```sql  
--Create statistics on an external table and use default sampling.  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress);  
  
--Create statistics on an external table and scan all the rows  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress) WITH FULLSCAN;  
```  

### <a name="e-using-create-statistics-with-fullscan-and-persist_sample_percent"></a>E. Verwenden von CREATE STATISTICS mit FULLSCAN und PERSIST_SAMPLE_PERCENT  
 Im folgenden Beispiel werden die `NamePurchase`-Statistiken für alle Zeilen in den Spalten `BusinessEntityID` und `EmailPromotion` der Tabelle `Person` erstellt. Zudem wird für alle nachfolgenden Updates, die keinen expliziten Prozentsatz für die Stichprobenentnahme angeben, ein Prozentsatz von 100 Prozent für die Stichprobenentnahme festgelegt.  
  
```sql  
CREATE STATISTICS NamePurchase  
    ON AdventureWorks2012.Person.Person (BusinessEntityID, EmailPromotion)  
    WITH FULLSCAN, PERSIST_SAMPLE_PERCENT = ON;  
```  
  
### <a name="examples-using-adventureworksdw-database"></a>Die Beispiele verwenden die Datenbank „AdventureWorksDW“. 
  
### <a name="f-create-statistics-on-two-columns"></a>F. Erstellen von Statistiken für zwei Spalten  
 Im folgenden Beispiel werden die `CustomerStats1`-Statistiken basierend auf den Spalten `CustomerKey` und `EmailAddress` der Tabelle `DimCustomer` erstellt. Die Statistiken werden basierend auf einer statistisch relevanten Stichprobenentnahme der Zeilen in der `Customer`-Tabelle erstellt.  
  
```sql  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress);  
```  
  
### <a name="g-create-statistics-by-using-a-full-scan"></a>G. Erstellen von Statistiken mithilfe eines vollständigen Scans  
 Im folgenden Beispiel wird die Statistik `CustomerStatsFullScan` basierend auf allen Zeilen in der Tabelle `DimCustomer` erstellt.  
  
```sql  
CREATE STATISTICS CustomerStatsFullScan 
ON DimCustomer (CustomerKey, EmailAddress) WITH FULLSCAN;  
```  
  
### <a name="h-create-statistics-by-specifying-the-sample-percentage"></a>H. Erstellen von Statistiken durch Angeben des Stichprobenprozentsatzes  
 Im folgenden Beispiel wird die Statistik `CustomerStatsSampleScan` basierend auf einem Scan von 50 Prozent der Zeilen in der Tabelle `DimCustomer` erstellt.  
  
```sql  
CREATE STATISTICS CustomerStatsSampleScan 
ON DimCustomer (CustomerKey, EmailAddress) WITH SAMPLE 50 PERCENT;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Statistiken](../../relational-databases/statistics/statistics.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [sys.stats_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)  
  
  

