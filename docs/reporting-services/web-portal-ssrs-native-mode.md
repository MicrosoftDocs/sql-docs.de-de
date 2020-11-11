---
title: Das Webportal eines Berichtsservers (einheitlicher Modus) | Microsoft-Dokumentation
ms.date: 01/31/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
description: Das Webportal eines Reporting Services-Berichtsservers ist eine webbasierte Umgebung für das Anzeigen von (mobilen) Berichten und KPIs sowie für das Navigieren durch Elemente in Ihrer Berichtsserverinstanz.
ms.topic: conceptual
ms.assetid: 7349e626-6ed5-4d21-b05f-cf042ad9ad70
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 301d77a972da39f4e3e046e5e983206e9a1ba5c9
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243762"
---
# <a name="the-web-portal-of-a-report-server-ssrs-native-mode"></a>Das Webportal eines Berichtsservers (einheitlicher SSRS-Modus)

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

Beim Webportal des Reporting Services-Berichtsservers handelt es sich um eine webbasierte Anwendung. Sie können in dem Portal Berichte, mobile Berichte sowie KPIs abrufen und durch die Elemente in Ihrer Berichtsserverinstanz navigieren. Sie können das Webportal auch dazu verwenden, eine einzelne Berichtsserverinstanz zu verwalten.

![Screenshot des SQL Server Reporting Services-Portals](../reporting-services/media/ssrsportal.png)

## <a name="what-is-the-web-portal"></a>Was ist das Webportal?

Sie können das Webportal dazu verwenden, die folgenden Aufgaben auszuführen:

- Anzeigen, Suchen, Drucken und Abonnieren von Berichten.
- Erstellen, Sichern und Warten der Ordnerhierarchie zum Organisieren von Elementen auf dem Server.
- Konfigurieren der rollenbasierten Sicherheit, die den Zugriff auf Elemente und Vorgänge bestimmt. Weitere Informationen finden Sie unter [Rollendefinitionen: vordefinierte Rollen](security/role-definitions-predefined-roles.md).
- Konfigurieren von Berichtsausführungseigenschaften, dem Berichtsverlauf und Berichtsparameter.
- Erstellen freigegebener Zeitpläne und Datenquellen für eine leichtere Verwaltung von Zeitplänen und Datenquellenverbindungen.
- Erstellen datengesteuerter Abonnements, die Berichte an eine lange Empfängerliste verteilen.
- Erstellen verknüpfter Berichte zur Wiederverwendung und Änderung des Zwecks eines vorhandenen Berichts auf verschiedene Weise.
- Herunterladen von weit verbreiteten Tools wie beispielsweise Berichts-Generator und Publisher für mobile Berichte.
- [Erstellen von KPIs](../reporting-services/working-with-kpis-in-reporting-services.md).
- Senden von Feedback oder Anfragen zu Features.

Mit dem Webportal können Sie die Ordner des Berichtsservers durchsuchen oder nach bestimmten Berichten suchen. Sie können einen Bericht, seine allgemeinen Eigenschaften und alte Versionen anzeigen, die in einem Berichtsverlauf erfasst sind. Je nach den Ihnen gewährten Berechtigungen können Sie unter Umständen Berichte abonnieren, die an ein E-Mail-Postfach oder einen freigegeben Ordner im Dateisystem übermittelt werden.

> [!NOTE]
> Informationen zu unterstützten Browsern und Versionen finden Sie unter [Browserunterstützung für Reporting Services und Power View](../reporting-services/browser-support-for-reporting-services-and-power-view.md).

Das Webportal wird nur für einen Berichtsserver verwendet, der im einheitlichen Modus ausgeführt wird. Er wird nicht für einen Berichtsserver unterstützt, den Sie für den integrierten Modus von SharePoint konfiguriert haben.

Einige Funktionen des Webportals sind nur in bestimmten Editionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] verfügbar. Weitere Informationen finden Sie unter [Von den SQL Server-Editionen unterstützte Reporting Services-Features](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

In einer neuen Installation verfügen nur lokale Administratoren über ausreichende Berechtigungen zum Verwenden des Inhalts und der Einstellungen. Wenn Sie anderen Benutzern Berechtigungen erteilen möchten, muss ein lokaler Administrator Rollenzuweisungen erstellen, die den Zugriff auf den Berichtsserver ermöglichen. Die Anwendungsseiten und Aufgaben, auf die ein Benutzer anschließend Zugriff erhält, sind von den Rollenzuweisungen für den Benutzer abhängig. Weitere Informationen finden Sie unter [Gewähren von Benutzerzugriff auf einen Berichtsserver](./security/grant-user-access-to-a-report-server.md).

> [!NOTE]
> Falls Sie auf dem lokalen Computer, auf dem der Server läuft, zum Webportal navigieren, wird Ihnen möglicherweise eine Meldung angezeigt, die besagt, dass Sie nicht berechtigt sind, diesen Ordner anzuzeigen. Das liegt an der Benutzerkontensteuerung (User Account Control; UAC) und daran, dass Sie den Browser nicht als Administrator ausführen. Sie können Microsoft Edge nicht als Administrator ausführen. Sie werden Internet Explorer verwenden müssen. Sie können entweder remote zum Server navigieren oder Internet Explorer als Administrator starten und zum Webportal gehen. Falls Sie das Webportal remote verwenden möchten, ist es erforderlich, dass Sie Ihrem Kontoinhalts-Manager Berechtigungen für den Ordner erteilen.  

## <a name="start-and-use-the-web-portal"></a>Starten und verwenden des Webportals

Das Webportal ist eine Webanwendung, die Sie öffnen, indem Sie die [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]-URL in die Adressleiste eines Browserfensters eingeben. Wenn Sie das [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] starten, variieren die angezeigten Seiten, Links und Optionen je nach den Berechtigungen, über die Sie auf dem Berichtsserver verfügen. Um einen Task auszuführen, müssen Sie einer Rolle zugewiesen sein, die den Task einschließt.  Ein Benutzer, dem eine Rolle mit vollen Berechtigungen zugewiesen wurde, hat Zugriff auf sämtliche Anwendungsmenüs und Seiten zum Verwalten eines Berichtsservers. Einem Benutzer, dem eine Rolle mit der Berechtigung zum Anzeigen und Ausführen von Berichten zugewiesen wurde, werden dagegen nur die Menüs und Seiten angezeigt, die diese Aktivitäten unterstützen. Jeder Benutzer kann über verschiedene Rollenzuweisungen für verschiedene Berichtsserver oder sogar für die verschiedenen Berichte und Ordner auf einem einzigen Berichtsserver verfügen.

Weitere Informationen zu den Rollen finden Sie unter [Erteilen von Berechtigungen für einen Berichtsserver im einheitlichen Modus](../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md).

### <a name="start-the-web-portal"></a>Starten des Webportals

Führen Sie folgende Schritte aus, um das Webportal über einen Webbrowser zu starten:

1. Öffnen Sie Ihren Webbrowser. Eine Liste der unterstützten Webbrowser finden Sie unter [Planen der Unterstützung für Reporting Services und Power View-Browser (Reporting Services 2014)](../reporting-services/browser-support-for-reporting-services-and-power-view.md).

2. Geben Sie in die Adressleiste des Webbrowsers die URL des Webportals ein.

    Standardmäßig lautet die URL *https://[ComputerName]/reports*.

    Der Berichtsserver ist möglicherweise für die Verwendung eines bestimmten Ports konfiguriert. Zum Beispiel, *https://[ComputerName]:80/reports* oder *https://[ComputerName]:8080/reports*.

## <a name="grouping-by-categories"></a>Gruppieren nach Kategorien

Das Webportal gruppiert Elemente in verschiedene Kategorien. Die folgenden Kategorien sind verfügbar.

- KPIs (Key Performance Indicators)
- Mobile Berichte
- Paginierte Berichte
- Power BI Desktop-Berichte
- Excel-Arbeitsmappen
- Datasets
- Projektmappen-Explorer
- Ressourcen

Sie können steuern, was angezeigt werden soll, indem Sie oben rechts **View** (Ansicht) auswählen. Wenn Sie „Show Hidden“ (Ausgeblendete anzeigen) auswählen, werden diese Elemente in einer helleren Farbe angezeigt.

![Screenshot: Dropdownmenü unter „Ansicht“, in dem die Option „Ausgeblendete Elemente anzeigen“ ausgewählt ist](../reporting-services/media/ssrswebportal-view.png)

![Screenshot der nicht verfügbaren Option „Paginierte Berichte“](../reporting-services/media/ssrswebportal-hidden.png)

### <a name="power-bi-desktop-reports-and-excel-workbooks"></a>Power BI Desktop-Berichte und Excel-Arbeitsmappen

Sie können Berechtigungen für Power BI Desktop-Berichte und Excel-Arbeitsmappen hochladen, organisieren und verwalten. Sie werden innerhalb des Webportals gruppiert.

![Screenshot der Bereiche „Power BI Desktop-Berichte“ und „Excel-Arbeitsmappen“](../reporting-services/media/ssrswebportal-view-pbi-and-excel.png)

Die Dateien werden innerhalb von Reporting Services gespeichert, ähnlich wie andere Ressourcendateien. Wenn Sie eines dieser Elemente auswählen, wird es lokal auf Ihrem Desktop heruntergeladen. Sie können Ihre Änderungen speichern, indem Sie sie erneut auf den Berichtsserver hochladen.

## <a name="search-for-items"></a>Suche nach Elementen

Wenn Sie einen Suchbegriff eingeben, werden Ihnen alle Elemente angezeigt, auf die Sie Zugriff haben. Die Ergebnisse werden in KPIs, Berichte, Datasets und andere Elemente kategorisiert. Sie können dann mit den Ergebnissen interagieren und diese zu Ihren Favoriten hinzufügen.

![Screenshot: SQL Server Reporting Services-Portal, auf dem das Textfeld „Suche“ hervorgehoben ist](../reporting-services/media/ssrswebportal-search.png)

## <a name="web-portal-tasks"></a>Webportal-Aufgaben

[Branding the web portal (Branding des Webportals)](../reporting-services/branding-the-web-portal.md)

[Arbeiten mit KPIs](../reporting-services/working-with-kpis-in-reporting-services.md)

[Working with shared datasets (Arbeiten mit freigegebenen Datasets)](../reporting-services/work-with-shared-datasets-web-portal.md)

## <a name="see-also"></a>Weitere Informationen

[Erstellen und Veröffentlichen von mobilen Berichten mit dem Publisher für mobile Berichte von SQL Server](../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md)  
[Konfigurieren einer URL (Berichtsserver-Konfigurations-Manager)](../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)  
[Reporting Services-Tools](../reporting-services/tools/reporting-services-tools.md)  
[Browserunterstützung für Reporting Services und Power View](../reporting-services/browser-support-for-reporting-services-and-power-view.md)  
[Von den SQL Server-Editionen unterstützte Reporting Services-Features](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md)  

Haben Sie dazu Fragen? [Besuchen Sie das Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)