---
description: Server und Katalog von Integration Services (SSIS)
title: Server und Katalog von Integration Services (SSIS) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- packages [Integration Services], managing
- managing packages [Integration Services]
ms.assetid: 6d667bba-7c25-492a-8f4d-70ebaca28f40
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d0883def093b7f1500d80c03622be0e9c0a4fd72
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354973"
---
# <a name="integration-services-ssis-server-and-catalog"></a>Server und Katalog von Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Nachdem Sie Pakete und Konfigurationen in [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]entworfen und getestet haben, können Sie die Projekte, die die Pakete enthalten, auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitstellen.  
  
 Der [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server stellt eine Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dar, die als Host für die **SSISDB** -Datenbank fungiert. Die Datenbank speichert die folgenden Objekte: Pakete, Projekte, Parameter, Berechtigungen, Servereigenschaften und Verwendungsverlauf.  
  
 Die **SSISDB** -Datenbank macht die Objektinformationen in öffentlichen Sichten verfügbar, die Sie abfragen können. Die Datenbank stellt auch gespeicherte Prozeduren bereit, die Sie aufrufen können, um die Objekte zu verwalten.  
  
 Bevor Sie die Projekte auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitstellen können, müssen Sie den **SSISDB** -Katalog erstellen.  
  
 Eine Übersicht über die Funktionen des SSISDB-Katalogs finden Sie unter [SSIS-Katalog](../../integration-services/catalog/ssis-catalog.md).  
  
## <a name="high-availability"></a>Hochverfügbarkeit  
 Wie andere Benutzerdatenbanken unterstützt die Datenbank **SSISDB** die Datenbankspiegelung und Replikation. Weitere Informationen zur Spiegelung und Replikation finden Sie unter [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
 Sie können auch die hohe Verfügbarkeit von SSISDB und die Inhalte bereitstellen, indem Sie die SSIS- und Always On-Verfügbarkeitsgruppen verwenden. Weitere Informationen finden Sie unter [Always On für den SSIS-Katalog (SSISDB)](ssis-catalog.md#always-on-for-ssis-catalog-ssisdb). Informationen finden Sie auch im folgenden Blogbeitrag von Matt Masson auf blogs.msdn.com: [SSIS with Always On](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-with-alwayson/ba-p/388091) (SSIS mit Always On).  
  
##  <a name="integration-services-server-in-sql-server-management-studio"></a><a name="ssms"></a> Integration Services-Server in SQL Server Management Studio  
 Wenn Sie eine Verbindung mit einer Instanz des [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dar, die als Host für die **SSISDB** -Datenbank fungiert, werden die folgenden Objekte im Objekt-Explorer angezeigt:  
  
-   **SSISDB-Datenbank**  
  
     Die **SSISDB** -Datenbank wird unter dem Knoten **Datenbanken** im Objekt-Explorer angezeigt. Sie können die Sichten abfragen und die gespeicherten Prozeduren aufrufen, die zur Verwaltung des [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Servers sowie der darauf gespeicherten Objekte verwendet werden.  
  
-   **Integration Services-Kataloge**  
  
     Unter dem Knoten **Integration Services-Kataloge** befinden sich Ordner für [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekte und -Umgebungen.  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [Anzeigen der Liste der auf dem Integration Services-Server gespeicherten Pakete](../../integration-services/catalog/view-the-list-of-packages-on-the-integration-services-server.md)  
  
-   [Bereitstellen von SQL Server Integration Services-Projekten und Paketen (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)  
  
-   [Ausführen von Integration Services-Paketen (SSIS)](../../integration-services/packages/run-integration-services-ssis-packages.md)  
  
## <a name="related-content"></a>Verwandte Inhalte  
 Blogeintrag [SSIS with Always On](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-with-alwayson/ba-p/388091) (SSIS mit Always On) bei blogs.msdn.com.  
  
  
