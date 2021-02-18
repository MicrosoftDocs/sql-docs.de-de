---
title: Was ist neu in SQLServer 2016
description: Erfahren Sie mehr über die Sicherheitsfeatures von SQL Server 2016, Abfragefunktionen, Hadoop- und Cloudintegration, R-Analyse und mehr.
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
keywords:
- Neu SQL Server
helpviewer_keywords:
- new features [SQL Server]
- SQL Server, what's new
- SQL Server 2008 what's new
- what's new [SQL Server]
ms.assetid: 6a428023-e3cc-4626-a88a-4c13ccbd7db0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c9313877cac9c4e22fdaa4234fc40f0701649934
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271737"
---
# <a name="whats-new-in-sql-server-2016"></a>Was ist neu in SQL Server 2016
[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]    
 Mit SQL Server 2016 können Sie intelligente, unternehmenskritische Anwendungen mithilfe einer skalierbaren, hybriden Datenbankplattform erstellen, in der bereits alles Features integriert sind – von arbeitsspeicherinterner Leistung über erweiterte Sicherheit bis hin zu datenbankinternen Analysen. Das SQL Server 2016-Release bietet neue Sicherheitsfunktionen, Abfragemöglichkeiten, Hadoop- und Cloudintegration, R-Analyse und viele weitere Verbesserungen und Erweiterungen. 

Auf dieser Seite finden Sie eine zusammenfassende Übersicht sowie Links zu detaillierten Informationen zu den Neuerungen, die SQL Server 2016 für jede SQL Server-Komponente bietet. 

![SQL Server 2016](../sql-server/media/sql-server-2016.png)

 **Probieren Sie SQL Server noch heute aus!** 
- Laden Sie die **kostenlose** [**neueste SQL Server-Edition**](https://www.microsoft.com/sql-server/sql-server-downloads) herunter.
- Laden Sie die neueste Version von [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) herunter. 
- Sie haben ein Azure-Konto? Starten Sie einen [virtuellen Computer mit vorinstalliertem SQL Server 2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview).

## <a name="sql-server-2016-database-engine"></a>SQL Server 2016-Datenbank-Engine
- Sie können jetzt während der Installation und Einrichtung von SQL Server **mehrere tempDB**-Datenbankdateien konfigurieren.
- Der neue **Abfragespeicher** speichert Abfragetexte, Ausführungspläne und Leistungsmetriken in der Datenbank und ermöglicht so eine einfache Leistungsüberwachung und Behebung von Leistungsproblemen. Ein Dashboard zeigt an, welche Abfragen am längsten dauern und am meisten Arbeitsspeicher- oder CPU-Ressourcen verbrauchen.
- **Temporale Tabellen** sind Verlaufstabellen, in denen alle Datenänderungen erfasst werden, einschließlich Datum und Uhrzeit.
- Die neue integrierte **JSON-Unterstützung** in SQL Server unterstützt Import-, Export-, Analyse- und Speichervorgänge in JSON.
- Die neue **PolyBase**-Abfrage-Engine integriert SQL Server mit externen Daten in Hadoop oder Azure Blob Storage. Sie können Daten importieren und exportieren sowie Abfragen ausführen.
- Mit dem neuen **Stretch Database**-Feature können Sie Daten aus einer lokalen SQL Server-Datenbank dynamisch und sicher in einer Azure SQL-Datenbank in der Cloud archivieren. SQL Server fragt automatisch sowohl die lokalen Daten als auch die Remotedaten in den verknüpften Datenbanken ab. 
- **In-Memory-OLTP:** 
    - Unterstützt jetzt die Einschränkungen FOREIGN KEY, UNIQUE und CHECK sowie die nativen kompilierten gespeicherte Prozeduren OR, NOT, SELECT DISTINCT, OUTER JOIN und Unterabfragen in SELECT.
    - Unterstützt Tabellen mit einer Größe von bis zu 2 TB (bisheriger Maximalwert: 256 GB). 
    - Bietet Erweiterungen des ColumnStore-Index für die Sortierung und die Unterstützung von Always On-Verfügbarkeitsgruppen.
- Neue Sicherheitsfeatures:
    - **Always Encrypted:** Wenn dieses Feature aktiviert ist, kann nur die Anwendung, die über den Verschlüsselungsschlüssel verfügt, auf die verschlüsselten sensiblen Daten in einer SQL Server 2016-Datenbank zugreifen. Der Schlüssel wird nie an SQL Server übergeben.
    - **Dynamische Datenmaskierung:** Wenn dieses Feature in der Tabellendefinition angegeben ist, werden maskierte Daten für die meisten Benutzer ausgeblendet, und nur Benutzer mit einer UNMASK-Berechtigung können alle Daten anzeigen.
    - **Sicherheit auf Zeilenebene:** Der Datenzugriff kann auf Ebene der Datenbank-Engine eingeschränkt werden, sodass Benutzer nur die Daten anzeigen können, die für sie relevant sind. 

## <a name="sql-server-2016-analysis-services-ssas"></a>SQL Server 2016 Analysis Services (SSAS)
SQL Server 2016 Analysis Services bietet eine verbesserte Leistung und Funktionalität für die Erstellung, Datenbankverwaltung, Filterung, Verarbeitung und viele weitere Vorgänge für Tabellenmodelldatenbanken mit **Kompatibilitätsgrad 1200**.
- **[SQL Server R Services](~/machine-learning/what-s-new-in-sql-server-machine-learning-services.md)** integrieren die für die statistische Analyse verwendete Programmiersprache R in SQL Server. 
- Der neue **Datenbankkonsistenzprüfung (Database Consistency Checker, DBCC)** wird intern ausgeführt, um potenzielle Probleme durch Datenbeschädigung zu erkennen.
- Die **direkte Abfrage**, die externe Daten live abfragt, anstatt sie vorher zu importieren, unterstützt jetzt weitere Datenquellen einschließlich Azure SQL, Oracle und Teradata. 
- Es gibt eine Vielzahl neuer **DAX-Funktionen (Data Access Expressions)** .
- Der neue Namespace **[Microsoft.AnalysisServices.Tabular](/dotnet/api/microsoft.analysisservices.tabular)** verwaltet Instanzen und Modelle im tabellarischen Modus. 
- [Analysis Services Management Objects (AMO)](/dotnet/api/) wurde überarbeitet und enthält jetzt eine zweite Assembly: **Microsoft.AnalysisServices.Core.dll**.

Weitere Informationen finden Sie unter [Analysis Services-Engine (SSAS)](/analysis-services/what-s-new-in-analysis-services). 

## <a name="sql-server-2016-integration-services-ssis"></a>SQL Server 2016 Integration Services (SSIS)
- Unterstützung für **Always On-Verfügbarkeitsgruppen**
- **Inkrementelle Paketbereitstellung**
- Unterstützung für **Always Encrypted**
- Neue **ssis_logreader**-Rolle auf Datenbankebene
- Neuer **benutzerdefinierter Protokolliergrad**
- **Spaltennamen für Fehler** im Datenfluss 
- Neue **Connectors**
- Unterstützung für das **Hadoop-Dateisystem (HDFS)**

Weitere Informationen finden Sie unter [Integration Services (SSIS)](../integration-services/what-s-new-in-integration-services-in-sql-server-2016.md).

## <a name="sql-server-2016-master-data-services-mds"></a>SQL Server 2016 Master Data Services (MDS)
- **Verbesserungen bei abgeleiteten Hierarchien**, einschließlich Unterstützung für rekursive und m:n-Beziehungen
- Filtern nach **domänenbasiertem Attribut**
- **Synchronisieren von Entitäten** zur gemeinsamen Nutzung von Entitäten in verschiedenen Modellen
- Genehmigungsworkflows über **Changesets**
- **Benutzerdefinierte Indizes** zur Verbesserung der Abfrageleistung
- Neue **Genehmigungslevel** für mehr Sicherheit
- Neu gestaltete **Geschäftsregelverwaltung**

Weitere Informationen finden Sie unter [Master Data Services (MDS)](../master-data-services/what-s-new-in-master-data-services-mds.md).

## <a name="sql-server-2016-reporting-services-ssrs"></a>SQL Server 2016 Reporting Services (SSRS)
Microsoft hat die Reporting Services in diesem Release sehr gründlich überarbeitet. 
- Neues **webbasiertes Berichtportal** mit KPI-Feature
- Neuer **Publisher für mobile Berichte**
- **Neu gestaltete Engine zum Rendern von Berichten**, die HTML5 unterstützt 
- Neue **Diagrammtypen**: Treemap und Sunburst 

Weitere Informationen finden Sie unter [Reporting Services (SSRS)](../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md).

## <a name="next-steps"></a>Nächste Schritte   
- [SQL Server-Setup](../database-engine/install-windows/install-sql-server.md)   
- [Versionsanmerkungen zu SQL Server 2016](../sql-server/sql-server-2016-release-notes.md) 
- [SQL Server 2016-Datenblatt](https://download.microsoft.com/download/C/5/3/C53C3AEF-653C-4598-8721-D522E8AC6A3A/SQL_Server_2016_Everything_Built-In_Datasheet_EN_US.pdf)
- [Von den SQL Server-Editionen unterstützte Funktionen](./editions-and-components-of-sql-server-2016.md)
- [Hardware- und Softwareanforderungen für die Installation von SQL Server 2016](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)
- [Installieren von SQL Server 2016 über den Installations-Assistenten](../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)
- [Setup und Wartungsinstallation](../database-engine/install-windows/install-sql-server-servicing-updates.md)
- [Neues SQL PowerShell-Modul](https://blogs.technet.microsoft.com/dataplatforminsider/2016/06/30/sql-powershell-july-2016-update/)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]