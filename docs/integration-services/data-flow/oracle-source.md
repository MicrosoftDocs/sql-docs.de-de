---
description: Oracle-Quelle
title: Oracle-Quelle
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6d26daafb62777091cb962332d47d8c00620e4d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343198"
---
# <a name="oracle-source"></a>Oracle-Quelle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Die Oracle-Quelle extrahiert mit den folgenden Modi Daten aus Oracle Database:

- Eine Tabelle oder Sicht.

- Die Ergebnisse einer SQL-Anweisung.

Die Quelle verwendet einen Oracle-Verbindungs-Manager, um eine Verbindung mit der Oracle-Quelle herzustellen. Weitere Informationen finden Sie unter [Oracle-Verbindungs-Manager](oracle-connection-manager.md).

## <a name="error-output"></a>Fehlerausgabe

Die Fehlerausgabe umfasst die folgenden Spalten:  

- **Fehlercode**: Eine Zahl, die den Fehlertyp des aktuellen Fehlers darstellt. Als Absender des Fehlercodes kommen infrage:
    - Oracle-Server. Weitere Informationen finden Sie in der detaillierten Fehlerbeschreibung der Dokumentation zur Oracle-Datenbank.
    - SSIS-Runtime. Eine Liste der SSIS-Fehlercodes finden Sie in der SSIS-Fehler- und Meldungsreferenz.
- **Fehlerspalte**: Die Quellspaltennummer, die den Konvertierungsfehler verursacht hat.

- **Fehlerdatenspalten**: Die Daten, die den Fehler verursacht haben.

Die Oracle-Quelle gibt in der Fehlerausgabe während des Lade- und Extraktionsprozesses aufgetretene Fehler zurück. Weitere Informationen finden Sie unter [Editor für Oracle-Quelle (Fehlerausgabeseite)](#oracle-source-editor-error-output-page).

## <a name="troubleshooting-the-oracle-source"></a>Problembehandlung der Oracle-Quelle

Sie können die ODBC-Aufrufe protokollieren, die die Oracle-Quelle an Oracle-Datenquellen richtet, um Probleme beim Exportieren von Daten zu beheben. Aktivieren Sie die Ablaufverfolgung für den ODBC-Treiber-Manager, um die ODBC-Aufrufe zu protokollieren, die von der Oracle-Quelle an Oracle-Datenquellen gesendet werden. Weitere Informationen finden Sie in der Microsoft-Dokumentation unter *Generieren einer ODBC-Ablaufverfolgung mit dem ODBC-Datenquellen-Administrator*.

## <a name="oracle-source-custom-properties"></a>Benutzerdefinierte Eigenschaften der Oracle-Quelle

Die benutzerdefinierten Eigenschaften der Oracle-Quelle wie unten beschrieben. Alle Eigenschaften weisen Lese-/Schreibzugriff auf.

|Eigenschaftenname|Datentyp|BESCHREIBUNG|
|:-|:-|:-|
|AccessMode|Ganze Zahl (Enumeration)|Der zum Zugreifen auf die Datenbank verwendete Modus. Die möglichen Werte sind **Tabellenname** und **SQL-Befehl**. Die Standardeinstellung ist der **Tabellenname**.|
|BatchSize|Integer|Die Größe des Batches für den Massenladevorgang. Dies ist die Anzahl der als Array extrahierten Datensätze. <br>Diese Eigenschaft wird nur vom **Erweiterten Editor** festgelegt.|
|DefaultCodePage|Integer|Die zu verwendende Codepage, wenn die Datenquelle keine Codepageinformationen enthält. <br>Diese Eigenschaft wird nur vom **Erweiterten Editor** festgelegt.|
|PreFetchCount|Integer|Die Anzahl der vorab abgerufenen Zeilen <br>Diese Eigenschaft wird nur vom **Erweiterten Editor** festgelegt.|
|SqlCommand|String|Der SQL-Befehl, der ausgeführt werden soll, wenn AccessMode auf SQL-Befehl festgelegt wird.|
|TableName|String|Der Name der Tabelle mit den Daten, die verwendet werden, wenn AccessMode auf „Tabellenname“ festgelegt wird.|

## <a name="configuring-the-oracle-source"></a>Konfigurieren der Oracle-Quelle

Sie können die Oracle-Quelle programmgesteuert oder mit dem SSIS-Designer konfigurieren.

Der Editor für die Oracle-Quelle wird in der folgenden Abbildung dargestellt. Er enthält eine Verbindungs-Manager-Seite, Spaltenseite und Fehlerausgabeseite.

Weitere Informationen finden Sie in einem der folgenden Abschnitte:

- [Editor für Oracle-Quelle (Verbindungs-Manager-Seite)](#oracle-source-editor-connection-manager-page)
- [Editor für Oracle-Quelle (Spaltenseite)](#oracle-source-editor-columns-page)
- [Editor für Oracle-Quelle (Fehlerausgabeseite)](#oracle-source-editor-error-output-page)

![Oracle-Quelle](media/oracle-source.png)

Das Dialogfeld **Erweiterter Editor** enthält die Eigenschaften, die programmgesteuert festgelegt werden können.

So öffnen Sie das Dialogfeld **Erweiterter Editor** :

- Klicken Sie auf dem Bildschirm **Datenfluss** des Integration Services-Projekts mit der rechten Maustaste auf die Oracle-Quelle, und wählen Sie **Erweiterten Editor anzeigen** aus.

Weitere Informationen zu den Eigenschaften, die Sie im Dialogfeld **Erweiterter Editor** festlegen können, finden Sie unter [Benutzerdefinierte Eigenschaften der Oracle-Quelle](#oracle-source-custom-properties).

## <a name="oracle-source-editor-connection-manager-page"></a>Editor für Oracle-Quelle (Verbindungs-Manager-Seite)

Auf der **Verbindungs-Manager-Seite** kann Oracle Database im Dialogfeld **Editor für Oracle-Quelle** als Quelle, Tabelle oder zur Ansicht ausgewählt werden.

**So öffnen Sie die Verbindungs-Manager-Seite des Editors für die Oracle-Quelle**

- Öffnen Sie in SQL Server Data Tools das SQL Server Integration Services-Paket (SSIS), das über die Oracle-Quelle verfügt.

- Doppelklicken Sie auf der Registerkarte „Datenfluss“ auf die Oracle-Quelle.
### <a name="options"></a>Tastatur

**Connection manager**

Wählen Sie einen vorhandenen Verbindungs-Manager aus der Liste aus, oder klicken Sie auf **Neu**, um einen neuen Oracle-Verbindungs-Manager zu erstellen.

**Neu**

Klicken Sie auf **Neu**. Das Dialogfeld **Oracle-Verbindungs-Manager-Editor**, in dem Sie einen neuen Verbindungs-Manager erstellen können, wird geöffnet.

**Datenzugriffsmodus**

Wählen Sie die Methode für die Auswahl von Daten aus der Quelle aus. Die Optionen sind in der folgenden Tabelle aufgeführt:

|Option|BESCHREIBUNG|
|:-|:-|
|Tabelle oder Sicht|Ruft Daten aus einer Tabelle oder Sicht in der Oracle-Datenquelle ab. Wenn diese Option ausgewählt ist, wählen Sie in der Liste eine verfügbare Tabelle oder Sicht für **Name der Tabelle oder Sicht** aus.|
|SQL-Befehl|Ruft mit einer SQL-Abfrage Daten aus der Oracle-Datenquelle ab. Bei Auswahl dieser Option geben Sie anhand einer der folgenden Methoden eine Abfrage ein: <br>Geben Sie den Text der SQL-Abfrage im Feld **SQL-Befehlstext** ein. <br>Klicken Sie auf **Durchsuchen** , um die SQL-Abfrage aus einer Textdatei zu laden. <br>Klicken Sie auf **Abfrage analysieren** , um die Syntax des Abfragetextes zu überprüfen.|

**Vorschau**

Klicken Sie auf **Vorschau** , um die ersten 200 Zeilen (max.) der Daten anzuzeigen, die aus der ausgewählten Tabelle bzw. Sicht extrahiert wurden.

## <a name="oracle-source-editor-columns-page"></a>Editor für Oracle-Quelle (Spaltenseite)

Auf der Seite **Spalten** des Dialogfelds **Editor für Oracle-Quelle** können Sie jeder externen Spalte (Quellspalte) eine Ausgabespalte zuordnen.

**So öffnen Sie die Spaltenseite des Editors für die Oracle-Quelle**

- Öffnen Sie in SQL Server Data Tools das SQL Server Integration Services-Paket (SSIS), das über die Oracle-Quelle verfügt.

- Doppelklicken Sie auf der Registerkarte „Datenfluss“ auf die Oracle-Quelle.

- Klicken Sie im „Editor für Oracle-Quelle“ auf „Spalten“.

### <a name="options"></a>Tastatur

**Verfügbare externe Spalten**

Eine Liste der verfügbaren externen Spalten, die ausgewählt werden können, um der Liste **Externe Spalten** hinzugefügt zu werden, in der Reihenfolge, in der sie ausgewählt sind. Mit der Tabelle können keine Spalten hinzugefügt oder gelöscht werden.

Aktivieren Sie das Kontrollkästchen **Alle auswählen**, um alle Spalten auszuwählen.

**Externe Spalten**

Die externen Spalten (Quellspalten), die Sie ausgewählt haben, sind in der Reihenfolge der Auswahl aufgelistet.
Um die Reihenfolge zu ändern, löschen Sie zuerst die Liste „Verfügbare externe Spalten“, und wählen Sie dann die Spalte(n) in einer anderen Reihenfolge aus.

**Ausgabespalte**

Der Name der ausgewählten externen Spalte (Quellspalte) ist der standardmäßige Ausgabename. Sie können einen beliebigen eindeutigen Namen eingeben.
> [!NOTE]
>
>Wenn Spalten mit nicht unterstützten Datentypen vorhanden sind, weist eine Warnung darauf hin, dass die Datentypen nicht unterstützt werden, und verwandte Spalten werden aus den Zuordnungsspalten entfernt.

## <a name="oracle-source-editor-error-output-page"></a>Editor für Oracle-Quelle (Fehlerausgabeseite)

Auf der Seite **Fehlerausgabe** des Dialogfelds **Editor für Oracle-Quelle** können Sie Optionen für die Fehlerbehandlung auswählen.

**So öffnen Sie die Seite „Fehlerausgabe“ des Editors für die Oracle-Quelle**

- Öffnen Sie in SQL Server Data Tools das SQL Server Integration Services-Paket (SSIS), das über die Oracle-Quelle verfügt.

- Doppelklicken Sie auf der Registerkarte „Datenfluss“ auf die Oracle-Quelle.

- Klicken Sie im Editor für die Oracle-Quelle auf „Fehlerausgabe“.

### <a name="options"></a>Tastatur

**Fehlerverhalten**

Wählen Sie aus, wie die Oracle-Quelle Fehler in einem Fluss behandeln soll: Fehler ignorieren, Zeile umleiten oder Komponente mit einem Fehler abbrechen.
**Verwandter Abschnitt**: [Fehlerbehandlung in Daten](./error-handling-in-data.md)

**Abschneiden**

Wählen Sie aus, wie die Oracle-Quelle das Abschneiden in einem Fluss behandeln soll: Fehler ignorieren, Zeile umleiten oder Komponente mit einem Fehler abbrechen.

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren Sie das [Oracle-Ziel](oracle-destination.md).
- Wenn Sie Fragen haben, besuchen Sie die [TechCommunity](https://aka.ms/AA5u35j).
