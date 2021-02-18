---
title: Speichern und Laden von R-Objekten mithilfe von ODBC
description: Das „RevoScaleR“-Paket enthält Serialisierungs- und Deserialisierungsfunktionen, die die Leistung deutlich steigern und das Objekt kompakter speichern.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 04/15/2018
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 379c6cc1d865b2820026948b352abb07b27eda52
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273021"
---
# <a name="save-and-load-r-objects-from-sql-server-using-odbc"></a>Speichern und Laden von R-Objekten aus SQL Server mithilfe von ODBC
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

SQL Server R Services können serialisierte R-Objekte in einer Tabelle speichern und dann die Objekt nach Bedarf aus der Tabelle laden, ohne den R-Code erneut ausführen oder das Modell erneut trainieren zu müssen. Diese Fähigkeit zum Speichern von R-Objekten in einer Datenbank ist entscheidend für Szenarios wie Trainieren und Speichern eines Modells und dann späteres Verwenden für Bewertungen oder Analysen.

Um die Leistung dieses ausschlaggebenden Schritts zu verbessern, enthält das **RevoScaleR** -Paket jetzt neue Serialisierungs- und Deserialisierungsfunktionen, die die Leistung deutlich steigern und das Objekt kompakter speichern. In diesem Artikel werden diese Funktionen und deren Verwendung beschrieben.

## <a name="overview"></a>Übersicht

Das **RevoScaleR** -Paket enthält jetzt neue Funktionen, mit denen es einfacher ist, R-Objekte in SQL Server zu speichern und die Objekte dann aus der SQL Server-Tabelle zu lesen. Im Allgemeinen verwendet jeder Funktionsaufruf einen einfachen Schlüssel-Wert-Speicher, in dem der Schlüssel der Name des Objekts und der Wert, der dem Schlüssel zugeordnet ist, das varbinary-R-Objekt ist, das in eine oder aus einer Tabelle verschoben werden soll.

Zum direkten Speichern von R-Objekten aus einer R-Umgebung in SQL Server müssen Sie folgende Schritte durchführen:

+ Etablieren Sie mithilfe der Datenquelle *RxOdbcData* eine Verbindung mit SQL Server.
+ Rufen Sie die neuen Funktionen über die ODBC-Verbindung auf.
+ Optional können Sie angeben, dass das Objekt nicht serialisiert werden soll. Wählen Sie dann einen neuen Komprimierungsalgorithmus aus, der anstelle des Standardkomprimierungsalgorithmus verwendet werden soll.

Standardmäßig wird jedes Objekt, das Sie aus R aufrufen, um es in SQL Server zu verschieben, serialisiert und komprimiert. Umgekehrt wird, wenn Sie ein Objekt aus einer SQL Server-Tabelle laden, um es in Ihrem R-Code zu verwenden, das Objekt deserialisiert und dekomprimiert.

## <a name="list-of-new-functions"></a>Liste neuer Funktionen

- `rxWriteObject` schreibt über die ODBC-Datenquelle ein R-Objekt in SQL Server.

- `rxReadObject` liest über eine ODBC-Datenquelle ein R-Objekt aus einer SQL Server-Datenbank.

- `rxDeleteObject` löscht ein R-Objekt aus der SQL Server-Datenbank, die in der ODBC-Datenquelle angegeben ist. Gibt es mehrere Objekte, die durch die Schlüssel/Version-Kombination gekennzeichnet sind, werden alle gelöscht.

- `rxListKeys` listet alle verfügbaren Objekte als Schlüssel-Wert-Paare auf. Dies erleichtert es Ihnen, die Namen und Versionen der R-Objekte zu ermitteln.

Ausführliche Hilfe zur Syntax jeder Funktion finden Sie in der R-Hilfe. Ausführliche Informationen stehen Ihnen auch in der [Referenzdokumentation zu ScaleR](/r-server/r-reference/revoscaler/revoscaler) zur Verfügung.

## <a name="how-to-store-r-objects-in-sql-server-using-odbc"></a>So speichern Sie R-Objekte über ODBC in SQL Server

In diesem Verfahren wird veranschaulicht, wie Sie die neuen Funktionen verwenden können, um ein Modell zu erstellen und dieses in SQL Server zu speichern.

1. Richten Sie die Verbindungszeichenfolge für die SQL Server-Instanz ein.
   ```R
   conStr <- 'Driver={SQL Server};Server=localhost;Database=storedb;Trusted_Connection=true'
   ```
2. Erstellen Sie in R ein *rxOdbcData* -Datenquellenobjekt, für das Sie die Verbindungszeichenfolge verwenden.
   ```R
   ds <- RxOdbcData(table="robjects", connectionString=conStr)
   ```

3. Löschen Sie die Tabelle, wenn sie bereits vorhanden ist und Sie keine alten Versionen der Objekte nachverfolgen möchten.

   ```R
   if(rxSqlServerTableExists(ds@table, ds@connectionString)) {
       rxSqlServerDropTable(ds@table, ds@connectionString)
       }
   ```
   
4. Definieren Sie eine Tabelle, in der binäre Objekten gespeichert werden können.

   ```R
   ddl <- paste(" CREATE TABLE [", ds@table, "] 
      (","  [id] varchar(200) NOT NULL,
       "," [value] varbinary(max),
       "," CONSTRAINT unique_id UNIQUE (id))", 
       sep = "") 
   ```
5. Öffnen Sie die ODBC-Verbindung, um die Tabelle zu erstellen, und schließen Sie die Verbindung, sobald die DDL-Anweisung abgeschlossen ist.

   ```R
    rxOpen(ds, "w") 
    rxExecuteSQLDDL(ds, ddl) 
    rxClose(ds)
    ```
6. Generieren Sie die R-Objekte, die Sie speichern möchten.

   ```R
   infertLogit <- rxLogit(case ~ age + parity + education + spontaneous + induced, 
     data = infert)
   ```
6. Verwenden Sie das zuvor erstellte *RxOdbcData* -Objekt, um das Modell in der Datenbank zu speichern.

   ```R
   rxWriteObject(ds, "logit.model", infertLogit)
   ```

## <a name="how-to-read-r-objects-from-sql-server-using-odbc"></a>So lesen Sie R-Objekte über ODBC aus SQL Server

In diesem Verfahren wird veranschaulicht, wie Sie die neuen Funktionen verwenden können, um ein Modell aus SQL Server zu laden.

1. Richten Sie die Verbindungszeichenfolge für die SQL Server-Instanz ein.

   ```R
   conStr2 <- 'Driver={SQL Server};Server=localhost;Database=storedb;Trusted_Connection=true'
   ```
2. Erstellen Sie in R ein *rxOdbcData* -Datenquellenobjekt, für das Sie die Verbindungszeichenfolge verwenden.

   ```R
   ds <- RxOdbcData(table="robjects", connectionString=conStr2)
   ```
3. Lesen Sie das Modell aus der Tabelle, indem Sie dessen R-Objektnamen angeben.

   ```R
    infertLogit2 <- rxReadObject(ds, "logit.model")
   ```