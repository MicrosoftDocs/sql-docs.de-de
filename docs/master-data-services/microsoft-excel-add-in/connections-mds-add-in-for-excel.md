---
title: Verbindungen
description: Zum Herunterladen von Daten in die Master Data Services-Add-in für Excel erstellen Sie zunächst eine Verbindung. Jedes Mal, wenn Sie Excel starten, müssen Sie eine Verbindung mit einem Repository herstellen.
ms.custom: microsoft-excel-add-in
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 2f2b2f9d-7744-460e-83cd-56d34ea70ff0
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: dfdbfc96b3a865aa04fdb4ae22406f0b37ea99ca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272431"
---
# <a name="connections-mds-add-in-for-excel"></a>Verbindungen (MDS-Add-I für Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Um Daten in den [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]herunterzuladen, müssen Sie zuerst eine Verbindung herstellen. Eine Verbindung ist, wie der [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Webdienst weiß, mit welcher MDS-Datenbank eine Verbindung hergestellt werden soll.  
  
 Die Verbindungszeichenfolge ist normalerweise die URL der [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]-Webanwendung, z.B. `https://contoso/mds`.  
  
 Jedes Mal, wenn Sie Excel starten, müssen Sie eine Verbindung mit einem MDS-Repository herstellen. Die einzige Ausnahme ist, wenn das aktive Arbeitsblatt bereits von MDS verwaltete Daten enthält. In diesem Fall wird jedes Mal automatisch eine Verbindung hergestellt, wenn Sie Daten im Blatt aktualisieren oder veröffentlichen.  
  
 Sie können mehrere Verbindungen herstellen. Die Verbindung, auf die zuletzt zugegriffen wurde, wird als Standard angesehen.  
  
 Es können gleichzeitig mehrere Benutzer verbunden werden. Konflikte können jedoch entstehen, wenn mehrere Benutzer versuchen, die gleichen Daten zu veröffentlichen. Weitere Informationen finden Sie unter [Übersicht: Importieren von Daten aus Excel &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md).  
  
## <a name="connect-automatically-and-load-frequently-used-data"></a>Herstellen einer automatischen Verbindung und Laden von häufig verwendeten Daten  
 Wenn Sie immer eine Verbindung mit dem gleichen Server herstellen und den gleichen Satz Daten laden möchten, können Sie Shortcutabfragedateien erstellen, die Verbindungs- und Filterinformationen enthalten. Weitere Informationen zu Abfragedateien finden Sie unter [Shortcutabfragedateien &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md).  
  
## <a name="data-quality-services"></a>Data Quality Services  
 Der [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)] hat die Data Quality Services-Funktionalität, mit der Sie Daten vor dem Veröffentlichen im MDS-Repository zuordnen können. Wenn Sie eine Verbindung herstellen und eine DQS-Datenbank auf der gleichen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wie die MDS-Datenbank installiert wird, können Sie DQS-Schaltflächen im Menüband anzeigen. Wenn die Datenbank DQS_Main nicht in der Instanz vorhanden ist, werden diese Schaltflächen nicht angezeigt und die Datenqualitätsfunktionalität ist nicht verfügbar.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Taskbeschreibung|Thema|  
|----------------------|-----------|  
|Herstellen einer Verbindung mit einer [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Datenbank|[Herstellen einer Verbindung mit einem MDS-Repository &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/connect-to-an-mds-repository-mds-add-in-for-excel.md)|  
|Laden Sie MDS-Daten in Excel.|[Export Data to Excel from Master Data Services](../../master-data-services/microsoft-excel-add-in/export-data-to-excel-from-master-data-services.md)|  
|Filtern Sie MDS-Daten, bevor Sie sie in Excel laden.|[Filter Data before Exporting &#40;MDS Add-in for Excel&#41; (Filtern von Daten vor dem Exportieren (MDS-Add-In für Excel))](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md)|  
  
## <a name="related-content"></a>Verwandte Inhalte  
  
-   [Overview: Exporting Data to Excel &#40;MDS Add-in for Excel&#41; (Übersicht: Exportieren von Daten nach Excel (MDS-Add-in für Excel))](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)  
  
-   [Shortcutabfragedateien &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md)  
  
-   [Master Data Services-Add-In für Microsoft Excel](../../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md)  
  
  
