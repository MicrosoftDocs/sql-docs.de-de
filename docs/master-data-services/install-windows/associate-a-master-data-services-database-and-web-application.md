---
title: Zuordnen von Datenbank und Webanwendung
description: In SQL Server können Sie eine Master Data Manager-Webanwendung mit einer Master Data Services-Datenbank verknüpfen, um die Datenbank anzugeben, die für Webvorgänge verwendet werden soll.
ms.custom: seo-lt-2019
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ccb25672-f71d-4135-b548-f50eb45d8fa5
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: aba0e7a327d50e2a502270c590e200c690890c98
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272491"
---
# <a name="associate-a-master-data-services-database-and-web-application"></a>Zuordnen einer Master Data Services-Datenbank und -Webanwendung

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Ordnen Sie die [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Webanwendung einer [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Datenbank zu, um die für Webvorgänge verwendete Datenbank anzugeben.  
  
## <a name="prerequisites"></a>Voraussetzungen  
  
-   [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] muss auf dem lokalen Computer installiert sein. Weitere Informationen finden Sie unter [Installieren von Master Data Services](../../master-data-services/install-windows/install-master-data-services.md).  
  
-   Eine lokale [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Webanwendung muss vorhanden sein. Weitere Informationen finden Sie unter [Erstellen einer Master Data Manager-Webanwendung &#40;Master Data Services&#41;](../../master-data-services/install-windows/create-a-master-data-manager-web-application-master-data-services.md).  
  
-   Eine lokale oder eine [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Remotedatenbank muss vorhanden sein. Weitere Informationen finden Sie unter [Erstellen einer Master Data Services-Datenbank](../../master-data-services/install-windows/create-a-master-data-services-database.md).  
  
### <a name="to-associate-a-master-data-services-database-and-web-application"></a>So ordnen Sie eine Master Data Services-Datenbank und -Webanwendung zu  
  
1.  Öffnen Sie [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)].  
  
2.  Klicken Sie im linken Bereich auf **Webkonfiguration**.  
  
3.  Wählen Sie auf der Seite **Webkonfiguration** unter **Webanwendung** aus der Liste **Website** die Website aus, in der die [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Webanwendung enthalten ist.  
  
4.  Wählen Sie im Feld **Webanwendung** die Webanwendung aus, die [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]hostet.  
  
5.  Klicken Sie unter **Anwendung einer Datenbank zuordnen** auf **Auswählen**. Das Dialogfeld **Verbindung mit Datenbank herstellen** wird geöffnet.  
  
6.  Geben Sie Verbindungsinformationen für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an, die die [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Datenbank hostet, und klicken Sie auf **Verbinden**.  
  
7.  Wählen Sie aus der Liste **Master Data Services-Datenbank** die Datenbank aus, der Sie die Webanwendung zuordnen möchten, und klicken Sie dann auf **OK**.  
  
8.  Überprüfen Sie unter **Anwendung einer Datenbank zuordnen**, ob die Instanz- und Datenbankinformationen korrekt sind, und klicken Sie dann auf **Anwenden**.  
  
## <a name="next-steps"></a>Nächste Schritte  
  
-   Beim Erstellen der Webanwendung wird automatisch der programmgesteuerte Zugriff auf die [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Webdienste aktiviert. Damit Entwickler Zugriff auf die Dienstmetadaten erhalten, um auf einfache Weise Proxyklassen für den programmgesteuerten Zugriff zu generieren, sollten Sie die Veröffentlichung von Metadaten aktivieren. Weitere Informationen finden Sie unter [Erstellen von Proxyklassen für den Master Data Manager-Webdienst](../../master-data-services/develop/create-master-data-manager-web-service-proxy-classes.md).  
  
-   Fügen Sie [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]Benutzer und Gruppen hinzu. Wenn keinen Benutzern oder Gruppen Zugriff auf [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]gewährt wurde, müssen Sie [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] mithilfe der Anmeldeinformationen für den [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Systemadministrator öffnen. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../../master-data-services/administrators-master-data-services.md) und [Users and Groups &#40;Master Data Services&#41;](../../master-data-services/users-and-groups-master-data-services.md) (Benutzer und Gruppen [Master Data Services]).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Installieren von Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)   
 [Webkonfiguration &#40;Seite im Konfigurations-Manager für Master Data Sevices&#41;](../../master-data-services/web-configuration-page-master-data-services-configuration-manager.md)  
  
  
