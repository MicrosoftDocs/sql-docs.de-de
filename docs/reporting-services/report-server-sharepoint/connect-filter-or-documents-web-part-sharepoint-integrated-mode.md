---
title: Verbinden des Webparts „Filter“ oder „Dokumente“ mit einem Webpart des Berichts-Viewers von Reporting Services | Microsoft-Dokumentation
description: 'Für SharePoint-Produkte: Hier erfahren Sie, wie Sie ein Dashboard oder eine Webpartseite erstellen, die ein Filter- oder ein Dokumentwebpart und ein Berichts-Viewer-Webpart enthält.'
ms.date: 11/26/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0e58600c502aa6ebb0a031480ab5bb66f48833ca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074851"
---
# <a name="connect-filter-or-documents-web-part-with-a-reporting-services-report-viewer-web-part"></a>Verbinden des Webparts „Filter“ oder „Dokumente“ mit einem Webpart des Berichts-Viewers von Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Wenn Sie ein SharePoint-Produkt verwenden, können Sie ein Dashboard oder eine Webpartseite erstellen, die ein Webpart „Filter“ oder ein Webpart „Dokumente“ und ein Berichts-Viewer-Webpart enthält. Unterstützte Versionen sind [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] oder [!INCLUDE[SPS2010](../../includes/sps2010-md.md)]. Ebenfalls unterstützt wird [!INCLUDE[winSPServ3](../../includes/winspserv3-md.md)] oder [!INCLUDE[offSPServ](../../includes/offspserv-md.md)] 2007. Durch Verbinden eines Filterwebparts können Benutzer, die Filterwerte in einem Filterwebpart auswählen, den Wert an einen parametrisierten Bericht auf derselben Seite senden. Durch Verbinden eines Webparts „Dokumente“ können Benutzer, die auf Berichte in der Bibliothek „Dokumente“ klicken, den Bericht in einem zugehörigen Berichts-Viewer-Webpart anzeigen.

> [!NOTE]
> Die Integration von Reporting Services in SharePoint ist nach SQL Server 2016 nicht mehr möglich.

 Das Filter-Webpart wird zum Senden von Werten an mindestens einen Parameter in einem Bericht verwendet. Wenn Sie ein Filter-Webpart verwenden möchten, müssen für den Bericht Parameter definiert sein, die mit den vom Webpart gesendeten Werten, Datentypen und dem Format kompatibel sind.  
  
 Das Webpart „Dokumente“ ist der Bibliothek „Dokumente“ der Homepage zugeordnet. Klicken Sie auf **Alle Websiteinhalte einblenden**, um Elemente aus der Bibliothek Dokumente anzuzeigen, hinzuzufügen oder zu entfernen. Klicken Sie in Bibliotheken auf **Dokumente**. Mit den Menüs **Neu**, **Upload** und **Aktionen** können Sie die Elemente in der Bibliothek Dokumente verwalten.  
  
## <a name="connect-a-filter-web-part"></a>Verbinden eines Filter-Webparts
  
1.  Öffnen oder erstellen Sie eine Webpartseite oder ein Dashboard.  
  
2.  Klicken Sie im Menü **Websiteaktionen** auf **Seite bearbeiten**.  
  
3.  Klicken Sie auf **Webpart hinzufügen**.  
  
4.  Klicken Sie unter **Alle Webparts** in der Kategorie **Verschiedenes** auf **Berichts-Viewer für SQL Reporting Services**.  
  
5.  Klicken Sie auf **Hinzufügen**. Das Webpart wird oben in der Zone hinzugefügt.  
  
6.  Klicken Sie in einer anderen Zone auf derselben Webpartseite oder demselben Dashboard auf **Webpart hinzufügen**.  
  
7.  Wählen Sie unter **Alle Webparts** im Abschnitt **Filter** ein Webpart aus.  
  
8.  Klicken Sie auf **Hinzufügen**. Das Webpart wird oben in der Zone hinzugefügt.  
  
9. Klicken Sie in der Zone mit dem Webpart auf das Menü **Bearbeiten** des Webparts, zeigen Sie auf **Verbindungen**, dann auf **Filterwerte senden an**, und klicken Sie anschließend auf **Berichts-Viewer** - *Berichtsname*.  
  
10. Checken Sie Ihre Änderungen ein, und speichern Sie die Seite.  
  
## <a name="connect-a-documents-web-part"></a>Verbinden eines Dokumente-Webparts  
  
1.  Öffnen oder erstellen Sie eine Webpartseite oder ein Dashboard.  
  
2.  Klicken Sie im Menü **Websiteaktionen** auf **Seite bearbeiten**.  
  
3.  Klicken Sie auf **Webpart hinzufügen**.  
  
4.  Klicken Sie unter **Alle Webparts** im Abschnitt **Listen und Bibliotheken** auf **Dokumente**.  
  
5.  Klicken Sie auf **Hinzufügen**. Das Webpart wird oben in der Zone hinzugefügt.  
  
6.  Klicken Sie unten im Toolbereich auf **Anwenden** , und klicken Sie anschließend auf **OK** , um den Bereich zu schließen.  
  
7.  Klicken Sie in einer anderen Zone auf derselben Webpartseite oder demselben Dashboard auf **Webpart hinzufügen**.  
  
8.  Klicken Sie unter **Alle Webparts** in der Kategorie **Verschiedenes** auf **Berichts-Viewer für SQL Reporting Services**.  
  
9. Klicken Sie auf **Hinzufügen**. Das Webpart wird oben in der Zone hinzugefügt.  
  
10. Klicken Sie in der Zone mit dem Webpart auf das Menü **Bearbeiten** des Webparts, zeigen Sie auf **Verbindungen**, dann auf **Berichtsdefinitionen abrufen von**, und klicken Sie anschließend auf **Dokumente**.  
  
11. Checken Sie Ihre Änderungen ein, und speichern Sie die Seite.  
  
## <a name="see-also"></a>Weitere Informationen

 [Add the Report Viewer Web Part to a Web Page (Hinzufügen des Berichts-Viewer-Webparts zu einer Webseite)](../../reporting-services/report-server-sharepoint/add-the-report-viewer-web-part-to-a-web-page.md)   
 [Berichts-Viewer-Webpart auf einer SharePoint-Website](./report-viewer-web-part-sharepoint-site.md)   
 [Customize the Report Viewer Web Part (Anpassen des Berichts-Viewer-Webparts)](../../reporting-services/report-server-sharepoint/customize-the-report-viewer-web-part.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)