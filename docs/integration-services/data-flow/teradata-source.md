---
description: Stellen Sie eine Verbindung mit der Teradata-Quelle her.
title: Herstellen einer Verbindung mit der Teradata-Quelle | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d0170a81d73edef1e893a16df93e1bc004415739
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337809"
---
# <a name="connect-to-the-teradata-source"></a>Stellen Sie eine Verbindung mit der Teradata-Quelle her.

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Die Teradata-Quelle nutzt Folgendes, um Daten aus Teradata-Datenbanken zu extrahieren:
- Eine Tabelle oder Sicht.
- Die Ergebnisse einer SQL-Anweisung.

Die Quelle verwendet den Teradata-Verbindungs-Manager, um eine Verbindung mit der Teradata-Quelle herzustellen. Weitere Informationen finden Sie unter [Verwenden des Teradata-Verbindungs-Managers](teradata-connection-manager.md).

## <a name="troubleshoot-the-teradata-source"></a>Behandeln von Problemen mit der Teradata-Quelle

Sie können die Aufrufe der Teradata-Quelle an die TPT-API (Teradata Parallel Transporter) protokollieren. Aktivieren Sie hierzu die Paketprotokollierung, und wählen Sie dann auf Paketebene das Ereignis **Diagnose** aus.

Sie können die ODBC-Aufrufe (Open Database Connectivity) der Teradata-Quelle an den Teradata-ODBC-Treiber protokollieren, indem Sie die Ablaufverfolgung für den ODBC-Treiber-Manager aktivieren. Weitere Informationen finden Sie unter [Generieren einer ODBC-Ablaufverfolgung mit dem ODBC-Datenquellen-Administrator](../../odbc/admin/setting-tracing-options.md).

## <a name="parallelism"></a>Parallelität

Die Teradata-Quelle unterstützt Parallelität, wobei Exportaufträge auf die gleiche oder verschiedene Tabellen gleichzeitig zugreifen können. Eine Datenbankvariable namens `MaxLoadTasks` legt die Obergrenze der Anzahl gleichzeitig ausgeführter Exportaufträge fest. Sie können diese maximale Anzahl mithilfe der Variablen `MaxLoadTasks` festlegen.

## <a name="teradata-source-custom-properties"></a>Benutzerdefinierte Eigenschaften der Teradata-Quelle

Die benutzerdefinierten Eigenschaften der Teradata-Quelle sind in der folgenden Tabelle aufgeführt. Alle Eigenschaften weisen Lese-/Schreibzugriff auf.

|Eigenschaftenname|Datentyp|BESCHREIBUNG|
|:-|:-|:-|
|AccessMode|Ganze Zahl (Enumeration)|Der zum Zugreifen auf die Datenbank verwendete Modus. Die möglichen Werte sind *Tabellenname* und *SQL-Befehl*. Der Standardwert ist *Tabellenname*.|
|BlockSize|Integer|Die Blockgröße in Bytes, die beim Zurückgeben von Daten an den Client verwendet wird. Der Standardwert ist 1.048.576 MB (1 MB). Der Mindestwert ist 256 Bytes. Der Höchstwert ist 16.7751.68 Bytes.<br> Diese Eigenschaft befindet sich im Bereich **Erweiterter Editor**.|
|BufferMaxSize|Integer|Die maximale Gesamtgröße des von der GetBuffer-Funktion zurückgegebenen Datenpuffers. Diese Größe muss ausreichend sein, um mindestens eine Datenzeile, einschließlich Zeilenkopf, eigentlicher Datenzeile und Puffernachspann, aufzunehmen. Die standardmäßige maximale Gesamtgröße des Datenpuffers ist 16.775.552 Bytes. <br>Weitere Informationen finden Sie unter [Exportieren von Daten aus einer Teradata-Datenbank mithilfe von GetBuffer](https://docs.teradata.com/reader/TvVKKmxaBAoyETJZD8zz_g/oaxiwNJmnCa6UctY4k498w).|
|BufferMode|Boolean|Der Standardwert ist *True*. Wenn die PutBuffer-Funktion verwendet wird, muss der Wert *True* sein. Diese Eigenschaft befindet sich im Bereich **Erweiterter Editor**.|
|DataEncryption|Boolean|Der Standardwert ist *False*. Wenn der Wert *True* ist, wird die vollständige Sicherheitsverschlüsselung verwendet.|
|DefaultCodePage|Integer|Die zu verwendende Codepage, wenn die Datenquelle keine Informationen zur Codepage enthält. Diese Eigenschaft befindet sich im Bereich **Erweiterter Editor**.|
|DetailedTracingLevel|Ganze Zahl (Enumeration)|Wählen Sie für die erweiterte Ablaufverfolgung eine der folgenden Optionen aus: <br> *Off*: Keine erweiterte Protokollierung. <br> *General*: Bei dieser Ablaufverfolgung werden treiberspezifische Aktivitäten protokolliert. <br> *CLI*: Bei dieser Ablaufverfolgung werden CLIv2-spezifische Aktivitäten protokolliert. <br> *Notify Method*: Aktivitäten im Zusammenhang mit der Benachrichtigung werden protokolliert. <br> *Common Library*: Die Ablaufverfolgung von Aktivitäten in der opCommon-Bibliothek wird protokolliert. <br> *All*: Die gesamte vorherige Ablaufverfolgung von Aktivitäten wird protokolliert. <br> Die Protokolldatei mit der erweiterten Ablaufverfolgung wird in der `DetailedTracingFile`-Eigenschaft definiert. <br> Die `DetailedTracingFile`-Eigenschaft muss festgelegt werden, wenn die Option nicht *Off* ist. Diese Eigenschaft befindet sich im Bereich **Erweiterter Editor**.|
|DetailedTracingFile|String|Der Pfad der Protokolldatei, die automatisch erzeugt wird, wenn *DetailedTracingLevel* nicht *Off* ist. Diese Eigenschaft befindet sich im Bereich **Erweiterter Editor**.|
|DiscardLargeRow|Boolean|Der Standardwert ist *False*. Große Zeilen (mit mehr als 64 KB) werden verworfen, wenn der Wert *True* ist.|
|ExtendedStringColumnsAllocation|Boolean|*Maximal Transfer Character Allocation Factor* wird verwendet, wenn der Wert *True* ist. <br> Dieser Wert muss auf *True* festgelegt werden, wenn die Eigenschaft `Export Width Table ID` der Teradata-Datenbank auf *Maximal Defaults* festgelegt ist. <br> Der Standardwert ist *False*.|
|JobMaxRowSize|Integer|Die maximale Zeilenlänge kann unterstützt werden. Dieser Wert wird benötigt, wenn der `DiscardLargeRow`-Wert *True* ist.<br>Gültige Werte: <br>*64* (Standardwert): Zeilenlängen von 2 Bytes können unterstützt werden. <br>*1024*: Zeilenlängen von 4 Bytes können unterstützt werden.|
|MaxSessions|Integer|Die maximale Anzahl der angemeldeten Sitzungen. Dieser Wert muss größer als 0 (null) sein. Der Standardwert ist eine Sitzung für jede verfügbare AMP-Instanz (Access Module Processor).|
|MinSessions|Integer|Die minimale Anzahl der angemeldeten Sitzungen. Dieser Wert muss größer als 0 (null) sein. Der Standardwert ist eine Sitzung für jede verfügbare AMP-Instanz.|
|QueryBandSessInfo|Varchar|Ein benutzerdefinierter, sitzungsbasierter Abfragebandausdruck im Format einer Verbindungszeichenfolge. Sie verwenden diese Eigenschaft für die Überwachung und Steuerung von Rückbuchungen. Diese Eigenschaft befindet sich im Bereich **Erweiterter Editor**.|
|SpoolMode|Varchar|Gültige Werte sind: <br>*Spool*: Verwenden Sie den Standardwert *Spool*. <br> *NoSpool*: Verwenden Sie *Spool* nicht. Dieser Wert ist nur gültig, wenn der Datenbankserver *NoSpool* unterstützt. <br>  *NoSpoolOnly*: Verwenden Sie *Spool* auf keinen Fall. Der Auftrag wird mit einem Fehler beendet, wenn der Datenbankserver *NoSpool* nicht unterstützt.|
|SqlCommand|String|Der SQL-Befehl, der ausgeführt werden soll, wenn `AccessMode` auf *SQL Command* festgelegt wird.|
|TableName|String|Der Name der Tabelle mit den Daten, die verwendet werden, wenn `AccessMode` auf *Tabellenname* festgelegt wird.|
|TenacityHours|Integer|Die Anzahl der Stunden, die der TPT-Treiber versucht, sich anzumelden, wenn die maximale Anzahl von Lade-/Exportvorgängen bereits läuft. Der Standardwert ist *4 Stunden*. Diese Eigenschaft befindet sich im Bereich **Erweiterter Editor**.|
|TenacitySleep|Integer|Die Anzahl der Minuten, die der TPT-Treiber bei Erreichen des Grenzwerts anhält, bevor er versucht, sich anzumelden. Der Grenzwert wird von den Eigenschaften `MaxSessions` und `TenacityHours` definiert. Der Standardwert ist 6 Minuten. Diese Eigenschaft befindet sich im Bereich **Erweiterter Editor**.|
|UnicodePassThrough|Boolean|*Off* (Standardwert): Deaktivieren Sie Unicode-Pass-Through. <br>*Am:* Aktivieren Sie Unicode-Pass-Through.|

## <a name="configure-the-teradata-source"></a>Konfigurieren der Teradata-Quelle

Sie können die Teradata-Quelle programmgesteuert oder mithilfe des SSIS-Designers (SQL Server Integration Services) konfigurieren.

Der Bereich **Teradata-Quell-Editor** ist in der folgenden Abbildung dargestellt. Weitere Informationen finden Sie in den folgenden Abschnitten des Teradata-Quell-Editors:

- [Der Bereich „Verbindungs-Manager“](#the-connection-manager-pane)
- [Der Bereich „Spalten“](#the-columns-pane)
- [Der Bereich „Fehlerausgabe“](#the-error-output-pane)

![Teradata-Quell-Editor](media/teradata-source.png)

Der Bereich **Erweiterter Editor** enthält Eigenschaften, die programmgesteuert festgelegt werden können. So öffnen Sie den Bereich
- Klicken Sie auf der Seite **Datenfluss** des Integration Services-Projekts mit der rechten Maustaste auf die Teradata-Quelle, und wählen Sie **Erweiterten Editor anzeigen** aus.

Weitere Informationen zu den Eigenschaften, die Sie im Bereich **Erweiterter Editor** festlegen können, finden Sie unter [Benutzerdefinierte Eigenschaften der Teradata-Quelle](#teradata-source-custom-properties).

## <a name="the-connection-manager-pane"></a>Der Bereich „Verbindungs-Manager“

Wählen Sie im Bereich **Verbindungs-Manager** die Instanz des Teradata-Verbindungs-Managers für die Quelle aus. In diesem Bereich können Sie auch eine Tabelle oder Sicht in der Datenbank auswählen. So öffnen Sie den Bereich

1. Öffnen Sie in SQL Server Data Tools das SSIS-Paket mit der Teradata-Quelle.

1. Doppelklicken Sie auf der Registerkarte **Datenfluss** auf die Teradata-Quelle.

1. Wählen Sie im Teradata-Quell-Editor die Registerkarte **Verbindungs-Manager** aus.

### <a name="options"></a>Tastatur

**Connection manager**

* Wählen Sie in der Liste einen vorhandenen Verbindungs-Manager aus, oder klicken Sie auf **Neu**, um eine neue Instanz des Teradata-Verbindungs-Managers zu erstellen.

**Neu**

* Wählen Sie **Neu** aus. Der Bereich **Teradata-Verbindungs-Manager-Editor** wird geöffnet. Erstellen Sie in diesem Bereich einen neuen Verbindungs-Manager.

**Datenzugriffsmodus**

* Wählen Sie die Methode zum Auswählen von Daten aus der Quelle. Die Optionen sind in der folgenden Tabelle aufgeführt:

    |Option|BESCHREIBUNG|
    |:-|:-|
    |Tabellenname – TPT-Export|Ruft Daten aus einer Tabelle oder Sicht in der Teradata-Datenquelle ab. Wenn diese Option ausgewählt ist, wählen Sie in der Liste eine verfügbare Tabelle oder Sicht für **Name der Tabelle oder Sicht** aus.|
    |SQL-Befehl – TPT-Export|Ruft mit einer SQL-Abfrage Daten aus der Teradata-Datenquelle ab. Bei Auswahl dieser Option geben Sie anhand einer der folgenden Methoden eine Abfrage ein: <ul><li>Geben Sie den Text der SQL-Abfrage im Feld **SQL-Befehlstext** ein.</li><li>Wählen Sie **Durchsuchen** aus, um die SQL-Abfrage aus einer Textdatei zu laden.</li><li>Wählen Sie **Abfrage analysieren** aus, um die Syntax des Abfragetexts zu überprüfen.</li></ul>|

**Vorschau**

* Wählen **Vorschau** aus, um (max.) die ersten 200 Zeilen der Daten anzuzeigen, die aus der ausgewählten Tabelle bzw. Sicht extrahiert wurden.

## <a name="the-columns-pane"></a>Der Bereich „Spalten“

Im Bereich **Spalten** können Sie jeder externen Spalte (Quellspalte) eine Ausgabespalte zuordnen. So öffnen Sie den Bereich

1. Öffnen Sie in SQL Server Data Tools das SSIS-Paket mit der Teradata-Quelle.

1. Doppelklicken Sie auf der Registerkarte **Datenfluss** auf die Teradata-Quelle.

1. Wählen Sie im Teradata-Quell-Editor die Registerkarte **Spalten** aus.

### <a name="options"></a>Tastatur

**Verfügbare externe Spalten**

In dieser Tabelle sind die verfügbaren externen Spalten aufgeführt, die Sie auswählen und der Liste **Externe Spalte** hinzufügen können. Sie können die Spalten in der von Ihnen gewünschten Reihenfolge auflisten. In dieser Tabelle können Spalten weder hinzugefügt noch gelöscht werden.

* Aktivieren Sie das Kontrollkästchen **Alle auswählen**, um alle Spalten auszuwählen.

**Externe Spalten**

Die externen Spalten (Quellspalten), die Sie ausgewählt haben, sind in Reihenfolge aufgelistet. Um die Reihenfolge zu ändern, löschen Sie zuerst die Liste **Verfügbare externe Spalten**, und wählen Sie dann die Spalten in einer anderen Reihenfolge aus.

**Ausgabespalte**

Obwohl der Name der ausgewählten externen (Quell-)Spalte der Standardausgabename ist, können Sie einen beliebigen eindeutigen Namen eingeben.

>[!NOTE]
>>Wenn es Spalten gibt, die nicht unterstützte Datentypen enthalten, erhalten Sie eine Warnung, in der diese nicht unterstützten Datentypen angezeigt werden. Die entsprechenden Spalten werden dann aus den Zuordnungsspalten entfernt.

## <a name="the-error-output-pane"></a>Der Bereich „Fehlerausgabe“

Im Bereich **Fehlerausgabe** können Sie Optionen für die Fehlerbehandlung auswählen. So öffnen Sie den Bereich

1. Öffnen Sie in SQL Server Data Tools das SSIS-Paket mit der Teradata-Quelle.

1. Doppelklicken Sie auf der Registerkarte **Datenfluss** auf die Teradata-Quelle.

1. Wählen Sie im Teradata-Quell-Editor die Registerkarte **Fehlerausgabe** aus.

### <a name="options"></a>Tastatur

**Fehlerverhalten**

* Wählen Sie aus, wie die Teradata-Quelle Fehler in einem Flow behandeln soll: 
  * Den Fehler ignorieren
  * Die Zeile umleiten
  * Die Komponente mit einem Fehler abbrechen

**Verwandte Themen:** Weitere Informationen finden Sie unter [Fehlerbehandlung in Daten](error-handling-in-data.md).

**Abschneiden**

* Wählen Sie aus, wie die Teradata-Quelle Abschneidevorgänge in einem Flow behandeln soll: 
  * Den Fehler ignorieren
  * Die Zeile umleiten
  * Die Komponente mit einem Fehler abbrechen

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren Sie das [Teradata-Ziel](teradata-destination.md).
- Wenn Sie Fragen haben, besuchen Sie die [Tech Community](https://aka.ms/AA6iwdw).