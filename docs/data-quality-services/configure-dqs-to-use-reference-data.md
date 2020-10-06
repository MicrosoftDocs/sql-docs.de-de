---
description: Konfigurieren von DQS zum Verwenden von Verweisdaten
title: Konfigurieren von DQS zum Verwenden von Verweisdaten
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
f1_keywords:
- sql13.dqs.administration.rdsconfiguration.f1
- sql13.dqs.administration.configuration.createDirectRDS.f1
- sql13.dqs.admin.config.rds.f1
ms.assetid: fae745e7-57a7-4cbc-8979-56ea8e392e4e
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 0ea7b94d092565827bbd0086f7f4d122ce219fac
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91725417"
---
# <a name="configure-dqs-to-use-reference-data"></a>Konfigurieren von DQS zum Verwenden von Verweisdaten

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird beschrieben, wie [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) zum Verwenden von Verweisdaten zum Bereinigen der Daten konfiguriert wird. Sie können entweder Verweis Daten von Azure Marketplace oder von direkten Online Verweis Daten Drittanbietern verwenden.  

> [!IMPORTANT]
> In diesem Artikel werden Referenzdatendienste von Drittanbietern erwähnt, die zuvor in Azure DataMarket verfügbar waren. DataMarket und Data Services – einschließlich z.B. Melissa-Adressdaten – wurden am 31.12.2016 eingestellt. Daher können Sie die Beispiele in diesem Artikel nicht mehr mit den angegebenen Diensten von DataMarket ausführen. Sie können weiterhin Referenzdatendienste nutzen, die von externen Referenzdatenanbietern direkt online angeboten werden.

## <a name="before-you-begin"></a>Vorbereitungen  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
 Um Verweisdaten vom Marketplace zu verwenden, müssen Sie über einen gültigen Marketplace-Kontoschlüssel verfügen. Ausführliche Informationen zum Erstellen eines Marketplace-Konto Schlüssels finden [Sie unter Erstellen Ihres Kontos](/previous-versions/azure/ff717655(v=azure.100)) () https://go.microsoft.com/fwlink/?LinkId=212936) . Sie können auch innerhalb von [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] einen Marketplace-Kontoschlüssel erstellen, indem Sie auf **Konfiguration** unter **Verwaltung** im [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] -Startbildschirm und dann auf **ID für DataMarket-Konto erstellen** auf der Registerkarte **Verweisdaten** klicken.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Sie müssen über die Rolle „dqs_administrator“ in der DQS_MAIN-Datenbank verfügen, um Verweisdaten-Diensteinstellungen in DQS zu konfigurieren.  
  
##  <a name="configure-dqs-to-use-reference-data-from-marketplace"></a><a name="Marketplace"></a> Konfigurieren von DQS zum Verwenden von Verweisdaten vom Marketplace  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)][Führen Sie die Data Quality-Client Anwendung](../data-quality-services/run-the-data-quality-client-application.md)aus.  
  
2.  Klicken Sie im [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] -Startbildschirm unter **Verwaltung**auf **Konfiguration**.  
  
3.  Geben Sie auf der Registerkarte **Verweisdaten** unter dem Bereich **Netzwerkeinstellungen** entsprechende Werte in den Feldern **Proxyserver** und **Port** ein, wenn Sie oder Ihre Organisation mithilfe des Proxyservers eine Verbindung zum Internet herstellt.  
  
4.  Geben Sie den Marketplace-Kontoschlüssel im Feld **ID des DataMarket-Kontos** ein, und klicken Sie auf das Symbol **DataMarket-Konto-ID überprüfen** , um den Kontoschlüssel zu überprüfen. Eine Meldung zeigt an, ob der angegebene Marketplace-Kontoschlüssel gültig ist.  
  
 Jetzt können Sie die Verweisdatendienste, die für den angegebenen Marketplace-Kontoschlüssel abonniert wurden, vom Marketplace in DQS verwenden.  
  
##  <a name="configure-dqs-to-use-reference-data-from-direct-online-third-party-reference-data-providers"></a><a name="ThirdParty"></a> Konfigurieren von DQS zum Verwenden von Verweisdaten von direkten Onlineverweisdatendrittanbietern  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)][Führen Sie die Data Quality-Client Anwendung](../data-quality-services/run-the-data-quality-client-application.md)aus.  
  
2.  Klicken Sie im [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] -Startbildschirm unter **Verwaltung**auf **Konfiguration**.  
  
3.  Geben Sie auf der Registerkarte **Verweisdaten** unter dem Bereich **Netzwerkeinstellungen** entsprechende Werte in den Feldern **Proxyserver** und **Port** ein, wenn Sie oder Ihre Organisation mithilfe des Proxyservers eine Verbindung zum Internet herstellt.  
  
4.  Klicken Sie im Bereich **Einstellungen für direkten Reference Data Service-Onlinedrittanbieter** auf das Symbol **Neuen Reference Data Service-Anbieter hinzufügen** .  
  
5.  Geben Sie im Dialogfeld **Neuen direkten Reference Data Service-Onlinedrittanbieter erstellen** die folgenden Details an:  
  
    1.  Geben Sie im Feld **Name** einen Namen für den neuen direkten Reference Data Service-Anbieter ein.  
  
    2.  (Optional) Geben Sie im Feld **Beschreibung** eine Beschreibung für den neuen direkten Reference Data Service-Anbieter ein.  
  
    3.  Geben Sie im Feld **Kategorie** die Kategorie der Daten ein, die vom neuen direkten Reference Data Service-Anbieter bereitgestellt werden.  
  
    4.  Geben Sie im Feld Schema das Schema an, das die Zeichenfolge von Feldern (Spaltennamen) definiert, die vom direkten Reference Data Service-Anbieter verwendet werden soll. Ein Feldname sollte kein Leerzeichen enthalten, und die Felder sollten durch Trennzeichen getrennt sein. Beispiel: `FirstName, LastName, City, State`.  
  
    5.  Geben Sie im Feld **URI** die URI für den direkten Reference Data Service-Anbieter ein. Nur sichere URIs (Adresse, die mit „https://“ beginnt) sind in DQS zulässig.  
  
    6.  Geben Sie im Feld **Max. Batchgröße** die maximale Anzahl von Datensätzen pro Batch ein, die an den Reference Data Service-Anbieter zum Bereinigen gesendet werden. Maximal 100 Datensätze pro Batch können für die Bereinigungsaktivität angegeben werden.  
  
    7.  Geben Sie im Feld **Konto-ID** die Konto-ID des Abonnenten des Reference Data Service-Anbieters ein.  
  
6.  Klicken Sie auf **OK** , um die Daten zu speichern, und schließen Sie das Dialogfeld **Neuen direkten Reference Data Service-Onlinedrittanbieter erstellen** . Der neu hinzugefügte direkte Reference Data Service-Onlinedrittanbieter wird im **Raster des direkten Reference Data Service-Anbieters** in DQS verfügbar.  
  
 Jetzt können Sie die Reference Data Services des neu konfigurierten direkten Reference Data Service-Onlinedrittanbieters in DQS verwenden.  
  
##  <a name="follow-up-after-configuring-dqs-to-use-reference-data"></a><a name="FollowUp"></a> Nachverfolgung: Nach dem Konfigurieren von DQS zum Verwenden von Verweisdaten  
 Sie müssen die erforderlichen Wissensdatenbankdomänen jetzt den Verweisdaten zuordnen, die bei den gerade konfigurierten Datenanbietern verfügbar sind. Weitere Informationen dazu finden Sie unter [Anfügen einer Domäne oder Verbunddomäne an Verweisdaten](../data-quality-services/attach-domain-or-composite-domain-to-reference-data.md).  
  
