---
description: DefinedSize-Eigenschaft
title: DefinedSize-Eigenschaft | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::DefinedSize
helpviewer_keywords:
- DefinedSize property [ADO]
ms.assetid: 3ee27314-a305-4fbc-8433-9ee9a909afd6
author: rothja
ms.author: jroth
ms.openlocfilehash: 756fe40bf7916dfa3e56a3e559be2874b5e8cd85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171306"
---
# <a name="definedsize-property"></a>DefinedSize-Eigenschaft
Gibt die Datenkapazität eines [Feld](../../../ado/reference/ado-api/field-object.md) Objekts an.  
  
## <a name="return-value"></a>Rückgabewert  
 Gibt einen **Long** -Wert zurück, der die definierte Größe eines Felds widerspiegelt, das vom Datentyp des Feld Objekts abhängt. Weitere Informationen finden Sie unter [Type](../../../ado/reference/ado-api/type-property-ado.md) . Bei einem Feld, das einen Datentyp mit fester Länge verwendet, entspricht der Rückgabewert der Größe des Datentyps in Bytes. Für ein Feld, das einen Datentyp variabler Länge verwendet, ist dies einer der folgenden:  
  
1.  Die maximale Länge des Felds in Zeichen (für " **adVarChar** " und " **adVarWchar**") oder in Bytes (für " **adVarBinary**" und " **advarnumeric**"), wenn das Feld eine definierte Länge aufweist. Beispielsweise hat das Feld **adVarChar (5)** eine maximale Länge von 5.  
  
2.  Die maximale Länge des Datentyps in Zeichen (für **adchar** und **adwchar**) oder in Bytes (für **adbinary** und **adNumeric**), wenn das Feld keine definierte Länge besitzt.  
  
3.  ~ 0 (bitweiser, der Wert ist nicht 0; alle Bits werden auf 1 festgelegt), wenn weder das Feld noch der Datentyp über eine definierte maximale Länge verfügt.  
  
4.  Für Datentypen, die keine Länge aufweisen, wird dieser Wert auf ~ 0 festgelegt (bitweiser Wert, der Wert ist nicht 0; alle Bits werden auf 1 festgelegt).  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **DefinedSize** -Eigenschaft, um die Datenkapazität eines **Feld** Objekts zu bestimmen.  
  
 Die **DefinedSize-Eigenschaft** und die [ActualSize](../../../ado/reference/ado-api/actualsize-property-ado.md) -Eigenschaft sind unterschiedlich. Nehmen Sie beispielsweise ein **Feld** Objekt mit einem deklarierten Typ von **adVarChar** und einen **DefinedSize** -Eigenschafts Wert von 50, der ein einzelnes Zeichen enthält. Der Wert der **ActualSize** -Eigenschaft, der zurückgegeben wird, ist die Länge des einzelnen Zeichens in Byte.  
  
## <a name="applies-to"></a>Gilt für  
 [Field-Objekt](../../../ado/reference/ado-api/field-object.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für ActualSize und DefinedSize-Eigenschaften (VB)](../../../ado/reference/ado-api/actualsize-and-definedsize-properties-example-vb.md)   
 [Beispiel für ActualSize und DefinedSize-Eigenschaften (VC + +)](../../../ado/reference/ado-api/actualsize-and-definedsize-properties-example-vc.md)   
 [ActualSize-Eigenschaft (ADO)](../../../ado/reference/ado-api/actualsize-property-ado.md)
