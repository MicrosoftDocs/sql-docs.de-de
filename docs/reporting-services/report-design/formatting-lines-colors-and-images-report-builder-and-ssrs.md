---
title: Formatieren von Linien, Farben und Bildern (Berichts-Generator) | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Linien, Farben, Datenbereiche und Bilder im Berichts-Generator verwenden. Zudem lernen Sie, wie Sie Elemente visuell verknüpfen, um die Lesbarkeit im Berichts-Generator zu erhöhen.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.textboxproperties.border.f1
- "10502"
- "10094"
- sql13.rtp.rptdesigner.shared.borderdv.f1
- sql13.rtp.rptdesigner.reportbody.border.f1
- "10063"
- sql13.rtp.rptdesigner.pictureproperties.border.f1
- sql13.rtp.rptdesigner.rectangleproperties.border.f1
- "10055"
- "10126"
- "10066"
- sql13.rtp.rptdesigner.subreportproperties.border.f1
ms.assetid: 0f5f0d2a-9537-4152-b441-b40d7f04cf4c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f807837cda829828b20be8e33a17530cb332aa28
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91935042"
---
# <a name="formatting-lines-colors-and-images-report-builder-and-ssrs"></a>Formatieren von Linien, Farben und Bildern (Berichts-Generator und SSRS)
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] können Sie Linien, Farben, Datenbereiche, Bilder und andere Berichtselemente formatieren.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="borders-lines-and-gridlines"></a>Rahmen, Linien und Gitternetzlinien  
 Mithilfe von Rahmen, Linien und Gitternetzlinien können Elemente auf einer Seite verbunden und angeordnet werden, damit Leser den Inhalt des Berichts besser erfassen können. Mithilfe der vordefinierten Rahmenstile können Sie einem Textfeld, einer Gruppe von Textfeldern oder einem Bild schnell einen Rahmen hinzufügen. Außerdem können Sie für die Rahmen, Linien und Gitternetzlinien den Stil, die Breite und die Farbe ändern. Rahmen werden um ein gesamtes ausgewähltes Element oder entlang der Umrandung eines Elements hinzugefügt, zum Beispiel an der Unterseite eines Textfelds.  
  
 Verwenden Sie die Registerkarte **Rahmen** im Dialogfeld **Eigenschaften** des Berichtselements, um Rahmen und Gitternetzlinien in einem Textfeld, Berichtslayout oder als Bildeinfassung zu formatieren. Wenn Sie z.B. einen Rahmen für ein Bild hinzufügen möchten, klicken Sie mit der rechten Maustaste auf das Bild, und klicken Sie anschließend im Dialogfeld **Bildeigenschaften** auf **Rahmen**.  
  
 Neben den Standardrahmen können auch zusätzliche Rahmen für Diagramme verwendet werden. Weitere Informationen finden Sie unter [Hinzufügen eines Rahmens zu einem Diagramm (Berichts-Generator und SSRS)](../../reporting-services/report-design/add-a-border-frame-to-a-chart-report-builder-and-ssrs.md).  
  
 Sie können auch dem Bericht selbst einen Rahmen hinzufügen. Weitere Informationen finden Sie unter [Hinzufügen eines Rahmens zu einem Bericht &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/add-a-border-to-a-report-report-builder-and-ssrs.md).  
  
## <a name="applying-background-colors"></a>Anwenden von Hintergrundfarben  
 Sie können dem Hintergrund eines gesamten Berichts, eines Textfelds im Bericht oder einer Zelle oder Gruppe von Zellen in einem Datenbereich eine Volltonfarbe hinzufügen. Die Hintergrundfarbe ist standardmäßig Weiß. Sie können jedoch auf der Registerkarte **Ausfüllen** im Dialogfeld **Eigenschaften** des Berichtselements eine andere Farbe auswählen. Wenn Sie z.B. die Hintergrundfarbe eines Textfelds ändern möchten, klicken Sie mit der rechten Maustaste auf das Textfeld, und wählen Sie **Textfeldeigenschaften**aus. Klicken Sie auf **Ausfüllen** , und wählen Sie die dann die gewünschte Farbe aus. In diesem Dialogfeld können Sie für das ausgewählte Element eine Hintergrundfarbe festlegen, oder Sie können ein Bild hinzufügen, das im Hintergrund angezeigt wird.  
  
 Wenn Sie das Diagramm verwenden, können Sie für Hintergrundfarben auch Farbverläufe und Musterarten angeben. Weitere Informationen finden Sie unter [Formatieren eines Diagramms (Berichts-Generator und SSRS)](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md).  
  
## <a name="using-images-as-formatting"></a>Verwenden von Bildern als Formatierung  
 Sie können Felder, die Bilder enthalten, einem Datenbereich hinzufügen. Wenn Sie ein Bildfeld verwenden, werden die Bilder im Bericht beim Ausführen des Berichts angezeigt.  
  
 Sie können dem Hintergrund des Berichts oder einem Rechteck, einem Textfeld, einer Tabelle, einer Matrix, einigen Teilen eines Diagramms oder den Text- und Seitenabschnitten eines Berichts auch Bilder hinzufügen, z. B. Logos. Weitere Informationen finden Sie unter [Bilder &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/images-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Formatieren von Text und Platzhaltern &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/formatting-text-and-placeholders-report-builder-and-ssrs.md)   
 [Formatieren von Zahlen und Datumsangaben &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/formatting-numbers-and-dates-report-builder-and-ssrs.md)   
 [Formatieren von Text in einem Textfeld (Berichts-Generator und SSRS)](../../reporting-services/report-design/format-text-in-a-text-box-report-builder-and-ssrs.md)   
 [Ausfüllen (Dialogfeld) (Berichts-Generator und SSRS)](/previous-versions/sql/)  
  
