---
title: BULK INSERT (Transact-SQL) | Microsoft-Dokumentation
description: Transact-SQL-Referenz für die BULK INSERT-Anweisung
ms.date: 02/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BULK_TSQL
- BULK_INSERT
- BULK_INSERT_TSQL
- BULK INSERT
dev_langs:
- TSQL
helpviewer_keywords:
- tables [SQL Server], importing data into
- inserting files
- views [SQL Server], importing data into
- BULK INSERT statement
- views [SQL Server], exporting data from
- importing data, bulk import
- bulk importing [SQL Server], BULK INSERT statement
- file importing [SQL Server]
ms.assetid: be3984e1-5ab3-4226-a539-a9f58e1e01e2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1d1659f427386fd8fa807bc10e782eca572a2278
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100342549"
---
# <a name="bulk-insert-transact-sql"></a>BULK INSERT (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Importiert eine Datendatei in eine Datenbanktabelle oder Sicht und verwendet dabei ein vom Benutzer angegebenes Format in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql
BULK INSERT
   { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }
      FROM 'data_file'
     [ WITH
    (
   [ [ , ] BATCHSIZE = batch_size ]
   [ [ , ] CHECK_CONSTRAINTS ]
   [ [ , ] CODEPAGE = { 'ACP' | 'OEM' | 'RAW' | 'code_page' } ]
   [ [ , ] DATAFILETYPE =
      { 'char' | 'native'| 'widechar' | 'widenative' } ]
   [ [ , ] DATA_SOURCE = 'data_source_name' ]
   [ [ , ] ERRORFILE = 'file_name' ]
   [ [ , ] ERRORFILE_DATA_SOURCE = 'data_source_name' ]
   [ [ , ] FIRSTROW = first_row ]
   [ [ , ] FIRE_TRIGGERS ]
   [ [ , ] FORMATFILE_DATA_SOURCE = 'data_source_name' ]
   [ [ , ] KEEPIDENTITY ]
   [ [ , ] KEEPNULLS ]
   [ [ , ] KILOBYTES_PER_BATCH = kilobytes_per_batch ]
   [ [ , ] LASTROW = last_row ]
   [ [ , ] MAXERRORS = max_errors ]
   [ [ , ] ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) ]
   [ [ , ] ROWS_PER_BATCH = rows_per_batch ]
   [ [ , ] ROWTERMINATOR = 'row_terminator' ]
   [ [ , ] TABLOCK ]

   -- input file format options
   [ [ , ] FORMAT = 'CSV' ]
   [ [ , ] FIELDQUOTE = 'quote_characters']
   [ [ , ] FORMATFILE = 'format_file_path' ]
   [ [ , ] FIELDTERMINATOR = 'field_terminator' ]
   [ [ , ] ROWTERMINATOR = 'row_terminator' ]
    )]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

*database_name:* der Name der Datenbank, in der sich die angegebene Tabelle oder Ansicht befindet. Fehlt die Angabe, ist dies die aktuelle Datenbank.

*schema_name:* der Name der Tabelle oder des Ansichtsschemas. *schema_name* ist optional, wenn das Standardschema des Benutzers, der den Massenimportvorgang ausführt, das Schema der angegebenen Tabelle oder Sicht ist. Wenn *schema* nicht angegeben wird und es sich bei dem Standardschema des Benutzers, der den Massenimportvorgang ausführt, nicht um das Schema der angegebenen Tabelle oder Sicht handelt, wird in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine Fehlermeldung zurückgegeben, und der Massenimportvorgang wird abgebrochen.

*table_name:* der Name der Tabelle oder Ansicht, in die der Massenimport von Daten erfolgen soll. Es können nur Sichten verwendet werden, deren Spalten alle auf dieselbe Basistabelle verweisen. Weitere Informationen über die Einschränkungen beim Laden von Daten in Sichten finden Sie unter [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md).

**'** _data_file_ **'** : Dies ist der vollständige Pfad der Datendatei mit den Daten, die in die angegebene Tabelle oder Ansicht importiert werden sollen. Mithilfe von BULK INSERT können Daten von einem Datenträger oder aus Azure Blob Storage importiert werden, einschließlich eines Netzwerks, einer Diskette, einer Festplatte usw.

Für *data_file* muss ein gültiger Pfad auf dem Server, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird, angegeben werden. Wenn *data_file* eine Remotedatei ist, geben Sie den UNC-Namen (Universal Naming Convention) an. Ein UNC-Name weist das Format \\\\*Systemname*\\*ShareName*\\*Path*\\*FileName*. Beispiel:

```sql
BULK INSERT Sales.Orders
FROM '\\SystemX\DiskZ\Sales\data\orders.dat';
```

**Anwendungsbereich:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 und Azure SQL-Datenbank.
Ab [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 kann sich data_file in Azure Blob Storage befinden. In diesem Fall müssen Sie die Option **data_source_name** angeben. Ein Beispiel dafür finden Sie unter [Importieren von Daten aus einer Datei in Azure Blob Storage](#f-importing-data-from-a-file-in-azure-blob-storage).

> [!IMPORTANT]
> Azure SQL-Datenbank unterstützt nur das Lesen aus Azure Blob Storage.

**'** _data_source_name_ **'** 
**Gilt für:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 und Azure SQL-Datenbank.
Es handelt sich um eine benannte externe Datenquelle, die auf den Azure Blob-Speicherort der Datei verweist, welche importiert wird. Die externe Datenquelle muss mithilfe der in [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 hinzugefügten `TYPE = BLOB_STORAGE`-Option erstellt werden. Weitere Informationen finden Sie unter [CREATE EXTERNAL DATA SOURCE (CREATE EXTERNAL DATA SOURCE)](../../t-sql/statements/create-external-data-source-transact-sql.md). Ein Beispiel dafür finden Sie unter [Importieren von Daten aus einer Datei in Azure Blob Storage](#f-importing-data-from-a-file-in-azure-blob-storage).

BATCHSIZE **=** _batch_size:_ gibt die Anzahl der Zeilen in einem Batch an. Jeder Batch wird als eine Transaktion auf den Server kopiert. Falls ein Fehler erzeugt wird, führt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für die Transaktion jedes Batches ein Commit oder Rollback aus. In der Standardeinstellung werden alle Daten, die sich in der angegebenen Datendatei befinden, als ein Batch behandelt. Informationen zu Leistungsaspekten finden Sie unter "Hinweise" weiter unten in diesem Thema.

CHECK_CONSTRAINTS: gibt an, dass alle Einschränkungen, die für die Zieltabelle oder -ansicht gelten, während des Massenimportvorgangs überprüft werden müssen. Ohne die Option CHECK_CONSTRAINTS werden alle CHECK- und FOREIGN KEY-Einschränkungen ignoriert, und nach Abschluss des Vorgangs wird die Einschränkung in der Tabelle als nicht vertrauenswürdig gekennzeichnet.

> [!NOTE]
> UNIQUE- und PRIMARY KEY-Einschränkungen werden immer erzwungen. Beim Importieren in eine Zeichenspalte, die mit einer NOT NULL-Einschränkung definiert ist, fügt BULK INSERT eine leere Zeichenfolge ein, wenn die Textdatei keinen Wert enthält.

An gewissen Punkten müssen Sie die Einschränkungen in der gesamten Tabelle überprüfen. Wenn die Tabelle vor dem Massenimportvorgang nicht leer war, kann der Aufwand einer erneuten Überprüfung der Einschränkung höher sein als das Anwenden von CHECK-Einschränkungen auf die inkrementellen Daten.

Die Deaktivierung von Einschränkungen (das Standardverhalten) kann z. B. erwünscht sein, wenn die Eingabedaten Zeilen enthalten, die Einschränkungen verletzen. Wenn CHECK-Einschränkungen deaktiviert sind, können Sie die Daten importieren und dann [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen verwenden, um die ungültigen Daten zu entfernen.

> [!NOTE]
> Die Option MAXERRORS kann zur Einschränkungsüberprüfung nicht verwendet werden.

CODEPAGE **=** { **'** ACP **'** \| **'** OEM **'** \| **'** RAW **'** \| **'** _code_page_ **'** } gibt die Codepage für die in der Datendatei enthaltenen Daten an. CODEPAGE ist nur dann von Bedeutung, wenn die Daten **char**-, **varchar**- oder **text**-Spalten mit Zeichenwerten enthalten, die größer als **127** oder kleiner als **32** sind. Ein Beispiel finden Sie unter [Angeben einer Codepage](#d-specifying-a-code-page).

> [!IMPORTANT]
> Die Option CODEPAGE wird unter Linux für [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] nicht unterstützt. Für [!INCLUDE[ssSQLv15_md](../../includes/sssql19-md.md)] ist nur die Option **'RAW'** für CODEPAGE zulässig.

> [!NOTE]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] empfiehlt, für jede Spalte einen Sortierungsnamen in einer [Formatdatei](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md) anzugeben.

|CODEPAGE-Wert|BESCHREIBUNG|
|--------------------|-----------------|
|ACP|Spalten vom Datentyp **char**, **varchar** oder **text** werden von der [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]/[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Codepage (ISO 1252) in die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Codepage konvertiert.|
|OEM (Standard)|Spalten vom Datentyp **char**, **varchar** oder **text** werden von der OEM-Codepage des Systems in die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Codepage konvertiert.|
|RAW|Dies ist die schnellste Option, da keine Konvertierung von einer Codepage in eine andere erfolgt.|
|*Codepage*|Bestimmte Codepagenummer, z. B. 850.<br /><br /> **&#42;&#42; Wichtig &#42;&#42;** In Versionen vor [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] wird die Codepage 65001 (UTF-8-Codierung) nicht unterstützt.|
| &nbsp; | &nbsp; |

DATAFILETYPE **=** { **'char'** \| **'native'** \| **'widechar'** \| **'widenative'** } gibt an, dass BULK INSERT den Importvorgang mithilfe des angegebenen DATAFILETYPE-Werts ausführt.

&nbsp;

|DATAFILETYPE-Wert|Alle Daten, die dargestellt sind in:|
|------------------------|------------------------------|
|**char** (Standard)|Zeichenformat<br /><br /> Weitere Informationen finden Sie unter [Verwenden des Zeichenformats zum Importieren oder Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md).|
|**native**|Systemeigene (Datenbank-) Datentypen. Erstellen Sie die native Datendatei durch das Massenimportieren von Daten aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe des Hilfsprogramms **bcp**.<br /><br /> Der Wert native bietet eine höhere Leistung als der Wert char. Das native Format wird für die Massenübertragung von Daten zwischen mehreren Instanzen von SQL Server mithilfe einer Datendatei empfohlen, die keinen erweiterten Zeichensatz bzw. Doppelbyte-Zeichensatz (Double-Byte Character Set, DBCS) enthält.<br /><br /> Weitere Informationen finden Sie unter [Verwenden des nativen Formats zum Importieren oder Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md).|
|**widechar**|Unicode-Zeichen<br /><br /> Weitere Informationen finden Sie unter [Verwenden des Unicode-Zeichenformats zum Importieren und Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).|
|**widenative**|Native (Datenbank-) Datentypen, außer in **char**-, **varchar**- und **text**-Spalten, in denen Date als Unicode gespeichert werden. Erstellen Sie die Datendatei **widenative** durch das Massenimportieren von Daten aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe des Hilfsprogramms **bcp**.<br /><br /> Der Wert vom Datentyp **widenative** bietet eine höhere Leistung als der **widechar**-Wert. Falls die Datendatei erweiterte [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]-Zeichen enthält, geben Sie **widenative** an.<br /><br /> Weitere Informationen finden Sie unter [Verwenden des nativen Unicode-Formats zum Importieren oder Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).|
| &nbsp; | &nbsp; |

ERRORFILE **='** _file_name_ **':** gibt die Datei an, die zum Erfassen der Zeilen verwendet wird, die Formatierungsfehler enthalten und nicht in ein OLE DB-Rowset konvertiert werden können. Diese Zeilen werden aus der Datendatei unverändert in diese Fehlerdatei kopiert.

Die Fehlerdatei wird bei Ausführung des Befehls erstellt. Falls die Datei bereits vorhanden ist, tritt ein Fehler auf. Darüber hinaus wird eine Kontrolldatei mit der Erweiterung .ERROR.txt erstellt. Diese Datei enthält einen Verweis auf jede Zeile in der Fehlerdatei und stellt eine Fehlerdiagnose bereit. Sobald die Fehler korrigiert wurden, können die Daten geladen werden.
**Anwendungsbereich:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Ab [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] kann sich `error_file_path` im Azure Blob Storage befinden.

'errorfile_data_source_name' **Gilt für:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Es handelt sich um eine benannte externe Datenquelle, die auf den Azure Blob-Speicherort der Fehlerdatei verweist, welche Fehler enthält, die während des Importierens gefunden wurden. Die externe Datenquelle muss mithilfe der in [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 hinzugefügten `TYPE = BLOB_STORAGE`-Option erstellt werden. Weitere Informationen finden Sie unter [CREATE EXTERNAL DATA SOURCE (CREATE EXTERNAL DATA SOURCE)](../../t-sql/statements/create-external-data-source-transact-sql.md).

FIRSTROW **=** _first_row_: Hiermit wird die Nummer der ersten zu ladenden Zeile angegeben. In der Standardeinstellung ist dies die erste Zeile in der angegebenen Datendatei. FIRSTROW ist einsbasiert.

> [!NOTE]
> Es ist nicht vorgesehen, dass das FIRSTROW-Attribut Spaltenheader überspringt. Header zu überspringen wird von der BULK INSERT-Anweisung nicht unterstützt. Wenn Zeilen ausgelassen werden, werden von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] nur die Feldabschlusszeichen berücksichtigt und die Daten in den Feldern von ausgelassenen Zeilen nicht überprüft.

FIRE_TRIGGERS: gibt an, dass INSERT-Trigger, die für die Zieltabelle definiert sind, während des Massenimportvorgangs ausgeführt werden. Falls Trigger für INSERT-Vorgänge in der Zieltabelle definiert sind, werden sie für jeden abgeschlossenen Batch ausgelöst.

Wenn FIRE_TRIGGERS nicht angegeben ist, werden keine INSERT-Trigger ausgeführt.

FORMATFILE_DATA_SOURCE **=** 'Datenquellenname' **Gilt für:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] 1.1.
Es handelt sich um eine benannte externe Datenquelle, die auf den Azure Blob-Speicherort der Formatatei verweist, welche das Schema der importierten Daten definiert. Die externe Datenquelle muss mithilfe der in [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 hinzugefügten `TYPE = BLOB_STORAGE`-Option erstellt werden. Weitere Informationen finden Sie unter [CREATE EXTERNAL DATA SOURCE (CREATE EXTERNAL DATA SOURCE)](../../t-sql/statements/create-external-data-source-transact-sql.md).

KEEPIDENTITY: gibt an, dass Identitätswerte in der importierten Datendatei für die Identitätsspalte verwendet werden sollen. Wird KEEPIDENTITY nicht angegeben, werden die Identitätswerte für diese Spalte zwar überprüft, jedoch nicht importiert. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] weist dann auf der Basis von Ausgangswerten und inkrementellen Werten, die beim Erstellen der Tabelle angegeben wurden, eindeutige Werte zu. Wenn die Datendatei keine Werte für die Identitätsspalte in der Tabelle oder Sicht enthält, geben Sie mithilfe einer Formatdatei an, dass die Identitätsspalte der Tabelle oder Sicht beim Importieren von Daten ausgelassen werden soll. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] weist der Spalte automatisch eindeutige Werte zu. Weitere Informationen finden Sie unter [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md).

Weitere Informationen über das Beibehalten von Identitätswerten finden Sie unter [Beibehalten von Identitätswerten beim Massenimport von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md).

KEEPNULLS: gibt an, dass in leere Spalten während des Massenimportvorgangs keine Standardwerte eingefügt werden sollen, sondern ein NULL-Wert für diese Spalten beibehalten werden soll. Weitere Informationen finden Sie unter [Beibehalten von NULL-Werten oder Verwenden von Standardwerten während des Massenimports &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md).

KILOBYTES_PER_BATCH **=** _kilobytes_per_batch_: Hiermit wird die ungefähre Datenmenge pro Batch in KB als *kilobytes_per_batch* angegeben. In der Standardeinstellung ist KILOBYTES_PER_BATCH unbekannt. Informationen zu Leistungsaspekten finden Sie unter "Hinweise" weiter unten in diesem Thema.

LASTROW **=** _last_row_: Hiermit wird die Nummer der letzten zu ladenden Zeile angegeben. Der Standardwert ist 0, wodurch die Daten bis zur letzten Zeile in der angegebenen Datendatei geladen werden.

MAXERRORS **=** _max_errors_: Hiermit wird die maximale Anzahl von Syntaxfehlern angegeben, die in den Daten zulässig sind, bevor der Massenimportvorgang abgebrochen wird. Jede Zeile, die beim Massenimportvorgang nicht importiert werden kann, wird ignoriert und zählt dabei als ein Fehler. Wenn *max_errors* nicht angegeben ist, wird der Standardwert 10 verwendet.

> [!NOTE]
> Die Option MAX_ERRORS kann nicht zur Einschränkungsüberprüfung oder zum Konvertieren der Datentypen **money** und **bigint** verwendet werden.

ORDER ( { *column* [ ASC | DESC ] } [ **,** ... *n* ] ): gibt an, wie die Daten in der Datendatei sortiert werden. Die Leistung des Massenkopierens wird verbessert, wenn die zu importierenden Daten entsprechend dem gruppierten Index der Tabelle (falls vorhanden) sortiert sind. Wenn die Datendatei in einer anderen Reihenfolge sortiert wird, die von der Reihenfolge eines Schlüssels des gruppierten Indexes abweicht, oder die Tabelle keinen gruppierten Index hat, wird die ORDER-Klausel ignoriert. Die angegebenen Spaltennamen müssen gültige Spaltennamen in der Zieltabelle sein. Standardmäßig geht der Masseneinfügevorgang davon aus, dass die Datendatei nicht sortiert ist. Beim optimierten Massenimport wird in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auch überprüft, ob die importierten Daten sortiert sind.

*n*: ist ein Platzhalter, der anzeigt, dass mehrere Spalten angegeben werden können.

ROWS_PER_BATCH **=** _rows_per_batch_ Hiermit wird die ungefähre Anzahl von Datenzeilen in der Datendatei angegeben.

Standardmäßig werden alle Daten in der Datendatei als einzelne Transaktion an den Server gesendet, und die Anzahl von Zeilen im Batch ist dem Abfrageoptimierer nicht bekannt. Wenn Sie ROWS_PER_BATCH (mit einem Wert > 0) angeben, verwendet der Server diesen Wert, um den Massenimportvorgang zu optimieren. Der für ROWS_PER_BATCH angegebene Wert sollte etwa der tatsächlichen Zeilenanzahl entsprechen. Informationen zu Leistungsaspekten finden Sie unter "Hinweise" weiter unten in diesem Thema.

TABLOCK: gibt an, dass eine Sperre auf Tabellenebene für die Dauer des Massenimportvorgangs aktiviert wird. Eine Tabelle kann gleichzeitig von mehreren Clients geladen werden, wenn die Tabelle keine Indizes aufweist und TABLOCK angegeben ist. Standardmäßig wird das Sperrverhalten durch die Tabellenoption **table lock on bulk load** bestimmt. Da weniger Sperrkonflikte in der Tabelle auftreten, wenn diese während des Massenimportvorgangs gesperrt wird, verbessert sich in manchen Fällen die Leistung erheblich. Informationen zu Leistungsaspekten finden Sie unter "Hinweise" weiter unten in diesem Thema.

Bei einem Columnstore-Index ist das Sperrverhalten anders, weil es intern in mehrere Rowsets unterteilt ist. Jeder Thread lädt Daten ausschließlich in jedes Rowset, indem er eine X-Sperre auf das Rowset anwendet, die das parallele Laden von Daten mit Datenladungssitzungen zulässt. Durch die Verwendung der Option TABLOCK führt der Thread eine X-Sperre in der Tabelle durch (im Gegensatz zur BU-Sperre für traditionelle Rowsets), wodurch verhindert wird, dass andere gleichzeitige Threads Daten gleichzeitig laden.

### <a name="input-file-format-options"></a>Formatoptionen der Eingabedatei

FORMAT **=** 'CSV' **Gilt für:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Gibt eine CSV-Datei an, die dem Standard [RFC 4180](https://tools.ietf.org/html/rfc4180) entspricht.

```sql
BULK INSERT Sales.Orders
FROM '\\SystemX\DiskZ\Sales\data\orders.csv'
WITH ( FORMAT='CSV');
```

FIELDQUOTE **=** 'field_quote' **Gilt für:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Gibt ein Zeichen an, das als Anführungszeichen in der CSV-Datei verwendet wird. Wenn dies nicht angegeben ist, wird das Anführungszeichen (") so verwendet, wie es im Standard [RFC 4180](https://tools.ietf.org/html/rfc4180) definiert ist.

FORMATFILE **=** '_format_file_path_': gibt den vollständigen Pfad einer Formatdatei an. Eine Formatdatei beschreibt die Datendatei, die gespeicherte Antworten enthält. Diese Antworten wurden mithilfe des Hilfsprogramms **bcp** für die gleiche Tabelle oder Sicht erstellt. Die Formatdatei muss verwendet werden, wenn Folgendes zutrifft:

- Die Datendatei enthält größere oder weniger Spalten als die Tabelle oder Sicht.
- Die Spalten befinden sich in einer unterschiedlichen Reihenfolge.
- Die Spaltentrennzeichen variieren.
- Es liegen andere Änderungen im Datenformat vor. Formatdateien werden in der Regel mit dem Hilfsprogramm **bcp** erstellt und nach Bedarf mit einem Text-Editor geändert. Weitere Informationen finden Sie unter [Das Hilfsprogramm bcp](../../tools/bcp-utility.md) und [Erstellen einer Formatdatei](../../relational-databases/import-export/create-a-format-file-sql-server.md).

**Anwendungsbereich:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 und Azure SQL-Datenbank.
Ab [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 kann sich format_file_path in Azure Blob Storage befinden.

FIELDTERMINATOR **='** _field_terminator_ **':** gibt das Feldabschlusszeichen an, das für Datendateien vom Typ **char** und **widechar** verwendet werden soll. Standardmäßig wird \t (Tabstoppzeichen) als Feldabschlusszeichen verwendet. Weitere Informationen finden Sie unter [Angeben von Feld- und Zeilenabschlusszeichen &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).

ROWTERMINATOR **='** _row_terminator_ **':** gibt das Zeilenabschlusszeichen an, das für **char**- und **widechar**-Datendateien verwendet werden soll. Standardmäßig wird **\r\n** (Neue-Zeile-Zeichen) als Zeilenabschlusszeichen verwendet. Weitere Informationen finden Sie unter [Angeben von Feld- und Zeilenabschlusszeichen &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).

## <a name="compatibility"></a>Kompatibilität

BULK INSERT erzwingt strenge Datenüberprüfungen für die aus einer Datei gelesenen Daten, die möglicherweise zum Fehlschlagen vorhandener Skripts führen können, wenn diese für ungültige Daten in einer Datendatei ausgeführt werden. BULK INSERT überprüft beispielsweise Folgendes:

- Die native Darstellung der Datentypen **float** oder **real** ist gültig.
- Unicode-Daten besitzen eine gerade Bytelänge.

## <a name="data-types"></a>Datentypen

### <a name="string-to-decimal-data-type-conversions"></a>Konvertierungen von Zeichenfolgen- in Dezimaldatentypen

Die in BULK INSERT-Vorgängen verwendeten Konvertierungen von Zeichenfolgen in Dezimaldatentypen folgen insofern denselben Regeln wie die [!INCLUDE[tsql](../../includes/tsql-md.md)]-[CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md)-Funktion, dass sie Zeichenfolgen mit numerischen Werten in wissenschaftlicher Schreibweise nicht akzeptiert werden. Daher behandelt BULK INSERT diese Zeichenfolgen als ungültige Werte und meldet Konvertierungsfehler.

Um dieses Verhalten zu umgehen, verwenden Sie eine Formatdatei zum Massenimport von **float**-Daten in wissenschaftlicher Schreibweise in Spalten im Dezimalformat. Beschreiben Sie in der Formatdatei diese Spalte explizit als vom Datentyp **real** oder **float**. Weitere Informationen zu diesen Datentypen finden Sie unter [float und real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md).

> [!NOTE]
> Formatdateien stellen **real**-Daten als **SQLFLT4**-Datentyp und **float**-Daten als **SQLFLT8**-Datentyp dar. Weitere Informationen über Nicht-XML-Formatdateien finden Sie unter [Angeben des Dateispeichertyps mithilfe von bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md).

#### <a name="example-of-importing-a-numeric-value-that-uses-scientific-notation"></a>Beispiel: Importieren eines numerischen Werts in wissenschaftlicher Schreibweise

In diesem Beispiel wird die folgende Tabelle verwendet:

```sql
CREATE TABLE t_float(c1 FLOAT, c2 DECIMAL (5,4));
```

 Der Benutzer möchte nun per Massenimport Daten in die `t_float`-Tabelle kopieren. Die Datendatei „C:\t_float-c.dat“ enthält **float**-Daten in wissenschaftlicher Schreibweise, wie zum Beispiel:

```input
8.0000000000000002E-28.0000000000000002E-2
```

Diese Daten können jedoch mithilfe von BULK INSERT nicht direkt in die `t_float`-Tabelle importiert werden, da die zweite Spalte der Tabelle, `c2`, den `decimal`-Datentyp verwendet. Daher ist eine Formatdatei erforderlich. In der Formatdatei müssen die **float**-Daten in wissenschaftlichem Format dem Dezimalformat der Spalte `c2` zugeordnet werden.

Die folgende Formatdatei verwendet den `SQLFLT8`-Datentyp, um der zweiten Spalte das zweite Datenfeld zuzuordnen:

```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<RECORD>
<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t" MAX_LENGTH="30"/>
<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="30"/> </RECORD> <ROW>
<COLUMN SOURCE="1" NAME="c1" xsi:type="SQLFLT8"/>
<COLUMN SOURCE="2" NAME="c2" xsi:type="SQLFLT8"/> </ROW> </BCPFORMAT>
```

Geben Sie die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung aus, und verwenden Sie dabei für diese Formatdatei den Dateinamen `C:\t_floatformat-c-xml.xml`, damit die Testdaten in die Testtabelle importiert werden:

```sql
BULK INSERT bulktest..t_float
FROM 'C:\t_float-c.dat' WITH (FORMATFILE='C:\t_floatformat-c-xml.xml');
```

> [!IMPORTANT]
> Azure SQL-Datenbank unterstützt nur das Lesen aus Azure Blob Storage.

### <a name="data-types-for-bulk-exporting-or-importing-sqlxml-documents"></a>Datentypen für den Massenexport bzw. -import von SQLXML-Dokumenten

Verwenden Sie in der Formatdatei einen der folgenden Datentypen für den Massenexport oder -import von SQLXML-Daten:

|Datentyp|Wirkung|
|---------------|------------|
|SQLCHAR oder SQLVARCHAR|Die Daten werden in der Clientcodepage gesendet bzw. in der durch die Sortierung implizierten Codeseite. Damit wird dieselbe Wirkung erzielt wie mit der Angabe von DATAFILETYPE **='char'** ohne Formatdatei.|
|SQLNCHAR oder SQLNVARCHAR|Die Daten werden im Unicode-Format gesendet. Damit wird dieselbe Wirkung erzielt wie mit der Angabe von DATAFILETYPE **= 'widechar'** ohne Formatdatei.|
|SQLBINARY oder SQLVARBIN|Die Daten werden ohne Konvertierung gesendet.|
| &nbsp; | &nbsp; |

## <a name="general-remarks"></a>Allgemeine Hinweise

Einen Vergleich der BULK INSERT-Anweisung, der INSERT ... SELECT \* FROM OPENROWSET(BULK...)-Anweisung und des **bcp**-Befehl finden Sie unter [Massenimport und -export von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md).

Informationen zum Vorbereiten von Daten für den Massenimport finden Sie unter [Vorbereiten von Daten für den Massenexport oder -import &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

Die BULK INSERT-Anweisung kann innerhalb einer benutzerdefinierten Transaktion ausgeführt werden, um Daten in eine Tabelle oder eine Sicht zu importieren. Damit mehrere Übereinstimmungen für den Massenimport von Daten verwendet werden, kann in einer Transaktion optional die BATCHSIZE-Klausel in der BULK INSERT-Anweisung angegeben werden. Wenn ein Rollback für eine Transaktion mit mehreren Batches ausgeführt wird, wird für jeden Batch, der bei der Transaktion an [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gesendet wurde, ein Rollback ausgeführt.

## <a name="interoperability"></a>Interoperabilität

### <a name="importing-data-from-a-csv-file"></a>Importieren von Daten aus einer CSV-Datei

Ab [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 unterstützt BULK INSERT genauso wie Azure SQL-Datenbank das CSV-Format.
Vor [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 wurden CSV-Dateien von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Massenimportvorgängen nicht unterstützt. In manchen Fällen kann jedoch eine CSV-Datei als Datendatei für einen Massenimport von Daten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verwendet werden. Informationen zu den Anforderungen hinsichtlich des Imports von Daten aus einer CSV-Datendatei finden Sie unter [Vorbereiten von Daten für den Massenexport oder -import &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

## <a name="logging-behavior"></a>Protokollierungsverhalten

 Informationen dazu, wann Zeileneinfügevorgänge, die durch den Massenimport in SQL Server-Instanzen ausgeführt werden, im Transaktionsprotokoll protokolliert werden, finden Sie unter [Voraussetzungen für die minimale Protokollierung beim Massenimport](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md). Die minimale Protokollierung wird in Azure SQL-Datenbank nicht unterstützt.

## <a name="restrictions"></a><a name="Limitations"></a> Einschränkungen

Bei Verwendung einer Formatdatei mit BULK INSERT können maximal 1024 Felder angegeben werden. Dieser Höchstwert entspricht der maximalen Zahl zulässiger Spalten in einer Tabelle. Wenn Sie eine Formatdatei mit BULK INSERT und einer Datendatei verwenden, in der mehr als 1024 Felder enthalten sind, löst BULK INSERT den Fehler 4822 aus. Das [Hilfsprogramm bcp](../../tools/bcp-utility.md) unterliegt dieser Einschränkung nicht. Verwenden Sie deshalb für Datendateien mit mehr als 1024 Feldern BULK INSERT ohne Formatdatei, oder verwenden Sie den Befehl **bcp**.

## <a name="performance-considerations"></a>Überlegungen zur Leistung

Wenn die Anzahl der in einem einzelnen Batch geleerten Seiten einen internen Schwellenwert überschreitet, könnte ein vollständiger Scan des Pufferpools ausgeführt werden, um die zu leerenden Seiten bei der Durchführung eines Commits für den Batch zu identifizieren. Dieser vollständige Scan kann sich negativ auf die Massenimportleistung auswirken. Die Überschreitung des internen Schwellenwerts ist wahrscheinlich, wenn ein großer Pufferpool mit einem langsamen E/A-Subsystem kombiniert wird. Um Pufferüberläufe auf großen Computern zu vermeiden, verwenden Sie entweder keinen TABLOCK-Hinweis (da dieser die Massenoptimierungen entfernt), oder verwenden Sie eine kleinere Batchgröße (die die Massenoptimierungen beibehält).

Da Computer unterschiedlich sind, wird empfohlen, verschiedene Batchgrößen mit den geladenen Daten zu testen, um die optimale Vorgehensweise zu bestimmen.

Wenn Sie Azure SQL-Datenbank verwenden, sollten Sie in Betracht ziehen, die Leistungsstufe der Datenbank oder der Instanz vor dem Importieren vorübergehend zu erhöhen, wenn Sie eine große Datenmenge importieren.

## <a name="security"></a>Sicherheit

### <a name="security-account-delegation-impersonation"></a>Delegierung von Sicherheitskonten (Identitätswechsel)

Wenn ein Benutzer einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldenamen verwendet, wird das Sicherheitsprofil des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Prozesskontos verwendet. Ein Anmeldename, für den die SQL Server-Authentifizierung verwendet wird, kann nicht außerhalb der Datenbank-Engine authentifiziert werden. Wenn ein BULK INSERT-Befehl durch einen Anmeldenamen initiiert wird, der die SQL Server-Authentifizierung verwendet, wird die Datenverbindung folglich mithilfe des Sicherheitskontexts des SQL Server-Prozesskontos (dem vom SQL Server-Datenbank-Engine-Dienst verwendeten Konto) hergestellt. Um die Quelldaten lesen zu können, müssen Sie dem von der SQL Server-Datenbank-Engine verwendeten Konto Zugriff auf die Quelldaten gewähren. Wenn sich ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Benutzer dagegen mithilfe der Windows-Authentifizierung anmeldet, kann der Benutzer nur diejenigen Dateien lesen, auf die vom Benutzerkonto zugegriffen werden kann. Das gilt unabhängig vom Sicherheitsprofil des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesses.

Wenn die BULK INSERT-Anweisung mit **sqlcmd** oder **osql** auf einem Computer ausgeführt wird, um Daten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem zweiten Computer einzufügen, und *data_file* auf einem dritten Computer mithilfe eines UNC-Pfads angegeben wird, kann der Fehler 4861 ausgegeben werden.

Verwenden Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung, um diesen Fehler zu beheben, und geben Sie einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen an, der das Sicherheitsprofil des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesskontos verwendet, oder konfigurieren Sie Windows so, dass die Delegierung von Sicherheitskonten aktiviert ist. Informationen zum Aktivieren der Delegierung für Benutzerkonten finden Sie in der Windows-Hilfe.

Weitere Informationen über diese und andere Überlegungen zur Sicherheit bei der Verwendung von BULK INSERT finden Sie unter [Importieren von Massendaten mithilfe von BULK INSERT oder OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

Wenn Sie Daten aus Azure Blob Storage importieren, die nicht öffentlich sind (anonymer Zugriff), erstellen Sie eine [DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)-Anweisung, die auf einem SAS-Schlüssel basiert, der mit einer [MASTER KEY](create-master-key-transact-sql.md)-Anweisung verschlüsselt ist. Erstellen Sie anschließend eine [externe Datenbankquelle](../../t-sql/statements/create-external-data-source-transact-sql.md) für die Verwendung in Ihrem BULK INSERT-Befehl. Ein Beispiel dafür finden Sie unter [Importieren von Daten aus einer Datei in Azure Blob Storage](#f-importing-data-from-a-file-in-azure-blob-storage).

### <a name="permissions"></a>Berechtigungen

Erfordert die Berechtigungen INSERT und ADMINISTER BULK OPERATIONS. In der Azure SQL-Datenbank sind INSERT- und ADMINISTER DATABASE BULK OPERATIONS-Berechtigungen erforderlich. SQL Server für Linux unterstützt weder ADMINISTER BULK OPERATIONS-Berechtigungen noch die Rolle „bulkadmin“. Nur `sysadmin` kann Masseneinfügungen für SQL Server für Linux durchführen. 

Darüber hinaus ist die ALTER TABLE-Berechtigung erforderlich, wenn mindestens eine der folgenden Bedingungen zutrifft:

- Es sind Einschränkungen vorhanden, und die Option CHECK_CONSTRAINTS ist nicht angegeben.

   > [!NOTE]
   > Die Deaktivierung von Einschränkungen wurde als Standardverhalten festgelegt. Verwenden Sie die Option CHECK_CONSTRAINTS, um Einschränkungen explizit zu überprüfen.

- Es sind Trigger vorhanden, und die Option FIRE_TRIGGER ist nicht angegeben.

   > [!NOTE]
   > Standardmäßig werden Trigger nicht ausgelöst. Verwenden Sie die Option FIRE_TRIGGER, um Trigger explizit auszulösen.

- Importieren Sie Identitätswerte mithilfe der Option KEEPIDENTITY aus Datendateien.

## <a name="examples"></a>Beispiele

### <a name="a-using-pipes-to-import-data-from-a-file"></a>A. Verwenden von |-Zeichen zum Importieren von Daten aus einer Datei

Im folgenden Beispiel werden Bestellinformationen aus der angegebenen Datendatei in die `AdventureWorks2012.Sales.SalesOrderDetail`-Tabelle importiert, wobei der senkrechte Strich (`|`) als Feldabschlusszeichen und `|\n` als Zeilenabschlusszeichen verwendet wird.

```sql
BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
   FROM 'f:\orders\lineitem.tbl'
   WITH
      (  
         FIELDTERMINATOR =' |'
         , ROWTERMINATOR =' |\n'
      );
```

> [!IMPORTANT]
> Azure SQL-Datenbank unterstützt nur das Lesen aus Azure Blob Storage.

### <a name="b-using-the-fire_triggers-argument"></a>B. Verwenden des FIRE_TRIGGERS-Arguments

Im folgenden Beispiel wird das `FIRE_TRIGGERS`-Argument angegeben.

```sql
BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
   FROM 'f:\orders\lineitem.tbl'
   WITH
     (
         FIELDTERMINATOR =' |'
         , ROWTERMINATOR = ':\n'
         , FIRE_TRIGGERS
      );
```

> [!IMPORTANT]
> Azure SQL-Datenbank unterstützt nur das Lesen aus Azure Blob Storage.

### <a name="c-using-line-feed-as-a-row-terminator"></a>C. Verwenden des Zeilenvorschubs als Zeilenabschlusszeichen

 Im folgenden Beispiel wird eine Datei importiert, in der der Zeilenvorschub als ein Zeilenabschlusszeichen verwendet wird, z. B. eine UNIX-Ausgabe:

```sql
DECLARE @bulk_cmd VARCHAR(1000);
SET @bulk_cmd = 'BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
FROM ''<drive>:\<path>\<filename>''
WITH (ROWTERMINATOR = '''+CHAR(10)+''')';
EXEC(@bulk_cmd);
```

> [!NOTE]
> Abhängig davon, wie Textdateien von Microsoft Windows behandelt werden, wird **(\n** automatisch durch **\r\n)** ersetzt.

> [!IMPORTANT]
> Azure SQL-Datenbank unterstützt nur das Lesen aus Azure Blob Storage.

### <a name="d-specifying-a-code-page"></a>D: Angeben einer Codepage

In den folgenden Beispielen wird veranschaulicht, wie eine Codepage angegeben wird.

```sql
BULK INSERT MyTable
FROM 'D:\data.csv'
WITH
( CODEPAGE = '65001'
   , DATAFILETYPE = 'char'
   , FIELDTERMINATOR = ','
);
```

> [!IMPORTANT]
> Azure SQL-Datenbank unterstützt nur das Lesen aus Azure Blob Storage.

### <a name="e-importing-data-from-a-csv-file"></a>E. Importieren von Daten aus einer CSV-Datei

Im folgende Beispiel wird gezeigt, wie eine CSV-Datei angegeben wird, bei der die Kopfzeile (erste Zeile) übersprungen, `;` als Feldabschlusszeichen und `0x0a` als Zeilenabschlusszeichen verwendet wird:

```sql
BULK INSERT Sales.Invoices
FROM '\\share\invoices\inv-2016-07-25.csv'
WITH (FORMAT = 'CSV'
      , FIRSTROW=2
      , FIELDQUOTE = '\'
      , FIELDTERMINATOR = ';'
      , ROWTERMINATOR = '0x0a');
```

> [!IMPORTANT]
> Azure SQL-Datenbank unterstützt nur das Lesen aus Azure Blob Storage.

### <a name="f-importing-data-from-a-file-in-azure-blob-storage"></a>F. Importieren von Daten aus einer Datei in Azure Blob Storage

Das folgende Beispiel zeigt, wie Daten aus einer CSV-Datei in einen Speicherort von Azure Blob Storage geladen werden, für den Sie einen SAS-Schlüssel erstellt haben. Der Speicherort von Azure Blob Storage wird als externe Datenquelle konfiguriert. Hierfür sind datenbankweit gültige Anmeldeinformationen mit einer Shared Access Signature (SAS) erforderlich, die mit einem Hauptschlüssel in der Benutzerdatenbank verschlüsselt ist.

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
          LOCATION = 'https://****************.blob.core.windows.net/invoices'
          , CREDENTIAL= MyAzureBlobStorageCredential --> CREDENTIAL is not required if a blob is configured for public (anonymous) access!
);

BULK INSERT Sales.Invoices
FROM 'inv-2017-12-08.csv'
WITH (DATA_SOURCE = 'MyAzureBlobStorage');
```

> [!IMPORTANT]
> Azure SQL unterstützt nur das Lesen aus Azure Blob Storage.

### <a name="g-importing-data-from-a-file-in-azure-blob-storage-and-specifying-an-error-file"></a>G. Importieren von Daten aus einer Datei im Azure Blob Storage und Festlegen einer Fehlerdatei

Das folgende Beispiel zeigt, wie man Daten aus einer CSV-Datei in einen Azure Blob Storage-Verzeichnis lädt, das als externe Datenquelle konfiguriert wurde und auch eine Fehlerdatei angibt. Dies erfordert datenbankweit gültige Anmeldeinformationen, die SAS verwenden. Beachten Sie, dass die Option ERRORFILE, wenn sie auf Azure SQL-Datenbank ausgeführt wird, von ERRORFILE_DATA_SOURCE begleitet werden sollte, da sonst beim Import ein Berechtigungsfehler auftreten könnte. Die in ERRORFILE angegebene Datei darf nicht im Container vorhanden sein.

```sql
BULK INSERT Sales.Invoices
FROM 'inv-2017-12-08.csv'
WITH (
         DATA_SOURCE = 'MyAzureInvoices'
         , FORMAT = 'CSV'
         , ERRORFILE = 'MyErrorFile'
         , ERRORFILE_DATA_SOURCE = 'MyAzureInvoices');
```

Vollständige `BULK INSERT`-Beispiele einschließlich der Konfiguration der Anmeldeinformation und externen Datenquelle finden Sie unter [Beispiele für Massenzugriff auf Daten in Azure Blob Storage](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

### <a name="additional-examples"></a>Zusätzliche Beispiele

 Weitere `BULK INSERT`-Beispiele finden Sie in den folgenden Themen:

- [Beispiele für den Massenimport und -export von XML-Dokumenten &#40;SQL Server&#41;](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)
- [Beibehalten von Identitätswerten beim Massenimport von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)
- [Beibehalten von NULL-Werten oder Verwenden von Standardwerten während des Massenimports &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)
- [Angeben von Feld- und Zeilenabschlusszeichen &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)
- [Massenimport von Daten mithilfe einer Formatdatei &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [Verwenden des Zeichenformats zum Importieren und Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)
- [Verwenden des nativen Formats zum Importieren oder Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)
- [Verwenden des Unicode-Zeichenformats zum Importieren und Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)
- [Verwenden des nativen Unicode-Formats zum Importieren oder Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)
- [Überspringen einer Tabellenspalte mithilfe einer Formatdatei &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [Verwenden einer Formatdatei zum Zuordnen von Tabellenspalten zu Datendateifeldern &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>Weitere Informationen

- [Massenimport und -export von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)
- [bcp (Hilfsprogramm)](../../tools/bcp-utility.md)
- [Formatdateien zum Importieren oder Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)
- [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
- [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)
- [Vorbereiten von Daten für den Massenexport oder -import &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)
- [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)