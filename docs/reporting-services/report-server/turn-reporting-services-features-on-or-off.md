---
title: Aktivieren und Deaktivieren von Features von Reporting Services | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie einzelne Features im einheitlichen Modus in den Reporting Services deaktivieren. Es gibt verschiedene Möglichkeiten, Features zu konfigurieren.
ms.date: 06/10/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, configuration
- security [Reporting Services], strategies
ms.assetid: b69db02a-43a7-4fdc-ad9b-438d817a7f83
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c6b42238a081adbd40f9756fb3d2fb8871e1d7cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023335"
---
# <a name="turn-reporting-services-features-on-or-off"></a>Aktivieren und Deaktivieren der Reporting Services-Funktionen
  Sie können Berichtsserver-Funktionen, die Sie nicht als Teil einer Sicherheitsstrategie verwenden, deaktivieren, um die Angriffsfläche eines Produktionsberichtsservers zu verkleinern. In den meisten Fällen sollten Sie die [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Features gleichzeitig ausführen, damit Sie alle Funktionen von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]verwenden können. Sie können jedoch je nach Bereitstellungsmodell die Funktionen deaktivieren, die Sie nicht benötigen. Beispielweise können Sie nur die Hintergrundverarbeitung aktivieren, wenn die gesamte Berichtsverarbeitung in Form von geplanten Vorgängen konfiguriert ist. Entsprechend können Sie nur den Berichtsserver-Webdienst ausführen, wenn Sie ausschließlich interaktive, bedarfsgesteuerte Berichte wünschen.  
  
 Die Prozeduren in diesem Artikel erläutern, wie Sie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Funktionen des einheitlichen Modus deaktivieren. Sie können die Funktionen auf verschiedene Arten konfigurieren, z.B. indem Sie die Datei `RsReportServer.config` direkt bearbeiten oder indem Sie das Facet **Oberflächenkonfiguration für Reporting Services** der richtlinienbasierten Verwaltung in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]bearbeiten. Verwenden Sie die Links, um auf die Prozeduren zuzugreifen, in denen das Aktivieren und Deaktivieren einer Funktion erläutert wird:  
  
-   [Report server web service (Report Server-Webdienst)](#RSWebSvc)  
  
-   [Geplante Ereignisse und Verarbeitung](#Sched)  
  
-   [Web portal (Webportal)](#WebPortal)  
  
-   [Integrierte Sicherheit von Windows für Berichtsdatenquellen](#WinIntSec)  
  
##  <a name="report-server-web-service"></a><a name="RSWebSvc"></a> Berichtsserver-Webdienst  
  
### <a name="to-turn-on-or-off-the-report-server-web-service-by-editing-configuration"></a>So aktivieren bzw. deaktivieren Sie den Berichtsserver-Webdienst, indem Sie die Konfiguration bearbeiten  
  
1.  Öffnen Sie die Datei `RsReportServer.config` in einem Text-Editor. Weitere Informationen finden Sie unter [Ändern einer Reporting Services-Konfigurationsdatei &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  
  
2.  Um den Berichtsserver-Webdienst zu aktivieren, legen Sie **IsWebServiceEnabled** auf **TRUE** fest:  
  
    ```  
    <IsWebServiceEnabled>true</IsWebServiceEnabled>  
    ```  
  
3.  Um den Berichtsserver-Webdienst zu deaktivieren, legen Sie **IsWebServiceEnabled** auf **FALSE** fest:  
  
    ```  
    <IsWebServiceEnabled>false</IsWebServiceEnabled>  
    ```  
  
4.  Speichern Sie die Änderungen, und schließen Sie dann die Datei.  
  
#### <a name="to-turn-on-or-off-the-report-server-web-service-by-using-sql-server-management-studio"></a>So aktivieren bzw. deaktivieren Sie den Berichtsserver-Webdienst mithilfe von SQL Server Management Studio  
  
1.  Öffnen Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , und stellen Sie eine Verbindung zu der [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Instanz her, die Sie konfigurieren möchten.  
  
2.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf den [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Knoten, zeigen Sie auf **Richtlinien**, und klicken Sie auf **Facets**.  
  
3.  Wählen Sie in der Liste **Facet** den Eintrag **Oberflächenkonfiguration für Reporting Services** aus.  
  
4.  Führen Sie unter **Facet-Eigenschaften** Folgendes durch:  
  
    -   Um den Report Server-Webdienst zu aktivieren, setzen Sie **WebServiceAndHTTPAccessEnabled** auf **True**.  
  
    -   Um den Report Server-Webdienst zu deaktivieren, setzen Sie **WebServiceAndHTTPAccessEnabled** auf **False**.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="scheduled-events-and-delivery"></a><a name="Sched"></a> Geplante Ereignisse und Übermittlung  
  
#### <a name="to-turn-on-or-off-scheduled-events-and-delivery-by-editing-configuration"></a>So aktivieren bzw. deaktivieren Sie geplante Ereignisse und die Übermittlung, indem Sie die Konfiguration bearbeiten  
  
1.  Öffnen Sie die Datei RSReportServer.config in einem Text-Editor. Weitere Informationen finden Sie unter [Ändern einer Reporting Services-Konfigurationsdatei &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  
  
2.  Um die geplante Berichtsverarbeitung und -übermittlung zu aktivieren, setzen Sie **IsSchedulingService**, **IsNotificationService** und **IsEventService** auf **true**:  
  
    ```  
    <IsSchedulingService>true</IsSchedulingService>  
    <IsNotificationService>true</IsNotificationService>  
    <IsEventService>true</IsEventService>  
    ```  
  
3.  Um die geplante Berichtsverarbeitung und -übermittlung zu deaktivieren, setzen Sie **IsSchedulingService**, **IsNotificationService** und **IsEventService** auf **false**:  
  
    ```  
    <IsSchedulingService>false</IsSchedulingService>  
    <IsNotificationService>false</IsNotificationService>  
    <IsEventService>false</IsEventService>  
    ```  
  
4.  Speichern Sie die Änderungen, und schließen Sie dann die Datei.  
  
> [!NOTE]  
>  Die Hintergrundverarbeitung kann nicht vollständig deaktiviert werden, da sie Datenbankverwaltungsfunktionen enthält, die für den Serverbetrieb benötigt werden.  
  
##  <a name="web-portal"></a><a name="WebPortal"></a> Webportal
  
Seit dem kumulativen Update 2 für SQL Server Reporting Services 2016 ist das Webportal immer aktiviert.
  
##  <a name="windows-integrated-security"></a><a name="WinIntSec"></a> Integrierte Sicherheit von Windows  
  
### <a name="to-turn-on-or-off-windows-integrated-security-by-using-sql-server-management-studio"></a>So aktivieren bzw. deaktivieren Sie die integrierte Windows-Sicherheit mithilfe von SQL Server Management Studio  
  
1.  Öffnen Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , und stellen Sie eine Verbindung zu der [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Instanz her, die Sie konfigurieren möchten.  
  
2.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf den [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Knoten, und klicken Sie dann auf **Eigenschaften**.  
  
3.  Klicken Sie im Dialogfeld **Servereigenschaften** unter **Seite auswählen** auf **Sicherheit**.  
  
    -   Um die integrierte Sicherheit von Windows zu aktivieren, wählen Sie die Option **Integrierte Sicherheit von Windows für Berichtsdatenquellen aktivieren**.  
  
    -   Um die integrierte Sicherheit von Windows zu deaktivieren, heben Sie die Auswahl der Option **Integrierte Sicherheit von Windows für Berichtsdatenquellen aktivieren** auf.  
  
4.  Klicken Sie auf **OK**.  
  
## <a name="see-also"></a>Weitere Informationen  
[Berichtsserver-Konfigurations-Manager (nativer Modus)](../install-windows/reporting-services-configuration-manager-native-mode.md)

 Haben Sie dazu Fragen? [Besuchen Sie das Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
  
