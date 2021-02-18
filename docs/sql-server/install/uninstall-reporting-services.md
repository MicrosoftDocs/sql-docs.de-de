---
title: Deinstallieren von Reporting Services | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie die Reporting Services deinstalliert werden. Dabei werden Inhalte, die Sie erstellt haben, oder Konfigurationen, die Sie bearbeitet haben, jedoch nicht entfernt.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 5c764a00-d4bc-465d-b32e-e4efce052ce4
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 650f22c78e0913d8b92c431de65963b802232141
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336052"
---
# <a name="uninstall-reporting-services"></a>Deinstallieren von Reporting Services
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Bei der Deinstallation von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] werden nicht der Inhalt, den Sie erstellt haben, bzw. die von Ihnen vorgenommenen Konfigurationsänderungen entfernt. Wenn es jedoch Inhalt gibt, den Sie nach der Deinstallation benötigen, sollten Sie Kopien des Inhalts erstellen, bevor Sie den Deinstallationsprozess starten.  
  
## <a name="uninstall-sharepoint-mode"></a>Deinstallation im SharePoint-Modus  
 Wenn Sie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] im SharePoint Modus deinstallieren, wird Folgendes entfernt:  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Dienst und -Dienstproxy.  
  
-   Für die [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Installation verwendete Dateien.  
  
 Die [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Dienstanwendungen werden nicht entfernt. Wenn Sie die Dienstanwendungen nicht mehr benötigen, löschen Sie diese mithilfe von Windows PowerShell oder der SharePoint-Zentraladministration.  
  
 Die Berichtselemente und verwandte Metadaten werden nicht entfernt. Diese Informationen sind im Inhalt und in den Konfigurationsdatenbanken enthalten, die mit den [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Dienstanwendungen verwandt sind. Die Datenbanken werden nicht entfernt, und Sie können die Datenbanken manuell zu einer anderen Installation von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] im SharePoint-Modus migrieren. Wenn Sie die Informationen jedoch nicht mehr benötigen, löschen Sie die Datenbanken. Weitere Informationen finden Sie unter [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md).  
  
 Im Folgenden finden Sie Beispielnamen der drei [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenbanken, die nicht entfernt werden.  
  
-   **Berichtsserver-Datenbank:** ReportingService_7f616e2d253040e8ab5653b3c09a065e  
  
-   **Temporäre Berichtsserver-Datenbank:** ReportingService_7f616e2d253040e8ab5653b3c09a065eTempDB  
  
-   **Datenbank für Berichtsserverwarnungen:** ReportingService_7f616e2d253040e8ab5653b3c09a065e_Alerting  
  
### <a name="uninstall-the-add-in-for-sharepoint-products"></a>Deinstallation des Add-Ins für SharePoint-Produkte.  
 Wenn Sie das Add-In von einem Computer deinstallieren, können Sie auswählen, ob Sie nur die Dateien entfernen möchten oder auch die [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Funktion von der Farm entfernen möchten. Informationen zur Deinstallation des [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Add-Ins für SharePoint-Produkte finden Sie unter [Installieren oder Deinstallieren des Reporting Services-Add-Ins für SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md).  
  
## <a name="uninstall-native-mode"></a>Deinstallation im einheitlichen Modus  
 Wenn Sie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] im einheitlichen Modus deinstallieren, wird alles, was nach der Installation **erstellt** oder **geändert** wurde, beibehalten. Beispiele für Datenbankdateien, Protokolldateien, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurationsdateien und Inhaltselemente wie Berichte und Datenquellendateien.  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ist eine Instanzfunktion und deshalb in der Systemsteuerung, den Programmen und Funktionen von Windows nicht aufgeführt. So deinstallieren Sie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] im einheitlichen Modus:  
  
1.  Klicken Sie in der Windows-Systemsteuerung auf **Programme und Funktionen**.  
  
2.  Wählen Sie unter **Programme und Funktionen** die Option **Microsoft SQL Server 2016** aus.  
  
3.  Wählen Sie im Deinstallations-Assistenten die Instanz aus, die die [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Instanzfunktion **RS** enthält.  
  
     ![rs_nativemode_uninstall_selectinstance](../../sql-server/install/media/rs-nativemode-uninstall-selectinstance.gif "rs_nativemode_uninstall_selectinstance")  
  
4.  Wählen Sie die [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Funktion aus, nachdem Sie die Instanz ausgewählt haben.  
  
     ![rs_nativemode_uninstall_selectfeatures](../../sql-server/install/media/rs-nativemode-uninstall-selectfeatures.gif "rs_nativemode_uninstall_selectfeatures")  
  
5.  Schließen Sie den Assistenten ab.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Vorgehensweise: Deinstallieren einer vorhandenen SQL Server-Instanz &#40;Setup&#41;](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md)   
 [Installieren oder Deinstallieren des PowerPivot für SharePoint-Add-Ins &#40;SharePoint 2013&#41;](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013)   
 [Installieren oder Deinstallieren des Reporting Services-Add-Ins für SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)  
  
