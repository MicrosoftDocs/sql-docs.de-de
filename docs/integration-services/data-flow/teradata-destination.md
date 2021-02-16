---
description: Teradata-Ziel
title: Teradata-Ziel | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 83453cfa36ae4b2e9f6596acc7206162c2b82b76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342269"
---
# <a name="teradata-destination"></a>Teradata-Ziel

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Das Teradata-Ziel führt ein Massenladen von Daten in eine Teradata-Datenbank aus.

Das Ziel verwendet den Teradata-Verbindungs-Manager, um eine Verbindung mit einer Datenquelle herzustellen. Weitere Informationen finden Sie unter [Verwenden des Teradata-Verbindungs-Managers](teradata-connection-manager.md).

## <a name="load-options"></a>Ladeoptionen

Das Teradata-Ziel unterstützt zwei Modi zum Laden von Daten:

- **TPT-Stream**: In diesem Modus wird der TPT-API-Stream-Operator (Teradata TPump-Protokoll) verwendet.

- **TPT-Ladevorgang** (schnelles Massenladen): In diesem Modus wird der TPT-API-Ladeoperator (Teradata-FastLoad-Protokoll) für schnelles Massenladen verwendet.

Der Modus für schnelles Laden unterliegt den folgenden Einschränkungen:

- Die Sitzungsbeschränkung für die Teradata-Datenbank wird durch den zuerst auftretenden der folgenden Faktoren festgelegt:
    - Mithilfe des Befehls SESSIONS festgelegte Sitzungslimits
    - Das Teradata-Datenbanklimit von einer Sitzung pro AMP-Instanz
    - Die Plattformbeschränkung für die maximale Anzahl von Sitzungen pro Anwendung: Definiert durch die Variable MaxSess in der Softwaredatei „CLISPB.DAT“ der Kommunikationsprozessorschnittstelle (Communications Processor, COP). Sie können den Befehl TDP SET MAXSESSIONS verwenden, um eine Plattformbeschränkung anzugeben. Der Standardgrenzwert entspricht der MAXSESS-Einstellung für den Server.

- Die Verknüpfung von Indizes wird nicht unterstützt.
- Fremdschlüsselverweise in Zieltabellen werden nicht unterstützt.
- Mit einem sekundären Index definierte Zieltabellen werden nicht unterstützt.

Weitere Informationen zu den Einschränkungen für das schnelle Laden von Teradata finden Sie in der Teradata-Referenz für schnelles Laden.

Sie können den Modus im [Teradata-Ziel-Editor (Seite „Verbindungs-Manager“)](#teradata-destination-editor-connection-manager-page) festlegen.

## <a name="error-handling"></a>Fehlerbehandlung

Fehler, die während des Ladevorgangs zurückgegeben werden, werden in temporäre Fehlertabellen geschrieben, die während des Ladevorgangs gesperrt sind.
Die **Maximale Anzahl von Fehlern (MaxErrors)**-Eigenschaft im „Erweiterten Editor“ legt die maximale Anzahl von Fehlern fest, die in diese Tabellen geschrieben werden können.

Wenn die „Maximale Anzahl von Fehlern“ größer als 0 (null) ist, werden Fehlertabellen mit eindeutigen Namen generiert, und Informationsmeldungen werden im Paketprotokoll ausgegeben. Die Fehler können von der standardmäßigen SSIS-Komponentenfehlerausgabe abgerufen werden.

Die temporären Tabellen werden gelöscht, sobald der Ladevorgang abgeschlossen ist. Wenn die temporären Tabellen nicht vom Teradata-Ziel gelesen werden können, werden sie nur dann gelöscht, wenn die **Fehlertabelle immer löschen**-Eigenschaft aktiviert ist.  Wenn der Ladevorgang vor dem Abschluss beendet wird, müssen Sie diese Tabellen bei Bedarf manuell löschen. Diese Tabellen befinden sich in der gleichen Datenbank wie die Zieltabelle.

Wenn die **Maximale Anzahl von Fehlern** erreicht wird, hängt der Status der Zieltabelle vom verwendeten Modus ab.

- Im Modus für schnelles Laden ist die Zieltabelle nicht verwendbar. Zum erneuten Ausführen müssen Sie die Zieltabelle kürzen oder löschen und neu erstellen. Rollback wird nicht unterstützt.
- Im TPT-Stream-Operatormodus wird das Teradata-Ziel über einen gepufferten Zeilenmechanismus ausgeführt. Wenn bei einem Auftrag ein Fehler auftritt, befinden sich alle Änderungen, die zum Zeitpunkt des Fehlers abgeschlossen wurden (Puffer wurden gesendet) dauerhaft in Zieltabellen. Es gibt kein Rollbackkonzept. Fehlertabellen werden gelöscht.

Das Teradata-Ziel verfügt über eine Fehlerausgabe. Weitere Informationen finden Sie unter [Teradata-Ziel-Editor (Seite „Fehlerausgabe“)](#teradata-destination-editor-error-output-page)

## <a name="parallelism"></a>Parallelität

Die Parallelität ist im Modus für schnelles Laden eingeschränkt. Mehrere unabhängige Aufträge für schnelles Laden können nicht gleichzeitig auf dieselbe Tabelle zugreifen. Außerdem wird die Anzahl gleichzeitiger Aufträge für schnelles Laden durch die Datenbankvariable **MaxLoadTasks** beschränkt.

Im TPT-Streammodus gibt es keine Einschränkung der Parallelität. Es ist möglich, mehrere Teradata-Ziele gleichzeitig für dieselbe Tabelle auszuführen, jedoch kann dies die Leistung pro Teradata-Ziel verringern. Weitere Informationen finden Sie in der Teradata-Dokumentation.

## <a name="troubleshooting-the-teradata-destination"></a>Problembehandlung beim Teradata-Ziel

Sie können die Aufrufe der Teradata-Quelle an die Teradata Parallel Transporter-API (TPT-API) protokollieren. Aktivieren Sie hierzu die Paketprotokollierung, und wählen Sie dann auf Paketebene das Ereignis **Diagnose** aus, um die Aufrufe zu protokollieren.

Sie können die ODBC-Aufrufe der Teradata-Quelle an den Teradata-ODBC-Treiber protokollieren, indem Sie die Ablaufverfolgung für den ODBC-Treiber-Manager aktivieren. Weitere Informationen finden Sie in der Microsoft-Dokumentation unter *Generieren einer ODBC-Ablaufverfolgung mit dem ODBC-Datenquellen-Administrator*.

## <a name="teradata-destination-custom-properties"></a>Benutzerdefinierte Eigenschaften von Teradata-Zielen

In der folgenden Tabelle werden die benutzerdefinierten Eigenschaften des Teradata-Ziels beschrieben. Alle Eigenschaften weisen Lese-/Schreibzugriff auf.

|Eigenschaftenname|Datentyp|BESCHREIBUNG|
|:-|:-|:-|
|AlwaysDropErrorTable|Boolean|Der Standardwert ist **False**. Wenn der Wert **True** ist, werden alle Fehlertabellen gelöscht, auch dann, wenn das Teradata-Ziel keine Lesevorgänge ausführen kann.|
|ArraySupport|Boolean|Der Standardwert ist **true**. Wenn der Wert **True** ist, verwenden DML-Gruppen ArraySupport. Gilt nur für TPT-Stream. Diese Eigenschaft befindet sich im **Erweiterten Editor**.|
|Puffer|Integer|Ein von 2 bis 64 festlegbarer Wert, auf den die Anzahl der Anforderungspuffer erhöht werden soll. Gilt nur für TPT-Stream. Diese Eigenschaft befindet sich im **Erweiterten Editor**.|
|BufferMode|Boolean|Der Standardwert ist **true**. Wenn die PutBuffer-Funktion verwendet wird, muss der Wert **True** sein. Diese Eigenschaft befindet sich im **Erweiterten Editor**.|
|BufferSize|Integer|Die Ausgabepuffergröße (in KB), die zum Senden von Ladepaketen verwendet wird. Der Standardwert ist 1024. Gilt nur für TPT-Ladevorgang. Diese Eigenschaft befindet sich im **Erweiterten Editor**.|
|DataEncryption|Boolean|Der Standardwert ist **False**. Wenn der Wert **True** ist, wird die vollständige Sicherheitsverschlüsselung verwendet.|
|DefaultCodePage|Integer|Die zu verwendende Codepage, wenn die Datenquelle keine Codepageinformationen enthält. <br>**Hinweis**: Diese Eigenschaft befindet sich im **Erweiterten Editor**.|
|DetailedTracingLevel|Ganze Zahl (Enumeration)|Wählen Sie für die erweiterte Ablaufverfolgung eine der folgenden Optionen aus: <br> **Off**: Keine erweiterte Protokollierung. <br> **General**: Bei dieser Ablaufverfolgung werden treiberspezifische Aktivitäten protokolliert. <br> **CLI**: Bei dieser Ablaufverfolgung werden CLIv2-spezifische Aktivitäten protokolliert. <br> **Notify Method**: Aktivitäten im Zusammenhang mit der Benachrichtigung werden protokolliert. <br> **Common Library**: Die Ablaufverfolgung von Aktivitäten in der opCommon-Bibliothek wird protokolliert. <br> **All**: Bei dieser Ablaufverfolgung werden alle obigen Aktivitäten protokolliert. <br> Die Protokolldatei mit der erweiterten Ablaufverfolgung wird in der **DetailedTracingFile**-Eigenschaft definiert. <br> Die **DetailedTracingFile**-Eigenschaft muss festgelegt werden, wenn die Option nicht „Off“ ist. <br> Diese Eigenschaft befindet sich im **Erweiterten Editor**.|
|DetailedTracingFile|String|Der Pfad der Protokolldatei, die automatisch erzeugt wird, wenn **DetailedTracingLevel** nicht **Off** ist. Diese Eigenschaft befindet sich im **Erweiterten Editor**.|
|DiscardLargeRow|Boolean|Der Standardwert ist **False**. Wenn der Wert **True** ist, werden große Zeilen (über 64 K) verworfen.|
|ErrorTableName|String|Fehlertabellenname. Der Standardwert ist der Name der Zieltabelle.|
|ExtendedStringColumnsAllocation|Boolean|Wenn der Wert **True** ist, wird **Maximal Transfer Character Allocation Factor** wird verwendet. <br> Dieser Wert muss auf **True** festgelegt werden, wenn die Teradata-Datenbank-Eigenschaft **Export Width Table ID** auf **Maximal Defaults** festgelegt ist. <br> Der Standardwert ist **False**.|
|FastLoad|Boolean|Wenn der Wert **True** ist, wird schnelles Laden verwendet. Der Standardwert ist **false**. Der Wert kann auch im [Teradata-Ziel-Editor (Seite „Verbindungs-Manager“)](#teradata-destination-editor-connection-manager-page) festgelegt werden.|
|MaxErrors|Integer|Die Anzahl der Fehler, die auftreten können, bevor der Datenfluss abgebrochen wird. Der Standardwert ist **0**(null). Dies bedeutet, dass die Fehlerzahl nicht begrenzt ist.<br> Wenn **Fluss umleiten** auf der Seite **Fehlerbehandlung** ausgewählt ist. Bevor der Fehlerzahl-Grenzwert erreicht wird, werden alle Fehler in der Fehlerausgabe zurückgegeben. Weitere Informationen finden Sie unter [Teradata-Ziel-Editor (Seite „Fehlerausgabe“)](#teradata-destination-editor-error-output-page)|
|MaxSessions|Integer|Die maximale Anzahl der angemeldeten Sitzungen. Dieser Wert muss größer als 0 (null) sein. Der Standardwert ist eine Sitzung für jede verfügbare AMP-Instanz.|
|MinSessions|Integer|Die minimale Anzahl der angemeldeten Sitzungen. Dieser Wert muss größer als 0 (null) sein. Der Standardwert ist eine Sitzung für jede verfügbare AMP-Instanz.|
|Pack|Integer|Die Anzahl der Anweisungen, die in eine Anforderung mit mehreren Anweisungen verpackt werden. Der Standardwert beträgt 20, der zulässige Höchstwert 2.400. Gilt nur für TPT-Stream. Diese Eigenschaft befindet sich im **Erweiterten Editor**.|
|PackMaximum|Boolean|Wenn der Wert **True** ist, wird der maximale Pack-Faktor für den aktuellen Streamauftrag dynamisch bestimmt. Gilt nur für TPT-Stream. Diese Eigenschaft befindet sich im **Erweiterten Editor**.|
|QueryBandSessInfo|Varchar|Ein benutzerdefinierter, sitzungsbasierter Abfragebereichausdruck, um Chargebacküberwachung und Governance zu aktivieren. Diese Eigenschaft muss das Verbindungszeichenfolgen-Format aufweisen. Diese Eigenschaft befindet sich im **Erweiterten Editor**.|
|ReplicationOveride|Ganze Zahl (Enumeration)|Optionen: <br> **Standard:** Es wird keine SET SESSION OVERRIDE REPLICATION-Anweisung an die Datenbank gesendet. Die Standardeinstellungen für die Datenbank werden verwendet. <br> **Am:** Die normalen Replikationsdienst-Steuerelemente werden überschrieben. <br> **Off**: Die normalen Replikationsdienst-Steuerelemente werden verwendet. <br> Diese Eigenschaft ist nur für TPT-Stream verfügbar. <br> Diese Eigenschaft befindet sich im **Erweiterten Editor**.|
|Robust|Boolean|Wenn der Wert **True** ist, wird für Wiederherstellungs- und Neustartvorgänge eine robuste Neustartlogik verwendet. Diese Eigenschaft ist nur für **TPT-Stream** verfügbar. Diese Eigenschaft befindet sich im **Erweiterten Editor**.|
|TableName|String|Der Name der Tabelle mit den Daten, der verwendet werden.|
|TenacityHours|Integer|Die Anzahl der Stunden, die der TPT-Treiber versucht, sich anzumelden, wenn bereits die maximale Anzahl von Lade-/Exportvorgängen ausgeführt wird. Die Standardeinstellung beträgt 4 Stunden. Diese Eigenschaft befindet sich im **Erweiterten Editor**.|
|TenacitySleep|Integer|Die Minuten, die der TPT-Treiber bei Erreichen des Grenzwerts anhält, bevor er versucht, sich anzumelden. Der Grenzwert wird durch die Eigenschaften **MaxSessions** und **TenacityHours** definiert. Der Standardwert beträgt sechs Minuten. Diese Eigenschaft befindet sich im **Erweiterten Editor**.|
|UnicodePassThrough|Boolean|Off (Standard): Deaktivieren des Unicode-Pass-Throughs. <br>On: Aktivieren des Unicode-Pass-Throughs.|

## <a name="configuring-the-teradata-destination"></a>Konfigurieren des Teradata-Ziels

Sie können das Teradata-Ziel programmgesteuert oder mit dem SSIS-Designer konfigurieren.

Der Teradata-Ziel-Editor wird in der folgenden Abbildung dargestellt. Er enthält eine Verbindungs-Manager-Seite, Zuordnungsseite und Fehlerausgabeseite.

Weitere Informationen finden Sie in einem der folgenden Themen:

- [Teradata-Ziel-Editor (Seite „Verbindungs-Manager“)](#teradata-destination-editor-connection-manager-page)
- [Teradata-Ziel-Editor (Seite „Zuordnungen“)](#teradata-destination-editor-mappings-page)
- [Teradata-Ziel-Editor (Seite „Fehlerausgabe“)](#teradata-destination-editor-error-output-page)

![Ziel-Editor](media/teradata-destination.png)

Das Dialogfeld **Erweiterter Editor** enthält die Eigenschaften, die programmgesteuert festgelegt werden können.
So öffnen Sie das Dialogfeld **Erweiterter Editor** :

- Klicken Sie auf dem Bildschirm **Datenfluss** des Integration Services-Projekts mit der rechten Maustaste auf das Teradata-Ziel, und wählen Sie **Erweiterten Editor anzeigen** aus.

Weitere Informationen zu den Eigenschaften, die Sie im Dialogfeld „Erweiterter Editor“ festlegen können, finden Sie unter [Benutzerdefinierte Eigenschaften von Teradata-Zielen](#teradata-destination-custom-properties).

## <a name="teradata-destination-editor-connection-manager-page"></a>Teradata-Ziel-Editor (Seite „Verbindungs-Manager“)

Auf der Seite **Verbindungs-Manager** des Dialogfelds **Teradata-Ziel-Editor** können Sie den Teradata-Verbindungs-Manager für das Ziel auswählen. Außerdem können Sie auf dieser Seite eine Tabelle oder Sicht aus der Datenbank auswählen.

So öffnen Sie die Seite „Verbindungs-Manager“ des Teradata-Ziel-Editors

- Öffnen Sie in SQL Server Data Tools das SQL Server Integration Services-Paket (SSIS), das über das Teradata-Ziel verfügt.

- Doppelklicken Sie auf der Registerkarte „Datenfluss“ auf das Teradata-Ziel.

- Klicken Sie im Teradata-Ziel-Editor auf „Verbindungs-Manager“.

### <a name="options"></a>Tastatur

**Connection manager**

Wählen Sie einen vorhandenen Verbindungs-Manager aus der Liste aus, oder klicken Sie auf **Neu**, um einen neuen Teradata-Verbindungs-Manager zu erstellen.

**Neu**

Klicken Sie auf **Neu**. Das Dialogfeld **Teradata-Verbindungs-Manager-Editor**, in dem Sie einen neuen Verbindungs-Manager erstellen können, wird geöffnet.

**Datenzugriffsmodus**

Wählen Sie die Methode für die Auswahl von Daten aus der Quelle aus. Die Optionen sind in der folgenden Tabelle aufgeführt:

|Option|BESCHREIBUNG|
|:-|:-|
|Tabellenname – TPT-Stream|Inkrementeller Modus mit dem TPT-Stream-Operator. <br>**Name der Tabelle oder Sicht**: Wählen Sie in der Liste eine verfügbare Tabelle oder Sicht aus. In dieser Liste werden nur die ersten 1.000 Tabellen angezeigt. Sie können das Tabellennamenpräfix eingeben oder für einen beliebigen Teil des Namens den Platzhalter (*) verwenden, um die Tabelle oder die Tabellen aufzulisten, die Sie verwenden möchten.|
|Tabellenname – TPT-Ladevorgang|Schneller Lademodus (direkter Pfad) unter Verwendung des TPT-API-Ladeoperators (Teradata-FastLoad-Protokoll), bei dem die Zieltabelle leer sein muss. <br>**Name der Tabelle oder Sicht**: Wählen Sie in der Liste eine verfügbare Tabelle oder Sicht aus. In dieser Liste werden nur die ersten 1.000 Tabellen angezeigt. Sie können das Tabellennamenpräfix eingeben oder für einen beliebigen Teil des Namens den Platzhalter (*) verwenden, um die Tabelle oder die Tabellen aufzulisten, die Sie verwenden möchten.|

**Datenverschlüsselung**: Kontrollkästchen zum Aktivieren der Datenverschlüsselung. Der Standardwert ist nicht ausgewählt.

**Fehlertabelle immer löschen**: Kontrollkästchen zum Aktivieren des Löschens der Fehlertabellen in allen Instanzen.

**Fehlertabelle**: Name der Tabelle, in die Fehler geschrieben werden.

**Mindestanzahl von Sitzungen**: Die Mindestanzahl der angemeldeten Sitzungen. Der Standardwert ist eine Sitzung für jede verfügbare AMP-Instanz. Der Wert muss größer als eins sein.

**Maximale Anzahl von Sitzungen**: Die maximale Anzahl der angemeldeten Sitzungen. Der Standardwert ist eine Sitzung für jede verfügbare AMP-Instanz. Der Wert muss größer als eins sein.

**Maximale Anzahl von Fehlern**: Die maximale Anzahl von Fehlern, die zurückgegeben werden können, bevor der Datenfluss beendet oder umgeleitet wird. 

## <a name="teradata-destination-editor-mappings-page"></a>Teradata-Ziel-Editor (Seite „Zuordnungen“)

Auf der Seite **Zuordnungen** des Dialogfelds **Teradata-Ziel-Editor** können Sie eine Zuordnung von Eingabe- zu Zielspalten vornehmen.

So öffnen Sie die Seite „Zuordnungen“ im Teradata-Ziel-Editor

- Öffnen Sie in SQL Server Data Tools das SQL Server Integration Services-Paket (SSIS), das über das Teradata-Ziel verfügt.

- Doppelklicken Sie auf der Registerkarte „Datenfluss“ auf das Teradata-Ziel.

- Klicken Sie im Teradata-Ziel-Editor auf „Fehlerausgabe“.

### <a name="options"></a>Tastatur

**Verfügbare Eingabespalten**

Die Liste der verfügbaren Eingabespalten. Ordnen Sie die Eingabespalten per Drag & Drop den verfügbaren Zielspalten zu.

**Verfügbare Zielspalten**

Die Liste der verfügbaren Zielspalten. Ordnen Sie die Zielspalten per Drag & Drop den verfügbaren Eingabespalten zu.

**Eingabespalte**

Zeigt die von Ihnen ausgewählten Eingabespalten an. Sie können Zuordnungen entfernen, indem Sie **< ignore >** auswählen, um Spalten aus der Ausgabe auszuschließen.

**Zielspalte**

Zeigt alle verfügbaren Zielspalten an, sowohl die zugeordneten als auch die nicht zugeordneten.

>[!NOTE]
>
>Spalten mit nicht unterstützten Datentypen werden mit einer Warnung aus der Zuordnung gelöscht.

## <a name="teradata-destination-editor-error-output-page"></a>Teradata-Ziel-Editor (Seite „Fehlerausgabe“)
Auf der Seite „Fehlerausgabe“ des Dialogfelds „Teradata-Ziel-Editor“ können Sie Optionen für die Fehlerbehandlung auswählen.

**So öffnen Sie die Seite „Fehlerausgabe“ des Teradata-Ziel-Editors**

- Öffnen Sie in SQL Server Data Tools das SQL Server Integration Services-Paket (SSIS), das über das Teradata-Ziel verfügt.

- Doppelklicken Sie auf der Registerkarte „Datenfluss“ auf das Teradata-Ziel.

- Klicken Sie im Teradata-Ziel-Editor auf „Fehlerausgabe“.

### <a name="options"></a>Tastatur

**Fehlerverhalten**

Wählen Sie aus, wie das Teradata-Ziel Fehler in einem Fluss behandeln soll: Fehler ignorieren, Zeile umleiten oder Komponente mit einem Fehler abbrechen.

**Verwandte Themen:** [Fehlerbehandlung in Daten](./error-handling-in-data.md)

**Abschneiden**

Wählen Sie aus, wie das Teradata-Ziel Kürzungen in einem Fluss behandeln soll: Fehler ignorieren, Zeile umleiten oder Komponente mit einem Fehler abbrechen.

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren des [Teradata-Verbindungs-Managers](teradata-connection-manager.md)
- Konfigurieren der [Teradata-Quelle](teradata-source.md)
- Konfigurieren des [Teradata-Ziels](teradata-destination.md)
- Wenn Sie Fragen haben, besuchen Sie die [Tech Community](https://aka.ms/AA6iwdw).
