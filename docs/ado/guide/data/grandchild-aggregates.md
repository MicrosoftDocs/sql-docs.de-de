---
description: Untergeordnete Aggregate
title: Aggregierte untergeordnete Aggregate | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- grandchild aggregates [ADO]
- data shaping [ADO], grandchild aggregates
ms.assetid: 4162d35f-2ce1-4218-80a5-b6933348837e
author: rothja
ms.author: jroth
ms.openlocfilehash: 7215846215db30d31d9a7b54f8e3a3aa2a955a43
ms.sourcegitcommit: 33e774fbf48a432485c601541840905c21f613a0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88806794"
---
# <a name="grandchild-aggregates"></a>Untergeordnete Aggregate
Der in einer-Klausel eines Shape-Befehls erstellten Kapitel-Spalte kann ein *Kapitel-Alias-Name* zugewiesen werden (in der Regel mit dem As-Schlüsselwort). Sie können jede Spalte in einem beliebigen Kapitel des geformten **Recordsets** mit einem voll qualifizierten Namen identifizieren, der das untergeordnete Element identifiziert, das die Spalte enthält. Wenn das übergeordnete Kapitel chap1 beispielsweise ein untergeordnetes Kapitel chap2 enthält, das eine Spalte Amount (Amt) aufweist, lautet der qualifizierte Name chap1. chap2. Amt. Der qualifizierte Name kann dann als Argument für eine der Aggregatfunktionen (Sum, AVG, Max, min, count, StDev oder any) verwendet werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenstrukturierung – Beispiel](./data-shaping-example.md)