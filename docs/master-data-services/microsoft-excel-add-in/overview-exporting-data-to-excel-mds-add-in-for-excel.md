---
description: 'Übersicht: Exportieren von Daten nach Excel (MDS-Add-in für Excel)'
title: Exportieren von Daten nach Excel
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: b628548b-982b-4e45-abf4-c8e83e3ab1c2
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ab060952626699683379dbbbca848f3af36c68e1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348874"
---
# <a name="overview-exporting-data-to-excel-mds-add-in-for-excel"></a>Übersicht: Exportieren von Daten nach Excel (MDS-Add-in für Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]müssen Sie Daten aus dem MDS-Repository in ein aktives Excel-Arbeitsblatt exportieren, bevor Sie damit arbeiten können. Wenn Sie die Daten nicht mehr für Ihre Arbeit benötigen, importieren Sie sie in das MDS-Repository, um sie für andere Benutzer freizugeben.  
  
 Die Datenmenge, die Sie exportieren können, sind auf die Daten beschränkt, für die Sie über eine Zugriffsberechtigung verfügen. Die Berechtigung, auf Daten zuzugreifen, wird in der [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Webanwendung oder programmgesteuert festgelegt.  
  
 Wenn Sie große Datenmengen exportieren, können Sie Warnungen festlegen, die angezeigt werden, wenn das Laden der Daten lange dauert. Klicken Sie hierzu in der Gruppe **Optionen** auf **Einstellungen**. Wählen Sie auf der Registerkarte **Daten** die Option **Filterwarnung für große Datasets anzeigen** aus.  
  
> [!WARNING]  
>  Eine MDS-aktivierte Arbeitsmappe darf nur in Excel mit dem MDS-Add-In für Excel geöffnet und aktualisiert werden. Das Öffnen der MDS-aktivierten Arbeitsmappe in Excel auf einem Computer, auf dem das MDS-Excel-Add-In nicht installiert ist, wird nicht unterstützt. Außerdem könnte dies die Arbeitsmappendatei beschädigen. Wenn Sie Daten für andere Personen freigeben möchten, empfiehlt es sich, ihnen eine Shortcutabfragedatei per E-Mail zu senden. Demgegenüber ist es nicht empfehlenswert die Arbeitsmappe nur zu speichern und sie per E-Mail zu versenden. Weitere Informationen über die Abfrage finden Sie unter [Senden einer Shortcutabfragedatei &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/email-a-shortcut-query-file-mds-add-in-for-excel.md).  
  
## <a name="filtering-data"></a>Filtern von Daten  
 Sie können Daten vor dem Exportieren filtern, um die Datenmenge einzuschränken, die Sie herunterladen werden. Dies schließt die Auswahl der zu ladenden Attribute (Spalten) ein, die Reihenfolge, in der die Attribute angezeigt werden sollen, und die Elemente (Datenzeilen), mit denen Sie arbeiten möchten. Weitere Informationen finden Sie unter [Filter Data before Exporting &#40;MDS Add-in for Excel&#41; (Filtern von Daten vor dem Exportieren (MDS-Add-In für Excel))](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md).  
  
## <a name="connect-automatically-and-load-frequently-used-data"></a>Herstellen einer automatischen Verbindung und Laden von häufig verwendeten Daten  
 Wenn Sie immer eine Verbindung mit dem gleichen Server herstellen und den gleichen Datensatz exportieren möchten, können Sie Shortcutabfragedateien erstellen, die Verbindungs- und Filterinformationen enthalten. Weitere Informationen zu Abfragedateien finden Sie unter [Shortcutabfragedateien &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md).  
  
## <a name="refreshing-data"></a>Aktualisieren von Daten  
 Daten im MDS-Repository werden möglicherweise von anderen Benutzern aktualisiert, nachdem Sie sie exportiert haben. Sie können diese Daten ohne Verlust von Änderungen, die Sie an nicht-MDS-Daten vorgenommen haben, abrufen. Weitere Informationen finden Sie unter [Aktualisieren von Daten &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/refreshing-data-mds-add-in-for-excel.md).  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Taskbeschreibung|Thema|  
|----------------------|-----------|  
|Filtern Sie MDS-Daten, bevor Sie sie in Excel laden.|[Filter Data before Exporting &#40;MDS Add-in for Excel&#41; (Filtern von Daten vor dem Exportieren (MDS-Add-In für Excel))](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md)|  
|Laden Sie MDS-Daten in Excel.|[Export Data to Excel from Master Data Services](../../master-data-services/microsoft-excel-add-in/export-data-to-excel-from-master-data-services.md)|  
|Ändern Sie die Reihenfolge der Spalten, bevor Sie Daten herunterladen.|[Neuanordnen von Spalten &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/reorder-columns-mds-add-in-for-excel.md)|  
  
## <a name="related-content"></a>Verwandte Inhalte  
  
-   [Verbindungen &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/connections-mds-add-in-for-excel.md)  
  
-   [Shortcutabfragedateien &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md)  
  
-   [Master Data Services-Add-In für Microsoft Excel](../../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md)  
  
-   [Sicherheit &#40;Master Data Services&#41;](../../master-data-services/security-master-data-services.md)  
  
  
