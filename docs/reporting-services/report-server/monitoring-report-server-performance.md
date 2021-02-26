---
title: Überwachen der Leistung des Berichtsservers | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die Leistung des Berichtsservers überwachen, um die Serveraktivität zu bewerten, Trends zu beobachten, Engpässe zu diagnostizieren und Daten zur Systemkonfiguration zu sammeln.
ms.date: 02/12/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- performance counters [Reporting Services]
- report servers [Reporting Services], performance
- counters [Reporting Services]
- monitoring performance [Reporting Services]
- SQL Server Reporting Services, performance
- performance [Reporting Services]
- Reporting Services, performance
ms.assetid: c1bc13d4-8297-4daf-bb19-4c1e5ba292a6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: deae632ce9305b58b72e24e1a57507f9f4c5ee89
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489434"
---
# <a name="monitoring-report-server-performance"></a>Überwachen der Leistung des Berichtsservers

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

  Überwachen Sie die Leistung des Berichtsservers mithilfe der Tools zur Leistungsüberwachung, um die Serveraktivität auszuwerten, Trends zu beobachten, Engpässe im System zu diagnostizieren oder Daten zu sammeln, mit denen Sie bestimmen können, ob die aktuelle Konfiguration ausreichend ist. Zum Optimieren der Serverleistung können Sie angeben, wie oft die Anwendungsdomäne des Berichtsservers wiederverwendet werden soll. Weitere Informationen finden Sie unter [Konfigurieren von verfügbarem Speicher für Berichtsserveranwendungen](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).  
  
## <a name="sources-of-performance-data"></a>Quellen von Leistungsdaten  
 Verwenden Sie eine Kombination aus Technologien und Tools, um sich umfassende Informationen zur Leistung des Systems zu beschaffen: [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows Server-Betriebssysteme stellen Leistungsinformationen mithilfe der folgenden Tools bereit:  
  
-   Task-Manager  
  
-   Ereignisanzeige  
  
-   Systemmonitor  
  
 Der Task-Manager stellt Informationen zu Programmen und Prozessen bereit, die auf einem Computer ausgeführt werden. Mit dem Task-Manager können Sie wichtige Indikatoren für die Leistung des Berichtsservers überwachen. Darüber hinaus können Sie die Aktivität von ausgeführten Prozessen bewerten und Grafiken sowie Daten zur CPU-Nutzung und Speicherauslastung anzeigen. Weitere Informationen zum Verwenden des Task-Managers finden Sie in der Produktdokumentation zu [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 Mit der Ereignisanzeige und dem Leistungsmonitor können Sie Protokolle und Warnungen zur Berichtsverarbeitung und zum Ressourcenverbrauch erstellen. Informationen zu Windows-Ereignissen, die von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]generiert werden, finden Sie unter [Windows-Anwendungsprotokoll](../../reporting-services/report-server/windows-application-log.md). Weitere Informationen zum Leistungsmonitor finden Sie unter „Windows-Leistungsindikatoren“ weiter unten in diesem Artikel.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Hilfsprogramme wie [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md) oder [Erweiterte Ereignisse](../../relational-databases/extended-events/extended-events.md) stellen außerdem Informationen zur Berichtsserver-Datenbank und zu temporären Datenbanken bereit, die zur Zwischenspeicherung und Sitzungsverwaltung verwendet werden.  
  
## <a name="windows-performance-counters"></a>Windows-Leistungsindikatoren  
 Die Überwachung bestimmter Leistungsindikatoren ermöglicht folgende Aktionen:  
  
-   Schätzen der zur Unterstützung einer prognostizierten Arbeitsauslastung erforderlichen Systemanforderungen.  
  
-   Erstellen einer Leistungsbasislinie, um die Auswirkungen von Konfigurationsänderungen oder Anwendungsupgrades zu messen.  
  
-   Überwachen der Anwendungsleistung unter bestimmten Arbeitsauslastungen, unabhängig davon, ob real oder künstlich generiert.  
  
-   Überprüfen, ob Hardwareupgrades sich wie gewünscht auf die Leistung auswirken.  
  
-   Überprüfen, ob an der Systemkonfiguration vorgenommene Änderungen sich wie gewünscht auf die Leistung auswirken.  

  
## <a name="reporting-services-performance-objects"></a>Reporting Services-Leistungsobjekte  
SQL Server 2016 Reporting Services umfasst die folgenden Leistungsobjekte:  
  
-   **MSRS 2016-Webdienst** und **MSRS 2016-Webdienst im SharePoint-Modus** zum Überwachen der Berichtsserverleistung. Diese Leistungsobjekte enthalten eine Reihe von Leistungsindikatoren zum Nachverfolgen der Verarbeitung auf einem Berichtsserver, die in der Regel über interaktive Vorgänge zum Anzeigen von Berichten gestartet wird. Diese Leistungsindikatoren werden zurückgesetzt, sobald der Berichtsserver-Webdienst beendet oder neu gestartet wird.  
  
-   **MSRS 2016-Windows-Dienst** und **MSRS 2016-Windows-Dienst im SharePoint-Modus** zum Überwachen der geplanten Vorgänge und der Berichtsübermittlung. Diese Leistungsobjekte enthalten eine Reihe von Leistungsindikatoren zum Nachverfolgen der Berichtsverarbeitung, die über geplante Vorgänge gestartet wird. Zu geplanten Vorgängen zählen Abonnement und Übermittlung, Berichtsausführungs-Momentaufnahmen und Berichtsverlauf.  
  
-   **ReportServer:Service** und **ReportServerSharePoint:Service** , HTTP-bezogene Ereignisse und die Verwaltung des Arbeitsspeichers zu überwachen. Diese Leistungsindikatoren sind spezifisch für [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], und sie verfolgen HTTP-bezogene Ereignisse für den Berichtsserver nach, wie z. B. Anforderungen, Verbindungen und Anmeldeversuche. Darüber hinaus schließt dieses Leistungsobjekt Leistungsindikatoren in Bezug auf die Speicherverwaltung ein.  
  
 Falls auf einem Computer mehrere Berichtsserverinstanzen vorhanden sind, können die Instanzen gemeinsam oder separat überwacht werden. Wählen Sie beim Hinzufügen eines Leistungsindikators die zu überwachenden Instanzen aus. Weitere Informationen zum Verwenden des Leistungsmonitors (perfmon.msc) und zum Hinzufügen von Leistungsindikatoren finden Sie in der Produktdokumentation zum [!INCLUDE[msCoName](../../includes/msconame-md.md)] [Windows-Leistungsmonitor](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc749249(v=ws.11)).  
  
## <a name="other-performance-counters"></a>Weitere Leistungsindikatoren  
 Benutzerdefinierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Leistungsindikatoren werden nur für die weiter oben aufgeführten Reporting Services-Leistungsobjekte bereitgestellt. Die folgenden .NET Framework-Leistungsobjekte stellen zusätzliche Leistungsüberwachungsdaten für den Berichtsserver bereit.
 
 > [!NOTE]
 > In Power BI-Berichtsserver und SQL Server Reporting Services 2017 und höher sind keine Reporting Services-Leistungsobjekte enthalten. Zur Leistungsüberwachung für den Berichtsserver stehen [.NET Framework-Leistungsindikatoren](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/performance-counters) zur Verfügung. 
 
|Leistungsobjekt|Notizen|  
|------------------------|-----------|  
|**.NET CLR Data** und **.NET CLR Memory**|Das Webportal nutzt [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]-Leistungsindikatoren. Weitere Informationen erhalten Sie, wenn Sie [Improving .NET Application Performance and Scalability](https://www.microsoft.com/download/details.aspx?id=11711) herunterladen.|  
|**Prozess**|Fügen Sie die Leistungsindikatoren **Elapsed Time** und **ID Process** für eine ReportingServicesService-Instanz hinzu, um die Prozessbetriebszeit nach Prozess-ID nachzuverfolgen.|  
  
## <a name="sharepoint-events"></a>SharePoint-Ereignisse  
 Zusätzlich zu den [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Leistungsobjekten möchten Sie möglicherweise auch SharePoint-Ereignisse konfigurieren, wenn Sie einen Berichtsserver im integrierten SharePoint-Modus ausführen und die Berichterstellungsumgebung für die Verwendung eines SharePoint-Produkts konfiguriert haben. Mit den Ereignissen für einen Berichtsserver im integrierten SharePoint-Modus in diesem Abschnitt können Sie Diagnoseereignisse prüfen, die möglicherweise hilfreiche Informationen zur Verfügung stellen, wenn Ihre Berichterstellungsumgebung mit SharePoint integriert ist.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 [Leistungsindikatoren für den MSRS 2016-Webdienst und den MSRS 2016-Windows-Dienst, Leistungsobjekte (einheitlicher Modus)](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)  
 Beschreibt die Leistungsindikatoren, die vom Berichtsserver-Webdienst verwendet werden.  
  
 [Leistungsindikatoren für den MSRS 2016-Webdienst im SharePoint-Modus und den MSRS 2016-Windows-Dienst im SharePoint-Modus, Leistungsobjekte (SharePoint-Modus)](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)  
 Beschreibt die Leistungsindikatoren, die vom Berichtsserver-Windows-Dienst verwendet werden.  
  
 [Performance Counters for the ReportServer:Service  and ReportServerSharePoint:Service Performance Objects (Leistungsindikatoren für die Leistungsobjekte ReportServer:Service und ReportServerSharePoint:Service)](../../reporting-services/report-server/performance-counters-reportserver-service-performance-objects.md)  
 Beschreibt die HTTP-bezogenen und speicherbezogenen Leistungsindikatoren in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
## <a name="see-also"></a>Weitere Informationen  
 [Konfigurieren von verfügbarem Speicher für Berichtsserveranwendungen](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md)   
 [Reporting Services-Berichtsserver &#40;einheitlicher Modus&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
 [Reporting Services-Tools](../../reporting-services/tools/reporting-services-tools.md)  
  
