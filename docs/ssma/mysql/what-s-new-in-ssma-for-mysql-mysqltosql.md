---
title: Neues in SSMA für MySQL (mysqlto SQL) | Microsoft-Dokumentation
description: Informieren Sie sich über Änderungen an SQL Server Migration Assistant (SSMA) für MySQL (mysqldesql) für jede Version.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 1451a0b0-6713-4d0c-954f-ea3d8fce1d31
ms.author: alexiva
ms.openlocfilehash: 138cb90558bfb73d0b9e0cd3ba75a7a41fc84229
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186456"
---
# <a name="whats-new-in-ssma-for-mysql-mysqltosql"></a>Neuerungen in SSMA für MySQL (MySqlToSql)

In diesem Artikel werden SQL Server Migration Assistant (SSMA) für MySQL-Änderungen in jeder Version aufgeführt.

## <a name="ssma-v817"></a>SSMA v 8.17

Die v 8.17-Version von SSMA für MySQL enthält die folgenden Änderungen:

* HTML-Bewertungsberichte aktualisieren, um den SQL-Text mit dem modernen Editor anzuzeigen

## <a name="ssma-v816"></a>SSMA v 8.16

Die v 8.16-Version von SSMA für MySQL enthält die folgenden Änderungen:

* Unterstützung für berechnete Spalten hinzufügen
* Beheben von Problemen beim wandeln der `INSERT` Anweisung für Tabellen mit UNIQUE-Einschränkungen und primär Schlüsseln
* Aktualisieren des Parsers in Bezug auf die `ANSI_QUOTES` `NO_BACKSLASH_ESCAPES` Server Modi
* Unterstützung für Legacy Parser entfernen
* Problem mit Objekten, die nicht aus der Datenbank aktualisiert werden

## <a name="ssma-v815"></a>SSMA v 8.15

Zusätzlich zu den Verbesserungen der Barrierefreiheit enthält die Version Version 8.15 von SSMA für MySQL folgende Änderungen:

* Revamp-Bewertungsberichte für die Arbeit in modernen Browsern
* Von der Datenbank zur Azure AD Authentifizierung bereitgestellte Autorität verwenden
* Benennen von aus Dateien geladenen Anweisungen verbessern

## <a name="ssma-v814"></a>SSMA v 8,14

Zusätzlich zu den Verbesserungen, um mehr Barrierefreiheit für Menschen mit Behinderungen sicherzustellen, erfordert die v-Version von SSMA für MySQL ein Projekt Upgrade, da es nun vollständige Quell-/zielserverversion in den Projekt Metadaten speichert.

## <a name="ssma-v813"></a>SSMA v 8,13

Die Version 8,13 von SSMA für MySQL enthält die folgenden Änderungen:

* Implizite Typumwandlungen bei der Umwandlung von Prozeduren und Funktionsaufrufen beachten
* Verbessern der Protokollierung für die Quell Verbindungs Zeichenfolge zur Behandlung von Verbindungsproblemen

## <a name="ssma-v812"></a>SSMA v 8.12

Die Version Version Version von SSMA für MySQL enthält die folgenden Änderungen:

* Konvertierung temporärer Tabellen-DDL

## <a name="ssma-v811"></a>SSMA v 8.11

Die Version Version 8.11 von SSMA für MySQL enthält die folgenden Änderungen:

* Verwenden der MSAL.NET-Bibliothek für die interaktive Azure Active Directory Authentifizierung

## <a name="ssma-v810"></a>SSMA v 8.10

Das Release v 8.10 von SSMA für MySQL enthält geringfügige Leistungsverbesserungen und Fehlerbehebungen.

## <a name="ssma-v89"></a>SSMA v 8,9

Das v 8,9-Release von SSMA für MySQL enthält die folgenden Änderungen:

* Behebung der Datenmigration räumlicher Typen
* Behebung des Problems mit Sonderzeichen in Project Name

## <a name="ssma-v88"></a>SSMA v 8.8

Die Version 8.8 von SSMA für MySQL umfasst Folgendes:

* Verbesserungen der Synchronisierungs Stabilität SQL Server
* Leistungsverbesserungen bei der GUI während der Bewertung und Konvertierung

## <a name="ssma-v87"></a>SSMA v 8.7

Das v 8.7-Release von SSMA für MySQL weist kleinere Korrekturen und Leistungsverbesserungen in der grafischen Benutzeroberfläche auf.

Außerdem bietet SSMA für MySQL jetzt eine Konvertierung für die- `LIMIT` Klausel, wenn Azure SQL als Ziel verwendet wird.

> [!IMPORTANT]
> Mit SSMA Version 8.5 und höher ist .NET 4.7.2 eine erforderliche Installation. Wenn Sie diese Version installieren müssen, können Sie die Lauf Zeit Datei [hier](https://dotnet.microsoft.com/download/dotnet-framework/net472)herunterladen.

## <a name="ssma-v86"></a>SSMA v 8.6

Zusätzlich zu einem Zielsatz von Korrekturen, die zur Verbesserung der Benutzerfreundlichkeit und Leistung entwickelt wurden, wurde die Version Version 8.6 von SSMA für MySQL erweitert, indem eine Einstellung hinzugefügt wurde, mit der Benutzer erweiterte SSMA-Eigenschaften im konvertierten Code weglassen können.

Um diese Einstellung zu nutzen, navigieren Sie in SSMA für MySQL **zu Extras**  >  **Projekteinstellungen**  >  **Allgemeine**  >  **Konvertierung**, und aktualisieren Sie dann unter " **misc**" den Wert der Einstellung **Erweiterte Eigenschaften** unterdrücken auf **Ja**.

![Einstellung für erweiterte Eigenschaften weglassen](../mysql/media/ssma-omit-extended-properties.png)

> [!IMPORTANT]
> Mit SSMA Version 8.5 und höher ist .NET 4.7.2 eine erforderliche Installation. Wenn Sie diese Version installieren müssen, können Sie die Lauf Zeit Datei [hier](https://dotnet.microsoft.com/download/dotnet-framework/net472)herunterladen.

## <a name="ssma-v85"></a>SSMA v 8.5

Die Version v 8.5 von SSMA für MySQL wurde mit Unterstützung für Azure Active Directory Authentifizierung und grundlegender Unterstützung für JSON-Funktionen in SQL Server erweitert, sowie eine Reihe gezielter Korrekturen, die zur Verbesserung der Benutzerfreundlichkeit und Leistung entwickelt wurden.

> [!IMPORTANT]
> Mit SSMA v 8.5 ist .NET 4.7.2 eine Installation, die Voraussetzung ist. Wenn Sie diese Version installieren müssen, können Sie die Lauf Zeit Datei [hier](https://dotnet.microsoft.com/download/dotnet-framework/net472)herunterladen.

## <a name="ssma-v84"></a>SSMA v 8.4

Die Version Version 8.4 von SSMA für MySQL wurde durch gezielte Korrekturen ergänzt, die zur Behebung von Problemen bei der Barrierefreiheit entwickelt wurden, und zum Beheben eines Fehlers im Zusammenhang mit den maximalen Index Spalten (um 32 anstelle von 16) für SQL Server 2016 und höhere Versionen zuzulassen.

> [!IMPORTANT]
> Mit SSMA, Version 7,4, jedoch 8,4, ist .NET 4.5.2 eine Installation, die Voraussetzung ist.

## <a name="ssma-v83"></a>SSMA v 8.3

Das v 8.3-Release von SSMA für MySQL wurde durch gezielte Korrekturen verbessert, die zur Verbesserung der Qualität und der konvertierungsmetriken entwickelt wurden. Außerdem bietet diese Version von SSMA für MySQL Korrekturen, die Folgendes beinhaltet:

* Behandeln von Problemen bei der Barrierefreiheit
* Fügen Sie grundlegende Unterstützung für `hierarchyid` Typ in SQL Server hinzu.

## <a name="ssma-v82"></a>SSMA v 8.2

Die Version 8.2 von SSMA für MySQL wurde durch eine Reihe von Korrekturen ergänzt, die zum Verbessern von Qualitäts-und konvertierungsmetriken entworfen wurden, sowie für folgende Korrekturen:

* Ein Problem mit deaktivierten nicht gruppierten Indizes nach der Datenmigration.
* Erkennung von .NET Framework während der automatischen Installation.
* Ein zeitweiliger Absturz, der auftritt, wenn eine neue Version heruntergeladen wird.

> [!NOTE]
> Ein bekanntes Problem bei der automatischen Aktualisierung kann zu einem Fehler bei einem Update von SSMA v 8.1 auf v 8.2 führen. Wenn dieser Fehler auftritt, laden Sie die neue Version herunter, und installieren Sie Sie manuell.

## <a name="ssma-v81"></a>SSMA v 8.1

Die Version 8.1 von SSMA für MySQL wurde durch gezielte Korrekturen ergänzt, die zur Verbesserung der Qualität und der konvertierungsmetriken entwickelt wurden.

> [!NOTE]
> Ein bekanntes Problem bei der automatischen Aktualisierung kann zu einem Fehler bei einem Update von SSMA v 8.0 auf v 8.1 führen. Wenn dieser Fehler auftritt, laden Sie die neue Version herunter, und installieren Sie Sie manuell.

## <a name="ssma-v80"></a>SSMA v 8.0

Das v 8.0-Release von SSMA für MySQL wurde durch gezielte Korrekturen verbessert, die zur Verbesserung der Qualität und der konvertierungsmetriken entworfen wurden. Diese Version bietet auch die folgenden neuen Features:

* Unterstützung für **Azure SQL-verwaltete Instanz** als Ziel. Sie können jetzt neue Projekte erstellen, die auf Azure SQL-verwaltete Instanz abzielen:

  ![SQL-Mi-Projekt](../media/ssma-newproject-sqldbmi.png)

* **Korrektur Ratgeber** nach der Konvertierung. Weitere Informationen hierzu [finden Sie hier](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/).

* Vorläufige Datenbank/Schema-Auswahl.

  Beim Herstellen einer Verbindung mit der Quelle kann der Benutzer jetzt Datenbanken/Schemas von Interesse auswählen. Wenn Sie nur die Schemas auswählen, für die die Migration geplant ist, sparen Sie Zeit während der ersten Verbindung und verbessern die SSMA-Gesamtleistung.

  ![SSMA-Filter Objekte](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA v 7.10

Das v 7.10-Release von SSMA für MySQL enthält die folgenden Änderungen:

* Gezielte Korrekturen zur Bereitstellung zusätzlicher Sicherheits-und Datenschutz Maßnahmen zum erfüllen von Änderungen an globalen Anforderungen.
* Eine Korrektur für die Konvertierung von Leerzeichen zwischen dem Funktionsnamen und der Argumentliste.

## <a name="ssma-v79"></a>SSMA v 7.9

Die Version Version 7.9 von SSMA für MySQL enthält die folgenden Änderungen:

* Gezielte Korrekturen, die Qualitäts-und konvertierungsmetriken verbessern.
* Partielle Unterstützung für die Migration räumlicher Datentypen von MySQL zu Azure SQL-Datenbank.
* Unterstützung in der SSMA-Befehlszeile, um Datentyp Zuordnung und Projekteinstellungen zu ändern.
* Unterstützung für das Migrieren von Daten mithilfe von SQL Server Integration Services (SSIS). Nach dem Umrechnen des Schemas ist es möglich, ein SSIS-Paket zu erstellen, indem Sie mit der rechten Maustaste auf die Kontextmenü Option klicken.
* Das Azure SQL-Daten bankverbindungs Dialogfeld in SSMA wurde ebenfalls geändert, um den voll qualifizierten Servernamen anzugeben. In früheren Versionen von SSMA musste das Azure SQL-Daten Bank Präfix in den Projekteinstellungen explizit angegeben werden.

## <a name="ssma-v78"></a>SSMA v 7.8

Das Release v 7.8 von SSMA für MySQL enthält die folgenden Änderungen:

* Ändern Sie in den Projekteinstellungen hervorgehobene Typzuordnung.
* Die Möglichkeit für Benutzer, Telemetriedaten zu deaktivieren.

## <a name="ssma-v77"></a>SSMA v 7.7

Das Release v 7.7 von SSMA für MySQL enthält die folgenden Änderungen:

* SSMA für MySQL wurde durch gezielte Korrekturen verbessert, die Qualitäts-und konvertierungsmetriken verbessern.
* Basierend auf der beliebten Nachfrage ist die 32-Bit-Version von SSMA für MySQL wieder verfügbar. Im Vergleich zur vorherigen Implementierung (vor v 7.4) gibt es zwei Installer-Pakete, aber Sie können nicht parallel installiert werden. Daher müssen Sie die am besten geeignete Version basierend auf den Konnektivitätskomponenten auswählen, die Sie besitzen. Es ist immer ratsam, die 64-Bit-Version zu verwenden, wenn möglich.
* SSMA für MySQL verfügt jetzt über den Verbindungs Modus für ODBC-Verbindungs Zeichenfolgen, der es Ihnen ermöglicht, ODBC-Treiber von Drittanbietern zu verwenden, die mit MySQL kompatibel sind.

## <a name="ssma-v76"></a>SSMA v 7.6

Die Version Version 7.6 von SSMA für MySQL wurde durch gezielte Korrekturen verbessert, die die Qualitäts-und konvertierungsmetriken sowie die Unterstützung für SQL Server 2017 (öffentliche Vorschau) verbessern. Unterstützung für SQL Server 2017 unter Windows und Linux befindet sich in der öffentlichen Vorschau Phase und sollte nicht für Produktions Migrationen verwendet werden.

## <a name="ssma-v75"></a>SSMA v 7.5

Das Release v 7.5 von SSMA für MySQL wurde durch mehrere Verbesserungen verbessert, um die Barrierefreiheit für Personen mit Behinderungen zu erhöhen.

## <a name="ssma-v74"></a>SSMA v 7.4

Die Version Version 7.4 von SSMA für MySQL enthält die folgenden Änderungen:

* Die Option **Abfrage Timeout** ist jetzt bei der Schema Objekt Ermittlung unter Quelle und Ziel verfügbar.

    ![Abfrage Timeout (Option)](../media/query-timeout_red.png)
* Die Qualitäts-und Konvertierungs Metrik wurde durch gezielte Korrekturen verbessert, basierend auf Kundenfeedback.

> [!IMPORTANT]
> .NET 4.5.2 ist eine Voraussetzung für die Installation von SSMA v 7.4. Außerdem wird ab Version 7.4 die 32-Bit-Version von SSMA eingestellt.

## <a name="ssma-v73"></a>SSMA v 7.3

Das Release v 7.3 von SSMA für MySQL enthält die folgenden Änderungen:

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

Die Version Version 7.2 von SSMA für MySQL enthält die folgenden Änderungen:

* Verbesserte Qualitäts-und Konvertierungs Metrik durch gezielte Korrekturen auf der Grundlage von Kundenfeedback.
* Telemetrie-Verbesserungen, um bessere Datenpunkte zur Behandlung von Kundenproblemen und zur Verbesserung der Konvertierungsraten von SSMA bereitzustellen.

## <a name="ssma-v71"></a>SSMA v 7.1

Die Version Version 7.1 von SSMA für MySQL enthält die folgenden Änderungen:

* SQL Server 2017 unter Windows und Linux CTP1 wird jetzt als Zielplattform für die Migration unterstützt. Diese Funktion befindet sich in der Technical Preview und ermöglicht das Schema und die Daten Verschiebung für SQL Server-Zielserver.
* SSMA unterstützt jetzt automatische Updates, um die neueste Version von SSMA herunterzuladen, sobald Sie verfügbar ist.
* Installierbare SSMA-Binärdateien werden jetzt über Windows Installer Paketdateien (MSI) übermittelt.

## <a name="may-2016"></a>Mai 2016

Die Version vom Mai 2016 von SSMA für MySQL enthält die folgenden Änderungen:

* Unterstützung für SQL Server 2016 hinzugefügt.
* Verbesserter Parser und Resolver.
* Die Installer-Überprüfung für .NET 2,0 wurde entfernt.
* Aktualisierte Extension Pack-Abhängigkeit von .NET 3,5 zu .NET 4,0.
* Korrigiert der standardmäßigen bigint-Typzuordnung für MySQL.
* Fixed `save-project` -und- `open-project` Befehle für die SSMA-Konsole.
* Fixed- `securepassword` Befehl für die SSMA-Konsole.
* Das zählen von Objekten für das anfängliche laden wurde korrigiert.
* Fehler beim Laden von MSSQL-Objekten.
* Fehler in globalen Einstellungen behoben.

## <a name="march-2016"></a>März 2016

Die Vorschauversion von SSMA für MySQL vom März 2016 bietet Unterstützung für die Migration zu SQL Server 2016.
  
## <a name="january-2016"></a>Januar 2016

Die Wartungsversion von SSMA für MySQL vom Januar 2016 enthält die folgenden Änderungen:

* Menü Element "View Log" zu SSMA hinzugefügt (RFC 5706203).
* Telemetrie hinzugefügt.
  
## <a name="july-2014"></a>Juli 2014

Die Version vom Juli 2014 von SSMA für MySQL enthält die folgenden Änderungen:
  
* Verbesserte Code Konvertierung in Azure SQL-Datenbank.
* Die Erweiterungspaket Funktionalität wurde zur Unterstützung von Azure SQL-Datenbank in das Schema verschoben.
* Leistungsverbesserungen, die für Datenbanken mit mehr als 10.000 Objekten getestet wurden.
* UI-Verbesserungen für den Umgang mit einer großen Anzahl von Objekten.
* Hervorhebung bekannter Lob-Schemas (sodass Sie bei der Konvertierung ignoriert werden können).
* Verbesserungen der Konvertierungsgeschwindigkeit
* Anzeigen der Objekt Anzahl in der Benutzeroberfläche.
* Verringerung der Berichts Größe um mehr als 25%.
* Verbesserte Fehlermeldungen für nicht analysierte Konstrukte.
  
## <a name="april-2014"></a>April 2014

Die Version vom April 2014 von SSMA für MySQL enthält die folgenden Änderungen:
  
* Unterstützung für SQL Server 2014 hinzugefügt.
* Fehler bei der Konvertierung in Azure korrigiert.
* Fehler in Bezug auf unsichtbare Berichts Seiten in IE 10 behoben.
  
## <a name="july-2011"></a>Juli 2011

Die Version vom Juli 2011 von SSMA für MySQL enthält die folgenden Änderungen:
  
* Unterstützung für die Konvertierung von `LIMIT` in [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] `OFFSET` .
* Verbesserte Fehlerberichterstattung während der Datenmigration.
  
## <a name="april-2011"></a>2011. April

Die Version vom April 2011 von SSMA für MySQL enthält die folgenden Änderungen:
  
* Einzelne installierbare "SSMA für MySQL", die [!INCLUDE [ssVersion2005](../../includes/ssversion2005-md.md)] , [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)] [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] und Azure SQL unterstützt.
* Die Möglichkeit, eine Verbindung herzustellen [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] .
* Verbessertes Client seitiges Daten Migrations Modul, das die parallele Migration von Daten unterstützt.
* Verbesserte Daten Migrations Leistung durch einfaches und Massen protokolliertes Wiederherstellungs Modell.
* Die Version der SSMA für MySQL-Konsole unterstützt die Abwärtskompatibilität. Sie können die Projekte öffnen, die von früheren Versionen von SSMA v 5.0 erstellt wurden.
* Das SSMA für MySQL v 5.0-Produkt kann nebeneinander (SxS) mit älteren Versionen des SSMA-Produkts installiert werden.
  
## <a name="july-2010"></a>Juli 2010

Die Version vom Juli 2010 von SSMA für MySQL enthält die folgenden Features:
  
**1. Verbesserungen an der Benutzeroberfläche:**

* Registerkarte "SQL-Modi" für MySQL-Datenbankobjekte
* Registerkarte "Einstellungen" für MySQL-Datenbankobjekte
* Registerkarte "Daten" für MySQL-Tabellen
* Aktualisierte Projekteinstellungen auf der Seite Konvertierung und Migration
* "Daten Migrations Einstellungen" auf Tabellenebene
  
**2. Verbesserungen beim Herstellen einer Verbindung mit MySQL und SQL Server:**
  
* SSL-Konnektivität in MySQL
* Verschlüsselte Konnektivität in SQL Server
  
**3. Verbesserungen am MySQL Metabase-Explorer:**
  
* Alle MySQL-Datenbankobjekte und ihre entsprechenden Registerkarten werden geladen.
  
**4. Verbesserungen bei der Objekt Konvertierung:**
  
* Konvertierung von MySQL-metabasisobjekten-Prozeduren, Funktionen, Sichten, Trigger und Anweisungen.
* Eingeschränkte Unterstützung für räumliche Datentypen in Tabellen.
* Option zum Konvertieren von MySQL-Funktionen in SQL Server gespeicherte Prozeduren
* Option zum Anwenden von SQL-Modi und der charset-Zuordnung während der Objekt Konvertierung
  
**5. Verbesserungen bei der Daten Migration:**  
  
* Unterstützung für die Datenmigration mit Server-Side und Client-Side Data Migration Engines
* Unterstützung für die Migration räumlicher Daten
* Benutzerdefiniertes SQL für die Daten Migration für Tabellen
  
**6. SSMA für MySQL-Konsole:**  
  
* Unterstützung der Konsolen Funktion für SSMA für MySQL  
* Unterstützung für Script-Level mit Schnittstellen  
  
## <a name="january-2010"></a>2010. Januar

Die Version vom Januar 2010 von SSMA für MySQL war die erste Version. Es enthielt die folgenden Features:
  
* Unterstützung für die Migration zu lokalen SQL Server und Azure SQL hinzugefügt.  
  
* **Merkmals Momentaufnahme:** Schema-und Datenmigration von MySQL-Tabellen/-Indizes/-Einschränkungen.
