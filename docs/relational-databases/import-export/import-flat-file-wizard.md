---
title: Importieren von Flatfiles in SQL | Microsoft-Dokumentation
description: Mit dem Assistenten zum Importieren von Flatfiles können Daten mühelos aus einer CSV- oder TXT-Datei in eine neue Datenbanktabelle kopiert werden. In diesem Artikel erfahren Sie, wie und wann der Assistent verwendet wird.
ms.custom: ''
ms.date: 09/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: data-movement
ms.topic: conceptual
f1_keywords:
- sql13.swb.importflatfile.f1
author: yualan
ms.author: alayu
ms.reviewer: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 444489bc74106ca43d645e26564543b4452ac8ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351348"
---
# <a name="import-flat-file-to-sql-wizard"></a>Assistent zum Importieren von Flatfiles in SQL
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
> Inhalte im Zusammenhang mit dem Import/Export-Assistenten finden Sie unter [SQL Server-Import/Export-Assistent](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

Mit dem Assistenten zum Importieren von Flatfiles können Daten mühelos aus einer Flatfile (CSV, TXT) in eine neue Tabelle in Ihrer Datenbank kopiert werden.  Der Assistent zum Importieren von Flatfiles unterstützt sowohl durch Kommas getrennte Formatdateien als auch Formatdateien mit fester Breite. In dieser Übersicht werden die Gründe für die Verwendung dieses Assistenten beschrieben. Außerdem erfahren Sie, wie er zu finden ist, und erhalten ein einfaches Beispiel, dem Sie folgen können.

## <a name="why-would-i-use-this-wizard"></a>Warum sollte ich diesen Assistenten verwenden?
Dieser Assistent wurde erstellt, um die aktuelle Importoberfläche mithilfe eines intelligenten Frameworks zu verbessern: Program Synthesis using Examples ([PROSE](https://microsoft.github.io/prose/)). Für einen Benutzer ohne spezielle Domänenkenntnisse kann das Importieren von Daten oft eine komplexe, fehleranfällige und mühsame Aufgabe darstellen. Dieser Assistent optimiert und vereinfacht den Importvorgang, sodass es genügt, eine Eingabedatei und einen eindeutigen Tabellennamen auszuwählen. Das PROSE-Framework kümmert sich um den Rest.

PROSE analysiert Datenmuster in Ihrer Eingabedatei, um daraus Spaltennamen, Typen, Trennzeichen und vieles mehr abzuleiten. Dieses Framework erlernt die Struktur der Datei und übernimmt den schwersten Teil der Arbeit, um die Benutzer zu entlasten.

Um noch mehr über die Verbesserung der Benutzerfreundlichkeit des Assistenten zum Importieren von Flatfiles zu erfahren, schauen Sie sich dieses Video an:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introducing-the-new-Import-Flat-File-Wizard-in-SSMS-173/player?WT.mc_id=dataexposed-c9-niner]

## <a name="prerequisites"></a>Voraussetzungen
Dieses Feature ist in SQL Server Management Studio (SSMS) v17.3 oder höher verfügbar. Stellen Sie sicher, dass Sie die neueste Version verwenden. Die neueste Version ist [hier](../../ssms/download-sql-server-management-studio-ssms.md) zu finden.
 
## <a name="getting-started"></a><a id="started"></a>Erste Schritte
Um den Assistenten zum Importieren von Flatfiles aufzurufen, gehen Sie folgendermaßen vor:

1. Öffnen Sie **SQL Server Management Studio**.
2. Stellen Sie eine Verbindung mit einer Instanz der SQL Server-Datenbank-Engine oder mit localhost her.
3. Erweitern Sie **Datenbanken**, klicken Sie mit der rechten Maustaste auf eine Datenbank („test“ im folgenden Beispiel), zeigen Sie auf **Aufgaben**, und klicken Sie oberhalb von „Daten importieren“ auf **Flatfile importieren**.

![Assistent: Menü](media/import-flat-file-wizard/import-flat-file-menu.png)

Weitere Informationen über die verschiedenen Funktionen des Assistenten finden Sie im folgenden Tutorial:

## <a name="tutorial"></a>Lernprogramm
Für den Zweck dieses Tutorials können Sie gerne Ihre eigene Flatfile verwenden. Andernfalls verwendet dieses Tutorial die folgende CSV-Datei aus Excel, die Sie kopieren können. Wenn Sie diese CSV-Datei verwenden, benennen Sie sie **example.csv**, und stellen Sie sicher, dass Sie sie als CSV-Datei an einem leicht zugänglichen Speicherort wie z.B. dem Desktop speichern.

![Assistent: Excel](media/import-flat-file-wizard/import-flat-file-example.png)

Übersicht:
1. [Öffnen des Assistenten](#step-1-access-wizard-and-intro-page)
2. [Angeben der Eingabedatei](#step-2-specify-input-file)
3. [Datenvorschau](#step-3-preview-data)
4. [Bearbeiten von Spalten](#step-4-modify-columns)
5. [Zusammenfassung](#step-5-summary)
6. [Ergebnisse](#step-6-results)

### <a name="step-1-access-wizard-and-intro-page"></a>Schritt 1: Öffnen des Assistenten und der Einführungsseite
Öffnen Sie den Assistenten, wie [hier](#started) beschrieben wird.

Die erste Seite des Assistenten ist die Startseite. Wenn Sie diese Seite nicht mehr anzeigen möchten, klicken Sie einfach auf **Diese Anfangsseite nicht mehr anzeigen**.

![Assistent: Einführung](media/import-flat-file-wizard/import-flat-file-intro.png)

### <a name="step-2-specify-input-file"></a>Schritt 2: Angeben der Eingabedatei
Klicken Sie auf „Durchsuchen“, um die Eingabedatei auszuwählen. Der Assistent sucht standardmäßig nach CSV- und TXT-Dateien. PROSE erkennt unabhängig von der Dateierweiterung, ob die Datei durch Kommas getrennt ist oder ein Format mit fester Breite vorliegt.

Der neue Tabellenname muss eindeutig sein, andernfalls lässt der Assistent Sie nicht fortfahren.

![Assistent: Angabe](media/import-flat-file-wizard/import-flat-file-specify.png)

### <a name="step-3-preview-data"></a>Schritt 3: Datenvorschau
Der Assistent generiert eine Vorschau, die für die ersten 50 Zeilen angezeigt werden kann. Wenn Probleme auftreten, klicken Sie auf „Abbrechen“. Fahren Sie andernfalls mit der nächsten Seite fort.

![Assistent: Vorschau](media/import-flat-file-wizard/import-flat-file-preview.png)

### <a name="step-4-modify-columns"></a>Schritt 4: Bearbeiten von Spalten
Der Assistent identifiziert die richtigen Spaltennamen, Datentypen usw. Hier können Sie die Felder bearbeiten, wenn sie nicht korrekt sind (z.B. muss der Datentyp „float“ anstatt von „int“ lauten).

Für Spalten, in denen leere Werte erkannt werden, wird „NULL-Werte zulassen“ aktiviert. Wenn Sie in einer Spalte jedoch NULL erwarten und „NULL-Werte zulassen“ nicht aktiviert ist, können Sie hier die Tabellendefinition aktualisieren, um NULL-Werte in einer oder allen Spalten zuzulassen.

Fahren Sie fort, wenn Sie fertig sind.

![Assistent: Ändern](media/import-flat-file-wizard/import-flat-file-modify.png)

### <a name="step-5-summary"></a>Schritt 5: Zusammenfassung
Dies ist lediglich eine Übersichtsseite, auf der Ihre aktuelle Konfiguration angezeigt wird. Wenn Probleme vorliegen, können Sie zu den vorherigen Abschnitten zurückwechseln. Andernfalls wird durch Klicken auf „Fertig stellen“ der Importvorgang gestartet.

![Assistent: Zusammenfassung](media/import-flat-file-wizard/import-flat-file-summary.png)

### <a name="step-6-results"></a>Schritt 6: Ergebnisse
Diese Seite gibt an, ob der Import erfolgreich war. Wenn ein grünes Häkchen angezeigt wird, war er erfolgreich. Andernfalls müssen Sie möglicherweise Ihre Konfiguration oder Ihre Eingabedatei auf Fehler überprüfen.

![Assistent: Ergebnisse](media/import-flat-file-wizard/import-flat-file-results.png)

## <a name="troubleshooting"></a>Problembehandlung
Der Assistent zum Importieren von Flatfiles erkennt Datentypen anhand der ersten 200 Zeilen.  In Szenarien, in denen Daten an späterer Position in der Flatfile nicht den automatisch erkannten Datentypen entsprechen, tritt während des Imports ein Fehler auf. Die Fehlermeldung lautet etwa wie folgt:
```
Error inserting data into table. (Microsoft.SqlServer.Prose.Import)
The given value of type String from the data source cannot be converted to type nvarchar of the specified target column. (System.Data)
String or binary data would be truncated. (System.Data)
```
Taktiken zum Vermindern dieses Fehlers:
- Durch Erhöhen der Größenwerte von Datentypen im Schritt [Spalten bearbeiten](#step-4-modify-columns) (z. B. durch Verlängern einer nvarchar-Spalte) lassen sich Abweichungen bei Daten im Rest der Flatfile möglicherweise kompensieren.
- Durch Aktivieren der Fehlerberichterstellung im Schritt [Spalten bearbeiten](#step-4-modify-columns) (z. B. durch Angabe einer kleineren Zahl) lässt sich erkennen, welche Zeilen in der Flatfile Daten enthalten, die nicht den ausgewählten Datentypen entsprechen. In einer Flatfile, in der die zweite Zeile zu einem Fehler führt, erzeugt die Ausführung des Importvorgangs mit einer Fehlerberichterstellung im Bereich 1 eine bestimmte Fehlermeldung.  Untersuchen Sie die Datei direkt im Speicherort. So können Sie die Datentypen basierend auf den Daten in den identifizierten Zeilen zielgenauer ändern.

![Ergebnisse der Fehlerberichterstellung](media/import-flat-file-wizard/import-flat-file-error.png)

```
Error inserting data into table occured while inserting rows 1 - 2. (Microsoft.SqlServer.Prose.Import)
The given value of type String from the data source cannot be converted to type float of the specified target column. (System.Data)
Failed to convert parameter value from a String to a Double. (System.Data)
```


## <a name="learn-more"></a>Weitere Informationen

Weitere Informationen zum Assistenten.
 
- **Weitere Informationen zum Importieren anderer Quellen.** Wenn Sie außer Flatfiles noch andere Dateien importieren möchten, finden Sie weitere Informationen unter [SQL Server-Import/Export-Assistent](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).
- **Weitere Informationen zum Herstellen einer Verbindung mit Flatfilequellen.** Weitere Informationen zum Herstellen einer Verbindung mit Flatfilequellen finden Sie unter [Herstellen einer Verbindung mit einer Flatfile-Datenquelle](../../integration-services/import-export-data/connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard.md).
- **Weitere Informationen zu PROSE.** Eine Übersicht über das intelligente Framework, das von diesem Assistenten verwendet wird, finden Sie unter [PROSE SDK](https://microsoft.github.io/prose/).