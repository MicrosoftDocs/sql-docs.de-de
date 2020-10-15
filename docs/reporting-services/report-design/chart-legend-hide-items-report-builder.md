---
title: Ausblenden von Legendenelementen im Diagramm (Berichts-Generator) | Microsoft-Dokumentation
description: Hier entdecken Sie, wie Sie Elemente für die Legende zum Anzeigen der wichtigen Daten im Berichts-Generator auswählen.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 92256240-0cd5-4be4-8904-d1e3b93cb6b3
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: cf51a3b08b8296ddfd2e7bf470d36d575485888b
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91935309"
---
# <a name="chart-legend---hide-items-report-builder"></a>Diagrammlegende: Ausblenden von Elementen (Berichts-Generator)
Standardmäßig werden alle in einem paginierten [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Bericht einem Nicht-Formdiagramm hinzugefügten Reihen in der Legende als Element hinzugefügt. Für Kreis-, Ring-, Trichter- und Pyramidendiagramme werden für alle dem Diagramm hinzugefügten Reihen der Legende einzelne Datenpunkte hinzugefügt.  
  
 Sie können in der Legende beliebige Elemente ausblenden. Wenn Sie ein Legendenelement ausblenden, wird dieses immer noch im Diagramm angezeigt. Dies ist hilfreich in Situationen, in denen für eine dem Diagramm hinzugefügte Reihe keine weiteren Informationen angezeigt werden sollen. Wenn Sie dem Diagramm beispielsweise eine berechnete Reihe (z. B. einen gleitenden Durchschnitt) hinzugefügt haben, empfiehlt es sich u. U., diesen Eintrag in der Legende auszublenden, sodass nur für die ursprüngliche Reihe weitere Informationen angezeigt werden.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-hide-an-item-from-display-in-the-legend"></a>So blenden Sie ein in der Legende angezeigtes Element aus  
  
1.  Klicken Sie mit der rechten Maustaste auf die auszublendende Reihe, und wählen Sie die Option **Reiheneigenschaften**aus.  
  
2.  Klicken Sie auf **Legende**. Aktivieren Sie die Option **Diese Reihe in Legende nicht anzeigen** .  
  
    > [!NOTE]  
    >  Eine Reihe kann nicht für eine Gruppe ausgeblendet und für andere angezeigt werden. Wenn Sie dem Bereich **Reihengruppen** ein Feld hinzugefügt haben, werden alle zu dieser Gruppe gehörenden Reihen ausgeblendet.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Formatieren der Legende in einem Diagramm &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/chart-legend-formatting-report-builder.md)   
 [Formatieren von Datenpunkten in einem Diagramm &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/formatting-data-points-on-a-chart-report-builder-and-ssrs.md)   
 [Ändern des Texts eines Legendenelements (Berichts-Generator und SSRS)](../../reporting-services/report-design/chart-legend-change-item-text-report-builder.md)   
 [Hinzufügen eines gleitenden Durchschnitts zu einem Diagramm (Berichts-Generator und SSRS)](../../reporting-services/report-design/add-a-moving-average-to-a-chart-report-builder-and-ssrs.md)   
 [Legendeneigenschaften (Dialogfeld), Allgemein (Berichts-Generator und SSRS)](./chart-legend-formatting-report-builder.md)  
  
