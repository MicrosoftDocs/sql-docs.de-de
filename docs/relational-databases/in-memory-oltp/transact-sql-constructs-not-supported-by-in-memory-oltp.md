---
title: Von In-Memory OLTP nicht unterstützte T-SQL
description: Erfahren Sie, welche Transact-SQL-Features für speicheroptimierte Tabellen, nativ kompilierte gespeicherte Prozeduren und benutzerdefinierte Funktionen nicht unterstützt werden.
ms.custom: seo-dt-2019
ms.date: 11/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: e3f8009c-319d-4d7b-8993-828e55ccde11
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a41ac84e7e34e0df190c725d9dbadf84f15cac47
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100353631"
---
# <a name="transact-sql-constructs-not-supported-by-in-memory-oltp"></a>Von In-Memory OLTP nicht unterstützte Transact-SQL-Konstrukte
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Speicheroptimierte Tabellen, systemintern kompilierte gespeicherte Prozeduren und benutzerdefinierte Funktionen unterstützen nicht die vollständige [!INCLUDE[tsql](../../includes/tsql-md.md)]-Oberfläche, die von datenträgerbasierten Tabellen und interpretierten gespeicherten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Prozeduren und benutzerdefinierten Funktionen unterstützt wird. Wird versucht, eine der nicht unterstützten Funktionen zu verwenden, gibt der Server einen Fehler zurück.  
  
 Im Text der Fehlermeldung werden der Typ der [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung (z. B. Funktion, Vorgang, Option) sowie der Name der Funktion oder des [!INCLUDE[tsql](../../includes/tsql-md.md)] -Schlüsselworts angegeben. Die meisten nicht unterstützten Funktionen geben den Fehler 10794 zurück, wobei der Fehlermeldungstext die nicht unterstützte Funktion angibt. In den folgenden Tabellen werden die möglichen [!INCLUDE[tsql](../../includes/tsql-md.md)] -Funktionen und -Schlüsselwörter im Meldungstext des Fehlers sowie die Korrekturmaßnahmen zum Beheben des Fehlers aufgelistet.  
  
 Weitere Informationen zu unterstützten Funktionen für speicheroptimierte Tabellen und systemintern kompilierte gespeicherte Prozeduren finden Sie unter:  
  
-   [Migrationsprobleme bei nativ kompilierten gespeicherten Prozeduren](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
-   [Transact-SQL-Unterstützung für OLTP im Arbeitsspeicher](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)  
  
-   [Nicht unterstützte SQL Server-Funktionen für In-Memory OLTP](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md)  
  
-   [Nativ kompilierte gespeicherte Prozeduren](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
## <a name="databases-that-use-in-memory-oltp"></a>Datenbanken, die In-Memory OLTP verwenden  
 In der folgenden Tabelle werden die nicht unterstützten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Funktionen und -Schlüsselwörter aufgelistet, die im Meldungstext eines In-Memory OLTP-Datenbankfehlers angezeigt werden können. In der Tabelle werden auch Hinweise zur Fehlerbehebung aufgeführt.  
  
|type|Name|Lösung|  
|----------|----------|----------------|  
|Option|AUTO_CLOSE|Die Datenbankoption AUTO_CLOSE=ON wird für Datenbanken, die eine MEMORY_OPTIMIZED_DATA-Dateigruppe enthalten, nicht unterstützt.|  
|Option|ATTACH_REBUILD_LOG|Die CREATE-Datenbankoption ATTACH_REBUILD_LOG wird für Datenbanken, die eine MEMORY_OPTIMIZED_DATA-Dateigruppe enthalten, nicht unterstützt.|  
|Funktion|DATABASE SNAPSHOT|Das Erstellen von Datenbankmomentaufnahmen wird für Datenbanken, die eine MEMORY_OPTIMIZED_DATA-Dateigruppe enthalten, nicht unterstützt.|  
|Funktion|Replikation mithilfe der sync_method "database snapshot" oder "database snapshot character"|Replikation mithilfe der sync_method "database snapshot" oder "database snapshot character" wird für Datenbanken, die eine MEMORY_OPTIMIZED_DATA-Dateigruppe enthalten, nicht unterstützt.|  
|Funktion|DBCC CHECKDB<br /><br /> DBCC CHECKTABLE|DBCC CHECKDB lässt die speicheroptimierten Tabellen in der Datenbank aus.<br /><br /> DBCC CHECKTABLE schlägt für speicheroptimierte Tabellen fehl.|  
  
## <a name="memory-optimized-tables"></a>Speicheroptimierte Tabellen  
 In der folgenden Tabelle werden die nicht unterstützten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Funktionen und -Schlüsselwörter aufgelistet, die im Meldungstext eines Fehlers einer speicheroptimierten Tabelle angezeigt werden können. In der Tabelle werden auch Hinweise zur Fehlerbehebung aufgeführt.  
  
|type|Name|Lösung|  
|----------|----------|----------------|  
|Funktion|EIN|Speicheroptimierte Tabellen können nicht in einer Dateigruppe oder einem Partitionsschema platziert werden. Entfernen Sie die ON-Klausel aus der **CREATE TABLE** -Anweisung.<br /><br /> Alle speicheroptimierten Tabellen sind speicheroptimierten Dateigruppen zugeordnet.|  
|Datentyp|*Datentypname*|Der angegebene Datentyp wird nicht unterstützt. Ersetzen Sie den Typ durch einen der unterstützten Datentypen. Weitere Informationen finden Sie unter [Unterstützte Datentypen für In-Memory OLTP](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md).|  
|Funktion|Berechnete Spalten|**Gilt für:** [!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] und [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Berechnete Spalten werden für speicheroptimierte Tabellen nicht unterstützt. Entfernen Sie die berechneten Spalten aus der **CREATE TABLE** -Anweisung.<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] und SQL Server ab Version [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] unterstützen berechnete Spalten in speicheroptimierten Tabellen und Indizes.|  
|Funktion|Replikation|Replikation wird für speicheroptimierte Tabellen nicht unterstützt.|  
|Funktion|FILESTREAM|Die FILESTREAM-Speicherung wird für Spalten von speicheroptimierten Tabellen nicht unterstützt. Entfernen Sie das **FILESTREAM** -Schlüsselwort aus der Spaltendefinition.|  
|Funktion|SPARSE|Spalten aus speicheroptimierten Tabellen können nicht als SPARSE definiert werden. Entfernen Sie das **SPARSE** -Schlüsselwort aus der Spaltendefinition.|  
|Funktion|ROWGUIDCOL|Die ROWGUIDCOL-Option wird für Spalten von speicheroptimierten Tabellen nicht unterstützt. Entfernen Sie das **ROWGUIDCOL** -Schlüsselwort aus der Spaltendefinition.|  
|Funktion|FOREIGN KEY|**Gilt für:** [!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] und SQL Server ab Version [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Für speicheroptimierte Tabellen werden FOREIGN KEY-Einschränkungen nur für die Fremdschlüssel unterstützt, die auf Primärschlüssel von anderen speicheroptimierten Tabellen verweisen. Entfernen Sie die Einschränkung aus der Tabellendefinition, wenn der Fremdschlüssel auf eine eindeutige Einschränkung verweist.<br/><br/>In [!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] werden FOREIGN KEY-Einschränkungen nicht für speicheroptimierte Tabellen unterstützt.|  
|Funktion|Gruppierter Index|Geben Sie einen nicht gruppierten Index an. Bei einem Primärschlüsselindex müssen Sie **PRIMARY KEY NONCLUSTERED** angeben.|  
|Funktion|DDL innerhalb von Transaktionen|Speicheroptimierte Tabellen und systemintern kompilierte gespeicherte Prozeduren können nicht im Rahmen einer Benutzertransaktion erstellt oder gelöscht werden. Starten Sie keine Transaktion, und vergewissern Sie sich, dass die Sitzungseinstellung IMPLICIT_TRANSACTIONS auf OFF festgelegt ist, bevor Sie die CREATE-Anweisung oder die DROP-Anweisung ausführen.|  
|Funktion|DDL-Trigger|Speicheroptimierte Tabellen und systemintern kompilierte gespeicherte Prozeduren können nicht erstellt oder gelöscht werden, wenn ein Server- oder Datenbanktrigger für den betreffenden DDL-Vorgang vorhanden ist. Entfernen Sie die Server- und Datenbanktrigger für CREATE/DROP TABLE und CREATE/DROP PROCEDURE.|  
|Funktion|EVENT NOTIFICATION|Speicheroptimierte Tabellen und systemintern kompilierte gespeicherte Prozeduren können nicht erstellt oder gelöscht werden, wenn eine Server- oder Datenbank-Ereignisbenachrichtigung für den betreffenden DDL-Vorgang vorhanden ist. Entfernen Sie die Server- und Datenbankereignisbenachrichtigungen in CREATE TABLE bzw. DROP TABLE und CREATE PROCEDURE bzw. DROP PROCEDURE.|  
|Funktion|FileTable|Speicheroptimierte Tabellen können nicht als File-Tabellen erstellt werden. Entfernen Sie das Argument **AS FileTable** aus der **CREATE TABLE** -Anweisung.|  
|Vorgang|Update der Primärschlüsselspalten|Primärschlüsselspalten in speicheroptimierten Tabellen und Tabellentypen können nicht aktualisiert werden. Wenn der Primärschlüssel aktualisiert werden muss, löschen Sie die alte Zeile, und fügen Sie die neue Zeile mit dem aktualisierten Primärschlüssel ein.|  
|Vorgang|CREATE INDEX|Indizes zu speicheroptimierten Tabellen müssen inline mit der **CREATE TABLE** - oder der **ALTER TABLE** -Anweisung angegeben werden.|  
|Vorgang|CREATE FULLTEXT INDEX|Volltextindizes werden für speicheroptimierte Tabellen nicht unterstützt.|  
|Vorgang|Schemaänderung|Speicheroptimierte Tabellen und nativ kompilierte gespeicherte Prozeduren unterstützen folgende Schemaänderungen nicht:<br/> [!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] und SQL Server ab [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]: ALTER TABLE-, ALTER PROCEDURE- und sp_rename-Vorgänge werden unterstützt. Andere Schemaänderungen, z.B. das Hinzufügen von erweiterten Eigenschaften, werden nicht unterstützt.<br/><br/>[!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]: ALTER TABLE- und ALTER PROCEDURE-Vorgänge werden unterstützt. Andere Schemaänderungen, einschließlich sp_rename, werden nicht unterstützt.<br/><br/>[!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)]: Schemaänderungen werden nicht unterstützt. Löschen Sie das Objekt, und erstellen Sie es mit der gewünschten Definition neu, um die Definition einer speicheroptimierten Tabelle oder einer nativ kompilierten gespeicherten Prozedur zu ändern.| 
|Vorgang|TRUNCATE TABLE|Der TRUNCATE-Vorgang wird für speicheroptimierte Tabellen nicht unterstützt. Löschen Sie alle Zeilen mit **DELETE FROM**_Tabelle_ , oder löschen Sie die Tabelle, und erstellen Sie sie neu, um alle Zeilen aus einer Tabelle zu entfernen.|  
|Vorgang|ALTER AUTHORIZATION|Das Ändern des Besitzers einer vorhandenen speicheroptimierten Tabelle oder systemintern kompilierten gespeicherten Prozedur wird nicht unterstützt. Löschen Sie die Tabelle oder die Prozedur, und erstellen Sie sie neu, um den Besitzer zu ändern.|  
|Vorgang|ALTER SCHEMA|Das Übertragen einer vorhandenen Tabelle oder einer nativ kompilierten gespeicherten Prozedur in ein anderes Schema wird nicht unterstützt. Löschen Sie das Objekt, und erstellen Sie es neu, um eine Übertragung zwischen Schemas zu ermöglichen.|  
|Vorgang|DBCC CHECKTABLE|DBCC CHECKTABLE wird für speicheroptimierte Tabellen nicht unterstützt. Führen Sie eine Sicherung der MEMORY_OPTIMIZED_DATA-Dateigruppe durch, um die Integrität der Prüfpunktdateien auf dem Datenträger zu überprüfen.|  
|Funktion|ANSI_PADDING OFF|Die **ANSI_PADDING** -Sitzungsoption muss beim Erstellen von speicheroptimierten Tabellen oder systemintern kompilierten gespeicherten Prozeduren auf ON festgelegt sein. Führen Sie **SET ANSI_PADDING ON** aus, bevor Sie die CREATE-Anweisung ausführen.|  
|Option|DATA_COMPRESSION|Die Datenkomprimierung wird für speicheroptimierte Tabellen nicht unterstützt. Entfernen Sie die Option aus der Tabellendefinition.|  
|Funktion|DTC|Auf speicheroptimierte Tabellen und systemintern kompilierte gespeicherte Prozeduren kann nicht aus verteilten Transaktionen zugegriffen werden. Verwenden Sie stattdessen SQL-Transaktionen.|  
|Vorgang|Speicheroptimierte Tabellen als Ziel von MERGE|Speicheroptimierte Tabellen können nicht das Ziel eines **MERGE** -Vorgangs sein. Verwenden Sie stattdessen **INSERT**-, **UPDATE**- oder **DELETE**-Anweisungen.|  
  
## <a name="indexes-on-memory-optimized-tables"></a>Indizes für speicheroptimierte Tabellen  
 In der folgenden Tabelle werden die möglichen [!INCLUDE[tsql](../../includes/tsql-md.md)] -Funktionen und -Schlüsselwörter im Meldungstext eines Fehlers mit dem Index einer speicheroptimierten Tabelle sowie die Korrekturmaßnahmen zum Beheben des Fehlers aufgelistet.  
  
|type|Name|Lösung|  
|----------|----------|----------------|  
|Funktion|Gefilterter Index|Gefilterte Indizes werden nicht mit speicheroptimierten Tabellen unterstützt. Lassen Sie die **WHERE** -Klausel in der Indexspezifikation aus.|  
|Funktion|Eingeschlossene Spalten|Bei speicheroptimierten Tabellen müssen keine eingeschlossenen Spalten angegeben werden. Alle Spalten der speicheroptimierten Tabelle werden implizit in jeden speicheroptimierten Index eingeschlossen.|  
|Vorgang|DROP INDEX|Das Löschen von Indizes für speicheroptimierte Tabellen wird nicht unterstützt. Sie können Indizes mithilfe von ALTER TABLE löschen.<br /><br /> Weitere Informationen zu speicheroptimierten Tabellen finden Sie unter [Ändern von speicheroptimierten Tabellen](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md).|  
|Indexoption|*Indexoption*|Es wird nur eine Index-Option unterstützt, und zwar BUCKET_COUNT für Hashindizes.|  
  
## <a name="nonclustered-hash-indexes"></a>Nicht gruppierte Hashindizes  
 In der folgenden Tabelle werden die möglichen [!INCLUDE[tsql](../../includes/tsql-md.md)] -Funktionen und -Schlüsselwörter im Meldungstext eines Fehlers mit einem nicht gruppierten Hashindex sowie die Korrekturmaßnahmen zum Beheben des Fehlers aufgelistet.  
  
|type|Name|Lösung|  
|----------|----------|----------------|  
|Option|ASC/DESC|Nicht gruppierte Hashindizes werden nicht sortiert. Entfernen Sie die Schlüsselwörter **ASC** und **DESC** aus der Indexschlüsselspezifikation.|  
  
## <a name="natively-compiled-stored-procedures-and-user-defined-functions"></a>Nativ kompilierte gespeicherte Prozeduren und benutzerdefinierte Funktionen  
 In der folgenden Tabelle werden die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Funktionen und -Schlüsselwörter aufgelistet, die im Meldungstext eines Fehlers mit nativ kompilierten gespeicherten Prozeduren und benutzerdefinierten Funktionen auftreten können. Sie enthält außerdem die Korrekturmaßnahmen zum Beheben des Fehlers.  
  
|type|Funktion|Lösung|  
|----------|-------------|----------------|  
|Funktion|Inline-Tabellenvariablen|Tabellentypen können nicht inline mit Variablendeklarationen deklariert werden. Tabellentypen müssen explizit mit einer **CREATE TYPE** -Anweisung deklariert werden.|  
|Funktion|Cursor|Cursor werden nicht von oder in systemintern kompilierten gespeicherten Prozeduren unterstützt.<br /><br /> Wenn Sie die Prozedur von einem Client aus ausführen, verwenden Sie RPC anstelle der Cursor-API. Vermeiden Sie bei ODBC die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung **EXECUTE**, und geben Sie stattdessen den Namen der Prozedur direkt an.<br /><br /> Wenn Sie die Prozedur aus einem [!INCLUDE[tsql](../../includes/tsql-md.md)] -Batch oder einer anderen gespeicherten Prozedur ausführen, vermeiden Sie es, einen Cursor mit der systemintern kompilierten gespeicherten Prozedur zu verwenden.<br /><br /> Wenn Sie eine systemintern kompilierte gespeicherte Prozedur erstellen und keinen Cursor verwenden, verwenden Sie setbasierte Logik oder eine **WHILE** -Schleife.|  
|Funktion|Nicht konstante Parameterstandardwerte|Beim Angeben von Standardwerten für Parameter für systemintern kompilierte gespeicherte Prozeduren müssen die Werte Konstanten sein. Entfernen Sie alle Platzhalter aus den Parameterdeklarationen.|  
|Funktion|EXTERNAL|CLR-gespeicherte Prozeduren können nicht systemintern kompiliert werden. Entfernen Sie entweder die Klausel AS EXTERNAL oder die Option NATIVE_COMPILATIONS aus der Anweisung CREATE PROCEDURE.|  
|Funktion|numbered_stored_procedures|Systemintern kompilierte gespeicherte Prozeduren dürfen nicht nummeriert sein. Entfernen Sie die **;** _Nummer_ aus der **CREATE PROCEDURE** -Anweisung.|  
|Funktion|Mehrzeilige INSERT ... VALUES-Anweisungen|Das Einfügen mehrerer Zeilen mit der gleichen **INSERT** -Anweisung in einer systemintern kompilierten gespeicherten Prozedur ist nicht möglich. Erstellen Sie **INSERT** -Anweisungen für jede Zeile.|  
|Funktion|Allgemeine Tabellenausdrücke (CTEs)|Allgemeine Tabellenausdrücke (Common Table Expressions, CTE) werden in systemintern kompilierten gespeicherten Prozeduren nicht unterstützt. Schreiben Sie die Abfrage um.|  
|Funktion|COMPUTE|Die **COMPUTE** -Klausel wird nicht unterstützt. Entfernen Sie sie aus der Abfrage.|  
|Funktion|SELECT INTO|Die **INTO** -Klausel wird mit der **SELECT** -Anweisung nicht unterstützt. Schreiben Sie die Abfrage als **INSERT INTO** _Tabelle_ **SELECT** neu.|  
|Funktion|Unvollständige Einfügespaltenliste|Generell müssen Werte in INSERT-Anweisungen für alle Spalten in der Tabelle angegeben werden.<br /><br /> Allerdings unterstützen wir DEFAULT-Einschränkungen und IDENTITY(1,1)-Spalten in speicheroptimierten Tabellen. Diese Spalten können in der INSERT-Spaltenliste ausgelassen werden. IDENTITY-Spalten müssen sogar ausgelassen werden.|  
|Funktion|*Function*|Einige integrierte Funktionen werden in nativ kompilierten gespeicherten Prozeduren nicht unterstützt. Entfernen Sie die abgelehnte Funktion aus der gespeicherten Prozedur. Weitere Informationen zu unterstützten integrierten Funktionen finden Sie unter<br />[Unterstützte Funktionen für nativ kompilierte T-SQL-Module](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)oder<br />[Systemintern kompilierte gespeicherte Prozeduren](./a-guide-to-query-processing-for-memory-optimized-tables.md).|  
|Funktion|CASE|**Gilt für:** [!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] und SQL Server ab Version [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>**CASE**-Ausdrücke werden in Abfragen innerhalb von nativ kompilierten gespeicherten Prozeduren nicht unterstützt. Erstellen Sie Abfragen für jeden einzelnen Fall. Weitere Informationen finden Sie unter [Implementieren eines CASE-Ausdrucks in einer systemintern kompilierten gespeicherten Prozedur](../../relational-databases/in-memory-oltp/implementing-a-case-expression-in-a-natively-compiled-stored-procedure.md).<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] und SQL Server ab Version [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] unterstützen CASE-Ausdrücke.|  
|Funktion|INSERT EXECUTE|Entfernen Sie den Verweis.|  
|Funktion|Führen Sie|Nur unterstützt, um nativ kompilierte gespeicherte Prozeduren und benutzerdefinierte Funktionen auszuführen|  
|Funktion|Benutzerdefinierte Aggregate|Benutzerdefinierte Aggregatfunktionen können nicht in systemintern kompilierten gespeicherten Prozeduren verwendet werden. Entfernen Sie den Verweis auf die Funktion aus der Prozedur.|  
|Funktion|Metadaten des Durchsuchenmodus|Systemintern kompilierte gespeicherte Prozeduren unterstützen keine Metadaten des Durchsuchenmodus. Stellen Sie sicher, dass die **NO_BROWSETABLE** -Sitzungsoption auf OFF festgelegt ist.|  
|Funktion|DELETE mit FROM-Klausel|Die **FROM** -Klausel wird für **DELETE** -Anweisungen mit einer Tabellenquelle in systemintern kompilierten gespeicherten Prozeduren nicht unterstützt.<br /><br /> **DELETE** wird mit der **FROM** -Klausel unterstützt, wenn der Vorgang die Tabelle angibt, aus der Daten gelöscht werden sollen.|  
|Funktion|UPDATE mit FROM-Klausel|Die **FROM** -Klausel wird nicht für **UPDATE** -Anweisungen in systemintern kompilierten gespeicherten Prozeduren unterstützt.| 
|Funktion|Temporäre Prozeduren|Temporäre gespeicherte Prozeduren können nicht systemintern kompiliert werden. Erstellen Sie entweder eine dauerhafte systemintern kompilierte gespeicherte Prozedur oder eine als temporär interpretierte gespeicherte [!INCLUDE[tsql](../../includes/tsql-md.md)] -Prozedur.|  
|Isolationsstufe|READ UNCOMMITTED|Die Isolationsstufe READ UNCOMMITTED wird für systemintern kompilierte gespeicherte Prozeduren nicht unterstützt. Verwenden Sie eine unterstützte Isolationsstufe, beispielsweise SNAPSHOT.|  
|Isolationsstufe|READ COMMITTED|Die Isolationsstufe READ COMMITTED wird für nativ kompilierte gespeicherte Prozeduren nicht unterstützt. Verwenden Sie eine unterstützte Isolationsstufe, beispielsweise SNAPSHOT.|  
|Funktion|Temporäre Tabellen|Tabellen in tempdb können nicht in systemintern kompilierten gespeicherten Prozeduren verwendet werden. Verwenden Sie stattdessen eine Tabellenvariable oder eine speicheroptimierte Tabelle mit DURABILITY=SCHEMA_ONLY.|  
|Funktion|DTC|Auf speicheroptimierte Tabellen und systemintern kompilierte gespeicherte Prozeduren kann nicht aus verteilten Transaktionen zugegriffen werden. Verwenden Sie stattdessen SQL-Transaktionen.|  
|Funktion|EXECUTE WITH RECOMPILE|Die **WITH RECOMPILE** -Option wird für systemintern kompilierte gespeicherte Prozeduren nicht unterstützt.|  
|Funktion|Ausführung von der dedizierten Administratorverbindung.|Systemintern kompilierte gespeicherte Prozeduren können nicht aus der dedizierten Administratorverbindung (DAC) ausgeführt werden. Verwenden Sie stattdessen eine reguläre Verbindung.|  
|Vorgang|Sicherungspunkt|Systemintern kompilierte gespeicherte Prozeduren können nicht aus Transaktionen aufgerufen werden, die über einen aktiven Sicherungspunkt verfügen. Entfernen Sie den Sicherungspunkt aus der Transaktion.|  
|Vorgang|ALTER AUTHORIZATION|Das Ändern des Besitzers einer vorhandenen speicheroptimierten Tabelle oder systemintern kompilierten gespeicherten Prozedur wird nicht unterstützt. Löschen Sie die Tabelle oder die Prozedur, und erstellen Sie sie neu, um den Besitzer zu ändern.|  
|Operator|OPENROWSET|Dieser Operator wird nicht unterstützt. Entfernen Sie **OPENROWSET** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Operator|OPENQUERY|Dieser Operator wird nicht unterstützt. Entfernen Sie **OPENQUERY** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Operator|OPENDATASOURCE|Dieser Operator wird nicht unterstützt. Entfernen Sie **OPENDATASOURCE** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Operator|OPENXML|Dieser Operator wird nicht unterstützt. Entfernen Sie **OPENXML** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Operator|CONTAINSTABLE|Dieser Operator wird nicht unterstützt. Entfernen Sie **CONTAINSTABLE** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Operator|FREETEXTTABLE|Dieser Operator wird nicht unterstützt. Entfernen Sie **FREETEXTTABLE** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Funktion|Tabellenwertfunktionen|Auf Tabellenwertfunktionen kann nicht aus systemintern kompilierten gespeicherten Prozeduren verwiesen werden. Eine mögliche Lösung für diese Einschränkung besteht darin, die Logik in den Tabellenwertfunktionen dem Prozedurtext hinzuzufügen.|  
|Operator|CHANGETABLE|Dieser Operator wird nicht unterstützt. Entfernen Sie **CHANGETABLE** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Operator|GOTO|Dieser Operator wird nicht unterstützt. Verwenden Sie andere prozedurale Konstrukte wie WHILE.|  
|Operator|OFFSET|Dieser Operator wird nicht unterstützt. Entfernen Sie **OFFSET** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Operator|INTERSECT|Dieser Operator wird nicht unterstützt. Entfernen Sie **INTERSECT** aus der systemintern kompilierten gespeicherten Prozedur. In einigen Fällen kann ein INNER JOIN verwendet werden, um dasselbe Ergebnis zu erhalten.|  
|Operator|EXCEPT|Dieser Operator wird nicht unterstützt. Entfernen Sie **EXCEPT** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Operator|APPLY|**Gilt für:** [!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] und SQL Server ab Version [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Dieser Operator wird nicht unterstützt. Entfernen Sie **APPLY** aus der systemintern kompilierten gespeicherten Prozedur.<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] und SQL Server ab Version [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] unterstützen den APPLY-Operator in nativ kompilierten Modulen.|  
|Operator|PIVOT|Dieser Operator wird nicht unterstützt. Entfernen Sie **PIVOT** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Operator|UNPIVOT|Dieser Operator wird nicht unterstützt. Entfernen Sie **UNPIVOT** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Operator|CONTAINS|Dieser Operator wird nicht unterstützt. Entfernen Sie **CONTAINS** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Operator|FREETEXT|Dieser Operator wird nicht unterstützt. Entfernen Sie **FREETEXT** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Operator|TSEQUAL|Dieser Operator wird nicht unterstützt. Entfernen Sie **TSEQUAL** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Operator|LIKE|Dieser Operator wird nicht unterstützt. Entfernen Sie **LIKE** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Operator|NEXT VALUE FOR|In systemintern kompilierten gespeicherten Prozeduren kann nicht auf Sequenzen verwiesen werden. Rufen Sie den Wert mit interpretiertem [!INCLUDE[tsql](../../includes/tsql-md.md)]ab, und übergeben Sie ihn dann an die systemintern kompilierte gespeicherte Prozedur. Weitere Informationen finden Sie unter [Implementieren von IDENTITY in einer speicheroptimierten Tabelle](../../relational-databases/in-memory-oltp/implementing-identity-in-a-memory-optimized-table.md).|  
|SET-Option|*Option*|SET-Optionen können in systemintern kompilierten gespeicherten Prozeduren nicht geändert werden. Bestimmte Optionen können mit der BEGIN ATOMIC-Anweisung festgelegt werden. Weitere Informationen finden Sie im Abschnitt zu ATOMIC-Blöcken in [Systemintern kompilierte gespeicherte Prozeduren](./a-guide-to-query-processing-for-memory-optimized-tables.md).|  
|Operand|TABLESAMPLE|Dieser Operator wird nicht unterstützt. Entfernen Sie **TABLESAMPLE** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Option|RECOMPILE|Systemintern kompilierte gespeicherte Prozeduren werden bei der Erstellungszeit kompiliert. Entfernen Sie **RECOMPILE** aus der Prozedurdefinition.<br /><br /> Sie können „sp_recompile“ für eine nativ kompilierte gespeicherte Prozedur ausführen, wodurch die Funktion bei der nächsten Ausführung erneut kompiliert.|  
|Option|ENCRYPTION|Diese Option wird nicht unterstützt. Entfernen Sie **ENCRYPTION** aus der Prozedurdefinition.|  
|Option|FOR REPLICATION|Systemintern kompilierte gespeicherte Prozeduren können nicht für die Replikation erstellt werden. Entfernen Sie **FOR REPLICATION** aus der Prozedurdefinition.|  
|Option|FOR XML|Diese Option wird nicht unterstützt. Entfernen Sie **FOR XML** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Option|FOR BROWSE|Diese Option wird nicht unterstützt. Entfernen Sie **FOR BROWSE** aus der systemintern kompilierten gespeicherten Prozedur.|  
|Jointipp|HASH, MERGE|Systemintern kompilierte Prozeduren unterstützen nur den Nested Loops-Joins. Hash- und Zusammenführungsjoins werden nicht unterstützt. Entfernen Sie den Jointipp.|  
|Abfragetipp|*Abfragetipp*|Dieser Abfragetipp befindet sich nicht in systemintern kompilierten gespeicherten Prozeduren. Unterstützte Abfragetipps finden Sie unter [Abfragehinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).|  
|Option|PERCENT|Diese Option wird nicht für **TOP** -Klauseln unterstützt. Entfernen Sie **PERCENT** aus der Abfrage in der systemintern kompilierten gespeicherten Prozedur.|  
|Option|WITH TIES|**Gilt für:** [!INCLUDE[ssSDS14_md](../../includes/sssql14-md.md)] und [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Diese Option wird nicht für **TOP** -Klauseln unterstützt. Entfernen Sie **WITH TIES** aus der Abfrage in der systemintern kompilierten gespeicherten Prozedur.<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] und SQL Server ab Version [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] unterstützen **TOP WITH TIES**.|  
|Aggregate-Funktion|*Aggregatfunktion*|Nicht alle Aggregatfunktionen werden unterstützt. Weitere Informationen zu den unterstützten Aggregatfunktionen in nativ kompilierten T-SQL-Modulen finden Sie unter [Unterstützte Funktionen für nativ kompilierte T-SQL-Module](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).|  
|Rangfolgefunktion|*Rangfolgefunktion*|Rangfolgefunktionen werden nicht in systemintern kompilierten gespeicherten Prozeduren unterstützt. Entfernen Sie sie aus der Prozedurdefinition.|  
|Funktion|*Function*|Diese Funktion wird nicht unterstützt. Weitere Informationen zu den unterstützten Funktionen in nativ kompilierten T-SQL-Modulen finden Sie unter [Unterstützte Funktionen für nativ kompilierte T-SQL-Module](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).|  
|-Anweisung.|*Anweisung*|Diese Anweisung wird nicht unterstützt. Weitere Informationen zu den unterstützten Funktionen in nativ kompilierten T-SQL-Modulen finden Sie unter [Unterstützte Funktionen für nativ kompilierte T-SQL-Module](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).|  
|Funktion|MIN und MAX verwendet mit Binär- und Zeichenfolgen|Die Aggregatfunktionen **MIN** und **MAX** können nicht für Zeichenwerte oder binäre Zeichenfolgenwerte in systemintern kompilierten gespeicherten Prozeduren verwendet werden.|  
|Funktion|GROUP BY ALL|ALL kann nicht mit GROUP BY-Klauseln in systemintern kompilierten gespeicherten Prozeduren verwendet werden. Entfernen Sie ALL aus der GROUP BY-Klausel.|  
|Funktion|GROUP BY ()|Gruppierung nach einer leeren Liste wird nicht unterstützt. Entfernen Sie entweder die GROUP BY-Klausel, oder schließen Sie Spalten in der Gruppierungsliste ein.|  
|Funktion|ROLLUP|**ROLLUP** kann nicht mit **GROUP BY** -Klauseln in systemintern kompilierten gespeicherten Prozeduren verwendet werden. Entfernen Sie **ROLLUP** aus der Prozedurdefinition.|  
|Funktion|CUBE|**CUBE** kann nicht mit **GROUP BY** -Klauseln in systemintern kompilierten gespeicherten Prozeduren verwendet werden. Entfernen Sie **CUBE** aus der Prozedurdefinition.|  
|Funktion|GROUPING SETS|**GROUPING SETS** kann nicht mit **GROUP BY** -Klauseln in systemintern kompilierten gespeicherten Prozeduren verwendet werden. Entfernen Sie **GROUPING SETS** aus der Prozedurdefinition.|  
|Funktion|BEGIN TRANSACTION, COMMIT TRANSACTION und ROLLBACK TRANSACTION|Verwenden Sie ATOMIC-Blöcke, um Transaktionen und Fehlerbehandlung zu steuern. Weitere Informationen finden Sie unter [ATOMIC-Blöcke](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).|  
|Funktion|Deklarationen von Inlinetabellenvariablen.|Tabellenvariablen müssen auf explizit definierte speicheroptimierte Tabellentypen verweisen. Sie sollten einen speicheroptimierten Tabellentyp erstellen und diesen Typ für die Variablendeklaration verwenden, statt den Typ inline anzugeben.|  
|Funktion|Datenträgerbasierte Tabellen|Auf datenträgerbasierte Tabellen kann nicht von systemintern kompilierten gespeicherten Prozeduren zugegriffen werden. Entfernen Sie Verweise auf datenträgerbasierte Tabellen aus den systemintern kompilierten gespeicherten Prozeduren. Sie können auch datenträgerbasierte Tabellen zu speicheroptimierten Tabellen migrieren.|  
|Funktion|Sichten|Auf Sichten kann nicht von systemintern kompilierten gespeicherten Prozeduren zugegriffen werden. Verweisen Sie nicht auf Sichten, sondern auf die zugrunde liegenden Basistabellen.|  
|Funktion|Tabellenwertfunktionen|**Gilt für:** [!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] und SQL Server ab Version [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Auf Tabellenwertfunktionen mit mehreren Anweisungen kann nicht von nativ kompilierten T-SQL-Modulen aus zugegriffen werden. Inline-Tabellenwertfunktionen werden unterstützt. Diese müssen jedoch mit WITH NATIVE_COMPILATION erstellt werden.<br/><br/>**Gilt für:** [!INCLUDE[ssSQL14-md](../../includes/ssSQL14-md.md)]<br/>Auf Tabellenwertfunktionen kann nicht aus nativ kompilierten T-SQL-Modulen verwiesen werden.|  
|Option|PRINT|Verweis entfernen|  
|Funktion|DDL|DDL wird innerhalb von nativ kompilierten T-SQL-Modulen nicht unterstützt.|  
|Option|STATISTICS XML|Wird nicht unterstützt. Beim Ausführen einer Abfrage, für die STATISTICS XML aktiviert ist, wird der XML-Inhalt ohne den Teil für die nativ kompilierte gespeicherte Prozedur zurückgegeben.|  
  
## <a name="transactions-that-access-memory-optimized-tables"></a>Transaktionen, die auf speicheroptimierte Tabellen zugreifen  
 In der folgenden Tabelle werden die möglichen [!INCLUDE[tsql](../../includes/tsql-md.md)] -Funktionen und -Schlüsselwörter im Meldungstext eines Fehlers mit Transaktionen, die auf speicheroptimierte Tabellen zugreifen sowie die Korrekturmaßnahmen zum Beheben des Fehlers aufgelistet.  
  
|type|Name|Lösung|  
|----------|----------|----------------|  
|Funktion|Sicherungspunkt|Das Erstellen von expliziten Sicherungspunkten in Transaktionen, die auf speicheroptimierte Tabellen zugreifen, wird nicht unterstützt.|  
|Funktion|Gebundene Transaktion|Gebundene Sitzungen können nicht an Transaktionen teilnehmen, die auf speicheroptimierte Tabellen zugreifen. Binden Sie die Sitzung nicht, bevor Sie die Prozedur ausführen.|  
|Funktion|DTC|Transaktionen, die auf speicheroptimierte Tabellen zugreifen, können keine verteilten Transaktionen sein.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Migrieren zu In-Memory OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
