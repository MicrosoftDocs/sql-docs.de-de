---
title: Berichtsserver-Webdienstendpunkte | Microsoft-Dokumentation
description: Der Berichtsserver-Webdienst stellt drei Endpunkte für die Verwaltung eines Berichtsservers sowie für das Ausführen von und die Navigation in Berichten bereit.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- management endpoints [Reporting Services]
- Web service [Reporting Services], endpoints
- endpoints [Reporting Services]
- execution endpoints [Reporting Services]
- Report Server Web service, endpoints
ms.assetid: f3f5d85f-9359-4508-bc5a-7f78a3cf7421
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1d9a0bfe8aa4a469899a558c1cbe1d3b38b337b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063745"
---
# <a name="report-server-web-service-endpoints"></a>Report Server-Webdienst-Endpunkte
  Der Report Server-Webdienst stellt mehrere Endpunkte für die Verwaltung eines Berichtsservers sowie für das Ausführen von Berichten und die Berichtsnavigation bereit.  
  
## <a name="the-management-endpoints"></a>Die Verwaltungsendpunkte  
 Drei Endpunkte sind für das Verwalten von Objekten auf einem Berichtsserver verfügbar: <xref:ReportService2005>, <xref:ReportService2006> und <xref:ReportService2010>. Der <xref:ReportService2005>-Endpunkt wird zum Verwalten von Objekten auf einem Berichtsserver verwendet, der für den einheitlichen Modus konfiguriert ist. Der <xref:ReportService2006>-Endpunkt wird zum Verwalten von Objekten auf einem Berichtsserver verwendet, der für den integrierten SharePoint-Modus konfiguriert ist. Der <xref:ReportService2010>-Endpunkt führt die Funktionen von <xref:ReportService2005> und <xref:ReportService2006> zusammen und kann Objekte auf einem Berichtsserver verwalten, die entweder für den einheitlichen oder integrierten SharePoint-Modus konfiguriert sind.  
  
> [!IMPORTANT]  
>  Wenn ein Berichtsserver für den integrierten SharePoint-Modus konfiguriert ist, geben die <xref:ReportService2005>-APIs einen **rsOperationNotSupportedSharePointMode**-Fehler zurück. Wenn ein Berichtsserver für den einheitlichen Modus konfiguriert ist, geben die <xref:ReportService2006>-APIs einen **rsOperationNotSupportedNativeMode**-Fehler zurück. Auch wenn modusspezifische APIs in <xref:ReportService2010> in einem nicht beabsichtigten Modus verwendet werden, geben die APIs die entsprechenden Fehler zurück.  
  
> [!NOTE]  
>  Der <xref:ReportService2005>-Endpunkt und der <xref:ReportService2006>-Endpunkt sind in [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] als veraltet markiert. Der <xref:ReportService2010>-Endpunkt schließt die Funktionen beider Endpunkte ein und beinhaltet zusätzliche Verwaltungsfunktionen.  
  
 Wenn der Berichtsserver für den einheitlichen Modus oder den integrierten SharePoint-Modus konfiguriert ist, kann über eine der folgenden URLs auf die WSDL für den Verwaltungsendpunkt zugegriffen werden:  
  
```  
https://<Server Name>/ReportServer/ReportService2010.asmx?wsdl  
```  
  
 Weitere Informationen finden Sie unter [Accessing the SOAP API (Zugriff auf die SOAP-API)](../../../reporting-services/report-server-web-service/accessing-the-soap-api.md).  
  
## <a name="the-execution-endpoint"></a>Der Ausführungsendpunkt  
 Der <xref:ReportExecution2005>-Endpunkt ermöglicht es den Entwicklern, die Berichtsverarbeitung sowie das Rendern von einem Berichtsserver sowohl im einheitlichen als auch im integrierten SharePoint-Modus problemlos anzupassen. Der Endpunkt enthält Klassen und Methoden aus den Vorgängerversionen des Report Server-Webdiensts. Darüber hinaus wurden dem Report Server-Webdienst viele neue Klassen und Methoden hinzugefügt, die über den Ausführungsendpunkt verfügbar gemacht werden.  
  
 Auf die WSDL-Datei für den Verwaltungsendpunkt kann über die folgende URL zugegriffen werden:  
  
```  
https://<Server Name>/ReportServer/ReportExecution2005.asmx?wsdl  
```  
  
 Wenn der Berichtsserver für den integrierten SharePoint-Modus konfiguriert ist, kann über die folgende URL auf die WSDL-Datei zugegriffen werden:  
  
```  
https://<Server Name>/<Site Name>/_vti_bin/ReportServer/ReportExecution2005.asmx?wsdl  
```  
  
 Weitere Informationen finden Sie unter [Accessing the SOAP API (Zugriff auf die SOAP-API)](../../../reporting-services/report-server-web-service/accessing-the-soap-api.md).  
  
## <a name="sharepoint-proxy-endpoints"></a>SharePoint-Proxyendpunkte  
 Wenn ein Berichtsserver für den integrierten SharePoint-Modus konfiguriert und das [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]-Add-In installiert ist, wird ein Satz von Proxyendpunkten auf dem SharePoint-Server installiert. Bei den Proxyendpunkten handelt es sich um die primäre API für das Entwickeln von Berichtslösungen, wenn ein Berichtsserver für den integrierten SharePoint-Modus konfiguriert ist. Wenn Sie für die Proxyendpunkte entwickeln, verwaltet das [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]-Add-In den Austausch der Anmeldeinformationen zwischen dem SharePoint-Server und dem Berichtsserver im Authentifizierungsmodus Vertrauenswürdiges Konto. Wenn Sie für die Berichtsserverendpunkte entwickeln, muss die aufrufende Anwendung den Austausch der Anmeldeinformationen im Authentifizierungsmodus Vertrauenswürdiges Konto behandeln. In der folgenden Tabelle sind die Endpunkte aufgeführt, die mit dem [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]-Add-In installiert werden.  
  
|Proxyendpunkt|BESCHREIBUNG|  
|--------------------|-----------------|  
|<xref:ReportService2006>|Stellt die APIs für das Verwalten eines Berichtsservers bereit, der für den integrierten SharePoint-Modus konfiguriert ist.<br /><br /> Hinweis: Die Unterstützung für diesen Endpunkt wird in [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] eingestellt.|  
|<xref:ReportService2010>|Stellt die APIs für das Verwalten eines Berichtsservers bereit, der entweder für den einheitlichen Modus oder für den integrierten SharePoint-Modus konfiguriert ist.|  
|<xref:ReportExecution2005>|Stellt die APIs für die Ausführung von Berichten und die Berichtsnavigation bereit.|  
|<xref:ReportServiceAuthentication>|Stellt die APIs zum Authentifizieren von Benutzern für einen Berichtsserver bereit, wenn die SharePoint-Webanwendung für die Formularauthentifizierung konfiguriert ist.|  
  
 Nachfolgend finden Sie Beispiel-URLs für Verweise auf die Proxyendpunkte auf einer SharePoint-Website.  
  
```  
https://<Server Name>/<Site Name>/_vti_bin/ReportServer/ReportService2010.asmx  
```  
  
```  
https://<Server Name>/<Site Name>/_vti_bin/ReportServer/ReportExecution2005.asmx  
```  
  
```  
https://<Server Name>/<Site Name>/_vti_bin/ReportServer/ReportServiceAuthentication.asmx  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Building Applications Using the Web Service and the .NET Framework (Erstellen von Anwendungen mit dem Webdienst und .NET Framework)](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)  
  
  
