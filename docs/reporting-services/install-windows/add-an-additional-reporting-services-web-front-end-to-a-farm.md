---
description: Hinzufügen eines zusätzlichen Reporting Services-Web-Front-Ends zu einer Farm
title: Hinzufügen eines zusätzlichen Reporting Services-Web-Front-Ends zu einer Farm | Microsoft-Dokumentation
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
ms.assetid: d7a11bda-ae26-49ac-b071-37d83cae5afe
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: dc2b1eaee352b34333be5a166168161194e1f03d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88418626"
---
# <a name="add-an-additional-reporting-services-web-front-end-to-a-farm"></a>Hinzufügen eines zusätzlichen Reporting Services-Web-Front-Ends zu einer Farm
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint-Modus schließt für Anwendungsserver und Web-Front-End-Server (WFE) benötigte Komponenten ein. In diesem Thema liegt der Schwerpunkt auf der Installation der erforderlichen [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Komponenten für einen WFE-Server, einschließlich der Anwendungsseiten, die von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Funktionen wie z. B. Abonnements, Datenwarnungen und [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)]verwendet werden. Die primäre für ein WFE benötigte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Installation ist das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Add-In für SharePoint 2016-Produkte.  
  
## <a name="prerequisites"></a>Voraussetzungen  
  
-   Sie müssen lokaler Administrator sein, um SQL Server-Setup auszuführen.  
  
-   Der Computer muss einer Domäne hinzugefügt werden.  
  
-   Sie müssen den Namen des vorhandenen Datenbankservers wissen, der die SharePoint-Konfiguration und Inhaltsdatenbanken hostet.  
  
-   Der Datenbankserver muss konfiguriert sein, um Remotedatenbankverbindungen zu berücksichtigen.  Wenn nicht, sind Sie nicht in der Lage, den neuen Server zur Farm hinzuzufügen, da der neue Server nicht in der Lage ist, eine Verbindung mit den SharePoint-Konfigurationsdatenbanken herzustellen.  
  
-   Auf dem neuen Server muss die gleiche Version von SharePoint installiert sein, die auf den aktuellen Farmservern ausgeführt wird. Wenn auf der Farm z.B. bereits SharePoint 2013 Service Pack 1 (SP1) installieren ist, müssen Sie SP1 auch auf dem neuen Server installieren, bevor er zur Farm hinzugefügt werden kann.  
  
## <a name="steps"></a>Schritte  
 Die Schritte in diesem Thema gehen davon aus, dass ein SharePoint-Farmadministrator den Server installiert und konfiguriert. Das Diagramm zeigt eine typische dreistufige Umgebung, und die nummerierten Elemente im Diagramm werden in der folgenden Liste beschrieben:  
  
-   (1) Mehrere Web-Front-End (WFE)-Server. Die WFE-Server erfordern das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Add-In für SharePoint 2010. Mit den folgenden Schritte wird dieser Ebene ein zweiter Anwendungsserver hinzugefügt.  
  
-   (2) Zwei Anwendungsserver, die [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] und Websites ausführen, z. B. Zentraladministration.  
  
-   (3) Zwei SQL Server-Datenbankserver.  
  
-   (4) Stellt eine Software oder eine Hardware-Netzwerklastenausgleichslösung (NLB) dar  
  
 ![Hinzufügen von SSRS zu einem neuen SharePoint WFE](../../reporting-services/install-windows/media/rs-sharepointscale-wfe.gif "Hinzufügen von SSRS zu einem neuen SharePoint WFE")  
  
 Die folgenden Schritte gehen davon aus, dass ein Administrator den Server installiert und konfiguriert.  
  
|Schritt|Beschreibung und Link|  
|----------|--------------------------|  
|Fügen Sie einen SharePoint-Server zu einer Farm hinzu.|Sie müssen SharePoint installieren, um eine weitere Reporting Services-Anwendung bereitzustellen.<br/><br/>Für SharePoint 2013 finden Sie Informationen dazu unter [Hinzufügen eines SharePoint-Servers zu einer Farm in SharePoint Server 2013](https://technet.microsoft.com/library/cc261752(v=office.15).aspx).<br/><br/>Für SharePoint 2016 finden Sie Informationen dazu unter [Hinzufügen eines SharePoint-Servers zu einer Farm in SharePoint Server 2016](https://technet.microsoft.com/library/cc261752(v=office.16).aspx).|  
|Installieren Sie das SQL Server Reporting Services-Add-In für SharePoint 2016-Produkte|Es gibt mehrere Methoden zum Installieren des Add-Ins. In den folgenden Schritten wird der SQL Server-Setup-Assistent verwendet. Weitere Informationen zur Installation des Add-Ins finden Sie unter [Installieren oder Deinstallieren des Reporting Services-Add-Ins für SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md).<br /><br /> 1) Führen Sie die SQL Server-Installation aus.<br /><br /> 2) Wählen Sie auf der Seite **Setuprolle** die Option **SQL Server-Funktionsinstallation**aus.<br /><br /> 3) Wählen Sie auf der Seite **Funktionsauswahl** die Option **Reporting Services-Add-In für SharePoint-Produkte**aus.<br /><br /> 4) Klicken Sie auf den nächsten Seiten auf **Weiter** , um die Setupoptionen zu vervollständigen.<br /><br/>Weitere Informationen zum Installieren von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] finden Sie unter [Installieren des ersten Berichtsservers im SharePoint-Modus](install-the-first-report-server-in-sharepoint-mode.md).|  
|Überprüfen Sie, ob der neue Server betriebsbereit ist.|1) Klicken Sie in der SharePoint-Zentraladministration in der Gruppe **Systemeinstellungen** auf **Server in dieser Farm verwalten** .<br /><br /> 2) Überprüfen Sie, ob der neue Server in der Liste aufgeführt ist.|  
|Aktualisieren Sie die NLB-Lösung.|Aktualisieren Sie die Hardware- oder Software-NLB-Umgebung, falls erforderlich, um den neuen Server einzuschließen.|  

## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen eines SharePoint-Servers zu einer Farm in SharePoint Server 2016](https://technet.microsoft.com/library/cc261752(v=office.16).aspx)  
[Hinzufügen eines SharePoint-Servers zu einer Farm in SharePoint Server 2013](https://technet.microsoft.com/library/cc261752(v=office.15).aspx)

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
