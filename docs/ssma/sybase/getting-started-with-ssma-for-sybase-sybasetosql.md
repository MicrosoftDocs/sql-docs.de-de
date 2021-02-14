---
title: Getting Started with SSMA for SAP ASE (sybasedesql) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die SQL Server Migration Assistant (SSMA) für die SAP ASE-Installation, und machen Sie sich mit der SSMA-Benutzeroberfläche vertraut.
ms.custom: ''
ms.date: 09/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: c4098516-f0fc-4690-97bb-3766dfd43156
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2ed1698af612a3bd960b7c16011dc4f95e33f61a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078861"
---
# <a name="getting-started-with-ssma-for-sap-ase-sybasetosql"></a>Ersten Schritte mit SSMA für SAP ASE (sybasedesql)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Mit Migration Assistant (SSMA) für SAP ASE können Sie Datenbankschemas für SAP Adaptive Server Enterprise (ASE) schnell in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbankschemas konvertieren, die resultierenden Schemas in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank hochladen und Daten aus SAP ASE zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank migrieren.  
  
Dieses Thema enthält eine Einführung in den Installationsvorgang und hilft Ihnen dabei, Sie mit der SSMA-Benutzeroberfläche vertraut zu machen.  
  
## <a name="installing-and-licensing-ssma"></a>Installieren und lizenzieren von SSMA  
Zum Verwenden von SSMA müssen Sie zuerst das SSMA-Client Programm auf einem Computer installieren, der sowohl auf die Quell Instanz von SAP ASE als auch auf die Ziel Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank zugreifen kann. Um die serverseitige Datenmigration verwenden zu können, müssen Sie das Erweiterungspaket und mindestens einen der SAP ASE-Anbieter (OLE DB oder ADO.net) auf dem Computer installieren, auf dem ausgeführt wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Diese Komponenten unterstützen die Datenmigration und die Emulation der SAP ASE-Systemfunktionen. Installationsanweisungen finden Sie unter [Installieren von SSMA für SAP ASE &#40;sybasedesql&#41;](../../ssma/sybase/installing-ssma-for-sybase-sybasetosql.md).  
  
Klicken Sie zum Starten von SSMA auf **Start**, zeigen Sie auf **Alle Programme**, zeigen Sie auf **[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant für Sybase**, und wählen Sie dann **[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant für Sybase aus**. Wenn Sie SSMA zum ersten Mal starten, wird ein Lizenzierungs Dialogfeld angezeigt. Sie müssen SSMA mithilfe einer Windows Live ID lizenzieren, bevor Sie SSMA verwenden können. Lizenzierungs Anweisungen finden Sie in den Installationsanweisungen im Thema [Installieren von SSMA für den Sybase-Client &#40;sybasedesql&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md) .  
  
## <a name="ssma-for-sap-ase-user-interface"></a>SSMA für SAP ASE-Benutzeroberfläche  
Nachdem SSMA installiert und lizenziert wurde, können Sie SSMA zum Migrieren von SAP ASE-Datenbanken zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank verwenden. Es hilft Ihnen, sich mit der SSMA-Benutzeroberfläche vertraut zu machen, bevor Sie beginnen. Das folgende Diagramm zeigt die Benutzeroberfläche für SSMA, einschließlich der Metadaten-Explorer, Metadaten, Symbolleisten, Ausgabebereich und Fehlerlisten Bereich:  
  
![SSMA für SAP ASE-Benutzeroberfläche](../../ssma/sybase/media/ssmaforsybaseuserinterface.jpg "SSMA für SAP ASE-Benutzeroberfläche")  
  
Um eine Migration zu starten, müssen Sie zunächst ein neues Projekt erstellen. Anschließend stellen Sie eine Verbindung mit SAP ASE her. Nach einer erfolgreichen Verbindung wird eine Hierarchie von SAP ASE-Datenbanken im Sybase-metadatenexplorer angezeigt. Sie können dann im Sybase-metadatenexplorer mit der rechten Maustaste auf Objekte klicken, um z. b. Berichte zu erstellen, die Konvertierungen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank bewerten. Sie können diese Aufgaben auch über die Symbolleisten und Menüs ausführen.  
  
Sie müssen auch eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank herstellen. Nach einer erfolgreichen Verbindung wird eine Hierarchie von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbanken in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure Metadaten-Explorer angezeigt. Nachdem Sie SAP ASE-Schemas in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbankschemas konvertiert haben, wählen Sie diese konvertierten Schemas in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure Metadaten-Explorer aus, und laden Sie dann die Schemas in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank.  
  
Nachdem Sie konvertierte Schemas in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank geladen haben, können Sie zum Sybase-metadatenexplorer zurückkehren und Daten aus SAP ASE-Datenbanken in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbanken migrieren.  
  
Weitere Informationen zu diesen Aufgaben und deren Durchführung finden Sie unter [Migrieren von SAP ASE-Datenbanken zu SQL Server-Azure SQL-Datenbank &#40;sybasedesql-&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md).  
  
In den folgenden Abschnitten werden die Features der SSMA-Benutzeroberfläche beschrieben.  
  
### <a name="metadata-explorers"></a>Metadatenexplorer  
SSMA enthält zwei metadatenexplorer zum Durchsuchen und Ausführen von Aktionen in SAP ASE und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbanken.  
  
#### <a name="sybase-metadata-explorer"></a>Sybase-metadatenexplorer  
Der Sybase-metadatenexplorer zeigt Informationen zu Datenbanken in der Quell Instanz von SAP ASE an.  
  
Mithilfe des Sybase-metadatenexplorers können Sie die folgenden Aufgaben ausführen:  
  
-   Durchsuchen Sie die Tabellen in den einzelnen Datenbanken.  
  
-   Wählen Sie die Objekte für die Konvertierung aus, und konvertieren Sie dann die Objekte in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Daten Bank Syntax. Weitere Informationen finden Sie unter [umstellen von SAP ASE-Datenbankobjekten &#40;sybasedesql&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md).  
  
-   Wählen Sie Objekte für die Datenmigration aus, und migrieren Sie dann die Daten von diesen Objekten zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank. Weitere Informationen finden Sie unter [Migrieren von SAP ASE-Daten in SQL Server Azure SQL-Datenbank &#40;sybasedesql&#41;](../../ssma/sybase/migrating-sybase-ase-data-into-sql-server-azure-sql-db-sybasetosql.md).  
  
#### <a name="sql-server-or-sql-azure-metadata-explorer"></a>SQL Server oder SQL Azure Metadaten-Explorer  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure metadatenexplorer zeigt Informationen über eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank an. Wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder mit Azure SQL-Datenbank herstellen, ruft SSMA Metadaten zu dieser Instanz ab und speichert Sie in der Projektdatei.  
  
Sie können mithilfe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von oder SQL Azure-metadatenexplorer konvertierte SAP ASE-Datenbankobjekte auswählen und dann diese Objekte in die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank laden (Synchronisieren).  
  
Weitere Informationen finden Sie unter [Laden von konvertierten Datenbankobjekten in SQL Server &#40;sybasedesql&#41;](../../ssma/sybase/loading-converted-database-objects-into-sql-server-sybasetosql.md).  
  
### <a name="metadata"></a>Metadaten  
Auf der rechten Seite jedes metadatenexplorers sind Registerkarten, die das ausgewählte Objekt beschreiben. Wenn Sie z. b. eine Tabelle im Sybase-metadatenexplorer auswählen, werden sechs Registerkarten angezeigt: **Table**, **SQL**, **Type Mapping**, **Data**, **Properties** und **Report**. Die Registerkarte **Bericht** enthält nur Informationen, nachdem Sie einen Bericht mit dem ausgewählten Objekt erstellt haben. Wenn Sie eine Tabelle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure Metadaten-Explorer auswählen, werden drei Registerkarten angezeigt: **Table**, **SQL** und **Data**.  
  
Die meisten metadateneinstellungen sind schreibgeschützt. Sie können jedoch die folgenden Metadaten ändern:  
  
-   Im Sybase-metadatenexplorer können Sie Prozeduren und Typzuordnungen ändern. Nehmen Sie diese Änderungen vor, bevor Sie Schemas konvertieren.  
  
-   In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure Metadaten-Explorer können Sie die [!INCLUDE[tsql](../../includes/tsql-md.md)] für gespeicherte Prozeduren ändern. Nehmen Sie diese Änderungen vor, bevor Sie die Schemas in Laden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Änderungen, die in einem metadatenexplorer vorgenommen werden, werden in den Projekt Metadaten, nicht in der Quell-oder Zieldatenbank widergespiegelt.  
  
### <a name="toolbars"></a>Symbolleisten  
SSMA verfügt über zwei Symbolleisten: eine Projektsymbol Leiste und eine Migrations Symbolleiste.  
  
#### <a name="the-project-toolbar"></a>Die Projektsymbol Leiste  
Die Projektsymbol Leiste enthält Schaltflächen für das Arbeiten mit Projekten, das Herstellen einer Verbindung mit SAP ASE und das Herstellen einer Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank. Diese Schaltflächen ähneln den Befehlen im Menü **Datei** .  
  
#### <a name="the-migration-toolbar"></a>Die Migrations Symbolleiste  
Die Migrations Symbolleiste enthält die folgenden Befehle:  
  
|Taste|Funktion|  
|----------|------------|  
|**Erstellen von Berichten**|Konvertiert die ausgewählten SAP ASE-Objekte in die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Syntax und erstellt dann einen Bericht, der anzeigt, wie erfolgreich die Konvertierung war.<br /><br />Dieser Befehl ist nur verfügbar, wenn Objekte im Sybase-metadatenexplorer ausgewählt werden.|  
|**Schema konvertieren**|Konvertiert die ausgewählten SAP ASE-Objekte in- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbankobjekte.<br /><br />Dieser Befehl ist nur verfügbar, wenn Objekte im Sybase-metadatenexplorer ausgewählt werden.|  
|**Migrieren von Daten**|Migriert Daten von der SAP ASE-Datenbank zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank. Vor dem Ausführen dieses Befehls müssen Sie die SAP ASE-Schemas in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbankschemas konvertieren und die Objekte anschließend in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank laden.<br /><br />Dieser Befehl ist nur verfügbar, wenn Objekte im Sybase-metadatenexplorer ausgewählt werden.|  
|**Beenden**|Beendet den aktuellen Prozess, z. b. das Umstellen von Objekten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder die Azure SQL-Daten Bank Syntax.|  
  
### <a name="menus"></a>Menüs  
SSMA enthält die folgenden Menüs:  
  
|Menü|BESCHREIBUNG|  
|--------|---------------|  
|**File**|Enthält Befehle für das Arbeiten mit Projekten, das Herstellen einer Verbindung mit SAP ASE und das Herstellen einer Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank.|  
|**Bearbeiten**|Enthält Befehle zum Suchen und arbeiten mit Text auf den Detailseiten, z. b [!INCLUDE[tsql](../../includes/tsql-md.md)] . zum Kopieren aus dem SQL-Detailbereich. Enthält auch die Option **Lesezeichen verwalten** , in der eine Liste vorhandener Lesezeichen angezeigt wird. Sie können die Schaltflächen auf der rechten Seite des Dialog Felds verwenden, um die Lesezeichen zu verwalten.|  
|**Anzeigen**|Enthält den Befehl Metadaten-Explorer **Synchronisieren** . Dadurch werden die Objekte zwischen Sybase-metadatenexplorer und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure metadatenexplorer synchronisiert. Enthält auch Befehle zum Anzeigen und Ausblenden der **Ausgabe** -und **Fehlerliste** Bereiche sowie ein Options **Layout** zum Verwalten der Layouts.|  
|**Extras**|Enthält Befehle zum Erstellen von Berichten, Exportieren von Daten und Migrieren von Objekten und Daten. Bietet auch Zugriff auf die Dialogfelder **globale Einstellungen** und **Projekteinstellungen** .|  
|**Tester**|Enthält Befehle zum Erstellen von Testfällen, Anzeigen von Testergebnissen und Befehlen für die Verwaltung der Datenbanksicherung.|  
|**Hilfe**|Bietet Zugriff **auf die** SSMA-Hilfe und das Dialogfeld Info.|  
  
### <a name="output-pane-and-error-list-pane"></a>Ausgabebereich und Fehlerliste Bereich  
Das Menü **Ansicht** enthält Befehle zum Umschalten der Sichtbarkeit des Ausgabe Bereichs und des Fehlerliste Bereichs:  
  
-   Im Ausgabebereich werden Statusmeldungen von SSMA während der Objekt Konvertierung, der Objekt Synchronisierung und der Datenmigration angezeigt.  
  
-   Im Fehlerliste Bereich werden Fehler-, Warn-und Informationsmeldungen in einer Liste angezeigt, die Sie sortieren können.  
  
## <a name="see-also"></a>Weitere Informationen  
[Migrieren von SAP ASE-Datenbanken zu SQL Server Azure SQL-Datenbank &#40;sybaseto SQL-&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
[Referenz zur Benutzeroberfläche &#40;sybasetosql&#41;](../../ssma/sybase/user-interface-reference-sybasetosql.md)  
