---
title: Report Server-Windows-Dienst (MSSQLServer) 107 | Microsoft-Dokumentation
description: 'Auf dieser Fehlerreferenzseite erfahren Sie mehr über die Ereignis-ID 107: Berichtsserver-Windows-Dienst (SQL Server) kann nicht mit der Berichtsserver-Datenbank verbunden werden.'
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: troubleshooting
ms.topic: conceptual
helpviewer_keywords:
- MSSQLServer 107 error
ms.assetid: 52b5704b-27f9-400a-a821-d8fa0786afe4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 375387ca85b13572014d206cf7a37ed3fc19d907
ms.sourcegitcommit: df1f0f2dfb9452f16471e740273cd1478ff3100c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87394565"
---
# <a name="report-server-windows-service-mssqlserver-107"></a>Report Server-Windows-Dienst (MSSQLServer) 107
    
## <a name="details"></a>Details  
  
|Category|Wert|  
|-|-|  
|Produktname|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Ereignis-ID|107|  
|Ereignisquelle|Report Server-Windows-Dienst|  
|Komponente|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|  
|Meldungstext|Es kann keine Verbindung zwischen dem Report Server-Windows-Dienst (MSSQLSERVER) und der Berichtsserver-Datenbank hergestellt werden.|  
  
## <a name="explanation"></a>Erklärung  
 Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Report Server-Dienst kann keine Verbindung zur Berichtsserver-Datenbank herstellen. Dieser Fehler tritt während des Neustarts eines Diensts auf, wenn keine Verbindung zur Berichtsserver-Datenbank hergestellt werden kann. Der Fehler tritt unter den folgenden Bedingungen auf:  
  
-   Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-[!INCLUDE[ssDE](../../includes/ssde-md.md)]-Dienst wird nicht ausgeführt, wenn der Berichtsserverdienst gestartet wird.  
  
-   Die Verbindung zum [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Dienst schlägt fehl, da Remoteverbindungen oder das TCP/IP-Protokoll nicht aktiviert sind.  
  
-   Die Berichtsserver-Datenbank wurde nicht ordnungsgemäß konfiguriert.  
  
-   Das Dienstkonto wurde nicht ordnungsgemäß konfiguriert, oder das Konto verfügt nicht mehr über die Berechtigung für die Berichtsserver-Datenbank. Dies tritt auf, wenn Sie beim Einrichten des Kontos oder der Berichtsserver-Datenbank nicht das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurationstool verwenden.  
  
## <a name="user-action"></a>Benutzeraktion  
 Starten Sie den [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Dienst, wenn er nicht bereits ausgeführt wird, und stellen Sie sicher, dass für das TCP/IP-Protokoll Remoteverbindungen zulässig sind.  
  
 Verwenden Sie zum Konfigurieren der Berichtsserver-Datenbank und des Dienstkontos das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurationstool.  
  
## <a name="internal-only"></a>Nur intern  
  
## <a name="see-also"></a>Weitere Informationen  
 [Konfigurieren des Berichtsserver-Dienstkontos &#40;SSRS-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)   
 [Reporting Services-Konfigurations-Manager &#40;einheitlicher Modus&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Start and Stop the Report Server Service (Starten und Beenden des Berichtsserverdiensts)](../../reporting-services/report-server/start-and-stop-the-report-server-service.md)  
  
  
