---
description: Erstellen und Verwalten von Volltextindizes
title: Erstellen und Verwalten von Volltextindizes | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/31/2020
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text indexes [SQL Server], about
ms.assetid: f8a98486-5438-44a8-b454-9e6ecbc74f83
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8a3c3792fa2aca009e02fc8fcdce6bf613b5e370
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341679"
---
# <a name="create-and-manage-full-text-indexes"></a>Erstellen und Verwalten von Volltextindizes
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
Dieses Thema beschreibt das Erstellen, Auffüllen und Verwalten von Volltextindizes in SQL Server.
  
## <a name="prerequisite---create-a-full-text-catalog"></a>Voraussetzung: Erstellen eines Volltextkatalogs
Bevor Sie einen Volltextindex erstellen können, müssen Sie einen Volltextkatalog erstellen. Der Katalog ist ein virtueller Container für ein oder mehrere Volltextindizes. Weitere Informationen finden Sie unter [Erstellen und Verwalten von Volltextkatalogen](../../relational-databases/search/create-and-manage-full-text-catalogs.md).
  
##  <a name="create-alter-or-drop-a-full-text-index"></a><a name="tasks"></a> Erstellen, Ändern oder Löschen eines Volltextindexes  
### <a name="create-a-full-text-index"></a>Erstellen eines Volltextindexes  
  
-   [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)  
  
### <a name="alter-a-full-text-index"></a>Ändern eines Volltextindexes
  
-   [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
### <a name="drop-a-full-text-index"></a>Löschen eines Volltextindexes 
  
-   [DROP FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-index-transact-sql.md)

## <a name="populate-a-full-text-index"></a>Auffüllen eines Volltextindexes
Der Vorgang, bei dem ein Volltextindex erstellt und verwaltet wird, wird als *Auffüllung* (oder *Durchforstung*) bezeichnet. Es gibt drei Typen der Auffüllung eines Volltextindexes:
-   Vollständige Auffüllung
-   Auffüllung basierend auf der Änderungsnachverfolgung
-   Inkrementelle Auffüllung basierend auf einem Zeitstempel

Weitere Informationen finden Sie unter [Auffüllen von Volltextindizes](../../relational-databases/search/populate-full-text-indexes.md).

##  <a name="view-the-properties-of-a-full-text-index"></a><a name="view"></a> Anzeigen der Eigenschaften eines Volltextindexes
### <a name="view-the-properties-of-a-full-text-index-with-transact-sql"></a>Anzeigen der Eigenschaften eines Volltextindexes mit Transact-SQL

|Katalogsicht oder dynamische Verwaltungssicht|BESCHREIBUNG|  
|----------------------------------------|-----------------|  
|[sys.fulltext_index_catalog_usages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-catalog-usages-transact-sql.md)|Gibt eine Zeile für jeden Verweis zwischen Volltextkatalog und Volltextindex zurück.|  
|[sys.fulltext_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)|Enthält eine Zeile für jede Spalte, die Teil eines Volltextindexes ist.|  
|[sys.fulltext_index_fragments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md)|Ein Volltextindex verwendet interne Tabellen, die als Volltextindexfragmente bezeichnet werden, um die umgekehrten Indexdaten zu speichern. Diese Sicht kann verwendet werden, um die Metadaten zu diesen Fragmenten abzufragen. Diese Sicht enthält eine Zeile für jedes Volltextindexfragment in jeder Tabelle, die einen Volltextindex enthält.|  
|[sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)|Enthält eine Zeile pro Volltextindex eines Tabellenobjekts.|  
|[sys.dm_fts_index_keywords &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)|Gibt Informationen zum Inhalt eines Volltextindex für die angegebene Tabelle zurück.|  
|[sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)|Gibt Informationen zum Inhalt auf Dokumentebene eines Volltextindex für die angegebene Tabelle zurück. Ein Schlüsselwort kann in mehreren Dokumenten angezeigt werden.|  
|[sys.dm_fts_index_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)|Gibt Informationen zu den aktuell ausgeführten Volltextindexauffüllungen zurück.|  
 
### <a name="view-the-properties-of-a-full-text-index-with-management-studio"></a>Anzeigen der Eigenschaften eines Volltextindexes mit Management Studio 
1.  Erweitern Sie in Management Studio im Objekt-Explorer den Server.  
  
2.  Erweitern Sie **Datenbanken**, und erweitern Sie dann die Datenbank, die den Volltextindex enthält.  
  
3.  Erweitern Sie **Tabellen**.  
  
4.  Klicken Sie mit der rechten Maustaste auf die Tabelle, für die der Volltextindex definiert ist. Wählen Sie **Volltextindex**, und klicken Sie dann im Kontextmenü **Volltextindex** auf **Eigenschaften**. Das Dialogfeld **Volltextindexeigenschaften** wird geöffnet.  
  
5.  Im Bereich **Seite auswählen** können Sie eine der folgenden Seiten auswählen:  
  
    |Seite|BESCHREIBUNG|  
    |----------|-----------------|  
    |**Allgemein**|Ändert die grundlegenden Eigenschaften des Volltextindex. Beinhaltet mehrere änderbare Eigenschaften und eine Reihe von nicht änderbaren Eigenschaften, wie z. B. Datenbankname, Tabellenname und den Namen der Volltextschlüsselspalte. Die änderbaren Eigenschaften lauten:<br /><br /> **Volltextindex-Stoppliste**<br /><br /> **Volltextindizierung aktiviert**<br /><br /> **Änderungsnachverfolgung**<br /><br /> **Sucheigenschaftenliste**|  
    |**Spalten**|Zeigt die Tabellenspalten an, die für die Volltextindizierung verfügbar sind. Die ausgewählte Spalte bzw. die Spalten werden volltextindiziert. Sie können beliebig viele verfügbare Spalten auswählen und in den Volltextindex aufnehmen. Weitere Informationen finden Sie unter [Auffüllen von Volltextindizes](populate-full-text-indexes.md).|
    |**Zeitpläne**|Verwenden Sie diese Seite, um Zeitpläne für einen SQL Server-Agent-Auftrag zu erstellen oder zu verwalten, der eine inkrementelle Tabellenauffüllung für die Auffüllungen des Volltextindexes beginnt. Weitere Informationen finden Sie unter [Auffüllen von Volltextindizes](../../relational-databases/search/populate-full-text-indexes.md).<br /><br /> Hinweis: Sobald Sie das Dialogfeld **Volltextindexeigenschaften** schließen, werden alle neu erstellten Zeitpläne einem SQL Server-Agent-Auftrag zugeordnet (Start Incremental Table Population on *Datenbankname*.*Tabellenname*).|  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)] um vorgenommene Änderungen zu speichern und das Dialogfeld **Volltextindexeigenschaften** zu schließen.  
  
##  <a name="view-the-properties-of-indexed-tables-and-columns"></a><a name="props"></a> Anzeigen der Eigenschaften von indizierten Tabellen und Spalten  
 Mehrere [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen, z. B. OBJECTPROPERTYEX, können verwendet werden, um den Wert verschiedener Eigenschaften der Volltextindizierung abzurufen. Diese Informationen sind für die Verwaltung und Problembehandlung der Volltextsuche hilfreich.  
  
 Die folgende Tabelle enthält die Volltexteigenschaften, die sich auf indizierte Tabellen und Spalten beziehen, sowie die zugehörigen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen.  
  
|Eigenschaft|BESCHREIBUNG|Funktion|  
|--------------|-----------------|--------------|  
|**FullTextTypeColumn**|TYPE COLUMN in der Tabelle, die die Dokumenttypinformationen der Spalte enthält.|[COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md)|  
|**IsFulltextIndexed**|Gibt an, ob eine Spalte für die Volltextindizierung aktiviert wurde.|COLUMNPROPERTY|  
|**IsFulltextKey**|Gibt an, ob der Index der Volltextschlüssel für eine Tabelle ist.|[INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md)|  
|**TableFulltextBackgroundUpdateIndexOn**|Gibt an, ob für eine Tabelle das Update von Volltextindizes im Hintergrund aktiviert wurde.|[OBJECTPROPERTYEX](../../t-sql/functions/objectpropertyex-transact-sql.md)|  
|**TableFulltextCatalogId**|ID des Volltextkatalogs, in dem die Daten des Volltextindex für die Tabelle gespeichert sind.|OBJECTPROPERTYEX|  
|**TableFulltextChangeTrackingOn**|Gibt an, ob für eine Tabelle die Volltext-Änderungsnachverfolgung aktiviert ist.|OBJECTPROPERTYEX|  
|**TableFulltextDocsProcessed**|Die Anzahl der seit dem Start der Volltextindizierung verarbeiteten Zeilen.|OBJECTPROPERTYEX|  
|**TableFulltextFailCount**|Die Anzahl von Zeilen, für die die Volltextsuche keinen Index erstellt hat.|OBJECTPROPERTYEX|  
|**TableFulltextItemCount**|Die Anzahl von Zeilen, für die ein Volltextindex erfolgreich erstellt wurde.|OBJECTPROPERTYEX|  
|**TableFulltextKeyColumn**|Die Spalten-ID der Volltextspalte für den eindeutigen Schlüssel.|OBJECTPROPERTYEX|  
|**TableFullTextMergeStatus**|Gibt an, ob eine Tabelle über einen Volltextindex verfügt, der gerade zusammengeführt wird.|OBJECTPROPERTYEX|  
|**TableFulltextPendingChanges**|Anzahl der zu verarbeitenden ausstehenden Änderungsnachverfolgungseinträge.|OBJECTPROPERTYEX|  
|**TableFulltextPopulateStatus**|Der Auffüllungsstatus einer Volltexttabelle.|OBJECTPROPERTYEX|  
|**TableHasActiveFulltextIndex**|Gibt an, ob eine Tabelle über einen aktiven Volltextindex verfügt.|OBJECTPROPERTYEX|  
  
##  <a name="get-info-about-the-full-text-key-column"></a><a name="key"></a> Abrufen von Informationen zur Volltextschlüsselspalte  
 Normalerweise müssen die Ergebnisse von CONTAINSTABLE- oder FREETEXTTABLE-Rowsetwertfunktionen mit der Basistabelle verknüpft werden. In solchen Fällen müssen Sie den Namen der eindeutigen Schlüsselspalte kennen. Sie können abfragen, ob ein bestimmter eindeutiger Index als Volltextschlüssel verwendet wird, und anschließend den Bezeichner der Volltextschlüsselspalte abrufen.  
  
### <a name="determine-whether-a-given-unique-index-is-used-as-the-full-text-key-column"></a>Überprüfen, ob ein bestimmter eindeutiger Index als Volltextschlüsselspalte verwendet wird  
  
Verwenden Sie eine [SELECT](../../t-sql/queries/select-transact-sql.md)-Anweisung, um die [INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md)-Funktion aufzurufen. Geben Sie im Funktionsaufruf die OBJECT_ID-Funktion an, um den Namen der Tabelle (*table_name*) in die Tabellen-ID umzuwandeln. Geben Sie den Namen eines eindeutigen Indexes der Tabelle und die **IsFulltextKey** -Indexeigenschaft an, wie im folgenden Beispiel gezeigt:  
  
```sql  
SELECT INDEXPROPERTY( OBJECT_ID('table_name'), 'index_name',  'IsFulltextKey' );  
```  
  
 Diese Anweisung gibt den Wert 1 zurück, wenn der Index verwendet wird, um die Eindeutigkeit für die Spalte des Volltextschlüssels zu erzwingen, oder den Wert 0, wenn dies nicht der Fall ist.  
  
 **Beispiel**  
  
 Im folgenden Beispiel wird abgefragt, ob der `PK_Document_DocumentNode` -Index zum Erzwingen der Eindeutigkeit der Volltextschlüsselspalte verwendet wird:  
  
```sql  
USE AdventureWorks  
GO  
SELECT INDEXPROPERTY ( OBJECT_ID('Production.Document'), 'PK_Document_DocumentNode',  'IsFulltextKey' )  
```  
  
 Dieses Beispiel gibt den Wert 1 zurück, wenn der `PK_Document_DocumentNode` -Index verwendet wird, um die Eindeutigkeit der Volltextschlüsselspalte zu erzwingen. Andernfalls wird 0 oder NULL zurückgegeben. NULL impliziert, dass ein ungültiger Indexname verwendet wird, der Indexname der Tabelle nicht zugeordnet werden kann, die Tabelle nicht vorhanden ist oder eine andere Fehlerbedingung vorliegt.  
  
### <a name="find-the-identifier-of-the-full-text-key-column"></a>Suchen des Bezeichners der Volltextschlüsselspalte  
  
Jede volltextfähige Tabelle beinhaltet eine Spalte, mit der die Eindeutigkeit aller Tabellenzeilen erzwungen wird (die *UNIQUE**KEY-Spalte*). Die **TableFulltextKeyColumn** -Eigenschaft, die mit der OBJECTPROPERTYEX-Funktion ermittelt werden kann, enthält die Spalten-ID der eindeutigen Schlüsselspalte.  
 
Um diesen Bezeichner abzurufen, können Sie mit einer SELECT-Anweisung die OBJECTPROPERTYEX-Funktion aufrufen. Verwenden Sie die OBJECT_ID-Funktion, um den Namen der Tabelle (*table_name*) in die Tabellen-ID umzuwandeln, und geben Sie die **TableFulltextKeyColumn** -Eigenschaft wie folgt an:  
  
```sql  
SELECT OBJECTPROPERTYEX(OBJECT_ID( 'table_name'), 'TableFulltextKeyColumn' ) AS 'Column Identifier';  
```  
  
 **Beispiele**  
  
 Im folgenden Beispiel wird der Bezeichner der Volltextschlüsselspalte oder NULL zurückgegeben. NULL impliziert, dass ein ungültiger Indexname verwendet wird, der Indexname der Tabelle nicht zugeordnet werden kann, die Tabelle nicht vorhanden ist oder eine andere Fehlerbedingung vorliegt.  
  
```sql
USE AdventureWorks;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID('Production.Document'), 'TableFulltextKeyColumn');  
GO  
```  
  
 Das folgende Beispiel zeigt, wie der Bezeichner der eindeutigen Schlüsselspalte verwendet werden kann, um den Namen der Spalte zu ermitteln.  
  
```sql  
USE AdventureWorks;  
GO  
DECLARE @key_column sysname  
SET @key_column = Col_Name(Object_Id('Production.Document'),  
ObjectProperty(Object_id('Production.Document'),  
'TableFulltextKeyColumn')   
)  
SELECT @key_column AS 'Unique Key Column';  
GO  
```  
  
 Dieses Beispiel gibt eine Resultsetspalte mit dem Namen `Unique Key Column`zurück, die eine einzelne Zeile mit dem Namen der eindeutigen Schlüsselspalte der Document-Tabelle enthält: DocumentNode. Beachten Sie, dass diese Abfrage NULL zurückgibt, wenn ein ungültiger Indexname verwendet wird, der Indexname der Tabelle nicht zugeordnet werden kann, die Tabelle nicht vorhanden ist oder eine andere Fehlerbedingung vorliegt.  

## <a name="index-varbinarymax-and-xml-columns"></a>Indizieren von varbinary(max)- und xml-Spalten  
 Wenn ein Volltextindex einer **varbinary(max)** -, **varbinary**- oder **xml** -Spalte erstellt wird, kann die Spalte mit den Volltextprädikaten (CONTAINS und FREETEXT) und -funktionen (CONTAINSTABLE und FREETEXTTABLE) wie jede andere volltextindizierte Spalte durchsucht werden.
   
### <a name="index-varbinarymax-or-varbinary-data"></a>Indizieren von varbinary(max)- oder varbinary-Daten  
 In einer einzelnen **varbinary(max)** - oder **varbinary** -Spalte können viele Dokumenttypen gespeichert werden. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt jeden Dokumenttyp, für den ein Filter im Betriebssystem installiert und verfügbar ist. Der Dokumenttyp jedes Dokuments wird durch die Dateierweiterung des Dokuments identifiziert. Zum Beispiel verwendet die Volltextsuche für die Dateierweiterung .doc den Filter für Microsoft Word-Dokumente. Eine Liste der verfügbaren Dokumenttypen erhalten Sie, indem Sie die [sys.fulltext_document_types](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md) -Katalogsicht abfragen.  
  
Beachten Sie, dass die Volltext-Engine vorhandene Filter nutzen kann, die im Betriebssystem installiert sind. Bevor die Filter, Wörtertrennungen und Wortstammerkennungen des Betriebssystems verwendet werden können, müssen Sie diese in der Serverinstanz laden. Dies wird im Folgenden beschrieben:  
  
```sql  
EXEC sp_fulltext_service @action='load_os_resources', @value=1  
```  
  
Zum Erstellen eines Volltextindexes für eine **varbinary(max)** -Spalte benötigt die Volltext-Engine Zugriff auf die Dateierweiterungen der Dokumente in der **varbinary(max)** -Spalte. Diese Informationen müssen in einer Tabellenspalte, der so genannten Typspalte, gespeichert werden. Die Spalte muss der **varbinary(max)** -Spalte im Volltextindex zugeordnet sein. Beim Indizieren eines Dokuments verwendet die Volltext-Engine die Dateierweiterung in der Typspalte, um den richtigen Filter zu ermitteln.  
   
### <a name="index-xml-data"></a>Indizieren von XML-Daten  
 In einer **xml** -Datentypspalte werden ausschließlich XML-Dokumente und -Fragmente gespeichert. Für die Dokumente wird immer der XML-Filter verwendet. Ein Typspalte ist daher nicht erforderlich. Bei **xml** -Spalten indiziert der Volltextindex den Inhalt der XML-Elemente und ignoriert die XML-Markups. Attributwerte werden volltextindiziert, sofern es sich nicht um numerische Werte handelt. Elementtags werden als Tokenbegrenzungen verwendet. Wohlgeformte XML- oder HTML-Dokumente und -Fragmente in mehreren Sprachen werden unterstützt.  
  
 Weitere Informationen zum Indizieren und Abfragen einer **xml**-Spalte finden Sie unter [Verwenden der Volltextsuche mit XML-Spalten](../../relational-databases/xml/use-full-text-search-with-xml-columns.md).  
  
##  <a name="disable-or-re-enable-full-text-indexing-for-a-table"></a><a name="disable"></a> Deaktivieren oder Reaktivieren der Volltextindizierung für eine Tabelle   
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sind standardmäßig alle von Benutzern erstellten Datenbanken volltextfähig. Zudem wird eine einzelne Tabelle automatisch für die Volltextindizierung aktiviert, sobald ein Volltextindex für die Tabelle erstellt wird und dem Index eine Spalte hinzugefügt wird. Eine Tabelle wird für die Volltextindizierung automatisch deaktiviert, wenn die letzte Spalte aus dem Volltextindex der Tabelle entfernt wird.  
  
 Für eine Tabelle mit einem Volltextindex können Sie mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Tabelle für die Volltextindizierung manuell deaktivieren und erneut aktivieren.  

1.  Erweitern Sie die Servergruppe, erweitern Sie **Datenbanken**, und erweitern Sie die Datenbank, die die Tabelle enthält, die für Volltextindizierung aktiviert werden soll.  
  
2.  Erweitern Sie **Tabellen**, und klicken Sie mit der rechten Maustaste auf die Tabelle, die Sie deaktivieren oder für Volltextindizierung erneut aktivieren möchten.  
  
3.  Wählen Sie **Volltextindex** aus, und klicken Sie anschließend auf **Volltextindizierung deaktivieren** oder **Volltextindizierung aktivieren**.  
  
##  <a name="remove-a-full-text-index-from-a-table"></a><a name="remove"></a> Entfernen eines Volltextindexes aus einer Tabelle  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Tabelle mit dem Volltextindex, den Sie löschen möchten  
  
2.  Wählen Sie **Volltextindex löschen** aus.  
  
3.  Klicken Sie auf **OK** , wenn Sie aufgefordert werden, das Löschen des Volltextindexes zu bestätigen.  
  
  
