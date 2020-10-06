---
description: Verteilungen (DMX)
title: Verteilungen (DMX) | Microsoft-Dokumentation
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 9deb3afaa6d0a4bc90281c2cc3998365eccb9838
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91726221"
---
# <a name="distributions-dmx"></a>Verteilungen (DMX)
[!INCLUDE[ssas](../includes/applies-to-version/ssas.md)]

  In [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] können Sie den Inhalt von Spalten in einer Mining Struktur definieren, um zu beeinflussen, wie Algorithmen die Daten in diesen Spalten verarbeiten, wenn Sie Mining Modelle erstellen. Für einige Algorithmen ist es hilfreich, vor dem Verarbeiten des Modells für jede kontinuierliche Spalte die Verteilung zu definieren, wenn für die Spalten bekannt ist, dass sie normal verteilte Werte enthalten. Wenn Sie die Verteilungen nicht definieren, liefern die sich ergebenden Miningmodelle möglicherweise ungenauere Vorhersagen, da die Algorithmen weniger Informationen zum Interpretieren der Daten haben.  
  
 [!INCLUDE[msCoName](../includes/msconame-md.md)] Data Mining-Algorithmen unterstützen die folgenden Verteilungstypen:  
  
 **Normaler**  
 Die Werte für die kontinuierliche Spalte bilden ein Histogramm, das einer gaußschen Normalverteilung folgt.  
  
 **Log Normal**  
 Die Werte für die kontinuierliche Spalte bilden ein Histogramm, bei dem der Logarithmus der Werte normal verteilt ist.  
  
 **Gleich**  
 Die Werte für die kontinuierliche Spalte bilden eine flache Kurve, in der alle Werte gleich wahrscheinlich sind.  
  
 Weitere Informationen zu [!INCLUDE[msCoName](../includes/msconame-md.md)] Data Mining Algorithmen finden Sie unter [Data Mining-Algorithmen &#40;Analysis Services Data Mining-&#41;](/analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining). Algorithmenanbieter von Drittanbietern unterstützen möglicherweise weitere Verteilungstypen. Verwenden Sie das **SUPPORTED_DISTRIBUTION_FLAGS** Schemarowset, um zu bestimmen, welche Verteilungs Typen von einem Algorithmus unterstützt werden.  
  
 Weitere Informationen zu Verteilungs Typen finden Sie unter [Spalten Verteilungen &#40;Data Mining-&#41;](/analysis-services/data-mining/column-distributions-data-mining).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Inhaltstypen &#40;Data Mining-&#41;](/analysis-services/data-mining/content-types-data-mining)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Referenz](../dmx/data-mining-extensions-dmx-reference.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Syntax Elemente](../dmx/data-mining-extensions-dmx-syntax-elements.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Funktionsreferenz](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Operator Verweis](../dmx/data-mining-extensions-dmx-operator-reference.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41;-Anweisungs Referenz](../dmx/data-mining-extensions-dmx-statements.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Syntax Konventionen](../dmx/data-mining-extensions-dmx-syntax-conventions.md)   
 [Allgemeine Vorhersagefunktionen &#40;DMX-&#41;](../dmx/general-prediction-functions-dmx.md)   
 [Struktur und Verwendung von DMX-Vorhersage Abfragen](../dmx/structure-and-usage-of-dmx-prediction-queries.md)   
 [Understanding the DMX Select Statement (Grundlegendes zur SELECT-Anweisung)](../dmx/understanding-the-dmx-select-statement.md)  
  
