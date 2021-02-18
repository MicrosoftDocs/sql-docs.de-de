---
title: Datenkomprimierung | Microsoft-Dokumentation
description: Anwenden von Zeilen- und Seitendatenkomprimierung oder Columnstore- und Columnstore-Archivierungskomprimierung mithilfe von SQL Server und Azure SQL-Datenbank.
ms.custom: ''
ms.date: 02/11/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- page compression [Database Engine]
- indexes [SQL Server], compressed
- compressed indexes [SQL Server]
- storage compression [Database Engine]
- tables [SQL Server], compressed
- storage [SQL Server], compressed
- compression [SQL Server]
- row compression [Database Engine]
- compression [SQL Server], about compressed tables and indexes
- data compression [Database Engine]
- compressed tables [SQL Server]
ms.assetid: 5f33e686-e115-4687-bd39-a00c48646513
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b8f28c1d2aaa3e1e1e431feaf265d7b1bc22f9d9
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525191"
---
# <a name="data-compression"></a>Datenkomprimierung

[!INCLUDE [sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE [sssdsmifull-md](../../includes/sssdsmifull-md.md)] unterstützen die Zeilen- und Seitenkomprimierung für rowstore-Tabellen und -Indizes sowie Columnstore und die Columnstore-Archivierungskomprimierung für Columnstore-Tabellen und -Indizes.  
  
 Verwenden Sie für rowstore-Tabellen und -Indizes die Datenkomprimierungsfunktion, um die Größe der Datenbank zu reduzieren. Zusätzlich zum Sparen von Speicherplatz kann mithilfe der Datenkomprimierung auch die Leistung von E/A-intensiven Arbeitsauslastungen verbessert werden, da die Daten auf weniger Seiten gespeichert werden und Abfragen weniger Seiten vom Datenträger lesen müssen. Zusätzliche CPU-Ressourcen sind jedoch auf dem Datenbankserver erforderlich, um die Daten zu komprimieren und dekomprimieren, während Daten mit der Anwendung ausgetauscht werden. Sie können die Zeilen- und Seitenkomprimierung für die folgenden Datenbankobjekte konfigurieren:   
-   Vollständige, als Heaps gespeicherte Tabellen  
-   Vollständige, als gruppierte Indizes gespeicherte Tabellen  
-   Vollständige nicht gruppierte Indizes  
-   Vollständige indizierte Sichten  
-   Bei partitionierten Tabellen und Indizes kann die Komprimierungsoption für jede Partition konfiguriert werden, wobei die verschiedenen Partitionen eines Objekts nicht die gleiche Komprimierungseinstellung aufweisen müssen.  
  
Für columnstore-Tabellen und -Indizes wird immer die columnstore-Komprimierung verwendet, die nicht vom Benutzer konfiguriert werden kann. Die columnstore-Archivierungskomprimierung sollte zur weiteren Verringerung der Datengröße verwendet werden, wenn Sie zusätzliche Zeit und CPU-Ressourcen zum Speichern und Abrufen der Daten aufwenden können.  Die columnstore-Archivierungskomprimierung kann für die folgenden Datenbankobjekte konfiguriert werden:  
-   Vollständige columnstore-Tabelle oder vollständiger gruppierter columnstore-Index.  Da eine columnstore-Tabelle als gruppierter columnstore-Index gespeichert wird, haben beide Vorgehensweisen dieselben Ergebnisse.  
-   Vollständiger nicht gruppierter columnstore-Index.  
-   Bei partitionierten columnstore-Tabellen und -Indizes kann die Option für die Archivierungskomprimierung für jede Partition konfiguriert werden, wobei die verschiedenen Partitionen nicht die gleiche Einstellung für die Archivierungskomprimierung aufweisen müssen.  
  
> [!NOTE]  
> Daten können außerdem im Format des GZIP-Algorithmus komprimiert werden. Dies stellt einen zusätzlichen Schritt dar und ist insbesondere geeignet, um Teile der Daten für die Archivierung alter Daten für die Langzeitaufbewahrung zu komprimieren. Mithilfe der Funktion `COMPRESS` komprimierte Daten können nicht indiziert werden. Weitere Informationen finden Sie unter [COMPRESS &#40;Transact-SQL&#41;](../../t-sql/functions/compress-transact-sql.md).  
  
## <a name="row-and-page-compression"></a>ROW- und PAGE-Komprimierung

Beachten Sie die folgenden Punkte, wenn Sie die Zeilen- und Seitenkomprimierung verwenden:  
-   Die Details der Datenkomprimierung können ohne vorherige Ankündigung in Service Packs oder nachfolgenden Versionen geändert werden.
-   Die Komprimierung ist verfügbar in [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)]  
-   Komprimierungsfunktionen sind nicht in jeder Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verfügbar. Weitere Informationen finden Sie unter [Von den SQL Server 2016-Editionen unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
-   Für Systemtabellen ist die Komprimierung nicht verfügbar.  
-   Die Komprimierung kann ermöglichen, dass mehr Zeilen auf einer Seite gespeichert werden, die maximale Zeilengröße einer Tabelle bzw. eines Indexes kann dadurch allerdings nicht geändert werden.  
-   Eine Tabelle kann nicht komprimiert werden, wenn die maximale Zeilengröße einschließlich Verarbeitungsbytes die maximale Größe von 8060 Bytes überschreitet. Beispiel: Eine Tabelle mit den Spalten `c1 CHAR(8000)` und `c2 CHAR(53)` kann aufgrund der zusätzlichen Verarbeitungsbytes nicht komprimiert werden. Bei Verwendung des vardecimal-Speicherformats wird die Zeilengröße überprüft, wenn das Format aktiviert ist. Bei der Zeilen- und Seitenkomprimierung wird die Überprüfung der Zeilengröße durchgeführt, wenn das Objekt zuerst komprimiert wird und die Überprüfung beim Einfügen bzw. Ändern der einzelnen Zeilen erfolgt. Bei der Komprimierung gelten die folgenden zwei Regeln:  
    -   Ein Update für einen Datentyp mit fester Länge muss immer erfolgreich sein.  
    -   Die Deaktivierung der Datenkomprimierung muss immer erfolgreich sein. Selbst wenn die komprimierte Zeile auf die Seite passt, d. h. wenn ihre Größe weniger als 8060 Bytes beträgt, verhindert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Updates, die nicht in die unkomprimierte Zeile passen würden.  
-   Bei Angabe einer Liste mit Partitionen kann der Komprimierungstyp für einzelne Partitionen auf ROW, PAGE oder NONE gesetzt werden. Ohne Angabe einer Liste mit Partitionen wird für alle Partitionen die in der Anweisung angegebene Datenkomprimierungseigenschaft festgelegt. Bei Erstellung einer Tabelle oder eines Indexes wird die Datenkomprimierung auf NONE festgelegt, falls nicht anders angegeben. Bei Änderung einer Tabelle wird die vorhandene Komprimierung beibehalten, falls nicht anders angegeben.  
-   Wenn Sie eine Partitionsliste bzw. eine Partition außerhalb des zulässigen Bereichs angeben, wird ein Fehler generiert.  
-   Nicht gruppierte Indizes erben die Komprimierungseigenschaft der Tabelle nicht. In diesem Fall müssen Sie die Komprimierungseigenschaft explizit festlegen, um die Indizes zu komprimieren. Standardmäßig wird die Komprimierungseinstellung bei Erstellung eines Indexes auf NONE festgelegt.  
-   Wenn ein gruppierter Index auf einem Heap erstellt wird, erbt der gruppierte Index den Komprimierungsstatus des Heaps, sofern kein anderer Komprimierungsstatus angegeben wird.  
-   Wenn ein Heap zur Komprimierung auf Seitenebene konfiguriert wird, erfolgt die Komprimierung auf Seitenebene für die Seiten ausschließlich mit folgenden Methoden:  
    -   Der Massenimport von Daten findet mit aktivierten Massenoptimierungen statt.  
    -   Die Daten werden mit der Syntax `INSERT INTO ... WITH (TABLOCK)` eingefügt und die Tabelle weist keinen nicht gruppierten Index auf.  
    -   Eine Tabelle wird durch Ausführen der Anweisung `ALTER TABLE ... REBUILD` mit der PAGE-Komprimierungsoption erneut erstellt.  
-   Neue Seiten, die in einem Heap als Teil von DML-Vorgängen zugeordnet sind, verwenden die Seitenkomprimierung erst nach der Neuerstellung des Heaps. Erstellen Sie den Heap neu, indem Sie die Komprimierung entfernen und neu anwenden oder indem Sie einen gruppierten Index erstellen und entfernen.  
-   Zur Änderung der Komprimierungseinstellung für einen Heap müssen alle nicht gruppierten Indizes der Tabelle neu erstellt werden, sodass sie auf die neuen Zeilenpositionen im Heap zeigen.  
-   Sie können die ROW- bzw. die PAGE-Komprimierung online oder offline aktivieren und deaktivieren. Die Online-Aktivierung der Komprimierung für einen Heap erfolgt mit einem einzelnen Thread.  
-   Die Speicherplatzanforderungen zur Aktivierung bzw. Deaktivierung der Zeilen- oder Seitenkomprimierung entsprechen den Anforderungen zur Indexerstellung bzw. -neuerstellung. Für partitionierte Daten können Sie den erforderlichen Speicherplatz reduzieren, indem sie die Komprimierung für die Partitionen einzeln aktivieren bzw. deaktivieren.  
-   Um den Komprimierungsstatus der Partitionen in einer partitionierten Tabelle zu ermitteln, fragen Sie die data_compression-Spalte der sys.partitions-Katalogsicht ab.  
-   Bei der Indexkomprimierung können Seiten auf Blattebene sowohl mit der Zeilen- als auch mit der Seitenkomprimierung komprimiert werden. Für Seiten auf Nichtblattebene erfolgt keine Seitenkomprimierung.  
-   Aufgrund ihrer Größe werden Datentypen mit umfangreichen Werten in einigen Fällen getrennt von den regulären Zeilendaten auf speziell dafür vorgesehenen Seiten gespeichert. Die Datenkomprimierung ist für getrennt gespeicherte Daten nicht verfügbar.  
-   Tabellen, die in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] das vardecimal-Speicherformat implementiert haben, behalten diese Einstellung bei Durchführung eines Upgrades bei. Sie können die Zeilenkomprimierung auf Tabellen mit dem vardecimal-Speicherformat anwenden. Da die Zeilenkomprimierung dem vardecimal-Speicherformat übergeordnet ist, besteht jedoch kein Grund, das vardecimal-Speicherformat beizubehalten. Der Komprimierungsgrad für Dezimalwerte wird durch die Kombination von vardecimal-Speicherformat und Zeilenkomprimierung nicht erhöht. Sie können die Seitenkomprimierung auf Tabellen mit dem vardecimal-Speicherformat anwenden, erzielen damit jedoch nicht unbedingt einen höheren Komprimierungsgrad für die Spalten im vardecimal-Speicherformat.  
  
    > [!NOTE]  
    > Alle unterstützten Versionen von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] unterstützen das vardecimal-Speicherformat. Da mit der Datenkomprimierung jedoch dasselbe Ergebnis erzielt wird, wurde das vardecimal-Speicherformat als veraltet markiert. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
## <a name="columnstore-and-columnstore-archive-compression"></a>Columnstore und die Columnstore-Archivierungskomprimierung  

Columnstore-Tabellen und -Indizes werden immer mit columnstore-Komprimierung gespeichert. Sie können die Größe von columnstore-Daten weiter reduzieren, indem Sie eine zusätzliche Komprimierung, die so genannte Archivierungskomprimierung, konfigurieren.  Zur Archivierungskomprimierung führt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] den Microsoft XPRESS-Komprimierungsalgorithmus für die Daten aus. Die Archivierungskomprimierung kann mithilfe der folgenden Datenkomprimierungstypen aktiviert bzw. deaktiviert werden:  
-   Verwenden Sie die **COLUMNSTORE_ARCHIVE** -Datenkomprimierung, um columnstore-Daten mit der Archivierungskomprimierung zu komprimieren.  
-   Verwenden Sie die **COLUMNSTORE** -Datenkomprimierung, um mit der Archivierungskomprimierung komprimierte Daten zu dekomprimieren. Die resultierenden Daten werden weiterhin mit der Columnstore-Komprimierung komprimiert.  
  
Um die Archivierungskomprimierung zu aktivieren, verwenden Sie [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) oder [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) mit der REBUILD-Option sowie DATA COMPRESSION = COLUMNSTORE_ARCHIVE.  
  
Beispiel:

```sql  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =  COLUMNSTORE_ARCHIVE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE_ARCHIVE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE_ARCHIVE ON PARTITIONS (2,4)) ;  
```  
  
Um die Archivierungskomprimierung zu deaktivieren und die Daten wieder mit der columnstodere-Komprimierung zu komprimieren, verwenden Sie [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) oder [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) mit der REBUILD-Option sowie DATA COMPRESSION = COLUMNSTORE.  

Beispiel:

```sql  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =  COLUMNSTORE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE ON PARTITIONS (2,4) ) ;  
```  
  
Im folgenden Beispiel wird die Datenkomprimierung für einige Partitionen auf die columnstore-Komprimierung und für andere Partitionen auf die columnstore-Archivierungskomprimierung festgelegt.  
  
```sql  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (  
    DATA_COMPRESSION =  COLUMNSTORE ON PARTITIONS (4,5),  
    DATA COMPRESSION = COLUMNSTORE_ARCHIVE ON PARTITIONS (1,2,3)  
) ;  
```  
  
### <a name="performance"></a>Leistung

 Mit der Archivierungskomprimierung komprimierte Columnstore-Indizes werden langsamer ausgeführt als Columnstore-Indizes ohne Archivierungskomprimierung. Die Archivierungskomprimierung sollte nur verwendet werden, wenn zusätzliche Zeit und CPU-Ressourcen zum Komprimieren und Abrufen der Daten aufgewendet werden können.  
  
 Der Vorteil der Archivierungskomprimierung ist weniger Speicherplatz, was nützlich für Daten ist, auf die nicht häufig zugegriffen wird. Beispiel: Wenn Sie für die monatlich anfallenden Daten jeweils eine Partition verwenden und sich Ihre Aktivitäten meist auf die letzten Monate beschränken, könnten Sie die Daten weiter zurückliegender Monate archivieren, um den Speicherbedarf zu reduzieren.  
  
### <a name="metadata"></a>Metadaten

Die folgenden Systemsichten enthalten Informationen zur Datenkomprimierung für gruppierte Indizes:  
-   [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) – Die Spalten **type** und **type_desc** enthalten CLUSTERED COLUMNSTORE und NONCLUSTERED COLUMNSTORE.  
-   [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md): Die Spalten **data_compression** und **data_compression_desc** enthalten COLUMNSTORE und COLUMNSTORE_ARCHIVE.  
  
Die Prozedur [sp_estimate_data_compression_savings &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) kann auch für Columnstore-Indizes gelten.  
  
## <a name="impact-on-partitioned-tables-and-indexes"></a>Auswirkungen auf partitionierte Tabellen und Indizes

 Wenn Sie die Datenkomprimierung mit partitionierten Tabellen und Indizes verwenden, beachten Sie Folgendes:  
-   Wenn Sie Partitionen mit der Anweisung `ALTER PARTITION` teilen, erben beide geteilten Partitionen das Datenkomprimierungsattribut der ursprünglichen Partition.  
-   Wenn Sie zwei Partitionen zusammenführen, erbt die zurückgegebene Partition das Datenkomprimierungsattribut der Zielpartition.  
-   Zum Wechseln einer Partition muss die Datenkomprimierungseigenschaft der Partition mit der Komprimierungseigenschaft der Tabelle übereinstimmen.  
-   Zur Änderung der Komprimierung einer partitionierten Tabelle bzw. eines Indexes stehen zwei Syntaxvariationen zur Verfügung.  
    -   Mit der folgenden Syntax wird nur die referenzierte Partition neu erstellt:  
    
        ```sql  
        ALTER TABLE <table_name>   
        REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =  <option>)  
        ```  
    
    -   Mit der folgenden Syntax wird die gesamte Tabelle neu erstellt, wobei für nicht referenzierte Partitionen die vorhandene Komprimierungseinstellung verwendet wird:  
    
        ```sql  
        ALTER TABLE <table_name>   
        REBUILD PARTITION = ALL   
        WITH (DATA_COMPRESSION = PAGE ON PARTITIONS(<range>),  
        ... )  
        ```  
  
     Für partitionierte Indizes gilt bei Verwendung von`ALTER INDEX` dasselbe Prinzip.  
  
-   Beim Löschen eines gruppierten Indexes behalten die entsprechenden Heappartitionen ihre Einstellung für die Datenkomprimierung bei, sofern nicht das Partitionierungsschema geändert wird. Wenn das Partitionierungsschema geändert wird, werden alle Partitionen neu erstellt und erhalten einen unkomprimierten Status. Um einen gruppierten Index zu löschen und das Partitionierungsschema zu ändern, sind folgende Schritte erforderlich:  
     1. Löschen Sie den gruppierten Index.  
     2. Ändern Sie die Tabelle mit der Option `ALTER TABLE ... REBUILD` zur Angabe der Komprimierungsoption.  
  
     OFFLINE können Sie einen gruppierten Index sehr schnell löschen, da lediglich die oberen Ebenen des gruppierten Indexes entfernt werden. Wenn ein gruppierter Index ONLINE gelöscht wird, muss [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] den Heap zwei Mal neu erstellen, ein Mal für Schritt 1 und ein Mal für Schritt 2.  
  
## <a name="how-compression-affects-replication"></a>Auswirkungen der Komprimierung auf die Replikation

Beachten Sie die folgenden Überlegungen, wenn Sie die Datenkomprimierung mit der Replikation verwenden:  
-   Wenn der Momentaufnahme-Agent das Anfangsschemaskript generiert, gilt für die Tabelle und die Indizes des neuen Schemas dieselbe Komprimierungseinstellung. Die Komprimierung kann nicht nur für die Tabelle aktiviert werden.  
-   Bei Transaktionsreplikationen wird durch die Artikelschemaoption festgelegt, für welche abhängigen Objekte und Eigenschaften ein Skript erstellt werden muss. Weitere Informationen finden Sie unter [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).  
     Der Verteilungs-Agent überprüft bei der Anwendung von Skripts Abonnenten mit früheren Versionen nicht. Wenn die Replikation der Komprimierung ausgewählt wird, kann die Tabelle auf Abonnenten mit früheren Versionen nicht erstellt werden. Aktivieren Sie die Replikation der Komprimierung für heterogene Topologien nicht.  
-   Bei Mergereplikationen überschreibt der Veröffentlichungskompatibilitätsgrad die Schemaoptionen und legt fest, für welche Schemaobjekte Skripts erstellt werden.  
     Bei heterogenen Topologien empfiehlt es sich, den Veröffentlichungskompatibilitätsgrad auf die frühere Abonnentenversion festzulegen, falls die neuen Komprimierungsoptionen nicht unterstützt werden müssen. Erforderlichenfalls können Sie Tabellen auf dem Abonnenten komprimieren, nachdem sie erstellt wurden.  
  
Die folgende Tabelle enthält Replikationseinstellungen, mit denen die Komprimierung während der Replikation gesteuert wird.  
  
|Benutzerabsicht|Replizieren des Partitionsschemas für eine Tabelle bzw. einen Index|Replizieren der Komprimierungseinstellungen|Skriptverhalten|  
|-----------------|-----------------------------------------------------|------------------------------------|------------------------|  
|Zur Replikation des Partitionsschemas und zur Aktivierung der Komprimierung auf dem Abonnenten für die Partition.|True|True|Sowohl für das Partitionsschema als auch für die Komprimierungseinstellungen wird ein Skript erstellt.|  
|Zur Replikation des Partitionsschemas ohne Komprimierung der Daten auf dem Abonnenten.|True|Falsch|Für das Partitionsschema wird ein Skript erstellt, für die Komprimierungseinstellungen der Partition jedoch nicht.|  
|Keine Replikation des Partitionsschemas und keine Komprimierung der Daten auf dem Abonnenten.|Falsch|False|Weder für die Partition noch für die Komprimierungseinstellungen wird ein Skript erstellt.|  
|Zur Komprimierung der Tabelle auf dem Abonnenten, wenn alle Partitionen auf dem Verleger komprimiert sind, ohne Replikation des Partitionsschemas.|False|True|Überprüft, ob alle Partitionen für die Komprimierung aktiviert wurden.<br /><br /> Skriptausgabe der Komprimierung auf Tabellenebene.|  
  
## <a name="impact-on-other-sql-server-components"></a>Auswirkung auf andere SQL Server-Komponenten

[!INCLUDE [sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
   
 Die Komprimierung erfolgt im der Speicher-Engine, und die Daten werden in den meisten anderen Komponenten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im unkomprimierten Zustand dargestellt. Hierdurch werden die Auswirkungen der Komprimierung auf die anderen Komponenten auf folgende Punkte beschränkt:  
-   Massenimport- und -exportvorgänge  
     Exportierte Daten werden im unkomprimierten Zeilenformat ausgegeben. Dies gilt auch für systemeigene Datenformate. Aus diesem Grund kann die exportierte Datendatei erheblich größer sein als die Quelldaten.  
     Beim Importieren von Daten werden die Daten in der Speicher-Engine in das komprimierte Zeilenformat konvertiert, falls die Komprimierung in der Zieltabelle aktiviert wurde. Hierbei ist die CPU-Nutzung höher als beim Import von Daten in eine unkomprimierte Tabelle.  
     Beim Massenimport von Daten in einen Heap mit Seitenkomprimierung versucht der Massenimportvorgang die Daten beim Einfügen mit der Seitenkomprimierung zu komprimieren.  
-   Die Komprimierung hat keine Auswirkungen auf Sicherungs- und Wiederherstellungsvorgänge.  
-   Die Komprimierung hat keine Auswirkungen auf den Protokollversand.  
-   Die Datenkomprimierung ist nicht kompatibel mit Sparsespalten. Daher können Tabellen mit Sparsespalten weder komprimiert werden, noch können Sparsespalten einer komprimierten Tabelle hinzugefügt werden.  
-   Die Aktivierung der Komprimierung kann bewirken, dass sich Abfragepläne ändern, da die Daten mit einer anderen Anzahl von Seiten und Zeilen pro Seite gespeichert werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Implementierung von Zeilenkomprimierung](../../relational-databases/data-compression/row-compression-implementation.md)   
 [Implementierung von Seitenkomprimierung](../../relational-databases/data-compression/page-compression-implementation.md)   
 [Implementierung von Unicode-Komprimierung](../../relational-databases/data-compression/unicode-compression-implementation.md)   
 [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)  
  
  

