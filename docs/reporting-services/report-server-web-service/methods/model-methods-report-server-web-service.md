---
title: Modellmethoden – Berichtsserver-Webdienst | Microsoft-Dokumentation
description: In diesem Artikel lernen Sie die Methoden kennen, die Sie zum Verwalten von Modellen im Berichtsserver-Webdienst verwenden können.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
ms.assetid: d3e658c9-bb22-480b-a3d5-bcde8f537ab2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 082c5b37029782a2e1222639199a3da4dee025b3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015250"
---
# <a name="model-methods---report-server-web-service"></a>Modellmethoden – Berichtsserver-Webdienst
  Sie können Modelle mithilfe dieser Methoden verwalten.  
  
|Methode|Action|  
|------------|------------|  
|<xref:ReportService2010.ReportingService2010.GenerateModel%2A>|Generiert auf Grundlage einer freigegebenen Datenquelle ein Standardmodell.|  
|<xref:ReportService2010.ReportingService2010.GetModelItemPermissions%2A>|Ruft die Benutzerberechtigungen ab, die dem Modellelement zugeordnet sind.|  
|<xref:ReportService2010.ReportingService2010.GetModelItemPolicies%2A>|Ruft die Richtlinien ab, die einem Modellelement zugeordnet sind.|  
|<xref:ReportService2010.ReportingService2010.GetUserModel%2A>|Gibt den semantischen Teil eines Modells für den aktuellen Benutzer zurück.|  
|<xref:ReportService2010.ReportingService2010.InheritModelItemParentSecurity%2A>|Löscht die Richtlinien, die einem Modellelement zugeordnet sind, und bewirkt, dass das Modellelement die Richtlinien von dessen übergeordnetem Element erbt.|  
|<xref:ReportService2010.ReportingService2010.ListModelDrillthroughReports%2A>|Listet Drillthroughberichte auf, die einer Entität in einem Modell zugeordnet sind.|  
|<xref:ReportService2010.ReportingService2010.ListModelItemChildren%2A>|Gibt ein Array untergeordneter Elemente von Modellelementen zurück.|  
|<xref:ReportService2010.ReportingService2010.ListModelItemTypes%2A>|Gibt eine Liste unterstützter Modellelementtypen zurück.|  
|<xref:ReportService2010.ReportingService2010.ListModelPerspectives%2A>|Listet Modelle und Perspektiven auf, die dem Benutzer zur Verfügung stehen.|  
|<xref:ReportService2010.ReportingService2010.RegenerateModel%2A>|Aktualisiert ein vorhandenes Modell auf Grundlage von Änderungen am Datenquellenschema.|  
|<xref:ReportService2010.ReportingService2010.RemoveAllModelItemPolicies%2A>|Löscht alle Richtlinien, die Modellelementen im angegebenen Modell zugeordnet sind.|  
|<xref:ReportService2010.ReportingService2010.SetModelDrillthroughReports%2A>|Ordnet mehrere Drillthroughberichte einem Modell zu.|  
|<xref:ReportService2010.ReportingService2010.SetModelItemPolicies%2A>|Legt Sicherheitsrichtlinien für ein Modellelement fest.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen von Anwendungen mit dem Webdienst und .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Berichtsserver-Webdienst](../../../reporting-services/report-server-web-service/report-server-web-service.md)   
 [Webdienstmethoden für Berichtsserver](../../../reporting-services/report-server-web-service/methods/report-server-web-service-methods.md)   
 [Technische Referenz (SSRS)](../../../reporting-services/technical-reference-ssrs.md)  
  
  
