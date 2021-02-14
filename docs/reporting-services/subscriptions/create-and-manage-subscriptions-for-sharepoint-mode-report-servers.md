---
title: Erstellen und Verwalten von Abonnements für Berichtsserver im SharePoint-Modus | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Reporting Services-Abonnement erstellen, um Berichte von einer SharePoint-Web-App zu übermitteln, die in einem im SharePoint-Modus ausgeführten Berichtsserver integriert ist.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: subscriptions
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [Reporting Services], creating
- subscriptions [Reporting Services], deleting
- subscriptions [Reporting Services], managing
ms.assetid: 44be7ee2-33ce-46e4-9d1a-a20aaf43a227
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bf68742926ef0f26c5a12a5d001ea9b14f9b199c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080092"
---
# <a name="create-and-manage-subscriptions-for-sharepoint-mode-report-servers"></a>Erstellen und Verwalten von Abonnements für Berichtsserver im SharePoint-Modus
  Sie können [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Abonnements erstellen, um Berichte von einer SharePoint-Webanwendung zu übermitteln, die in einem im SharePoint-Modus ausgeführten Berichtsserver integriert ist. Abonnements können Berichte an eine Dokumentbibliothek, einen Dateiordner oder als E-Mail übermitteln. Dieses Thema fasst die Anforderungen und Schritte zum Erstellen eines [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Abonnements zusammen.  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]**<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint-Modus &#124; SharePoint 2010 und SharePoint 2013|  
  
 Beim Erstellen eines Abonnements gibt es drei Möglichkeiten zum Angeben der Übermittlung:  
  
-   **Dokumentbibliothek:** Sie können ein Abonnement erstellen, bei dem ein Dokument, das auf dem ursprünglichen Bericht basiert, an eine Bibliothek innerhalb derselben SharePoint-Website wie der des ursprünglichen Berichts übermittelt wird. Sie können das Dokument nicht an eine Bibliothek auf einem anderen Server oder einer anderen Website in der gleichen Websiteauflistung übermitteln. Wenn Sie das Dokument übermitteln möchten, müssen Sie für die Bibliothek, an die der Bericht übermittelt wird, über die Berechtigung zum Hinzufügen von Elementen verfügen.  
  
-   **Dateiordner:** Sie können ein Dokument, das auf dem ursprünglichen Bericht basiert, an einen freigegebenen Ordner im Dateisystem übermitteln. Sie müssen einen vorhandenen Ordner auswählen, auf den über eine Netzwerkverbindung zugegriffen werden kann.  
  
-   **E-Mail:** Wenn der Berichtsserver für die Verwendung der E-Mail-Übermittlungserweiterung für den Berichtsserver konfiguriert ist, können Sie ein Abonnement erstellen, von dem ein Bericht oder eine exportierte Berichtsdatei (die in einem Ausgabeformat gespeichert ist) an Ihren Posteingang gesendet wird. Wenn Sie nur die Benachrichtigung, aber keinen Bericht oder keine Berichts-URL erhalten möchten, deaktivieren Sie die Kontrollkästchen **Einen Link zum Bericht einschließen** und **Bericht in Nachricht anzeigen** .  
  
 **In diesem Thema:**  
  
-   [Allgemeine Anforderungen für Abonnements](#bkmk_subscription_requirements)  
  
-   [So erstellen Sie ein Abonnement für die Übermittlung eines Berichts an eine SharePoint-Bibliothek](#bkmk_tosharepoint_library)  
  
-   [So erstellen Sie ein Abonnement für die Übermittlung eines Berichts an eine SharePoint-Bibliothek](#bkmk_tosharepoint_library)  
  
-   [So erstellen Sie ein Abonnement für die Berichtsserver-E-Mail-Übermittlung](#bkmk_subscription_for_email)  
  
-   [So zeigen Sie ein Abonnement an oder ändern es](#bkmk_to_modify_subscription)  
  
-   [So löschen Sie ein Abonnement](#bkmk_to_delete_subscription)  
  
##  <a name="general-requirements-for-subscriptions"></a><a name="bkmk_subscription_requirements"></a> Allgemeine Anforderungen für Abonnements  
 Wenn Sie ein Abonnement erstellen möchten, müssen für den Bericht gespeicherte Anmeldeinformationen verwendet werden, und Sie müssen über die Berechtigung zum Anzeigen des Berichts und zum Erstellen von Warnungen verfügen.  
  
 Wenn Sie ein Abonnement erstellen, können Sie ein Ausgabedateiformat auswählen. Nicht alle Berichte funktionieren in jedem Format optimal. Bevor Sie ein Format in einem Abonnement auswählen, sollten Sie den Bericht öffnen und ihn in verschiedene Formate exportieren, um zu überprüfen, ob er wie erwartet angezeigt wird.  
  
 Benutzer benötigen die Listenberechtigung **Elemente bearbeiten** in SharePoint, wenn sie in der Lage sein sollen, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Abonnements zu erstellen. Weitere Informationen finden Sie unter [Referenz zu SharePoint Website- und Listenberechtigungen für Berichtsserverelemente](../../reporting-services/security/sharepoint-site-and-list-permission-reference-for-report-server-items.md).  
  
> [!IMPORTANT]  
>  Bei einem Abonnement, bei dem ein Bericht an eine Bibliothek oder einen freigegebenen Ordner übermittelt wird, wird eine neue, statische Datei erstellt, die auf dem ursprünglichen Bericht basiert. Es handelt sich jedoch nicht um eine echte Berichtsdefinition, die im Berichts-Viewer-Webpart ausgeführt wird. Wenn der ursprüngliche Bericht über interaktive Funktionen (z. B. Drillthroughlinks) oder dynamischen Inhalt verfügt, sind diese Funktionen in der statischen Datei, die an den Zielspeicherort übermittelt wird, nicht verfügbar. Wenn Sie eine "Webseite" auswählen, können Sie begrenzt Interaktivität beibehalten. Da das Dokument jedoch keine RDL-Datei darstellt, die im Berichts-Viewer ausgeführt wird, werden durch das Durchklicken eines Berichts neue Seiten in der Browsersitzung erstellt, durch die Sie einen Bildlauf durchführen müssen, um zur Website zurückzukehren.  
  
 Sie können die Dateinamenerweiterung eines exportierten Berichts nicht in "RDL" umbenennen, damit er im Berichts-Viewer-Webpart ausgeführt wird. Wenn Sie ein Abonnement erstellen möchten, in dem ein tatsächlicher Bericht bereitgestellt wird, verwenden Sie die Berichtsserver-E-Mail-Übermittlungserweiterung, und legen Sie Optionen fest, einen Link zu dem Bericht einzuschließen.  
  
 Die Versionseinstellungen für die Bibliothek, die das übermittelte Dokument enthält, bestimmen, ob mit jeder Übermittlung eine neue Version des Dokuments erstellt wird. In der Standardeinstellung sind die Versionseinstellungen für jede Bibliothek aktiviert. Sofern Sie nicht explizit **Keine Versionskontrolle** auswählen, wird bei der Übermittlung eine neue Hauptversion des Dokuments erstellt. Es werden nur Hauptversionen des Dokuments erstellt. Nebenversionen werden bei der Abonnementübermittlung nie erstellt, selbst dann nicht, wenn Sie eine Versionsverwaltungsoption auswählen, die Nebenversionen zulässt. Wenn Sie die Anzahl der beibehaltenen Hauptversionen begrenzen, werden ältere Übermittlungen beim Erreichen der Obergrenze durch neuere ersetzt.  
  
 Die Ausgabeformate, die Sie für ein Abonnement auswählen, basieren auf Renderingerweiterungen, die auf dem Berichtsserver installiert sind. Sie können nur Ausgabeformate auswählen, die von den Renderingerweiterungen auf dem Berichtsserver unterstützt werden.  
  
###  <a name="to-create-a-subscription-to-deliver-a-report-to-a-sharepoint-library"></a><a name="bkmk_tosharepoint_library"></a> So erstellen Sie ein Abonnement für die Übermittlung eines Berichts an eine SharePoint-Bibliothek  
  
1.  Wechseln Sie zu einer SharePoint-Bibliothek, die den Bericht enthält.  
  
2.  Klicken Sie neben dem Bericht auf den Pfeil nach unten, und wählen Sie **Abonnements verwalten** aus.  
  
3.  Klicken Sie auf **Abonnement hinzufügen**.  
  
4.  Wählen Sie unter **Übermittlungserweiterung** die Option **SharePoint-Dokumentbibliothek** aus.  
  
5.  Wählen Sie unter **Dokumentbibliothek** eine Bibliothek innerhalb derselben Website aus.  
  
6.  Geben Sie unter **Dateioptionen** den Dateinamen und den Titel für das Dokument an, das vom Abonnement erstellt wird.  
  
7.  Wählen Sie unter **Ausgabeformat** das Anwendungsformat aus.  
  
     Webarchiv (MHTML) ist die Standardeinstellung, da hierdurch eine eigenständige HTML-Datei erzeugt wird. Es werden jedoch keine interaktiven Berichtsfunktionen beibehalten, die möglicherweise im ursprünglichen Bericht enthalten sind.  
  
8.  Geben Sie unter **Optionen für das Überschreiben** eine Option an, mit der bestimmt wird, ob eine Datei bei nachfolgenden Übermittlungen überschrieben werden soll. Wenn Sie vorherige Übermittlungen beibehalten möchten, können Sie **Datei mit eindeutigem Namen erstellen** auswählen. Eine Zahl wird an neue Dateien angefügt, um einen eindeutigen Dateinamen zu erstellen.  
  
9. Geben Sie unter **Übermittlungsereignis** einen Zeitplan oder ein Ereignis an, mit dem das Abonnement ausgeführt wird. Sie können einen benutzerdefinierten Zeitplan erstellen, einen freigegebenen Zeitplan auswählen (sofern ein solcher verfügbar ist) oder das Abonnement ausführen, sobald die Daten für einen Bericht, der mit Momentaufnahmedaten ausgeführt wird, aktualisiert werden. Weitere Informationen zu Zeitplänen und Datenverarbeitung finden Sie unter [Festlegen von Verarbeitungsoptionen (Reporting Services im integrierten SharePoint-Modus)](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md).  
  
10. Geben Sie unter **Parameter** die Werte an, die Sie beim Verarbeiten des Abonnements für den Bericht verwenden möchten, wenn Sie ein Abonnement für einen parametrisierten Bericht erstellen möchten. Der Parameter-Abschnitt ist auf dieser Seite nicht sichtbar, wenn der Bericht, den Sie auswählen, keine Parameter enthält. Weitere Informationen zu Parametern finden Sie unter [Festlegen von Parametern für einen veröffentlichten Bericht &#40;Reporting Services im integrierten SharePoint-Modus&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).  
  
###  <a name="to-create-a-subscription-for-shared-folder-delivery"></a><a name="bkmk_subscription_for_sharedfolder"></a> So erstellen Sie ein Abonnement für die Übermittlung an freigegebene Ordner  
  
1.  Wechseln Sie zu einer SharePoint-Bibliothek, die den Bericht enthält.  
  
2.  Klicken Sie neben dem Bericht auf den Pfeil nach unten, und wählen Sie **Abonnements verwalten** aus.  
  
3.  Klicken Sie auf **Abonnement hinzufügen**.  
  
4.  Wählen Sie unter **Übermittlungserweiterung** die Option **Windows-Dateifreigabe** aus.  
  
5.  Geben Sie unter **Dateiname** den Namen der Datei ein, die im freigegebenen Ordner erstellt wird.  
  
6.  Geben Sie unter **Pfad** einen Ordnerpfad im UNC-Format (Uniform Naming Convention) ein, der den Netzwerknamen des Computers enthält. Der Ordnerpfad darf keine nachgestellten umgekehrten Schrägstriche enthalten. Ein Beispielpfad könnte `\\ComputerName01\Public\MyReports`lauten, wobei Public und MyReports freigegebene Ordner sind.  
  
7.  Wählen Sie unter **Renderformat** das Anwendungsformat für den Bericht aus.  
  
8.  Wählen Sie unter **Schreibmodus** zwischen **keiner** Option, der Option zum automatischen **Inkrementieren oder der Option zum** **Überschreiben** aus. Mit diesen Optionen wird bestimmt, ob nachfolgende Übermittlungen eine Datei überschreiben. Wenn Sie vorherige Übermittlungen beibehalten möchten, können Sie die **Option zum automatischen Inkrementieren** auswählen. Eine Zahl wird an neue Dateien angefügt, um einen eindeutigen Dateinamen zu erstellen. Wenn Sie **keine** Option auswählen, wird die Übermittlung nicht ausgeführt, falls bereits eine Datei mit demselben Namen am Zielspeicherort vorhanden ist.  
  
9. Wählen Sie unter **Dateierweiterung** die Option **Wahr** aus, um eine Dateinamenerweiterung hinzuzufügen, die dem Anwendungsdateiformat entspricht, oder wählen Sie Falsch, um die Datei ohne Erweiterung zu erstellen.  
  
10. Geben Sie unter **Benutzername** und **Kennwort** Anmeldeinformationen ein, die über Schreibberechtigungen für den freigegebenen Ordner verfügen.  
  
11. Geben Sie unter **Übermittlungsereignis** einen Zeitplan oder ein Ereignis an, mit dem das Abonnement ausgeführt wird. Sie können einen benutzerdefinierten Zeitplan erstellen, einen freigegebenen Zeitplan auswählen (sofern ein solcher verfügbar ist) oder das Abonnement ausführen, sobald die Daten für einen Bericht, der mit Momentaufnahmedaten ausgeführt wird, aktualisiert werden. Weitere Informationen zu Zeitplänen und Datenverarbeitung finden Sie unter [Festlegen von Verarbeitungsoptionen (Reporting Services im integrierten SharePoint-Modus)](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md).  
  
12. Geben Sie unter **Parameter** die Werte an, die Sie beim Verarbeiten des Abonnements für den Bericht verwenden möchten, wenn Sie ein Abonnement für einen parametrisierten Bericht erstellen möchten. Weitere Informationen zu Parametern finden Sie unter [Festlegen von Parametern für einen veröffentlichten Bericht &#40;Reporting Services im integrierten SharePoint-Modus&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).  
  
###  <a name="to-create-a-subscription-for-report-server-e-mail-delivery"></a><a name="bkmk_subscription_for_email"></a> So erstellen Sie ein Abonnement für die Berichtsserver-E-Mail-Übermittlung  
  
1.  Wechseln Sie zu einer SharePoint-Bibliothek, die den Bericht enthält.  
  
2.  Klicken Sie neben dem Bericht auf den Pfeil nach unten, und wählen Sie **Abonnements verwalten** aus.  
  
3.  Klicken Sie auf **Abonnement hinzufügen**.  
  
4.  Wählen Sie unter **Übermittlungserweiterung** die Option **E-Mail** aus.  
  
5.  Geben Sie unter **Übermittlungsoptionen** eine E-Mail-Adresse an, an die der Bericht gesendet werden soll.  
  
6.  Sie können auch die Betreffzeile ändern. Für die Betreffzeile werden integrierte Parameter verwendet, die den Berichtsnamen und die Uhrzeit der Verarbeitung des Berichts aufzeichnen. Dies sind die einzigen verwendbaren integrierten Parameter. Bei den Parametern handelt es sich um Platzhalter, mit denen der in der Betreffzeile angezeigte Text angepasst wird. Sie können diese aber auch durch statischen Text ersetzen.  
  
7.  Wählen Sie **Einen Link zum Bericht einschließen** aus, wenn Sie eine Berichts-URL in den Text der Nachricht einbetten möchten.  
  
8.  Geben Sie unter **Berichtsinhalt** an, ob Sie den tatsächlichen Bericht in den Text der Nachricht einbetten möchten.  
  
     Durch das Renderingformat und den Browser wird festgelegt, ob der Bericht eingebettet oder angefügt wird. Falls Ihr Browser HTML 4.0 und MHTML unterstützt und Sie das Webarchiv-Renderingformat auswählen, wird der Bericht in die Nachricht eingebettet. Bei allen anderen Renderingformaten (CSV, PDF usw.) werden Berichte als Anlagen übermittelt. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] führt keine Überprüfung der Größe der Anlage oder Nachricht vor dem Senden des Berichts durch. Überschreitet die Anlage oder Nachricht den von Ihrem Mailserver zugelassenen Höchstwert, wird der Bericht nicht übermittelt. Wählen Sie für große Berichte eine der anderen Übermittlungsoptionen (z. B. URL oder Benachrichtigung) aus.  
  
9. Geben Sie unter **Übermittlungsereignis** einen Zeitplan oder ein Ereignis an, mit dem das Abonnement ausgeführt wird. Sie können einen benutzerdefinierten Zeitplan erstellen, einen freigegebenen Zeitplan auswählen (sofern ein solcher verfügbar ist) oder das Abonnement ausführen, sobald die Daten für einen Bericht, der mit Momentaufnahmedaten ausgeführt wird, aktualisiert werden. Weitere Informationen zu Zeitplänen und Datenverarbeitung finden Sie unter [Festlegen von Verarbeitungsoptionen (Reporting Services im integrierten SharePoint-Modus)](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md).  
  
10. Geben Sie unter **Parameter** die Werte an, die Sie beim Verarbeiten des Abonnements für den Bericht verwenden möchten, wenn Sie ein Abonnement für einen parametrisierten Bericht erstellen möchten. Weitere Informationen zu Parametern finden Sie unter [Festlegen von Parametern für einen veröffentlichten Bericht &#40;Reporting Services im integrierten SharePoint-Modus&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).  
  
###  <a name="to-view-or-modify-a-subscription"></a><a name="bkmk_to_modify_subscription"></a> So zeigen Sie ein Abonnement an oder ändern es  
  
1.  Wechseln Sie zu einer SharePoint-Bibliothek, die den Bericht enthält.  
  
2.  Klicken Sie neben dem Bericht auf den Pfeil nach unten, und klicken Sie auf **Abonnements verwalten**.  
  
3.  Alle Abonnements werden durch den Übermittlungstyp identifiziert. Klicken Sie auf den Abonnementtyp, um die vorhandenen Eigenschaften anzuzeigen und zu ändern.  
  
###  <a name="to-delete-a-subscription"></a><a name="bkmk_to_delete_subscription"></a> So löschen Sie ein Abonnement  
  
1.  Wechseln Sie zu einer SharePoint-Bibliothek, die den Bericht enthält.  
  
2.  Klicken Sie neben dem Bericht auf den Pfeil nach unten, und klicken Sie auf **Abonnements verwalten**.  
  
3.  Aktivieren Sie das Kontrollkästchen neben dem Abonnement, und klicken Sie auf **Löschen**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Abonnements und Übermittlung &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [E-Mail Delivery in Reporting Services (E-Mail-Übermittlung in Reporting Services)](../../reporting-services/subscriptions/e-mail-delivery-in-reporting-services.md)   
 [Dateifreigabeübermittlung in Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md)   
 [SharePoint-Bibliotheksübermittlung in Reporting Services](../../reporting-services/subscriptions/sharepoint-library-delivery-in-reporting-services.md)   
 [Konfigurieren eines Berichtsservers für die E-Mail-Übermittlung (Berichtsserver-Konfigurations-Manager)](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  
  
  
