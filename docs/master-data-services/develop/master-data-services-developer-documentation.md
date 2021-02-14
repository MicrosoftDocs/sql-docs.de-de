---
description: Master Data Services Developer Documentation
title: Entwicklerdokumentation
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: 067b1f69-84eb-4a13-b220-120cd63704b4
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4062f365552fbbb2076c68f5bf6d3d912b81a490
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350100"
---
# <a name="master-data-services-developer-documentation"></a>Master Data Services Developer Documentation

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Hier finden Sie Informationen zum Schreiben von Code, um anzupassen, wie Sie und Ihre Benutzer mit [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] interagieren. In diesem Artikel werden folgende Themen erläutert:  
  
-   Schreiben Sie ein Programm, das auf den [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]-Webdienst zugreift. Der [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]-Webdienst ist ein WCF-Dienst (Windows Communication Foundation), den Entwickler verwenden, um [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]-Funktionen durch Code zu steuern.  
  
-   Integrieren Sie [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Funktionen in vorhandene Anwendungen.  
  
-   Schreiben Sie Code, um wiederkehrende oder komplexe Aktionen auszuführen, die sich über die [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Benutzeroberfläche nur schwer oder gar nicht ausführen lassen.  
  
-   Erstellen Sie einen benutzerdefinierten Workflow, der als Reaktion auf eine von Ihnen angegebene Geschäftsregel ausgeführt wird. Ein benutzerdefinierter Workflow ruft von Ihnen geschriebenen Code auf, durch den jede zum Verarbeiten des Workflows erforderliche Aktion ausgeführt werden kann.  
  
## <a name="master-data-manager-web-service"></a>Master Data Manager-Webdienst  
 Der [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Webdienst ermöglicht die programmgesteuerte Verwendung der Funktionen von [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] an jedem Computer, der auf die [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Website zugreifen kann. Bevor Sie Code für den Zugriff auf den Webdienst schreiben können, müssen Sie Proxyklassen generieren, die in einem von Ihnen angegebenen Namespace enthalten sind. In dieser Dokumentation wird <xref:Microsoft.MasterDataServices> als Proxynamespace verwendet. Die Hauptproxyklasse, mit der Sie Webdienstvorgänge ausführen, ist die <xref:Microsoft.MasterDataServices.ServiceClient>-Klasse, welche die <xref:Microsoft.MasterDataServices.IService>-Schnittstelle implementiert. Rufen Sie über Ihren Code Methoden der <xref:Microsoft.MasterDataServices.ServiceClient>-Klasse auf, um auf den [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]-Webdienst zuzugreifen. Die übrigen Klassen im Namespace werden von den Webdienstvorgängen verwendet.  
  
### <a name="web-service-content"></a>Webdienstinhalt  
 [Erstellen von Proxyklassen für den Master Data Manager-Webdienst](../../master-data-services/develop/create-master-data-manager-web-service-proxy-classes.md)  
 Beschreibt die Aktivierung der Veröffentlichung von Metadaten über die [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Website sowie die Erstellung von Proxyklassen für den programmgesteuerten Zugriff auf Webdienstvorgänge.  
  
 [Kategorisierte Webdienstvorgänge &#40;Master Data Services&#41;](../../master-data-services/develop/categorized-web-service-operations-master-data-services.md)  
 Eine kategorisierte Liste der Webdienstvorgänge der <xref:Microsoft.MasterDataServices.ServiceClient>-Klasse.  
  
## <a name="custom-workflows"></a>Benutzerdefinierte Workflows  
 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] verwendet Geschäftsregeln, um grundlegende Workflowlösungen zu erstellen. Sie können Daten automatisch aktualisieren und validieren und E-Mail-Benachrichtigungen auf Basis von Bedingungen senden, die Sie angeben. Geschäftsregeln in [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] dienen zur Verwaltung der häufigsten Workflowszenarios. Erfordert Ihr Workflow eine komplexere Ereignisverarbeitung wie Genehmigungen mit mehreren Ebenen oder komplexe Entscheidungsstrukturen, können Sie [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] so konfigurieren, dass Daten an eine von Ihnen erstellte benutzerdefinierte Assembly gesendet werden. Zur Handhabung von benutzerdefinierten Workflows müssen Sie SQL Server MDS Workflow Integration Service auf dem Webanwendungs Computer konfigurieren und starten und eine Assembly erstellen, die die [masterdataservices. workflowtypeextender. iworkflowtypeextender](/previous-versions/sql/sql-server-2016/hh758785(v=sql.130)) -Schnittstelle implementiert.  
  
### <a name="custom-workflow-content"></a>Benutzerdefinierter Workflowinhalt  
 [Erstellen eines benutzerdefinierten Workflows &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-master-data-services.md)  
 Anweisungen zum Erstellen einer Workflowhandlerassembly, zum Konfigurieren und Starten des SQL Server MDS Workflow Integration Service sowie zum Erstellen einer Geschäftsregel in [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] für den Start eines benutzerdefinierten Workflows.  
  
## <a name="web-server-namespaces"></a>Webserver-Namespaces  
 Durch [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] wird eine Reihe von Assemblys auf dem Webservercomputer installiert. Diese Assemblys enthalten Namespaces, die für erweiterte Szenarien verwendet werden können, in denen das Verhalten des Webservercomputers angepasst wird. In der folgenden Tabelle werden diese Namespaces beschrieben.  
  
|Namespace|BESCHREIBUNG|  
|---------------|-----------------|  
|[Microsoft. masterdataservices. Deployment](/previous-versions/sql/sql-server-2016/ff487448(v=sql.130))|Enthält Klassen, die zum Erstellen eines Bereitstellungspakets aus einem Modell und zum Bereitstellen eines Pakets in einer [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]-Datenbank verwendet werden können.|  
|<xref:Microsoft.MasterDataServices.Services>|Enthält eine Klasse zum Empfangen und Verarbeiten von Webdienstvorgängen, die durch die [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]-Webanwendung an den Webservercomputer übergeben wurden.|  
|<xref:Microsoft.MasterDataServices.Services.DataContracts>|Enthält Klassen, mit denen definiert wird, wie Daten vom Clientcomputer über die [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Webanwendung an den Webservercomputer übergeben werden.|  
|<xref:Microsoft.MasterDataServices.Services.MessageContracts>|Enthält Klassen, mit denen definiert wird, wie Anforderungen und Antworten vom Clientcomputer über die [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Webanwendung an den Webservercomputer übergeben werden.|  
|<xref:Microsoft.MasterDataServices.Services.ServiceContracts>|Enthält die Schnittstelle, mit der die Vorgänge definiert werden, die durch den [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Webdienst aufgerufen werden können.|  
  
  
