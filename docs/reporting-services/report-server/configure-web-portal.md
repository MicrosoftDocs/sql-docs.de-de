---
title: Konfigurieren des Webportals | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Webportalanwendung konfigurieren, in der Sie Berichte anzeigen, Berichtsserverinhalt verwalten und den Benutzerzugriff auf einen Berichtsserver im einheitlichen Modus gewähren können.
ms.date: 05/10/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- the web portal [Reporting Services], configuring
ms.assetid: e918986c-af15-48f6-8178-256aed829c6a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: ebbce2708647e15b68ec497cfadd0af2c7c5b154
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074691"
---
# <a name="configure-the-web-portal"></a>Konfigurieren des Webportals

Das Webportal ist eine Front-End-Webanwendung, in der Sie Berichte anzeigen, Berichtsserverinhalt verwalten und den Benutzerzugriff auf einen Berichtsserver im einheitlichen Modus gewähren können. Das Webportal wird mit dem Berichtsserver-Webdienst in derselben Berichtsserverinstanz installiert und kann konfiguriert werden, wenn Sie beim Setup die Option **Install in the default native mode configuration** (Installation mit der Standardkonfiguration im einheitlichen Modus durchführen) auswählen. Sie können das Webportal auch nach der Installation konfigurieren. Dieser Artikel enthält Informationen zu den folgenden Webportal-Konfigurationsszenarios:

## <a name="prerequisites"></a>Voraussetzungen

Für das Webportal müssen folgende Voraussetzungen erfüllt sein:

- Sie müssen einen minimal konfigurierten Berichtsserver haben. Weitere Informationen finden Sie unter [Configure a Report Server (Konfigurieren eines Berichtsservers)](../../reporting-services/report-server/configure-a-report-server-reporting-services-native-mode.md).

- Der Berichtsserver muss im einheitlichen Modus ausgeführt werden. Das Webportal kann mit einem Berichtsserver, für den der integrierte SharePoint-Modus konfiguriert ist, nicht verwendet werden. In SQL Server 2012 kann ein Berichtsserver nicht von einem Modus in den anderen wechseln. Wenn Sie den von der Umgebung verwendeten Berichtsservertyp ändern möchten, müssen Sie den gewünschten Berichtsservermodus installieren und dann die Berichtselemente vom alten Berichtsserver zum neuen Berichtsserver kopieren oder verschieben. Dieser Prozess wird in der Regel als „Migration“ bezeichnet. Die für das Migrieren erforderlichen Schritte hängen vom Modus ab, zu dem Sie migrieren, und von der Version, von der Sie migrieren. Weitere Informationen finden Sie unter [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md).

- Außerdem benötigen Sie Internet Explorer 11 oder höher mit aktivierter Skripterstellung. Weitere Informationen finden Sie unter [Browserunterstützung für Reporting Services und Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md).

## <a name="configure-the-web-portal-to-use-the-default-url"></a>Konfigurieren des Webportals zur Verwendung der Standard-URL

Das Webportal ist eine Webanwendung, auf die Benutzer über einen Webbrowser zugreifen. Sie müssen auf jeden Fall die URL definieren, die verwendet wird, um die Anwendung in einem Browserfenster zu öffnen. Die URL besteht aus einem Hostnamen, einem Port und einem virtuellem Verzeichnis. Zu den Standardwerten für diese URL gehören der Wert für den Hostnamen und der Wert für den Port, die Sie für die URL des Berichtsserver-Webdiensts definiert haben, sowie der Name des virtuellen Verzeichnisses **reports** . Bei einer benannten Instanz ist das virtuelle Verzeichnis **reports_instance**, wobei **instance** dem Namen der [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Instanz entspricht.

Standardmäßig besteht die URL des Webportals aus dem eindeutigen Namen eines virtuellen Verzeichnisses sowie dem Port und dem Hostnamen, der für den Berichtsserver-Webdienst definiert ist, der in derselben Instanz ausgeführt wird. In den meisten Fällen handelt es sich bei dem Hostnamen um den Netzwerknamen des Berichtsservercomputers, aber es kann sich auch im eine IP-Adresse oder einen Hostheader handeln, der den Computer auflöst. Verwenden Sie die Seite **Webportal-URL** im [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Konfigurationstool, um das Webportal so zu konfigurieren, dass es die Standard-URL verwendet.

> [!TIP]
> Wenn Sie versuchen, auf das Webportal auf einem Remotecomputer zuzugreifen und im Browser Meldungen zu Verbindungsfehlern erhalten, sind häufig die Firewalleinstellungen dafür verantwortlich. Anweisungen finden Sie unter [Konfigurieren einer Firewall für den Zugriff auf den Berichtsserver](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).

#### <a name="to-configure-the-default-the-web-portal-url-and-virtual-directory"></a>Konfigurieren der Standard-URL für das Webportal und das virtuelle Verzeichnis

1. Starten Sie das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurationstool, und stellen Sie eine Verbindung mit der Berichtsserverinstanz her.

2. Klicken Sie im [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Konfigurationstool auf die **Webportal-URL**, um die Seite zum Konfigurieren der URL zu öffnen.

3. Geben Sie für das Webportal einen eindeutigen Namen des virtuellen Verzeichnisses ein.

4. Klicken Sie auf **Anwenden**.

5. Wenn Sie [!INCLUDE[wiprlhlong](../../includes/wiprlhlong-md.md)] oder Windows Server 2008 verwenden, sind unter Umständen zusätzliche Schritte nötig, bevor Sie das Webportal lokal verwalten können. Weitere Informationen finden Sie unter [Konfigurieren eines Berichtsservers im einheitlichen Modus für die lokale Verwaltung &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).

## <a name="configure-the-web-portal-to-use-a-specific-report-server-url"></a>Konfigurieren des Webportals für die Verwendung einer bestimmten Berichtsserver-URL

Standardmäßig stellt das Webportal eine Verbindung zu dem Berichtsserver-Webdienst her, der im selben Berichtsserverdienst ausgeführt wird. Das Webportal verwendet zum Herstellen der Verbindung die URL des Berichtsserver-Webdiensts. Wenn Sie mehrere URLs für den Berichtsserver-Webdienst definieren, verwendet das Webportal die zuletzt definierte URL. In manchen Bereitstellungen soll das Webportal jedoch möglicherweise über eine statische URL die Verbindung mit dem Webdienst herstellen. Dies kann z. B. der Fall sein, wenn Sie für einen bestimmten Port oder eine bestimmte IP-Adresse die Paketfilterung konfiguriert haben und alle Verbindungen mit dem Berichtsserver die definierten Filterregeln passieren sollen.

Wenn Sie URLs im [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Konfigurationstool konfigurieren, erkennt das Webportal automatisch neue und aktualisierte URLs für den Berichtsserver, der in derselben Instanz ausgeführt wird, und verwendet diese. Wenn Ihre Bereitstellung die Verwendung einer einzelnen, statischen URL für alle Berichtsserveranforderungen erfordert, können Sie diese URL in der Datei RSReportServer.config angeben.

#### <a name="to-configure-a-static-report-server-url"></a>So konfigurieren Sie eine statische Berichtsserver-URL

1. Öffnen Sie die Datei **RSReportServer.config** in einem Text-Editor. Standardmäßig befindet sich diese Datei unter „\Programme\Microsoft SQL Server\MSRS12.\<*instancename*>\Reporting Services\ReportServer“.  

2. Suchen Sie **ReportServerURL**.

3. Ersetzen Sie die URL durch die URL der Berichtsserverinstanz.

4. Speichern Sie Ihre Änderungen, und schließen Sie die Datei.

Weitere Informationen zum Bearbeiten von Konfigurationsdateien finden Sie unter [Ändern einer Reporting Services-Konfigurationsdatei &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md) und [RSReportServer.config-Konfigurationsdatei](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).

## <a name="customize-styles-or-application-title"></a>Anpassen von Formaten und Anwendungstitel

Sie können ein benutzerdefiniertes Markenpaket erstellen, um die für das Webportal verwendeten Farben zu ändern. Weitere Informationen finden Sie unter [Branding the web portal (Branding des Webportals)](../branding-the-web-portal.md).

#### <a name="to-modify-application-title"></a>So ändern Sie den Anwendungstitel

1. Melden Sie sich mit einem Konto an, dem auf dem Berichtsserver **Systemadministrator** -Berechtigungen zugewiesen sind.

2. Öffnen Sie Internet Explorer.

3. Geben Sie die Webportal-URL ein. Standardmäßig lautet sie „https://\<**your-server-name**>/reports“. Wenn Sie jedoch Reporting Services als Instanz mit einem Namen installiert haben, ändert sich die Standard-URL in „https://\<**your-server-name**>/reports\<**_instancename**>“.

4. Wählen Sie **Siteeinstellungen** aus.

5. Ersetzen Sie auf der Registerkarte **Allgemein** unter **Name** **SQL Server Reporting Services** durch einen anderen Namen.

6. Wählen Sie **Übernehmen**.

## <a name="next-steps"></a>Nächste Schritte

[Web portal (Webportal)](../../reporting-services/web-portal-ssrs-native-mode.md)  
[Browser Support for Reporting Services (Browserunterstützung für Reporting Services)](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)
[Configure a URL (Konfigurieren einer URL)](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)   
[Überprüfen einer Installation von Reporting Services](../../reporting-services/install-windows/verify-a-reporting-services-installation.md)   
[Aktivieren und Deaktivieren der Reporting Services-Funktionen](../../reporting-services/report-server/turn-reporting-services-features-on-or-off.md)   
[Verwalten eines Berichtsservers von Reporting Services im einheitlichen Modus](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)   
[RsReportServer.config-Konfigurationsdatei](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[Configure a Native Mode Report Server for Local Administration (Konfigurieren eines Berichtsservers im einheitlichen Modus für die lokale Verwaltung)](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md)

 Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
