---
title: Was ist Azure Data Studio?
description: Azure Data Studio ist ein kostenloses, schlankes Tool, das unter Windows, macOS und Linux ausgeführt werden kann und zur Verwaltung von SQL Server, Azure SQL-Datenbank und Azure Synapse Analytics dient.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: overview
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18, sqlfreshmay19
ms.date: 10/20/2020
ms.openlocfilehash: b9adac14bfd458b96fe4e46c478af2d2b5d6a181
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464869"
---
# <a name="what-is-azure-data-studio"></a>Was ist Azure Data Studio?

Azure Data Studio ist ein plattformübergreifendes Datenbanktool für Datenexperten, das die lokalen und cloudbasierten Datenplattformen unter Windows, macOS und Linux nutzt.

Azure Data Studio bietet eine moderne Editor-Benutzeroberfläche mit IntelliSense, Codeausschnitten und Integration der Quellcodeverwaltung sowie ein integriertes Terminal. Das Tool wurde speziell für die Bedürfnisse der Benutzer von Datenplattformen konzipiert und bietet eine integrierte Diagrammdarstellung von Abfrageresultsets sowie anpassbare Dashboards.

Der Quellcode für Azure Data Studio und den dazugehörigen Datenanbietern ist auf GitHub verfügbar und Gegenstand eines Lizenzvertrags für Quellcode, gemäß dem die Software zwar geändert und verwendet, aber nicht weitervertrieben oder auf einem Clouddienst gehostet werden darf. Weitere Informationen finden Sie in den [Häufig gestellten Fragen zu Azure Data Studio](faq.yml).

**[Herunterladen und Installieren von Azure Data Studio](./download-azure-data-studio.md)**

## <a name="sql-code-editor-with-intellisense"></a>SQL-Code-Editor mit IntelliSense

Azure Data Studio bietet ein modernes Benutzererlebnis für die Programmierung in SQL. Das Tool erleichtert Ihnen die tägliche Arbeit, da Sie viele Funktionen über die Tastatur ansteuern können und zudem von einer Reihe integrierter Features profitieren: Fenster mit mehreren Registerkarten, umfangreicher SQL-Editor, IntelliSense, Vervollständigung von Schlüsselwörtern, Codeausschnitte, Codenavigation und Integration der Quellcodeverwaltung (Git). Sie können SQL-Abfragen bedarfsgesteuert ausführen und Ergebnisse im Text-, JSON- oder Excel-Format anzeigen und speichern. Bearbeiten Sie Daten, organisieren Sie Ihre bevorzugten Datenbankverbindungen und durchsuchen Sie Datenbankobjekte in einem vertrauten Objektbrowser. Informationen zum Verwenden des SQL-Editors finden Sie unter [Verwenden des SQL-Editors zum Erstellen von Datenbankobjekten](tutorial-sql-editor.md).

## <a name="smart-sql-code-snippets"></a>Intelligente SQL-Codeausschnitte

SQL-Codeausschnitte generieren die richtige SQL-Syntax zum Erstellen von Datenbanken, Tabellen, Ansichten, gespeicherten Prozeduren, Benutzern, Anmeldungen, Rollen und zum Aktualisieren vorhandener Datenbankobjekte. Verwenden Sie intelligente Codeausschnitte, um zu Entwicklungs- oder Testzwecken schnell Kopien Ihrer Datenbank zu erstellen und um Skripts für CREATE- und INSERT-Vorgänge zu generieren und auszuführen.

Azure Data Studio stellt auch Funktionen zum Erstellen benutzerdefinierter SQL-Codeausschnitte bereit. Weitere Informationen finden Sie unter [Erstellen und Verwenden von Codeausschnitten](code-snippets.md).

## <a name="customizable-server-and-database-dashboards"></a>Anpassbare Server- und Datenbankdashboards

Erstellen Sie informative, anpassbare Dashboards, um Ihre Datenbanken zu überwachen und Leistungsengpässe schnell zu beheben. Informationen zu Erkenntniswidgets sowie Dashboards für Datenbanken (und Server) finden Sie unter [Verwalten von Servern und Datenbanken mit Erkenntniswidgets](insight-widgets.md).

## <a name="connection-management-server-groups"></a>Verbindungsverwaltung (Servergruppen)

Servergruppen stellen eine Möglichkeit dar, Informationen zu Verbindungen mit den Servern und Datenbanken zu organisieren, mit denen Sie arbeiten. Weitere Informationen dazu finden Sie unter [Servergruppen](server-groups.md).

## <a name="integrated-terminal"></a>Integriertes Terminal

Sie können die von Ihnen bevorzugten Befehlszeilentools (z. B. Bash, PowerShell, sqlcmd, bcp und ssh) auf der Azure Data Studio-Benutzeroberfläche direkt im Fenster „Integriertes Terminal“ verwenden. Weitere Informationen zum integrierten Terminal finden Sie unter [Integriertes Terminal](integrated-terminal.md).

## <a name="extensibility-and-extension-authoring"></a>Erweiterbarkeit und Erstellen von Erweiterungen

Optimieren Sie das Benutzererlebnis in Azure Data Studio, indem Sie die Funktionalität der Basisinstallation erweitern. Azure Data Studio stellt Erweiterbarkeitspunkte für Datenverwaltungsaktivitäten sowie Unterstützung für die Erstellung von Erweiterungen bereit.

Weitere Informationen zur Erweiterbarkeit in Azure Data Studio finden Sie unter [Erweiterbarkeit](extensibility.md).
Informationen zum Erstellen von Erweiterungen finden Sie unter [Erstellen von Erweiterungen](extensions/extension-authoring.md).

## <a name="feature-comparison-with-sql-server-management-studio-ssms"></a>Featurevergleich mit SQL Server Management Studio (SSMS)

**Verwenden Sie Azure Data Studio in folgenden Fällen**:

- Sie verbringen einen Großteil Ihrer Zeit mit der Bearbeitung oder Ausführung von Abfragen.
- Sie benötigen eine Funktion, mit der Sie Resultsets schnell visualisieren und in Diagrammen darstellen können.
- Sie können die meisten administrativen Aufgaben mit sqlcmd oder PowerShell über das integrierte Terminal ausführen.
- Sie benötigen nur wenige Assistentenfunktionen.
- Sie müssen keine detaillierten administrativen oder plattformbezogenen Konfiguration durchführen.
- Sie verwenden macOS oder Linux.

**Verwenden Sie SQL Server Management Studio in folgenden Fällen**:

- Sie führen komplexe administrative Konfigurationen oder Plattformkonfigurationen durch.
- Die Sicherheitsverwaltung, einschließlich der Benutzerverwaltung, der Sicherheitsrisikobewertung und der Konfiguration von Sicherheitsfeatures, gehört zu Ihren Aufgaben.
- Sie benötigen Ratgeber und Dashboards für die Leistungsoptimierung.
- Sie verwenden Datenbankdiagramme und Tabellen-Designer.
- Sie benötigen Zugriff auf registrierte Server.
- Sie verwenden Liveabfragestatistiken oder Clientstatistiken.

### <a name="shell-features"></a>Shellfeatures

|Funktion|Azure Data Studio|SSMS|
|:---|:---|:---|
|Azure-Anmeldung|Ja|Ja|
|Dashboard|Ja| |
|Erweiterungen|Ja| |
|Integriertes Terminal|Ja||
|Objekt-Explorer|Ja|Ja|
|Skripterstellung für Objekte|Ja|Ja|
|Projektsystem|Ja||
|Aus Tabelle auswählen|Ja|Ja|
|Quellcodeverwaltung|Ja||
|Aufgabenbereich|Ja||
|Designs, einschließlich „Dunkler Modus“|Ja||
|Azure-Ressourcen-Explorer|Vorschau||
|Assistent zum Generieren von Skripts||Ja|
|Objekteigenschaften||Ja|
|Tabellen-Designer||Ja|

### <a name="query-editor"></a>Abfrage-Editor

|Funktion|Azure Data Studio|SSMS|
|:---|:---|:---|
|Diagrammviewer|Ja||
|Ergebnisse im CSV-, JSON- oder XLSX-Format exportieren|Ja||
|Ergebnisse in Datei||Ja|
|Ergebnisse in Text||Ja|
|IntelliSense|Ja|Ja|
|Codeausschnitte|Ja|Ja|
|Plan anzeigen|Vorschau|Ja|
|Clientstatistiken||Ja|
|Liveabfragestatistiken||Ja|
|Abfrageoptionen||Ja|
|Räumlicher Viewer||Ja|
|SQLCMD|Ja|Ja|

### <a name="operating-system-support"></a>Betriebssystemunterstützung

|Funktion|Azure Data Studio|SSMS|
|:---|:---|:---|
|Windows|Ja|Ja|
|macOS|Ja||
|Linux|Ja||

### <a name="data-engineering"></a>Datentechnik

|Funktion|Azure Data Studio|SSMS|
|:---|:---|:---|
|External Data Wizard (Assistent für externe Daten)|Vorschau||
|HDFS-Integration|Vorschau||
|Notebooks|Vorschau||

### <a name="database-administration"></a>Datenbankverwaltung

|Funktion|Azure Data Studio|SSMS|
|:---|:---|:---|
|Sicherung/Wiederherstellung|Ja|Ja|
|Flatfile-Import|Ja|Ja|
|SQL-Agent|Vorschau|Ja|
|SQL Profiler|Vorschau|Ja|
|Always On||Ja|
|Always Encrypted||Ja|
|Assistent zum Kopieren von Daten||Ja|
|Ratgeber für die Datenoptimierung||Ja|
|Datenbankdiagramme||Ja|
|Fehlerprotokollanzeige||Ja|
|Wartungspläne||Ja|
|Abfrage mehrerer Server||Ja|
|Richtlinienbasierte Verwaltung||Ja|
|PolyBase||Ja|
|Abfragespeicher||Ja|
|Registrierte Server||Ja|
|Replikation||Ja|
|Sicherheitsverwaltung||Ja|
|Service Broker||Ja|
|SQL Assessment|Vorschau|Ja|
|SQL Mail||Ja|
|Template Explorer||Ja|
|Sicherheitsrisikobewertung||Ja|
|XEvent-Verwaltung||Ja|

### <a name="database-development"></a>Datenbankentwicklung
|Funktion|Azure Data Studio|SSMS|
|:---|:---|:---|
|DACPACs importieren/exportieren|Ja|Ja|
|SQL-Projekte|Vorschau||
|Schemavergleich|Ja||

## <a name="next-steps"></a>Nächste Schritte

- [Herunterladen und Installieren von Azure Data Studio](./download-azure-data-studio.md)
- [FAQ zu Azure Data Studio](faq.yml)
- [Herstellen einer Verbindung mit und Abfragen von SQL Server](quickstart-sql-server.md)
- [Herstellen einer Verbindung mit und Abfragen von Azure SQL-Datenbank](quickstart-sql-database.md)

[!INCLUDE[get-help-sql-tools](../includes/paragraph-content/get-help-sql-tools.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
