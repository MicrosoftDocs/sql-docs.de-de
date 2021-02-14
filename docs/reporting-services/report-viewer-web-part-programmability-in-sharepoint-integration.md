---
title: Berichts-Viewer-Webpart-Programmierbarkeit in SharePoint-Integration | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie benutzerdefinierte Webparts erstellen, die mithilfe von Webpartverbindungen Berichtspfade und Parameter zu Berichts-Viewer-Webparts angeben.
ms.date: 03/04/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: reference
ms.assetid: 714017b7-1bd6-4950-a3c6-d0df8450a877
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d7054e2af081bd76505f3adebbae4cc7541f8a44
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023274"
---
# <a name="report-viewer-web-part-programmability-in-sharepoint-integration"></a>Berichts-Viewer-Webpart-Programmierbarkeit in SharePoint-Integration
  Das Berichts-Viewer-Webpart ist ein Serversteuerelement, das mehrere öffentliche Anwendungsprogrammierschnittstellen (API) enthält, die es Entwicklern ermöglichen, benutzerdefinierte SharePoint-Anwendungen zu erstellen. Sie können benutzerdefinierte Webparts erstellen, die Berichtspfad und Parameter mit Webpartverbindungen zu Berichts-Viewer-Webparts angeben. Sie können auch das Webpart in eine benutzerdefinierte SharePoint-Webpartseite einbetten und es mit der öffentlichen API anpassen.  
  
## <a name="connecting-to-report-viewer-web-part-with-custom-web-parts"></a>Herstellen einer Verbindung mit Berichts-Viewer-Webpart mit benutzerdefinierten Webparts  
 Das Berichts-Viewer-Webpart ist ein Verbindungsconsumer zu SharePoint-Webparts, die <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> oder „T:Microsoft.SharePoint.WebPartPages.IFilterValues“ implementieren. Ein <xref:System.Web.UI.WebControls.WebParts.IWebPartRow>-Webpart wie das **Dokumente**-Webpart kann einen Berichtspfad zu einem Berichts-Viewer-Webpart angeben, wenn es auf der gleichen Webpartseite wie das Berichts-Viewer-Webpart platziert wird. Genauso kann ein T:Microsoft.SharePoint.WebPartPages.IFilterValues-Webpart, wie z.B. der **Textfilter** oder der **Auswahlfilter**, einen Berichtsparameter zu einem Berichts-Viewer-Webpart angeben, wenn es auf der gleichen Webpartseite wie das Berichts-Viewer-Webpart platziert wird.  
  
### <a name="implementing-a-report-path-provider-with-iwebpartrow"></a>Implementieren eines Berichtspfadanbieters mit IWebPartRow  
 Um durch Webpartverbindungen einen Berichtspfad zum Berichts-Viewer-Webpart anzugeben, gehen Sie wie folgt vor:  
  
1.  Erstellen Sie einen Webpart, der die <xref:System.Web.UI.WebControls.WebParts.IWebPartRow>-Schnittstelle implementiert.  
  
2.  Fügen Sie den Webpart zur selben Webpartseite wie den Berichts-Viewer-Webpart hinzu.  
  
3.  Verbinden Sie den Webpart mit dem Berichts-Viewer-Webpart in der webbasierten Webpart-Entwurfsbenutzeroberfläche.  
  
    > [!NOTE]  
    >  Sie können nur jeweils ein <xref:System.Web.UI.WebControls.WebParts.IWebPartRow>-Webpart mit dem Berichts-Viewer-Webpart verbinden, und Sie können nicht gleichzeitig ein <xref:System.Web.UI.WebControls.WebParts.IWebPartRow>-Webpart und ein T:Microsoft.SharePoint.WebPartPages.IFilterValues-Webpart mit dem Berichts-Viewer-Webpart verbinden.  
  
 Führen Sie folgende in der <xref:System.Web.UI.WebControls.WebParts.IWebPartRow>-Methode beschriebenen Schritte aus, damit Ihr <xref:System.Web.UI.WebControls.WebParts.IWebPartRow.GetRowData%2A>-Webpart einwandfrei mit dem T:Microsoft.ReportingServices.SharePoint.UI.WebParts.ReportViewer-Webpart funktioniert:  
  
-   Rufen Sie die Rückrufmethode mit einem <xref:System.Data.DataRowView>-Objekt als Eingabeparameter auf.  
  
-   Stellen Sie sicher, dass das <xref:System.Data.DataRowView>-Objekt eine Spalte mit dem Namen "DocUrl" enthält, die den Berichtspfad enthält.  
  
    > [!NOTE]  
    >  Das Berichts-Viewer-Webpart im Add-In für [!INCLUDE[offSPServ](../includes/offspserv-md.md)] 2010 unterstützt auch das Empfangen von Berichtspfaden, die die Spalte "FileRef" verwenden.  
  
### <a name="implementing-a-report-parameter-provider-with-ifiltervalues"></a>Implementieren eines Berichtsparameteranbieters mit IFilterValues  
 Ein Webpart, das T:Microsoft.SharePoint.WebPartPages.IFilterValues implementiert, kann einen Parameterwert für das Berichts-Viewer-Webpart bereitstellen. Der an den Berichts-Viewer-Webpart gesendete Parameterwert unterliegt gemäß der Angabe in der Berichtsdefinition denselben Einschränkungen wie der Berichtsparameter, zum Beispiel Datentyp, gültige Werte usw.  
  
 Um dem Berichts-Viewer-Webpart einen Berichtsparameter bereitzustellen, gehen Sie wie folgt vor:  
  
1.  Erstellen Sie ein Webpart, das die T:Microsoft.SharePoint.WebPartPages.IFilterValues-Schnittstelle implementiert.  
  
2.  Fügen Sie das Webpart zu derselben Seite wie das T:Microsoft.ReportingServices.SharePoint.UI.WebParts.ReportViewer-Webpart hinzu.  
  
3.  Verbinden Sie Ihr T:Microsoft.SharePoint.WebPartPages.IFilterValues-Webpart mit dem Berichts-Viewer-Webpart in der webbasierten Webpart-Entwurfsbenutzeroberfläche.  
  
    > [!NOTE]  
    >  Sie können mehrere T:Microsoft.SharePoint.WebPartPages.IFilterValues-Webparts gleichzeitig mit dem Berichts-Viewer-Webpart verbinden. Allerdings können Sie nicht gleichzeitig ein <xref:System.Web.UI.WebControls.WebParts.IWebPartRow>-Webpart und ein T:Microsoft.SharePoint.WebPartPages.IFilterValues-Webpart mit dem Berichts-Viewer-Webpart verbinden.  
  
  
