---
title: Hinzufügen eines externen Bilds (Berichts-Generator) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie im Berichts-Generator in Ihrem Bericht ein Bild aus einer externen Quelle mit entsprechender Überprüfung und den entsprechenden Berechtigungen hinzufügen.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 81fd4a1f-79a9-4967-86d6-6229413c0995
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b61f3a4fea633045ba064a79243a6e6c7f66149a
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91934808"
---
# <a name="add-an-external-image-report-builder-and-ssrs"></a>Hinzufügen eines externen Bilds (Berichts-Generator und SSRS)
  Externe Bilder können auf einem Berichtsserver im einheitlichen Modus bzw. im integrierten SharePoint-Modus oder auf einer beliebigen anderen Website gespeichert sein. Wenn Sie externe Bilder im Bericht verwenden, müssen Sie prüfen, ob das Bild vorhanden ist und ob der Leser des Berichts Zugriffsberechtigungen für das Bild hat. Weitere Informationen finden Sie unter [Bilder &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/images-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-add-an-external-image"></a>So fügen Sie ein externes Bild hinzu  
  
1.  Klicken Sie in der Berichtsentwurfsansicht auf der Registerkarte **Einfügen** auf **Bild**.  
  
2.  Klicken Sie auf die Entwurfsoberfläche, und ziehen Sie ein Feld auf die gewünschte Größe des Bilds.  
  
3.  Geben Sie im Dialogfeld **Bildeigenschaften** auf der Registerkarte **Allgemein** einen Namen im Textfeld **Name** ein, oder übernehmen Sie den Standardnamen.  
  
4.  (Optional) Geben Sie im Textfeld **QuickInfo** Text ein, der angezeigt wird, wenn der Benutzer in einem in HTML gerenderten Bericht auf das Bild zeigt.  
  
5.  Wählen Sie unter **Bildquelle auswählen**die Option **Extern**aus.  
  
     Wenn ein Bild auf einem Berichtsserver im einheitlichen Modus gespeichert ist, geben Sie im Feld **Dieses Bild verwenden** einen relativen Pfad zu dem Bild ein, z.B. „../images/image1.jpg“.  
  
     Wenn Sie ein Bild auf einem Berichtsserver im integrierten SharePoint-Modus oder auf einer beliebigen anderen Website verwenden möchten, geben Sie in das Feld **Dieses Bild verwenden** eine vollständige URL zum Bild ein, z. B. „https://\<SharePointservername>/\<site>/Documents/images/image1.jpg“.  
  
     Weitere Informationen finden Sie unter [Angeben von Pfaden zu externen Elementen &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/specifying-paths-to-external-items-report-builder-and-ssrs.md).  
  
6.  (Optional) Klicken Sie auf **Größe**, **Sichtbarkeit**, **Aktion**oder **Rahmen** , um weitere Eigenschaften für das Bildberichtselement festzulegen.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a>Weitere Informationen  
 [Einbetten eines Bilds in einen Bericht &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/embed-an-image-in-a-report-report-builder-and-ssrs.md)   
 [Hinzufügen eines Hintergrundbilds (Berichts-Generator und SSRS)](../../reporting-services/report-design/add-a-background-image-report-builder-and-ssrs.md)   
 [Bildeigenschaften (Dialogfeld), Allgemein (Berichts-Generator und SSRS)](./images-report-builder-and-ssrs.md)  
  
