---
title: Neues in SSMA für DB2 (DB2ToSQL) | Microsoft-Dokumentation
description: Informieren Sie sich über Änderungen an SQL Server Migration Assistant (SSMA) für DB2 (DB2ToSQL) für jede Version.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 1cc38f85-3caa-42d0-8c76-a380c1d15c67
ms.author: alexiva
ms.openlocfilehash: a12bb61546ed042a98a3cae521572ef55723cfdf
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186507"
---
# <a name="whats-new-in-ssma-for-db2-db2tosql"></a>Neues in SSMA für DB2 (DB2ToSQL)

In diesem Artikel wird SQL Server Migration Assistant (SSMA) für DB2-Änderungen in jeder Version aufgeführt.

## <a name="ssma-v817"></a>SSMA v 8.17

Die v 8.17-Version von SSMA für DB2 enthält die folgenden Änderungen:

* Verbessern der Konvertierung von Translation-Funktionen
* Korrigieren der Datenmigration für Tabellen mit berechneten Spalten
* HTML-Bewertungsberichte aktualisieren, um den SQL-Text mit dem modernen Editor anzuzeigen

## <a name="ssma-v816"></a>SSMA v 8.16

Die v 8.16-Version von SSMA für DB2 enthält die folgenden Änderungen:

* Korrektur der Konvertierung von Spalten Aliasen mit Sonderzeichen
* Korrektur der Konvertierung für die `SELECTIVITY` Klausel
* Konvertierung für Klausel verbessern `WITH ROW MOVEMENT`
* Unterstützung für Legacy Parser entfernen
* Problem mit Objekten, die nicht aus der Datenbank aktualisiert werden

## <a name="ssma-v815"></a>SSMA v 8.15

Zusätzlich zu den Verbesserungen der Barrierefreiheit enthält die Version Version 8.15 von SSMA für DB2 folgende Änderungen:

* Korrektur der Konvertierung von `MIN` / `MAX` Aggregatfunktionen mit Datums-/uhrzeitargumenten
* Korrektur des Fehlers in `VARCHAR_FORMAT` der Emulations Funktion, wenn `DD` Platzhalter verwendet wird
* Verbessern der Typzuordnungen für den `TIME` Datentyp
* Verbessern der Konvertierung von `ROUND` -und- `TRUNC` Funktionen mit numerischen Argumenten
* Revamp-Bewertungsberichte für die Arbeit in modernen Browsern
* Von der Datenbank zur Azure AD Authentifizierung bereitgestellte Autorität verwenden
* Benennen von aus Dateien geladenen Anweisungen verbessern

## <a name="ssma-v814"></a>SSMA v 8,14

Zusätzlich zu den Verbesserungen, um mehr Barrierefreiheit für Menschen mit Behinderungen sicherzustellen, erfordert die v-Version von SSMA für DB2 ein Projekt Upgrade, da es nun vollständige Quell-/zielserverversion in den Projekt Metadaten speichert.

## <a name="ssma-v813"></a>SSMA v 8,13

Die Version 8,13 von SSMA für DB2 enthält die folgenden Änderungen:

* Unterstützung für gefilterte eindeutige Indizes
* Implizite Typumwandlungen bei der Umwandlung von Prozeduren und Funktionsaufrufen beachten
* Verbessern der Protokollierung für die Quell Verbindungs Zeichenfolge zur Behandlung von Verbindungsproblemen

## <a name="ssma-v812"></a>SSMA v 8.12

Die Version Version Version von SSMA für DB2 enthält die folgenden Änderungen:

* Konvertierung der `STRIP` Funktion
* Verbesserte Verarbeitung von Prozedur Optionen

## <a name="ssma-v811"></a>SSMA v 8.11

Die Version Version 8.11 von SSMA für DB2 enthält die folgenden Änderungen:

* Unterstützung für DB2 für i (v 7.1 und höher)
* Übersetzung von `SQLSTATE` und `SQLCODE`
* Konvertierungs Fehlermeldung für neben wirknde Operatoren innerhalb einer Funktion
* Verwenden der MSAL.NET-Bibliothek für die interaktive Azure Active Directory Authentifizierung

## <a name="ssma-v810"></a>SSMA v 8.10

Das Release v 8.10 von SSMA für DB2 behandelt eine Regression bei der Fremdschlüssel Ermittlung und enthält geringfügige Leistungsverbesserungen.

## <a name="ssma-v89"></a>SSMA v 8,9

Das v 8,9-Release von SSMA für DB2 enthält die folgenden Änderungen:

* Korrektur für die Konvertierung der `TIMESTAMPDIFF` Funktion
* Korrektur für die Ermittlung von Indizes, wenn ein partitionierter Index vorhanden ist
* Behebung der Fremdschlüssel Ermittlung, wenn der primäre Index in einem anderen Schema definiert ist
* Verbesserte Konvertierung für Spalten, die den Namen integrierter Funktionen entsprechen
* Behebung des Problems mit Sonderzeichen in Project Name

## <a name="ssma-v88"></a>SSMA v 8.8

Die Version 8.8 von SSMA für DB2 umfasst Folgendes:

* Verbesserungen der Synchronisierungs Stabilität SQL Server
* Leistungsverbesserungen bei der GUI während der Bewertung und Konvertierung
* Aktualisierte Zuordnung von `ROWID` zu, `varbinary(40)` um die Datenmigration zu vereinfachen
* Verbesserte Konvertierung von `SELECT ... FROM NEW/OLD TABLE` Anweisungen
* Neue Konvertierung von `ALTER` Anweisungen für Prozeduren und Funktionen
* Neue Konvertierung von destrukturierungszuweisungen

## <a name="ssma-v87"></a>SSMA v 8.7

Das v 8.7-Release von SSMA für DB2 umfasst den brandneuen DB2-Syntax Parser sowie kleinere Korrekturen und Leistungsverbesserungen in grafischer Benutzeroberfläche.

Außerdem bietet SSMA für DB2 nun Folgendes:

* Eine Korrektur für die Ermittlung von Fremdschlüsseln bei der Migration von DB2 auf LUW.
* Verbesserte Konvertierung der- `SELECT ... FOR UPDATE` Anweisung.
* Verbesserte Konvertierung für die `COUNT` Funktion in MQ-Tabellen.
* Konvertierung von- `SAVEPOINT` Anweisungen.
* Konvertierung zum Emulieren des DB2's-Verhaltens für `NULL` Werte in der- `ORDER BY` Klausel.
* Unterstützung für die-Anweisung wird unterstützt `ASSOCIATE RESULT SET` .

> [!IMPORTANT]
> Mit SSMA Version 8.5 und höher ist .NET 4.7.2 eine erforderliche Installation. Wenn Sie diese Version installieren müssen, können Sie die Lauf Zeit Datei [hier](https://dotnet.microsoft.com/download/dotnet-framework/net472)herunterladen.

## <a name="ssma-v86"></a>SSMA v 8.6

Zusätzlich zu einem Zielsatz von Korrekturen, die zur Verbesserung der Benutzerfreundlichkeit und Leistung entwickelt wurden, wurde die Version Version 8.6 von SSMA für DB2 durch Hinzufügen einer Einstellung verbessert, mit der Benutzer die erweiterten SSMA-Eigenschaften im konvertierten Code weglassen können.

Um diese Einstellung zu nutzen, navigieren Sie in SSMA für DB2 **zu Extras**  >  **Projekteinstellungen**  >  **Allgemeine**  >  **Konvertierung**, und aktualisieren Sie dann unter " **misc**" den Wert der Einstellung **Erweiterte Eigenschaften** unterdrücken auf **Ja**.

![Einstellung für erweiterte Eigenschaften weglassen](../db2/media/ssma-omit-extended-properties.png)

Außerdem bietet SSMA für DB2 nun Folgendes:

* Eine Korrektur für die Konvertierung von Funktionen, die Standardargument Werte verwenden.
* Verbesserte Verarbeitung der- `PARAMETER` Klausel für Funktionen.
* Die Möglichkeit, die Anweisung zu konvertieren `LEAVE` .

> [!IMPORTANT]
> Mit SSMA Version 8.5 und höher ist .NET 4.7.2 eine erforderliche Installation. Wenn Sie diese Version installieren müssen, können Sie die Lauf Zeit Datei [hier](https://dotnet.microsoft.com/download/dotnet-framework/net472)herunterladen.

## <a name="ssma-v85"></a>SSMA v 8.5

Die Version v 8.5 von SSMA für DB2 wurde mit Unterstützung für Azure Active Directory Authentifizierung und grundlegender Unterstützung für JSON-Funktionen in SQL Server erweitert, sowie eine Reihe gezielter Korrekturen zur Verbesserung der Benutzerfreundlichkeit und Leistung.

Außerdem wurde SSMA für DB2 wie folgt erweitert:

* Unterstützung für das Hinzufügen der Konvertierung für die- `GET DIAGNOSTICS` Anweisung `ROW_NUMBER`
* Eine Korrektur für einen Fehler im Zusammenhang mit Leerzeichen am Anfang des Objekt namens wird nicht beachtet.

> [!IMPORTANT]
> Mit SSMA v 8.5 ist .NET 4.7.2 eine Installation, die Voraussetzung ist. Wenn Sie diese Version installieren müssen, können Sie die Lauf Zeit Datei [hier](https://dotnet.microsoft.com/download/dotnet-framework/net472)herunterladen.

## <a name="ssma-v84"></a>SSMA v 8.4

Die Version Version 8.4 von SSMA für DB2 wurde durch gezielte Korrekturen ergänzt, die zur Behebung von Problemen bei der Barrierefreiheit entwickelt wurden, und zum Beheben eines Fehlers im Zusammenhang mit den maximalen Index Spalten (um 32 anstelle von 16) für SQL Server 2016 und höhere Versionen zuzulassen.

> [!IMPORTANT]
> Mit SSMA, Version 7,4, jedoch 8,4, ist .NET 4.5.2 eine Installation, die Voraussetzung ist.

## <a name="ssma-v83"></a>SSMA v 8.3

Das v 8.3-Release von SSMA für DB2 wurde durch gezielte Korrekturen verbessert, die zur Verbesserung der Qualität und der konvertierungsmetriken entwickelt wurden. Außerdem bietet diese Version von SSMA für DB2 Korrekturen, die Folgendes beinhaltet:

* Behandeln von Problemen bei der Barrierefreiheit
* Fügen Sie grundlegende Unterstützung für `hierarchyid` Typ in SQL Server hinzu.
* Ersetzen Sie die Verwendung der Trim-Funktion in z/OS-Ermittlungs Abfragen mit `RTRIM` / `LTRIM` .
* Ermöglicht Benutzern das Angeben der Paket Sammlung beim Herstellen einer Verbindung im ' Standard Modus ' (standardmäßig `NULLID` ).
* Fügen Sie eine Konvertierung für hinzu `CREATE TABLE AS SELECT` .
* Verbessern von Konvertierungen für globale temporäre Tabellen.
* Beheben Sie ein Problem mit der Überprüfung der Objekt Eindeutigkeit, um Tabellen gegenüber Einschränkungen zu priorisieren, wenn die Namen kollidieren.
* Beheben Sie ein Problem beim Laden von Standard Spaltenwerten für `DATE` und `TIMESTAMP` für z/OS.
* Unterstützung für Unicode-Zeilenvorschub Zeichen (auch als bezeichnet `NEL` ).
* Beheben eines Problems mit der Cursor Konvertierung mit fehlender `RETURN TO` Klausel.
* Fügen Sie Unterstützung für Bezeichnungen und hinzu `GOTO` .

## <a name="ssma-v82"></a>SSMA v 8.2

Die Version 8.2 von SSMA für DB2 wurde mit erweitert, um Probleme mit Verbindungen mit Azure SQL-Datenbank über das SSMA-Konsolen Tool und fehlende COUNT_BIG Spalte in der views-Deklaration während der Konvertierung zu beheben. Darüber hinaus enthält diese Version einen Satz von Korrekturen, die für die Verbesserung von Qualitäts-und konvertierungsmetriken entworfen wurden, sowie Fehlerbehebungen für:

* Ein Problem mit deaktivierten nicht gruppierten Indizes nach der Datenmigration.
* Erkennung von .NET Framework während der automatischen Installation.
* Ein zeitweiliger Absturz, der auftritt, wenn eine neue Version heruntergeladen wird.

> [!NOTE]
> Ein bekanntes Problem bei der automatischen Aktualisierung kann zu einem Fehler bei einem Update von SSMA v 8.1 auf v 8.2 führen. Wenn dieser Fehler auftritt, laden Sie die neue Version herunter, und installieren Sie Sie manuell.

## <a name="ssma-v81"></a>SSMA v 8.1

Die Version 8.1 von SSMA für DB2 wurde verbessert, um gezielte Korrekturen bereitzustellen, die zur Verbesserung der Qualität und der konvertierungsmetriken entwickelt wurden.

> [!NOTE]
> Ein bekanntes Problem bei der automatischen Aktualisierung kann zu einem Fehler bei einem Update von SSMA v 8.0 auf v 8.1 führen. Wenn dieser Fehler auftritt, laden Sie die neue Version herunter, und installieren Sie Sie manuell.

## <a name="ssma-v80"></a>SSMA v 8.0

Das v 8.0-Release von SSMA für DB2 wurde verbessert, um gezielte Korrekturen zur Verbesserung der Qualität und Konvertierungs Metriken bereitzustellen. Diese Version bietet auch die folgenden neuen Features:

* Unterstützung für **Azure SQL-verwaltete Instanz** als Ziel. Sie können jetzt neue Projekte erstellen, die auf Azure SQL-verwaltete Instanz abzielen:

  ![SQL-Mi-Projekt](../media/ssma-newproject-sqldbmi.png)

* **Korrektur Ratgeber** nach der Konvertierung. Weitere Informationen hierzu [finden Sie hier](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/).

* Vorläufige Datenbank/Schema-Auswahl.

  Beim Herstellen einer Verbindung mit der Quelle kann der Benutzer jetzt Datenbanken/Schemas von Interesse auswählen. Wenn Sie nur die Schemas auswählen, für die die Migration geplant ist, sparen Sie Zeit während der ersten Verbindung und verbessern die SSMA-Gesamtleistung.

  ![SSMA-Filter Objekte](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA v 7.10

Das v 7.10-Release von SSMA für DB2 enthält die folgenden Änderungen:

* Gezielte Korrekturen zur Bereitstellung zusätzlicher Sicherheits-und Datenschutz Maßnahmen zum erfüllen von Änderungen an globalen Anforderungen.
* Eine Korrektur für die Konvertierung von- `BEGIN-END` Blöcken.

## <a name="ssma-v79"></a>SSMA v 7.9

Die Version Version 7.9 von SSMA für DB2 enthält die folgenden Änderungen:

* Gezielte Korrekturen, die Qualitäts-und konvertierungsmetriken verbessern.
* Unterstützung in der SSMA-Befehlszeile, um Datentyp Zuordnung und Projekteinstellungen zu ändern.
* Unterstützung für das Migrieren von Daten mithilfe von SQL Server Integration Services (SSIS). Nach dem Umrechnen des Schemas ist es möglich, ein SSIS-Paket zu erstellen, indem Sie mit der rechten Maustaste auf die Kontextmenü Option klicken.
* Das Azure SQL-Daten bankverbindungs Dialogfeld in SSMA wurde ebenfalls geändert, um den voll qualifizierten Servernamen anzugeben. In früheren Versionen von SSMA musste das Azure SQL-Daten Bank Präfix in den Projekteinstellungen explizit angegeben werden.

## <a name="ssma-v78"></a>SSMA v 7.8

Das Release v 7.8 von SSMA für DB2 enthält die folgenden Änderungen:

* Ändern Sie in den *Projekteinstellungen* hervorgehobene Typzuordnung.
* Die Möglichkeit für Benutzer, Telemetriedaten zu deaktivieren.

## <a name="ssma-v77"></a>SSMA v 7.7

Das Release v 7.7 von SSMA für DB2 enthält die folgenden Änderungen:

* Gezielte Korrekturen, die Qualitäts-und konvertierungsmetriken verbessern.
* Basierend auf der beliebten Nachfrage ist die 32-Bit-Version von SSMA für DB2 zurück. Im Vergleich zur vorherigen Implementierung (vor v 7.4) gibt es zwei Installer-Pakete, aber Sie können nicht parallel installiert werden. Daher müssen Sie die am besten geeignete Version basierend auf den Konnektivitätskomponenten auswählen, die Sie besitzen. Es ist immer ratsam, die 64-Bit-Version zu verwenden, wenn möglich.

## <a name="ssma-v76"></a>SSMA v 7.6

Die Version Version 7.6 von SSMA für DB2 wurde durch gezielte Korrekturen ergänzt, die Qualitäts-und konvertierungsmetriken sowie die Unterstützung für SQL Server 2017 (öffentliche Vorschau) verbessern. Unterstützung für SQL Server 2017 unter Windows und Linux befindet sich in der öffentlichen Vorschau Phase und sollte nicht für Produktions Migrationen verwendet werden.

## <a name="ssma-v75"></a>SSMA v 7.5

Das Release v 7.5 von SSMA für DB2 wurde durch mehrere Verbesserungen verbessert, um mehr Barrierefreiheit für Menschen mit Behinderungen sicherzustellen.

## <a name="ssma-v74"></a>SSMA v 7.4

Die Version Version 7.4 von SSMA für DB2 enthält die folgenden Änderungen:

* Die Option **Abfrage Timeout** ist jetzt bei der Schema Objekt Ermittlung unter Quelle und Ziel verfügbar.

  ![Abfrage Timeout (Option)](../media/query-timeout_red.png)

* Die Qualitäts-und Konvertierungs Metrik wurde durch gezielte Korrekturen verbessert, basierend auf Kundenfeedback.

  > [!IMPORTANT]
  > .NET 4.5.2 ist eine Voraussetzung für die Installation von SSMA v 7.4. Ab Version 7.4 wird die 32-Bit-Version von SSMA nicht mehr unterstützt.

## <a name="ssma-v73"></a>SSMA v 7.3

Das Release v 7.3 von SSMA für DB2 enthält die folgenden Änderungen:

* Verbesserte Qualitäts-und Konvertierungs Metrik durch gezielte Korrekturen auf der Grundlage von Kundenfeedback.
* Das SSMA-Erweiterbarkeits Framework wurde über die folgenden Elemente verfügbar gemacht:
  * Exportieren Sie die-Funktionalität in ein SQL Server Data Tools-Projekt (SSDT).
    * Sie können jetzt Schema Skripts aus SSMA in ein SSDT-Projekt exportieren. Sie können die Schema Skripts verwenden, um zusätzliche Schema Änderungen vorzunehmen und die Datenbank bereitzustellen.

      ![Als SSDT-Projekt Befehl speichern](../media/export-schema-scripts_red.png)
  * Bibliotheken, die von SSMA zum Ausführen benutzerdefinierter Konvertierungen genutzt werden können.
    * Nun können Sie Code erstellen, mit dem benutzerdefinierte Syntax Konvertierungen und Konvertierungen verarbeitet werden können, die zuvor nicht von SSMA verarbeitet wurden.
      * Anweisungen zum Erstellen eines benutzerdefinierten Konverters finden Sie in diesem Blogbeitrag, in [dem die Konvertierungs Funktionen von SQL Server Migration Assistant erweitert](https://blogs.msdn.microsoft.com/datamigration/2017/02/21/2185/)werden.
      * Herunterladen eines Beispielprojekts für die Konvertierung aus diesem [Blogbeitrag](https://blogs.msdn.microsoft.com/datamigration/ssmafororacleconversionsample/).

## <a name="ssma-v72"></a>SSMA v 7.2

Die Version Version 7.2 von SSMA für DB2 enthält die folgenden Änderungen:

* Verbesserte Qualitäts-und Konvertierungs Metrik durch gezielte Korrekturen auf der Grundlage von Kundenfeedback.
* Telemetrie-Verbesserungen, um bessere Datenpunkte zur Behandlung von Kundenproblemen und zur Verbesserung der Konvertierungsraten von SSMA bereitzustellen.

## <a name="ssma-v71"></a>SSMA v 7.1

Die Version Version 7.1 von SSMA für DB2 enthält die folgenden Änderungen:

* SQL Server 2017 unter Windows und Linux CTP1 wird jetzt als Zielplattform für die Migration unterstützt. Diese Funktion befindet sich in der Technical Preview und ermöglicht das Schema und die Daten Verschiebung für SQL Server-Zielserver.
* Unterstützung für automatische Updates, um die neueste Version von SSMA herunterzuladen, sobald Sie verfügbar ist.
* Installierbare SSMA-Binärdateien werden jetzt über Windows Installer Paketdateien (MSI) übermittelt.

## <a name="may-2016"></a>Mai 2016

Die Version vom Mai 2016 von SSMA für DB2 enthält die folgenden Änderungen:

* Unterstützung für SQL Server 2016 hinzugefügt.
* Die Konvertierung von DB2 in-Memory-und regulären Tabellen in SQL Server in-Memory-und hekaton-Features wurde hinzugefügt.
* Die Konvertierung von DB2-Zugriffs Steuerungen in SQL Server Richtlinien Objekte (Sicherheit auf Zeilenebene für DB2) wurde hinzugefügt.
* Die Konvertierung von DB2-Tabellen mit System Versionsverwaltung in SQL Server Temporale Tabellen wurde hinzugefügt.
* Verbesserter DB2-Parser und-Resolver.
* Die Installer-Überprüfung für .NET 2,0 wurde entfernt.
* Unnötige dll-Datei \* aus DB2-Installer entfernt.
* Fixed `save-project` -und- `open-project` Befehle für die SSMA-Konsole.
* Fixed- `securepassword` Befehl für die SSMA-Konsole.
* Das zählen von Objekten für das anfängliche laden wurde korrigiert.
* Fehler in globalen Einstellungen behoben.
  
## <a name="march-2016"></a>März 2016

Die Vorschauversion von SSMA für DB2 vom März 2016 bietet Unterstützung für die Migration zu SQL Server 2016.

## <a name="january-2016"></a>Januar 2016

Die Wartungsversion von SSMA für DB2 vom Januar 2016 enthält die folgenden Änderungen:
  
* Unterstützung für eine Reihe von Standardfunktionen hinzugefügt.
* Beheben von DB2-Parser-Fehlern.
* Die Unterstützung für DB2 v9 zOS (RFC 5690920) wurde korrigiert.
* Fehler bei aufgelösten DB2-Bezeichnern beim Konvertieren behoben.
* Menü Element "View Log" zu SSMA hinzugefügt (RFC 5706203).
* Telemetrie hinzugefügt.
  
## <a name="november-2014"></a>November 2014

Die Version vom November 2014 von SSMA für DB2 war die erste Version.
