---
title: Neues in SSMA für Oracle (oracleto SQL) | Microsoft-Dokumentation
description: Informieren Sie sich über Änderungen an SQL Server Migration Assistant (SSMA) für Oracle (oracledesql) für jede Version.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: f305ebb6-7393-4a43-abb3-6332b739d690
ms.author: alexiva
ms.openlocfilehash: 4dc76c57f7194c1ee96bd091d674b793dad29faa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081877"
---
# <a name="whats-new-in-ssma-for-oracle-oracletosql"></a>Neues in SSMA für Oracle (oracleto SQL)

In diesem Artikel wird SQL Server Migration Assistant (SSMA) für Oracle-Änderungen in jeder Version aufgeführt.

## <a name="ssma-v816"></a>SSMA v 8.16

Die v 8.16-Version von SSMA für Oracle enthält die folgenden Änderungen:

* Unterstützung für Identitäts Spalten hinzufügen
* Unterstützung für Legacy Parser entfernen
* Problem mit Objekten, die nicht aus der Datenbank aktualisiert werden

## <a name="ssma-v815"></a>SSMA v 8.15

Zusätzlich zu den Verbesserungen der Barrierefreiheit enthält die Version Version 8.15 von SSMA für Oracle folgende Änderungen:

* Einstellung zur Verwendung der vollständigen Typspezifikation für `%type` -und-Attribute hinzufügen `%rowtype`
* Revamp-Bewertungsberichte für die Arbeit in modernen Browsern
* Von der Datenbank zur Azure AD Authentifizierung bereitgestellte Autorität verwenden
* Benennen von aus Dateien geladenen Anweisungen verbessern

## <a name="ssma-v814"></a>SSMA v 8,14

Zusätzlich zu den Verbesserungen, um mehr Barrierefreiheit für Personen mit Behinderungen sicherzustellen, enthält die v-Version von SSMA für Oracle die folgenden Änderungen:

* Speichert die vollständige Quell-/zielserverversion in den Projekt Metadaten (erfordert ein Projekt Upgrade).
* Verwenden Sie nach Möglichkeit DBA-Daten Wörterbücher für die Objekt Ermittlung
* Beheben mehrerer Parser-Probleme ( `PIVOT` / `UNPIVOT` , `MERGE` , Alternative Anführungs Punkte)
* Korrektur der Konvertierung `INSERTING` / `DELETING` / `UPDATING` spezieller Funktionen in Triggern

## <a name="ssma-v813"></a>SSMA v 8,13

Die Version 8,13 von SSMA für Oracle enthält die folgenden Änderungen:

* Korrektur der Konvertierung von `SQLCODE` und `SQLERRM` speziellen Funktionen in lokalen Prozeduren
* Implizite Typumwandlungen bei der Umwandlung von Prozeduren und Funktionsaufrufen beachten
* Verbessern der Protokollierung für die Quell Verbindungs Zeichenfolge zur Behandlung von Verbindungsproblemen

## <a name="ssma-v812"></a>SSMA v 8.12

Die Version Version Version von SSMA für Oracle enthält die folgenden Änderungen:

* Unterstützung für Unterabfrage-Factoring-Klauseln in- `INSERT` / `UPDATE` / `MERGE` / `DELETE` Anweisungen
* Konvertierungs Nachricht für `ON DELETE SET NULL` Klausel in Fällen von Multipfad-oder Zirkel verweisen
* Verbesserte Konvertierung von durch dynamische SQL-Zeichenfolge erstellten Cursorn
* ODP.net auf v 19,8 aktualisieren

## <a name="ssma-v811"></a>SSMA v 8.11

Die Version Version 8.11 von SSMA für Oracle enthält die folgenden Änderungen:

* Unterstützung für Unterabfragen in der `INSERT ... VALUES` Anweisung
* Verbesserte Konvertierung der `COMMIT` Anweisung
* Korrigieren der Fehler in der `CONNECT BY LEVEL` klauselkonvertierung
* Die Wiederherstellungs Logik für Parserfehler wurde aktualisiert und ist weniger gierig
* Verwenden der MSAL.NET-Bibliothek für die interaktive Azure Active Directory Authentifizierung

## <a name="ssma-v810"></a>SSMA v 8.10

Die Version "v 8.10" von SSMA für Oracle enthält geringfügige Leistungsverbesserungen sowie die folgenden Änderungen:

* Behebung des Tester-Problems mit Index organisierten Tabellen
* Behebung der Namen erweiterter gespeicherter Prozeduren im Erweiterungspaket

## <a name="ssma-v89"></a>SSMA v 8,9

Das v 8,9-Release von SSMA für Oracle enthält die folgenden Änderungen:

* Konvertierung dynamischer SQL-Zeichen folgen Literale
* Konvertierung für `LAG` `FIRST_VALUE` und `LAST_VALUE` analytische Funktionen
* Unterstützung für grundlegende `ALTER TRIGGER` / `ALTER INDEX` DDL hinzufügen (aktivieren/deaktivieren usw.)
* Verbesserte Konvertierung für Spalten, die den Namen integrierter Funktionen entsprechen
* Generieren von gefilterten eindeutigen Indizes für `NULL` -fähige Spalten
* Verbesserte Konvertierung der Variablen Deklaration für Azure Synapse Analytics
* Behebung des Problems mit Sonderzeichen in Project Name

## <a name="ssma-v88"></a>SSMA v 8.8

Die Version 8.8 von SSMA für Oracle umfasst Folgendes:

* Verbesserungen der Synchronisierungs Stabilität SQL Server
* Leistungsverbesserungen bei der GUI während der Bewertung und Konvertierung
* Verbesserte Konvertierung von analytischen `OVER PARTITION` Klauseln
* Neue Konvertierung für `LEAD` analytische Funktion
* Neue Konvertierung für Unterabfrage-factoringklauseln
* Neue `REPLICATE` Verteilungs Option für Azure Synapse Analytics
* Brand New Oracle Syntax Parser, um die Konvertierungs Leistung weiter zu verbessern

## <a name="ssma-v87"></a>SSMA v 8.7

Das v 8.7-Release von SSMA für Oracle weist kleinere Korrekturen und Leistungsverbesserungen in der grafischen Benutzeroberfläche auf.

Außerdem ermöglicht SSMA für Oracle jetzt das Filtern von Objekten basierend auf dem Gültigkeits Status im Dialogfeld "Erweiterte Objektauswahl".

> [!IMPORTANT]
> Mit SSMA Version 8.5 und höher ist .NET 4.7.2 eine erforderliche Installation. Wenn Sie diese Version installieren müssen, können Sie die Lauf Zeit Datei [hier](https://dotnet.microsoft.com/download/dotnet-framework/net472)herunterladen.

## <a name="ssma-v86"></a>SSMA v 8.6

Zusätzlich zu einem Zielsatz von Korrekturen, die zur Verbesserung der Benutzerfreundlichkeit und Leistung entwickelt wurden, wurde die Version Version 8.6 von SSMA für Oracle durch Hinzufügen einer Einstellung verbessert, mit der Benutzer erweiterte SSMA-Eigenschaften im konvertierten Code weglassen können.

Um diese Einstellung zu nutzen, navigieren Sie in SSMA für Oracle **zu Extras**  >  **Projekteinstellungen**  >  **Allgemeine**  >  **Konvertierung**, und aktualisieren Sie dann unter " **misc**" den Wert der Einstellung **Erweiterte Eigenschaften** unterdrücken auf **Ja**.

![Einstellung für erweiterte Eigenschaften weglassen](../oracle/media/ssma-omit-extended-properties.png)

Außerdem bietet SSMA für Oracle jetzt eine verbesserte Analyse der- `XMLTABLE` Klausel.

> [!IMPORTANT]
> Mit SSMA Version 8.5 und höher ist .NET 4.7.2 eine erforderliche Installation. Wenn Sie diese Version installieren müssen, können Sie die Lauf Zeit Datei [hier](https://dotnet.microsoft.com/download/dotnet-framework/net472)herunterladen.

## <a name="ssma-v85"></a>SSMA v 8.5

Die Version v 8.5 von SSMA für Oracle wurde mit Unterstützung für Azure Active Directory Authentifizierung und grundlegender Unterstützung für JSON-Funktionen in SQL Server erweitert, sowie eine Reihe gezielter Korrekturen zur Verbesserung der Benutzerfreundlichkeit und Leistung.

Außerdem wurde SSMA für Oracle durch folgende Unterstützung erweitert:

* Begrenzen der Anzahl ausgewählter Objekte für die Ermittlung auf 990 (das Limit von Oracle- `WHERE .. IN (..)` Klauseln beträgt 1000 Elemente).
* Datenmigration von `RAW` zu `UNIQUEIDENTIFIER` .
* Die-Klausel wird verarbeitet `PARALLEL_ENABLE` .

Schließlich bietet das Release v 8.5 von SSMA für Oracle nun Folgendes:

* Verbesserte Leistung von konvertierten verpackten Konstanten.
* Ein Update für Oracle Datenanbieter für .net auf Version 19.5.0.

> [!IMPORTANT]
> Mit SSMA v 8.5 ist .NET 4.7.2 eine Installation, die Voraussetzung ist. Wenn Sie diese Version installieren müssen, können Sie die Lauf Zeit Datei [hier](https://dotnet.microsoft.com/download/dotnet-framework/net472)herunterladen.

## <a name="ssma-v84"></a>SSMA v 8.4

Die Version Version 8.4 von SSMA für Oracle wurde durch gezielte Korrekturen ergänzt, die zur Behebung von Problemen bei der Barrierefreiheit entwickelt wurden, und zum Beheben eines Fehlers im Zusammenhang mit den maximalen Index Spalten (um 32 anstelle von 16) für SQL Server 2016 und höhere Versionen zu beheben.

Außerdem wird in dieser Version von SSMA für Oracle die Konvertierung von `SYS_REFCURSOR` als Parameter für gespeicherte Prozeduren hinzugefügt `OUT` .

> [!IMPORTANT]
> Bei den SSMA-Versionen 7,4 bis 8,4 ist .NET 4.5.2 eine erforderliche Installation.

## <a name="ssma-v83"></a>SSMA v 8.3

Das v 8.3-Release von SSMA für Oracle wurde durch gezielte Korrekturen verbessert, die zur Verbesserung der Qualität und der konvertierungsmetriken entwickelt wurden. Außerdem bietet diese Version von SSMA für Oracle Korrekturen, die Folgendes beinhaltet:

* Behandeln von Problemen bei der Barrierefreiheit
* Fügen Sie grundlegende Unterstützung für `hierarchyid` Typ in SQL Server hinzu.
* Beheben Sie ein Problem mit einem unbekannten Rückgabetyp für eine Funktion, die über das Synonym aufgerufen wird.
* Aktualisieren Sie ODP.net auf v 19,3.

## <a name="ssma-v82"></a>SSMA v 8.2

Die Version 8.2 von SSMA für Oracle wurde wie folgt erweitert:

* Fügen Sie Unterstützung für hinzu `DBMS_OUTPUT.ENABLE` / `DISABLE` .
* Entfernen Sie `CAST AS FLOAT` für die `BINARY_FLOAT` `BINARY_DOUBLE` Spalten und in der standardmäßigen Daten Migrations Abfrage.
* Korrektur der Sequenz Aktualisierung, wenn sich der aktuelle Wert geändert hat.
* Beheben Sie den Fehler im Zusammenhang mit der Fehlinterpretation von Pseudo Spalten ( `ROWNUM` usw.), wenn eine Spalte mit demselben Namen vorhanden ist.
* Korrektur eines Absturzes, der beim Umrechnen `FOR` von Schleifen mit mehrdeutigem nicht aufgelösten Bezeichner

Darüber hinaus enthält diese Version einen Satz von Korrekturen, die für die Verbesserung von Qualitäts-und konvertierungsmetriken entworfen wurden, sowie Fehlerbehebungen für:

* Ein Problem mit deaktivierten nicht gruppierten Indizes nach der Datenmigration.
* Erkennung von .NET Framework während der automatischen Installation.
* Ein zeitweiliger Absturz, der auftritt, wenn eine neue Version heruntergeladen wird.

> [!NOTE]
> Ein bekanntes Problem bei der automatischen Aktualisierung kann zu einem Fehler bei einem Update von SSMA v 8.1 auf v 8.2 führen. Wenn dieser Fehler auftritt, laden Sie die neue Version herunter, und installieren Sie Sie manuell.

## <a name="ssma-v81"></a>SSMA v 8.1

Die Version 8.1 von SSMA für Oracle wurde durch gezielte Korrekturen ergänzt, die zur Verbesserung der Qualität und der konvertierungsmetriken entwickelt wurden.

> [!NOTE]
> Ein bekanntes Problem bei der automatischen Aktualisierung kann zu einem Fehler bei einem Update von SSMA v 8.0 auf v 8.1 führen. Wenn dieser Fehler auftritt, laden Sie die neue Version herunter, und installieren Sie Sie manuell.

## <a name="ssma-v80"></a>SSMA v 8.0

Das v 8.0-Release von SSMA für Oracle wurde durch gezielte Korrekturen verbessert, die zur Verbesserung der Qualität und der konvertierungsmetriken entworfen wurden. Diese Version bietet auch die folgenden neuen Features:

* Unterstützung für **Azure SQL-verwaltete Instanz** als Ziel. Sie können jetzt neue Projekte erstellen, die auf Azure SQL-verwaltete Instanz abzielen:

  ![SQL-Mi-Projekt](../media/ssma-newproject-sqldbmi.png)

  > [!NOTE]
  > Das SSMA für Oracle Extension Pack wurde ebenfalls aktualisiert und ermöglicht Remote Installationen auf Azure SQL-verwaltete Instanz:
  >
  > ![SSMA für Oracle-Erweiterungspaket](../media/ssma-oracle-ext-pack.png)

  Einige Features, einschließlich Tester und Server seitiger Datenmigration, werden nicht unterstützt, wenn Sie auf Azure SQL-verwaltete Instanz abzielen. Weitere Informationen hierzu finden Sie [hier](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/migrate-your-oracle-database-to-azure-sql-database-managed-instance-using-ssma-8-0/).

* **Korrektur Ratgeber** nach der Konvertierung. Weitere Informationen hierzu [finden Sie hier](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/).

* Vorläufige Datenbank/Schema-Auswahl.

  Beim Herstellen einer Verbindung mit der Quelle kann der Benutzer jetzt Datenbanken/Schemas von Interesse auswählen. Wenn Sie nur die Schemas auswählen, für die die Migration geplant ist, sparen Sie Zeit während der ersten Verbindung und verbessern die SSMA-Gesamtleistung.

  ![SSMA-Filter Objekte](../media/ssma-filter-objects.png)

* Die Möglichkeit, den offiziellen, verwalteten .NET-Treiber zum Herstellen einer Verbindung mit Oracle zu verwenden. Der OCI-Treiber ist keine Voraussetzung für die Verwendung von SQL Server Migration Assistant für Oracle.

* Die Möglichkeit zum Zuordnen `ROWID` `UROWID` von und zu `VARCHAR` standardmäßig. Wurde von geändert `uniqueidentifier` , um die Datenmigration für explizite Spalten zu ermöglichen `ROWID` .

## <a name="ssma-v710"></a>SSMA v 7.10

Das v 7.10-Release von SSMA für Oracle enthält die folgenden Änderungen:

* Gezielte Korrekturen zur Bereitstellung zusätzlicher Sicherheits-und Datenschutz Maßnahmen zum erfüllen von Änderungen an globalen Anforderungen.
* Eine Konvertierungs Verbesserung im Zusammenhang mit hierarchischen Abfragen.

## <a name="ssma-v79"></a>SSMA v 7.9

Die Version Version 7.9 von SSMA für Oracle enthält die folgenden Änderungen:

* Gezielte Korrekturen, die Qualitäts-und konvertierungsmetriken verbessern.
* Unterstützung für die Migration von "Continue"-Anweisungen von Oracle zu SQL Server.
* Unterstützung in der SSMA-Befehlszeile, um Datentyp Zuordnung und Projekteinstellungen zu ändern.
* Unterstützung für das Migrieren von Daten mithilfe von SQL Server Integration Services (SSIS). Nach dem Umrechnen des Schemas ist es möglich, ein SSIS-Paket zu erstellen, indem Sie mit der rechten Maustaste auf die Kontextmenü Option klicken.
* Das Azure SQL-Daten bankverbindungs Dialogfeld in SSMA wurde ebenfalls geändert, um den voll qualifizierten Servernamen anzugeben. In früheren Versionen von SSMA musste das Azure SQL-Daten Bank Präfix in den Projekteinstellungen explizit angegeben werden.

## <a name="ssma-v78"></a>SSMA v 7.8

Das Release v 7.8 von SSMA für Oracle enthält die folgenden Änderungen:

* Unterstützung für:
  * Zeilen Ausdruck für die- `IN` Klausel.
  * Implizite Typumwandlungen.
  * `UID` Konvertierung für Azure SQL-Datenbank.
* Ändern Sie in den **Projekteinstellungen** hervorgehobene Typzuordnung.
* Die Möglichkeit für Benutzer, Telemetriedaten zu deaktivieren.

## <a name="ssma-v77"></a>SSMA v 7.7

Das Release v 7.7 von SSMA für Oracle enthält die folgenden Änderungen:

* SSMA für Oracle wurde durch gezielte Korrekturen verbessert, die Qualitäts-und konvertierungsmetriken verbessern.
* Basierend auf der beliebten Nachfrage ist die 32-Bit-Version von SSMA für Oracle wieder verfügbar. Im Vergleich zur vorherigen Implementierung (vor v 7.4) gibt es zwei Installer-Pakete, aber Sie können nicht parallel installiert werden. Daher müssen Sie die am besten geeignete Version basierend auf den Konnektivitätskomponenten auswählen, die Sie besitzen. Es ist immer ratsam, die 64-Bit-Version zu verwenden, wenn möglich.
* SQL Server 2017-Unterstützung ist jetzt auch mit dem Oracle-Erweiterungspaket, das unter Linux unterstützt wird (neue Remote Installationsoption). Beachten Sie, dass die Extension Pack-Funktionalität bei der Installation unter Linux eingeschränkt ist, da der Tester und die serverseitigen Daten Migrations Funktionen nicht unterstützt werden.
* SSMA für Oracle ermöglicht Ihnen das Migrieren materialisierter Sichten als reguläre Tabellen (konfigurierbar durch die Einstellungen unter **Projekteinstellungen**  ->  **Synchronisierung** Ermittlungs  ->  **Tabellen für materialisierte Sichten ermitteln**).

## <a name="ssma-v76"></a>SSMA v 7.6

Die Version Version 7.6 von SSMA für Oracle wurde durch gezielte Korrekturen ergänzt, die Qualitäts-und konvertierungsmetriken sowie die Unterstützung für SQL Server 2017 (öffentliche Vorschau) verbessern. Unterstützung für SQL Server 2017 unter Windows und Linux befindet sich in der öffentlichen Vorschau Phase und sollte nicht für Produktions Migrationen verwendet werden.

## <a name="ssma-v75"></a>SSMA v 7.5

Das Release v 7.5 von SSMA für Oracle enthält die folgenden Änderungen:

* Verbessert durch mehrere Verbesserungen, um mehr Barrierefreiheit für Menschen mit Behinderungen sicherzustellen.
* Das Thema wurde aktualisiert, um die Qualitäts-und Konvertierungs Metrik mit gezielten Korrekturen zu verbessern, z. b. eine verbesserte Behandlung von Datums-und Gleit Komma Datentypen während der Datenmigration

## <a name="ssma-v74"></a>SSMA v 7.4

Die Version Version 7.4 von SSMA für Oracle enthält die folgenden Änderungen:

* SSMA für Oracle unterstützt jetzt Azure Synapse-Analysen als Zielplattform für die Migration.

  ![Fenster „Neues Projekt“](../media/new-project.png)
  * Unterstützt die Data Warehouse Speicheroptionen, wie in der folgenden Abbildung dargestellt:

  ![Speicheroptionen für Data Warehouse](../media/storage-options_red.png)
  * Unterstützt die Daten Verteilungs Optionen, wie in der folgenden Abbildung dargestellt:

  ![Datenverteilung für Data Warehouse](../media/data-distribution_red.png)

* Die Option **Abfrage Timeout** ist jetzt bei der Schema Objekt Ermittlung unter Quelle und Ziel verfügbar.

  ![Abfrage Timeout (Option)](../media/query-timeout_red.png)

* Die Qualitäts-und Konvertierungs Metrik wurde durch gezielte Korrekturen verbessert, basierend auf Kundenfeedback.

> [!IMPORTANT]
> .NET 4.5.2 ist eine Voraussetzung für die Installation von SSMA v 7.4. Außerdem wird ab Version 7.4 die 32-Bit-Version von SSMA eingestellt.

## <a name="ssma-v73"></a>SSMA v 7.3

Das Release v 7.3 von SSMA für Oracle enthält die folgenden Änderungen:

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

Die Version Version 7.2 von SSMA für Oracle enthält die folgenden Änderungen:

* Verbesserte Qualitäts-und Konvertierungs Metrik durch gezielte Korrekturen auf der Grundlage von Kundenfeedback.
* Telemetrie-Verbesserungen, um bessere Datenpunkte zur Behandlung von Kundenproblemen und zur Verbesserung der Konvertierungsraten von SSMA bereitzustellen.

## <a name="ssma-v71"></a>SSMA v 7.1

Die Version Version 7.1 von SSMA für Oracle enthält die folgenden Änderungen:

* SQL Server 2017 unter Windows und Linux CTP1 wird jetzt als Zielplattform für die Migration unterstützt. Diese Funktion befindet sich in der Technical Preview und ermöglicht das Schema und die Daten Verschiebung für SQL Server-Zielserver.
* SSMA unterstützt jetzt automatische Updates, um die neueste Version von SSMA herunterzuladen, sobald Sie verfügbar ist.
* Installierbare SSMA-Binärdateien werden jetzt über Windows Installer Paketdateien (MSI) übermittelt.

## <a name="may-2016"></a>Mai 2016

Die Version vom Mai 2016 von SSMA für Oracle enthält die folgenden Änderungen:

* Unterstützung für SQL Server 2016 hinzugefügt.
* Die Konvertierung von Oracle-Flashback-Archiv Tabellen in SQL Server Temporale Tabellen wurde hinzugefügt.

  > [!NOTE]
  > SSMA kopiert keine Verlaufs Daten aus Oracle-Datenarchiv Tabellen. Folglich müssen die Verlaufs Daten während des Migrations Vorgangs manuell kopiert werden. Außerdem wird die Verlaufs Tabelle in SQL Server Management Studio angezeigt, während SSMA die Verlaufs Tabelle nicht im SQL Server Metadaten-Explorer anzeigt, da Sie als Systemtabelle behandelt wird.
  >
  > SQL Server 2016 bietet keine Unterstützung für mehrere Oracle-Flash Features, einschließlich:
  >
  >   * Abfrage der Oracle-flashbacktransaktion
  >   * `DBMS_FLASHBACK` Paket
  >   * Rückblenden der Transaktion
  >   * Rückblenden von Daten Archive
  >   * Tabelle "Flashback"
  >   * Dropdown-Drop
  >   * Flashback-Datenbank

* Die Konvertierung der Oracle-VPD-Richtlinie in SQL Server Richtlinien Objekte (Sicherheit auf Zeilenebene für Oracle) wurde hinzugefügt.
* Geringere Zeit beim ersten Laden für Oracle.
* Verbesserter Parser und Resolver.
* Die Installer-Überprüfung für .NET 2,0 wurde entfernt.
* Aktualisierte Extension Pack-Abhängigkeit von .NET 3,5 zu .NET 4,0.
* Fixed `save-project` -und- `open-project` Befehle für die SSMA-Konsole.
* Fixed- `securepassword` Befehl für die SSMA-Konsole.
* Das zählen von Objekten für das anfängliche laden wurde korrigiert.
* Korrigieren von Zeichen Datentypen für Oracle korrigiert.
* Fehler in globalen Einstellungen behoben.

## <a name="march-2016"></a>März 2016

In der Vorschauversion von März 2016 von SSMA für Oracle wurde die Unterstützung für hinzugefügt:

* Migration zu SQL Server 2016.
* Migrieren von Oracle-Sicherheit auf Zeilenebene (mit einigen Einschränkungen).
* Migrieren von Oracle in Memory-Tabellen zu SQL Server columnstore.

## <a name="january-2016"></a>Januar 2016

Die Wartungsversion von SSMA für Oracle vom Januar 2014 enthält die folgenden Änderungen:

* Unterstützung für gruppierte Indizes hinzugefügt.
* Korrigieren langsamer Oracle-Schema Abfragen (RFC 5076207).
* Fehler beim Herstellen einer Verbindung mit Azure über die Konsole.
* Menü Element "View Log" zu SSMA hinzugefügt (RFC 5706203).
* Telemetrie hinzugefügt.

## <a name="july-2014"></a>Juli 2014

Die Version vom Juli 2014 von SSMA für Oracle enthält die folgenden Änderungen:

* Unterstützung für Azure SQL-Datenbank hinzugefügt.
* Die Erweiterungspaket Funktionalität wurde zur Unterstützung von Azure SQL-Datenbank in das Schema verschoben.
* Unterstützung für in Oracle materialisierte Sichten hinzugefügt.
* Unterstützung für SQL Server 2014-Speicher optimierte Tabellen hinzugefügt.
* Enthaltene Leistungsverbesserungen, die für Datenbanken mit mehr als 10.000 Objekten getestet wurden.
* Verbesserungen der Benutzeroberfläche für den Umgang mit einer großen Anzahl von Objekten.
* Hervorhebung bekannter Lob-Schemas hinzugefügt.
* Verbesserungen der Konvertierungsgeschwindigkeit.
* Unterstützung für die Anzeige der Objekt Anzahl in der Benutzeroberfläche hinzugefügt
* Reduzierte Berichts Größe um mehr als 25%.
* Verbesserte Fehlermeldungen für nicht analysierte Konstrukte.

## <a name="april-2014"></a>April 2014

Die Version vom April 2014 von SSMA für Oracle enthält die folgenden Änderungen:

* Unterstützung von MS SQL Server 2014 hinzugefügt.
* Unterstützung von Oracle 12 und der Abfrageoptimierung hinzugefügt.
* Fehler bei der Konvertierung in Azure korrigiert.
* Fehler in Bezug auf unsichtbare Berichts Seiten in IE 10 behoben.

## <a name="january-2012"></a>Januar 2012

Die Version vom Januar 2012 von SSMA für Oracle fügt Unterstützung für `RowType` -und-Eingabeparameter hinzu, die standardmäßig `RecordType` auf `NULL` .

## <a name="july-2011"></a>Juli 2011

Die Version vom Juli 2011 von SSMA für Oracle enthält die folgenden Änderungen:

* Unterstützung für die Konvertierung der Oracle-Sequenz in den [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] Sequence Generator hinzugefügt.
* Verbesserte Fehlerberichterstattung während der Datenmigration.
* Verbesserte Konvertierung der Anweisung mit reservierten Wörtern.
* Verbesserte implizite Konvertierung des Datums Werts in eine Funktion.

## <a name="april-2011"></a>2011. April

Die Version vom April 2011 von SSMA für Oracle enthält die folgenden Änderungen:

* Konsolidiertes "SSMA für Oracle"-Produkt, das [!INCLUDE [ssVersion2005](../../includes/ssversion2005-md.md)] , und unterstützt [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)] [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] .
* Unterstützung für die Verbindungs Herstellung und die Migration zu hinzugefügt [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)]
* Verbessertes Client seitiges Daten Migrations Modul, das die parallele Migration von Daten unterstützt.
* Verbesserte Daten Migrations Leistung mit `Simple` und `Bulk` protokollierten Wiederherstellungs Modellen.
* Unterstützung für die Abwärtskompatibilität von Projekten hinzugefügt, die von früheren Versionen von SSMA (v 4.0 und v 4.2) erstellt wurden.
* Die Möglichkeit zur parallelen Installation von SSMA für Oracle v 5.0 (SxS) mit älteren Versionen von SSMA (v 4.0 und v 4.2) wurde hinzugefügt.
* Unterstützung für die Berichterstellung User-Defined Typen (einschließlich Untertyp, `VARRAY` , `NESTED TABLE` , Objekttabelle und Objekt Ansicht) und ihrer Verwendungen in PL/SQL-Blöcken mit speziellen Fehlermeldungen hinzugefügt.

## <a name="july-2010"></a>Juli 2010

Die Version vom Juli 2010 von SSMA für Oracle hat Folgendes hinzugefügt:

* Unterstützung für die Migration zu SQL Server 2008 R2.
* Eine neue SSMA-Konsolenanwendung für die Befehlszeilen Ausführung.
* Unterstützung für die Datenmigration mit Server-Side und Client-Side Datenmigrations-Engines.
* Unterstützung für die "Custom Select"-Anweisung bei der Datenmigration.
* Unterstützung für die Migration von Oracle 11g R2.

## <a name="june-2008"></a>2008. Juni

Die Version vom Juni 2008 von SSMA für Oracle enthält die folgenden Änderungen:

* Es wurden Verbesserungen am Bewertungsbericht hinzugefügt, einschließlich zusätzlicher Informationen zu Synonymen, Rohdaten Quelle für die Analyse von Objekten, Panels und SQL Server logoentfernung und layoutpersistenz.
* Verbesserungen bei der Objekt Konvertierung:
  * Pakete `DBMS_LOB` , `DBMS_SQL` Konvertierung hinzugefügt.
  * Die Joins wurden überarbeitet.
  * Änderung der Auflistung von Auflistungen und Einträgen, die Konvertierung von Datensätzen in einfachen Fällen, die für jedes Feld über separate Variablen freigegeben werden.
  * Verbesserungen der Implementierung von Datensätzen und Sammlungen.
  * Hinzugefügte Fenster Aggregations Funktionen.
  * `ROLLUP`/`CUBE` hinzugefügte Klausel.
  * Verbesserung für `NEXTVAL` / `CURVAL` .
  * Spalten Gruppierung in `SET` -Klausel, Gruppierungs Gruppen und Gruppierungs-ID wurden hinzugefügt.
  * `MERGE` Anweisung hinzugefügt.
  * Unterstützung neuer DateTime-Typen und Konvertierung von Datensätzen und Sammlungen als CLR-Datentypen hinzugefügt.
* Neue Features von Tester hinzugefügt. Tabellen können jetzt mithilfe von Tester als Objekte getestet werden, eine Aufrufreihenfolge mehrerer testfähiger Objekte im Testfall kann geändert werden, Benutzer können Prozeduren und Funktionen mit Datensätzen und Auflistungen als Parameter und Rückgabewerte testen, und es wurde eine Abhängigkeits Analyse hinzugefügt, um nur verwendete Tabellen zu überprüfen.
  
## <a name="august-2007"></a>2007. August

Die Version vom August 2007 von SSMA für Oracle hat Folgendes hinzugefügt:

* Mit einer neuen Tester-Komponente können Sie Testfälle erstellen, verwalten und ausführen, um den konvertierten SQL-Code zu überprüfen.
* Die Unterstützung für die Konvertierung von Oracle-Untertypen,-Auflistungen und lokalen Modulen wurde dem SQL-Konverter hinzugefügt.
* Mit einer neuen Synchronisierungs Funktion können Sie bestimmte Objekte mit SQL Server Datenbank synchronisieren.
* Neue Konvertierungsoptionen.
  
## <a name="april-2007"></a>2007. April

Die Version vom April 2007 von SSMA für Oracle war die erste Version.
