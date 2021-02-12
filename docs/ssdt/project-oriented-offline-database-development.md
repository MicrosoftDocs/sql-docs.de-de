---
title: Projektorientierte Offlinedatenbankentwicklung
description: In diesem Artikel erfahren Sie, wie Sie verfügbare Ressourcen in projektorientierten Entwicklungsaufgaben für eine Offlinedatenbank anzeigen, z. B. das Importieren von Objekten in eine Datenbank und das Verwenden von Sequenzobjekten.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.dbprojectwizard.general
- sql.data.tools.dbprojectwizard.summary
ms.assetid: e61e830d-9fcd-45e7-b7b4-93a42155dd56
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: e36876d1b30b12dbae1dd4c7d3c72230070c9ef8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077055"
---
# <a name="project-oriented-offline-database-development"></a>Projektorientierte Offlinedatenbankentwicklung

In diesem Abschnitt werden die Funktionen von SQL Server Data Tools (SSDT) zum Erstellen, Debuggen und Veröffentlichen von Datenbankprojekten beschrieben.  
  
Mit SSDT können Sie ein Offlinedatenbankprojekt erstellen und Schemaänderungen durch Hinzufügen, Ändern oder Löschen der Definitionen von Objekten (die durch Skripts dargestellt werden) im Projekt implementieren, ohne dass eine Verbindung mit einer Serverinstanz erforderlich ist. All dies ist mithilfe des Tabellen-Designers oder des Transact\-SQL-Editors möglich. Sie können auch Transact\-SQL- und CLR-Objekte im selben Projekt schreiben und debuggen. Mit einem Schemavergleich können Sie sicherstellen, dass das Projekt mit der Produktionsdatenbank synchronisiert ist, und Sie können in jeder Phase des Entwicklungszyklus Momentaufnahmen zu Vergleichszwecken erstellen. Während Sie in einer teambasierten Umgebung an den Datenbankprojekten arbeiten, können Sie Versionskontrolle für alle Dateien nutzen. Nach dem Entwickeln, Testen und Debuggen des Datenbankprojekts können Sie es an entsprechend berechtigte Mitarbeiter weitergeben, die das Projekt in einer Produktionsumgebung veröffentlichen.  
  
> [!NOTE]  
> Die Vorgehensweisen in diesem Abschnitt enthalten eine Reihe von Aufgaben, die aufeinander aufbauend ausgeführt werden können.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
|Thema|BESCHREIBUNG|  
|---------|---------------|  
|[Importieren in ein Datenbankprojekt](../ssdt/import-into-a-database-project.md)|Beschreibt das Importieren von Objekten aus einer Livedatenbank, DACPAC-Datei oder einem Skript.|  
|[Dialogfeld „Datenbankverweis hinzufügen“](../ssdt/add-database-reference-dialog-box.md)|Beschreibt verschiedene Verfahren zum Hinzufügen eines Datenbankverweises.|  
|[Dialogfeld „Nach Updates suchen“](../ssdt/check-for-updates-dialog-box.md)|Beschreibt, wie SQL Server Data Tools nach Produktupdates suchen kann.|  
|[Datenbankprojekteinstellungen](../ssdt/database-project-settings.md)|Beschreibt verschiedene Projekteinstellungen, die Aspekte der Datenbank- und Buildkonfiguration steuern.|  
|[Vorgehensweise: Durchsuchen von Objekten in einem SQL Server-Datenbankprojekt](../ssdt/how-to-browse-objects-in-a-sql-server-database-project.md)|Der SQL Server-Objekt-Explorer in Visual Studio enthält jetzt einen dedizierten Projektknoten, unter dem sämtliche in der Projektmappe enthaltenen SQL Server-Datenbankprojekte in einer Hierarchie gruppiert sind, die SQL Server Management Studio ähnelt.|  
|[Das Fenster „Datentoolvorgänge“](../ssdt/data-tools-operations-window.md)|Beschreibt das Fenster **Datentoolvorgänge** , in dem der Status einiger Vorgänge und ggf. Fehlermeldungen angezeigt werden.|  
|[Optionen des Transact-SQL-Editors](../ssdt/transact-sql-editor-options.md)|Beschreibt die Transact\-SQL-Optionen.|  
|[Vorgehensweise: Erstellen eines neuen Datenbankprojekts](../ssdt/how-to-create-a-new-database-project.md)|Erstellen Sie ein Datenbankprojekt, und importieren Sie ein vorhandenes Datenbankschema.|  
|[Vorgehensweise: Vergleichen verschiedener Datenbankdefinitionen mithilfe des Schemavergleichs](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)|Vergleichen Sie die Schemas einer Datenbank und eines Projekts, und synchronisieren Sie sie.|  
|[Vorgehensweise: Erstellen und Bereitstellen in einer lokalen Datenbank](../ssdt/how-to-build-and-deploy-to-a-local-database.md)|Verwenden Sie die lokale bedarfsgesteuerte SQL Server-Instanz, die beim Debuggen eines Datenbankprojekts aktiviert wird.|  
|[Vorgehensweise: Ändern der Zielplattform und Veröffentlichen eines Datenbankprojekts](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md)|Ändern Sie die SQL Server-Zielplattform für das Projekt in eine unterstützte SQL Server-Instanz, und überprüfen Sie die Syntax.|  
|[Vorgehensweise: Erstellen einer Momentaufnahme eines Projekts](../ssdt/how-to-create-a-snapshot-of-a-project.md)|Erstellen Sie einen schreibgeschützten Proxy des Datenbankschemas, und stellen Sie bei der Anwendung unerwünschter Änderungen auf das Projekt das Quellprojekt wieder her.|  
|[Vorgehensweise: Verwenden von Microsoft SQL Server 2012-Objekten im Projekt](../ssdt/how-to-use-microsoft-sql-server-2012-objects-in-your-project.md)|Fügen Sie dem Projekt ein neues Sequenzobjekt hinzu.|  
|[Vorgehensweise: Arbeiten mit CLR-Datenbankobjekten](../ssdt/how-to-work-with-clr-database-objects.md)|Erstellen und veröffentlichen Sie CLR-Objekte im SQL Server Data Tools-Datenbankprojekt.|  
|[Vorgehensweise: Konvertieren von Visual Studio 2010-Datenbankprojekten in SQL Server-Datenbankprojekte und Neuzuweisen zu einer anderen Plattform](../ssdt/how-to-convert-visual-studio-2010-database-projects-to-ssql-server-projects.md)|Konvertieren Sie vorhandene, in Visual Studio 2010 erstellte SQL Server-Datenbank-, CLR-Objekt- und Datenschichtanwendungs-Projekte in das SQL Server Data Tools-Datenbankprojekt.|  
|[Vorgehensweise: Festlegen von Skripts vor und nach der Bereitstellung](../ssdt/how-to-specify-predeployment-or-postdeployment-scripts.md)|Erläutert die Verwendung von Skripts, die Sie vor oder nach der Bereitstellung einer Datenbank ausführen möchten.|  
  
## <a name="related-sections"></a>Verwandte Abschnitte

[Verwalten von Tabellen, Beziehungen und Beheben von Fehlern](../ssdt/manage-tables-relationships-and-fix-errors.md)
