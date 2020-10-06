---
description: Inhaltstypen (DMX)
title: Inhaltstypen (DMX) | Microsoft-Dokumentation
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: b1a400e5247cc812cb72ba3422cb361888342155
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91726326"
---
# <a name="content-types-dmx"></a>Inhaltstypen (DMX)
[!INCLUDE[ssas](../includes/applies-to-version/ssas.md)]

  Data Mining-Algorithmen benötigen über den Datentyp hinausgehende, zusätzliche Informationen, damit sie richtig ausgewertet werden können, z. B. den Inhaltstyp. Der Inhaltstyp unterstützt dabei, für den jeweiligen Algorithmus zu ermitteln, wie die Daten in der Spalte verarbeitet werden sollen.  
  
 Jeder Algorithmus unterstützt bestimmte Inhaltstypen. Beispielsweise kann der [!INCLUDE[msCoName](../includes/msconame-md.md)] Naive Bayes-Algorithmus keine kontinuierlichen Spalten verwenden. Wenn Sie eine kontinuierliche Spalte in einem [!INCLUDE[msCoName](../includes/msconame-md.md)] Naive Bayes-Modell verwenden möchten, müssen Sie die Daten in der Spalte diskretisieren. Für einige Algorithmen sind bestimmte Inhaltstypen erforderlich, damit sie richtig funktionieren. Beispielsweise ist für den [!INCLUDE[msCoName](../includes/msconame-md.md)] Time Series-Algorithmus eine Schlüsselzeitspalte erforderlich, um die Zeitspanne zu kennzeichnen, in der die Daten gesammelt wurden.  
  
 Eine umfassende Beschreibung der Inhaltstypen, die von [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] unterstützt werden, finden Sie unter [Content Types &#40;Data Mining&#41;](/analysis-services/data-mining/content-types-data-mining).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Data Mining-Algorithmen &#40;Analysis Services Data Mining-&#41;](/analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Referenz](../dmx/data-mining-extensions-dmx-reference.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Syntax Elemente](../dmx/data-mining-extensions-dmx-syntax-elements.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Funktionsreferenz](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Operator Verweis](../dmx/data-mining-extensions-dmx-operator-reference.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41;-Anweisungs Referenz](../dmx/data-mining-extensions-dmx-statements.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Syntax Konventionen](../dmx/data-mining-extensions-dmx-syntax-conventions.md)   
 [Allgemeine Vorhersagefunktionen &#40;DMX-&#41;](../dmx/general-prediction-functions-dmx.md)   
 [Struktur und Verwendung von DMX-Vorhersage Abfragen](../dmx/structure-and-usage-of-dmx-prediction-queries.md)   
 [Understanding the DMX Select Statement (Grundlegendes zur SELECT-Anweisung)](../dmx/understanding-the-dmx-select-statement.md)  
  
