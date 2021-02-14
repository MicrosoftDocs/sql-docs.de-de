---
title: Analyse und Berichterstellung mit Microsoft Business Intelligence-Tools (BI)
description: In diesem Artikel erfahren Sie mehr über Arbeitsauslastungen für die Datenanalyse und die Berichterstellung sowie über die Microsoft BI-Tools, die für diese Arbeitsauslastungen am besten geeignet sind.
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.prod: reporting-services
ms.technology: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.custom: seodec18
ms.date: 07/22/2020
ms.openlocfilehash: bd23d6a1e586a317503374f15e7ecc90381b1563
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978432"
---
# <a name="analysis-and-reporting-with-microsoft-business-intelligence-bi-tools"></a>Analyse und Berichterstellung mit Microsoft Business Intelligence-Tools (BI)

Die Auswahl des richtigen Business Intelligence-Tools kann schwierig sein. Erhalten Sie Informationen zu den verschiedenen Microsoft-Angeboten und finden Sie das für Ihre Anforderungen am besten geeignete Angebot.

In der folgenden Tabelle werden die verschiedenen Arbeitsauslastungen für Datenanalyse und Berichterstellung den Microsoft BI-Tools zugeordnet, die für diese Arbeitsauslastungen am besten geeignet sind. Um weitere Informationen zu einem Produkt zu erhalten, klicken Sie auf den Produktlink in der Tabelle.  
  
 Wenn Sie für die Suche nach den richtigen Tools eine kurze Übersicht benötigen, informieren Sie sich unter [Einführung in Microsoft Business Intelligence (BI)-Tools](https://download.microsoft.com/download/D/2/0/D20E1C5F-72EA-4505-9F26-FEF9550EFD44/Introducing_Microsoft_BI_Tools.docx).

> [!NOTE]
> Die Integration von Reporting Services in SharePoint ist nach SQL Server 2016 nicht mehr möglich.
  
|Arbeitsauslastungen|Benutzer|Excel-basiertes BI-Tool|SharePoint-basiertes BI-Tool|SharePoint Online-basiertes BI-Tool|Power BI-basiertes BI-Tool|SQL Server-basiertes BI-Tool|  
|---------------|----------|-|-|--------------|-|-|  
|**Self-Service-BI**|Analyst/Endbenutzer||||||  
|Einfache Suche und einfacher Zugriff auf öffentliche und Unternehmensdaten||[Excel 2016](https://support.office.com/article/What-s-new-in-Excel-2016-for-Windows-5fdb9208-ff33-45b6-9e08-1f5cdb3a6c73?ui=en-US&rs=en-US&ad=US)|||[Azure Data Catalog](https://azure.microsoft.com/services/data-catalog/)||  
|Erstellen leistungsstarker Datenmodelle||[Power Pivot](https://support.office.com/article/Power-Pivot-Overview-and-Learning-f9001958-7901-4caa-ad80-028a6d2432ed?ui=en-US&rs=en-US&ad=US)|||[Power BI Desktop](/power-bi/fundamentals/desktop-get-the-desktop)||  
|Ausführen von Self-Service-Vorhersageanalysen||||||[Data Mining-Add-Ins für Excel](/previous-versions/sql/2014/analysis-services/data-mining-client-for-excel-sql-server-data-mining-add-ins?view=sql-server-2014&preserve-view=true) |  
|Visualisieren und Untersuchen von Daten||[Power View](https://support.office.com/article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e)<br /><br /> [3D-Karten](https://support.office.com/article/Visualize-your-data-in-3D-Maps-ce6b1d5c-4602-4dae-b487-91ec0268e75d)|||[Power BI Desktop](/power-bi/fundamentals/desktop-get-the-desktop)||  
|Fragen mithilfe von Abfragen in natürlicher Sprache|||||[Q & A](/power-bi/consumer/end-user-q-and-a)|
|Zugreifen auf Berichte über mobile Geräte||||[HTML 5 (unterstützt die Anzeige von Dateien unter 10 MB)](create-deploy-and-manage-mobile-and-paginated-reports.md)<br /><br /> | [HTML 5 (unterstützt die Anzeige von unter 250 GB)](https://www.microsoft.com/en-us/microsoft-365/blog/2013/07/08/what-powers-power-bi-in-office-365/)<br /><br /> [Mobile Power BI-App auf iOS-Geräten](/power-bi/consumer/mobile/mobile-iphone-app-get-started)<br /><br /> [Mobile Power BI-App auf Android-Geräten](/power-bi/consumer/mobile/mobile-android-app-get-started) <br /><br /> [Mobile Power BI-App für Windows 10](/power-bi/consumer/mobile/mobile-windows-10-phone-app-get-started)|  
|Zusammenarbeit und gemeinsame Nutzung|||[SharePoint-Websites](/sharepoint/getting-started)|[SharePoint-Teamwebsites](https://go.microsoft.com/fwlink/?LinkId=391850)|[Power BI-Websites](/power-bi/service-how-to-collaborate-distribute-dashboards-reports)||  
|**Unternehmens-BI**|IT-Profi||||||  
|Erstellen mehrdimensionaler/tabellarischer Unternehmensmodelle||||||[Analysis Services](/analysis-services/analysis-services-overview)|  
|Erstellen von Ad-hoc-Datenvisualisierungen|||[Power View für SharePoint](https://go.microsoft.com/fwlink/?LinkId=391858)||||  
|Erstellen von Dashboards|||[SharePoint-Dashboards](https://go.microsoft.com/fwlink/?LinkId=391859)<br /><br /> [PerformancePoint-Dienste](/SharePoint/administration/performancepoint-services-overview)||[Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/)||  
|Erstellen von Betriebsberichten||||||*[Reporting Services](create-deploy-and-manage-mobile-and-paginated-reports.md)|  
|Erstellen benutzerdefinierter und eingebetteter Berichte|||||[Power BI Embedded](create-deploy-and-manage-mobile-and-paginated-reports.md)|  
|**Erweiterte Analysen**|Data Scientist||||||  
|Ausführen von Self-Service-Vorhersageanalysen||||||[Data Mining-Add-Ins für Excel](/previous-versions/sql/2014/analysis-services/data-mining-client-for-excel-sql-server-data-mining-add-ins?view=sql-server-2014&preserve-view=true) |  
|Verwenden von Data Mining-Algorithmen||||||[Data Mining in Analysis Services (Data Mining in Analysediensten)](/analysis-services/data-mining/data-mining-ssas)<br/><br/>[SQL Server R Services](../machine-learning/r/sql-server-r-services.md?viewFallbackFrom=sql-server-ver15)|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |
