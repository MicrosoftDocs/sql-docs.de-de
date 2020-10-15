---
title: Skripterstellung und PowerShell mit Reporting Services | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie mehr über die Unterstützung für das Erstellen von Skripts und das Verwenden von PowerShell-Cmdlets für Reporting Services-Berichtsserver im SharePoint-Modus.
ms.date: 09/14/2015
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- scripts [Reporting Services]
- Reporting Services, scripting
- scripting [Reporting Services]
ms.assetid: 1ac2646d-ed5a-4436-b18f-2150c33f3d87
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bc45a65da0f0cda892f32bb9f2d465c9c04c2671
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91986364"
---
# <a name="scripting-and-powershell-with-reporting-services"></a>Skripterstellung und PowerShell mit Reporting Services
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] unterstützt eine Vielzahl von Entwicklungs- und Verwaltungsszenarien über Skripts, beispielsweise das Befehlszeilenprogramm RS.exe, PowerShell-Cmdlets für Berichtsserver im SharePoint-Modus und die Nutzung des [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Objektmodells in PowerShell für den einheitlichen und den SharePoint-Modus.  
  
-   Administratoren können Skripts in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] erstellen, um das Bereitstellen und Verwalten einer Berichtsserverinstallation zu automatisieren. Zudem können Administratoren [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skripts generieren und ausführen, mit denen eine Berichtsserver-Datenbank erstellt, konfiguriert und aktualisiert werden kann. Administratoren können auch die in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] verfügbaren Funktionen zum Aufzeichnen und Wiedergeben von Skripts verwenden, um routinemäßige Wartungsaufgaben zu automatisieren.  
  
-   Entwickler können benutzerdefinierte Anwendungen erstellen, die Skripts enthalten. Sie können Skripts ausführen, mit denen den Report Server-Webdienst aufgerufen wird. Fast jeder Vorgang, den Sie in verwaltetem Code schreiben können, kann auch als Skript geschrieben werden.  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] unterstützen das [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET-Skript als Skriptsprache, die vom Hilfsprogramm RS.exe, einem auf dem Berichtsserver ausgeführten Skripthost, verarbeitet werden kann.  
  
## <a name="reporting-services-sharepoint-mode-powershell-cmdlets-and-samples"></a>PowerShell-Cmdlets für Reporting Services im SharePoint-Modus und Beispiele  
 ![PowerShell-Inhalt](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell-Inhalt")  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint-Modus umfasst [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Cmdlets für die Verwaltung des Berichtsservers.  
  
-   [PowerShell cmdlets for Reporting Services SharePoint Mode](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md) enthält die folgenden Beispiele:  
  
    -   Erstellen einer Dienstanwendung und eines Proxys  
  
    -   Überprüfen und Aktualisieren einer Übermittlungserweiterung  
  
    -   Abrufen und Festlegen von Eigenschaften der Reporting Services-Anwendungsdatenbank, z. B. Timeout der Datenbank  
  
    -   Datenerweiterungen auflisten  
  
## <a name="reporting-services-object-model-and-powershell-samples"></a>Reporting Services-Objektmodell und Powershell-Beispiele  
 ![PowerShell-Inhalt](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell-Inhalt")  
  
 PowerShell-Aufruf des Kern-Objektmodells und zum größten Teil gültig für SharePoint- und einheitlichen Modus, z. B. die Migrationsarbeit, Abonnementarbeit und weitere Beispiele für Abonnementarbeit in SQL15.  
  
-   [Verwenden von PowerShell, um Reporting Services-Abonnenten zu ändern und aufzulisten sowie ein Abonnement auszuführen](../../reporting-services/subscriptions/manage-subscription-owners-and-run-subscription-powershell.md).  
  
-   [Verwenden von PowerShell zum Erstellen einer Azure-VM mit einem Berichtsserver im einheitlichen Modus](/previous-versions/azure/dn449661(v=azure.100)).  
  
-   Weitere Informationen finden Sie im Abschnitt „Zugreifen auf die WMI-Klassen mit PowerShell“ unter [Zugreifen auf den Reporting Services-WMI-Anbieter](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md).  
  

## <a name="rsexe-scripting-samples"></a>Skriptbeispiele für RS.exe  
  
-   [Reporting Services-Beispielskript für "rs.exe" zum Migrieren von Inhalten zwischen Berichtsservern](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md).  
  
-   Zusätzliche Skripts, Anwendungs- und Erweiterungsbeispiele finden Sie unter [SQL Server Reporting Services-Produktbeispiele](https://go.microsoft.com/fwlink/?LinkId=177889).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Hilfsprogramm RS.exe &#40;SSRS&#41;](../../reporting-services/tools/rs-exe-utility-ssrs.md)   
 [Skripts für Bereitstellungs- und Verwaltungsaufgaben](../../reporting-services/tools/script-deployment-and-administrative-tasks.md)   
 [Script with the rs.exe Utility and the Web Service (Skripterstellung mit dem Hilfsprogramm rs.exe und dem Webdienst)](../../reporting-services/tools/script-with-the-rs-exe-utility-and-the-web-service.md)  
  
