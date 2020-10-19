---
title: Konfigurieren von Datenquelleneigenschaften für einen paginierten Bericht – SSRS | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie in Reporting Services Datenquelleneigenschaften für einen paginierten Bericht konfigurieren. Außerdem erfahren Sie, wie Sie die Eigenschaften festlegen, um die Verbindungsinformationen für die Datenquelle zu variieren.
ms.date: 05/24/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
helpviewer_keywords:
- data sources [Reporting Services], embedded
ms.assetid: 27af5195-c845-40e0-9a9c-efe569424022
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 3dc1e561835cf3e44f48ed1ef77fe22d289a3ec6
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91935023"
---
# <a name="configure-data-source-properties-for-a-paginated-report"></a>Konfigurieren von Datenquelleneigenschaften für einen paginierten Bericht
  Bei der Ausführung eines paginierten Berichts ruft der Berichtsserver Eigenschaftsinformationen ab, um festzulegen, wie die Verbindung mit einer Datenquelle hergestellt werden soll. Dabei werden der Typ der Datenquelle, die Verbindungszeichenfolge sowie Anmeldeinformationen auf den Eigenschaftenseiten für die Datenquelle des veröffentlichten Berichts angegeben. Sie können die Eigenschaften festlegen, um andere Informationen für die Verbindungsherstellung mit der Datenquelle anzugeben als bei der Erstellung des Berichts genannt wurden.  
  
 Alternativ können Sie stattdessen die freigegebene Datenquelle angeben, wenn Sie über eine vordefinierte freigegebene Datenquelle verfügen, in der bereits die gewünschten Verbindungsinformationen festgelegt sind. Klicken Sie zur Verwendung einer freigegebenen Datenquelle auf der Eigenschaftenseite für die Datenquelle des Berichts auf **Eine freigegebene Datenquelle** .  
  
## <a name="to-configure-an-embedded-data-source"></a>So konfigurieren Sie eine eingebettete Datenquelle  
  
1.  Navigieren Sie im Webportal zu dem Bericht, für den Sie eine berichtsspezifische Datenquelle konfigurieren möchten.  
  
3.  Klicken Sie auf die Auslassungspunkte ( **...** ) in der oberen rechten Ecke und anschließend auf **Verwalten**.  
  
4.  Klicken Sie auf die Registerkarte **Datenquellen** . Hiermit wird die Eigenschaftenseite der Datenquelle des Berichts aufgerufen.  
  
5.  Klicken Sie auf **Eine benutzerdefinierte Datenquelle** , um Verbindungsinformationen für die Datenquelle innerhalb des Berichts anzugeben.  
  
6.  Geben Sie in der Liste **Verbindungstyp** die Datenverarbeitungserweiterung an, die zum Verarbeiten von Daten aus der Datenquelle verwendet wird.  
  
7.  Geben Sie in das Feld **Verbindungszeichenfolge**die Verbindungszeichenfolge an, die vom Berichtsserver zum Herstellen der Verbindung zur Datenquelle verwendet wird. Es wird empfohlen, dass Sie keine Anmeldeinformationen in der Verbindungszeichenfolge angeben.  
  
     Im folgenden Beispiel wird eine Verbindungszeichenfolge zum Herstellen einer Verbindung mit der lokalen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank veranschaulicht:  
  
    ```  
    data source=<localservername>; initial catalog=AdventureWorks2012  
    ```  
  
8.  Geben Sie für **Verbindung herstellen über**an, wie die Anmeldeinformationen bei Ausführung des Berichts abgerufen werden:  
  
    -   Wenn der Benutzer zur Eingabe eines Anmeldenamens und eines Kennworts aufgefordert werden soll, klicken Sie auf **Bereitgestellte Anmeldeinformationen vom Benutzer, der den Bericht ausführt**.  
  
    -   Wenn Sie die Datenquelle mit Berichten verwenden möchten, die Abonnements oder andere geplante Vorgänge (z.B. automatisierte Berichtsverlaufgenerierung) unterstützen, klicken Sie auf **Anmeldeinformationen, die sicher auf dem Berichtsserver gespeichert sind**.  
  
    -   Wenn der Berichtsserver die Anmeldeinformationen des auf den Bericht zugreifenden Benutzers an den Server übergeben soll, der die externe Datenquelle hostet, klicken Sie auf **Integrierte Sicherheit von Windows**. In diesem Fall wird der Benutzer nicht aufgefordert, einen Benutzernamen oder ein Kennwort einzugeben.  
  
    -   Klicken Sie auf **Anmeldeinformationen sind nicht erforderlich**, wenn Sie eine Datenquelle verwenden, die nicht mit Anmeldeinformationen arbeitet (z.B. wenn es sich bei der Datenquelle um eine XML-Datei handelt, auf die vom Dateisystem zugegriffen wird). Diesen Typ Anmeldeinformationen sollten Sie nur dann angeben, wenn er von der Datenquelle unterstützt wird. Wenn Sie diese Option für eine Datenquelle aktivieren, die Authentifizierung erfordert, schlägt die Verbindungsherstellung fehl. Vergewissern Sie sich bei der Auswahl dieser Option, dass Sie das unbeaufsichtigte Ausführungskonto konfigurieren, mit dem der Berichtsserver eine Verbindung zu anderen Computern herstellen kann, um Daten oder Dateien abzurufen, wenn keine Anmeldeinformationen zur Verfügung stehen.  
  
 Weitere Informationen zum Konfigurieren von Anmeldeinformationen finden Sie unter [Angeben der Anmeldeinformationen und Verbindungsinformationen für Berichtsdatenquellen](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md). Weitere Informationen zum Konto für die unbeaufsichtigte Ausführung finden Sie unter [Konfigurieren des unbeaufsichtigten Ausführungskontos &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen, Ändern und Löschen von freigegebenen Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md)   
[Verwalten von Berichtsdatenquellen](../../reporting-services/report-data/manage-report-data-sources.md)
  
