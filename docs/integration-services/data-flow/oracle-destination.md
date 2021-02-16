---
description: Oracle-Ziel
title: Oracle-Ziel | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3b28d420d66e55565c882c9fa2ae41f86e1c0764
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343236"
---
# <a name="oracle-destination"></a>Oracle-Ziel

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Das Massenladen in das Oracle-Ziel lädt Daten in die Oracle-Datenbank.

Das Ziel verwendet den Oracle-Verbindungs-Manager, um eine Verbindung mit einer Datenquelle herzustellen. Weitere Informationen finden Sie unter [Oracle-Verbindungs-Manager](oracle-connection-manager.md).

Ein Oracle-Ziel enthält Zuordnungen zwischen Eingabespalten und Spalten in der Zieldatenquelle. Sie müssen nicht allen Zielspalten Eingabespalten zuordnen. In Abhängigkeit von den Eigenschaften der Zielspalten können jedoch Fehler auftreten, falls den Zielspalten keine Eingabespalten zugeordnet sind. Wenn eine Zielspalte z. B. keine NULL-Werte zulässt, muss dieser Spalte eine Eingabespalte zugeordnet werden. Außerdem tritt zur Runtime ein Fehler auf, wenn die Eingabedaten mit dem Typ der Zielspalte nicht kompatibel sind. Je nach Einstellung des Fehlerverhaltens wird der Fehler ignoriert, ein Fehler verursacht oder die Zeile an die Fehlerausgabe umgeleitet.

Das Oracle-Ziel weist eine reguläre Eingabe und eine Fehlerausgabe auf.

Spalten mit nicht unterstützten Datentypen werden vor der Zuordnung mit einer Warnung aus Spalten gelöscht.
Weitere Informationen finden Sie unter [Datentypunterstützung bei Microsoft Connector für Oracle](oracle-data-type-support.md).

## <a name="load-options"></a>Ladeoptionen

Zwei Zugriffslademodi werden unterstützt. Der Modus kann im [Editor für Oracle-Ziel (Seite „Verbindungs-Manager“)](#oracle-destination-editor-connection-manager-page) festgelegt werden. Die beiden Modi sind:

- Batchladevorgang: In diesem Modus werden Daten in Batches in eine Oracle-Tabelle geladen, und der gesamte Batch wird in derselben Transaktion eingefügt.
Ausführliche Informationen zum Konfigurieren dieses Modus finden Sie unter [Editor für Oracle-Ziel (Seite „Verbindungs-Manager“)](#oracle-destination-editor-connection-manager-page) und [Benutzerdefinierte Eigenschaften von Oracle-Zielen](#oracle-destination-custom-properties).

- Schnelles Laden mithilfe von direktem Pfad: Hier wird der Modus des Treibers mit direktem Pfad zum Laden der Oracle-Tabelle verwendet. Es gibt Einschränkungen bei der Verwendung dieses Modus. Weitere Informationen finden Sie in der Oracle-Dokumentation.  
Ausführliche Informationen zum Konfigurieren dieses Modus finden Sie unter [Editor für Oracle-Ziel (Seite „Verbindungs-Manager“)](#oracle-destination-editor-connection-manager-page) und [Benutzerdefinierte Eigenschaften von Oracle-Zielen](#oracle-destination-custom-properties).

## <a name="error-handling"></a>Fehlerbehandlung

Das Oracle-Ziel verfügt über eine Fehlerausgabe. Die Komponentenfehlerausgabe enthält die folgenden Ausgabespalten:

- **Fehlercode**: Eine Zahl, die den Fehlertyp des aktuellen Fehlers darstellt. Als Absender des Fehlercodes kommen infrage:
    - Oracle-Server. Weitere Informationen finden Sie in der detaillierten Fehlerbeschreibung der Dokumentation zur Oracle-Datenbank.
    - SSIS-Runtime. Eine Liste der SSIS-Fehlercodes finden Sie in der SSIS-Fehler- und Meldungsreferenz.
- **Fehlerspalte**: Die Quellspaltennummer, die den Konvertierungsfehler verursacht hat.

- **Fehlerdatenspalten**: Die Daten, die den Fehler verursachen.

Typen von Ausgabefehlern beim unterstützten Ladevorgang sind: Datenkonvertierung, Abschneiden oder Einschränkungsverletzung usw. Siehe [Editor für Oracle-Ziel (Seite „Fehlerausgabe“)](#oracle-destination-editor-error-output-page).

Die **Maximale Anzahl von Fehlern (MaxErrors)**-Eigenschaft legt die maximale Anzahl von Fehlern fest, die auftreten können. Die Ausführung wird beendet und es werden Fehler zurückgegeben, wenn die maximale Anzahl erreicht ist. Nur vor Erreichen der maximalen Fehlerzahl ausgeführte Datensätze werden in die Zieltabelle einbezogen. Informationen zur detaillierten Konfiguration finden Sie unter [Editor für Oracle-Ziel (Seite „Verbindungs-Manager“)](#oracle-destination-editor-connection-manager-page).

## <a name="parallelism"></a>Parallelität

Im Batchlademodus gibt es keine Einschränkung für die Konfiguration paralleler Ausführungen, aber die Leistung kann durch den standardmäßigen Datensatz-Sperrmechanismus beeinträchtigt werden. Die Höhe des Leistungsverlustes hängt von den Daten und der Organisation der Tabelle ab.

Im Protokoll für den direkten Pfad (schnelles Laden) können nicht mehrere Oracle-Ziele zur gleichzeitigen Ausführung für dieselbe Tabelle konfiguriert werden, jedoch kann der parallele Modus verwendet werden.

Ein paralleler direkter Pfad ermöglicht mehrere Ladevorgänge über den direkten Pfad, wobei die gleichzeitige Ausführung mehrerer Oracle-Ziele für dieselbe Tabelle konfiguriert werden kann. Oracle sperrt die Zieltabelle nicht exklusiv für die Verwendung in der Sitzung für schnelles Laden, sodass zusätzliche Zielkomponenten für schnelles Laden ausgeführt werden können, um dieselbe Zieltabelle parallel zu laden.
Der parallele direkte Pfad ist restriktiver, und jede Verwendung der Parallelität sollte im Voraus geplant werden.

Es gibt keinen Grund, eine einzelne parallele Sitzung zu verwenden.

Informationen zu Einschränkungen bei der Verwendung paralleler direkter Pfade finden Sie in der Oracle-Dokumentation.

Weitere Informationen finden Sie unter [Benutzerdefinierte Eigenschaften von Oracle-Zielen](#oracle-destination-custom-properties).

## <a name="troubleshooting-the-oracle-destination"></a>Problembehandlung des Oracle-Ziels

Sie können die ODBC-Aufrufe protokollieren, die die Oracle-Quelle an Oracle-Datenquellen richtet, um Probleme beim Exportieren von Daten zu beheben. Aktivieren Sie die Ablaufverfolgung für den ODBC-Treiber-Manager, um die ODBC-Aufrufe zu protokollieren, die von der Oracle-Quelle an Oracle-Datenquellen gesendet werden. Weitere Informationen finden Sie in der Microsoft-Dokumentation unter *Generieren einer ODBC-Ablaufverfolgung mit dem ODBC-Datenquellen-Administrator*.

## <a name="oracle-destination-custom-properties"></a>Benutzerdefinierte Eigenschaften von Oracle-Zielen

In der folgenden Tabelle werden die benutzerdefinierten Eigenschaften des Oracle-Ziels beschrieben. Alle Eigenschaften weisen Lese-/Schreibzugriff auf.

|Eigenschaftenname|Datentyp|BESCHREIBUNG|Lademodus|
|:-|:-|:-|:-|
|BatchSize|Integer|Die Größe des Batches für den Massenladevorgang. Dies ist die Anzahl der Zeilen, die als Batch geladen werden.|Wird nur im Batchmodus verwendet.|
|DefaultCodePage|Integer|Die zu verwendende Codepage, wenn die Datenquelle keine Codepageinformationen enthält. <br>**Hinweis**: Diese Eigenschaft wird nur vom **Erweiterten Editor** festgelegt.|Verwendung für beide Modi.|
|FastLoad|Boolean|Gibt an, ob das schnelle Laden verwendet wird. Der Standardwert ist **false**. Der Wert kann auch im [Editor für Oracle-Ziel (Seite „Verbindungs-Manager“)](#oracle-destination-editor-connection-manager-page) festgelegt werden. |Verwendung für beide Modi.|
|MaxErrors|Integer|Die Anzahl der Fehler, die auftreten können, bevor der Datenfluss abgebrochen wird. Der Standardwert ist **0**(null). Dies bedeutet, dass die Fehlerzahl nicht begrenzt ist.<br> Wenn **Fluss umleiten** auf der Seite **Fehlerbehandlung** ausgewählt ist. Bevor der Fehlerzahl-Grenzwert erreicht wird, werden alle Fehler in der Fehlerausgabe zurückgegeben. Weitere Informationen finden Sie unter [Fehlerbehandlung](#error-handling).|Verwendung nur im Modus für schnelles Laden.|
|NoLogging|Boolean|Gibt an, ob die Datenbankprotokollierung deaktiviert ist. Der Standardwert ist **False**, was bedeutet, dass die Protokollierung aktiviert ist.|Verwendung für beide Modi.|
|Parallel|Boolean|Gibt an, ob paralleles Laden zulässig ist. **True** gibt an, dass andere Ladesitzungen für dieselbe Zieltabelle ausgeführt werden dürfen.<br> Weitere Informationen finden Sie unter [Parallelität](#parallelism).|Verwendung nur im Modus für schnelles Laden.|
|TableName|String|Der Name der Tabelle mit den Daten, der verwendet werden.|Verwenden Sie ihn für beide Modi.|
|TableSubName|String|Der untergeordnete Name oder die untergeordnete Partition. Dieser Wert ist optional.<br> **Hinweis**: Diese Eigenschaft kann nur im **Erweiterten Editor** festgelegt werden.|Verwendung nur im Modus für schnelles Laden.|
|TransactionSize|Integer|Die Anzahl von Einfügungen, die in einer einzelnen Transaktion möglich sind. Der Standardwert ist **BatchSize**.|Wird nur im Batchmodus verwendet.|
|TransferBufferSize|Integer|Die Größe des Übertragungspuffers. Der Standardwert ist 64 KB.|Verwendung nur im Modus für schnelles Laden.|

## <a name="configuring-the-oracle-destination"></a>Konfigurieren des Oracle-Ziels

Sie können das Oracle-Ziel programmgesteuert oder mit dem SSIS-Designer konfigurieren.

Der Editor für Oracle-Ziel wird in der folgenden Abbildung dargestellt. Er enthält eine Verbindungs-Manager-Seite, Zuordnungsseite und Fehlerausgabeseite.

Weitere Informationen finden Sie in einem der folgenden Abschnitte:

- [Editor für Oracle-Ziel (Seite „Verbindungs-Manager“)](#oracle-destination-editor-connection-manager-page)
- [Editor für Oracle-Ziel (Seite „Zuordnungen“)](#oracle-destination-editor-mappings-page)
- [Editor für Oracle-Ziel (Seite „Fehlerausgabe“)](#oracle-destination-editor-error-output-page)

![Oracle-Ziel](media/oracle-destination.png)

Das Dialogfeld **Erweiterter Editor** enthält die Eigenschaften, die programmgesteuert festgelegt werden können.
So öffnen Sie das Dialogfeld **Erweiterter Editor** :

- Klicken Sie auf dem Bildschirm **Datenfluss** des Integration Services-Projekts mit der rechten Maustaste auf das Oracle-Ziel, und wählen Sie **Erweiterten Editor anzeigen** aus.

Weitere Informationen zu den Eigenschaften, die Sie im Dialogfeld „Erweiterter Editor“ festlegen können, finden Sie unter [Benutzerdefinierte Eigenschaften von Oracle-Zielen](#oracle-destination-custom-properties).

## <a name="oracle-destination-editor-connection-manager-page"></a>Editor für Oracle-Ziel (Seite „Verbindungs-Manager“)

Auf der Seite **Verbindungs-Manager** des Dialogfelds **Editor für Oracle-Ziel** können Sie den Oracle-Verbindungs-Manager für das Ziel auswählen. Außerdem können Sie auf dieser Seite eine Tabelle oder Sicht aus der Datenbank auswählen.

**So öffnen Sie die Seite „Verbindungs-Manager“ des Editors für Oracle-Ziel**

- Öffnen Sie in SQL Server Data Tools das SQL Server Integration Services-Paket (SSIS), das über das Oracle-Ziel verfügt.

- Doppelklicken Sie auf der Registerkarte „Datenfluss“ auf das ODBC-Ziel.

- Klicken Sie im Editor für Oracle-Ziel auf „Verbindungs-Manager“.

### <a name="options"></a>Tastatur

**Connection manager**

Wählen Sie einen vorhandenen Verbindungs-Manager aus der Liste aus, oder klicken Sie auf **Neu**, um einen neuen Oracle-Verbindungs-Manager zu erstellen.

**Neu**

Klicken Sie auf **Neu**. Das Dialogfeld **Oracle-Verbindungs-Manager-Editor**, in dem Sie einen neuen Verbindungs-Manager erstellen können, wird geöffnet.

**Datenzugriffsmodus**

Wählen Sie die Methode für die Auswahl von Daten aus der Quelle aus. Die Optionen sind in der folgenden Tabelle aufgeführt:

|Option|BESCHREIBUNG|
|:-|:-|
|Tabellenname|Konfigurieren Sie das Oracle-Ziel, um im Batchmodus zu arbeiten. Optionen:<br><br> **Name der Tabelle oder Sicht**: Wählen Sie in der Liste eine verfügbare Tabelle oder Sicht in der Datenbank aus.<br><br> **Transaktionsgröße**: Anzahl von Einfügungen, die in einer einzelnen Transaktion möglich sind. Der Standardwert ist **BatchSize**.<br><br> **Batchgröße**: Geben Sie die Größe des Batches (Anzahl der geladenen Zeilen) für das Massenladen ein.
|Tabellenname – schnelles Laden|Konfigurieren Sie das Oracle-Ziel für den Schnelllademodus (direkter Pfad). <br><br>Diese Optionen stehen zur Verfügung:<br><br> **Name der Tabelle oder Sicht**: Wählen Sie in der Liste eine verfügbare Tabelle oder Sicht in der Datenbank aus.<br><br> **Paralleles Laden**: Gibt an, ob paralleles Laden aktiviert ist. Weitere Informationen finden Sie unter [Parallelität](#parallelism).<br><br> **Keine Protokollierung**: Dieses Kontrollkästchen dient zum Deaktivieren der Datenbankprotokollierung. Diese Protokollierung ist eine Oracle-Datenbank, die für Wiederherstellungszwecke verwendet wird und nicht mit der Ablaufverfolgung in Zusammenhang steht.<br><br> **Maximale Anzahl von Fehlern**: Die maximale Anzahl der Fehler, die auftreten können, bevor der Datenfluss abgebrochen wird. Der Standardwert ist 0 (null). Dies bedeutet, dass die Fehlerzahl nicht begrenzt ist.<br><br> Alle Fehler, die auftreten können, werden in der Fehlerausgabe zurückgegeben.<br><br> **Übertragungspuffergröße (KB)** : Geben Sie die Größe des Übertragungspuffers ein. Die Standardgröße ist 64 KB.|

**Vorhandene Daten anzeigen**

Klicken Sie auf **Vorhandene Daten anzeigen**, um die ersten 200 Datenzeilen für die ausgewählte Tabelle anzuzeigen.

## <a name="oracle-destination-editor-mappings-page"></a>Editor für Oracle-Ziel (Seite „Zuordnungen“)

Auf der Seite **Zuordnungen** des Dialogfelds **Editor für Oracle-Ziel** können Sie eine Zuordnung von Eingabe- zu Zielspalten vornehmen.

**So öffnen Sie die Seite „Zuordnungen“ im Editor für Oracle-Ziel**

- Öffnen Sie in SQL Server Data Tools das SQL Server Integration Services-Paket (SSIS), das über das Oracle-Ziel verfügt.

- Doppelklicken Sie auf der Registerkarte „Datenfluss“ auf das ODBC-Ziel.

- Klicken Sie im Editor für Oracle-Ziel auf „Zuordnungen“.

### <a name="options"></a>Tastatur

**Verfügbare Eingabespalten**

Die Liste der verfügbaren Eingabespalten. Ordnen Sie die Eingabespalten per Drag & Drop den verfügbaren Zielspalten zu.

**Verfügbare Zielspalten**

Die Liste der verfügbaren Zielspalten. Ordnen Sie die Zielspalten per Drag & Drop den verfügbaren Eingabespalten zu.

**Eingabespalte**

Zeigt die von Ihnen ausgewählten Eingabespalten an. Sie können Zuordnungen entfernen, indem Sie **< ignore >** auswählen, um Spalten aus der Ausgabe auszuschließen.

**Zielspalte**

Zeigt alle verfügbaren Zielspalten an, sowohl die zugeordneten als auch die nicht zugeordneten.

> [!NOTE]
>
>Spalten mit nicht unterstützten Datentypen werden mit einer Warnung aus der Zuordnung gelöscht.

## <a name="oracle-destination-editor-error-output-page"></a>Editor für Oracle-Ziel (Seite „Fehlerausgabe“)

Auf der Seite „Fehlerausgabe“ des Dialogfelds „Editor für Oracle-Ziel“ können Sie Optionen für die Fehlerbehandlung auswählen.

**So öffnen Sie die Seite „Fehlerausgabe“ des Editors für Oracle-Ziel**

- Öffnen Sie in SQL Server Data Tools das SQL Server Integration Services-Paket (SSIS), das über das Oracle-Ziel verfügt.

- Doppelklicken Sie auf der Registerkarte „Datenfluss“ auf das ODBC-Ziel.

- Klicken Sie im „Editor für Oracle-Ziel“ auf „Fehlerausgabe“.

### <a name="options"></a>Tastatur

**Fehlerverhalten**

Wählen Sie aus, wie die Oracle-Quelle Fehler in einem Fluss behandeln soll: Fehler ignorieren, Zeile umleiten oder Komponente mit einem Fehler abbrechen.
**Verwandter Abschnitt**: [Fehlerbehandlung in Daten](./error-handling-in-data.md)

**Abschneiden**

Wählen Sie aus, wie die Oracle-Quelle das Abschneiden in einem Fluss behandeln soll: Fehler ignorieren, Zeile umleiten oder Komponente mit einem Fehler abbrechen.

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren Sie den [Oracle-Verbindungs-Manager](oracle-connection-manager.md).
- Konfigurieren Sie die [Oracle-Quelle](oracle-source.md).
- Konfigurieren Sie das [Oracle-Ziel](oracle-destination.md).
- Wenn Sie Fragen haben, besuchen Sie die [TechCommunity](https://aka.ms/AA5u35j).
