---
description: Berichtsserverintegration für Power BI (Configuration Manager)
title: Integration von Power BI-Berichtsserver (Configuration Manager) | Microsoft-Dokumentation
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.date: 09/17/2017
ms.openlocfilehash: 47964ebf5702542452227589e1426948825cc216
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92678871"
---
# <a name="power-bi-report-server-integration-configuration-manager"></a>Berichtsserverintegration für Power BI (Configuration Manager)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

Die Seite  **Power BI-Integration** in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Configuration Manager wird zum Registrieren des Berichtsservers mit dem gewünschten verwalteten Azure Active Directory-Mandanten verwendet, um es Benutzern des Berichtsservers zu ermöglichen, unterstützte Elemente an [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] -Dashboards anzuheften. Eine Liste der unterstützten Elemente, die angeheftet werden können, finden Sie unter [Anheften von Reporting Services-Elementen an Power BI-Dashboards](../../reporting-services/pin-reporting-services-items-to-power-bi-dashboards.md).

## <a name="requirements-for-power-bi-integration"></a><a name="bkmk_requirements"></a> Anforderungen für die Power BI-Integration

Zusätzlich zu einer aktiven Internetverbindung zum Öffnen des [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] -Diensts sind die folgenden Anforderungen zum Abschließen der Integration von [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)]erforderlich.

- **Azure Active Directory:** Ihr Unternehmen muss Azure Active Directory verwenden, das die Verzeichnis- und Identitätsverwaltung für Azure-Dienste und Webanwendungen bietet. Weitere Informationen finden Sie unter [Was ist Azure Active Directory?](/azure/active-directory/fundamentals/active-directory-whatis).

- **Verwalteter Mandant:** Das [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] -Dashboard, an das Sie Berichtselemente anheften möchten, muss Teil eines verwalteten Azure AD-Mandanten sein.  Ein verwalteter Mandant wird automatisch beim erstmaligen Abonnieren der Azure-Dienste wie Microsoft 365 und Microsoft Intune für Ihr Unternehmen erstellt.   Virale Mandanten werden derzeit nicht unterstützt.  Weitere Informationen finden Sie in den Abschnitten „Erläuterung zum Azure AD-Mandanten“ und „Gewusst wie: Abrufen eines Azure AD-Verzeichnisses“ in [Erläuterungen zum Azure AD-Verzeichnis](/previous-versions/azure/azure-services/jj573650(v=azure.100)#BKMK_WhatIsAnAzureADTenant).

- Der Benutzer, der die [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] -Integration durchführt, muss ein Mitglied des Azure AD-Mandanten, ein [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Systemadministrator und ein Systemadministrator für die ReportServer-Katalogdatenbank sein.

- Der Benutzer, der die [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] -Integration durchführt, muss den [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Configuration Manager über das zum Installieren von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]verwendete Konto oder über das Konto starten, unter dem der [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Dienst ausgeführt wird.

- Berichte, aus denen Sie Informationen anheften möchten, müssen gespeicherte Anmeldeinformationen verwenden. Dies ist keine Voraussetzung für die [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] -Integration selbst, aber für den Aktualisierungsprozess für die angehefteten Elemente.  Beim Anheften eines Berichtselements wird ein [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Abonnement zum Verwalten des Aktualisierungszeitplans der Kacheln in [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)]erstellt. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Abonnements: Erfordern gespeicherte Anmeldeinformationen. Wenn ein Bericht keine gespeicherten Anmeldeinformationen verwendet, kann ein Benutzer weiterhin Berichtselemente anheften, aber wenn das zugehörige Abonnement versucht, die Daten auf [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)]zu aktualisieren, wird eine Fehlermeldung angezeigt, die der folgenden Meldung auf der Seite **Meine Abonnements** ähnelt.

    PowerBI-Übermittlungsfehler: Dashboard: Analysebeispiel für IT-Ausgaben, Anzeige: Diagramm 2, Fehler: Die aktuelle Aktion kann nicht abgeschlossen werden. Die Datenquellen-Anmeldeinformationen des Benutzers entsprechen nicht die Anforderungen zum Ausführen dieses Berichts oder freigegebenen Datasets. Dies gilt auch für die Datenquellen-Anmeldeinformationen des Benutzers.

Weitere Informationen zum Speichern von Anmeldeinformationen finden Sie im Abschnitt „Konfigurieren von gespeicherten Anmeldeinformationen für eine berichtsspezifische Datenquelle“ unter [Speichern von Anmeldeinformationen in einer Reporting Services-Datenquelle](../../reporting-services/report-data/store-credentials-in-a-reporting-services-data-source.md).

Ein Administrator kann die  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Protokolldateien auf weitere Informationen überprüfen.  Es werden Meldungen wie die Folgenden angezeigt. Eine hervorragende Möglichkeit zum Überprüfen und Überwachen von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Protokolldateien ist die Verwendung von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Power Query für die Dateien.  Weitere Informationen und ein kurzes Video finden Sie unter [Berichtsserverdienst-Ablaufverfolgungsprotokoll](../../reporting-services/report-server/report-server-service-trace-log.md).

- Abonnement!WindowsService_1!1458!09/24/2015-00:09:27:: e FEHLER: PowerBI-Übermittlungsfehler: Dashboard: Analysebeispiel für IT-Ausgaben, Anzeige: Diagramm 2, Fehler: Die aktuelle Aktion kann nicht abgeschlossen werden. Die Datenquellen-Anmeldeinformationen des Benutzers entsprechen nicht die Anforderungen zum Ausführen dieses Berichts oder freigegebenen Datasets. Entweder sind die Datenquellen-Anmeldeinformationen des Benutzers nicht in der Berichtsserverdatenbank gespeichert oder die Datenquelle des Benutzers ist so konfiguriert, dass keine Anmeldeinformationen erforderlich sind, aber das Konto für die unbeaufsichtigte Ausführung nicht angegeben ist.

- Benachrichtigung!WindowsService_1!1458!09/24/2015-00:09:27:: e FEHLER: Fehler beim Verarbeiten von Abonnement fcdb8581-d763-4b3b-ba3e-8572360df4f9: PowerBI-Übermittlungsfehler: Dashboard: Analysebeispiel für IT-Ausgaben, Anzeige: Diagramm 2, Fehler: Die aktuelle Aktion kann nicht abgeschlossen werden. Die Datenquellen-Anmeldeinformationen des Benutzers entsprechen nicht den Anforderungen zum Ausführen dieses Berichts oder freigegebenen Datasets. Entweder sind die Datenquellen-Anmeldeinformationen des Benutzers nicht in der Berichtsserverdatenbank gespeichert oder die Datenquelle des Benutzers ist so konfiguriert, dass keine Anmeldeinformationen erforderlich sind, aber das Konto für die unbeaufsichtigte Ausführung nicht angegeben ist.

## <a name="to-integrate-and-register-the-report-server"></a><a name="bkmk_steps2integrate"></a> So integrieren und registrieren Sie den Berichtsserver

Führen Sie die folgenden Schritte des [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Configuration Managers aus. Weitere Informationen finden Sie unter [Report Server Configuration Manager (Konfigurations-Manager des Berichtsservers)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).

1. Wählen Sie die [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] -Integrationsseite aus.

2. Wählen Sie **Mit Power BI registrieren** aus.

    >[!Note]
    > Stellen Sie sicher, dass Port 443 nicht blockiert ist.

3. Im [!INCLUDE[msCoName](../../includes/msconame-md.md)] -Anmeldedialogfeld geben Sie die Anmeldeinformationen ein, die Sie zum Anmelden bei [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)]verwenden.

4. Nach Abschluss der Registrierung führt der Abschnitt **Power BI-Registrierungsdaten** die Azure-Mandanten-ID und die Umleitungs-URL(s) auf.  Die URLs werden im Rahmen der Anmeldung und der Kommunikation für das [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] -Dashboard verwendet, um mit dem registrierten Berichtsserver kommunizieren zu können.

5. Klicken Sie auf die Schaltfläche **Kopieren** des Fensters **Ergebnisse** , um die Registrierungsdetails in die Windows-Zwischenablage zu kopieren, damit Sie sie für die zukünftige Verwendung speichern können.

## <a name="unregister-with-power-bi"></a><a name="bkmk_unregister"></a> Aufheben der Registrierung mit Power BI

**Registrierung aufheben:** Das Aufheben der Registrierung für den Berichtsserver aus Azure Active Directory führt zu Folgendem:

- Der Link **Meine Einstellungen** wird nicht mehr in der Menüleiste des Webportals angezeigt.

- Berichtselemente, die bereits angeheftet wurden, sind weiterhin auf Dashboards angeheftet. Die Kacheln auf dem Dashboard werden jedoch nicht mehr aktualisiert.

- Die [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Abonnements, die die Kacheln aktualisiert haben, sind weiterhin auf dem Berichtsserver vorhanden, aber wenn sie zum konfigurierten Zeitplan ausgeführt werden, zeigen sie eine Fehlermeldung wie die Folgende an.

    **Die Übermittlungserweiterung für dieses Abonnement konnte nicht geladen werden.**

Wählen Sie auf der Seite **Power BI** in Configuration Manager die Schaltfläche **Aufheben der Registrierung mit Power BI** aus.

##  <a name="update-registration"></a><a name="bkmk_updateregistration"></a> Aktualisieren der Registrierung

Verwenden Sie die Option **Registrierung aktualisieren** , wenn die Konfiguration des Berichtsservers geändert wurde. Wenn Sie z. B. die URLs hinzufügen oder entfernen möchten, die Benutzer für die Navigation zum [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]verwenden.

- Wählen Sie in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Configuration Manager die **Webportal-URL** aus.

     Wählen Sie **Erweitert** aus.

- Wählen Sie **Hinzufügen** aus, um eine neue HTTP-Identität für das [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] hinzuzufügen, und wählen Sie dann **OK** aus.

     Das [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] -Symbol wird geändert, um die Änderung der Serverkonfiguration anzuzeigen.  ![ssrs_powebi_icon_warning](../../reporting-services/install-windows/media/ssrs-powebi-icon-warning.png "ssrs_powebi_icon_warning")

- Wählen Sie auf der Seite **Power BI-Integration** die Option **Aktualisieren der Registrierung** aus.

     Sie werden aufgefordert, sich bei Azure AD anzumelden. Die Seite wird aktualisiert und es wird die neue URL angezeigt, die in **Umleitungs-URLs** aufgeführt ist.

##  <a name="summary-of-the-power-bi-integration-and-pin-process"></a><a name="bkmk_integration_process"></a> Zusammenfassung der Power BI-Integration und des Anheftungsprozesses

In diesem Abschnitt werden die grundlegenden Schritte und die damit verbundenen Technologien zusammengefasst, die in die Integration des Berichtsservers mit [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] und das Anheften eines Berichtselements an ein Dashboard einbezogen sind.

 **Integrieren:**

1. Wenn Sie in Configuration Manager die Schaltfläche **Mit Power BI registrieren** auswählen, werden Sie aufgefordert, sich bei Azure Active Directory anzumelden.

2. Die [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] -Client-App wird mit Ihrem verwalteten Mandanten registriert.

3. Die Power BI-Clientanwendung wird in Ihrem verwalteten Mandanten in Azure Active Directory erstellt.

4. Die Registrierung umfasst Umleitungs-URLs, die verwendet werden, wenn sich Benutzer vom Berichtsserver anmelden.  Die App-ID und die URLs werden in der ReportServer-Datenbank gespeichert. Die Umleitungs-URL wird während der Authentifizierungsaufrufe in Azure verwendet, sodass der Aufruf an den Berichtsserver zurückgegeben werden kann. Wenn sich Benutzer z.B. anmelden oder Elemente an ein Dashboard anheften.

5. Die App-ID und URLs werden im Konfigurations-Manager angezeigt.

 ![ssrs_pbiflow_integration](../../reporting-services/install-windows/media/ssrs-pbiflow-integration.png "ssrs_pbiflow_integration")

 **Wenn ein Benutzer ein Berichtselement an ein Dashboard anheftet:**

1. Benutzer zeigen eine Berichtsvorschau im [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-[!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] an und wenn sie das erste Mal klicken, um ein Berichtselement aus dem [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] anzuheften.

2. Sie werden zur Azure Active Directory-Anmeldeseite umgeleitet. Sie können sich auch über die -Seite [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] **Meine Einstellungen** anmelden. Wenn sich Benutzer bei einem verwalteten Azure-Mandanten anmelden, wird eine Beziehung zwischen dem Azure-Konto und den [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Berechtigungen erstellt.  Weitere Informationen finden Sie unter [Meine Einstellungen für die Power BI-Integration &#40;Webportal&#41;](../my-settings-for-power-bi-integration-web-portal.md)integrieren.

3. Ein Benutzersicherheitstoken wird an den Berichtsserver zurückgegeben.

4. Das Benutzersicherheitstoken wird in der ReportServer-Datenbank gespeichert.

5. Eine Liste der Gruppen und Dashboards, auf die der Benutzer Zugriff hat, werden von dem [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] -Dienst abgerufen.  Der Benutzer wählt die Zielgruppe und das Zieldashboard aus und konfiguriert, wie oft die Daten auf der [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] -Kachel aktualisiert werden sollen.

6. Das Berichtselement wird an das Dashboard angeheftet.

7. Es wird ein [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Abonnement erstellt, um die geplante Aktualisierung des Berichtselements auf der Dashboardkachel zu verwalten. Das Abonnement verwendet das Sicherheitstoken, das bei der Anmeldung des Benutzers erstellt wurde.

     Das Token ist für **90 Tage** gültig. Anschließend müssen sich die Benutzer neu anmelden, um ein neues Benutzertoken zu erstellen. Wenn das Token abgelaufen ist, werden die angehefteten Kacheln weiterhin im Dashboard angezeigt, aber die Daten werden nicht mehr aktualisiert.  Die für angeheftete Elemente verwendeten [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Abonnements weisen einen Fehler auf, bis ein neues Benutzertoken erstellt wird. Weitere Informationen finden Sie unter [Meine Einstellungen für die Power BI-Integration (Webportal)](../my-settings-for-power-bi-integration-web-portal.md). um weitere Informationen zu erhalten.

Wenn ein Benutzer ein Element ein zweites Mal anheftet, werden die Schritte 1 bis 4 übersprungen und stattdessen die App-ID und die URLs aus der ReportServer-Datenbank abgerufen. Der Ablauf wird dann mit Schritt 5 fortgesetzt.

![Diagramm, das zeigt, was passiert, wenn ein Benutzer ein Berichtselement an ein Dashboard anheftet](../../reporting-services/install-windows/media/ssrs-pin-to-powerbi-flow.png)

 **Wenn ein Abonnement ausgelöst wird, um eine Dashboardkachel zu aktualisieren:**

1. Wenn das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Abonnement ausgelöst wird, wird der Bericht gerendert.

2. Das Benutzertoken wird aus der ReportServer-Datenbank abgerufen.

3. Das Zustand und die Daten des Berichtselements werden mit dem Token an den [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)]-Dienst gesendet.

4. Das Token wird zur Überprüfung an Azure AD gesendet. Wenn das Token gültig ist, werden die Daten des Berichts an die Dashboardkachel gesendet, und die Date-Eigenschaft der Kachel wird aktualisiert.

5. Wenn das Token nicht gültig ist, wird ein Fehler zurückgegeben und auf dem Berichtsserver protokolliert.  Es werden keine Statusinformationen oder sonstigen Informationen an das Dashboard gesendet.

![Diagramm, das zeigt, was passiert, wenn ein Abonnement ausgelöst wird, um eine Dashboardkachel zu aktualisieren](../../reporting-services/install-windows/media/ssrs-subscription-to-powerbi-flow.png)

   <iframe width="560" height="315" src="https://www.youtube.com/embed/QhPQObqmMPc" frameborder="0" allowfullscreen></iframe>

## <a name="considerations-and-limitations"></a>Überlegungen und Einschränkungen

* Virale und Government-Mandanten werden nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

[Meine Einstellungen für die Power BI-Integration &#40;Webportal&#41;](../my-settings-for-power-bi-integration-web-portal.md)  
[Anheften von Reporting Services-Elementen an Power BI-Dashboards](../../reporting-services/pin-reporting-services-items-to-power-bi-dashboards.md)
[Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)