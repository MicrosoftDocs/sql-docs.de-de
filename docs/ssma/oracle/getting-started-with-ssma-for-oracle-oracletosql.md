---
title: Ersten Schritte mit SSMA für Oracle (oracleto SQL) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die SQL Server Migration Assistant (SSMA) für den Oracle-Installationsprozess, und machen Sie sich mit der SSMA-Benutzeroberfläche vertraut.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- SSMA for Oracle, Metadata Explorers
- SSMA for Oracle, Toolbars
ms.assetid: df79664c-972e-4bef-865a-ce609789fee7
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: ce9d4908113daddf51707b4b7a80c4085defc228
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076711"
---
# <a name="getting-started-with-ssma-for-oracle-oracletosql"></a>Erste Schritte mit SSMA für Oracle (OracleToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Mit Migration Assistant (SSMA) für Oracle können Sie schnell Oracle-Datenbankschemas in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Schemas konvertieren, die resultierenden Schemas in hochladen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und Daten von Oracle zu migrieren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Dieses Thema enthält eine Einführung in den Installationsvorgang und hilft Ihnen dabei, Sie mit der SSMA-Benutzeroberfläche vertraut zu machen.  
  
## <a name="installing-ssma"></a>Installieren von SSMA  
Zum Verwenden von SSMA müssen Sie zuerst das SSMA-Client Programm auf einem Computer installieren, der auf die Oracle-Quelldatenbank und die-Ziel Instanz zugreifen kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Sie müssen dann ein Erweiterungspaket und mindestens einen der Oracle-Anbieter (OLE DB oder [!INCLUDE[vstecado](../../includes/vstecado_md.md)] ) auf dem Computer installieren, auf dem ausgeführt wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Diese Komponenten unterstützen die Datenmigration und die Emulation von Oracle-Systemfunktionen. Installationsanweisungen finden Sie unter [Installieren von SSMA für Oracle &#40;oracleto SQL&#41;](../../ssma/oracle/installing-ssma-for-oracle-oracletosql.md).  
  
Klicken Sie zum Starten von SSMA auf **Start**, zeigen Sie auf **Alle Programme**, zeigen Sie auf **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant für Oracle**, und klicken Sie dann auf **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant für Oracle**.  
  
## <a name="ssma-for-oracle-user-interface"></a>SSMA für Oracle-Benutzeroberfläche  
Nach der Installation von SSMA können Sie SSMA zum Migrieren von Oracle-Datenbanken zu verwenden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Es hilft Ihnen, sich mit der SSMA-Benutzeroberfläche vertraut zu machen, bevor Sie beginnen. Das folgende Diagramm zeigt die Benutzeroberfläche für SSMA, einschließlich der Metadaten-Explorer, Metadaten, Symbolleisten, Ausgabebereich und Fehlerlisten Bereich:  
  
![SSMA für die Oracle-Benutzeroberfläche](../../ssma/oracle/media/ssma_oracle_ui.jpg "SSMA für die Oracle-Benutzeroberfläche")  
  
Um eine Migration zu starten, müssen Sie zunächst ein neues Projekt erstellen. Anschließend stellen Sie eine Verbindung mit einer Oracle-Datenbank her. Nach einer erfolgreichen Verbindung werden Oracle-Schemas im Oracle-metadatenexplorer angezeigt. Sie können dann im Oracle Metadata Explorer mit der rechten Maustaste auf Objekte klicken, um Aufgaben wie das Erstellen von Berichten, die Konvertierungen in bewerten, auszuführen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Sie können diese Aufgaben auch mithilfe der Symbolleisten und Menüs ausführen.  
  
Sie müssen auch eine Verbindung mit einer Instanz von herstellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Nach einer erfolgreichen Verbindung wird eine Hierarchie von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbanken im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatenexplorer angezeigt. Nachdem Sie Oracle-Schemas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Schemas konvertiert haben, wählen Sie diese konvertierten Schemas im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatenexplorer aus, und synchronisieren Sie dann die Schemas mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Nachdem Sie konvertierte Schemas mit synchronisiert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] haben, können Sie zum Oracle-metadatenexplorer zurückkehren und Daten aus Oracle-Schemas in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbanken migrieren.  
  
Weitere Informationen zu diesen Aufgaben und deren Durchführung finden Sie unter [Migrieren von Oracle-Datenbanken zu SQL Server &#40;oracleto SQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md).  
  
In den folgenden Abschnitten werden die Features der SSMA-Benutzeroberfläche beschrieben.  
  
### <a name="metadata-explorers"></a>Metadatenexplorer  
SSMA enthält zwei metadatenexplorer zum Durchsuchen und Ausführen von Aktionen für Oracle-und- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbanken.  
  
#### <a name="oracle-metadata-explorer"></a>Oracle-metadatenexplorer  
Oracle Metadata Explorer zeigt Informationen zu Oracle-Schemas an. Mit dem Oracle-metadatenexplorer können Sie die folgenden Aufgaben ausführen:  
  
-   Durchsuchen Sie die Objekte in den einzelnen Schemas.  
  
-   Wählen Sie die Objekte für die Konvertierung aus, und konvertieren Sie dann die Objekte in die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Syntax. Weitere Informationen finden Sie unter [umstellen von Oracle-Schemas &#40;oracleto SQL&#41;](../../ssma/oracle/converting-oracle-schemas-oracletosql.md).  
  
-   Wählen Sie Tabellen für die Datenmigration aus, und migrieren Sie dann die Daten aus diesen Tabellen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Weitere Informationen finden Sie unter [Migrieren von Oracle-Daten in SQL Server &#40;oracleto SQL&#41;](../../ssma/oracle/migrating-oracle-data-into-sql-server-oracletosql.md).  
  
#### <a name="sql-server-metadata-explorer"></a>Metadatenexplorer SQL Server  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Im metadatenexplorer werden Informationen zu einer Instanz von angezeigt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Wenn Sie eine Verbindung mit einer Instanz von herstellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ruft SSMA Metadaten zu dieser Instanz ab und speichert Sie in der Projektdatei.  
  
Sie können den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatenexplorer verwenden, um konvertierte Oracle-Datenbankobjekte auszuwählen und diese Objekte dann mit der Instanz von zu synchronisieren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Weitere Informationen finden Sie unter [Laden von konvertierten Datenbankobjekten in SQL Server &#40;oracleto SQL&#41;](../../ssma/oracle/loading-converted-database-objects-into-sql-server-oracletosql.md).  
  
### <a name="metadata"></a>Metadaten  
Auf der rechten Seite jedes metadatenexplorers sind Registerkarten, die das ausgewählte Objekt beschreiben. Wenn Sie z. b. eine Tabelle im Oracle-metadatenexplorer auswählen, werden sechs Registerkarten angezeigt: **Table**, **SQL**, **Type Mapping, Report**, **Properties** und **Data**. Die Registerkarte **Bericht** enthält nur Informationen, nachdem Sie einen Bericht erstellt haben, der das ausgewählte Objekt enthält. Wenn Sie eine Tabelle im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatenexplorer auswählen, werden drei Registerkarten angezeigt: **Table**, **SQL** und **Data**.  
  
Die meisten metadateneinstellungen sind schreibgeschützt. Sie können jedoch die folgenden Metadaten ändern:  
  
-   Im Oracle-metadatenexplorer können Sie Prozeduren und Typzuordnungen ändern. Nehmen Sie vor dem Konvertieren von Schemas Änderungen vor, um die geänderten Prozeduren und Typzuordnungen zu konvertieren.  
  
-   Im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatenexplorer können Sie die [!INCLUDE[tsql](../../includes/tsql-md.md)] für gespeicherte Prozeduren ändern. Um diese Änderungen in anzuzeigen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , nehmen Sie diese Änderungen vor, bevor Sie die Schemas in Laden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Änderungen, die in einem metadatenexplorer vorgenommen werden, werden in den Projekt Metadaten, nicht in der Quell-oder Zieldatenbank widergespiegelt.  
  
### <a name="toolbars"></a>Symbolleisten  
SSMA verfügt über zwei Symbolleisten: eine Projektsymbol Leiste und eine Migrations Symbolleiste.  
  
#### <a name="the-project-toolbar"></a>Die Projektsymbol Leiste  
Die Projektsymbol Leiste enthält Schaltflächen zum Arbeiten mit Projekten, zum Herstellen einer Verbindung mit Oracle und zum Herstellen einer Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Diese Schaltflächen ähneln den Befehlen im Menü **Datei** .  
  
#### <a name="migration-toolbar"></a>Migrations Symbolleiste  
In der folgenden Tabelle werden die Befehle für die Migrations Symbolleiste angezeigt:  
  
|Taste|Funktion|  
|------|--------|  
|**Erstellen von Berichten**|Konvertiert die ausgewählten Oracle-Objekte in die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Syntax und erstellt dann einen Bericht, der die erfolgreiche Konvertierung der Konvertierung anzeigt.<br /><br />Dieser Befehl ist deaktiviert, es sei denn, Objekte werden im Oracle-metadatenexplorer ausgewählt.|  
|**Schema konvertieren**|Konvertiert die ausgewählten Oracle-Objekte in- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Objekte.<br /><br />Dieser Befehl ist deaktiviert, es sei denn, Objekte werden im Oracle-metadatenexplorer ausgewählt.|  
|**Migrieren von Daten**|Migriert Daten aus der Oracle-Datenbank zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vor dem Ausführen dieses Befehls müssen Sie die Oracle-Schemas in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Schemas konvertieren und dann die Objekte in Laden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br />Dieser Befehl ist deaktiviert, es sei denn, Objekte werden im Oracle-metadatenexplorer ausgewählt.|  
|**Beenden**|Beendet den aktuellen Prozess.|  
  
### <a name="menus"></a>Menüs  
In der folgenden Tabelle sind die SSMA-Menüs aufgeführt.  
  
|Menü|BESCHREIBUNG|  
|----|-----------|  
|**File**|Enthält Befehle für das Arbeiten mit-Projekten, das Herstellen einer Verbindung mit Oracle und das Herstellen einer Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Bearbeiten**|Enthält Befehle zum Suchen und arbeiten mit Text auf den Detailseiten, z. b [!INCLUDE[tsql](../../includes/tsql-md.md)] . zum Kopieren aus dem SQL-Detailbereich. Enthält auch die Option **Lesezeichen verwalten** , in der eine Liste vorhandener Lesezeichen angezeigt werden kann. Sie können die Schaltflächen auf der rechten Seite des Dialog Felds verwenden, um die Lesezeichen zu verwalten.|  
|**Anzeigen**|Enthält den Befehl Metadaten-Explorer **Synchronisieren** . , Die die Objekte zwischen dem Oracle-metadatenexplorer und dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatenexplorer synchronisiert. Enthält auch Befehle zum Anzeigen und Ausblenden der **Ausgabe** -und **Fehlerliste** Bereiche sowie ein Options **Layout** zum Verwalten der Layouts.|  
|**Extras**|Enthält Befehle zum Erstellen von Berichten und zum Migrieren von Objekten und Daten. Bietet auch Zugriff auf die Dialogfelder **globale Einstellungen** und **Projekteinstellungen** .|  
|**Tester**|Enthält Befehle zum Erstellen und arbeiten mit Testfällen, Repository und Sicherungs Verwaltungssystem.|  
|**Hilfe**|Bietet Zugriff **auf die** SSMA-Hilfe und das Dialogfeld Info.|  
  
### <a name="output-pane-and-error-list-pane"></a>Ausgabebereich und Fehlerliste Bereich  
Das Menü **Ansicht** enthält Befehle zum Umschalten der Sichtbarkeit des Ausgabe Bereichs und des Fehlerliste Bereichs:  
  
-   Im Ausgabebereich werden Statusmeldungen von SSMA während der Objekt Konvertierung, der Objekt Synchronisierung und der Datenmigration angezeigt.  
  
-   Im Fehlerliste Bereich werden Fehler-, Warn-und Informationsmeldungen in einer sortierbaren Liste angezeigt.  
  
## <a name="see-also"></a>Weitere Informationen  
[Migrieren von Oracle-Daten in SQL Server &#40;oracleto SQL-&#41;](../../ssma/oracle/migrating-oracle-data-into-sql-server-oracletosql.md)  
[Referenz zur Benutzeroberfläche &#40;oracletosql&#41;](../../ssma/oracle/user-interface-reference-oracletosql.md)  
  
