---
description: Erstellen von Proxyklassen für den Master Data Manager-Webdienst
title: Erstellen von Proxyklassen für den Master Data Manager-Webdienst
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: 8bdab026-a0c0-41f3-9d36-f3919c23247f
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 2df08cca57a68d38c72f19921243ae95ff394c23
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350096"
---
# <a name="create-master-data-manager-web-service-proxy-classes"></a>Erstellen von Proxyklassen für den Master Data Manager-Webdienst

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Der [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Webdienst ermöglicht die programmgesteuerte Verwendung der Funktionen von [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] an jedem Computer, der auf die [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Website zugreifen kann. Vor dem Schreiben des Codes für den Zugriff auf den Webdienst sind Proxyklassen zu erstellen. Die Hauptproxyklasse, mit der Sie Webdienstvorgänge ausführen, ist die <xref:Microsoft.MasterDataServices.ServiceClient>-Klasse, welche die <xref:Microsoft.MasterDataServices.IService>-Schnittstelle implementiert.  
  
## <a name="enable-web-service-metadata-publishing"></a>Aktivieren der Veröffentlichung von Webdienst-Metadaten  
 Bevor Sie Proxyklassen generieren können, ist die Veröffentlichung der Webdienst-Metadaten zu aktivieren. Gehen Sie dazu folgendermaßen vor:  
  
1.  Öffnen Sie die Datei „Web.config“ von [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] in einem Text-Editor. Diese Datei befindet sich im Ordner "WebApplication" des [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]-Installationspfads.  
  
2.  Suchen Sie den Abschnitt **mdtaushttpbehavior** unter **\<serviceBehaviors>** . Legen Sie für das- **\<serviceMetadata>** Element **HttpGetEnabled** auf **true** fest.  
  
    > [!NOTE]  
    >  Wenn Sie Webdienste über Transport Layer Security (TLS) aktivieren möchten, die zuvor als Secure Sockets Layer (SSL) bekannt waren, legen Sie **HttpsGetEnabled** im Abschnitt **mdtaushttpbehavior** der web.config-Datei auf **true** fest. Außerdem müssen Sie **mdtaushttpbinding** ändern, sodass es auch für TLS konfiguriert ist, und den nicht-TLS-Abschnitt auskommentieren.  
  
3.  Speichern Sie die an der Datei vorgenommenen Änderungen.  
  
4.  Testen Sie die Metadaten-Veröffentlichung durch Navigieren zur Dienst-URL, beispielsweise: `https://yourserver/MDS/service/service.svc`. Ist die Metadaten-Veröffentlichung aktiviert, wird eine Seite angezeigt. Diese beginnt mit:   
    „You have created a service.“ (Sie haben einen Dienst erstellt.)  
  
## <a name="creating-proxy-classes-by-using-visual-studio"></a>Erstellen von Proxyklassen mit Visual Studio  
 Ist Visual Studio 2010 installiert, lassen sich Proxyklassen am einfachsten durch das Hinzufügen eines **Dienstverweises** zum Projekt erstellen. Die Adresse des Dienstverweises ist die URL der [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]-Webanwendung, wobei "/service/service.svc" angefügt wird. Beispiel: `https://yourserver/MDS/service/service.svc`. Weitere Informationen finden Sie unter [Vorgehensweise: Hinzufügen, Aktualisieren oder Entfernen eines Dienstverweises](/previous-versions/bb628652(v=vs.140)).  
  
## <a name="creating-proxy-classes-by-using-svcutilexe"></a>Erstellen von Proxyklassen mit "Svcutil.exe"  
 Sie müssen entweder [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] oder den [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows SDK installiert haben, damit Svcutil.exe auf dem Computer vorhanden ist. Wenn Sie [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] verwenden, müssen Sie den Befehl über die [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]-Eingabeaufforderung ausführen. Weitere Informationen finden Sie unter [ServiceModel Metadata Utility-Tool (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe) und [Generieren eines WCF-Clients aus Dienstmetadaten](/dotnet/framework/wcf/feature-details/generating-a-wcf-client-from-service-metadata).  
  
 Verwenden Sie zum Erstellen mehrerer C#-Proxyklassen mit "Svcutil.exe" einen Befehl wie folgt:  
  
```  
svcutil.exe https://<server_name:port>/<virtual_path>/Service/Service.svc   
/out:<proxy_name>.cs /messageContract /tcv:Version35   
/noconfig /ct:System.Collections.ObjectModel.Collection`1   
/namespace:*,Microsoft.MasterDataServices  
```  
  
 Hierbei gilt:  
  
-   *servername*:*port* entspricht dem Computernamen und der Portnummer des Computers, auf dem [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] gehostet wird.  
  
-   *virtual_path* ist der virtuelle Pfad von [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] in den Internetinformationsdiensten (IIS).  
  
-   *proxy_name* ist der Name für die generierte Proxydatei.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Kategorisierte Webdienstvorgänge &#40;Master Data Services&#41;](../../master-data-services/develop/categorized-web-service-operations-master-data-services.md)  
  
