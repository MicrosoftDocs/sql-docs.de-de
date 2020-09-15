---
title: Aktivieren des Features zur Berichtsserver-Dateisynchronisierung in SharePoint | Microsoft-Dokumentation
description: Das Dateisynchronisierungsfeature der Reporting Services für Berichtsserver verwendet SharePoint-Ereignishandler zum Synchronisieren des Berichtsserverkatalogs mit Elementen in Dokumentbibliotheken.
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: e05dfa8cf6b519468ec631c5b6aa0eb49c040141
ms.sourcegitcommit: e8f6c51d4702c0046aec1394109bc0503ca182f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87934263"
---
# <a name="activate-the-report-server-file-sync-feature-in-sharepoint"></a>Aktivieren des Features zur Berichtsserver-Dateisynchronisierung in SharePoint

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

Das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Feature zur Berichtsserver-Dateisynchronisierung verwendet SharePoint-Ereignishandler, um den Berichtsserverkatalog mit Elementen in Dokumentbibliotheken zu synchronisieren. Diese Funktion ist nützlich, wenn Benutzer veröffentlichte Berichtselemente häufig direkt in die SharePoint-Dokumentbibliotheken hochladen. Wenn die Dateisynchronisierungsfunktion nicht aktiviert ist, werden Inhalte weiterhin synchronisiert. Dies erfolgt allerdings nicht so häufig.

> [!NOTE]
> Die Integration von Reporting Services in SharePoint ist nach SQL Server 2016 nicht mehr möglich.
  
 Das Feature zur Dateisynchronisierung kann in der SharePoint-Websiteverwaltung aktiviert werden, nachdem Sie das [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)]-Add-In für SharePoint-Produkte installiert haben.  
  
 Die Funktion kann für einzelne Websites manuell aktiviert und deaktiviert werden, jedoch nicht auf Ebene der Websitesammlung.  
  
## <a name="prerequisites"></a>Voraussetzungen

 Das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Add-In für SharePoint muss installiert sein. Falls das Add-In nicht installiert ist, wird die Dateisynchronisierungsfunktion nicht in der Funktionsliste der Website angezeigt.  
  
 Um die Installation zu überprüfen, zeigen Sie die Liste der installierten Anwendungen in der [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows- **Systemsteuerung**an. Wenn das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Add-In installiert ist, folgen Sie den Anweisungen in diesem Thema, um das Feature zur Berichtsserver-Dateisynchronisierung zu aktivieren.  
  
### <a name="to-activate-or-deactivate-the-reporting-services-file-sync-feature-on-a-site"></a>So aktivieren oder deaktivieren Sie das Feature zur Berichtsserver-Dateisynchronisierung auf einer Website
  
1.  Klicken Sie auf der Hauptseite der Website auf das Menü **Websiteaktionen** , und klicken Sie dann auf **Siteeinstellungen**.  
  
2.  Klicken Sie im Bereich **Websiteaktionen** auf **Websitefunktionen verwalten**.  
  
3.  Suchen Sie **Berichtsserver-Dateisynchronisierung** in der Liste.  
  
4.  Klicken Sie auf **Aktivieren**.  

> [!NOTE]
> Gehen Sie auf die gleiche Weise vor, um das Feature zur Berichtsserver-Dateisynchronisierung zu deaktivieren. Klicken Sie jedoch auf **Deaktivieren**.

## <a name="see-also"></a>Weitere Informationen

 [Aktivieren der Berichtsserver- und Power View-Integrationsfunktionen in SharePoint](../../reporting-services/report-server-sharepoint/site-collection-features-report-server-and-power-view.md)   
 [Installieren oder Deinstallieren des Reporting Services-Add-Ins für SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)   
 [Installieren oder Deinstallieren des Reporting Services-Add-Ins für SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
