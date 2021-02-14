---
description: FilterAxis-Eigenschaft (ADO MD)
title: FilterAxis-Eigenschaft (ADO MD) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cellset::FilterAxis
- FilterAxis
helpviewer_keywords:
- FilterAxis property [ADO MD]
ms.assetid: 9c656963-531e-4cd1-b698-d5f42a9b7ba3
author: rothja
ms.author: jroth
ms.openlocfilehash: e53967bf577bfc69c07d7c1e5e49e42a3ca613d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054715"
---
# <a name="filteraxis-property-ado-md"></a>FilterAxis-Eigenschaft (ADO MD)
Gibt Filter Informationen zum aktuellen [Cellset](./cellset-object-ado-md.md)an.  
  
## <a name="return-values"></a>Rückgabewerte  
 Gibt ein [Achsen](./axis-object-ado-md.md) Objekt zurück und ist schreibgeschützt.  
  
## <a name="remarks"></a>Bemerkungen  
 Mit der **FilterAxis** -Eigenschaft geben Sie Informationen zu den Dimensionen zurück, die zum Segmentieren der Daten verwendet wurden. Die [DimensionCount](./dimensioncount-property-ado-md.md) -Eigenschaft der **Achse** gibt die Anzahl der Slicerdimensionen zurück. Diese Achse weist in der Regel nur eine Zeile auf.  
  
 Die von **FilterAxis** zurückgegebene **Achse** ist nicht in der [Achsen](./axes-collection-ado-md.md) Auflistung für ein [Cellset](./cellset-object-ado-md.md) -Objekt enthalten.  
  
## <a name="applies-to"></a>Gilt für  
 [Cellset-Objekt (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Axis-Objekt (ADO MD)](./axis-object-ado-md.md)   
 [Dimensions Objekt (ADO MD)](./dimension-object-ado-md.md)   
 [DimensionCount-Eigenschaft (ADO MD)](./dimensioncount-property-ado-md.md)