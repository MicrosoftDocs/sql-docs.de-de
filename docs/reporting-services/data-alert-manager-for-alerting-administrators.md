---
title: Datenwarnungs-Manager für Warnungsadministratoren | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Warnungen über den Datenwarnungs-Manager für SharePoint in SQL Server Reporting Services verwalten.
ms.date: 07/02/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- managing, alerts
- managing, data alerts
ms.assetid: 32fd968f-1c0c-4ba8-851c-8a3b5e1fbbf2
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 7bd656cc4b543ecb2c291adcd9745f3c4b1c79bf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484342"
---
# <a name="data-alert-manager-for-alerting-administrators"></a>Datenwarnungs-Manager für Warnungsadministratoren

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016](../includes/ssrs-appliesto-2016.md)] [!INCLUDE [ssrs-appliesto-not-2017](../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../includes/ssrs-previous-versions.md)]

SQL Server Reporting Services stellt den Datenwarnungs-Manager zum Verwalten von Datenwarnungen für SharePoint-Warnungsadministratoren bereit. Warnungsadministratoren können Informationen zu allen auf der Website gespeicherten Warnungen anzeigen und Warnungen löschen. Das folgende Bild zeigt SharePoint-Warnungsadministratoren die verfügbaren Funktionen im Datenwarnungs-Manager.

![Warnungs-Manager für SharePoint-Websiteadministratoren](../reporting-services/media/rs-alertmanagersite.gif "Warnungs-Manager für SharePoint-Websiteadministratoren")

> [!NOTE]
> Die Integration von Reporting Services in SharePoint ist nach SQL Server 2016 nicht mehr möglich.

 Wenn die Website für Datenwarnungen aktiviert ist, werden zwei SharePoint-Seiten (MyDataAlerts.aspx und SiteDataAlerts.aspx) erstellt und der SharePoint-Website hinzugefügt. SiteDataAlerts.aspx entspricht dem Datenwarnungs-Manager für Warnungsadministratoren. Warnungsadministratoren können den Datenwarnungs-Manager über die SharePoint-Seite "Siteeinstellungen" öffnen. Warnungsadministratoren müssen die SharePoint-Berechtigung "Benachrichtigungen verwalten" besitzen, um den Datenwarnungs-Manager zu öffnen.  
  
 Sie können den Datenwarnungs-Manager auch direkt über eine URL öffnen. Die Syntax der URL wird nachfolgend angezeigt:  
  
 `http: //<site name>/_layouts/ReportServer/ SiteDataAlerts.aspx`  
  
> [!NOTE]  
>  Als Warnungsadministrator können Sie Information Workern die Berechtigung erteilen, auf die [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] -Datenwarnungsfunktionen zuzugreifen. Weitere Informationen zu den erforderlichen Berechtigungen finden Sie unter [Reporting Services-Datenwarnungen](../reporting-services/reporting-services-data-alerts.md).  
  
##  <a name="viewing-data-alert-information"></a><a name="ViewingAlerts"></a> Anzeigen von Datenwarnungsinformationen  
 Wenn Reporting Services installiert ist und in SharePoint konfiguriert wird, enthält die SharePoint-Seite "Siteeinstellungen" die **Reporting Services** -Optionen. Warnungsadministratoren klicken in Reporting Services auf die Option **Datenwarnungen verwalten** , um den Datenwarnungs-Manager zu öffnen. Das folgende Bild zeigt, wie sich der Datenwarnungs-Manager auf der Seite "Siteeinstellungen" öffnen lässt.  
  
 ![Reporting Services-Abschnitt der Seite „Siteeinstellungen“](../reporting-services/media/rs-sitesettings.gif "Reporting Services-Abschnitt der Seite „Siteeinstellungen“")  
  
 Der Datenwarnungs-Manager umfasst eine Tabelle mit dem Warnungsnamen, Berichtsnamen, Namen des Warnungseigentümers, der Nummer der Warnmeldung, der letzten Ausführung der Warnung, der letzten Änderung der Warnungsdefinition und dem Status der Warnmeldung. Wenn die Datenwarnung nicht generiert oder gesendet werden kann, enthält die Statusspalte Informationen zum Fehler und hilft Ihnen bei der Behebung des Problems mit der Warnung. Weitere Informationen finden Sie unter [Verwalten aller Datenwarnungen auf einer SharePoint-Website im Datenwarnungs-Manager](../reporting-services/manage-all-data-alerts-on-a-sharepoint-site-in-data-alert-manager.md).  
  
 In der folgenden Tabelle werden Beispieldaten einer Tabelle im Datenwarnungs-Manager angezeigt. Tritt ein Fehler auf, werden die Fehlermeldung und der Bezeichner des Eintrags im Protokoll (eine GUID) in das Feld **Status** der Tabelle aufgenommen.  
  
|Name der Warnung|Berichtsname|Erstellt von|Gesendete Warnungen|Zuletzt ausgeführt|Zuletzt geändert|Status|  
|----------------|-----------------|----------------|-----------------|--------------|-------------------|------------|  
|SalesQTR|SalesByTerritoryAndQTR|Lauren Johnson|4|6/12/2011|6/1/2011|Die letzte Warnung war erfolgreich, und die Warnung wurde gesendet.|  
|UnitsSold|ProductsSalesByQTR|Michael Blythe|2|7/1/2011|6/28/2011|Die letzte Warnung wurde erfolgreich ausgeführt, aber die Daten blieben unverändert, und es wurde keine Warnung gesendet.|  
|InventoryCount|StockStatusByQTR|Lauren Johnson|7|7/10/2011|7/2/2011|\<error message>"The log file contains detailed information about the error. Refer to the log entry with the identifier: \<GUID>." (Die Protokolldatei enthält ausführliche Informationen zum Fehler. Weitere Informationen erhalten Sie unter dem Protokolleintrag mit dem Bezeichner: <GUID>.)|  
|TopPromotion|PromotionTracking|Cristian Petculescu|0||5/23/2011|Die Warnung wurde erstellt.|  
  
 Weitere Informationen finden Sie unter [Verwalten aller Datenwarnungen auf einer SharePoint-Website im Datenwarnungs-Manager](../reporting-services/manage-all-data-alerts-on-a-sharepoint-site-in-data-alert-manager.md).  
  
 Sie können alle von Websitebenutzern erstellten Warnungen anzeigen. Sie wählen erst einen Benutzer und wählen dann, ob alle Warnungen oder nur Warnungen für einen bestimmten Bericht angezeigt werden sollen.  
  
  
##  <a name="delete-data-alerts"></a><a name="DeleteAlerts"></a> Löschen von Datenwarnungen  
 Warnungsdefinitionen lassen sich über den Datenwarnungs-Manager löschen. Jede Datenwarnungsdefinition hat einen Eigentümer (den SharePoint-Benutzer, der sie erstellt hat). Eigentümer können nur die von ihnen erstellten Warnungsdefinitionen löschen. Weitere Informationen finden Sie unter [Verwalten meiner Datenwarnungen im Datenwarnungs-Manager](../reporting-services/manage-my-data-alerts-in-data-alert-manager.md).  
  
 Ein SharePoint-Warnungsadministrator kann Warnungsdefinitionen auflisten und dann löschen, die von einem beliebigen Benutzer der Website erstellt wurden. Weitere Informationen finden Sie unter [Verwalten aller Datenwarnungen auf einer SharePoint-Website im Datenwarnungs-Manager](../reporting-services/manage-all-data-alerts-on-a-sharepoint-site-in-data-alert-manager.md)  
  
 Nach dem Löschen der Warnungsdefinition werden keine weiteren Warnungen gesendet. Wenn Sie jedoch die Warnungsdatenbank abfragen, stellen Sie möglicherweise fest, dass die Warnungsdefinition immer noch vorhanden ist. Der Warndienst führt den Cleanup nach einem Zeitplan durch. Die Warnungsdefinition wird beim nächsten Cleanup dauerhaft gelöscht. Das Standardintervall für den Cleanup beträgt 20 Minuten. Dieses und andere Cleanupintervalle sind konfigurierbar. Weitere Informationen finden Sie unter [Reporting Services-Datenwarnungen](../reporting-services/reporting-services-data-alerts.md).  
  
  
##  <a name="related-tasks"></a><a name="HowTo"></a> Verwandte Aufgaben  
 In diesem Abschnitt ist eine Prozedur aufgeführt, die das Verwalten der Warnungen zeigt.  
  
-   [Verwalten aller Datenwarnungen auf einer SharePoint-Website im Datenwarnungs-Manager](../reporting-services/manage-all-data-alerts-on-a-sharepoint-site-in-data-alert-manager.md)  

## <a name="see-also"></a>Weitere Informationen

[Reporting Services-Datenwarnungen](../reporting-services/reporting-services-data-alerts.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
