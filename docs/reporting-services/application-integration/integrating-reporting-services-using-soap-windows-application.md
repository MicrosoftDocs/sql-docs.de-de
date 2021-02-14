---
title: Verwenden der SOAP-API in Windows-Anwendungen
description: Über die Reporting Services-SOAP-API können Sie auf die Funktionen des Berichtsservers zugreifen. Sie können auf den Webdienst in einer Windows-App zugreifen, indem Sie Aufrufe für den Dienst ausführen.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.custom: seo-lt-2019
ms.topic: reference
helpviewer_keywords:
- rendered reports [Reporting Services]
- Windows applications [Reporting Services]
- Windows Forms [Reporting Services]
- SOAP [Reporting Services], Windows applications
ms.assetid: e4804792-20cd-4df2-9257-fb958ff447b4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a91f7cd3a95b722db1c595767ed71900bc55af55
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082561"
---
# <a name="integrating-reporting-services-using-soap---windows-application"></a>Integrieren von Reporting Services mit SOAP: Windows-Anwendung
  Über die Reporting Services-SOAP-API können Sie auf alle Funktionen des Berichtsservers zugreifen. Bei der SOAP-API handelt es sich um einen Webdienst, auf den problemlos zugegriffen werden kann, um Funktionen zur Unternehmensberichterstellung für benutzerdefinierte Geschäftsanwendungen bereitzustellen. Sie können in einer Windows-Anwendung auf den Webdienst zugreifen, indem Sie einfach Code schreiben, mit dem der Dienst aufgerufen wird. Mit [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] können Sie eine Proxyklasse generieren, die die Eigenschaften und Methoden des Webdiensts verfügbar macht und es Ihnen ermöglicht, bei der Erstellung von Geschäftsanwendungen auf Basis von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Technologie eine vertraute Infrastruktur und vertraute Tools zu verwenden.  
  
## <a name="integrating-report-management-functionality-using-windows-forms"></a>Integrieren der Berichtsverwaltungsfunktionalität mit Windows Forms  
 Anders als beim URL-Zugriff macht die SOAP-API sämtliche Verwaltungsfunktionen verfügbar, die über den Berichtsserver zur Verfügung stehen. Dies bedeutet, dass Entwicklern über SOAP sämtliche Administratorfunktionen des Berichts-Managers zur Verfügung stehen. Mit Windows Forms können Sie also ein vollständiges Management- und Verwaltungstool entwickeln. Sie können beispielsweise Benutzern in der Windows-Anwendung ermöglichen, den Inhalt des Berichtsserver-Namespace abzurufen. Zu diesem Zweck haben Sie die Möglichkeit, mit der <xref:ReportService2010.ReportingService2010.ListChildren%2A>-Methode des Webdiensts alle Elemente in der Berichtsserver-Datenbank aufzulisten und anschließend das Steuerelement Listview, Treeview oder Combobox zu verwenden, um diese Elemente für die Benutzer anzuzeigen. Den folgenden Webdienstcode können Sie verwenden, um die aktuelle Liste der verfügbaren Berichte im Ordner Meine Berichte abzurufen, wenn ein Benutzer auf eine Schaltfläche in einem Formular klickt:  
  
```vb  
' Button click event that retrieves a list of reports from  
' the My Reports folder and displays them in a combo box  
Private Sub listReportsButton_Click(sender As Object, e As System.EventArgs)  
   ' Create a new Web service object and set credentials  
   ' to Windows Authentication  
   Dim rs As New ReportingService2010()  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
  
   ' Return the list of items in My Reports  
   Dim items As CatalogItem() = rs.ListChildren("/Adventureworks 2008 Sample Reports", False)  
  
   Dim ci As CatalogItem  
   For Each ci In  items  
      ' If the item is a report, add it to   
      ' a combo box  
      If ci.TypeName = "Report" Then  
         catalogComboBox.Items.Add(ci.Name)  
      End If  
   Next ci  
End Sub 'listReportsButton_Click  
```  
  
```csharp  
// Button click event that retrieves a list of reports from  
// the My Reports folder and displays them in a combo box  
private void listReportsButton_Click(object sender, System.EventArgs e)  
{  
   // Create a new Web service object and set credentials  
   // to Windows Authentication  
   ReportingService2010 rs = new ReportingService2010();  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
  
   // Return the list of items in My Reports  
   CatalogItem[] items = rs.ListChildren("/Adventureworks 2008 Sample Reports", false);  
  
   foreach (CatalogItem ci in items)  
   {  
      // If the item is a report, add it to   
      // a combo box  
      if (ci.TypeName == "Report")  
         catalogComboBox.Items.Add(ci.Name);  
   }  
}  
```  
  
 Von dort aus können Sie es Benutzern ermöglichen, den Bericht im Kombinationsfeld auszuwählen und dann mithilfe eines Webbrowser-Steuerelements oder eines Bildsteuerelements eine Vorschau des Berichts im Formular anzuzeigen.  
  
## <a name="enabling-report-viewing-and-navigation-using-windows-forms"></a>Aktivieren der Anzeige von Berichten und der Berichtsnavigation mit Windows Forms  
 Es gibt zwei Methoden zum Integrieren von Berichten in Windows Forms-Anwendungen.  
  
 Sie können die SOAP-API verwenden, um Berichte mithilfe der <xref:ReportExecution2005.ReportExecutionService.Render%2A>-Methode in jedes der unterstützten Renderingformate zu rendern. Die Verwendung dieser Methode bringt im Vergleich zur Aktivierung der Anzeige von Berichten und der Berichtsnavigation durch SOAP kleine Nachteile mit sich:  
  
-   Sie können die integrierte Funktionalität der Berichtssymbolleiste nicht nutzen, die über URL-Zugriff im HTML-Viewer verfügbar ist.  
  
-   Wenn Sie in das HTML-Format rendern, müssen Sie mit der <xref:ReportExecution2005.ReportExecutionService.RenderStream%2A>-Methode alle Bilder oder Ressourcen separat als zusätzliche Datenströme rendern.  
  
-   Wenn Sie den URL-Zugriff verwenden, gibt es beim Rendern von Berichten einen leichten Leistungsvorteil im Vergleich zur SOAP-API.  
  
 Sie können die <xref:ReportExecution2005.ReportExecutionService.Render%2A>-Methode der SOAP-API jedoch verwenden, um Berichte zu rendern und sie programmgesteuert in verschiedenen Ausgabeformaten zu speichern. Dies ist ein Vorteil gegenüber dem URL-Zugriff, der eine Benutzereingabe erfordert. Wenn Sie mit der SOAP-API-<xref:ReportExecution2005.ReportExecutionService.Render%2A>-Methode einen Bericht rendern, können Sie in jedes der unterstützten Ausgabeformate rendern.  
  
 Sie können auch die frei verteilbaren Report Viewer-Steuerelemente verwenden, die im Lieferumfang von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsOrcas](../../includes/vsorcas-md.md)] enthalten sind. Durch die Report Viewer-Steuerelemente kann [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Funktionalität problemlos in benutzerdefinierte Anwendungen eingebettet werden. Die Report Viewer-Steuerelemente wurden für Entwickler konzipiert, die vorbereitete, vollständig erstellte Berichte als Bestandteil eines Anwendungsfeaturesatzes bereitstellen möchten (z.B. für eine Anwendung zur Websiteverwaltung mit Berichten, die Clickstreamanalysen für Unternehmenswebsites enthalten). Das Einbetten der Steuerelemente in eine Anwendung stellt eine optimierte Alternative zum Aufnehmen der [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Serverkomponenten in der Anwendungsbereitstellung dar. Die Steuerelemente stellen Berichtsfunktionalität bereit, jedoch ohne die in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] vorhandene zusätzliche Unterstützung für das Erstellen, Veröffentlichen, Verteilen und Übermitteln von Berichten.  
  
 Es gibt zwei Versionen der Report Viewer-Steuerelemente: eine für umfassende Windows-Clientanwendungen und eine für [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]-Anwendungen. Die Steuerelemente unterstützen sowohl den lokalen Verarbeitungsmodus als auch den Remoteverarbeitungsmodus. Beim lokalen Verarbeitungsmodus stellt die Anwendung die Berichtsdefinition und -Datasets bereit und löst die Berichtsverarbeitung aus. Beim Remoteverarbeitungsmodus finden Datenabruf und Berichtsverarbeitung auf dem Berichtsserver statt, und die Steuerung wird für die Anzeige und für die Berichtsnavigation verwendet. Mit diesem Modell können Sie anspruchsvolle Anwendungen erstellen, die vom Desktop auf das Unternehmen zugeschnitten werden können.  
  
 Report Viewer-Steuerelemente sind in der Onlinehilfe zu [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] dokumentiert. Weitere Informationen finden Sie in der Produktdokumentation zu [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen von Anwendungen mit dem Webdienst und .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Integration von Reporting Services in Anwendungen](../../reporting-services/application-integration/integrating-reporting-services-into-applications.md)   
 [Verwenden der SOAP-API in einer Webanwendung](../../reporting-services/application-integration/integrating-reporting-services-using-soap-web-application.md)  
  
  
