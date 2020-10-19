---
title: Konfigurieren und Verwalten eines Berichtsservers (einheitlicher Modus) | Microsoft-Dokumentation
description: Hier erfahren Sie mehr zu den Ansätzen, die Sie zum Konfigurieren von Reporting Services verwenden können. Außerdem sind einige Artikel zum Konfigurieren von Komponenten, Features oder Serverfunktionen verlinkt.
ms.date: 05/15/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, components
- deploying [Reporting Services], component options
- report servers [Reporting Services], component options
- configuration options [Reporting Services]
- administering Reporting Services
- components [Reporting Services], configuring
- configuring servers [Reporting Services]
ms.assetid: cfec012b-56f1-4346-9814-247acf22351c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 3f9d31706fd0ba46c565d4c9049a981dd06fb54b
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933805"
---
# <a name="configure-and-administer-a-report-server-ssrs-native-mode"></a>Konfigurieren und Verwalten eines Berichtsservers (einheitlicher SSRS-Modus)
  In diesem Artikel werden die Vorgehensweisen zum Konfigurieren von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] zusammengefasst. Es enthält außerdem eine Liste der Themen, in denen das Konfigurieren bestimmter Komponenten, Funktionen und Serverfunktionen erläutert wird. Zum Konfigurieren von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]können Sie folgendermaßen vorgehen:  
  
-   Verwenden des Berichtsserver-Konfigurations-Managers. Viele Themen in diesem Abschnitt enthalten Informationen zum Konfigurieren bestimmter Funktionen über dieses Tool.  
  
-   Verwenden von [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] zum Anpassen von Servereigenschaften, zum Aktivieren von „Meine Berichte“ und Ablaufverfolgungsprotokollen und zum siteweiten Festlegen von Standardwerten. Weitere Informationen zur Verwaltung von Siteeinstellungen finden Sie unter [Reporting Services-Berichtsserver &#40;einheitlicher Modus&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md) . Beachten Sie, dass Sie Skripts erstellen und ausführen können, mit denen Servereigenschaften programmgesteuert festgelegt werden. Weitere Informationen finden Sie unter [Skripts für Bereitstellungs- und Verwaltungsaufgaben](../../reporting-services/tools/script-deployment-and-administrative-tasks.md) und [Berichtsserver-Systemeigenschaften](../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md).  
  
-   Verwenden Sie das Webportal zum Gewähren von Berechtigungen für den Zugriff auf den Berichtsserver. Berechtigungen werden durch Rollenzuweisungen festgelegt, die Sie für jeden Benutzer bzw. jedes Gruppenkonto definieren. Weitere Informationen finden Sie unter [Rollen und Berechtigungen &#40;Reporting Services&#41;](../../reporting-services/security/roles-and-permissions-reporting-services.md).  
  
-   Ändern Sie alternativ die Konfigurationsdateien, wenn Sie Anwendungseinstellungen ändern möchten. Weitere Informationen zu den entsprechenden Dateien und Richtlinien für deren Änderung finden Sie unter [Reporting Services Configuration Files](../../reporting-services/report-server/reporting-services-configuration-files.md).  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 [Konfigurieren von Berichtsserver-URLs &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)  
 Beschreibt die Definition von URLs, die für den Zugriff auf den Berichtsserver und das Webportal verwendet werden.  
  
 [Configure the Report Server Service Account &#40;Report Server Configuration Manager&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md) (Konfigurieren des Berichtsserver-Dienstkontos &#40;Berichtsserver-Konfigurations-Manager&#41;)  
 Gibt Empfehlungen und nennt Schritte zum Ändern von Dienstkonten und Kennwörtern.  
  
 [Erstellen einer Berichtsserver-Datenbank &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
 Beschreibt das Erstellen einer Berichtsserver-Datenbank, die für das Speichern von Server-Metadaten und -Objekten erforderlich ist.  
  
 [Konfigurieren einer Verbindung mit der Berichtsserver-Datenbank &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)  
 Beschreibt das Ändern der Verbindungszeichenfolge, die vom Berichtsserver zum Herstellen einer Verbindung mit der Berichtsserver-Datenbank verwendet wird.  
  
 [E-Mail Delivery in Reporting Services (E-Mail-Übermittlung in Reporting Services)](../install-windows/e-mail-settings-reporting-services-native-mode-configuration-manager.md)  
 Beschreibt, wie Sie für einen Berichtsserver die Verteilung von Berichten per E-Mail konfigurieren.  
  
 [Configure the Unattended Execution Account &#40;Report Server Configuration Manager&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md) (Konfigurieren des unbeaufsichtigten Ausführungskontos &#40;Berichtsserver-Konfigurations-Manager&#41;)  
 Beschreibt, wie Sie für ein Benutzerkonto die Verarbeitung von Berichten im unbeaufsichtigten Modus konfigurieren.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Reporting Services-Konfigurationsdateien](../../reporting-services/report-server/reporting-services-configuration-files.md)   
 [Berichtsserver-Konfigurations-Manager &#40;einheitlicher Modus&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Sicherheit und Schutz für Reporting Services](../../reporting-services/security/reporting-services-security-and-protection.md)   
 [Reporting Services-Berichtsserver (einheitlicher Modus)](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)  
  
  
