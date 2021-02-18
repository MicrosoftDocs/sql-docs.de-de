---
description: Importieren von JSON-Dokumenten in SQL Server
title: Importieren von JSON-Dokumenten
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
ms.assetid: 0e908ec0-7173-4cd2-8f48-2700757b53a5
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 34f8655a24a84d50c5f677d819330537fa030cc6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075056"
---
# <a name="import-json-documents-into-sql-server"></a>Importieren von JSON-Dokumenten in SQL Server

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

In diesem Artikel wird das Importieren von JSON-Dateien in SQL Server beschrieben. Zurzeit werden viele JSON-Dokumente in Dateien gespeichert. Anwendungen protokollieren Informationen in JSON-Dateien, Sensoren generieren Informationen, die in JSON-Dateien gespeichert werden, usw. Es ist wichtig, in der Lage zu sein, die in Dateien gespeicherten JSON-Daten zu lesen, in SQL Server zu laden und sie zu analysieren.

## <a name="import-a-json-document-into-a-single-column"></a>Importieren eines JSON-Dokuments in eine einzelne Spalte

**OPENROWSET(BULK)** ist eine Tabellenwertfunktion, die Daten aus einer beliebigen Datei auf dem lokalen Laufwerk oder im Netzwerk lesen kann, wenn SQL Server über Lesezugriff für diesen Speicherort verfügt. Sie gibt eine Tabelle mit einer einzelnen Spalte zurück, die den Inhalt der Datei enthält. Es gibt verschiedene Optionen, die Sie mit der OPENROWSET(Bulk)-Funktion verwenden können, z.B. Trennzeichen. Im einfachsten Fall können Sie einfach den gesamten Inhalt einer Datei als Textwert laden. (Dieser einzelne große Wert wird als ein „Single Character Large Object“ oder SINGLE_CLOB bezeichnet.) 

Hier finden Sie ein Beispiel für die **OPENROWSET(BULK)**-Funktion, die den Inhalt einer JSON-Datei liest und als einzelnen Wert an den Benutzer zurückgibt:

```sql
SELECT BulkColumn
 FROM OPENROWSET (BULK 'C:\JSON\Books\book.json', SINGLE_CLOB) as j;
```

OPENROWSET(BULK) liest den Inhalt der Datei und gibt ihn in `BulkColumn` zurück.

Sie können auch den Inhalt der Datei in eine lokale Variable oder in eine Tabelle laden, wie im folgenden Beispiel gezeigt:

```sql
-- Load file contents into a variable
SELECT @json = BulkColumn
 FROM OPENROWSET (BULK 'C:\JSON\Books\book.json', SINGLE_CLOB) as j

-- Load file contents into a table 
SELECT BulkColumn
 INTO #temp 
 FROM OPENROWSET (BULK 'C:\JSON\Books\book.json', SINGLE_CLOB) as j
```

Nachdem Sie den Inhalt der JSON-Datei geladen haben, können Sie den JSON-Text in einer Tabelle speichern.


## <a name="import-json-documents-from-azure-file-storage"></a>Importieren von JSON-Dokumenten aus Azure File Storage

Sie können auch wie oben beschrieben OPENROWSET(BULK) zum Lesen von JSON-Dateien aus anderen Dateispeicherorten verwenden, auf die SQL Server zugreifen kann. Azure File Storage unterstützt z.B. das SMB-Protokoll. Daher können Sie der Azure File Storage-Freigabe mithilfe der folgenden Vorgehensweise eine virtuelle Festplatte zuordnen:

1.  Erstellen Sie mithilfe des Azure-Portals oder Azure PowerShell ein Dateispeicherkonto (z.B. `mystorage`), eine Dateifreigabe (z.B. `sharejson`) und einen Ordner in Azure File Storage.
2.  Laden Sie einige JSON-Dateien in die Dateispeicherfreigabe hoch.
3.  Erstellen Sie auf Ihrem Computer eine ausgehende Firewallregel in Windows-Firewall, die Port 445 zulässt. Beachten Sie, dass Ihr Internetdienstanbieter diesen Port möglicherweise blockiert. Wenn ein DNS-Fehler (Fehler 53) im folgenden Schritt auftritt, haben Sie Port 445 nicht geöffnet, oder Ihr Internetdienstanbieter blockiert ihn.
4. Binden Sie die Azure File Storage-Dateifreigabe als lokales Laufwerk (z.B. `T:`) ein.

    Hier finden Sie die Befehlssyntax:

    ```dos
    net use [drive letter] \\[storage name].file.core.windows.net\[share name] /u:[storage account name] [storage account access key]
    ```

    Hier finden Sie ein Beispiel, in dem der lokale Laufwerkbuchstabe `T:` der Azure File Storage-Dateifreigabe zugewiesen wird:

    ```dos
    net use t: \\mystorage.file.core.windows.net\sharejson /u:myaccount hb5qy6eXLqIdBj0LvGMHdrTiygkjhHDvWjUZg3Gu7bubKLg==
    ```

    Der Speicherkontoschlüssel und der primäre und sekundäre Speicherkonto-Zugriffsschlüssel befinden sich im Abschnitt „Schlüssel“ unter „Einstellungen“ im Azure-Portal.

5.  Sie können jetzt über die Azure File Storage-Dateifreigabe auf die JSON-Dateien zugreifen, indem Sie das zugeordnete Laufwerk verwenden, wie im folgenden Beispiel gezeigt:

    ```sql
    SELECT book.* FROM
     OPENROWSET(BULK N't:\books\books.json', SINGLE_CLOB) AS json
     CROSS APPLY OPENJSON(BulkColumn)
     WITH( id nvarchar(100), name nvarchar(100), price float,
        pages_i int, author nvarchar(100)) AS book
    ```

Weitere Informationen zu Azure File Storage finden Sie unter [File Storage](https://azure.microsoft.com/services/storage/files/).

## <a name="import-json-documents-from-azure-blob-storage"></a>Importieren von JSON-Dokumenten aus Azure BLOB-Speicher

Sie können Dateien mit dem Befehl T-SQL BULK INSERT oder der OPENROWSET-Funktion direkt aus Azure Blob Storage in Azure SQL-Datenbank laden.

> [!NOTE]
> Diese Funktion wird in [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und Azure SQL hinzugefügt.

Erstellen Sie zunächst eine externe Datenquelle, wie im folgenden Beispiel gezeigt.

```sql
CREATE EXTERNAL DATA SOURCE MyAzureBlobStorage
 WITH ( TYPE = BLOB_STORAGE,
        LOCATION = 'https://myazureblobstorage.blob.core.windows.net',
        CREDENTIAL= MyAzureBlobStorageCredential);
```

Führen Sie anschließend einen BULK INSERT-Befehl mit der Option DATA_SOURCE aus.

```sql
BULK INSERT Product
FROM 'data/product.dat'
WITH ( DATA_SOURCE = 'MyAzureBlobStorage');
```

## <a name="parse-json-documents-into-rows-and-columns"></a>Analysieren von JSON-Dokumenten in Zeilen und Spalten

Statt die gesamte JSON-Datei als einzelnen Wert zu lesen, sollten Sie sie analysieren und die Bücher in der Tabelle sowie die Eigenschaften in Reihen und Zeilen zurückgeben. Im folgenden Beispiel wird eine JSON-Datei mit einer Liste von Büchern von [dieser Website](https://github.com/tamingtext/book/blob/master/apache-solr/example/exampledocs/books.json) verwendet.

### <a name="example-1"></a>Beispiel 1

Im einfachsten Beispiel können Sie einfach die gesamte Liste aus der Datei laden. 

```sql
SELECT value
 FROM OPENROWSET (BULK 'C:\JSON\Books\books.json', SINGLE_CLOB) as j
 CROSS APPLY OPENJSON(BulkColumn)
```

Beim vorangehenden OPENROWSET wird aus der Datei ein einzelner Textwert gelesen. OPENROWSET gibt den Wert als BulkColumn zurück und übergibt es weiter an die Funktion OPENJSON. OPENJSON durchläuft das Array von JSON-Objekten im BulkColumn-Array und gibt in jeder Zeile ein Buch zurück. Jede Zeile wird, wie nachfolgend gezeigt, als JSON formatiert.

```json
{"id":"978-0641723445", "cat":["book","hardcover"], "name":"The Lightning Thief", ... }
{"id":"978-1423103349", "cat":["book","paperback"], "name":"The Sea of Monsters", ... }
{"id":"978-1857995879", "cat":["book","paperback"], "name":"Sophie's World : The Greek", ... } 
{"id":"978-1933988177", "cat":["book","paperback"], "name":"Lucene in Action, Second", ... }
```

### <a name="example-2"></a>Beispiel 2

Die OPENJSON-Funktion kann den JSON-Inhalt analysieren und ihn in eine Tabelle oder ein Resultset transformieren. Im folgenden Beispiel wird der Inhalt geladen, die geladene JSON-Datei analysiert und die fünf Felder als Spalten zurückgegeben:

```sql
SELECT book.*
 FROM OPENROWSET (BULK 'C:\JSON\Books\books.json', SINGLE_CLOB) as j
 CROSS APPLY OPENJSON(BulkColumn)
 WITH( id nvarchar(100), name nvarchar(100), price float,
 pages_i int, author nvarchar(100)) AS book
```

In diesem Beispiel liest OPENROWSET(BULK) den Inhalt der Datei und übergibt den Inhalt mit einem definierten Schema für die Ausgabe an die OPENJSON-Funktion. OPENJSON vergleicht Eigenschaften in JSON-Objekten mithilfe der Spaltennamen. Die Eigenschaft `price` wird z.B. als `price`-Spalte zurückgegeben und in den float-Datentyp konvertiert. Dies sind die Ergebnisse:

|Id|Name|Preis|Seiten_i|Autor|
|---|---|---|---|---|
|978-0641723445|The Lightning Thief (Diebe im Olymp)|12,5|384|Rick Riordan| 
|978-1423103349|The Sea of Monsters (Im Bann des Zyklopen)|6.49|304|Rick Riordan| 
|978-1857995879|Sophie’s World : The Greek Philosophers (Sofies Welt: Roman über die Geschichte der Philosophie)|3.07|64|Jostein Gaarder| 
|978-1933988177|Lucene in Action, Zweite Auflage (nur englisch)|30.5|475|Michael McCandless|
||||||

Jetzt können Sie die Tabelle an den Benutzer zurückgeben oder die Daten in eine andere Tabelle laden.

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Weitere Informationen zu JSON in SQL Server und Azure SQL-Datenbank  
  
### <a name="microsoft-videos"></a>Microsoft-Videos

Eine visuelle Einführung in die JSON-Unterstützung, die in SQL Server und Azure SQL-Datenbank integriert ist, finden Sie in den folgenden Videos:

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a>Weitere Informationen

[Konvertieren von JSON-Daten in Zeilen und Spalten mit OPENJSON](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md)

