---
description: Aktualisieren von Daten (MDS-Add-In für Excel)
title: Aktualisieren von Daten
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 58dbe99a-288d-4f1c-9cd5-704d6836c945
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e0661048822933b8b49274d7addc40cec130e289
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343192"
---
# <a name="refreshing-data-mds-add-in-for-excel"></a>Aktualisieren von Daten (MDS-Add-In für Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Aktualisieren Sie in [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]Daten, wenn Sie die aktuellsten Informationen aus dem MDS-Repository ohne Öffnen eines neuen Arbeitsblatts abrufen möchten. Sie können entweder alle Zellen oder eine Auswahl von Zellen aktualisieren. Dies kann nützlich sein, wenn Sie Spalten mit benutzerdefinierten Formeln oder anderen Daten eingefügt haben, die nicht in MDS verwaltet werden, und Sie diese Daten beibehalten möchten.  
  
## <a name="when-you-can-refresh-mds-managed-data"></a>Wann Sie von MDS verwaltete Daten aktualisieren können  
 Sie können von MDS verwaltete Daten in einem aktiven Arbeitsblatt aktualisieren, wenn das aktive Arbeitsblatt bereits von MDS verwaltete Daten enthält. Wenn Sie Attributwerte geändert haben oder dem Arbeitsblatt Elemente hinzugefügt haben, müssen Sie die Änderungen veröffentlichen, bevor Sie eine Aktualisierung vornehmen können.  
  
## <a name="refreshing-a-selection"></a>Aktualisieren einer Auswahl  
 Sie können zwischen dem Aktualisieren aller Zellen und dem Aktualisieren ausgewählter Zellen wählen. Die ausgewählten Zellen müssen zusammenhängend sein. Wenn ein Satz zusammenhängender Zellen ausgewählt wird, werden alle von MDS verwalteten Zellen in diesem Satz so aktualisiert, um die derzeit auf dem Server gespeicherten Werte anzuzeigen. Unveränderte Zeilen und Spalten, die nicht von MDS verwaltet werden, sind davon nicht betroffen.  
  
## <a name="what-happens-when-you-refresh-mds-managed-data"></a>Was geschieht, wenn Sie von MDS verwaltete Daten aktualisieren  
 Wenn Sie Daten im [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]aktualisieren, richtet sich die weitere Verwendung der von MDS verwalteten Daten im Blatt danach, was sich im MDS-Repository seit dem letzten Laden der Daten geändert hat.  
  
-   Wenn dem Repository neue Elemente hinzugefügt wurden, werden sie am Ende des Arbeitsblatts hinzugefügt und grün hervorgehoben.  
  
-   Wenn Elemente aus dem Repository gelöscht wurden, werden sie aus dem Arbeitsblatt gelöscht.  
  
-   Wenn sich ein Attributwert im MDS-Repository geändert hat, wird der Wert im Arbeitsblatt mit dem Wert aus dem MDS-Repository aktualisiert. Die Zellenfarbe ändert sich nicht.  
  
> [!WARNING]
>  -   Wenn nicht verwaltete Daten im aktiven Arbeitsblatt in Zeilen unter den von MDS verwalteten Daten vorhanden sind, werden die nicht verwalteten Daten möglicherweise überschrieben. Dies ist der Fall, wenn Sie das Blatt und neue Zeilen mit von MDS verwalteten Daten, die mit den nicht verwalteten Daten überlappen, aktualisieren.  
> -   Wenn Sie eine Aktualisierung vornehmen, werden die Kommentare zu den von MDS verwalteten Zellen gelöscht.  
  
## <a name="how-to-refresh-mds-managed-data"></a>Vorgehensweise: Aktualisieren der von MDS verwalteten Daten  
 In der Gruppe **Verbinden und laden** auf dem Menüband hat die Schaltfläche **Aktualisieren** zwei Optionen: **Alle aktualisieren** und **Auswahl aktualisieren**. Die Standardaktion der Menübandschaltfläche lautet **Alle aktualisieren**. Um das ganze Blatt mit Werten vom Server zu aktualisieren, klicken Sie auf die Schaltfläche **Aktualisieren** , oder wählen Sie die Option **Alle aktualisieren** aus. Sie müssen zusammenhängende Zellen auswählen und die Option **Auswahl aktualisieren** auswählen, um nur einige der Zellen in einem Blatt zu aktualisieren.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Taskbeschreibung|Thema|  
|----------------------|-----------|  
|Herstellen einer Verbindung mit einer [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Datenbank|[Herstellen einer Verbindung mit einem MDS-Repository &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/connect-to-an-mds-repository-mds-add-in-for-excel.md)|  
|Laden Sie MDS-Daten in Excel.|[Export Data to Excel from Master Data Services](../../master-data-services/microsoft-excel-add-in/export-data-to-excel-from-master-data-services.md)|  
  
## <a name="related-content"></a>Verwandte Inhalte  
  
-   [Verbindungen &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/connections-mds-add-in-for-excel.md)  
  
-   [Overview: Exporting Data to Excel &#40;MDS Add-in for Excel&#41; (Übersicht: Exportieren von Daten nach Excel (MDS-Add-in für Excel))](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)  
  
-   [Master Data Services-Add-In für Microsoft Excel](../../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md)  
  
  
