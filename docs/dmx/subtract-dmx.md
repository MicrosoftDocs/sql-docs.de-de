---
description: '- Subtrahieren DMX'
title: '- Subtrahieren (DMX) | Microsoft-Dokumentation'
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: a7a89271417972f4df94a15544f770b9702bed45
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88500831"
---
# <a name="--subtract-dmx"></a>- (Subtraktion) (DMX)
[!INCLUDE[ssas](../includes/applies-to-version/ssas.md)]

  Führt eine arithmetische Operation aus, die eine Zahl von einer anderen subtrahiert.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
Numeric_Expression - Numeric_Expression  
```  
  
#### <a name="parameters"></a>Parameter  
 *Numeric_Expression*  
 Ein gültiger DMX-Ausdruck (Data Mining-Erweiterungen), der einen numerischen Wert zurückgibt.  
  
## <a name="return-value"></a>Rückgabewert  
 Ein Wert, der den Datentyp des Parameters aufweist, der in der Rangfolge höher eingestuft ist.  
  
## <a name="remarks"></a>Bemerkungen  
 Beide Ausdrücke müssen denselben Datentyp haben, oder es muss möglich sein, einen Ausdruck implizit in den Datentyp des anderen Ausdrucks zu konvertieren. Hat ein Ausdruck als Ergebnis den Wert NULL, gibt der Operator das Ergebnis des anderen Ausdrucks zurück.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Arithmetische Operatoren &#40;DMX-&#41;](../dmx/operators-arithmetic.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Operator Verweis](../dmx/data-mining-extensions-dmx-operator-reference.md)   
 [Operatoren &#40;DMX-&#41;](../dmx/operators-dmx.md)  
  
  
