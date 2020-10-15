---
title: Reporting Services-SharePoint-Dienst und -Dienstanwendungen | Microsoft-Dokumentation
description: Das Erstellen einer Dienstanwendung in SQL Server Reporting Services im SharePoint-Modus macht den Dienst verfügbar und erzeugt die Dienstanwendungsdatenbank.
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: 1d1a544f300e6e49e5355294a1e0f99515482113
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91934643"
---
# <a name="reporting-services-sharepoint-service-and-service-applications"></a>Reporting Services-SharePoint-Dienst und -Dienstanwendungen

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

  Der SharePoint-Modus von Reporting Services basiert auf der SharePoint-Dienstarchitektur und verwendet einen SharePoint-Dienst sowie mindestens eine Dienstanwendung. Beim Erstellen einer Dienstanwendung wird der Dienst verfügbar gemacht und die Datenbank der Dienstanwendung generiert. Sie können mehrere Reporting Services-Dienstanwendungen erstellen, doch für die meisten Bereitstellungsszenarien ist eine Dienstanwendung ausreichend.  

> [!NOTE]
> Die Integration von Reporting Services in SharePoint ist nach SQL Server 2016 nicht mehr möglich.
  
## <a name="creating-a-reporting-services-service-application"></a>Erstellen einer Reporting Services-Dienstanwendung

 Sie können die SharePoint-Zentraladministrations- oder PowerShell-Skripts verwenden, um die Reporting Services-Dienstanwendungen zu erstellen. Weitere Informationen zum Verwenden der SharePoint-Zentraladministration finden Sie im Abschnitt „Erstellen einer Reporting Services-Dienstanwendung“ in [Installieren des SharePoint-Modus von Reporting Services für SharePoint 2010](../install-windows/install-the-first-report-server-in-sharepoint-mode.md). Lesen Sie den PowerShell-Abschnitt weiter unten in diesem Thema, um ein Beispiel zu einem PowerShell-Skript für die Erstellung von Dienstanwendungen zu erhalten.  
  
## <a name="modify-the-associations-of-the-service-application-with-a-proxy-group"></a>Ändern der Zuordnungen der Dienstanwendung mit einer Proxygruppe

 Die Seite Neu zum Erstellen einer Dienstanwendung enthält den Abschnitt **Zuordnung der Webanwendung**. Dieser Abschnitt ermöglicht es Ihnen, die Dienstanwendung im Rahmen der Erstellung zuzuordnen. Führen Sie die folgenden Schritte aus, um die Zuordnung zu ändern und der Dienstanwendung eine Kundenkonfiguration zuzuweisen. Sie können dasselbe allgemeine Verfahren auch verwenden, um den Proxy der Standardgruppe hinzuzufügen, anstatt die Zuordnung der Dienstanwendung in eine benutzerdefinierte Gruppe zu ändern.  
  
1.  Klicken Sie in der SharePoint-Zentraladministration in der Anwendungsverwaltung auf **Zuordnungen von Dienstanwendungen konfigurieren**.  
  
2.  Ändern Sie auf der Seite Zuordnungen von Dienstanwendungen die Ansicht in **Dienstanwendungen**.  
  
3.  Suchen und klicken Sie auf den Namen der neuen Reporting Services-Dienstanwendung. Sie können auch auf den Namen der Anwendungsproxygruppe **default** klicken, um den Proxy zur Standardgruppe hinzuzufügen, anstatt die folgenden Schritte auszuführen.  
  
4.  Wählen Sie im Auswahlfeld **Folgende Gruppe von Verbindungen bearbeiten** die Option **Benutzerdefiniert**.  
  
5.  Aktivieren Sie das Kontrollkästchen für den Proxy, und klicken Sie auf **OK**.  
  
## <a name="edit-service-application-properties"></a>Bearbeiten von Eigenschaften für Dienstanwendungen

 Sie können die Eigenschaftenseite der Dienstanwendung erneut öffnen, um die Eigenschaften zu ändern.  
  
1.  Klicken Sie in der SharePoint-Zentraladministration in der Gruppe Anwendungsverwaltung auf **Dienstanwendungen verwalten**.  
  
2.  Wählen Sie die Dienstanwendung aus, indem Sie zum Auswählen der ganzen Zeile auf die Typspalte klicken. Wenn Sie auf den Namen der Anwendung klicken, wird die Seite mit Verwaltungsoptionen für den Dienst und nicht die Eigenschaftenseite der Dienstanwendung geöffnet.  
  
3.  Klicken Sie im Menüband Dienstanwendungen auf **Eigenschaften**.  
  
## <a name="create-a-reporting-services-service-application-using-powershell"></a>Erstellen einer Reporting Services-Dienstanwendung mit PowerShell

 Sie können die Dienstanwendung und den Proxy mithilfe von PowerShell erstellen. Im Beispiel unten wird davon ausgegangen, dass Sie wissen, welchen Anwendungspool Sie für die Dienstanwendung konfigurieren möchten.  
  
1.  Fügen Sie das Anwendungspoolobjekt des Anwendungspoolnamens einer Variablen hinzu, die an die neue Aktion übergeben wird.  
  
    ```  
    $appPoolName = get-spserviceapplicationpool "<application pool name>"  
    ```  
  
2.  Erstellen Sie die Dienstanwendung mit einem von Ihnen angegebenen Namen und Anwendungspool.  
  
    ```  
    New-SPRSServiceApplication -Name 'MyServiceApplication' -ApplicationPool $appPoolName -DatabaseName 'MyServiceApplicationDatabase' -DatabaseServer '<Server Name>'  
    ```  
  
3.  Rufen Sie das neue Dienstanwendungsobjekt ab, und übergeben Sie das Objekt in die Pipe des neuen Proxy-Cmdlets.  
  
    ```  
    Get-SPRSServiceApplication -name MyServiceApplication | New-SPRSServiceApplicationProxy "MyServiceApplicationProxy"  
    ```  
  
## <a name="related-tasks"></a>Zugehörige Aufgaben
  
|Aufgabe|Link|  
|----------|----------|  
|Verwalten der Einstellungen der Dienstanwendung|[Manage a Reporting Services SharePoint Service Application (Verwalten einer Reporting Services-SharePoint-Dienstanwendung)](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)|  
|Führen Sie für die Dienstanwendung (einschließlich zugehöriger Komponenten wie Verschlüsselungsschlüssel und Proxy) eine Sicherung und Wiederherstellung aus.|[Backup and Restore Reporting Services SharePoint Service Applications (Sichern und Wiederherstellen von Reporting Services-SharePoint-Dienstanwendungen)](../../reporting-services/report-server-sharepoint/backup-and-restore-reporting-services-sharepoint-service-applications.md)|  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)