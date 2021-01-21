---
title: 'Tutorial: Suchen und Starten von Reporting Services-Tools | Microsoft-Dokumentation'
description: In diesem Artikel lernen Sie die Tools kennen, die verwendet werden, um einen Berichtsserver zu konfigurieren, Berichtsserverinhalte und -vorgänge zu verwalten und paginierte und mobile Reporting Services-Berichte zu erstellen und zu veröffentlichen.
ms.date: 12/09/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.custom: seodec18
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, tutorials
- Reporting Services, tools
- Reporting Services Configuration tool
- Business Intelligence Development Studio, locating and starting tool
- Model Designer [Reporting Services], locating and starting tool
- Report Designer [Reporting Services], tutorials
- tools [Reporting Services]
- tutorials [Reporting Services]
- Report Builder
ms.assetid: 51ad69d8-fe92-4662-a7cd-d235692f0c03
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 60d621e9bb833615aaed5e6f622afb9591916037
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597064"
---
# <a name="tutorial-how-to-locate-and-start-reporting-services-tools-ssrs"></a>Tutorial: So suchen und starten Sie Reporting Services-Tools (SSRS)

In diesem Tutorial werden die Tools vorgestellt, mit denen Berichtsserver konfiguriert, Berichtsserverinhalte und -vorgänge verwaltet sowie paginierte und mobile [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Berichte erstellt und veröffentlicht werden. Wenn Sie mit den Tools bereits vertraut sind, können Sie zu anderen Tutorials wechseln, in denen Sie wichtige Funktionen für die Verwendung von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]kennen lernen. Weitere Tutorials finden Sie unter [Reporting Services-Tutorials &#40;SSRS&#41;](../../reporting-services/reporting-services-tutorials-ssrs.md)kennen lernen.

## <a name="report-server-configuration-manager-native-mode"></a><a name="bkmk_configuration_manager"></a>Berichtsserver-Konfigurations-Manager (einheitlicher Modus)
Verwenden Sie den Konfigurations-Manager im einheitlichen Modus, um folgende Aufgaben auszuführen:

- Angeben des Dienstkontos.
- Erstellen oder Aktualisieren der Berichtsserver-Datenbank.
- Ändern der Verbindungseigenschaften.
- Angeben von URLs.
- Verwalten von Verschlüsselungsschlüsseln.
- Konfigurieren der unbeaufsichtigten Berichtsverarbeitung und E-Mail-Übermittlung von Berichten.

**Installation:** [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Configuration Manager wird bei der Installation des einheitlichen Modus von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] installiert. Weitere Informationen finden Sie unter [Installieren des Reporting Services-Berichtsservers im einheitlichen Modus](../../reporting-services/install-windows/install-reporting-services-native-mode-report-server.md).

### <a name="to-start-the-report-server-configuration-manager"></a>Starten des Berichtsserver-Konfigurations-Managers

1. Geben Sie im Windows-Startbildschirm **reporting** ein, und klicken Sie in den Suchergebnissen in **Apps** auf **Berichtsserver-Konfigurations-Manager**.

    ![Berichtsserver-Konfigurations-Manager beim Start](../../reporting-services/tools/media/bi-ssrs-configmanager-win8-startscreen.gif "Berichtsserver-Konfigurations-Manager beim Start")

    **Oder**

    Klicken Sie auf **Starten**, auf **Programme**, auf [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], auf **Konfigurationstools** und dann auf **Berichtsserver-Konfigurations-Manager**.

    Das Dialogfeld **Auswahl der Berichtsserver-Installationsinstanz** wird angezeigt, mit dem Sie die Berichtsserverinstanz auswählen können, die Sie konfigurieren möchten.

2. Geben Sie in **Servername** den Namen des Computers an, auf dem die Berichtsserverinstanz installiert ist. Standardmäßig ist der Name des lokalen Computers angegeben, Sie können jedoch auch den Namen einer Remoteinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eingeben.

    Falls Sie einen Remotecomputer angeben, klicken Sie auf **Suchen** , um eine Verbindung herzustellen. Der Berichtsserver muss zuvor für die Remoteverwaltung konfiguriert werden. Weitere Informationen finden Sie unter [Konfigurieren eines Berichtsservers für die Remoteverwaltung](../../reporting-services/report-server/configure-a-report-server-for-remote-administration.md).

3. Wählen Sie unter **Instanzname** die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Instanz aus, die Sie konfigurieren möchten. In der Liste werden ausschließlich Berichtsserverinstanzen von SQL Server 2008 und höher angezeigt. Frühere Versionen von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]können nicht konfiguriert werden.

4. Klicken Sie auf **Verbinden**.

5. Wenn Sie sicherstellen möchten, dass das Tool gestartet wurde, vergleichen Sie Ihre Ergebnisse mit dem folgenden Bild:

    ![Reporting Services-Konfigurationstool](../../reporting-services/tools/media/rs-ui-reportserverconfigkatmai.png "Reporting Services-Konfigurationstool")

 **Nächste Schritte:** [Konfigurieren und Verwalten eines Berichtsservers &#40;einheitlicher SSRS-Modus&#41;](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md) und [Berichtsserver-Konfigurations-Manager &#40;einheitlicher Modus&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).

## <a name="web-portal-native-mode"></a>Webportal (einheitlicher Modus)

Verwenden Sie das [Web portal (SSRS Native Mode)](../../reporting-services/web-portal-ssrs-native-mode.md) , um Berechtigungen festzulegen, Abonnements und Zeitpläne zu verwalten und mit Berichten zu arbeiten. Sie können das Webportal auch zum Anzeigen von Berichten verwenden.

**Installation:** Das Webportal wird bei der Installation des einheitlichen Modus von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] installiert: [Installieren des Reporting Services-Berichtsservers im einheitlichen Modus](../../reporting-services/install-windows/install-reporting-services-native-mode-report-server.md)

Damit Sie das Webportal öffnen können, müssen Sie über ausreichende Berechtigungen verfügen (anfangs besitzen nur Mitglieder der lokalen Administratorengruppe Berechtigungen, die den Zugriff auf Webportalfunktionen ermöglichen). Das Webportal enthält verschiedene Seiten und Optionen, die sich je nach den Rollenzuweisungen des aktuellen Benutzers unterscheiden. Für Benutzer ohne Berechtigungen wird eine leere Seite angezeigt. Für Benutzer mit Berechtigungen zum Anzeigen von Berichten werden Links angezeigt, auf die sie klicken können, um die Berichte zu öffnen. Weitere Informationen zu Berechtigungen finden Sie unter [Rollen und Berechtigungen (Reporting Services)](../../reporting-services/security/roles-and-permissions-reporting-services.md).

### <a name="to-start-the-web-portal"></a>So starten Sie das Webportal

1. Öffnen Sie Ihren Browser. Informationen zu unterstützten Browsern und Browserversionen finden Sie unter [Browserunterstützung für Reporting Services und Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md).

2. Geben Sie in die Adressleiste des Webbrowsers die URL des Webportals ein. Standardmäßig lautet die URL `https://<serverName>/reports`kennen lernen. Zur Bestätigung des Servernamens und der URL können Sie das Reporting Services-Konfigurationstool verwenden. Weitere Informationen zu URLs in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] finden Sie unter [Konfigurieren von Berichtsserver-URLs &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md).

3. Das Webportal wird im Browserfenster geöffnet. Die Startseite entspricht dem Basisordner. In Abhängigkeit von den Berechtigungen werden möglicherweise zusätzliche Ordner, Links zu Berichten sowie Ressourcendateien auf der Startseite angezeigt. Möglicherweise werden auf der Symbolleiste auch zusätzliche Schaltflächen und Befehle angezeigt.

4. Wenn Sie das Webportal auf dem lokalen Berichtsserver ausführen, finden Sie weitere Informationen unter [Konfigurieren eines Berichtsservers im einheitlichen Modus für die lokale Verwaltung &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).

## <a name="management-studio"></a><a name="bkmk_managements_studio"></a> Management Studio

Mit [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] können Berichtsserveradministratoren einen Berichtsserver neben weiteren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Komponentenservern verwalten. Weitere Informationen finden Sie im [Tutorial zu SQL Server Management Studio](../../ssms/quickstarts/ssms-connect-query-sql-server.md).

### <a name="to-start-sql-server-management-studio"></a>So starten Sie SQL Server Management Studio

1. Geben Sie im Windows-Startbildschirm **sql server** ein, und klicken Sie in den Suchergebnissen in **Apps** auf **SQL Server Management Studio**.

    ![Management Studio vom Windows-Startbildschirm](../../reporting-services/tools/media/bi-ssms-win8-startscreen.gif "Management Studio vom Windows-Startbildschirm")

    **Oder**

    Klicken Sie auf **Start**, auf **Alle Programme**, auf [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]und klicken Sie dann auf **SQL Server Management Studio**. Das Dialogfeld **Mit Server verbinden** wird angezeigt.

2. Wenn das Dialogfeld **Verbindung mit Server herstellen** nicht angezeigt wird, klicken Sie im **Objekt-Explorer** auf **Verbinden** , und wählen Sie dann **Reporting Services** aus.

3. Wählen Sie in der Liste **Servertyp** die Option **Reporting Services** aus. Wenn [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] nicht in der Liste aufgeführt wird, ist es nicht installiert.

4. Wählen Sie in der Liste **Servername** eine Berichtsserverinstanz aus. In der Liste werden lokale Instanzen angezeigt. Sie können auch den Namen einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Remoteinstanz eingeben.

5. Klicken Sie auf **Verbinden**. Sie können den Stammknoten erweitern, um Servereigenschaften festzulegen, Rollendefinitionen zu ändern oder Berichtsserverfunktionen zu deaktivieren.

## <a name="sql-server-data-tools-with-report-designer-and-report-wizard"></a><a name="bkmk_ssdt"></a> SQL Server-Datentools mit Berichts-Designer und Berichts-Assistent

Sie können zwischen zwei verschiedenen Tools zum Erstellen paginierter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Berichte auswählen: dem [Berichts-Generator](#bkmk_report_builder) und dem Berichts-Designer.

Der Berichts-Designer steht in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] – Visual Studio zur Verfügung. Die Entwurfsoberfläche im Berichts-Designer umfasst Fenster mit Registerkarten, Assistenten und Menüs, mit denen der Zugriff auf Berichts- und Modellerstellungsfunktionen möglich ist. Das Berichts-Designer-Tool steht zur Verfügung, wenn Sie ein Berichtsserverprojekt oder eine Vorlage des Berichtsserver-Assistenten in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]auswählen. Weitere Informationen finden Sie unter [Reporting Services in SQL Server Data Tools &#40;SSDT&#41;](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md).

Herunterladen von [SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md).

### <a name="to-start-report-designer"></a>So starten Sie Berichts-Designer

1. Öffnen Sie **SQL Server Data Tools**.

2. Zeigen Sie im Menü **Datei** auf **Neu**, und klicken Sie dann auf **Projekt**.

3. Klicken Sie in der Liste **Projekttypen** auf **Business Intelligence-Projekte**.

4. Klicken Sie in der Liste **Vorlagen** auf **Berichtsserverprojekt**. Mit der folgenden Grafik wird veranschaulicht, wie die Projektvorlagen im Dialogfeld angezeigt werden:

    ![Dialogfeld „Neue Projektvorlage“](../../reporting-services/tools/media/rs-ui-newrsproject.gif "Dialogfeld „Neue Projektvorlage“")

5. Geben Sie einen Namen und einen Speicherort für das Projekt ein, oder klicken Sie auf **Durchsuchen** , und wählen Sie einen Speicherort aus.

6. [!INCLUDE[clickOK](../../includes/clickok-md.md)] [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] wird mit der [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]-Startseite geöffnet. Der Projektmappen-Explorer umfasst Kategorien zum Erstellen von Berichten und Datenquellen. Mit diesen Kategorien können Sie neue Berichte und Datenquellen erstellen. Wenn Sie eine Berichtsdefinition erstellen, werden Fenster im Registerformat angezeigt. Die Fenster im Registerformat lauten Daten, Layout und Vorschau.

Informationen zum Erstellen Ihrer ersten Berichte finden Sie unter [Erstellen eines einfachen Tabellenberichts (SSRS-Tutorial)](../../reporting-services/create-a-basic-table-report-ssrs-tutorial.md). Weitere Informationen zu Abfrage-Designern, die Sie im Berichts-Designer verwenden können, finden Sie unter [Abfrageentwurfstools &#40;SSRS&#41;](../../reporting-services/report-data/query-design-tools-ssrs.md)kennen lernen.

## <a name="ssrbnoversion"></a><a name="bkmk_report_builder"></a> [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]

Der [Berichts-Generator in SQL Server](../../reporting-services/report-builder/report-builder-in-sql-server-2016.md) ist eine eigenständige Anwendung zum Erstellen paginierter Berichte außerhalb von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sie haben die Möglichkeit, alle vorhandenen Berichte anzupassen und zu aktualisieren. Dabei spielt es keine Rolle, ob sie im Berichts-Designer oder in früheren Versionen des [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]s erstellt wurden. Die Installation kann über das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Webportal oder über das Microsoft Download Center erfolgen.

Wenn Ihr paginierter Bericht vollständig ist, veröffentlichen Sie ihn auf einem Berichtsserver, oder [speichern Sie ihn im Power BI-Dienst](/power-bi/paginated-reports-save-to-power-bi-service).
[Laden Sie den Berichts-Generator](https://go.microsoft.com/fwlink/?LinkID=219138) aus dem Microsoft Download Center herunter.

### <a name="to-start-ssrbnoversion"></a>So starten Sie den [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]

1. Wählen Sie im Menü **Neu** des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Webportals die Option **Paginierter Bericht** aus.

    ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png "PBI_SSMRP_NewMenu")

2. Wenn der [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] noch nicht auf diesem Computer installiert ist, wählen Sie **[!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] abrufen**.

    oder

    [Laden Sie den Berichts-Generator](https://go.microsoft.com/fwlink/?LinkID=219138) aus dem Microsoft Download Center herunter.

3. [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] wird geöffnet, und Sie können einen paginierten Bericht erstellen oder öffnen.

## <a name="ss_mobilereptpub_long"></a><a name="bkmk_mobile_report_pub"></a> [!INCLUDE[SS_MobileReptPub_Long](../../includes/ss-mobilereptpub-long.md)]

Verwenden Sie den [Publisher für mobile Berichte von Microsoft SQL Server](../mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md), um mobile Berichte zu erstellen, die Sie im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Webportal und auf mobilen Geräten wie iPads und iPhones anzeigen können.
Die Installation kann über das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Webportal oder über das Microsoft Download Center erfolgen.

[Laden Sie den Publisher für mobile Berichte von SQL Server](https://go.microsoft.com/fwlink/?LinkID=733527) aus dem Microsoft Download Center herunter.

### <a name="to-start-ss_mobilereptpub_short"></a>So starten Sie den [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)]

1. Wählen Sie im Menü **Neu** des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Webportals die Option **Mobiler Bericht** aus.

     ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png "PBI_SSMRP_NewMenu")

2. Wenn der [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] noch nicht auf diesem Computer installiert ist, wählen Sie **[!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] abrufen**.

    oder

    [Laden Sie den Publisher für mobile Berichte von SQL Server](https://go.microsoft.com/fwlink/?LinkID=733527) aus dem Microsoft Download Center herunter.

3. [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] wird geöffnet, und Sie können einen mobilen Bericht erstellen oder öffnen.

## <a name="next-steps"></a>Nächste Schritte

[Laden Sie den Publisher für mobile Berichte von SQL Server](https://go.microsoft.com/fwlink/?LinkID=733527)  
[Laden Sie den Berichts-Generator](https://go.microsoft.com/fwlink/?LinkID=219138)  
[Herunterladen von SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md)  
[Installieren des SharePoint-Modus von Reporting Services](../../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md)  
[Reporting Services-Berichtsserver](../../reporting-services/report-server-sharepoint/reporting-services-report-server.md)  
[Query Design Tools (Abfrageentwurfstools)](../../reporting-services/report-data/query-design-tools-ssrs.md)  
[Tutorials zu Reporting Services](../../reporting-services/reporting-services-tutorials-ssrs.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)