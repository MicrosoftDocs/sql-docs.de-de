---
description: Projekte (SQL Server Management Studio)
title: Projekte (SQL Server Management Studio)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c13af859-ca66-4e43-b76a-0650ac6566c0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 321257f45a593afdaf8c69228b2018347074d7ce
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88417876"
---
# <a name="projects-sql-server-management-studio"></a>Projekte (SQL Server Management Studio)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Ein [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] -Projekt ist eine Sammlung logisch verbundener Skripts und Dateien, die zusammen zur Verwaltung und Entwicklung von Datenbanken gespeichert werden können.  
  
## <a name="script-project-overview"></a>Übersicht über das Skriptprojekt  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Skriptprojekte werden in der Projektmappen-Explorer-Komponente von [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]angezeigt. Ein Skriptprojekt kann null oder mehr Projektdateien enthalten. Sie können einer Projektmappe ein Projekt hinzufügen oder mehrere Projekte innerhalb einer Projektmappe kombinieren.  
  
Projekte können Folgendes umfassen:  
  
-   **Verbindungen**. Eine dauerhafte Verbindung in einem Projekt enthält Anmeldeinformationen, den Servernamen, die Standarddatenbank, das bevorzugte Protokoll, den Authentifizierungstyp und Verbindungseigenschaften. Verbindungsinformationen lassen sich optional auch zusammen mit einem Skript speichern (siehe unten).  
  
-   **SQLScripts**. Häufig verwendete SQL-Skripts für den Benutzer. Wenn Sie im Projekt auf eine SQL-Datei doppelklicken, wird das ausgewählte Skript vom SQL-Editor geöffnet.  
  
-   **MDX-, DMX- und XMLA-Skripts**. Häufig verwendete MDX-Skripts für den Benutzer. Wenn Sie im Projekt auf eine MDX-Datei doppelklicken, wird das ausgewählte Skript vom Editor geöffnet.  
  
-   **Verschiedenes**. Dieser Ordner kann für Dateien verwendet werden, die sich nicht problemlos einem der anderen standardmäßigen Knotentypen zuordnen lassen. Dazu zählen z. B. Textdateien mit Projektzielen.  
  
Projekte können auch in ein Quellcodeverwaltungs-System integriert werden.  
  
## <a name="connecting-to-an-instance-of-sql-server-from-a-script-project"></a>Herstellen einer Verbindung mit einer SQL Server-Instanz aus einem Skriptprojekt  
Ein Skriptprojekt kann Verbindungen zu einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]enthalten. Sie können eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in einem Projekt durch Klicken auf die Verbindung herstellen. Daraufhin wird ein SQL-Skriptfenster geöffnet, das mit der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verbunden ist, die in der ausgewählten Verbindung definiert ist. Beim Öffnen eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] - oder MDX-Skripts mit einer Verbindung, von der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung verwendet wird, werden Sie zum Angeben des Kennworts aufgefordert. Dabei wird nach dem Öffnen des Editors und Laden des Skripts das Dialogfeld **Verbindung mit SQL Server herstellen** angezeigt.  
  
Die Verbindung wird geschlossen, sobald das entsprechende Fenster geschlossen wird.  
  
Mit dem Eigenschaftenfenster in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]können Sie Informationen zu einer Verbindung ändern.  
  
## <a name="project-tasks"></a>Projekttasks  
  
|Taskbeschreibung|Thema|  
|--------------------|---------|  
|Beschreibt, wie ein neues Projekt in einer Projektmappe erstellt wird.|[Erstellen eines Projekts](../../ssms/solution/create-a-project.md)|  
|Beschreibt, wie einer Projektmappe ein vorhandenes Projekt hinzugefügt wird.|[Hinzufügen eines vorhandenen Projekts zu einer Projektmappe](../../ssms/solution/add-an-existing-project-to-a-solution.md)|  
|Beschreibt, wie der Standardspeicherort zum Speichern von Projektdateien geändert wird.|[Ändern des Standardspeicherorts für Projekte](../../ssms/solution/change-the-default-location-for-projects.md)|  
|Beschreibt, wie die aktuellen Eigenschaften eines Projekts angezeigt werden.|[Anzeigen der Projekteigenschaften](../../ssms/solution/view-project-properties.md)|  
|Beschreibt, wie einem Projekt neue Elemente hinzugefügt werden, z. B. Verbindungen oder Skriptdateien.|[Hinzufügen neuer Elemente zu einem Projekt](../../ssms/solution/add-new-items-to-a-project.md)|  
|Beschreibt, wie die Verbindungsinformationen für eine Abfrage festgelegt werden.|[Verknüpfen einer Abfrage mit einer Verbindung in einem Projekt](../../ssms/solution/associate-a-query-with-a-connection-in-a-project.md)|  
|Beschreibt, wie die Verbindungsinformationen für eine Abfrage geändert werden.|[Ändern der mit einer Abfrage verknüpften Verbindung](../../ssms/solution/change-the-connection-associated-with-a-query.md)|  
|Beschreibt, wie Verbindungseigenschaften geändert werden.|[Anzeigen oder Ändern der Eigenschaften einer Verbindung in einem Projekt](../../ssms/solution/view-or-change-the-properties-of-a-connection-in-a-project.md)|  
  
## <a name="see-also"></a>Weitere Informationen  
[Projektmappen-Explorer](../../ssms/solution/solution-explorer.md)  
[Projektmappen (SQL Server Management Studio)](../../ssms/solution/solutions-sql-server-management-studio.md)  
[Quellcodeverwaltung des Projektmappen-Explorers](https://docs.microsoft.com/sql/ssms/solution/solution-explorer)  
  
