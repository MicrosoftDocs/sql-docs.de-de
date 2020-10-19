---
title: Starten und Beenden des Berichtsserverdiensts | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den Windows-Dienst starten und anhalten, der den Berichtsserver-Webdienst, das Webportal und eine Hintergrundverarbeitungsanwendung umfasst.
ms.date: 05/21/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- stopping Report Server service
- Report Server Windows service, starting
- Report Server service, starting
- starting Report Server service
ms.assetid: 6ec69ac3-27b0-472d-91e1-733af9078ed2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b7f60e735ecd2483f8f105666ee181cb48eaa98b
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91987506"
---
# <a name="start-and-stop-the-report-server-service"></a>Starten und Beenden des Berichtsserverdiensts

  Der Berichtsserver wird als Windows-Dienst implementiert, der den Berichtsserver-Webdienst, das Webportal und eine Hintergrundverarbeitungsanwendung umfasst. Der Dienst muss ausgeführt werden, damit Sie die Berichtsserverfunktionalitäten nutzen können. Durch Beenden des Diensts werden alle Berichtsservervorgänge beendet.  
  
 Wenn der Dienst beendet ist, werden Anforderungen für die Verarbeitung von geplanten Berichts- und Abonnementverarbeitungen in die Warteschlange gestellt. Dies liegt daran, dass diese Ereignisse von vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ausgeführten Aufträgen erstellt werden. Wenn Sie einen Backlog von Vorgängen vermeiden möchten, während der Dienst deaktiviert ist, sollten Sie erwägen, den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ebenfalls zu beenden.  
  
 Sie können den Berichtsserverdienst mit einer Vielzahl von Tools starten oder beenden, u.&nbsp;a. mit dem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurationstool, dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Manager und dem Services-Tool von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 Wenn Sie über das Starten und Beenden des Diensts hinaus weitere Vorgänge durchführen möchten, z.B. Ändern des Dienstkontos, müssen Sie das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Konfigurationstool verwenden. Wenn Sie zum Ändern des Dienstkontos andere Tools verwenden, kann dadurch Ihre [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Installation beschädigt werden. Weitere Informationen finden Sie unter [Konfigurieren des Berichtsserver-Dienstkontos &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  
  
 Der Dienst kann nicht angehalten und dann fortgesetzt werden. Es gibt keine Startparameter. Obwohl keine expliziten Abhängigkeiten vorhanden sind, muss der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ausgeführt werden, wenn auf dem Berichtsserver Abonnements oder geplante Berichtsvorgänge unterstützt werden sollen.  
  
## <a name="use-the-reporting-services-configuration-tool"></a>Verwenden des Reporting Services-Konfigurationstools  
  
1. Starten Sie das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurationstool, und stellen Sie eine Verbindung mit dem Berichtsserver her.  
  
2. Klicken Sie auf der Seite „Berichtsserverstatus“ auf **Beenden** oder auf **Starten**.  
  
## <a name="use-administrative-tools"></a>Verwenden von Verwaltungstools  

- Öffnen Sie unter „Verwaltung“ den Eintrag „Dienste“, klicken Sie mit der rechten Maustaste auf **SQL Server Reporting Services (MSSQLSERVER)** , und klicken Sie auf **Beenden** bzw. **Neu starten**.  
  
- Wenn mehrere Instanzen ausgeführt werden oder der Berichtsserver als eine benannte Instanz ausgeführt wird, überprüfen Sie, ob der Instanzname in Klammern der Berichtsserverinstanz entspricht, die beendet oder neu gestartet werden soll.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Berichtsserver-Konfigurations-Manager &#40;einheitlicher Modus&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Starten, Beenden oder Anhalten des SQL Server-Agent-Diensts](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
  
