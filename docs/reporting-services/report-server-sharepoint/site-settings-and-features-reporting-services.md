---
title: Einstellungen und Funktionen für Reporting Services-Websites (SharePoint-Modus) | Microsoft-Dokumentation
description: Es gibt benutzerdefinierte Features auf Websiteebene und Websitefeatures, die Sie auf der Seite zu Websiteeinstellungen für SharePoint im SharePoint-Modus in den Reporting Services verwalten können.
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: feabf5c1a0847983af4a10dea5d4c15a5f51199d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461431"
---
# <a name="reporting-services-site-settings-and-site-features-sharepoint-mode"></a>Einstellungen und Funktionen für Reporting Services-Websites (SharePoint-Modus)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Der SharePoint-Modus von Reporting Services bietet mehrere benutzerdefinierte Funktionen auf Websiteebene sowie Websitefunktionen, die auf der Seite mit den SharePoint-Websiteeinstellungen verwaltet werden können. Die Einstellungen gelten auf der gesamten Website und für alle Reporting Services-Dienstanwendungen. Sie müssen über Inhalts-Manager- und Systemadministratorberechtigungen verfügen, um diese Seite anzuzeigen.  

> [!NOTE]
> Die Integration von Reporting Services in SharePoint ist nach SQL Server 2016 nicht mehr möglich.

|Siteeinstellung|BESCHREIBUNG|  
|------------------|-----------------|  
|Websiteeinstellungen für Reporting Services|In diesem Thema beschriebene websiteweite Einstellungen.|  
|Datenwarnungen verwalten|Verwaltung der Datenwarnungsfunktion.|  
|Berichtsserver-Dateisynchronisierung|Eine Funktion auf Websiteebene, die standardmäßig deaktiviert wird.<br /><br /> Synchronisiert Berichtsserverdateien (.rdl, .rsds, .smdl, .rsd, .rsc, .rdlx) einer SharePoint-Dokumentbibliothek mit dem Berichtsserver, wenn Dateien direkt in der Dokumentbibliothek hinzugefügt oder aktualisiert werden.<br /><br /> Weitere Informationen finden Sie unter [Aktivieren des Features zur Berichtsserver-Dateisynchronisierung in der SharePoint-Zentraladministration](../../reporting-services/report-server-sharepoint/activate-the-report-server-file-sync-feature-in-sharepoint-ca.md).|  
  
## <a name="open-the-reporting-services-site-settings-page"></a>Öffnen der Reporting Services-Seite „Siteeinstellungen“
  
1.  Klicken Sie auf der SharePoint-Website im Menü **Websiteaktionen** auf **Siteeinstellungen**.  
  
2.  Klicken Sie im Abschnitt **Reporting Services** auf **Einstellungen für Reporting Services-Websites**.  
  
## <a name="options-for-reporting-services-site-settings"></a>Optionen für Reporting Services-Websiteeinstellungen
  
|Option|BESCHREIBUNG|  
|------------|-----------------|  
|**Download des RSClientPrint-ActiveX-Steuerelements aktivieren**|Vom Steuerelement wird ein benutzerdefiniertes Dialogfeld zum Drucken angezeigt, das die in Dialogfeldern zum Drucken üblichen Funktionen enthält. Dazu zählen Druckvorschau, Seitenauswahl zum Angeben bestimmter Seiten und Bereiche, Seitenränder und Ausrichtung. Weitere Informationen zum Steuerelement finden Sie unter [Using the RSClientPrint Control in Custom Applications](../../reporting-services/report-server-web-service/net-framework/using-the-rsclientprint-control-in-custom-applications.md)|  
|**Remotefehler im lokalen Modus aktivieren**|Zeigen Sie bei der Ausführung im lokalen Modus ausführliche Fehlermeldungen auf Remotecomputern an, oder blenden Sie sie aus. Wenn ungefähr folgende Fehlermeldung angezeigt wird, ist die Aktivierung von Remotefehlern möglicherweise nützlich:<br /><br /> `For more information about this error navigate to the report server on the local server machine or enable remote errors`|  
|**Barrierefreiheit-Metadaten für Berichte aktivieren**|Barrierefreiheit-Metadaten in der HTML-Ausgabe für Berichte aktivieren|  
|**Genaue Größenanpassung der Datenvisualisierung für Berichte aktivieren**|Konfigurieren Sie das Verhalten für die Datenvisualisierungsanpassung in einem Tablix, damit sie genau passt. Dies schließt Diagramm, Messgerät und Karte ein. Ist die Funktion deaktiviert, bewirkt das Verhalten für Datenvisualisierungen die ungefähre Anpassung, wodurch möglicherweise Leerzeichen auftreten. Diese Einstellung gilt nur für das Rendern im Berichts-Viewer-Webpart. Sie müssen zum Verwalten dieses Verhaltens für serverseitiges Rendering die Datei **rsreportserver.config** ändern. Weitere Informationen finden Sie unter<br /><br /> [Konfigurationsdatei „RsReportServer.config“](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).<br /><br /> [Customize Rendering Extension Parameters in RSReportServer.Config](../../reporting-services/customize-rendering-extension-parameters-in-rsreportserver-config.md).<br /><br /> [HTML Device Information Settings](../../reporting-services/html-device-information-settings.md).<br /><br /> Die Aktivierung der genauen Anpassung hat möglicherweise Auswirkungen auf die Leistung, da die Verarbeitung zur Ermittlung der genauen Größe möglicherweise länger als eine ungefähre Anpassung dauert.|  
  
## <a name="see-also"></a>Weitere Informationen

 [Manage a Reporting Services SharePoint Service Application (Verwalten einer Reporting Services-SharePoint-Dienstanwendung)](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)  
  
  
