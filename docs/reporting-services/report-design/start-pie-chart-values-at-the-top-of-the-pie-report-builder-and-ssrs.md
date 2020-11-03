---
title: Beginnen der Kreisdiagrammwerte an der Kreisoberseite (Berichts-Generator) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Kreisdiagrammwerte oben im Diagramm anstatt bei 90 Grad zur Oberseite beginnen.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: d0e6fb59-ca4e-4d70-97cb-0ad183da21d3
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b1158cf4a88bb491b8ed1cb492eec1c3021cb6f9
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907058"
---
# <a name="start-pie-chart-values-at-the-top-of-the-pie-report-builder-and-ssrs"></a>Beginnen der Kreisdiagrammwerte an der Kreisoberseite (Berichts-Generator und SSRS)
Standardmäßig beginnt bei Kreisdiagrammen in paginierten [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] -Berichten der erste Wert im Dataset bei 90 Grad zur Oberseite des Kreises versetzt. 

![Berichts-Generator: Screenshot eines Kreisdiagramms, dessen Dataset bei 90 Grad beginnend angezeigt wird](../../reporting-services/media/report-builder-pie-chart-start-at-90.png)

*Die Diagrammwerte beginnen bei 90 Grad.*

Sie können den ersten Wert aber auch ganz oben anordnen. 

![Berichts-Generator: Screenshot eines Kreisdiagramms, dessen Dataset bei null Grad beginnend angezeigt wird](../../reporting-services/media/report-builder-pie-chart-start-at-top.png)

*Die Diagrammwerte beginnen an der Kreisoberseite.*
  
## <a name="to-start-the-pie-chart-at-the-top-of-the-pie"></a>So beginnen Sie das Kreisdiagramm an der Kreisoberseite  
  
1.  Klicken Sie auf den Kreis selbst.  
  
2.  Zum Anzeigen des Bereichs **Eigenschaften** klicken Sie auf der Registerkarte **Ansicht** auf **Eigenschaften**.  
  
3.  Ändern Sie im Bereich **Eigenschaften** unter **Benutzerdefinierte Attribute** den Eintrag **PieStartAngle** von **0** in **270**.  
  
4.  Klicken Sie auf **Ausführen** , um den Bericht in der Vorschau anzuzeigen.  
  
 Der erste Wert beginnt jetzt an der Kreisdiagrammoberseite.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Formatieren eines Diagramms &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [Kreisdiagramme &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/pie-charts-report-builder-and-ssrs.md)  
  
  
