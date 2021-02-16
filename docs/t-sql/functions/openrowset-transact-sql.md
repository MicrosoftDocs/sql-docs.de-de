---
description: OPENROWSET (Transact-SQL)
title: OPENROWSET (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/30/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENROWSET_TSQL
- OPENROWSET
dev_langs:
- TSQL
helpviewer_keywords:
- data sources [SQL Server]
- OPENROWSET function
- remote data access [SQL Server], OPENROWSET
- ad hoc distributed queries
- OPENROWSET function, Transact-SQL
- OPENROWSET statement
- OLE DB data sources [SQL Server]
- ad hoc connection information
ms.assetid: f47eda43-33aa-454d-840a-bb15a031ca17
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: a2c7af1aaa1840b3c3468a00541f96325d8a5c52
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100352405"
---
# <a name="openrowset-transact-sql"></a>OPENROWSET (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Enthält alle für einen Zugriff auf Remotedaten von einer OLE DB-Datenquelle notwendigen Verbindungsinformationen. Diese Methode ist eine Alternative zum Zugriff auf Tabellen eines Verbindungsservers. Sie ist eine einmalig verwendete Ad-hoc-Methode zum Verbinden und Zugreifen auf Remotedaten mithilfe von OLE DB. Für häufigere Verweise auf OLE DB-Datenquellen verwenden Sie stattdessen Verbindungsserver. Weitere Informationen finden Sie unter [Verbindungsserver &#40;Datenbank-Engine&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md). Auf die `OPENROWSET`-Funktion kann in der FROM-Klausel einer Abfrage so verwiesen werden, als handele es sich um einen Tabellennamen. Auf die `OPENROWSET`-Funktion kann auch als Zieltabelle einer `INSERT`-, `UPDATE`- oder `DELETE`-Anweisung verwiesen werden, je nach den Funktionen des OLE DB-Anbieters. Obwohl die Abfrage möglicherweise mehrere Resultsets zurückgibt, gibt `OPENROWSET` nur das erste Resultset zurück.

`OPENROWSET` unterstützt auch Massenvorgänge über einen integrierten BULK-Anbieter, mit dem Daten aus einer Datei gelesen und als Rowset zurückgegeben werden können.

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql
OPENROWSET
( { 'provider_name' 
    , { 'datasource' ; 'user_id' ; 'password' | 'provider_string' }
    , {   <table_or_view> | 'query' }
   | BULK 'data_file' ,
       { FORMATFILE = 'format_file_path' [ <bulk_options> ]
       | SINGLE_BLOB | SINGLE_CLOB | SINGLE_NCLOB }
} )

<table_or_view> ::= [ catalog. ] [ schema. ] object

<bulk_options> ::=

   [ , DATASOURCE = 'data_source_name' ]

   [ , ERRORFILE = 'file_name' ]
   [ , ERRORFILE_DATA_SOURCE = 'data_source_name' ]
   [ , MAXERRORS = maximum_errors ]

   [ , FIRSTROW = first_row ]
   [ , LASTROW = last_row ]
   [ , ROWS_PER_BATCH = rows_per_batch ]
   [ , ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) [ UNIQUE ] ]
  
   -- bulk_options related to input file format
   [ , CODEPAGE = { 'ACP' | 'OEM' | 'RAW' | 'code_page' } ]
   [ , FORMAT = 'CSV' ]
   [ , FIELDQUOTE = 'quote_characters']
   [ , FORMATFILE = 'format_file_path' ]
   [ , FORMATFILE_DATA_SOURCE = 'data_source_name' ]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

### <a name="provider_name"></a>'*provider_name*'
Eine Zeichenfolge für den Anzeigenamen (oder die PROGID) des OLE DB-Anbieters, wie er in der Registrierung angegeben wurde. *provider_name* verfügt nicht über einen Standardwert. Beispiele für Anbieternamen sind die folgenden: `Microsoft.Jet.OLEDB.4.0`, `SQLNCLI` oder `MSDASQL`.

### <a name="datasource"></a>'*datasource*'
Eine Zeichenfolgenkonstante, die einer bestimmten OLE DB-Datenquelle entspricht. *datasource* ist die DBPROP_INIT_DATASOURCE-Eigenschaft, die zum Initialisieren des Anbieters an die IDBProperties-Schnittstelle des Anbieters übergeben werden muss. Normalerweise enthält diese Zeichenfolge den Namen der Datenbankdatei, den Namen des Datenbankservers oder einen Namen, mit dem der Anbieter die Datenbank(en) suchen kann.
Die Datenquelle kann der Dateipfad `C:\SAMPLES\Northwind.mdb'` für `Microsoft.Jet.OLEDB.4.0`-Anbieter oder die Verbindungszeichenfolge `Server=Seattle1;Trusted_Connection=yes;` für `SQLNCLI`-Anbieter sein.

### <a name="user_id"></a>'*user_id*'
Eine Zeichenfolgenkonstante für den Benutzernamen, der an den angegebenen OLE DB-Anbieter übergeben wird. *user_id* gibt den Sicherheitskontext für die Verbindung an und wird als DBPROP_AUTH_USERID-Eigenschaft übergeben, um den Anbieter zu initialisieren. *user_id* darf keine Anmelde-ID von Microsoft Windows sein.

### <a name="password"></a>'*password*'
Eine Zeichenfolgenkonstante für das Benutzerkennwort, das an den OLE DB-Anbieter übergeben wird. *password* wird beim Initialisieren des Anbieters als DBPROP_AUTH_PASSWORD-Eigenschaft übergeben. *password* darf kein Microsoft Windows-Kennwort sein.

```sql
SELECT a.*
   FROM OPENROWSET('Microsoft.Jet.OLEDB.4.0',
                   'C:\SAMPLES\Northwind.mdb';
                   'admin';
                   'password',
                   Customers) AS a;
```

### <a name="provider_string"></a>'*provider_string*'
Eine anbieterspezifische Verbindungszeichenfolge, die als DBPROP_INIT_PROVIDERSTRING-Eigenschaft übergeben wird, um den OLE DB-Anbieter zu initialisieren. *provider_string* kapselt normalerweise alle zum Initialisieren des Anbieters benötigten Verbindungsinformationen. Eine Liste der Schlüsselwörter, die vom OLE DB-Anbieter von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client erkannt werden, finden Sie unter [Initialization and Authorization Properties (Initialisierungs- und Autorisierungseigenschaften)](../../relational-databases/native-client-ole-db-data-source-objects/initialization-and-authorization-properties.md).

```sql
SELECT d.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
                            Department) AS d;
```

### <a name="table_or_view"></a><table_or_view>
Hier handelt es sich um die Remotetabelle oder die Ansicht, die die Daten enthält, die `OPENROWSET` lesen sollte. Dabei kann es sich um ein Objekt mit dreiteiligem Name bestehend aus den folgenden Komponenten handeln:
- *catalog* (optional) ist der Name des Katalogs oder der Datenbank, in der sich das angegebene Objekt befindet.
- *schema* (optional) ist der Name des Schemas oder des Besitzers für das angegebene Objekt.
- *object* ist der Objektname, der das zu verwendende Objekt eindeutig identifiziert.

```sql
SELECT d.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
                 AdventureWorks2012.HumanResources.Department) AS d;
```

### <a name="query"></a>'*query*'
Eine Zeichenfolgenkonstante, die zum Anbieter geschickt und von ihm ausgeführt wird. Die lokale Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verarbeitet nicht diese Abfrage, sondern die vom Anbieter zurückgegebenen Abfrageergebnisse (eine Pass-Through-Abfrage). Pass-Through-Abfragen eignen sich bei Anbietern, die ihre Tabellendaten nicht über Tabellennamen verfügbar machen, sondern nur über eine Befehlssprache. Pass-Through-Abfragen werden auf dem Remoteserver unterstützt, wenn der Abfrageanbieter das Command-Objekt von OLE DB und die dafür notwendigen Schnittstellen unterstützt. Weitere Informationen finden Sie unter [SQL Server Native Client &#40;OLE DB&#41; Reference (Verweis für SQL Server Native Client &#40;OLE DB&#41;)](../../relational-databases/native-client-ole-db-interfaces/sql-server-native-client-ole-db-interfaces.md).

```sql
SELECT a.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
     'SELECT TOP 10 GroupName, Name
     FROM AdventureWorks2012.HumanResources.Department') AS a;
```

### <a name="bulk"></a>BULK
Verwendet den BULK-Rowsetanbieter für OPENROWSET, um Daten aus einer Datei zu lesen. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann OPENROWSET aus einer Datendatei lesen, ohne die Daten in eine Zieltabelle zu laden. Auf diese Weise können Sie OPENROWSET mit einer einfachen SELECT-Anweisung verwenden.

> [!IMPORTANT]
> Azure SQL-Datenbank unterstützt nur das Lesen aus Azure Blob Storage.

Die Argumente der Option BULK ermöglichen eine erhebliche Kontrolle darüber, wo das Lesen von Daten begonnen und beendet werden soll, wie mit Fehlern umgegangen wird und wie Daten interpretiert werden sollen. Beispielsweise können Sie angeben, dass die Datendatei als einzeiliges, einspaltiges Rowset vom Datentyp **varbinary**, **varchar** oder **nvarchar** gelesen wird. Das Standardverhalten ist in den folgenden Argumentbeschreibungen erläutert.

 Informationen zum Verwenden der BULK-Option finden Sie unter "Hinweise" weiter unten in diesem Thema. Informationen zu den für die Option BULK erforderlichen Berechtigungen finden Sie im Abschnitt "Berechtigungen" weiter unten in diesem Thema.

> [!NOTE]
> Wenn OPENROWSET (BULK ...) zum Importieren von Daten mit dem vollständigen Wiederherstellungsmodell verwendet wird, wird die Protokollierung nicht optimiert.

Informationen zum Vorbereiten von Daten für Massenimport finden Sie unter [Vorbereiten von Daten für den Massenexport oder -import &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

#### <a name="bulk-data_file"></a>BULK '*data_file*'
Der vollständige Pfad der Datendatei, deren Daten in die Zieltabelle kopiert werden sollen.

```sql
SELECT * FROM OPENROWSET(
   BULK 'C:\DATA\inv-2017-01-19.csv',
   SINGLE_CLOB) AS DATA;
```

**Anwendungsbereich:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Ab [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 kann sich „data_file“ in Azure-Blobspeicher befinden. Beispiele finden Sie unter [Beispiele für Massenzugriff auf Daten in Azure Blob Storage](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

> [!IMPORTANT]
> Azure SQL-Datenbank unterstützt nur das Lesen aus Azure Blob Storage.

#### <a name="bulk-error-handling-options"></a>Fehlerbehandlungsoptionen mit BULK

##### <a name="errorfile"></a>ERRORFILE
`ERRORFILE` ='*file_name*' gibt die Datei an, die zum Erfassen der Zeilen verwendet wird, die Formatierungsfehler enthalten und nicht in ein OLE DB-Rowset konvertiert werden können. Diese Zeilen werden aus der Datendatei unverändert in diese Fehlerdatei kopiert.

Die Fehlerdatei wird zu Beginn der Ausführung des Befehls erstellt. Falls die Datei bereits vorhanden ist, wird ein Fehler ausgelöst. Darüber hinaus wird eine Kontrolldatei mit der Erweiterung .ERROR.txt erstellt. Diese Datei enthält einen Verweis auf jede Zeile in der Fehlerdatei und stellt eine Fehlerdiagnose bereit. Sobald die Fehler korrigiert wurden, können die Daten geladen werden.
**Anwendungsbereich:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Ab [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] kann sich `error_file_path` im Azure Blob Storage befinden.

##### <a name="errorfile_data_source_name"></a>ERRORFILE_DATA_SOURCE_NAME
**Anwendungsbereich:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Es handelt sich um eine benannte externe Datenquelle, die auf den Azure Blob-Speicherort der Fehlerdatei verweist, welche Fehler enthält, die während des Importierens gefunden wurden. Die externe Datenquelle muss mithilfe der in [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 hinzugefügten `TYPE = BLOB_STORAGE`-Option erstellt werden. Weitere Informationen finden Sie unter [CREATE EXTERNAL DATA SOURCE (CREATE EXTERNAL DATA SOURCE)](../../t-sql/statements/create-external-data-source-transact-sql.md).

##### <a name="maxerrors"></a>MAXERRORS
`MAXERRORS` =*maximum_errors* gibt an, nach wie vielen Syntaxfehlern oder nicht übereinstimmenden Zeilen gemäß der Definition im Dateiformat OPENROWSET eine Ausnahme auslöst. Bis zum Erreichen von MAXERRORS ignoriert OPENROWSET fehlerhafte Zeilen und lädt diese nicht, wobei jede fehlerhafte Zeile als ein Fehler gezählt wird.

Der Standardwert für *maximum_errors* ist 10.

> [!NOTE]
> `MAX_ERRORS` kann für CHECK-Einschränkungen oder zum Konvertieren der Datentypen **money** und **bigint** nicht verwendet werden.

#### <a name="bulk-data-processing-options"></a>Datenverarbeitungsoptionen mit BULK

##### <a name="firstrow"></a>FIRSTROW
`FIRSTROW` =*first_row* gibt die Nummer der ersten zu ladenden Zeile an. Der Standardwert ist 1. Damit wird die erste Zeile in der festgelegten Datendatei angegeben. Die Zeilennummern werden durch Zählen der Zeilenabschlusszeichen bestimmt. FIRSTROW ist einsbasiert.

##### <a name="lastrow"></a>LASTROW
`LASTROW` =*last_row* gibt die Nummer der letzten zu ladenden Zeile an. Die Standardeinstellung ist 0. Damit wird die letzte Zeile in der festgelegten Datendatei angegeben.

##### <a name="rows_per_batch"></a>ROWS_PER_BATCH
`ROWS_PER_BATCH` =*rows_per_batch* gibt die ungefähre Anzahl von Datenzeilen in der Datendatei an. Der Wert sollte von der gleichen Größenordnung sein wie die tatsächliche Zeilenanzahl.

`OPENROWSET` importiert eine Datendatei immer als einzelnen Batch. Wenn Sie jedoch *rows_per_batch* mit einem Wert > 0 angeben, verwendet der Abfrageprozessor den Wert unter *rows_per_batch* als Hinweis für die Zuordnung der Ressourcen im Abfrageplan.

Standardmäßig ist ROWS_PER_BATCH unbekannt. ROWS_PER_BATCH = 0 hat den gleichen Effekt, als würden Sie ROWS_PER_BATCH nicht angeben.

##### <a name="order"></a>ORDER
`ORDER` ( { *column* [ ASC | DESC ] } [ ,... *n* ] [ UNIQUE ] ) ist ein optionaler Hinweis, der angibt, wie die Daten in der Datendatei sortiert werden. Standardmäßig geht der Massenvorgang davon aus, dass die Datendatei nicht sortiert ist. Die Leistung kann verbessert werden, wenn die festgelegte Reihenfolge vom Abfrageoptimierer zur Generierung eines effizienteren Abfrageplans verwendet werden kann. Folgendes sind Beispiele dafür, wann die Festlegung einer Sortierung von Vorteil sein kann:

- Einfügen von Zeilen in eine Tabelle mit einem gruppierten Index, in der die Rowsetdaten nach dem Schlüssel des gruppierten Index sortiert sind.
- Verknüpfen des Rowsets mit einer anderen Tabelle, in der die Sortierungs- und Joinspalten übereinstimmen.
- Aggregieren der Rowsetdaten nach den Sortierspalten.
- Verwenden des Rowsets als Quelltabelle in der FROM-Klausel einer Abfrage, wobei die Sortierungs- und Joinspalten übereinstimmen.

##### <a name="unique"></a>UNIQUE
`UNIQUE` gibt an, dass die Datendatei keine doppelten Einträge aufweist.

Wenn die tatsächlichen Zeilen in der Datendatei nicht entsprechend der angegebenen Reihenfolge sortiert sind oder wenn der UNIQUE-Hinweis angegeben wird und doppelte Schlüssel vorhanden sind, wird ein Fehler zurückgegeben.

Spaltenaliase sind erforderlich, wenn ORDER verwendet wird. Die Spaltenaliasliste muss auf die abgeleitete Tabelle verweisen, auf die von der BULK-Klausel zugegriffen wird. Die Spaltennamen, die in der ORDER-Klausel angegeben sind, verweisen auf diese Spaltenaliasliste. Spalten mit großen Werttypen (**varchar(max)** , **nvarchar(max)** , **varbinary(max)** und **xml**) und großen Objekttypen (LOB) (**text**, **ntext** und **image**) können nicht angegeben werden.

##### <a name="single_blob"></a>SINGLE_BLOB
Gibt die Inhalte von *data_file* als einzelne Zeile, als einspaltiges Rowset des **varbinary(max)** -Typs zurück.

> [!IMPORTANT]
> Es wird empfohlen, XML-Daten anstelle von mit SINGLE_CLOB und SINGLE_NCLOB ausschließlich mithilfe der SINGLE_BLOB-Option zu importieren, da nur SINGLE_BLOB alle Windows-Codierungskonvertierungen unterstützt.

##### <a name="single_clob"></a>SINGLE_CLOB
Wenn *data_file* als ASCII gelesen wird, wird der Inhalt als einzeiliges, einspaltiges Rowset vom Typ **varchar(max)** zurückgegeben, wobei die Sortierung der aktuellen Datenbank verwendet wird.

##### <a name="single_nclob"></a>SINGLE_NCLOB
Wenn *data_file* als UNICODE gelesen wird, wird der Inhalt als einzeiliges, einspaltiges Rowset vom Typ **varchar(max)** zurückgegeben, wobei die Sortierung der aktuellen Datenbank verwendet wird.

```sql
SELECT *
   FROM OPENROWSET(BULK N'C:\Text1.txt', SINGLE_NCLOB) AS Document;
```

#### <a name="bulk-input-file-format-options"></a>Formatoptionen der Eingabedatei mit BULK

##### <a name="codepage"></a>CODEPAGE
`CODEPAGE` = { 'ACP' \| 'OEM' \| 'RAW' \| '*code_page*' } gibt die Codepage für die in der Datendatei enthaltenen Daten an. CODEPAGE ist nur dann von Bedeutung, wenn die Daten **char**-, **varchar**- oder **text**-Spalten mit Zeichenwerten enthalten, die größer als 127 oder kleiner als 32 sind.

> [!IMPORTANT]
> Die Option `CODEPAGE` wird unter Linux nicht unterstützt.

> [!NOTE]
> Es wird empfohlen, dass Sie für jede Spalte in einer Formatdatei einen Sortierungsnamen angeben, außer wenn die 65001-Option Priorität vor der Angabe von Sortierung/Codepage haben soll.

|CODEPAGE-Wert|BESCHREIBUNG|
|--------------------|-----------------|
|ACP|Konvertiert Spalten vom Datentyp **char**, **varchar** oder **text** von der ANSI-/[!INCLUDE[msCoName](../../includes/msconame-md.md)]-Windows-Codepage (ISO 1252) in die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Codepage.|
|OEM (Standard)|Konvertiert Spalten vom Datentyp **char**, **varchar** oder **text** von der OEM-Codepage des Systems in die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Codepage.|
|RAW|Es erfolgt keine Konvertierung in eine andere Codepage. Dies ist die schnellste Option.|
|*Codepage*|Gibt die Quellcodepage an, nach der die Zeichendaten in der Datendatei codiert werden, beispielsweise 850.<br /><br /> **Wichtig**: In Versionen vor [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] wird die Codepage 65001 (UTF-8-Codierung) nicht unterstützt.|

##### <a name="format"></a>FORMAT
`FORMAT` **=** ‚CSV‘ **Gilt für:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Gibt eine CSV-Datei an, die dem Standard [RFC 4180](https://tools.ietf.org/html/rfc4180) entspricht.

```sql
SELECT *
FROM OPENROWSET(BULK N'D:\XChange\test-csv.csv',
    FORMATFILE = N'D:\XChange\test-csv.fmt',
    FIRSTROW=2,
    FORMAT='CSV') AS cars;
```

##### <a name="formatfile"></a>FORMATFILE
`FORMATFILE` ='*format_file_path*' gibt den vollständigen Pfad einer Formatdatei an. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt zwei Typen von Formatdateien: XML- und Nicht-XML-Formatdateien.

Eine Formatdatei ist erforderlich, um Spaltentypen im Resultset zu definieren. Die einzige Ausnahme hierzu ist, dass SINGLE_CLOB, SINGLE_BLOB oder SINGLE_NCLOB angegeben ist. In diesem Fall ist die Formatdatei nicht erforderlich.

Weitere Informationen finden Sie unter [Massenimport von Daten mithilfe einer Formatdatei &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).

**Anwendungsbereich:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Ab [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 kann sich format_file_path in Azure Blob Storage befinden. Beispiele finden Sie unter [Beispiele für Massenzugriff auf Daten in Azure Blob Storage](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

##### <a name="fieldquote"></a>FIELDQUOTE
`FIELDQUOTE` **=** 'field_quote' **Gilt für:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Gibt ein Zeichen an, das als Anführungszeichen in der CSV-Datei verwendet wird. Wenn dies nicht angegeben ist, wird das Anführungszeichen (") so verwendet, wie es im Standard [RFC 4180](https://tools.ietf.org/html/rfc4180) definiert ist.

## <a name="remarks"></a>Bemerkungen

Mit `OPENROWSET` kann nur auf Remotedaten von OLE DB-Datenquellen zugegriffen werden, wenn für den angegebenen Anbieter die Registrierungsoption **DisallowAdhocAccess** explizit auf 0 festgelegt wird und wenn die erweiterte Konfigurationsoption „Ad Hoc Distributed Queries“ aktiviert ist. Wenn diese Optionen nicht festgelegt sind, ermöglicht das Standardverhalten keinen Ad-hoc-Zugriff.

Beim Zugriff auf OLE DB-Remotedatenquellen wird die Anmelde-ID vertrauenswürdiger Verbindungen nicht automatisch von dem Server delegiert, auf dem der Client mit dem Server verbunden ist, der abgefragt wird. Die Authentifizierungsdelegierung muss konfiguriert sein.

Der Katalog- und Schemaname sind erforderlich, falls der OLE DB-Anbieter mehrere Kataloge und Schemas in der angegebenen Datenquelle unterstützt. Die Angabe von Werten für _catalog_ und _schema_ kann entfallen, falls diese vom OLE DB-Anbieter nicht unterstützt werden. Falls der Anbieter nur Schemanamen unterstützt, muss ein zweiteiliger Name im Format _schema_ **.** _object_ angegeben werden. Falls der Anbieter nur Katalognamen unterstützt, muss ein dreiteiliger Name im Format _catalog_ **.** _schema_ **.** _object_ angegeben werden. Dreiteilige Namen müssen für Pass-Through-Abfragen angegeben werden, für die der OLE DB-Anbieter von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client verwendet wird. Weitere Informationen finden Sie unter [Transact-SQL-Syntaxkonventionen &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

Für die Argumente von `OPENROWSET` können keine Variablen verwendet werden.

Jeder Aufruf von `OPENDATASOURCE`, `OPENQUERY` oder `OPENROWSET` in der `FROM`-Klausel wird einzeln und unabhängig von anderen Aufrufen dieser Funktionen ausgewertet, die als Ziel des Updates verwendet werden, auch wenn für die beiden Aufrufe identische Argumente angegeben werden. Insbesondere haben Filter- oder Joinbedingungen, die auf das Ergebnis eines dieser Aufrufe angewendet werden, keine Auswirkungen auf die Ergebnisse des jeweils anderen.

## <a name="using-openrowset-with-the-bulk-option"></a>Verwenden von OPENROWSET mit der Option BULK

Die folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Erweiterungen unterstützen die OPENROWSET(BULK...)-Funktion:

- Mithilfe einer FROM-Klausel, die mit `SELECT` verwendet wird, kann `OPENROWSET(BULK...)` anstelle eines Tabellennamens mit voller `SELECT`-Funktionalität aufgerufen werden.

   `OPENROWSET` mit der `BULK`-Option erfordert in der `FROM`-Klausel einen abhängigen Namen (wird auch als Bereichsvariable oder Alias bezeichnet). Spaltenaliase können angegeben werden. Wenn keine Spaltenaliasliste angegeben wird, sind für die Formatdatei Spaltennamen notwendig. Durch das Angeben von Spaltenaliasen werden die Spaltennamen in der Formatdatei wie folgt überschrieben:

  - `FROM OPENROWSET(BULK...) AS table_alias`
  - `FROM OPENROWSET(BULK...) AS table_alias(column_alias,...n)`

  > [!IMPORTANT]
  > Wenn das Hinzufügen von `AS <table_alias>` fehlschlägt, führt dies zu folgendem Fehler: Meldung 491, Ebene 16, Status 1, Zeile 20: Für das Massenrowset in der FROM-Klausel muss ein abhängiger Name angegeben werden.

- Eine `SELECT...FROM OPENROWSET(BULK...)`-Anweisung fragt Daten in einer Datei direkt ab, ohne dass die Daten in eine Tabelle importiert werden. `SELECT...FROM OPENROWSET(BULK...)`-Anweisungen können auch Massenspaltenaliase auflisten, indem eine Formatdatei verwendet wird, um Spaltennamen und auch Datentypen anzugeben.
- Durch die Verwendung von `OPENROWSET(BULK...)` als Quelltabelle in einer `INSERT`- oder `MERGE`-Anweisung wird ein Massenimport von Daten aus einer Datendatei in eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Tabelle ausgeführt. Weitere Informationen finden Sie unter [Importieren von Massendaten mithilfe von BULK INSERT oder OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).
- Wenn die Option `OPENROWSET BULK` mit einer `INSERT`-Anweisung verwendet wird, unterstützt die BULK-Klausel Tabellenhinweise. Zusätzlich zu den regulären Tabellenhinweisen wie `TABLOCK`, kann die `BULK`-Klausel die folgenden spezialisierten Tabellenhinweise akzeptieren: `IGNORE_CONSTRAINTS` (ignoriert nur die `CHECK`- and `FOREIGN KEY`-Einschränkungen), `IGNORE_TRIGGERS`, `KEEPDEFAULTS` und `KEEPIDENTITY`. Weitere Informationen finden Sie unter [Tabellenhinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).

  Weitere Informationen zur Verwendung von `INSERT...SELECT * FROM OPENROWSET(BULK...)`-Anweisungen finden Sie unter [Massenimport und -export von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md). Informationen dazu, wann Zeileneinfügevorgänge, die durch den Massenimport ausgeführt werden, im Transaktionsprotokoll protokolliert werden, finden Sie unter [Voraussetzungen für die minimale Protokollierung beim Massenimport](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).

> [!NOTE]
> Für die Verwendung von `OPENROWSET` ist es wichtig, nachvollziehen zu können, wie in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit Identitätswechseln umgegangen wird. Weitere Informationen zu Überlegungen zur Sicherheit finden Sie unter [Importieren von Massendaten mithilfe von BULK INSERT oder OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

### <a name="bulk-importing-sqlchar-sqlnchar-or-sqlbinary-data"></a>Massenimport von SQLCHAR-, SQLNCHAR- oder SQLBINARY-Daten

Wenn keine maximale Länge für SQLCHAR-, SQLNCHAR- oder SQLBINARY-Daten angegeben wird, geht OPENROWSET(BULK...) davon aus, dass sie 8000 Bytes nicht überschreitet. Wenn sich die zu importierenden Daten in einem LOB-Datenfeld befinden, das Objekte vom Typ **varchar(max)** , **nvarchar(max)** oder **varbinary(max)** mit mehr als 8000 Bytes enthält, müssen Sie eine XML-Formatdatei verwenden, die die maximale Länge für das Datenfeld definiert. Um die maximale Länge anzugeben, bearbeiten Sie die Formatdatei, und deklarieren Sie das Attribut MAX_LENGTH.

> [!NOTE]
> Bei einer automatisch generierten Formatdatei wird die Länge oder maximale Länge für ein LOB-Feld nicht angeben. Sie können eine Formatdatei jedoch bearbeiten und die Länge oder maximale Länge manuell angeben.

### <a name="bulk-exporting-or-importing-sqlxml-documents"></a>Massenexportieren und -importieren von SQLXML-Dokumenten

Verwenden Sie in der Formatdatei einen der folgenden Datentypen für den Massenexport oder -import von SQLXML-Daten.

|Datentyp|Wirkung|
|---------------|------------|
|SQLCHAR oder SQLVARYCHAR|Die Daten werden in der Clientcodepage gesendet bzw. in der durch die Sortierung implizierten Codeseite.|
|SQLNCHAR oder SQLNVARCHAR|Die Daten werden im Unicode-Format gesendet.|
|SQLBINARY oder SQLVARYBIN|Die Daten werden ohne Konvertierung gesendet.|

## <a name="permissions"></a>Berechtigungen

Die `OPENROWSET`-Berechtigungen für OPENROWSET werden anhand der Berechtigungen des an den OLE DB-Anbieter übergebenen Benutzernamens bestimmt. Eine `ADMINISTER BULK OPERATIONS`- oder `ADMINISTER DATABASE BULK OPERATIONS`-Berechtigung ist erforderlich, um die `BULK`-Option zu verwenden.

## <a name="examples"></a>Beispiele

### <a name="a-using-openrowset-with-select-and-the-sql-server-native-client-ole-db-provider"></a>A. Verwenden von OPENROWSET mit SELECT und dem SQL Server Native Client OLE DB-Anbieter

Im folgenden Beispiel wird der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client-OLE DB-Anbieter für den Zugriff auf die `HumanResources.Department`-Tabelle in der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank auf dem Remoteserver `Seattle1` verwendet. (Wenn Sie SQLNCLI verwenden, leitet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zur neuesten Version des OLE DB-Anbieters von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client um.) Mithilfe einer `SELECT`-Anweisung wird das zurückgegebene Rowset definiert. Die Anbieterzeichenfolge enthält die Schlüsselwörter `Server` und `Trusted_Connection`. Diese Schlüsselwörter werden vom OLE DB-Anbieter von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client erkannt.

```sql
SELECT a.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
     'SELECT GroupName, Name, DepartmentID
      FROM AdventureWorks2012.HumanResources.Department
      ORDER BY GroupName, Name') AS a;
```

### <a name="b-using-the-microsoft-ole-db-provider-for-jet"></a>B. Verwenden des Microsoft OLE DB-Anbieters für Jet

Im folgenden Beispiel wird mithilfe des [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB-Anbieters für Jet auf die `Customers`-Tabelle in der `Northwind`-Datenbank von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Access zugegriffen.

> [!NOTE]
> In diesem Beispiel wird vorausgesetzt, dass Access installiert ist. Um dieses Beispiel auszuführen, müssen Sie die Northwind-Datenbank installieren.

```sql
SELECT CustomerID, CompanyName
   FROM OPENROWSET('Microsoft.Jet.OLEDB.4.0',
      'C:\Program Files\Microsoft Office\OFFICE11\SAMPLES\Northwind.mdb';
      'admin';'',Customers);
```

> [!IMPORTANT]
> Azure SQL-Datenbank unterstützt nur das Lesen aus Azure Blob Storage.

### <a name="c-using-openrowset-and-another-table-in-an-inner-join"></a>C. Verwenden von OPENROWSET und einer weiteren Tabelle in einem INNER JOIN

Im folgenden Beispiel werden alle Daten aus der `Customers`-Tabelle in der `Northwind`-Datenbank der lokalen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sowie alle Daten der `Orders`-Tabelle in der `Northwind`-Datenbank von Microsoft Access auf demselben Computer ausgewählt.

> [!NOTE]
> In diesem Beispiel wird vorausgesetzt, dass Access installiert ist. Um dieses Beispiel auszuführen, müssen Sie die Northwind-Datenbank installieren.

```sql
USE Northwind;
GO
SELECT c.*, o.*
FROM Northwind.dbo.Customers AS c
   INNER JOIN OPENROWSET('Microsoft.Jet.OLEDB.4.0',
   'C:\Program Files\Microsoft Office\OFFICE11\SAMPLES\Northwind.mdb';'admin';'', Orders)
   AS o
   ON c.CustomerID = o.CustomerID ;
```

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] unterstützt nur das Lesen aus Azure Blob Storage.

### <a name="d-using-openrowset-to-bulk-insert-file-data-into-a-varbinarymax-column"></a>D: Verwenden von OPENROWSET zum Masseneinfügen von Dateidaten in eine Spalte vom Typ varbinary(max)

 Das folgende Beispiel erstellt eine kleine Tabelle für Demonstrationszwecke und fügt Dateidaten aus der Datei `Text1.txt`, die im Stammverzeichnis `C:` gespeichert ist, in eine Spalte vom Datentyp `varbinary(max)` ein.

```sql
CREATE TABLE myTable(FileName NVARCHAR(60),
  FileType NVARCHAR(60), Document VARBINARY(max));
GO

INSERT INTO myTable(FileName, FileType, Document)
   SELECT
      'Text1.txt' AS FileName
      , '.txt' AS FileType
      , *
   FROM OPENROWSET(BULK N'C:\Text1.txt', SINGLE_BLOB) AS Document;
GO
```

> [!IMPORTANT]
> Azure SQL-Datenbank unterstützt nur das Lesen aus Azure Blob Storage.

### <a name="e-using-the-openrowset-bulk-provider-with-a-format-file-to-retrieve-rows-from-a-text-file"></a>E. Verwenden des OPENROWSET BULK-Anbieters mit einer Formatdatei zum Abrufen von Zeilen aus einer Textdatei

Im folgenden Beispiel werden mithilfe einer Formatdatei Zeilen aus der durch Tabstopps getrennten Textdatei `values.txt` abgerufen, die die folgenden Daten enthält:

```
1     Data Item 1
2     Data Item 2
3     Data Item 3
```

Die Formatdatei `values.fmt` beschreibt die Spalten in `values.txt`:

```
9.0
2  
1  SQLCHAR  0  10 "\t"        1  ID                      SQL_Latin1_General_Cp437_BIN
2  SQLCHAR  0  40 "\r\n"      2  Description        SQL_Latin1_General_Cp437_BIN
```

In der folgenden Abfrage werden diese Daten abgerufen:

```sql
SELECT a.* FROM OPENROWSET( BULK 'c:\test\values.txt',
   FORMATFILE = 'c:\test\values.fmt') AS a;
```

> [!IMPORTANT]
> Azure SQL-Datenbank unterstützt nur das Lesen aus Azure Blob Storage.

### <a name="f-specifying-a-format-file-and-code-page"></a>F. Angeben einer Formatdatei und Codepage

Im folgenden Beispiel wird gezeigt, wie Sie sowohl die FORMATFILE- als auch die CODEPAGE-Option zur gleichen Zeit verwenden.

```sql
INSERT INTO MyTable SELECT a.* FROM
OPENROWSET (BULK N'D:\data.csv', FORMATFILE =
    'D:\format_no_collation.txt', CODEPAGE = '65001') AS a;
```

### <a name="g-accessing-data-from-a-csv-file-with-a-format-file"></a>G. Zugriff auf Daten aus einer CSV-Datei mit einer Formatdatei

**Anwendungsbereich:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.

```sql
SELECT *
FROM OPENROWSET(BULK N'D:\XChange\test-csv.csv',
    FORMATFILE = N'D:\XChange\test-csv.fmt',
    FIRSTROW=2,
    FORMAT='CSV') AS cars;
```

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] unterstützt nur das Lesen aus Azure Blob Storage.

### <a name="h-accessing-data-from-a-csv-file-without-a-format-file"></a>H. Zugriff auf Daten aus einer CSV-Datei ohne eine Formatdatei

```sql
SELECT * FROM OPENROWSET(
   BULK 'C:\Program Files\Microsoft SQL Server\MSSQL14.CTP1_1\MSSQL\DATA\inv-2017-01-19.csv',
   SINGLE_CLOB) AS DATA;
```

```sql
SELECT *
FROM OPENROWSET
   (  'MSDASQL'
     ,'Driver={Microsoft Access Text Driver (*.txt, *.csv)}'
     ,'select * from E:\Tlog\TerritoryData.csv')
;
```

> [!IMPORTANT]
>
> - Der ODBC-Treiber sollte eine 64-Bit-Treiber sein. Öffnen Sie die Registerkarte **Treiber** der Anwendung [ODBC-Datenquellen](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md) in Windows, um dies zu bestätigen. Es gibt einen 32-Bit-`Microsoft Text Driver (*.txt, *.csv)`, der nicht mit einer 64-Bit-Version von „sqlservr.exe“ funktioniert.
> - Azure SQL-Datenbank unterstützt nur das Lesen aus Azure Blob Storage.

### <a name="i-accessing-data-from-a-file-stored-on-azure-blob-storage"></a>I. Zugriff auf Daten aus einer in Azure Blob Storage gespeicherten Datei

**Anwendungsbereich:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Im folgenden Beispiel wird eine externe Datenquelle verwendet, die auf einen Container im Azure-Speicherkonto und auf für SAS erstellte datenbankweit gültige Anmeldeinformationen verweist.

```sql
SELECT * FROM OPENROWSET(
   BULK 'inv-2017-01-19.csv',
   DATA_SOURCE = 'MyAzureInvoices',
   SINGLE_CLOB) AS DataFile;
```

Vollständige `OPENROWSET`-Beispiele einschließlich der Konfiguration der Anmeldeinformation und externen Datenquelle finden Sie unter [Beispiele für Massenzugriff auf Daten in Azure Blob Storage](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

### <a name="j-importing-into-a-table-from-a-file-stored-on-azure-blob-storage"></a>J. Importieren aus einer in Azure Blob Storage gespeicherten Datei in eine Tabelle

Das folgende Beispiel zeigt, wie Daten mithilfe des OPENROWSET-Befehls aus einer CSV-Datei in einen Speicherort von Azure Blob Storage geladen werden, für den Sie einen SAS-Schlüssel erstellt haben. Der Speicherort von Azure Blob Storage wird als externe Datenquelle konfiguriert. Hierfür sind datenbankweit gültige Anmeldeinformationen mit einer Shared Access Signature (SAS) erforderlich, die mit einem Hauptschlüssel in der Benutzerdatenbank verschlüsselt ist.

```sql
--> Optional - a MASTER KEY is not required if a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'YourStrongPassword1';
GO
--> Optional - a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE DATABASE SCOPED CREDENTIAL MyAzureBlobStorageCredential
 WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
 SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************';

 -- NOTE: Make sure that you don't have a leading ? in SAS token, and
 -- that you have at least read permission on the object that should be loaded srt=o&sp=r, and
 -- that expiration period is valid (all dates are in UTC time)

CREATE EXTERNAL DATA SOURCE MyAzureBlobStorage
WITH ( TYPE = BLOB_STORAGE,
          LOCATION = 'https://****************.blob.core.windows.net/curriculum'
          , CREDENTIAL= MyAzureBlobStorageCredential --> CREDENTIAL is not required if a blob is configured for public (anonymous) access!
);

INSERT INTO achievements with (TABLOCK) (id, description)
SELECT * FROM OPENROWSET(
   BULK  'csv/achievements.csv',
   DATA_SOURCE = 'MyAzureBlobStorage',
   FORMAT ='CSV',
   FORMATFILE='csv/achievements-c.xml',
   FORMATFILE_DATA_SOURCE = 'MyAzureBlobStorage'
    ) AS DataFile;
```

> [!IMPORTANT]
> Azure SQL-Datenbank unterstützt nur das Lesen aus Azure Blob Storage mithilfe des SAS-Tokens.

### <a name="additional-examples"></a>Zusätzliche Beispiele

Zusätzliche Beispiele für die Verwendung von `INSERT...SELECT * FROM OPENROWSET(BULK...)` finden Sie in den folgenden Themen:

- [Beispiele für den Massenimport und -export von XML-Dokumenten &#40;SQL Server&#41;](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)
- [Beibehalten von Identitätswerten beim Massenimport von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)
- [Beibehalten von NULL-Werten oder Verwenden von Standardwerten während des Massenimports &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)
- [Massenimport von Daten mithilfe einer Formatdatei &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [Verwenden des Zeichenformats zum Importieren und Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)
- [Überspringen einer Tabellenspalte mithilfe einer Formatdatei &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [Auslassen eines Datenfelds mithilfe einer Formatdatei &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)
- [Verwenden einer Formatdatei zum Zuordnen von Tabellenspalten zu Datendateifeldern &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>Weitere Informationen

- [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)
- [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)
- [Massenimport und -export von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)
- [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
- [OPENDATASOURCE &#40;Transact-SQL&#41;](../../t-sql/functions/opendatasource-transact-sql.md)
- [OPENQUERY &#40;Transact-SQL&#41;](../../t-sql/functions/openquery-transact-sql.md)
- [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)
- [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)
- [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)
- [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)
- [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)
