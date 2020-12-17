---
title: Verwenden der SOAP-API in Webanwendungen
description: Sie können auf die Funktionen des Berichtsservers über die Reporting Services-SOAP-API zugreifen, auf die Zugriff besteht, um Berichtsfeatures für Unternehmen bieten zu können.
ms.date: 06/26/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.custom: seo-lt-2019
ms.topic: reference
helpviewer_keywords:
- SOAP [Reporting Services], Web applications
- impersonation [Reporting Services]
- user impersonation [Reporting Services]
- report servers [Reporting Services], SOAP
- Web applications [Reporting Services]
ms.assetid: e8ca4455-0dc3-4741-8872-3636114938ad
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 78ebc356c31dcd32c650d0b04e78c20939e1cabb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466631"
---
# <a name="integrating-reporting-services-using-soap---web-application"></a>Integrieren von Reporting Services mithilfe von SOAP: Webanwendung
  Über die Reporting Services-SOAP-API können Sie auf alle Funktionen des Berichtsservers zugreifen. Da es sich um einen Webdienst handelt, kann problemlos auf die SOAP-API zugegriffen werden, um Funktionen zur Unternehmensberichterstellung für Ihre benutzerdefinierten Geschäftsanwendungen bereitzustellen. Wenn Sie über eine Webanwendung auf den Report Server-Webdienst zugreifen, gehen Sie ganz ähnlich vor, als würden Sie über eine [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Anwendung auf die SOAP-API zugreifen. Mit dem [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] können Sie eine Proxyklasse generieren, die die Eigenschaften und Methoden des Berichtsserver-Webdiensts verfügbar macht und es Ihnen ermöglicht, für die Erstellung von Geschäftsanwendungen auf Basis der [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Technologie eine vertraute Infrastruktur und vertraute Tools zu verwenden.  
  
 Von einer Webanwendung aus kann auf die [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Funktionen für die Berichterstellung ebenso leicht wie von einer Windows-Anwendung aus zugegriffen werden. Wenn Sie mit einer Webanwendung arbeiten, können Sie unter anderem Elemente zur Berichtsserver-Datenbank hinzufügen oder daraus entfernen, Einstellungen für die Elementsicherheit festlegen, Elemente in der Berichtsserver-Datenbank ändern und die Zeitplanung und Übermittlung verwalten.  
  
## <a name="enabling-impersonation"></a>Aktivieren des Identitätswechsels  
 Der erste Schritt beim Konfigurieren der Webanwendung besteht darin, den Identitätswechsel vom Webdienstclient aus zu aktivieren. Mit Identitätswechsel können [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]-Anwendungen mit der Identität des Clients ausgeführt werden, in dessen Namen sie verwendet werden. [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] nutzt [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internetinformationsdienste (Internet Information Services, IIS), um den Benutzer zu authentifizieren und ein authentifiziertes Token an die [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]-Anwendung zu übergeben oder ein nicht authentifiziertes Token zu übergeben, falls der Benutzer nicht authentifiziert werden kann. In jedem dieser Fälle nimmt die [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]-Anwendung die Identität des jeweils empfangenen Tokens an, wenn der Identitätswechsel aktiviert ist. Sie können den Identitätswechsel auf dem Client aktivieren, indem Sie die Datei Web.config der Clientanwendung folgendermaßen ändern:  
  
```asp.net  
<!-- Web.config file. -->  
<identity impersonate="true"/>  
```  
  
> [!NOTE]  
>  Der Identitätswechsel ist standardmäßig deaktiviert.  
  
 Weitere Informationen zum [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]-Identitätswechsel finden Sie in der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]-SDK-Dokumentation.  
  
## <a name="managing-the-report-server-using-soap-api"></a>Verwalten des Berichtsservers mit der SOAP-API  

::: moniker range="=sql-server-2016"

 Sie können die Webanwendung auch verwenden, um einen Berichtsserver und seinen Inhalt zu verwalten. Der mit [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] gelieferte Berichts-Manager ist ein Beispiel für eine Webanwendung, die komplett mit [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] und der Reporting Services-SOAP-API erstellt wurde. Sie können den benutzerdefinierten Webanwendungen die Berichtsverwaltungsfunktionalität des Berichts-Managers hinzufügen. Sie sollten beispielsweise eine Liste der verfügbaren Berichte in der Berichtsserver-Datenbank zurückgeben und diese Berichte in einem [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]-**Listbox**-Steuerelement anzeigen, in dem die Benutzer eine Auswahl treffen können. Der folgende Code stellt eine Verbindung zur Berichtsserver-Datenbank her und gibt eine Liste der Elemente in der Berichtsserver-Datenbank zurück. Die verfügbaren Berichte werden dann zu einem ListBox-Steuerelement hinzugefügt, das für jeden Bericht eine Pfadangabe anzeigt.  

::: moniker-end

::: moniker range=">=sql-server-2017"

 Sie können die Webanwendung auch verwenden, um einen Berichtsserver und seinen Inhalt zu verwalten. Das in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] enthaltene Webportal ist ein Beispiel für eine Webanwendung, die den Großteil der Aufgaben verwaltet, die Sie typischerweise mit Reporting Services durchführen würden. Sie können den benutzerdefinierten Webanwendungen die Berichtsverwaltungsfunktionalität des Webportals hinzufügen. Sie sollten beispielsweise eine Liste der verfügbaren Berichte in der Berichtsserver-Datenbank zurückgeben und diese Berichte in einem [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]-**Listbox**-Steuerelement anzeigen, in dem die Benutzer eine Auswahl treffen können. Der folgende Code stellt eine Verbindung zur Berichtsserver-Datenbank her und gibt eine Liste der Elemente in der Berichtsserver-Datenbank zurück. Die verfügbaren Berichte werden dann zu einem ListBox-Steuerelement hinzugefügt, das für jeden Bericht eine Pfadangabe anzeigt.  

::: moniker-end
  
```vb  
Private Sub Page_Load(sender As Object, e As System.EventArgs)  
   ' Create a Web service proxy object and set credentials  
   Dim rs As New ReportingService2005()  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
  
   ' Return a list of catalog items in the report server database  
   Dim items As CatalogItem() = rs.ListChildren("/", True)  
  
   ' For each report, display the path of the report in a Listbox  
   Dim ci As CatalogItem  
   For Each ci In  items  
      If ci.Type = ItemTypeEnum.Report Then  
         catalogListBox.Items.Add(ci.Path)  
      End If  
   Next ci  
End Sub ' Page_Load   
```  
  
```csharp  
private void Page_Load(object sender, System.EventArgs e)  
{  
   // Create a Web service proxy object and set credentials  
   ReportingService2005 rs = new ReportingService2005();  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
  
   // Return a list of catalog items in the report server database  
   CatalogItem[] items = rs.ListChildren("/", true);  
  
   // For each report, display the path of the report in a Listbox  
   foreach(CatalogItem ci in items)  
   {  
      if (ci.Type == ItemTypeEnum.Report)  
         catalogListBox.Items.Add(ci.Path);  
   }  
}  
```  
  
## <a name="see-also"></a>Weitere Informationen  

 [Erstellen von Anwendungen mit dem Webdienst und .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Integration von Reporting Services in Anwendungen](../../reporting-services/application-integration/integrating-reporting-services-into-applications.md)   
 [Verwenden der SOAP-API in einer Windows-Anwendung](../../reporting-services/application-integration/integrating-reporting-services-using-soap-windows-application.md)  
  
  
