---
description: Ordinal-Eigenschaft (ADO MD Cell)
title: Ordinal-Eigenschaft (ADO MD Zelle) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cell::Ordinal
- Ordinal
helpviewer_keywords:
- Ordinal property [ADO MD]
ms.assetid: a6001168-b954-47f0-ba0d-c05c4cc40c58
author: rothja
ms.author: jroth
ms.openlocfilehash: caadd690a43683b4e31ae73b99a7f24217fb8529
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164456"
---
# <a name="ordinal-property-ado-md-cell"></a>Ordinal-Eigenschaft (ADO MD Cell)
Identifiziert eine [Zelle](./cell-object-ado-md.md) eindeutig anhand ihrer Position in einem CellSet.  
  
## <a name="return-values"></a>Rückgabewerte  
 Gibt eine **lange** ganze Zahl zurück und ist schreibgeschützt.  
  
## <a name="remarks"></a>Bemerkungen  
 Der Ordinalwert der Zelle identifiziert die Zelle innerhalb eines Cellsets eindeutig. Konzeptionell werden Zellen in einem CellSet nummeriert, als ob das Cellset ein *p*-dimensionales Array wäre, wobei *p* für die Anzahl der [Achsen](./axes-collection-ado-md.md)steht. Zellen werden beginnend mit 0 (null) in der Reihenfolge der Reihenfolge nummeriert. Dies ist die Formel zum Berechnen der Ordinalzahl einer Zelle:  
  
 Der Ordinalwert der Zelle kann mit der [Item](./item-property-ado-md-cellset.md) -Eigenschaft des [Cellset](./cellset-object-ado-md.md) -Objekts verwendet werden, um die [Zelle](./cell-object-ado-md.md)schnell abzurufen.  
  
## <a name="applies-to"></a>Gilt für  
 [Cell-Objekt (ADO MD)](./cell-object-ado-md.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Achsen Beispiel (VBScript)](./axis-example-vbscript.md)   
 [CellSet-Objekt (ADO MD)](./cellset-object-ado-md.md)   
 [Item-Eigenschaft (ADO MD Cellset)](./item-property-ado-md-cellset.md)   
 [Ordinal-Eigenschaft (ADO MD Position)](./ordinal-property-ado-md-position.md)