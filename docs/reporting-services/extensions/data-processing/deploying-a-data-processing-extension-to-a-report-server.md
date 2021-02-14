---
title: 'Vorgehensweise: Bereitstellen einer Datenverarbeitungserweiterung für einen Berichtsserver | Microsoft-Dokumentation'
description: In diesem Artikel finden Sie heraus, wie Sie eine Datenverarbeitungserweiterung auf einem Berichtsserver bereitstellen, indem Sie lernen, welche Einträge welchen Konfigurationsdateien hinzugefügt werden müssen.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- data processing extensions [Reporting Services], deploying
- assemblies [Reporting Services], data processing extension deployments
ms.assetid: e00dface-70f8-434b-9763-8ebee18737d2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1aadbda9a8842777b395612c59d753d3afb875ec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060955"
---
# <a name="deploying-a-data-processing-extension-to-a-report-server"></a>Bereitstellen einer Datenverarbeitungserweiterung für einen Berichtsserver
  Berichtsserver verwenden Datenverarbeitungserweiterungen zum Abrufen und Verarbeiten von Daten in gerenderten Berichten. Sie sollten Ihre Assembly für Datenverarbeitungserweiterungen auf dem Berichtsserver als private Assembly bereitstellen. Sie müssen auch einen Eintrag in der Konfigurationsdatei des Berichtsservers RSReportServer.config vornehmen.  
  
## <a name="procedures"></a>Prozeduren  
  
#### <a name="to-deploy-a-data-processing-extension-assembly"></a>So stellen Sie eine Assembly für Datenverarbeitungserweiterungen bereit  
  
1.  Kopieren Sie die Assembly aus dem Bereitstellungsverzeichnis in das BIN-Verzeichnis des Berichtsservers, auf dem Sie die Datenverarbeitungserweiterung verwenden möchten. Standardmäßig befindet sich das „bin“-Verzeichnis des Berichtsservers unter dem Pfad „%Programme%\Microsoft SQL Server\MSRS10_50.\<*Instance Name*>\Reporting Services\ReportServer\bin“.  
  
    > [!NOTE]  
    >  Dieser Schritt verhindert ein Upgrade auf eine neuere Instanz von SQL Server. Weitere Informationen finden Sie unter [Upgrade and Migrate Reporting Services](../../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md).  
  
2.  Nachdem die Assemblydatei kopiert wurde, öffnen Sie die Datei RSReportServer.config. Die Datei RSReportServer.config befindet sich im Verzeichnis "ReportServer". Sie müssen einen Eintrag in der Konfigurationsdatei für die Datei Ihrer Datenverarbeitungserweiterungsassembly vornehmen. Sie können die Konfigurationsdatei mit Visual Studio oder mit einem einfachen Text-Editor wie dem Microsoft-Editor öffnen.  
  
3.  Suchen Sie das **Data** -Element in der Datei RSReportServer.config. In folgendem Verzeichnis muss ein Eintrag für Ihre neu erstellte Datenverarbeitungserweiterung erstellt werden:  
  
    ```  
    <Extensions>  
       <Data>  
          <Your extension configuration information goes here>  
       </Data>  
    </Extensions>  
    ```  
  
4.  Fügen Sie einen Eintrag für die Datenverarbeitungserweiterung hinzu. Der Eintrag sollte ein **Extension**-Element mit den Werten **Name** und **Typ** enthalten und kann folgendermaßen aussehen:  
  
    ```  
    <Extension Name="ExtensionName" Type="CompanyName.ExtensionName.MyConnectionClass, MyExtensionAssembly" />  
    ```  
  
     Der Wert für **Name** ist der eindeutige Name der Datenverarbeitungserweiterung. Der Wert für **Typ** ist eine durch Trennzeichen getrennte Liste, die einen Eintrag für den vollqualifizierten Namespace der Klasse enthält, welche die Schnittstellen <xref:Microsoft.ReportingServices.Interfaces.IExtension> und <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> implementiert, gefolgt vom Namen der Assembly (ohne die DLL-Dateierweiterung). Standardmäßig sind Datenverarbeitungserweiterungen sichtbar. Um eine Erweiterung auf Benutzeroberflächen wie dem Berichts-Manager auszublenden, fügen Sie dem **Extension** -Element das Attribut **Visible** hinzu und legen es auf **false** fest.  
  
5.  Fügen Sie eine Codegruppe für die benutzerdefinierte Assembly hinzu, die die Berechtigung **FullTrust** für Ihre Erweiterung erteilt. Hierzu fügen Sie die Codegruppe zur Datei „rssrvpolicy.config“ hinzu, die sich standardmäßig in %Programme%\Microsoft SQL Server\\<MSRS10_50.\<*Instance Name*>\Reporting Services\ReportServer befindet. Die Codegruppe kann folgendermaßen aussehen:  
  
    ```  
    <CodeGroup class="UnionCodeGroup"  
       version="1"  
       PermissionSetName="FullTrust"  
       Name="MyExtensionCodeGroup"  
       Description="Code group for my data processing extension">  
          <IMembershipCondition class="UrlMembershipCondition"  
             version="1"  
             Url="C:\Program Files\Microsoft SQL Server\MSRS10_50.<Instance Name>\Reporting Services\ReportServer\bin\MyExtensionAssembly.dll"  
           />  
    </CodeGroup>  
    ```  
  
 Die URL-Mitgliedschaft ist eine der vielen Mitgliedschaftsbedingungen, die Sie für die Datenverarbeitungserweiterung auswählen können. Weitere Informationen zur Codezugriffssicherheit in [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] finden Sie unter [Sichere Entwicklung &#40;Reporting Services&#41;](../../../reporting-services/extensions/secure-development/secure-development-reporting-services.md).  
  
## <a name="verifying-the-deployment"></a>Überprüfen der Bereitstellung  
 Sie können prüfen, ob Ihre Datenverarbeitungserweiterung erfolgreich auf dem Berichtsserver bereitgestellt wurde, indem Sie die Webdienstmethode <xref:ReportService2010.ReportingService2010.ListExtensions%2A> verwenden. Sie können auch den Berichts-Manager öffnen und prüfen, ob die Erweiterung in der Liste der verfügbaren Datenquellen enthalten ist. Weitere Informationen zu Berichts-Manager und Datenquellen finden Sie unter [Erstellen, Ändern und Löschen von freigegebenen Datenquellen (SSRS)](../../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Bereitstellen von Datenverarbeitungserweiterungen](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)   
 [Erweiterungen für Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implementieren von Datenverarbeitungserweiterungen](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Reporting Services Extension Library (Reporting Services-Erweiterungsbibliothek)](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
