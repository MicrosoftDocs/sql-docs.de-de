---
title: Veröffentlichen einer freigegebenen Datenquelle in einer SharePoint-Bibliothek | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie eine freigegebene Datenquelle auf einem Berichtsserver veröffentlichen, der im integrierten SharePoint-Modus ausgeführt wird.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reports
ms.topic: conceptual
helpviewer_keywords:
- data sources [Reporting Services], publishing to a SharePoint library
- SharePoint integration [Reporting Services], publishing to a library
- publishing reports [Reporting Services], to a SharePoint library
ms.assetid: 966ed425-3ce2-4e76-8237-3c1c977954ae
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0a60966d6ba73a7669562d406b460f749303ca56
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91988602"
---
# <a name="publish-a-shared-data-source-to-a-sharepoint-library"></a>Veröffentlichen einer freigegebenen Datenquelle in einer SharePoint-Bibliothek
  Wenn Sie eine freigegebene Datenquelle auf einem Berichtsserver veröffentlichen möchten, der im integrierten SharePoint-Modus ausgeführt wird, müssen Sie die Berichtsprojekteigenschaften im Berichts-Designer festlegen. In den Projekteigenschaften müssen alle Verweise auf Server, Berichte und freigegebene Datenquellen vollqualifizierte URLs sein.  
  
 Sie müssen für die SharePoint-Website über eine Berechtigung als **Mitglied** oder **Besitzer** verfügen. Weitere Informationen finden Sie unter [Beispiele für URLs von veröffentlichten Berichtselementen auf einem Berichtsserver im SharePoint-Modus &#40;SSRS&#41;](../../reporting-services/tools/url-examples-for-items-on-a-report-server-sharepoint-mode.md).  
  
### <a name="to-publish-a-shared-data-source-to-a-sharepoint-site"></a>So veröffentlichen Sie eine freigegebene Datenquelle auf einer SharePoint-Website  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]das vorhandene bzw. ein neues Berichtsserverprojekt.  
  
2.  Klicken Sie im Menü **Projekt** auf **Eigenschaften**. Das Dialogfeld _\<project>_ **Eigenschaftenseiten** wird geöffnet.  
  
3.  Wählen Sie die **Konfiguration** aus, die Sie zum Veröffentlichen auf einer SharePoint-Website verwenden.  
  
4.  Wenn Sie die freigegebenen Datenquellen in Ihrem Projekt veröffentlichen und bereits veröffentlichte freigegebene Datenquellen überschreiben möchten, legen Sie **OverwriteDataSources** auf **True**fest.  
  
5.  (Optional) Geben Sie für **TargetDataSourceFolder**eine URL zu einer SharePoint-Bibliothek oder zu einem Bibliotheksordner ein. Beispiel: `https://TestServer/TestSite/Documents/DataSources`.  
  
     Wenn Sie keinen Wert angeben, wird der Wert **TargetReportFolder** verwendet.  
  
6.  Geben Sie für **TargetReportFolder**eine URL zu einer Bibliothek oder einem Bibliotheksordner ein. Beispiel: `https://TestServer/TestSite/Documents/Reports`.  
  
7.  Geben Sie für **TargetServerURL**eine URL zu einer SharePoint-Website auf oberster Ebene oder zu einer Unterwebsite ein. Wenn Sie keine Website angeben, wird die standardmäßige Stammwebsite verwendet. Beispiel: `https://servername`, `https://servername/site` oder `https://servername/site/subsite`.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
9. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die zu veröffentlichende freigegebene Datenquelle, und klicken Sie anschließend auf **Bereitstellen**. Die Datenquelle wird an dem in **TargetDataSourceFolder**angegebenen Speicherort veröffentlicht. Im Ausgabefenster werden Bereitstellungsfehler angezeigt.  
  
    > [!NOTE]  
    >  Nachdem Sie eine freigegebene Datenquelle auf einer SharePoint-Website veröffentlicht haben, wird die Dateinamenerweiterung in RSDS geändert. Sie können eine freigegebene Datenquelle direkt auf der SharePoint-Website bearbeiten und verwalten. Weitere Informationen finden Sie unter [Erstellen und Verwalten von freigegebenen Datenquellen (Reporting Services im integrierten SharePoint-Modus)](/previous-versions/sql/).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Veröffentlichen eines Berichts in einer SharePoint-Bibliothek](../../reporting-services/reports/publish-a-report-to-a-sharepoint-library.md)   
 [Beispiele für URLs von veröffentlichten Berichtselementen auf einem Berichtsserver im SharePoint-Modus (SSRS)](../../reporting-services/tools/url-examples-for-items-on-a-report-server-sharepoint-mode.md)   
 [Eigenschaftsseiten für Projekt (Dialogfeld)](../../reporting-services/tools/project-property-pages-dialog-box.md)   
 [Festlegen von Bereitstellungseigenschaften &#40;Reporting Services&#41;](../../reporting-services/tools/set-deployment-properties-reporting-services.md)   
 [Veröffentlichen von Berichten auf einem Berichtsserver](../../reporting-services/reports/publishing-reports-to-a-report-server.md)   
 [Verwenden einer Office Data Connection &#40;.odc&#41; für Berichte &#40;Reporting Services im integrierten SharePoint-Modus&#41;](../../reporting-services/report-data/use-an-office-data-connection-odc-with-reports.md)  
  
