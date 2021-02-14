---
description: Filtern (Dialogfeld, MDS-Add-In für Excel)
title: Filtern (Dialogfeld)
ms.custom: microsoft-excel-add-in
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: b987b141-5abf-4161-a073-4cfc3e7f5aae
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e650bd924e893b21cbc6b154887d9cd6e66565b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352771"
---
# <a name="filter-dialog-box-mds-add-in-for-excel"></a>Filtern (Dialogfeld, MDS-Add-In für Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Verwenden Sie in [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]das Dialogfeld **Filtern** , um die Liste der von MDS verwalteten Daten vor dem Laden in Excel einzugrenzen.  
  
 Dieses Dialogfeld enthält drei Abschnitte: **Spalten**, **Zeilen** und **Zusammenfassung**.  
  
## <a name="columns"></a>Spalten  
 Verwenden Sie den Abschnitt **Spalten** , um zu bestimmen, welche Attribute (Spalten) Sie in Excel anzeigen möchten.  
  
|Name des Steuerelements|BESCHREIBUNG|  
|------------------|-----------------|  
|Attributtyp|Ein Attributtyp beschreibt den Typ von Elementen, mit dem Sie arbeiten möchten. In den meisten Fällen lautet dieser **Blatt**. Weitere Informationen zu Elementtypen finden Sie unter [Elemente &#40;Master Data Services&#41;](../../master-data-services/members-master-data-services.md).|  
|Explizite Hierarchie|Wenn Sie den Attributtyp **Konsolidiert** ausgewählt haben, wählen Sie die Hierarchie aus, zu der die konsolidierten Elemente gehören. Weitere Informationen finden Sie unter [Explizite Hierarchien &#40;Master Data Services&#41;](../../master-data-services/explicit-hierarchies-master-data-services.md).|  
|Attributgruppen|Attributgruppen sind eine Art, Teilmengen von Attributen zu gruppieren. Wählen Sie eine Attributgruppe aus, wenn Sie eine Teilmenge verfügbarer Attribute anzeigen möchten. Weitere Informationen zu Attributgruppen finden Sie unter [Attributgruppen &#40;Master Data Services&#41;](../../master-data-services/attribute-groups-master-data-services.md).|  
|Alles auswählen|Klicken Sie, um alle in der Liste angezeigten Attribute auszuwählen.|  
|Auswahl aufheben|Klicken Sie, um die in der Liste angezeigten ausgewählten Attribute zu löschen.<br /><br /> Sie können weder **Name** noch **Code** löschen.|  
|Pfeil nach oben/Pfeil nach unten|Klicken Sie auf diese Schaltfläche, um das ausgewählte Attribut in der Liste nach oben oder unten zu verschieben. Die Reihenfolge von oben nach unten entspricht der Reihenfolge von links nach rechts, in der die Spalten im Arbeitsblatt angezeigt werden.|  
  
## <a name="rows"></a>Zeilen  
 Verwenden Sie den Abschnitt **Zeilen** , um zu bestimmen, welche Elemente (Zeilen) in Excel angezeigt werden sollen. Dafür definieren Sie Kriterien, mit denen Sie die Zeilen filtern, die angezeigt werden sollen.  
  
|Name des Steuerelements|BESCHREIBUNG|  
|------------------|-----------------|  
|attribute|Zeigt ein Attribut an, nach dem Sie filtern möchten. Wenn keine Attribute aufgeführt sind, liegt dies daran, dass sie nicht hinzugefügt wurden.<br /><br /> Hinweis: Sie können nach Attributen filtern, die nicht im Arbeitsblatt angezeigt werden sollen.|  
|Betreiber|Zeigt Operatoren an, die dem ausgewählten Typ von Attribut entsprechen. Weitere Informationen finden Sie unter [Filteroperatoren &#40;Master Data Services&#41;](../../master-data-services/filter-operators-master-data-services.md).|  
|Kriterien|Die Kriterien, nach denen Sie filtern möchten.|  
|Aktualisieren der Zusammenfassung|Wenn Sie mit großen Datasets arbeiten, klicken Sie, um den Abschnitt **Zusammenfassung** mit Details der zu ladenden Datenmenge zu aktualisieren.|  
|Hinzufügen|Wenn Sie im Abschnitt **Spalten** auf ein Attribut und dann auf **Hinzufügen** klicken, wird der Liste der Filter ein Attribut hinzugefügt.|  
|Alle entfernen|Entfernt alle Filter aus der Liste.|  
|Remove (Entfernen)|Entfernt den ausgewählten Filter aus der Liste.|  
  
## <a name="summary"></a>Zusammenfassung  
 Verwenden Sie den Abschnitt **Zusammenfassung** , um Details zur Datenmenge, die geladen werden soll, vor dem Laden anzuzeigen.  
  
|Name des Steuerelements|BESCHREIBUNG|  
|------------------|-----------------|  
|Modell|Der Name des Modells.|  
|Version|Der Name der Version.|  
|Entität|Der Name der Entität.|  
|Zeilen|Die Anzahl der Zeilen, die in Excel geladen werden, basierend auf den im Abschnitt **Zeilen** angewendeten Filtern.|  
|Spalten|Die Anzahl der Spalten, die in Excel geladen werden, basierend auf den im Abschnitt **Spalten** ausgewählten Attributen.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Daten vor dem Exportieren &#40;MDS-Add-in für Excel Filtern&#41;](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md)   
 [Overview: Exporting Data to Excel &#40;MDS Add-in for Excel&#41; (Übersicht: Exportieren von Daten nach Excel (MDS-Add-in für Excel))](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)  
  
  
