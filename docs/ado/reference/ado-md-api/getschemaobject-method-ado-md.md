---
description: GetSchemaObject-Modell (ADO MD)
title: Getschemaobject-Methode (ADO MD) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- GetSchemaObject
- Cellset::GetSchemaObject
helpviewer_keywords:
- GetSchemaObject method [ADO MD]
ms.assetid: 36b754b4-6b17-4dd1-a925-bca46938b7c4
author: rothja
ms.author: jroth
ms.openlocfilehash: 96b0a2b6b8fc0a8d87c51c68701a64c971255ec8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054625"
---
# <a name="getschemaobject-method-ado-md"></a>GetSchemaObject-Modell (ADO MD)
Ruft ein ADO MD Schema Objekt ([Dimension](./dimension-object-ado-md.md), [Hierarchie](./hierarchy-object-ado-md.md), [Ebene](./level-object-ado-md.md) [oder Element](./member-object-ado-md.md)) nach dem [UniqueName](./uniquename-property-ado-md.md)ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
Set object = CubeDef.GetSchemaObject (ObjType, UniqueName)  
```  
  
#### <a name="parameters"></a>Parameter  
 *ObjType*  
 Ein [schemaobjecttypeenum](./schemaobjecttypeenum.md) -Wert, der angibt, welcher Typ von Schema Objekt (Dimension, Hierarchie, Ebene oder Element) abgerufen werden soll.  
  
 *UniqueName*  
 Eine **Zeichenfolge** , die den **UniqueName** -Eigenschafts Wert des abzurufenden-Objekts angibt.  
  
## <a name="remarks"></a>Bemerkungen  
 **Getschemaobject** Ruft Objekte mithilfe ihrer eindeutigen Namen ab, wie von der **UniqueName** -Eigenschaft angegeben. Die Namen der übergeordneten Objekte müssen nicht bekannt sein, und übergeordnete Auflistungen müssen nicht aufgefüllt werden, um ein Schema Objekt abzurufen.  
  
## <a name="applies-to"></a>Gilt für  
 [CubeDef-Objekt (ADO MD)](./cubedef-object-ado-md.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [CubeDef-Objekt (ADO MD)](./cubedef-object-ado-md.md)