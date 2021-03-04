---
description: CREATE EXTERNAL TABLE (Transact-SQL)
title: CREATE EXTERNAL TABLE (Transact-SQL)
ms.custom: ''
ms.date: 02/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_EXTERNAL_TABLE
- CREATE EXTERNAL TABLE
- PolyBase, T-SQL
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, table create
- PolyBase, external table
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ed9508c4c5e518cb7d0b27694be8fa290f25be88
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837557"
---
# <a name="create-external-table-transact-sql"></a>CREATE EXTERNAL TABLE (Transact-SQL)

Erstellt eine externe Tabelle.

Dieser Artikel stellt die Syntax, Argumente, Anweisungen, Berechtigungen und Beispiele für das SQL-Produkt Ihrer Wahl bereit.

Weitere Informationen zu Syntaxkonventionen finden Sie unter [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL-Datenbank](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-sql-server"></a>Übersicht: SQL Server

Dieser Befehl erstellt eine externe Tabelle für PolyBase, um auf Daten in einem Hadoop-Cluster oder in Azure Blob Storage zuzugreifen. Eine externe PolyBase-Tabelle, die auf Daten in einem Hadoop-Cluster oder in Azure Blob Storage verweist.

**GILT FÜR**: SQL Server 2016 (oder höher)

Verwendet eine externe Tabelle mit einer externen Datenquelle für PolyBase-Abfragen. Externe Datenquellen werden zum Herstellen von Verbindungen verwendet und unterstützen diese primären Anwendungsfälle:

- Datenvirtualisierung und Laden von Dateien mithilfe von [PolyBase](../../relational-databases/polybase/polybase-guide.md)
- Massenladevorgänge mit SQL Server oder SQL-Datenbank mithilfe von `BULK INSERT` oder `OPENROWSET`

Siehe auch [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) und [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).

## <a name="syntax"></a>Syntax

```syntaxsql
-- Create a new external table
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )
    WITH (
        LOCATION = 'folder_or_filepath',
        DATA_SOURCE = external_data_source_name,
        FILE_FORMAT = external_file_format_name
        [ , <reject_options> [ ,...n ] ]
    )
[;]

<reject_options> ::=
{
    | REJECT_TYPE = value | percentage
    | REJECT_VALUE = reject_value
    | REJECT_SAMPLE_VALUE = reject_sample_value
}

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
```

## <a name="arguments"></a>Argumente

*{ database_name.schema_name.table_name | schema_name.table_name | table_name }* : Der ein- bis dreiteilige Name der zu erstellenden Tabelle. Für eine externe Tabelle speichert SQL nur die Metadaten der Tabelle mit den grundlegenden Statistiken über die Datei oder den Ordner, auf die in Hadoop oder Azure Blob Storage verwiesen wird. Es werden keine tatsächlichen Daten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verschoben oder gespeichert.

> [!IMPORTANT]
> Wenn der externe Datenquellentreiber einen dreiteiligen Namen unterstützt, wird dringend empfohlen, diesen dreiteiligen Namen anzugeben, um eine optimale Leistung zu erzielen.  

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE unterstützt das Konfigurieren von Spaltenname, Datentyp, NULL-Zulässigkeit und Sortierung. Sie können DEFAULT CONSTRAINT nicht für externe Tabellen verwenden.

Die Spaltendefinitionen, einschließlich der Datentypen und der Anzahl der Spalten, müssen mit den Daten in den externen Dateien übereinstimmen. Wenn ein Konflikt besteht, werden die Zeilen der Datei beim Abfragen der tatsächlichen Daten zurückgewiesen.

LOCATION = '*folder_or_filepath*': Gibt den Ordner oder den Dateipfad und Dateinamen für die tatsächlichen Daten in Hadoop oder Azure Blob Storage an. Der Speicherort beginnt im Stammordner. Der Stammordner ist der in der externen Datenquelle angegebene Datenspeicherort.

In SQL Server erstellt die Anweisung CREATE EXTERNAL TABLE den Pfad und den Ordner, wenn diese noch nicht vorhanden sind. Sie können dann INSERT INTO zum Exportieren von Daten aus einer lokalen SQL Server-Tabelle in die externe Datenquelle verwenden. Weitere Informationen finden Sie unter [PolyBase-Abfragen](../../relational-databases/polybase/polybase-queries.md).

Wenn LOCATION als Ordner angegeben wird, ruft eine PolyBase-Abfrage, die aus der externen Tabelle auswählt, Dateien aus dem Ordner und allen Unterordnern ab. PolyBase gibt wie Hadoop keine ausgeblendeten Ordner zurück. Es werden auch keine Dateien zurückgegeben, deren Dateiname mit einem Unterstrich (_) oder einem Punkt (.) beginnt.

In diesem Beispiel gibt eine PolyBase-Abfrage Zeilen aus „mydata.txt“ und „mydata2.txt“ zurück, wenn LOCATION='/webdata/'. „Mydata3.txt“ wird nicht zurückgegeben, da es sich um eine Datei eines ausgeblendeten Ordners handelt. Es werden auch keine „_hidden.txt“-Dateien zurückgegeben, da es sich um eine ausgeblendete Datei handelt.

![Rekursive Daten für externe Tabellen](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Rekursive Daten für externe Tabellen")

Legen Sie das Attribut \<polybase.recursive.traversal> in der Konfigurationsdatei „core-site.xml“ auf FALSE fest, um den Standardordner zu ändern, und nur aus dem Stammordner zu lesen. Diese Datei befindet sich unter `<SqlBinRoot>\PolyBase\Hadoop\Conf with SqlBinRoot the bin root of SQl Server`. Beispiel: `C:\\Program Files\\Microsoft SQL Server\\MSSQL13.XD14\\MSSQL\\Binn`.

DATA_SOURCE = *external_data_source_name*: Gibt den Namen der externen Datenquelle an, die den Speicherort der externen Daten enthält. Bei diesem Speicherort handelt es sich um ein Hadoop-Dateisystem (HDFS), einen Azure Store-Blobcontainer oder Azure Data Lake Store. Verwenden Sie zum Erstellen einer externen Datenquelle [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *external_file_format_name*: Gibt den Namen des externen Dateiformatobjekts an, das den Dateityp und die Komprimierungsmethode für die externen Daten enthält. Verwenden Sie zum Erstellen eines externen Dateiformats [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Reject-Optionen: Sie können Reject-Parameter angeben, die bestimmen, wie PolyBase *modifizierte* Datensätze behandelt, die aus der externen Datenquelle abgerufen werden. Ein Datensatz gilt als „dirty“ (modifiziert), wenn die tatsächlichen Datentypen oder die Anzahl der Spalten nicht den Spaltendefinitionen der externen Tabelle entsprechen.

Wenn Sie die Reject-Werte nicht angeben oder ändern, verwendet PolyBase Standardwerte. Diese Informationen über die Reject-Parameter werden als zusätzliche Metadaten gespeichert, wenn Sie eine externe Tabelle mit der CREATE EXTERNAL TABLE-Anweisung erstellen. Wenn eine zukünftige SELECT- oder SELECT INTO SELECT-Anweisung Daten aus der externen Tabelle auswählt, wird PolyBase die Reject-Optionen verwenden, um die Anzahl oder den Prozentsatz der Zeilen zu bestimmen, die zurückgewiesen werden können, bevor die tatsächliche Abfrage fehlschlägt. Die Abfrage gibt (Teil-) Ergebnisse zurück, bis der Reject-Schwellenwert überschritten wird. Daraufhin wird eine entsprechende Fehlermeldung ausgelöst.

REJECT_TYPE = **value** | percentage: Gibt an, ob die Option „REJECT_VALUE“ als Literalwert oder als Prozentsatz angegeben wird.

value: REJECT_VALUE ist ein Literalwert und kein Prozentsatz. Die PolyBase-Abfrage schlägt fehl, wenn die Anzahl der abgelehnten Zeilen *reject_value* überschreitet.

Die SELECT-Abfrage von PolyBase schlägt beispielsweise bei „REJECT_VALUE = 5“ und „REJECT_TYPE = value“ fehl, nachdem fünf Zeilen abgelehnt wurden.

percentage: REJECT_VALUE ist ein Prozentsatz und kein Literalwert. Eine PolyBase-Abfrage schlägt fehl, wenn der *Prozentsatz* fehlerhafter Zeilen *reject_value* überschreitet. Der Prozentsatz der fehlerhaften Zeilen wird in Intervallen berechnet.

REJECT_VALUE = *reject_value*: Gibt den Wert oder den Prozentsatz der Zeilen an, die zurückgewiesen werden können, bevor die Abfrage fehlschlägt.

Wenn REJECT_TYPE = Wert, muss *reject_value* eine ganze Zahl zwischen 0 und 2.147.483.647 sein.

Wenn REJECT_TYPE = Prozentzahl, muss *reject_value* eine Gleitkommazahl zwischen 0 und 100 sein.

REJECT_SAMPLE_VALUE = *reject_sample_value*: Dieses Attribut ist erforderlich, wenn Sie REJECT_TYPE = Prozentsatz angeben. Bestimmt die Anzahl der Zeilen, bei denen versucht wird, sie abzurufen, bevor die PolyBase den Prozentsatz der abgelehnten Zeilen neu berechnet.

Der *reject_sample_value*-Parameter muss eine ganze Zahl zwischen 0 und 2.147.483.647 sein.

Ist beispielsweise REJECT_SAMPLE_VALUE = 1000, dann berechnet PolyBase den Prozentsatz von fehlerhaften Zeilen nach dem Importversuch von 1000 Zeilen aus der externen Datendatei. Ist der Prozentsatz von fehlerhaften Zeilen kleiner als *reject_value*, führt PolyBase einen erneuten Abrufversuch von 1000 Zeilen aus. Nach jedem weiteren Importversuch von 1000 Zeilen wird der Prozentsatz von fehlerhaften Zeilen weiterhin neu berechnet.

> [!NOTE]
> Da die Berechnung des Prozentsatzes von fehlerhaften Zeilen durch PolyBase in Intervallen erfolgt, kann der tatsächliche Prozentsatz fehlerhafter Zeilen *reject_value* überschreiten.

Beispiel:

In diesem Beispiel wird verdeutlicht, wie die drei REJECT-Optionen interagieren. Gilt beispielsweise REJECT_TYPE = Prozentsatz, REJECT_VALUE = 30 und REJECT_SAMPLE_VALUE = 100, dann könnte das folgende Szenario auftreten:

- PolyBase versucht, die ersten 100 Zeilen abzurufen. Davon sind 25 fehlerhaft und 75 erfolgreich.
- Der berechnete Prozentsatz fehlerhafter Zeilen ist mit 25 % kleiner als der REJECT-Wert von 30 %. Aus diesem Grund wird PolyBase weiterhin versuchen, Daten aus der externen Datenquelle abzurufen.
- PolyBase versucht, die nächsten 100 Zeilen zu laden. Dieses Mal sind 25 Zeilen erfolgreich und 75 Zeilen fehlerhaft.
- Der Prozentsatz fehlerhafter Zeilen wird mit 50 % neu berechnet. Der Prozentsatz fehlerhafter Zeilen hat den REJECT-Wert von 30 % überschritten.
- Die PolyBase-Abfrage schlägt fehl, da nach der Rückgabe der ersten 200 Zeilen 50 % der Zeilen abgelehnt werden. Beachten Sie, dass übereinstimmende Zeilen zurückgegeben wurden, bevor die PolyBase-Abfrage erkennt, dass der Schwellenwert zum Zurückweisen überschritten wurde.

SCHEMA_NAME: Die SCHEMA_NAME-Klausel bietet die Möglichkeit, die externe Tabellendefinition einer Tabelle in einem anderen Schema auf der Remotedatenbank zuzuordnen. Verwenden Sie diese Klausel, um zwischen Schemas zu unterscheiden, die auf den lokalen und Remotedatenbanken vorhanden sind.

OBJECT_NAME: Die OBJECT_NAME-Klausel bietet die Möglichkeit, die externe Tabellendefinition einer Tabelle mit einem anderen Namen auf der Remotedatenbank zuzuordnen. Verwenden Sie diese Klausel, um zwischen Objektnamen zu unterscheiden, die auf den lokalen und Remotedatenbanken vorhanden sind.

DISTRIBUTION ist optional. Dieses Argument ist für Datenbanken des Typs SHARD_MAP_MANAGER erforderlich. Dieses Argument steuert, ob eine Tabelle wie eine Tabelle mit Shards oder replizierte Tabelle behandelt wird. Mit Tabellen des Typs **SHARDED** (*Spaltenname*) überlappen die Daten aus verschiedenen Tabellen nicht. **REPLICATED** gibt an, dass Tabellen dieselben Daten auf jeder Shard enthalten. **ROUND_ROBIN** gibt an, dass eine anwendungsspezifische Methode zum Verteilen von Daten verwendet wird.

## <a name="permissions"></a>Berechtigungen

Folgende Benutzerberechtigungen sind erforderlich:

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**
- **CONTROL DATABASE**

Beachten Sie, dass die Anmeldung, die die externe Datenquelle erstellt, die Berechtigung zum Lesen und Schreiben in der externen Datenquelle, die in Hadoop oder Azure Blob Storage gespeichert ist, benötigt.

> [!IMPORTANT]
> Mit der Berechtigung ALTER ANY EXTERNAL DATA SOURCE besitzt jeder Prinzipal die Fähigkeit, beliebige externe Datenquellenobjekte zu erstellen und zu ändern. Damit ist auch der Zugriff auf alle datenbankweit gültigen Anmeldeinformationen der Datenbank möglich. Da es sich hierbei um eine weitreichende Berechtigung handelt, darf sie nur vertrauenswürdigen Prinzipalen innerhalb des Systems erteilt werden.

## <a name="error-handling"></a>Fehlerbehandlung

Beim Ausführen der CREATE EXTERNAL TABLE-Anweisung versucht PolyBase, eine Verbindung mit der externen Datenquelle herzustellen. Tritt bei der Verbindung ein Fehler auf, schlägt die Anweisung fehl. Die externe Tabelle wird nicht erstellt. Da PolyBase erneut versucht, die Verbindung aufzubauen, bevor die Abfrage endgültig fehlschlägt, kann es eine Minute oder länger dauern, bis der Befehl fehlschlägt.

## <a name="general-remarks"></a>Allgemeine Hinweise

PolyBase speichert die aus der externen Datenquelle abgerufenen Zeilen in Szenarios mit Ad-hoc-Abfragen, z.B. bei SELECT FROM EXTERNAL TABLE, in einer temporären Tabelle. Nachdem die Abfrage abgeschlossen ist, entfernt und löscht PolyBase die temporäre Tabelle. Es werden keine permanenten Daten in SQL-Tabellen gespeichert.

Im Gegensatz dazu speichert PolyBase die aus der externen Datenquelle abgerufenen Zeilen in Importszenarios, z.B. bei SELECT INTO FROM EXTERNAL TABLE, permanent in einer SQL-Tabelle. Die neue Tabelle wird beim Ausführen der Abfrage erstellt, wenn PolyBase die externen Daten abruft.

PolyBase kann einen Teil der Abfrageberechnung an Hadoop übertragen, um die Abfrageleistung zu verbessern. Diese Aktion wird als Prädikatweitergabe bezeichnet. Um sie zu aktivieren, geben Sie die Option „Resource Manager Location“ von Hadoop in [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) an.

Sie können zahlreiche externe Tabellen erstellen, die auf die gleichen oder andere externe Datenquellen verweisen.

## <a name="limitations-and-restrictions"></a>Einschränkungen

Da die Daten für eine externe Tabelle nicht direkt von SQL Server verwaltet und gesteuert werden, können die Daten jederzeit von einem externen Prozess geändert oder entfernt werden. Aus diesem Grund sind Abfrageergebnisse für eine externe Tabelle nicht garantiert deterministisch. Die gleiche Abfrage kann bei jeder Ausführung für eine externe Tabelle unterschiedliche Ergebnisse zurückgeben. Auf ähnliche Weise kann eine Abfrage fehlschlagen, wenn die externen Daten verschoben oder entfernt werden.

Sie können zahlreiche externe Tabellen erstellen, die alle auf unterschiedliche externe Datenquellen verweisen. Wenn Sie Abfragen für verschiedene Hadoop-Datenquellen gleichzeitig ausführen, muss jede Hadoop-Datenquelle die gleiche „Hadoop Connectivity“-Serverkonfigurationseinstellung verwenden. Beispielsweise können Sie nicht gleichzeitig eine Abfrage für einen Cloudera Hadoop-Cluster und einen Hortonworks Hadoop-Cluster ausführen, da diese unterschiedliche Konfigurationseinstellungen verwenden. Weitere Informationen zu den Konfigurationseinstellungen und den unterstützten Kombinationen finden Sie unter [Konfiguration der PolyBase-Netzwerkkonnektivität](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).

Nur diese DDL-Anweisungen (Data Definition Language) sind in externen Tabellen zulässig:

- CREATE TABLE und DROP TABLE
- CREATE STATISTICS und DROP STATISTICS
- CREATE VIEW und DROP VIEW

Nicht unterstützte Konstruktionen und Operationen:

- Die DEFAULT-Einschränkung auf externen Tabellenspalten
- DML-Vorgänge (Data Manipulation Language): DELETE, INSERT und UPDATE

### <a name="query-limitations"></a>Abfrageeinschränkungen

PolyBase kann bei 32 gleichzeitigen PolyBase-Abfragen maximal 33.000 Dateien pro Ordner verarbeiten. Diese maximale Anzahl schließt sowohl Dateien als auch Unterordner im jeweiligen HDFS-Ordner ein. Werden weniger als 32 Abfragen gleichzeitig ausgeführt, können auch PolyBase-Abfragen für Ordner in HDFS ausgeführt werden, die mehr als 33.000 Dateien enthalten. Es wird empfohlen, dass Sie externe Dateipfade kurz halten und nicht mehr als 30.000 Dateien pro HDFS-Ordner verwenden. Wenn auf zu viele Dateien verwiesen wird, kann eine Out-of-Memory-Ausnahme von Java Virtual Machine (JVM) auftreten.

### <a name="table-width-limitations"></a>Einschränkungen der Tabellenbreite

PolyBase in SQL Server 2016 verfügt über eine Begrenzung für die Zeilenbreite von 32 KB, basierend auf der Maximalgröße einer einzelnen gültigen Zeile je Tabellendefinition. Wenn die Summe des Spaltenschemas größer als 32 KB ist, kann PolyBase die Daten nicht abfragen.

### <a name="data-type-limitations"></a>Einschränkungen für Datentypen

Die folgenden Datentypen können nicht in externen PolyBase-Tabellen verwendet werden:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Jeder benutzerdefinierte Typ

## <a name="locking"></a>Sperren

Freigegebene Sperre für das SCHEMARESOLUTION-Objekt.

## <a name="security"></a>Sicherheit

Die Datendateien für eine externe Tabelle werden in Hadoop oder Azure Blob Storage gespeichert. Diese Datendateien werden von Ihrem eigenen Prozess erstellt und verwaltet. Die Sicherheit der externen Daten liegt in Ihrer Verantwortung.

## <a name="examples"></a>Beispiele

### <a name="a-create-an-external-table-with-data-in-text-delimited-format"></a>A. Erstellen einer externen Tabelle mit Daten im Texttrennzeichenformat

Dieses Beispiel zeigt die erforderlichen Schritte zur Erstellung einer externen Tabelle, die Daten in Texttrennzeichendateien formatiert. Es definiert eine externe Datenquelle *mydatasource* und ein externes Dateiformat *myfileformat*. In der CREATE EXTERNAL TABLE-Anweisung wird dann auf diese Objekte auf Datenbankebene verwiesen. Weitere Informationen finden Sie unter [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) und unter [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat
WITH (
    FORMAT_TYPE = DELIMITEDTEXT,
    FORMAT_OPTIONS (FIELD_TERMINATOR ='|')
);

CREATE EXTERNAL TABLE ClickStream (
    url varchar(50),
    event_date date,
    user_IP varchar(50)
)
WITH (
        LOCATION='/webdata/employee.tbl',
        DATA_SOURCE = mydatasource,
        FILE_FORMAT = myfileformat
    )
;
```

### <a name="b-create-an-external-table-with-data-in-rcfile-format"></a>B. Erstellen einer externen Tabelle mit Daten im RCFile-Format

Dieses Beispiel zeigt die erforderlichen Schritte zur Erstellung einer externen Tabelle, die Daten als RCFiles formatiert. Es definiert eine externe Datenquelle *mydatasource_rc* und ein externes Dateiformat *mfileformat_rc*. In der CREATE EXTERNAL TABLE-Anweisung wird dann auf diese Objekte auf Datenbankebene verwiesen. Weitere Informationen finden Sie unter [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) und unter [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource_rc
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat_rc
WITH (
    FORMAT_TYPE = RCFILE,
    SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'
)
;

CREATE EXTERNAL TABLE ClickStream_rc (
    url varchar(50),
    event_date date,
    user_ip varchar(50)
)
WITH (
        LOCATION='/webdata/employee_rc.tbl',
        DATA_SOURCE = mydatasource_rc,
        FILE_FORMAT = myfileformat_rc
    )
;
```

### <a name="c-create-an-external-table-with-data-in-orc-format"></a>C. Erstellen einer externen Tabelle mit Daten im ORC-Format

Dieses Beispiel zeigt die erforderlichen Schritte zur Erstellung einer externen Tabelle, die Daten als ORC-Dateien formatiert. Es definiert eine externe Datenquelle mydatasource_orc und ein externes Dateiformat myfileformat_orc. In der CREATE EXTERNAL TABLE-Anweisung wird dann auf diese Objekte auf Datenbankebene verwiesen. Weitere Informationen finden Sie unter [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) und unter [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource_orc
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat_orc
WITH (
    FORMAT = ORC,
    COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
)
;

CREATE EXTERNAL TABLE ClickStream_orc (
    url varchar(50),
    event_date date,
    user_ip varchar(50)
)
WITH (
        LOCATION='/webdata/',
        DATA_SOURCE = mydatasource_orc,
        FILE_FORMAT = myfileformat_orc
    )
;
```

### <a name="d-querying-hadoop-data"></a>D: Abfragen von Hadoop-Daten

Clickstream ist eine externe Tabelle, die mit der durch Tabstopps getrennte Textdatei „employee.tbl“ in einem Hadoop-Cluster verbunden ist. Die folgende Abfrage sieht wie eine Abfrage für eine Standardtabelle aus. Allerdings ruft diese Abfrage Daten aus Hadoop ab und berechnet dann die Ergebnisse.

```sql
SELECT TOP 10 (url) FROM ClickStream WHERE user_ip = 'xxx.xxx.xxx.xxx'
;
```

### <a name="e-join-hadoop-data-with-sql-data"></a>E. Verknüpfen von Hadoop-Daten mit SQL-Daten

Diese Abfrage sieht wie ein Standard-JOIN für zwei SQL-Tabellen aus. Der Unterschied besteht darin, dass PolyBase die Clickstream-Daten aus Hadoop abruft und sie dann der UrlDescription-Tabelle hinzufügt. Eine Tabelle ist eine externe Tabelle, und die andere ist eine standardmäßige SQL-Tabelle.

```sql
SELECT url.description
FROM ClickStream cs
JOIN UrlDescription url ON cs.url = url.name
WHERE cs.url = 'msdn.microsoft.com'
;
```

### <a name="f-import-data-from-hadoop-into-a-sql-table"></a>F. Importieren Sie Daten aus Hadoop in eine SQL-Tabelle

Dieses Beispiel erstellt eine neue „ms_user“-SQL-Tabelle, die das Ergebnis einer Verknüpfung zwischen der standardmäßigen SQL-Tabelle *user* und der externen Tabelle *ClickStream* permanent speichert.

```sql
SELECT DISTINCT user.FirstName, user.LastName
INTO ms_user
FROM user INNER JOIN (
    SELECT * FROM ClickStream WHERE cs.url = 'www.microsoft.com'
    ) AS ms
ON user.user_ip = ms.user_ip
;
```

### <a name="g-create-an-external-table-for-a-sharded-data-source"></a>G. Erstellen einer externen Tabelle für eine Datenquelle mit Shards

In diesem Beispiel wird eine Remote-DMV mithilfe der Klauseln SCHEMA_NAME und OBJECT_NAME einer externen Tabelle zugeordnet.

```sql
CREATE EXTERNAL TABLE [dbo].[all_dm_exec_requests]([session_id] smallint NOT NULL,
  [request_id] int NOT NULL,
  [start_time] datetime NOT NULL,
  [status] nvarchar(30) NOT NULL,
  [command] nvarchar(32) NOT NULL,
  [sql_handle] varbinary(64),
  [statement_start_offset] int,
  [statement_end_offset] int,
  [cpu_time] int NOT NULL)
WITH
(
  DATA_SOURCE = MyExtSrc,
  SCHEMA_NAME = 'sys',
  OBJECT_NAME = 'dm_exec_requests',
  DISTRIBUTION=ROUND_ROBIN
);
```

### <a name="h-create-an-external-table-for-sql-server"></a>H. Erstellen einer externen Tabelle für SQL Server

```sql
     -- Create a Master Key
      CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';
    GO
     /*  specify credentials to external data source
     *  IDENTITY: user name for external source.
     *  SECRET: password for external source.
     */
     CREATE DATABASE SCOPED CREDENTIAL SqlServerCredentials
     WITH IDENTITY = 'username', Secret = 'password';
    GO

    /* LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE SQLServerInstance
    WITH (
    LOCATION = 'sqlserver://SqlServer',
    -- PUSHDOWN = ON | OFF,
      CREDENTIAL = SQLServerCredentials
    );
    GO

    CREATE SCHEMA sqlserver;
    GO

     /* LOCATION: sql server table/view in 'database_name.schema_name.object_name' format
     * DATA_SOURCE: the external data source, created above.
     */
     CREATE EXTERNAL TABLE sqlserver.customer(
     C_CUSTKEY INT NOT NULL,
     C_NAME VARCHAR(25) NOT NULL,
     C_ADDRESS VARCHAR(40) NOT NULL,
     C_NATIONKEY INT NOT NULL,
     C_PHONE CHAR(15) NOT NULL,
     C_ACCTBAL DECIMAL(15,2) NOT NULL,
     C_MKTSEGMENT CHAR(10) NOT NULL,
     C_COMMENT VARCHAR(117) NOT NULL
      )
      WITH (
      LOCATION='tpch_10.dbo.customer',
      DATA_SOURCE=SqlServerInstance
     );
```

### <a name="i-create-an-external-table-for-oracle"></a>I. Erstellen einer externen Tabelle für Oracle

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

   /*
   * LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
   * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
   * CONNECTION_OPTIONS: Specify driver location
   * CREDENTIAL: the database scoped credential, created above.
   */
   CREATE EXTERNAL DATA SOURCE external_data_source_name
   WITH (
     LOCATION = 'oracle://<server address>[:<port>]',
     -- PUSHDOWN = ON | OFF,
     CREDENTIAL = credential_name)

   /*
   * LOCATION: Oracle table/view in '.<schema_name>.<object_name>' format
   * DATA_SOURCE: the external data source, created above.
   */
   CREATE EXTERNAL TABLE customers(
   [O_ORDERKEY] DECIMAL(38) NOT NULL,
   [O_CUSTKEY] DECIMAL(38) NOT NULL,
   [O_ORDERSTATUS] CHAR COLLATE Latin1_General_BIN NOT NULL,
   [O_TOTALPRICE] DECIMAL(15,2) NOT NULL,
   [O_ORDERDATE] DATETIME2(0) NOT NULL,
   [O_ORDERPRIORITY] CHAR(15) COLLATE Latin1_General_BIN NOT NULL,
   [O_CLERK] CHAR(15) COLLATE Latin1_General_BIN NOT NULL,
   [O_SHIPPRIORITY] DECIMAL(38) NOT NULL,
   [O_COMMENT] VARCHAR(79) COLLATE Latin1_General_BIN NOT NULL
   )
   WITH (
    LOCATION='.mySchema.customer',
    DATA_SOURCE= external_data_source_name
   );
```

### <a name="j-create-an-external-table-for-teradata"></a>J. Erstellen einer externen Tabelle für Teradata

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';

   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

    /* LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CONNECTION_OPTIONS: Specify driver location
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (
    LOCATION = teradata://<server address>[:<port>],
   -- PUSHDOWN = ON | OFF,
    CREDENTIAL =credential_name
    );


     /* LOCATION: Teradata table/view in '<database_name>.<object_name>' format
      * DATA_SOURCE: the external data source, created above.
      */
     CREATE EXTERNAL TABLE customer(
      L_ORDERKEY INT NOT NULL,
      L_PARTKEY INT NOT NULL,
     L_SUPPKEY INT NOT NULL,
     L_LINENUMBER INT NOT NULL,
     L_QUANTITY DECIMAL(15,2) NOT NULL,
     L_EXTENDEDPRICE DECIMAL(15,2) NOT NULL,
     L_DISCOUNT DECIMAL(15,2) NOT NULL,
     L_TAX DECIMAL(15,2) NOT NULL,
     L_RETURNFLAG CHAR NOT NULL,
     L_LINESTATUS CHAR NOT NULL,
     L_SHIPDATE DATE NOT NULL,
     L_COMMITDATE DATE NOT NULL,
     L_RECEIPTDATE DATE NOT NULL,
     L_SHIPINSTRUCT CHAR(25) NOT NULL,
     L_SHIPMODE CHAR(10) NOT NULL,
     L_COMMENT VARCHAR(44) NOT NULL
     )
     WITH (
     LOCATION='customer',
     DATA_SOURCE= external_data_source_name
     );
```

### <a name="k-create-an-external-table-for-mongodb"></a>K. Erstellen einer externen Tabelle für MongoDB

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';

   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

     /* LOCATION: Location string should be of format '<type>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CONNECTION_OPTIONS: Specify driver location
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (
    LOCATION = mongodb://<server>[:<port>],
    -- PUSHDOWN = ON | OFF,
      CREDENTIAL = credential_name
    );

     /* LOCATION: MongoDB table/view in '<database_name>.<schema_name>.<object_name>' format
     * DATA_SOURCE: the external data source, created above.
     */
     CREATE EXTERNAL TABLE customers(
     [O_ORDERKEY] DECIMAL(38) NOT NULL,
     [O_CUSTKEY] DECIMAL(38) NOT NULL,
     [O_ORDERSTATUS] CHAR COLLATE Latin1_General_BIN NOT NULL,
     [O_TOTALPRICE] DECIMAL(15,2) NOT NULL,
     [O_ORDERDATE] DATETIME2(0) NOT NULL,
     [O_COMMENT] VARCHAR(79) COLLATE Latin1_General_BIN NOT NULL
     )
     WITH (
     LOCATION='customer',
     DATA_SOURCE= external_data_source_name
     );
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL-Datenbank \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-sql-database"></a>Übersicht: Azure SQL-Datenbank

Erstellt in Azure SQL-Datenbank eine externe Tabelle für [elastische Abfragen (in der Vorschau)](/azure/sql-database/sql-database-elastic-query-overview/).


Siehe auch [ERSTELLEN EINER EXTERNEN DATENQUELLE](../../t-sql/statements/create-external-data-source-transact-sql.md).

## <a name="syntax"></a>Syntax

```syntaxsql
-- Create a table for use with elastic query  
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH ( <sharded_external_table_options> )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
  
<sharded_external_table_options> ::=  
        DATA_SOURCE = external_data_source_name,
        SCHEMA_NAME = N'nonescaped_schema_name',  
        OBJECT_NAME = N'nonescaped_object_name',  
        [DISTRIBUTION  = SHARDED(sharding_column_name) | REPLICATED | ROUND_ROBIN]]  
    )  
[;]  
```

## <a name="arguments"></a>Argumente

*{ database_name.schema_name.table_name | schema_name.table_name | table_name }* : Der ein- bis dreiteilige Name der zu erstellenden Tabelle. Für eine externe Tabelle speichert SQL nur die Metadaten der Tabelle mit den grundlegenden Statistiken über die Datei oder den Ordner, auf die in Azure SQL-Datenbank verwiesen wird. Es werden keine tatsächlichen Daten in Azure SQL-Datenbank verschoben oder gespeichert.

> [!IMPORTANT]
> Wenn der externe Datenquellentreiber einen dreiteiligen Namen unterstützt, wird dringend empfohlen, diesen dreiteiligen Namen anzugeben, um eine optimale Leistung zu erzielen.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE unterstützt das Konfigurieren von Spaltenname, Datentyp, NULL-Zulässigkeit und Sortierung. Sie können DEFAULT CONSTRAINT nicht für externe Tabellen verwenden.

> [!NOTE]
> `Text`, `nText` und `XML` sind keine unterstützten Datentypen für Spalten in externen Tabellen für Azure SQL-Datenbank.

Die Spaltendefinitionen, einschließlich der Datentypen und der Anzahl der Spalten, müssen mit den Daten in den externen Dateien übereinstimmen. Wenn ein Konflikt besteht, werden die Zeilen der Datei beim Abfragen der tatsächlichen Daten zurückgewiesen.

Externe Shardtabellenoptionen

Hiermit wird die externe Datenquelle (eine nicht-SQL Server-Datenquelle) und eine Verteilungsmethode für die [elastische Datenbankabfrage](/azure/azure-sql/database/elastic-query-overview) angegeben.

DATA_SOURCE: Die DATA_SOURCE-Klausel definiert die externe Datenquelle (eine Shardzuordnung), die für die externe Tabelle verwendet wird. Ein Beispiel finden Sie unter [Erstellen externer Tabellen](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning#13-create-external-tables).

SCHEMA_NAME und OBJECT_NAME: Die Klauseln SCHEMA_NAME und OBJECT_NAME ordnen die Definition der externen Tabelle einer Tabelle in einem anderen Schema zu. Falls nicht angegeben, wird davon ausgegangen, dass das Schema des Remoteobjekts „dbo“ und sein Name mit dem definierten Namen der externen Tabelle identisch ist. Dies ist nützlich, wenn der Name der Remotetabelle bereits in der Datenbank verwendet wird, in der Sie die externe Tabelle erstellen möchten. Sie möchten z. B. eine externe Tabelle zum Abrufen einer aggregierten Sicht von Katalogsichten oder DMVs für Ihre horizontal hochskalierte Datenebene definieren. Da Katalogsichten und DMVs bereits lokal vorhanden sind, können Sie ihre Namen nicht für die Definition der externen Tabelle verwenden. Verwenden Sie stattdessen in den Klauseln SCHEMA_NAME und/oder OBJECT_NAME einen anderen Namen und den Namen der Katalogsicht oder DMV. Ein Beispiel finden Sie unter [Erstellen externer Tabellen](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning#13-create-external-tables).

DISTRIBUTION: Die DISTRIBUTION-Klausel gibt die Datenverteilung für diese Tabelle an. Der Abfrageprozessor nutzt die Informationen in der DISTRIBUTION-Klausel, um die effizientesten Abfragepläne zu erstellen.

- SHARDED bedeutet, dass Daten datenbankübergreifend horizontal partitioniert werden. Der Partitionierungsschlüssel für die Datenverteilung ist der Parameter <sharding_column_name>.
- REPLICATED bedeutet, dass identische Kopien der Tabelle in jeder Datenbank vorhanden sind. Sie müssen sicherstellen, dass die Replikate in allen Datenbanken identisch sind.
- ROUND_ROBIN bedeutet, dass die Tabelle mit einer anwendungsabhängigen Verteilungsmethode horizontal partitioniert wird.

## <a name="permissions"></a>Berechtigungen

Benutzer mit Zugriff auf die externe Tabelle erhalten automatisch Zugriff auf die zugrunde liegenden Remotetabellen gemäß den Anmeldeinformationen, die in der externen Datenquellendefinition angegeben sind. Vermeiden Sie eine unerwünschte Erhöhung von Berechtigungen durch die Anmeldeinformationen der externen Datenquelle. Verwenden Sie GRANT oder REVOKE für eine externe Tabelle, als handele es sich um eine normale Tabelle. Nachdem Sie die externe Datenquelle und die externen Tabellen definiert haben, können Sie jetzt vollständiges T-SQL in den externen Tabellen verwenden.

## <a name="error-handling"></a>Fehlerbehandlung

Wenn während der Ausführung der CREATE EXTERNAL TABLE-Anweisung beim Verbindungsversuch ein Fehler auftritt, schlägt die Anweisung fehl. Die externe Tabelle wird nicht erstellt. Da SQL-Datenbank erneut versucht, die Verbindung aufzubauen, bevor die Abfrage endgültig fehlschlägt, kann es eine Minute oder länger dauern, bis der Befehl fehlschlägt.

## <a name="general-remarks"></a>Allgemeine Hinweise

SQL-Datenbank speichert die aus der externen Datenquelle abgerufenen Zeilen in Szenarien mit Ad-hoc-Abfragen, z.B. bei SELECT FROM EXTERNAL TABLE, in einer temporären Tabelle. Nachdem die Abfrage abgeschlossen ist, entfernt und löscht SQL-Datenbank die temporäre Tabelle. Es werden keine permanenten Daten in SQL-Tabellen gespeichert.

Im Gegensatz dazu speichert SQL-Datenbank die aus der externen Datenquelle abgerufenen Zeilen in Importszenarien, z.B. bei SELECT INTO FROM EXTERNAL TABLE, permanent in einer SQL-Tabelle. Die neue Tabelle wird beim Ausführen der Abfrage erstellt, wenn SQL-Datenbank die externen Daten abruft.

Sie können zahlreiche externe Tabellen erstellen, die auf die gleichen oder andere externe Datenquellen verweisen.

## <a name="limitations-and-restrictions"></a>Einschränkungen

Der Zugriff auf Daten über eine externe Tabelle entspricht nicht der Isolationssemantik in SQL Server. Das bedeutet, dass beim Abfragen einer externen Tabelle keine Sperren oder Momentaufnahmeisolation erzwungen werden und sich daher zurückgegebene Daten ändern können, wenn die Daten in der externen Datenquelle geändert werden.  Die gleiche Abfrage kann bei jeder Ausführung für eine externe Tabelle unterschiedliche Ergebnisse zurückgeben. Auf ähnliche Weise kann eine Abfrage fehlschlagen, wenn die externen Daten verschoben oder entfernt werden.

Sie können zahlreiche externe Tabellen erstellen, die alle auf unterschiedliche externe Datenquellen verweisen.

Nur diese DDL-Anweisungen (Data Definition Language) sind in externen Tabellen zulässig:

- CREATE TABLE und DROP TABLE
- CREATE VIEW und DROP VIEW

Nicht unterstützte Konstruktionen und Operationen:

- Die DEFAULT-Einschränkung auf externen Tabellenspalten
- DML-Vorgänge (Data Manipulation Language): DELETE, INSERT und UPDATE

Nur die in einer Abfrage definierten Literalprädikate können per Push an die externe Datenquelle übertragen werden. Darin unterscheiden sie sich von verknüpften Servern und dem Zugriff darauf. Dort können Prädikate verwendet werden, die während der Abfrageausführung bestimmt wurden. Ein Beispiel hierfür ist die Verwendung in Verbindung mit einer geschachtelten Schleife in einem Abfrageplan. Dadurch wird häufig die gesamte externe Tabelle lokal kopiert und anschließend verknüpft.

```sql
  \\ Assuming External.Orders is an external table and Customer is a local table.
  \\ This query  will copy the whole of the external locally as the predicate needed
  \\ to filter isn't known at compile time. Its only known during execution of the query
  
  SELECT Orders.OrderId, Orders.OrderTotal
    FROM External.Orders
   WHERE CustomerId in (SELECT TOP 1 CustomerId
                          FROM Customer
                          WHERE CustomerName = 'MyCompany')
```

Mit externen Tabellen kann die Verwendung von Parallelität im Abfrageplan verhindert werden.

Externe Tabellen werden als Remoteabfrage implementiert, wodurch die geschätzte Anzahl der zurückgegebenen Zeilen in der Regel 1.000 beträgt. Weitere Regeln basierend auf dem Prädikatstyp können zum Filtern der externen Tabelle verwendet werden. Dabei handelt es sich um regelbasierte Schätzungen und nicht um Schätzwerte, die auf den tatsächlichen Daten der externen Tabelle beruhen. Der Optimierer greift nicht auf die Remotedatenquelle zu, um genauere Schätzungen zu erhalten.

### <a name="data-type-limitations"></a>Einschränkungen für Datentypen

Die folgenden Datentypen können nicht in externen PolyBase-Tabellen verwendet werden:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Jeder benutzerdefinierte Typ

## <a name="locking"></a>Sperren

Freigegebene Sperre für das SCHEMARESOLUTION-Objekt.

## <a name="examples"></a>Beispiele

### <a name="a-create-external-table-for-azure-sql-database"></a>A. Erstellen einer externen Tabelle für Azure SQL-Datenbank

```sql
CREATE EXTERNAL TABLE [dbo].[CustomerInformation]
( [CustomerID] [int] NOT NULL,
  [CustomerName] [varchar](50) NOT NULL,
  [Company] [varchar](50) NOT NULL)
WITH
( DATA_SOURCE = MyElasticDBQueryDataSrc)
```

## <a name="see-also"></a>Weitere Informationen

- [Übersicht über elastische Abfragen in Azure SQL-Datenbank](/azure/sql-database/sql-database-elastic-query-overview)
- [Erstellen von Berichten für horizontal hochskalierte Clouddatenbanken](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning)
- [Erste Schritte mit datenbankübergreifenden Abfragen (vertikale Partitionierung)](/azure/sql-database/sql-database-elastic-query-getting-started-vertical)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-synapse-analytics"></a>Übersicht: Azure Synapse Analytics

Verwenden Sie eine externe Tabelle, um:

- Daten in Hadoop oder in Azure Blob Storage mit [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen abzufragen.
- Daten aus Hadoop oder Azure Blob Storage zu importieren und zu speichern.
- Daten aus Azure Data Lake Storage zu importieren und zu speichern.

Siehe auch [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) und [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).  

## <a name="syntax"></a>Syntax

### [[!INCLUDE[sss-dedicated-pool-md.md](../../includes/sss-dedicated-pool-md.md)]](#tab/dedicated)
```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH (
        LOCATION = 'hdfs_folder_or_filepath',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
  
<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage,  
    | REJECT_VALUE = reject_value,  
    | REJECT_SAMPLE_VALUE = reject_sample_value,
    | REJECTED_ROW_LOCATION = '/REJECT_Directory'
}  
```
### [[!INCLUDE[sssod-md.md](../../includes/sssod-md.md)]](#tab/serverless)
```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name } 
    ( <column_definition> [ ,...n ] )   
    WITH ( 
        LOCATION = 'folder_or_filepath',   
        DATA_SOURCE = external_data_source_name,   
        FILE_FORMAT = external_file_format_name   
    )   
[;]   
<column_definition> ::= 
column_name <data_type> 
    [ COLLATE collation_name ] 
```
---

## <a name="arguments"></a>Argumente

*{ database_name.schema_name.table_name | schema_name.table_name | table_name }* : Der ein- bis dreiteilige Name der zu erstellenden Tabelle. Für eine externe Tabelle werden nur die Metadaten der Tabelle mit den grundlegenden Statistiken über die Datei oder den Ordner gespeichert, auf die in Azure Data Lake, Hadoop oder Azure Blob Storage verwiesen wird. Bei der Erstellung externer Tabellen werden keine tatsächlichen Daten verschoben oder gespeichert.

> [!IMPORTANT]
> Wenn der externe Datenquellentreiber einen dreiteiligen Namen unterstützt, wird dringend empfohlen, diesen dreiteiligen Namen anzugeben, um eine optimale Leistung zu erzielen.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE unterstützt das Konfigurieren von Spaltenname, Datentyp, NULL-Zulässigkeit und Sortierung. Sie können DEFAULT CONSTRAINT nicht für externe Tabellen verwenden.

> [!NOTE]
> `Text`, `nText` und `XML` sind keine unterstützten Datentypen für Spalten in externen Tabellen für Azure SQL Warehouse.

Die Spaltendefinitionen, einschließlich der Datentypen und der Anzahl der Spalten, müssen mit den Daten in den externen Dateien übereinstimmen. Wenn ein Konflikt besteht, werden die Zeilen der Datei beim Abfragen der tatsächlichen Daten zurückgewiesen.

LOCATION = '*folder_or_filepath*': Gibt den Ordner oder den Dateipfad und Dateinamen für die tatsächlichen Daten in Azure Data Lake, Hadoop oder Azure Blob Storage an. Der Speicherort beginnt im Stammordner. Der Stammordner ist der in der externen Datenquelle angegebene Datenspeicherort. Die Anweisung [CREATE EXTERNAL TABLE AS SELECT](create-external-table-as-select-transact-sql.md) erstellt den Pfad und den Ordner, wenn diese noch nicht vorhanden sind. `CREATE EXTERNAL TABLE` erstellt den Pfad und den Ordner nicht.

Wenn LOCATION als Ordner angegeben wird, ruft eine PolyBase-Abfrage, die aus der externen Tabelle auswählt, Dateien aus dem Ordner und allen Unterordnern ab. PolyBase gibt wie Hadoop keine ausgeblendeten Ordner zurück. Es werden auch keine Dateien zurückgegeben, deren Dateiname mit einem Unterstrich (_) oder einem Punkt (.) beginnt.

In diesem Beispiel gibt eine PolyBase-Abfrage Zeilen aus „mydata.txt“ und „mydata2.txt“ zurück, wenn LOCATION='/webdata/'. „Mydata3.txt“ wird nicht zurückgegeben, da es sich um einen Unterordner eines ausgeblendeten Ordners handelt. Es werden auch keine „_hidden.txt“-Dateien zurückgegeben, da es sich um eine ausgeblendete Datei handelt.

![Rekursive Daten für externe Tabellen](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Rekursive Daten für externe Tabellen")

DATA_SOURCE = *external_data_source_name*: Gibt den Namen der externen Datenquelle an, die den Speicherort der externen Daten enthält. Dieser Speicherort befindet sich in Azure Data Lake. Verwenden Sie zum Erstellen einer externen Datenquelle [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *external_file_format_name*: Gibt den Namen des externen Dateiformatobjekts an, das den Dateityp und die Komprimierungsmethode für die externen Daten enthält. Verwenden Sie zum Erstellen eines externen Dateiformats [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Reject-Optionen: Sie können Reject-Parameter angeben, die bestimmen, wie PolyBase *modifizierte* Datensätze behandelt, die aus der externen Datenquelle abgerufen werden. Ein Datensatz gilt als „dirty“ (modifiziert), wenn die tatsächlichen Datentypen oder die Anzahl der Spalten nicht den Spaltendefinitionen der externen Tabelle entsprechen.

Wenn Sie die Reject-Werte nicht angeben oder ändern, verwendet PolyBase Standardwerte. Diese Informationen über die Reject-Parameter werden als zusätzliche Metadaten gespeichert, wenn Sie eine externe Tabelle mit der CREATE EXTERNAL TABLE-Anweisung erstellen. Wenn eine zukünftige SELECT- oder SELECT INTO SELECT-Anweisung Daten aus der externen Tabelle auswählt, wird PolyBase die Reject-Optionen verwenden, um die Anzahl oder den Prozentsatz der Zeilen zu bestimmen, die zurückgewiesen werden können, bevor die tatsächliche Abfrage fehlschlägt. Die Abfrage gibt (Teil-) Ergebnisse zurück, bis der Reject-Schwellenwert überschritten wird. Daraufhin wird eine entsprechende Fehlermeldung ausgelöst.

REJECT_TYPE = **value** | percentage: Gibt an, ob die Option „REJECT_VALUE“ als Literalwert oder als Prozentsatz angegeben wird.

value: REJECT_VALUE ist ein Literalwert und kein Prozentsatz. Die PolyBase-Abfrage schlägt fehl, wenn die Anzahl der abgelehnten Zeilen *reject_value* überschreitet.

Die SELECT-Abfrage von PolyBase schlägt beispielsweise bei „REJECT_VALUE = 5“ und „REJECT_TYPE = value“ fehl, nachdem fünf Zeilen abgelehnt wurden.

percentage: REJECT_VALUE ist ein Prozentsatz und kein Literalwert. Eine PolyBase-Abfrage schlägt fehl, wenn der *Prozentsatz* fehlerhafter Zeilen *reject_value* überschreitet. Der Prozentsatz der fehlerhaften Zeilen wird in Intervallen berechnet.

REJECT_VALUE = *reject_value*: Gibt den Wert oder den Prozentsatz der Zeilen an, die zurückgewiesen werden können, bevor die Abfrage fehlschlägt.

Wenn REJECT_TYPE = Wert, muss *reject_value* eine ganze Zahl zwischen 0 und 2.147.483.647 sein.

Wenn REJECT_TYPE = Prozentzahl, muss *reject_value* eine Gleitkommazahl zwischen 0 und 100 sein.

REJECT_SAMPLE_VALUE = *reject_sample_value*: Dieses Attribut ist erforderlich, wenn Sie REJECT_TYPE = Prozentsatz angeben. Bestimmt die Anzahl der Zeilen, bei denen versucht wird, sie abzurufen, bevor die PolyBase den Prozentsatz der abgelehnten Zeilen neu berechnet.

Der *reject_sample_value*-Parameter muss eine ganze Zahl zwischen 0 und 2.147.483.647 sein.

Ist beispielsweise REJECT_SAMPLE_VALUE = 1000, dann berechnet PolyBase den Prozentsatz von fehlerhaften Zeilen nach dem Importversuch von 1000 Zeilen aus der externen Datendatei. Ist der Prozentsatz von fehlerhaften Zeilen kleiner als *reject_value*, führt PolyBase einen erneuten Abrufversuch von 1000 Zeilen aus. Nach jedem weiteren Importversuch von 1000 Zeilen wird der Prozentsatz von fehlerhaften Zeilen weiterhin neu berechnet.

> [!NOTE]
> Da die Berechnung des Prozentsatzes von fehlerhaften Zeilen durch PolyBase in Intervallen erfolgt, kann der tatsächliche Prozentsatz fehlerhafter Zeilen *reject_value* überschreiten.

Beispiel:

In diesem Beispiel wird verdeutlicht, wie die drei REJECT-Optionen interagieren. Gilt beispielsweise REJECT_TYPE = Prozentsatz, REJECT_VALUE = 30 und REJECT_SAMPLE_VALUE = 100, dann könnte das folgende Szenario auftreten:

- PolyBase versucht, die ersten 100 Zeilen abzurufen. Davon sind 25 fehlerhaft und 75 erfolgreich.
- Der berechnete Prozentsatz fehlerhafter Zeilen ist mit 25 % kleiner als der REJECT-Wert von 30 %. Aus diesem Grund wird PolyBase weiterhin versuchen, Daten aus der externen Datenquelle abzurufen.
- PolyBase versucht, die nächsten 100 Zeilen zu laden. Dieses Mal sind 25 Zeilen erfolgreich und 75 Zeilen fehlerhaft.
- Der Prozentsatz fehlerhafter Zeilen wird mit 50 % neu berechnet. Der Prozentsatz fehlerhafter Zeilen hat den REJECT-Wert von 30 % überschritten.
- Die PolyBase-Abfrage schlägt fehl, da nach der Rückgabe der ersten 200 Zeilen 50 % der Zeilen abgelehnt werden. Beachten Sie, dass übereinstimmende Zeilen zurückgegeben wurden, bevor die PolyBase-Abfrage erkennt, dass der Schwellenwert zum Zurückweisen überschritten wurde.

REJECTED_ROW_LOCATION = *Verzeichnis*

Gibt das Verzeichnis in der externen Datenquelle an, in das die abgelehnten Zeilen und die entsprechende Fehlerdatei geschrieben werden sollen.
Ist das angegebene Verzeichnis nicht vorhanden, wird es von PolyBase für Sie erstellt. Es wird ein untergeordnetes Verzeichnis mit dem Namen „\_rejectedrows“ erstellt. Mit dem „\_“-Zeichen wird sichergestellt, dass das Verzeichnis für andere Datenverarbeitungsvorgänge übergangen wird, es sei denn, es ist explizit im LOCATION-Parameter angegeben. In diesem Verzeichnis befindet sich ein Ordner, der ausgehend von der Zeit der Lastübermittlung im Format „JahrMonatTag-StundeMinuteSekunde“ erstellt wurde (z.B. 20180330-173205). In diesen Ordner werden zwei Arten von Dateien geschrieben: die Ursachendatei (_reason-Datei) und die Datendatei.

Sowohl die Ursachendateien als auch die Datendateien haben die „queryID“, die der CTAS-Anweisung zugeordnet ist. Da die Daten und die Ursachen in getrennten Dateien gespeichert sind, haben die zugehörigen Dateien ein entsprechendes Suffix.

## <a name="permissions"></a>Berechtigungen

Folgende Benutzerberechtigungen sind erforderlich:

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**

> [!NOTE]
> CONTROL DATABASE-Berechtigungen sind nur zum Erstellen von MASTER KEY, DATABASE SCOPED CREDENTIAL und EXTERNAL DATA SOURCE erforderlich.

Beachten Sie, dass die Anmeldung, die die externe Datenquelle erstellt, die Berechtigung zum Lesen und Schreiben in der externen Datenquelle, die in Hadoop oder Azure Blob Storage gespeichert ist, benötigt.

> [!IMPORTANT]
> Mit der Berechtigung ALTER ANY EXTERNAL DATA SOURCE besitzt jeder Prinzipal die Fähigkeit, beliebige externe Datenquellenobjekte zu erstellen und zu ändern. Damit ist auch der Zugriff auf alle datenbankweit gültigen Anmeldeinformationen der Datenbank möglich. Da es sich hierbei um eine weitreichende Berechtigung handelt, darf sie nur vertrauenswürdigen Prinzipalen innerhalb des Systems erteilt werden.

## <a name="error-handling"></a>Fehlerbehandlung

Beim Ausführen der CREATE EXTERNAL TABLE-Anweisung versucht PolyBase, eine Verbindung mit der externen Datenquelle herzustellen. Tritt bei der Verbindung ein Fehler auf, schlägt die Anweisung fehl. Die externe Tabelle wird nicht erstellt. Da PolyBase erneut versucht, die Verbindung aufzubauen, bevor die Abfrage endgültig fehlschlägt, kann es eine Minute oder länger dauern, bis der Befehl fehlschlägt.

## <a name="general-remarks"></a>Allgemeine Hinweise

PolyBase speichert die aus der externen Datenquelle abgerufenen Zeilen in Szenarios mit Ad-hoc-Abfragen, z.B. bei SELECT FROM EXTERNAL TABLE, in einer temporären Tabelle. Nachdem die Abfrage abgeschlossen ist, entfernt und löscht PolyBase die temporäre Tabelle. Es werden keine permanenten Daten in SQL-Tabellen gespeichert.

Im Gegensatz dazu speichert PolyBase die aus der externen Datenquelle abgerufenen Zeilen in Importszenarios, z.B. bei SELECT INTO FROM EXTERNAL TABLE, permanent in einer SQL-Tabelle. Die neue Tabelle wird beim Ausführen der Abfrage erstellt, wenn PolyBase die externen Daten abruft.

PolyBase kann einen Teil der Abfrageberechnung an Hadoop übertragen, um die Abfrageleistung zu verbessern. Diese Aktion wird als Prädikatweitergabe bezeichnet. Um sie zu aktivieren, geben Sie die Option „Resource Manager Location“ von Hadoop in [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) an.

Sie können zahlreiche externe Tabellen erstellen, die auf die gleichen oder andere externe Datenquellen verweisen.

## <a name="limitations-and-restrictions"></a>Einschränkungen

Da die Verwaltung der Daten für eine externe Tabelle nicht direkt in Azure Synapse gesteuert werden kann, können die Daten jederzeit von einem externen Prozess geändert oder gelöscht werden. Aus diesem Grund sind Abfrageergebnisse für eine externe Tabelle nicht garantiert deterministisch. Die gleiche Abfrage kann bei jeder Ausführung für eine externe Tabelle unterschiedliche Ergebnisse zurückgeben. Auf ähnliche Weise kann eine Abfrage fehlschlagen, wenn die externen Daten verschoben oder entfernt werden.

Sie können zahlreiche externe Tabellen erstellen, die alle auf unterschiedliche externe Datenquellen verweisen.

Nur diese DDL-Anweisungen (Data Definition Language) sind in externen Tabellen zulässig:

- CREATE TABLE und DROP TABLE
- CREATE STATISTICS und DROP STATISTICS
- CREATE VIEW und DROP VIEW

Nicht unterstützte Konstruktionen und Operationen:

- Die DEFAULT-Einschränkung auf externen Tabellenspalten
- DML-Vorgänge (Data Manipulation Language): DELETE, INSERT und UPDATE

### <a name="query-limitations"></a>Abfrageeinschränkungen

Es wird davon abgeraten, mehr als 30.000 Dateien pro Ordner zu überschreiten. Wenn auf zu viele Dateien verwiesen wird, kann eine Ausnahme aufgrund ungenügenden Arbeitsspeichers von Java Virtual Machine (JVM) auftreten oder die Leistung kann beeinträchtigt werden.

### <a name="table-width-limitations"></a>Einschränkungen der Tabellenbreite

PolyBase in Azure Data Warehouse verfügt über eine Begrenzung für die Zeilenbreite von 1 MB, basierend auf der Maximalgröße einer einzelnen gültigen Zeile je Tabellendefinition. Wenn die Summe des Spaltenschemas größer als 1 MB ist, kann PolyBase die Daten nicht abfragen.

### <a name="data-type-limitations"></a>Einschränkungen für Datentypen

Die folgenden Datentypen können nicht in externen PolyBase-Tabellen verwendet werden:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Jeder benutzerdefinierte Typ

## <a name="locking"></a>Sperren

Freigegebene Sperre für das SCHEMARESOLUTION-Objekt.

## <a name="examples"></a>Beispiele

### <a name="a-importing-data-from-adls-gen-2-into-azure-ssdw"></a>A. Importieren von Daten aus ADLS Gen 2 in Azure [!INCLUDE[ssDW](../../includes/ssdw-md.md)]. 

Beispiele für ADLS Gen 1 finden Sie unter [Erstellen einer externen Datenquelle](create-external-data-source-transact-sql.md).

```sql

-- These values come from your Azure Active Directory Application used to authenticate to ADLS Gen 2. 
CREATE DATABASE SCOPED CREDENTIAL ADLUser
WITH IDENTITY = '<clientID>@\<OAuth2.0TokenEndPoint>',
SECRET = '<KEY>' ;

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (TYPE = HADOOP,
      LOCATION = 'abfss://data@pbasetr.azuredatalakestore.net'
)

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(
    FORMAT_TYPE = DELIMITEDTEXT
    , FORMAT_OPTIONS ( FIELD_TERMINATOR = '|'
       , STRING_DELIMITER = ''
      , DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fff'
      , USE_TYPE_DEFAULT = FALSE
      )
)

CREATE EXTERNAL TABLE [dbo].[DimProductexternal]
( [ProductKey] [int] NOT NULL,
  [ProductLabel] nvarchar NULL,
  [ProductName] nvarchar NULL )
WITH
(
    LOCATION='/DimProduct/' ,
    DATA_SOURCE = AzureDataLakeStore ,
    FILE_FORMAT = TextFileFormat ,
    REJECT_TYPE = VALUE ,
    REJECT_VALUE = 0
) ;

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey] ) )
AS SELECT * FROM
[dbo].[DimProduct_external] ;
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-analytics-platform-system"></a>Übersicht: Analyseplattformsystem

Verwenden Sie eine externe Tabelle, um:

- Daten in Hadoop oder in Azure Blob Storage mit [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen abzufragen.
- Daten aus Hadoop oder Azure Blob Storage in Analytics Platform System zu importieren und speichern.

Siehe auch [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) und [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).

## <a name="syntax"></a>Syntax

```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH (
        LOCATION = 'hdfs_folder_or_filepath',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]

<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage,  
    | REJECT_VALUE = reject_value,  
    | REJECT_SAMPLE_VALUE = reject_sample_value,
  
}  
```

## <a name="arguments"></a>Argumente

*{ database_name.schema_name.table_name | schema_name.table_name | table_name }* : Der ein- bis dreiteilige Name der zu erstellenden Tabelle. Für eine externe Tabelle speichert Analytics Platform System nur die Metadaten der Tabelle mit den grundlegenden Statistiken über die Datei oder den Ordner, auf die in Hadoop oder Azure Blob Storage verwiesen wird. Es werden keine tatsächlichen Daten verschoben oder in Analytics Platform System gespeichert.

> [!IMPORTANT]
> Wenn der externe Datenquellentreiber einen dreiteiligen Namen unterstützt, wird dringend empfohlen, diesen dreiteiligen Namen anzugeben, um eine optimale Leistung zu erzielen.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE unterstützt das Konfigurieren von Spaltenname, Datentyp, NULL-Zulässigkeit und Sortierung. Sie können DEFAULT CONSTRAINT nicht für externe Tabellen verwenden.

Die Spaltendefinitionen, einschließlich der Datentypen und der Anzahl der Spalten, müssen mit den Daten in den externen Dateien übereinstimmen. Wenn ein Konflikt besteht, werden die Zeilen der Datei beim Abfragen der tatsächlichen Daten zurückgewiesen.

LOCATION = '*folder_or_filepath*': Gibt den Ordner oder den Dateipfad und Dateinamen für die tatsächlichen Daten in Hadoop oder Azure Blob Storage an. Der Speicherort beginnt im Stammordner. Der Stammordner ist der in der externen Datenquelle angegebene Datenspeicherort.

In Analytics Platform System erstellt die Anweisung [CREATE EXTERNAL TABLE AS SELECT](create-external-table-as-select-transact-sql.md) den Pfad und den Ordner, wenn diese noch nicht vorhanden sind. `CREATE EXTERNAL TABLE` erstellt den Pfad und den Ordner nicht.

Wenn LOCATION als Ordner angegeben wird, ruft eine PolyBase-Abfrage, die aus der externen Tabelle auswählt, Dateien aus dem Ordner und allen Unterordnern ab. PolyBase gibt wie Hadoop keine ausgeblendeten Ordner zurück. Es werden auch keine Dateien zurückgegeben, deren Dateiname mit einem Unterstrich (_) oder einem Punkt (.) beginnt.

In diesem Beispiel gibt eine PolyBase-Abfrage Zeilen aus „mydata.txt“ und „mydata2.txt“ zurück, wenn LOCATION='/webdata/'. „Mydata3.txt“ wird nicht zurückgegeben, da es sich um einen Unterordner eines ausgeblendeten Ordners handelt. Es werden auch keine „_hidden.txt“-Dateien zurückgegeben, da es sich um eine ausgeblendete Datei handelt.

![Rekursive Daten für externe Tabellen](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Rekursive Daten für externe Tabellen")

Legen Sie das Attribut \<polybase.recursive.traversal> in der Konfigurationsdatei „core-site.xml“ auf FALSE fest, um den Standardordner zu ändern, und nur aus dem Stammordner zu lesen. Diese Datei befindet sich unter `<SqlBinRoot>\PolyBase\Hadoop\Conf with SqlBinRoot the bin root of SQl Server`. Beispiel: `C:\\Program Files\\Microsoft SQL Server\\MSSQL13.XD14\\MSSQL\\Binn`.

DATA_SOURCE = *external_data_source_name*: Gibt den Namen der externen Datenquelle an, die den Speicherort der externen Daten enthält. Dieser Speicherort ist entweder eine Hadoop oder ein Azure Blob Storage. Verwenden Sie zum Erstellen einer externen Datenquelle [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *external_file_format_name*: Gibt den Namen des externen Dateiformatobjekts an, das den Dateityp und die Komprimierungsmethode für die externen Daten enthält. Verwenden Sie zum Erstellen eines externen Dateiformats [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Reject-Optionen: Sie können Reject-Parameter angeben, die bestimmen, wie PolyBase *modifizierte* Datensätze behandelt, die aus der externen Datenquelle abgerufen werden. Ein Datensatz gilt als „dirty“ (modifiziert), wenn die tatsächlichen Datentypen oder die Anzahl der Spalten nicht den Spaltendefinitionen der externen Tabelle entsprechen.

Wenn Sie die Reject-Werte nicht angeben oder ändern, verwendet PolyBase Standardwerte. Diese Informationen über die Reject-Parameter werden als zusätzliche Metadaten gespeichert, wenn Sie eine externe Tabelle mit der CREATE EXTERNAL TABLE-Anweisung erstellen. Wenn eine zukünftige SELECT- oder SELECT INTO SELECT-Anweisung Daten aus der externen Tabelle auswählt, wird PolyBase die Reject-Optionen verwenden, um die Anzahl oder den Prozentsatz der Zeilen zu bestimmen, die zurückgewiesen werden können, bevor die tatsächliche Abfrage fehlschlägt. Die Abfrage gibt (Teil-) Ergebnisse zurück, bis der Reject-Schwellenwert überschritten wird. Daraufhin wird eine entsprechende Fehlermeldung ausgelöst.

REJECT_TYPE = **value** | percentage: Gibt an, ob die Option „REJECT_VALUE“ als Literalwert oder als Prozentsatz angegeben wird.

value: REJECT_VALUE ist ein Literalwert und kein Prozentsatz. Die PolyBase-Abfrage schlägt fehl, wenn die Anzahl der abgelehnten Zeilen *reject_value* überschreitet.

Die SELECT-Abfrage von PolyBase schlägt beispielsweise bei „REJECT_VALUE = 5“ und „REJECT_TYPE = value“ fehl, nachdem fünf Zeilen abgelehnt wurden.

percentage: REJECT_VALUE ist ein Prozentsatz und kein Literalwert. Eine PolyBase-Abfrage schlägt fehl, wenn der *Prozentsatz* fehlerhafter Zeilen *reject_value* überschreitet. Der Prozentsatz der fehlerhaften Zeilen wird in Intervallen berechnet.

REJECT_VALUE = *reject_value*: Gibt den Wert oder den Prozentsatz der Zeilen an, die zurückgewiesen werden können, bevor die Abfrage fehlschlägt.

Wenn REJECT_TYPE = Wert, muss *reject_value* eine ganze Zahl zwischen 0 und 2.147.483.647 sein.

Wenn REJECT_TYPE = Prozentzahl, muss *reject_value* eine Gleitkommazahl zwischen 0 und 100 sein.

REJECT_SAMPLE_VALUE = *reject_sample_value*: Dieses Attribut ist erforderlich, wenn Sie REJECT_TYPE = Prozentsatz angeben. Bestimmt die Anzahl der Zeilen, bei denen versucht wird, sie abzurufen, bevor die PolyBase den Prozentsatz der abgelehnten Zeilen neu berechnet.

Der *reject_sample_value*-Parameter muss eine ganze Zahl zwischen 0 und 2.147.483.647 sein.

Ist beispielsweise REJECT_SAMPLE_VALUE = 1000, dann berechnet PolyBase den Prozentsatz von fehlerhaften Zeilen nach dem Importversuch von 1000 Zeilen aus der externen Datendatei. Ist der Prozentsatz von fehlerhaften Zeilen kleiner als *reject_value*, führt PolyBase einen erneuten Abrufversuch von 1000 Zeilen aus. Nach jedem weiteren Importversuch von 1000 Zeilen wird der Prozentsatz von fehlerhaften Zeilen weiterhin neu berechnet.

> [!NOTE]
> Da die Berechnung des Prozentsatzes von fehlerhaften Zeilen durch PolyBase in Intervallen erfolgt, kann der tatsächliche Prozentsatz fehlerhafter Zeilen *reject_value* überschreiten.

Beispiel:

In diesem Beispiel wird verdeutlicht, wie die drei REJECT-Optionen interagieren. Gilt beispielsweise REJECT_TYPE = Prozentsatz, REJECT_VALUE = 30 und REJECT_SAMPLE_VALUE = 100, dann könnte das folgende Szenario auftreten:

- PolyBase versucht, die ersten 100 Zeilen abzurufen. Davon sind 25 fehlerhaft und 75 erfolgreich.
- Der berechnete Prozentsatz fehlerhafter Zeilen ist mit 25 % kleiner als der REJECT-Wert von 30 %. Aus diesem Grund wird PolyBase weiterhin versuchen, Daten aus der externen Datenquelle abzurufen.
- PolyBase versucht, die nächsten 100 Zeilen zu laden. Dieses Mal sind 25 Zeilen erfolgreich und 75 Zeilen fehlerhaft.
- Der Prozentsatz fehlerhafter Zeilen wird mit 50 % neu berechnet. Der Prozentsatz fehlerhafter Zeilen hat den REJECT-Wert von 30 % überschritten.
- Die PolyBase-Abfrage schlägt fehl, da nach der Rückgabe der ersten 200 Zeilen 50 % der Zeilen abgelehnt werden. Beachten Sie, dass übereinstimmende Zeilen zurückgegeben wurden, bevor die PolyBase-Abfrage erkennt, dass der Schwellenwert zum Zurückweisen überschritten wurde.

## <a name="permissions"></a>Berechtigungen

Folgende Benutzerberechtigungen sind erforderlich:

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**
- **CONTROL DATABASE**

Beachten Sie, dass die Anmeldung, die die externe Datenquelle erstellt, die Berechtigung zum Lesen und Schreiben in der externen Datenquelle, die in Hadoop oder Azure Blob Storage gespeichert ist, benötigt.

> [!IMPORTANT]
> Mit der Berechtigung ALTER ANY EXTERNAL DATA SOURCE besitzt jeder Prinzipal die Fähigkeit, beliebige externe Datenquellenobjekte zu erstellen und zu ändern. Damit ist auch der Zugriff auf alle datenbankweit gültigen Anmeldeinformationen der Datenbank möglich. Da es sich hierbei um eine weitreichende Berechtigung handelt, darf sie nur vertrauenswürdigen Prinzipalen innerhalb des Systems erteilt werden.

## <a name="error-handling"></a>Fehlerbehandlung

Beim Ausführen der CREATE EXTERNAL TABLE-Anweisung versucht PolyBase, eine Verbindung mit der externen Datenquelle herzustellen. Tritt bei der Verbindung ein Fehler auf, schlägt die Anweisung fehl. Die externe Tabelle wird nicht erstellt. Da PolyBase erneut versucht, die Verbindung aufzubauen, bevor die Abfrage endgültig fehlschlägt, kann es eine Minute oder länger dauern, bis der Befehl fehlschlägt.

## <a name="general-remarks"></a>Allgemeine Hinweise

PolyBase speichert die aus der externen Datenquelle abgerufenen Zeilen in Szenarios mit Ad-hoc-Abfragen, z.B. bei SELECT FROM EXTERNAL TABLE, in einer temporären Tabelle. Nachdem die Abfrage abgeschlossen ist, entfernt und löscht PolyBase die temporäre Tabelle. Es werden keine permanenten Daten in SQL-Tabellen gespeichert.

Im Gegensatz dazu speichert PolyBase die aus der externen Datenquelle abgerufenen Zeilen in Importszenarios, z.B. bei SELECT INTO FROM EXTERNAL TABLE, permanent in einer SQL-Tabelle. Die neue Tabelle wird beim Ausführen der Abfrage erstellt, wenn PolyBase die externen Daten abruft.

PolyBase kann einen Teil der Abfrageberechnung an Hadoop übertragen, um die Abfrageleistung zu verbessern. Diese Aktion wird als Prädikatweitergabe bezeichnet. Um sie zu aktivieren, geben Sie die Option „Resource Manager Location“ von Hadoop in [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) an.

Sie können zahlreiche externe Tabellen erstellen, die auf die gleichen oder andere externe Datenquellen verweisen.

## <a name="limitations-and-restrictions"></a>Einschränkungen

Da die Verwaltung der Daten für eine externe Tabelle nicht direkt in der Appliance gesteuert werden kann, können die Daten jederzeit von einem externen Prozess geändert oder gelöscht werden. Aus diesem Grund sind Abfrageergebnisse für eine externe Tabelle nicht garantiert deterministisch. Die gleiche Abfrage kann bei jeder Ausführung für eine externe Tabelle unterschiedliche Ergebnisse zurückgeben. Auf ähnliche Weise kann eine Abfrage fehlschlagen, wenn die externen Daten verschoben oder entfernt werden.

Sie können zahlreiche externe Tabellen erstellen, die alle auf unterschiedliche externe Datenquellen verweisen. Wenn Sie Abfragen für verschiedene Hadoop-Datenquellen gleichzeitig ausführen, muss jede Hadoop-Datenquelle die gleiche „Hadoop Connectivity“-Serverkonfigurationseinstellung verwenden. Beispielsweise können Sie nicht gleichzeitig eine Abfrage für einen Cloudera Hadoop-Cluster und einen Hortonworks Hadoop-Cluster ausführen, da diese unterschiedliche Konfigurationseinstellungen verwenden. Weitere Informationen zu den Konfigurationseinstellungen und den unterstützten Kombinationen finden Sie unter [Konfiguration der PolyBase-Netzwerkkonnektivität](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).

Nur diese DDL-Anweisungen (Data Definition Language) sind in externen Tabellen zulässig:

- CREATE TABLE und DROP TABLE
- CREATE STATISTICS und DROP STATISTICS
- CREATE VIEW und DROP VIEW

Nicht unterstützte Konstruktionen und Operationen:

- Die DEFAULT-Einschränkung auf externen Tabellenspalten
- DML-Vorgänge (Data Manipulation Language): DELETE, INSERT und UPDATE

### <a name="query-limitations"></a>Abfrageeinschränkungen

PolyBase kann bei 32 gleichzeitigen PolyBase-Abfragen maximal 33.000 Dateien pro Ordner verarbeiten. Diese maximale Anzahl schließt sowohl Dateien als auch Unterordner im jeweiligen HDFS-Ordner ein. Werden weniger als 32 Abfragen gleichzeitig ausgeführt, können auch PolyBase-Abfragen für Ordner in HDFS ausgeführt werden, die mehr als 33.000 Dateien enthalten. Es wird empfohlen, dass Sie externe Dateipfade kurz halten und nicht mehr als 30.000 Dateien pro HDFS-Ordner verwenden. Wenn auf zu viele Dateien verwiesen wird, kann eine Out-of-Memory-Ausnahme von Java Virtual Machine (JVM) auftreten.

### <a name="table-width-limitations"></a>Einschränkungen der Tabellenbreite

PolyBase in SQL Server 2016 verfügt über eine Begrenzung für die Zeilenbreite von 32 KB, basierend auf der Maximalgröße einer einzelnen gültigen Zeile je Tabellendefinition. Wenn die Summe des Spaltenschemas größer als 32 KB ist, kann PolyBase die Daten nicht abfragen.

In Azure Synapse Analytics wurde diese Begrenzung auf 1 MB erhöht.

### <a name="data-type-limitations"></a>Einschränkungen für Datentypen

Die folgenden Datentypen können nicht in externen PolyBase-Tabellen verwendet werden:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Jeder benutzerdefinierte Typ

## <a name="locking"></a>Sperren

Freigegebene Sperre für das SCHEMARESOLUTION-Objekt.

## <a name="security"></a>Sicherheit

Die Datendateien für eine externe Tabelle werden in Hadoop oder Azure Blob Storage gespeichert. Diese Datendateien werden von Ihrem eigenen Prozess erstellt und verwaltet. Die Sicherheit der externen Daten liegt in Ihrer Verantwortung.

## <a name="examples"></a>Beispiele

### <a name="a-join-hdfs-data-with-analytics-platform-system-data"></a>A. Verknüpfen von HDFS-Daten mit Analytics Platform System-Daten

```sql
SELECT cs.user_ip FROM ClickStream cs
JOIN User u ON cs.user_ip = u.user_ip
WHERE cs.url = 'www.microsoft.com'
;
```

### <a name="b-import-row-data-from-hdfs-into-a-distributed-analytics-platform-system-table"></a>B. Importieren von Zeilendaten aus HDFS in eine verteilte Analytics Platform System-Tabelle

```sql
CREATE TABLE ClickStream_PDW
WITH ( DISTRIBUTION = HASH (url) )
AS SELECT url, event_date, user_ip FROM ClickStream
;
```

### <a name="c-import-row-data-from-hdfs-into-a-replicated-analytics-platform-system-table"></a>C. Importieren von Zeilendaten aus HDFS in eine replizierte Analytics Platform System-Tabelle

```sql
CREATE TABLE ClickStream_PDW
WITH ( DISTRIBUTION = REPLICATE )
AS SELECT url, event_date, user_ip
FROM ClickStream
;
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)

::: moniker-end
