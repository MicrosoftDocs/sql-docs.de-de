---
title: Aktualisieren von Anmeldeinformationen in Berichtsdatenquellen von einer SharePoint-Website | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie sich in Berichte eingebettete Datenquellen und freigegebene Datenquellen, die in einer SharePoint-Dokumentbibliothek gespeichert sind, aktualisieren lassen.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: e0c50b6e-89e7-4b4d-8fe5-c90682c5d1b1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e1dc0c59a6fbe96062e48e24e2f6b126770f0c84
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91935151"
---
# <a name="update-credentials-in-report-data-sources-from-a-sharepoint-site"></a>Aktualisieren von Anmeldeinformationen in Berichtsdatenquellen von einer SharePoint-Website
  In diesem Thema wird beschrieben, wie sich in Berichte eingebettete Datenquellen und freigegebene Datenquellen, die in einer SharePoint-Dokumentbibliothek gespeichert sind, aktualisieren lassen.  
  
 Viele der Berichte können Datenquellen beinhalten oder freigegebene Datenquellen verwenden, die zur Verwendung der Windows-Authentifizierung konfiguriert sind. In einigen Fällen wie bei der Erstellung von Datenwarnungen für in einer SharePoint-Dokumentbibliothek gespeicherte Berichte müssen Sie die Anmeldeinformationen der Datenquelle auf gespeicherte Anmeldeinformationen aktualisieren. Verlangen Sie alternativ keine Anmeldeinformationen.  
  
 Um gespeicherte Anmeldeinformationen in Berichten zu verwenden, erstellen und verwenden Sie ggf. eine neue SQL Server-Anmeldung. Weitere Informationen finden Sie unter [Erstellen eines Anmeldenamens](../../relational-databases/security/authentication-access/create-a-login.md).  
  
### <a name="to-update-an-embedded-data-source-to-use-stored-credentials"></a>So aktualisieren Sie eine eingebettete Datenquelle zur Verwendung von gespeicherten Anmeldeinformationen  
  
1.  Wechseln Sie zur SharePoint-Dokumentbibliothek, in der Sie den Bericht gespeichert haben.  
  
2.  Klicken Sie für den Bericht auf das Symbol des Dropdownmenüs zum Erweitern, und klicken Sie dann auf **Datenquellen verwalten**.  
  
     Die Seite "Datenquellen verwalten" wird geöffnet.  
  
3.  Klicken Sie in der Spalte **Name** auf die Datenquelle.  
  
4.  Überprüfen Sie für **Verbindungstyp** , ob die Option **Benutzerdefinierte Datenquelle** aktiviert ist.  
  
     Diese Option gibt an, dass die Datenquelle in den Bericht eingebettet ist.  
  
5.  Lassen Sie die Optionen **Datenquellentyp** und **Verbindungszeichenfolge** unverändert, sofern für den Bericht keine Verbindung zu einer anderen Datenquelle, einem anderen Server oder einem anderen Datenspeicher hergestellt werden soll.  
  
6.  Wählen Sie für **Anmeldeinformationen**die Option **Gespeicherte Anmeldeinformationen**aus. Diese Option kann nur verwendet werden, wenn die Datenquelle keine Anmeldeinformationen annimmt oder wenn Sie Anmeldeinformationen anderweitig übergeben.  
  
     Die Option **Anmeldeinformationen sind nicht erforderlich** kann auch in einigen Fällen verwendet werden.  
  
     Bei einigen Datenquellentypen muss das Konto mit unbeaufsichtigter Ausführung auf dem Berichtsserver konfiguriert werden. Weitere Informationen finden Sie im Thema für den entsprechenden Datenquellentyp in [Hinzufügen von Daten aus externen Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/add-data-from-external-data-sources-ssrs.md) und [Konfigurieren des unbeaufsichtigten Ausführungskontos &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
7.  Geben Sie einen Benutzernamen und ein Kennwort ein.  
  
    -   Wenn das Konto ein Windows-Domänenbenutzerkonto ist, geben Sie es im Format \<domain>\\<Konto\> an, und klicken Sie anschließend auf **Beim Herstellen einer Verbindung mit der Datenquelle als Windows-Anmeldeinformationen verwenden**.  
  
    -   Wenn Benutzername und Kennwort Datenbank-Anmeldeinformationen sind, wählen Sie **Beim Herstellen einer Verbindung mit der Datenquelle als Windows-Anmeldeinformationen verwenden**nicht aus. Wenn der Datenbankserver Identitätswechsel oder Delegierung unterstützt, wählen Sie ggf. die Option **Ausführungskontext für dieses Konto festlegen**aus.  
  
8.  Klicken Sie zum Überprüfen, ob mit den aktualisierten Anmeldeinformationen für die Datenquelle eine Verbindung hergestellt werden kann, auf **Verbindung testen**.  
  
9. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-update-a-shared-data-source-to-use-stored-credentials"></a>So aktualisieren Sie eine freigegebene Datenquelle zur Verwendung von gespeicherten Anmeldeinformationen  
  
1.  Wechseln Sie zur SharePoint-Dokumentbibliothek, in der Sie die freigegebene Datenquelle gespeichert haben.  
  
2.  Klicken Sie für die freigegebene Datenquelle auf das Symbol des Dropdownmenüs zum Erweitern, und klicken Sie dann auf **Datenquellendefinition bearbeiten**.  
  
     Die Seite "Datenquelle" wird geöffnet.  
  
3.  Lassen Sie die Optionen **Datenquellentyp** und **Verbindungszeichenfolge** unverändert, sofern für die freigegebene Datenquelle keine Verbindung zu einer anderen Datenquelle, einem anderen Server oder einem anderen Datenspeicher hergestellt werden soll.  
  
4.  Wählen Sie für **Anmeldeinformationen**die Option **Gespeicherte Anmeldeinformationen**aus.  
  
     Die Option **Anmeldeinformationen sind nicht erforderlich** kann auch in einigen Fällen verwendet werden. Diese Option kann nur verwendet werden, wenn die Datenquelle keine Anmeldeinformationen annimmt oder wenn Sie Anmeldeinformationen anderweitig übergeben.  
  
     Bei einigen Datenquellentypen muss das Konto mit unbeaufsichtigter Ausführung auf dem Berichtsserver konfiguriert werden. Weitere Informationen finden Sie im Thema für den entsprechenden Datenquellentyp in [Hinzufügen von Daten aus externen Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/add-data-from-external-data-sources-ssrs.md) und [Konfigurieren des unbeaufsichtigten Ausführungskontos &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
5.  Geben Sie einen Benutzernamen und ein Kennwort ein.  
  
    -   Wenn das Konto ein Windows-Domänenbenutzerkonto ist, geben Sie es im Format \<domain>\\<Konto\> an, und klicken Sie anschließend auf **Beim Herstellen einer Verbindung mit der Datenquelle als Windows-Anmeldeinformationen verwenden**.  
  
    -   Wenn Benutzername und Kennwort Datenbank-Anmeldeinformationen sind, wählen Sie **Beim Herstellen einer Verbindung mit der Datenquelle als Windows-Anmeldeinformationen verwenden**nicht aus. Wenn der Datenbankserver Identitätswechsel oder Delegierung unterstützt, wählen Sie ggf. die Option **Ausführungskontext für dieses Konto festlegen**aus.  
  
6.  Klicken Sie zum Überprüfen, ob mit den aktualisierten Anmeldeinformationen für die Datenquelle eine Verbindung hergestellt werden kann, auf **Verbindung testen**.  
  
7.  Stellen Sie sicher, dass die Option "Diese Datenquelle aktivieren" aktiviert ist.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a>Weitere Informationen  
 [Hochladen von Dokumenten in eine SharePoint-Bibliothek &#40;Reporting Services im SharePoint-Modus&#41;](../../reporting-services/report-server-sharepoint/upload-documents-to-a-sharepoint-library-reporting-services-in-sharepoint-mode.md)  
  
  
