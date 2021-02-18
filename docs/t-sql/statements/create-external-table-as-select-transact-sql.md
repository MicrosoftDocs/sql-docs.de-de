---
description: CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)
title: CREATE EXTERNAL TABLE AS SELECT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/10/2017
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.subservice: design
ms.topic: conceptual
f1_keywords:
- CREATE EXTERNAL TABLE AS SELECT
- CREATE_EXTERNAL_TABLE_AS_SELECT
- CREATE EXTERNAL TABLE AS SELECT_TSQL
helpviewer_keywords:
- External, table
- PolyBase, external table
- External, table create as select
- PolyBase, create table as select
ms.assetid: 32dfe254-6df7-4437-bfd6-ca7d37557b0a
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 14a7dce61b50ced328c103ec88973e9bf7b18c7d
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100345243"
---
# <a name="create-external-table-as-select-transact-sql"></a>CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Erstellt eine externe Tabelle und exportiert dann die Ergebnisse einer [!INCLUDE[tsql](../../includes/tsql-md.md)]-SELECT-Anweisung gleichzeitig nach Hadoop oder Azure Blob Storage.

 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql 
CREATE EXTERNAL TABLE [ [database_name  . [ schema_name ] . ] | schema_name . ] table_name   
    WITH (   
        LOCATION = 'hdfs_folder',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
    AS <select_statement>  
[;]  
  
<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage  
    | REJECT_VALUE = reject_value  
    | REJECT_SAMPLE_VALUE = reject_sample_value  
}  
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>  
```

## <a name="arguments"></a>Argumente
 **[ [ *Datenbankname* . [ *Schemaname* ] . ] | *Schemaname* . ] *Tabellenname***: Dies ist der ein- bis dreiteilige Name der Tabelle, die in der Datenbank erstellt werden soll. Bei einer externen Tabelle werden nur die Metadaten der Tabelle in der relationalen Datenbank gespeichert. 

 **LOCATION =  '*HDFS-Ordner*'** : Gibt an, wohin die Ergebnisse der SELECT-Anweisung in der externen Datenquelle geschrieben werden sollen. Der Speicherort ist ein Ordnername und kann wahlweise einen Pfad enthalten, der relativ zum Stammordner des Hadoop-Clusters oder der Azure Blob Storage-Instanz ist. PolyBase erstellt den Pfad und den Ordner, wenn diese nicht bereits vorhanden sind.

Die externen Dateien mit dem Namen *QueryID_date_time_ID.format* werden in *hdfs_folder* geschrieben, wobei *ID* ein inkrementeller Bezeichner ist und *format* das Format der exportierten Daten. Beispiel : QID776_20160130_182739_0.orc.

 **DATA_SOURCE = *Name_der_externen_Datenquelle***: Gibt den Namen eines externen Datenquellenobjekts an. Dieses enthält den Speicherort, in dem die externen Daten gespeichert sind oder gespeichert werden sollen. Der Speicherort ist entweder ein Hadoop-Cluster oder eine Azure Blob Storage-Instanz. Verwenden Sie zum Erstellen einer externen Datenquelle [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md).

 **FILE_FORMAT = *Name_des_externen_Dateiformats***: Gibt den Namen des externen Dateiformatobjekts an, in dem das Format für die externe Datendatei enthalten ist. Verwenden Sie zum Erstellen eines externen Dateiformats [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md).

 Zu dem Zeitpunkt der Ausführung der CREATE EXTERNAL TABLE AS SELECT-Anweisung werden keine **REJECT-Optionen** angewendet. Stattdessen werden sie hier angegeben, damit die Datenbank sie später beim Importieren von Daten aus der externen Tabelle verwenden kann. Wenn die CREATE TABLE AS SELECT-Anweisung später Daten aus der externen Tabelle auswählt, verwendet die Datenbank die REJECT-Optionen, um die Anzahl oder den Prozentsatz an Zeilen zu bestimmen, für die ein Importfehler auftreten kann, bevor der Importvorgang beendet wird.

   - **REJECT_VALUE = *Ablehnungswert***: Gibt die Anzahl oder den Prozentsatz an Zeilen an, für die ein Importfehler auftreten kann, bevor die Datenbank den Importvorgang anhält.

   - **REJECT_TYPE = **value** | percentage**: Gibt an, ob die REJECT_VALUE-Option als Literalwert oder Prozentsatz angegeben ist.

      - **value**: Wird verwendet, wenn REJECT_VALUE ist ein Literalwert ist, kein Prozentsatz. Die Datenbank beendet das Importieren von Zeilen aus der externen Datendatei, wenn die Anzahl der fehlerhaften Zeilen den Wert von *reject_value* überschreitet.

        Gilt beispielsweise „REJECT_VALUE = 5“ und „REJECT_TYPE = value“, dann beendet die Datenbank den Zeilenimport, nachdem fünf Zeilen nicht importiert werden konnten.

      - **percentage**: Wird verwendet, wenn REJECT_VALUE ein Prozentsatz ist, kein Literalwert. Die Datenbank beendet das Importieren von Zeilen aus der externen Datendatei, wenn der *Prozentsatz* der fehlerhaften Zeilen den Prozentsatz von *reject_value* überschreitet. Der Prozentsatz der fehlerhaften Zeilen wird in Intervallen berechnet.

   - **REJECT_SAMPLE_VALUE = *Ablehnungswert***: Ist erforderlich, wenn „REJECT_TYPE = percentage“ angegeben ist. Dieser Wert gibt die Anzahl von Zeilen für einen Importversuch an, bevor die Datenbank den Prozentsatz der fehlerhaften Zeilen neu berechnet.

      Gilt beispielsweise REJECT_SAMPLE_VALUE = 1000, dann berechnet die Datenbank den Prozentsatz fehlerhafter Zeilen nach dem Importversuch von 1000 Zeilen aus der externen Datendatei. Ist der Prozentsatz fehlerhafter Zeilen kleiner als *reject_value*, führt die Datenbank einen erneuten Ladeversuch von 1000 Zeilen aus. Nach jedem weiteren Importversuch von 1000 Zeilen berechnet die Datenbank den Prozentsatz fehlerhafter Zeilen neu.

     > [!NOTE]
     >  Da die Datenbank den Prozentsatz fehlerhafter Zeilen in Intervallen berechnet, kann der tatsächliche Prozentsatz fehlerhafter Zeilen den *Ablehnungswert* überschreiten.

     **Beispiel:**

     In diesem Beispiel wird verdeutlicht, wie die drei REJECT-Optionen interagieren. Gilt beispielsweise REJECT_TYPE = Prozentsatz, REJECT_VALUE = 30 und REJECT_SAMPLE_VALUE = 100, dann könnte das folgende Szenario auftreten:

     - Die Datenbank versucht, die ersten 100 Zeilen zu laden. Davon können 75 Zeilen erfolgreich geladen werden, 25 jedoch nicht.
     - Der berechnete Prozentsatz fehlerhafter Zeilen ist mit 25 % kleiner als der Ablehnungswert von 30 %. Somit muss der Ladevorgang nicht angehalten werden.
     - Die Datenbank versucht, die nächsten 100 Zeilen zu laden. Dieses Mal können 25 Zeilen erfolgreich geladen werden, 75 dagegen nicht.
     - Der Prozentsatz fehlerhafter Zeilen wird mit 50 % neu berechnet. Der Prozentsatz fehlerhafter Zeilen hat den REJECT-Wert von 30 % überschritten.
     - Nach dem Ladeversuch von 200 Zeilen schlägt der Ladevorgang mit 50 % fehlerhaften Zeilen fehl. Dieser Prozentsatz liegt über dem angegebenen Grenzwert von 30 %.

 **WITH *Allgemeiner Tabellenausdruck***: Gibt ein temporäres benanntes Resultset an, das als allgemeiner Tabellenausdruck (CTE, Common Table Expression) bezeichnet wird. Weitere Informationen finden Sie unter [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md). 

 **SELECT \<select_criteria>** füllt die neue Tabelle mit den Ergebnissen einer SELECT-Anweisung auf. *Select_criteria* ist der Hauptteil der SELECT-Anweisung, der bestimmt, welche Daten in die neue Tabelle kopiert werden sollen. Informationen zu SELECT-Anweisungen finden Sie unter [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).

## <a name="permissions"></a>Berechtigungen

 *Datenbankbenutzer* benötigen sämtliche folgenden Berechtigungen oder Mitgliedschaften, um diesen Befehl ausführen zu können:

- **ALTER SCHEMA**-Berechtigung für das lokale Schema, das die neue Tabelle oder Mitgliedschaft in der festen Datenbankrolle **db_ddladmin** enthält
- **CREATE TABLE**-Berechtigung oder -Mitgliedschaft in der festen Datenbankrolle **db_ddladmin**
- **SELECT**-Berechtigung für alle Objekte, auf die in *select_criteria* verwiesen wird

 Eine Anmeldung erfordert alle folgenden Berechtigungen:

- **ADMINISTER BULK OPERATIONS**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**
- **Schreibberechtigung** zum Lesen und Schreiben im Hadoop-Cluster oder in der Blob Storage-Instanz.

 > [!IMPORTANT]
 >  Mit der Berechtigung ALTER ANY EXTERNAL DATA SOURCE erhält jeder Prinzipal die Fähigkeit, beliebige externe Datenquellenobjekte zu erstellen und zu ändern. Damit ist auch der Zugriff auf alle datenbankweit gültigen Anmeldeinformationen der Datenbank möglich. Dies ist eine weitreichende Berechtigung und darf nur vertrauenswürdigen Prinzipalen im System erteilt werden.

## <a name="error-handling"></a>Fehlerbehandlung
 Beim Exportieren von Daten mithilfe der CREATE EXTERNAL TABLE AS SELECT-Anweisung in eine Datei mit Texttrennzeichen gibt es keine Ablehnungsdatei für Zeilen mit Exportfehler.

 Wenn Sie eine externe Tabelle erstellen, versucht die Datenbank, eine Verbindung mit dem externen Hadoop-Cluster oder der Blob Storage-Instanz herzustellen. Bei einem Verbindungsfehler kann der Befehl nicht ausgeführt werden, und die externe Tabelle wird nicht erstellt. Da mindestens drei Versuche für einen Verbindungsaufbau erfolgen, kann es eine Minute oder länger dauern, bis für den Befehl ein Fehler auftritt.

 Tritt bei der CREATE EXTERNAL TABLE AS SELECT-Anweisung ein Fehler auf oder wird sie abgebrochen, erfolgt ein einmaliger Versuch durch die Datenbank, alle in der externen Datenquelle bereits erstellten neuen Dateien und Ordner zu entfernen.

 Die Datenbank meldet alle Java-Fehler, die während des Datenexports auf der externen Datenquelle auftreten.

##  <a name="general-remarks"></a><a name="GeneralRemarks"></a> Allgemeine Hinweise
 Nach Abschluss der CREATE EXTERNAL TABLE AS SELECT-Anweisung können Sie [!INCLUDE[tsql](../../includes/tsql-md.md)]-Abfragen in der externen Tabelle ausführen. Diese Vorgänge importieren Daten für die Dauer der Abfrage in die Datenbank, sofern für den Import nicht die CREATE TABLE AS SELECT-Anweisung verwendet wird.

 Der Name und die Definition der externen Tabelle werden in den Metadaten der Datenbank gespeichert. Die Daten werden in der externen Datenquelle gespeichert.

 Die externen Dateien heißen *QueryID_date_time_ID.format*, wobei *ID* ein inkrementeller Bezeichner ist und *Format* das Format der exportierten Daten. Beispiel : QID776_20160130_182739_0.orc.

 Die CREATE EXTERNAL TABLE AS SELECT-Anweisung erstellt immer eine nicht partitionierte Tabelle, selbst wenn die Quelltabelle partitioniert ist.

 Bei Abfrageplänen, die mit EXPLAIN erstellt wurden, verwendet die Datenbank die folgenden Abfrageplanvorgänge für externe Tabellen:

- Verschieben von externer Zufallswiedergabe
- Verschieben von externer Übertragung
- Verschieben von externer Partition

 **Anwendungsbereich:** Parallel Data Warehouse

Als Voraussetzung für das Erstellen einer externen Tabelle muss der Applianceadministrator die Hadoop-Konnektivität konfigurieren. Weitere Informationen finden Sie in der Dokumentation zu Analytics Platform System unter „Configure Connectivity to External Data“ (Konfigurieren der Konnektivität mit externen Daten), die Sie aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=48241) herunterladen können.

## <a name="limitations-and-restrictions"></a>Einschränkungen
 Daten aus externen Tabellen befinden sich außerhalb der Datenbank. Sicherungs- und Wiederherstellungsvorgänge werden nur für Daten ausgeführt, die in der Datenbank gespeichert sind. Das bedeutet, dass nur die Metadaten gesichert und wiederhergestellt werden.

 Beim Wiederherstellen einer Datenbanksicherung, die eine externe Tabelle enthält, überprüft die Datenbank die Verbindung mit der externen Datenquelle nicht. Wenn auf die ursprüngliche Quelle nicht zugegriffen werden kann, werden die Metadaten der externen Tabelle trotzdem erfolgreich wiederhergestellt. Bei SELECT-Vorgängen für die externe Tabelle treten jedoch Fehler auf.

 Die Datenbank kann keine Datenkonsistenz zwischen Datenbank und externen Daten garantieren. Sie als Benutzer tragen die alleinige Verantwortung für die Konsistenz zwischen externen Daten und Datenbank.

 DML-Vorgänge (Data Manipulation Language, Datenbearbeitungssprache) werden in externen Tabellen nicht unterstützt. So können Sie beispielsweise externe Daten nicht mit den [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen UPDATE, INSERT oder DELETE ändern.

 Die Anweisungen CREATE TABLE, DROP TABLE, CREATE STATISTICS, DROP STATISTICS, CREATE VIEW und DROP VIEW sind die einzigen DDL-Vorgänge (Data Definition Language, Datenbeschreibungssprache), die für externe Tabelle zulässig sind.

 Externe Tabellen für den serverlosen SQL-Pool können nicht an einem Speicherort erstellt werden, an dem Sie zurzeit über Daten verfügen. Um einen Speicherort wiederzuverwenden, der bereits zum Speichern von Daten verwendet wurde, muss der Speicherort auf ADLS manuell gelöscht werden.

 PolyBase kann bei 32 gleichzeitigen PolyBase-Abfragen maximal 33.000 Dateien pro Ordner verarbeiten. Diese maximale Anzahl schließt sowohl Dateien als auch Unterordner im jeweiligen HDFS-Ordner ein. Bei weniger als 32 gleichzeitigen Abfragen können Benutzer auch PolyBase-Abfragen für Ordner in HDFS ausführen, die mehr als 33.000 Dateien enthalten. Benutzern von Hadoop und PolyBase wird empfohlen, kurze Dateipfade und nicht mehr als 30.000 Dateien pro HDFS-Ordner zu verwenden. Verweise auf eine zu große Anzahl von Dateien können zu einer JVM-Ausnahme aufgrund unzureichenden Arbeitsspeichers führen.

 [SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md) hat keine Auswirkungen auf die CREATE EXTERNAL TABLE AS SELECT-Anweisung. Verwenden Sie [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md), um ein ähnliches Verhalten zu erzielen.

 Wenn CREATE EXTERNAL TABLE AS SELECT aus dem Formattyp RCFILE auswählt, dürfen die Spaltenwerte in RCFILE kein Pipezeichen (|) enthalten.

Eine CREATE EXTERNAL TABLE AS SELECT-Anweisung in Parquet- oder ORC-Dateien führt zu Fehlern wie z. B. abgelehnten Datensätzen, wenn folgende Zeichen in den Daten vorhanden sind:

- |
- “ (Anführungszeichen)
- /r/n
- /r
- /n

Wenn Sie CREATE EXTERNAL TABLE AS SELECT mit diesen Zeichen verwenden möchten, müssen Sie zunächst die CREATE EXTERNAL TABLE AS SELECT-Anweisung ausführen, um die Daten in durch Trennzeichen getrennte Textdateien zu exportieren. Diese können Sie dann mit einem externen Tool in Parquetdateien oder ORC-Dateien konvertieren.

## <a name="locking"></a>Sperren
 Akzeptiert eine gemeinsame Sperre für das SCHEMARESOLUTION-Objekt.

##  <a name="examples"></a><a name="Examples"></a> Beispiele

### <a name="a-create-a-hadoop-table-by-using-create-external-table-as-select"></a>A. Erstellen einer Hadoop-Tabelle mithilfe von CREATE EXTERNAL TABLE AS SELECT

 Das folgende Beispiel erstellt eine neue externe Tabelle mit dem Namen `hdfsCustomer`, die die Spaltendefinitionen und Daten aus der Quelltabelle `dimCustomer` verwendet.

 Die Tabellendefinition ist in der Datenbank gespeichert. Die Ergebnisse der SELECT-Anweisung werden in die Datei „/pdwdata/customer.tbl“ auf der externen Hadoop-Datenquelle *customer_ds* exportiert. Die Datei wird entsprechend dem externen Dateiformat *customer_ff* formatiert.

 Der Dateiname wird von der Datenbank generiert und enthält die Abfrage-ID. So lässt sich die Abfrage einfacher mit der Datei abgleichen, die von der Abfrage erzeugt wird.

 Der Pfad `hdfs://xxx.xxx.xxx.xxx:5000/files/`, der dem Customer-Verzeichnis vorangeht, muss bereits vorhanden sein. Sollte das Customer-Verzeichnis noch nicht vorhanden sein, wird es von der Datenbank erstellt.

> [!NOTE]
>  In diesem Beispiel wird 5000 angegeben. Wird kein Port angegeben, verwendet die Datenbank 8020 als Standardport.

 Dies ergibt `hdfs:// xxx.xxx.xxx.xxx:5000/files/Customer/ QueryID_YearMonthDay_HourMinutesSeconds_FileIndex.txt.` als Hadoop-Speicherort und -Dateiname.

```sql  
-- Example is based on AdventureWorks   
CREATE EXTERNAL TABLE hdfsCustomer  
WITH (  
        LOCATION='/pdwdata/customer.tbl',  
        DATA_SOURCE = customer_ds,  
        FILE_FORMAT = customer_ff  
) AS SELECT * FROM dimCustomer;  
```

### <a name="b-use-a-query-hint-with-create-external-table-as-select"></a>B. Verwenden eines Abfragehinweises mit CREATE EXTERNAL TABLE AS SELECT

 Diese Abfrage zeigt die grundlegende Syntax für die Verwendung eines JOIN-Abfragehinweises mit der CREATE EXTERNAL TABLE AS SELECT-Anweisung. Nach dem Senden der Abfrage verwendet die Datenbank die Hashjoinstrategie, um den Abfrageplan zu generieren. Weitere Informationen zu Join-Abfragehinweisen und zur Verwendung der OPTION-Klausel finden Sie unter [OPTION-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md).

> [!NOTE]
>  In diesem Beispiel wird 5000 angegeben. Wird kein Port angegeben, verwendet die Datenbank 8020 als Standardport.

```sql  
-- Example is based on AdventureWorks  
CREATE EXTERNAL TABLE dbo.FactInternetSalesNew  
WITH   
    (   
        LOCATION = '/files/Customer',  
        DATA_SOURCE = customer_ds,  
        FILE_FORMAT = customer_ff  
    )  
AS SELECT T1.* FROM dbo.FactInternetSales T1 JOIN dbo.DimCustomer T2  
ON ( T1.CustomerKey = T2.CustomerKey )  
OPTION ( HASH JOIN );  
```

## <a name="see-also"></a>Weitere Informationen
 - [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)
 - [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md)
 - [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)
 - [CREATE TABLE &#40;Azure Synapse Analytics, Parallel Data Warehouse&#41;](~/t-sql/statements/create-table-azure-sql-data-warehouse.md)
 - [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)
 - [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)
 - [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)



