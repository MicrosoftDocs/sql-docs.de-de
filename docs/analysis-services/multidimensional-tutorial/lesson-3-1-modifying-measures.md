---
title: Ändern von Measures | Microsoft-Dokumentation
ms.date: 05/06/2019
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9a9d0559a0421d8badd5e939a85947a53e0f6e8b
ms.sourcegitcommit: 54c8420b62269f6a9e648378b15127b5b5f979c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2019
ms.locfileid: "65403952"
---
# <a name="lesson-3-1---modifying-measures"></a>Lektion 3-1: Ändern von Measures
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]

Sie können die **FormatString** -Eigenschaft verwenden, um Formatierungseinstellungen zu definieren, die die Darstellung von Measures steuern. In dieser Aufgabe geben Sie Formatierungseigenschaften für die Währungs- und Prozentsatzmeasures im [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Tutorial-Cube an.  
  
### <a name="to-modify-the-measures-of-the-cube"></a>So ändern Sie die Measures des Cubes  
  
1.  Wechseln Sie zur Registerkarte **Cubestruktur** des Cube-Designers für den [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Tutorial-Cube, erweitern Sie die **Internet Sales** -Measuregruppe im Bereich **Measures** , klicken Sie mit der rechten Maustaste auf **Order Quantity**, und klicken Sie anschließend auf **Eigenschaften**.  
  
2.  Klicken Sie im Eigenschaftenfenster auf das Pushpin-Symbol **Automatisch im Hintergrund** , um das Eigenschaftenfenster offen zu halten.  
  
    Das Ändern von Eigenschaften für eine Reihe von Elementen ist einfacher, wenn das Eigenschaftenfenster geöffnet bleibt.  
  
3.  Klicken Sie im Eigenschaftenfenster auf die Liste **FormatString** , und geben Sie anschließend **#,#** ein.  
  
4.  Klicken Sie auf der Symbolleiste der Registerkarte **Cubestruktur** auf der linken Seite auf das Symbol **Measuresraster anzeigen** .  
  
    In der Rasteransicht können Sie mehrere Measures gleichzeitig auswählen.  
  
5.  Wählen Sie eine der folgenden Measures aus. Um mehrere Measures auszuwählen, halten Sie beim Klicken die STRG-TASTE gedrückt:  
  
    -   **Unit Price**  
  
    -   **Extended Amount**  
  
    -   **Discount Amount**  
  
    -   **Product Standard Cost**  
  
    -   **Total Product Cost**  
  
    -   **Sales Amount**  
  
    -   **Tax Amt**  
  
    -   **Freight**  
  
6.  Wählen Sie im Eigenschaftenfenster in der **FormatString** -Liste **Currency**aus.  
  
7.  Wählen Sie im Dropdown-Listenfeld oben im Eigenschaftenfenster (rechts unterhalb der Titelleiste) das Measure **Unit Price Discount Pct**und anschließend in der Liste **FormatString** die Option **Percent** aus.  
  
8.  Ändern Sie im Eigenschaftenfenster die **Name** -Eigenschaft für das **Unit Price Discount Pct** -Measure zu **Unit Price Discount Percentage**.  
  
9. Klicken Sie im Bereich **Measures** auf **Tax Amt** , und ändern Sie den Namen dieses Measures in **Tax Amount**.  
  
10. Klicken Sie im Eigenschaftenfenster auf das Symbol **Automatisch im Hintergrund** , um das Eigenschaftenfenster auszublenden, und anschließend auf der Symbolleiste der Registerkarte **Cubestruktur** auf **Measuresstruktur anzeigen** .  
  
11. Klicken Sie im Menü **Datei** auf **Alle speichern**.  
  
## <a name="next-task-in-lesson"></a>Nächste Aufgabe in dieser Lektion  
[Ändern der Customer-Dimension](lesson-3-2-modifying-the-customer-dimension.md)  
  
## <a name="see-also"></a>Siehe auch  
[Definieren von Datenbankdimensionen](../multidimensional-models/define-database-dimensions.md)  
[Konfigurieren von Measureeigenschaften](../multidimensional-models/configure-measure-properties.md)  
  
  
  