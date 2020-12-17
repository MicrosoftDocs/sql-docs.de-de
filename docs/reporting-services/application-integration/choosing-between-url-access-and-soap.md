---
title: Entscheidung zwischen URL-Zugriff und SOAP
description: 'Es gibt zwei Möglichkeiten, Reporting Services in benutzerdefinierte Anwendungen zu integrieren: URL-Zugriff und die Reporting Services-SOAP-API. Erfahren Sie, wie Sie eine Auswahl treffen.'
ms.date: 10/19/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.custom: seo-lt-2019
ms.topic: reference
author: maggiesMSFT
ms.author: maggies
monikerRange: = sql-server-2016
ms.openlocfilehash: bab3605274c3858577fa1bb5f82743c6cd447772
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484362"
---
# <a name="choose-between-url-access-and-soap-in-reporting-services"></a>Entscheidung zwischen URL-Zugriff und SOAP in Reporting Services

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-2017](../../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../../includes/ssrs-appliesto-not-pbirs.md)]

Die Integration von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in benutzerdefinierte Anwendungen ist manchmal nicht ganz einfach. Die Herausforderung liegt jedoch nicht in der Komplexität des Programmiermodells oder in den APIs, sondern in den vielen Möglichkeiten der Integration. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] wurde von Grund auf als Entwicklerplattform konzipiert, daher stand die Flexibilität bei der Programmierung immer im Vordergrund. Die hohe Flexibilität fordert jedoch häufige Entscheidungen, wenn die [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Berichtsnavigations- und Verwaltungsfunktionen in die vorhandenen Geschäftsanwendungen integriert werden.

> [!NOTE]
> Ab SQL Server 2017 Reporting Services ist der Zugriff auf die REST-API für die Entwicklung von Lösungen verfügbar. Der Zugriff auf die SOAP-API ist veraltet. Weitere Informationen finden Sie unter [Develop with the REST APIs for Reporting Services (Entwickeln mit den REST-APIs für Reporting Services)](../developer/rest-api.md).
  
 Es gibt zwei Möglichkeiten, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in benutzerdefinierte Anwendungen zu integrieren: URL-Zugriff und die Reporting Services-SOAP-API. Welche der beiden Arten verwendet werden soll, hängt von mehreren Faktoren ab. In einigen Fällen müssen für die Integration von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in die benutzerdefinierten Geschäftsanwendungen sowohl URL-Zugriff als auch SOAP verwendet werden. Sie sollten sich folgende Fragen stellen:  
  
-   Welche Art Berichtsfunktionen benötigen Sie oder die Endbenutzer? Brauchen Sie einfache Start- und Navigationsfunktionen für die Berichte, oder benötigen Sie ausgereifte Berichtsserver-Verwaltungsfunktionen aus Ihrer benutzerdefinierten Geschäftsanwendung?  
  
-   In welcher Art Umgebung arbeiten die Benutzer normalerweise? Ist die Geschäftsanwendung eine Webanwendung oder eine Windows-Anwendung? Wie leicht können die Benutzer von einer Win32-Umgebung in eine Webumgebung wechseln? Welche Art der Kontrolle benötigen Sie für die Umgebung, in der die Berichte ausgeführt und verwaltet werden?  
  
 Wenn Sie diese Fragen beantwortet haben, können Sie entscheiden, wie Sie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in Ihre IT-Infrastruktur integrieren. Normalerweise wird der URL-Zugriff für die Anzeige und Navigation der Berichte bevorzugt. Der URL-Zugriff ermöglicht eine rasche Navigation durch die Berichte ohne den Aufwand eines Webdiensts. Außerdem ist der URL-Zugriff derzeit die einzige Programmiertechnik, die die komplette Funktionalität von HTML Viewer, einschließlich der Berichtssymbolleiste, für die Berichtsnavigation verwendet. Darüber hinaus sorgt der URL-Zugriff für eine bessere Leistung als SOAP, da das Marshalling der SOAP-Anforderungen zum Server hin und wieder zurück umgangen wird. In den Integrationsszenarien, in denen rasch und problemlos mit integrierten Anzeige- und Navigationstools auf die Berichte zugegriffen werden muss, ist der URL-Zugriff die bessere Variante.  
  
> [!NOTE]  
> Der Berichtsserver-URL-Zugriff unterstützt HTML Viewer und die erweiterten Funktionen der Berichtssymbolleiste. Die SOAP-API unterstützt diese Art des gerenderten Berichts nicht. Wenn Sie Berichte mithilfe der SOAP-API rendern, müssen Sie Ihre eigene Berichtssymbolleiste entwerfen und entwickeln.
  
 Weitere Informationen zur Berichtssymbolleiste finden Sie unter [HTML Viewer and the Report Toolbar (HTML-Viewer und die Berichtssymbolleiste)](../../reporting-services/html-viewer-and-the-report-toolbar.md).  
  
 Weitere Informationen zum URL-Zugriff finden Sie unter [URL Access (URL-Zugriff)](../../reporting-services/url-access-ssrs.md).  
  
 Der URL-Zugriff ist sinnvoll für die Anzeige von Berichten, er liefert jedoch nicht die Berichts- und Namespace-Verwaltungsfunktionen, die in bestimmten Unternehmensberichtsszenarien von erheblicher Bedeutung sein können. In diesem Fall empfiehlt sich die Verwendung der umfangreichen Funktionalität der Reporting Services-SOAP-API. Mit der SOAP-API können Sie Berichte verwalten und bereitstellen, Zeitpläne erstellen, Servereigenschaften konfigurieren, Berichtsserver-Namespaces verwalten, Abonnements erstellen und vieles mehr. Die SOAP-API stellt Ihnen die vollständige Verwaltungsfunktionalität in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] zur Verfügung. Die SOAP-API kann die Berichtsanzeige und -navigation auch über die <xref:ReportExecution2005.ReportExecutionService.Render%2A>-Methode der API ermöglichen. Die Berichtsanzeige über die SOAP-API ermöglicht jedoch weder die integrierte Anzeigefunktion der Berichtssymbolleiste noch die automatische Berichtsinteraktivität wie beim URL-Zugriff.  
  
 Weitere Informationen zur Reporting Services-SOAP-API finden Sie unter [Report Server Web Service (Berichtsserver-Webdienst)](../../reporting-services/report-server-web-service/report-server-web-service.md).  
  
 In der Mehrheit der Fälle werden sowohl der URL-Zugriff als auch die SOAP-Aufrufe benötigt, um Ihre Berichtsanforderungen zu erfüllen. SOAP wird verwendet, wenn Sie zum ersten Mal eine Verbindung zur Berichtsserver-Datenbank herstellen und die verfügbare Liste der Berichte in einer Benutzeroberfläche darstellen. URL-Zugriff wird verwendet, um die einzelnen Berichte tatsächlich aufzurufen und darin zu navigieren.  
  
 Ein Beispiel für die Kombination des URL-Zugriffs mit dem Webdienst, um eine integrierte Berichterstellung bereitzustellen, finden Sie unter [SQL Server Reporting Services-Produktbeispiele](https://go.microsoft.com/fwlink/?LinkId=177889).

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
