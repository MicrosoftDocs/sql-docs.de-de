---
title: Zeitplaneigenschaften (Registerkarte Berichte) | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die Seite „Reporting Services-Zeitplaneigenschaften“ in SQL Server Management Studio, auf der alle Berichte für einen bestimmten freigegebenen Zeitplan aufgelistet sind.
ms.date: 06/30/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
f1_keywords:
- sql13.swb.reportserver.scheduleproperties.reports.f1
ms.assetid: 7db728bd-4b08-43ef-a49a-e8dcdd37cf89
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 556fc5e4c785345ede475dcadb2c5673489a279f
ms.sourcegitcommit: e8f6c51d4702c0046aec1394109bc0503ca182f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939390"
---
# <a name="schedule-properties-reports-page"></a>Zeitplaneigenschaften (Registerkarte Berichte)
  Verwenden Sie die [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] -Seite mit den Zeitplaneigenschaften in [!INCLUDE[ssManStudioFull_md](../../includes/ssmanstudiofull-md.md)] , um eine Liste aller Berichte anzuzeigen, die den bestimmten freigegebenen Zeitplan verwenden. Zeitpläne können zum Aktualisieren von Berichtsmomentaufnahmen, zum Generieren des Berichtsverlaufs, zum Auslösen eines Abonnements oder zum Kennzeichnen einer zwischengespeicherten Kopie des Berichts als abgelaufen verwendet werden. Wenn Sie herausfinden möchten, wie der Zeitplan verwendet wird, dann zeigen Sie die Eigenschaften- und Abonnementinformationen des Berichts an.  
  
 Zwar wird auf dieser Seite jeder Bericht angezeigt, der den freigegebenen Zeitplan verwendet, jedoch wird nicht angegeben, wie oft der freigegebene Zeitplan von einem einzelnen Bericht verwendet wird. Angenommen, 20 Abonnenten des Company Sales-Berichts verwenden alle den gleichen freigegebenen Zeitplan, um die Abonnementverarbeitung auszulösen. In diesem Fall wird der Company Sales-Bericht nur einmal in dieser Liste angezeigt, obwohl der Bericht 20 Verweise auf den freigegebenen Zeitplan besitzt.  
  
 So öffnen Sie die Seite „Zeitplaneigenschaften“:
 1. Starten Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].
 2. Stellen Sie eine Verbindung mit einem Berichtsserver her.
 3. Öffnen Sie den Ordner **Freigegebene Zeitpläne** .
 4. Klicken Sie mit der rechten Maustaste auf einen freigegebenen Zeitplan, und wählen Sie **Eigenschaften**aus.
 5. Klicken Sie auf **Berichte**.  
  
  Sie können freigegebene Zeitpläne auch im **-Webportal über** Siteeinstellungen [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] verwalten.
  
> [!NOTE]  
>  Diese Funktion ist nicht in jeder Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verfügbar. Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Editionen und unterstütze Funktionen von SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
## <a name="options"></a>Tastatur  
 **Ordner**  
 Gibt den Pfad des Berichts an.  
  
 **Report**  
 Gibt den Namen des Berichts an, der den Zeitplan verwendet.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen, Ändern oder Löschen von Zeitplänen](../../reporting-services/subscriptions/create-modify-and-delete-schedules.md)   
 [Zeitpläne](../../reporting-services/subscriptions/schedules.md)   
 [Berichtsserver im Management Studio (F1-Hilfe)](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)   
 [Vorgehensweise: Herstellen einer Verbindung mit einem Berichtsserver in Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)   
 [Konfigurieren allgemeiner Berichtseigenschaften (Berichts-Manager)](https://docs.microsoft.com/sql/reporting-services/reports/configure-execution-properties-for-a-report-report-manager)  
  
  

