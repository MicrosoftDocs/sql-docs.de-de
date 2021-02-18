---
description: Erstellen von Datenbankprojekten mit SQL Server Management Studio
title: Erstellen von Datenbankprojekten
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- scripts [SQL Server], database projects
- database projects [SQL Server]
- projects [SQL Server Management Studio], about projects
- projects [SQL Server Management Studio]
ms.assetid: c2e80045-894d-44cf-b65c-e547ed738947
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.openlocfilehash: a2a22864cfb800a257e67ea2600336cbeba42aa1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341108"
---
# <a name="build-database-projects-by-using-sql-server-management-studio"></a>Erstellen von Datenbankprojekten mit SQL Server Management Studio

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Bei einem Datenbankskriptprojekt handelt es sich um einen organisierten Satz von Skripts, Verbindungsinformationen und Vorlagen, die alle einer Datenbank oder einem Teil einer Datenbank zugeordnet sind. [!INCLUDE[msCoName](../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] stellt [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] zum Verwalten und Entwerfen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Datenbanken im Kontext eines Skriptprojekts bereit. [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] umfasst Designer, Editoren, Handbücher und Assistenten, die Benutzern bei der Entwicklung, Bereitstellung und Verwaltung von Datenbanken behilflich sind.  
  
## <a name="sql-server-management-studio"></a>SQL Server Management Studio  
[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] ist eine Sammlung von Verwaltungstools zum Verwalten der Komponenten, die zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]gehören. Diese integrierte Umgebung ermöglicht es Benutzern, eine Reihe von Aufgaben, wie z. B. das Sichern von Daten, das Bearbeiten von Abfragen und das Automatisieren häufiger Funktionen, in einer einzigen Benutzeroberfläche auszuführen.  
  
[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] enthält folgende Tools:  
  
-   Der Code-Editor ist ein Skript-Editor mit vielen Funktionen zum Erstellen und Bearbeiten von Skripts. [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] stellt vier Versionen des Code-Editors bereit: den [!INCLUDE[ssDE](../includes/ssde_md.md)] Abfrage-Editor für [!INCLUDE[tsql](../includes/tsql-md.md)] -Skripts, den DMX-Abfrage-Editor, den MDX-Abfrage-Editor und den XML-/A-Abfrage-Editor.  
  
-   Objekt-Explorer, um Objekte, die zu Instanzen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]gehören, zu suchen, zu ändern, auszuführen oder um ein Skript für diese Objekte zu erstellen.  
  
-   Vorlagen-Explorer, um Vorlagen zu suchen und ein Skript dafür zu erstellen  
  
-   Projektmappen-Explorer, um zusammengehörige Skripts als Teile eines Projekts anzuordnen und zu speichern  
  
-   Eigenschaftenfenster, um die aktuellen Eigenschaften von ausgewählten Objekten anzuzeigen  
  
[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] unterstützt rationelle Arbeitsprozesse, indem Folgendes bereitgestellt wird:  
  
-   Zugriff bei getrennter Verbindung. Sie können Skripts erstellen und bearbeiten, ohne mit einer Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]verbunden zu sein.  
  
-   Skripterstellung in jedem Dialogfeld. Sie können ein Skript in jedem Dialogfeld erstellen, damit Sie die Skripts nach dem Erstellen lesen, ändern, speichern und wieder verwenden können.  
  
-   Nicht-modale Dialogfelder. Wenn Sie auf ein Benutzeroberflächen-Dialogfeld zugreifen, können Sie andere Ressourcen in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] durchsuchen, ohne das Dialogfeld zu schließen.  
  
## <a name="solutions-and-script-projects"></a>Projektmappen und Skriptprojekte  
Mit dem Projektmappen-Explorer können Sie Datenbankprojektmappen speichern und erneut öffnen. Mit Projektmappen werden zusammengehörige Skriptprojekte und Dateien angeordnet. In Skriptprojekten werden [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Skriptdateien, SQL-Vorlagen, Verbindungsinformationen und sonstige Dateien gespeichert. Wenn ein Skript in einem Skriptprojekt gespeichert wird, können Benutzer folgende Aktionen ausführen:  
  
-   Aufrechterhalten der Versionskontrolle für Skripts  
  
-   Speichern von Ergebnisoptionen mit einem Skript  
  
-   Organisieren von zusammengehörigen Skripts zu einem einzigen Skriptprojekt  
  
-   Speichern von Verbindungsinformationen mit Skripts  
  
Der Projektmappen-Explorer ist ein Tool für Entwickler, die Skripts, die zum gleichen Projekt gehören, erstellen und wieder verwenden. Wenn ein ähnlicher Task später erforderlich ist, können Sie Skriptgruppen verwenden, die in einem Projekt gespeichert wurden. Wenn Sie bereits Anwendungen mithilfe von [!INCLUDE[msCoName](../includes/msconame_md.md)] [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] erstellt haben, wird Ihnen der Projektmappen-Explorer vertraut sein.  
  
Eine Projektmappe besteht aus mindestens einem Skriptprojekt. Ein Projekt besteht aus mindestens einem Skript oder mindestens einer Verbindung. In einem Projekt können außerdem andere Dateien als nur Skriptdateien vorhanden sein.  
  
## <a name="see-also"></a>Weitere Informationen  
[Verwenden von SQL Server Management Studio](./sql-server-management-studio-ssms.md)  
[Projektmappen &#40;SQL Server Management Studio&#41;](../ssms/solution/solutions-sql-server-management-studio.md)  
