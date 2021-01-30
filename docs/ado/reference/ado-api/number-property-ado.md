---
description: Number-Eigenschaft (ADO)
title: Number-Eigenschaft (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::Number
- Error::GetNumber
- Error::get_Number
helpviewer_keywords:
- number property [ADO]
ms.assetid: f92323c5-dd11-4a63-a505-d9014a0f067f
author: rothja
ms.author: jroth
ms.openlocfilehash: 82206b827f505994cd151833e0bee4b1ff4ad7de
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167033"
---
# <a name="number-property-ado"></a>Number-Eigenschaft (ADO)
Gibt die Zahl an, durch die ein [Fehler](./error-object.md) Objekt eindeutig identifiziert wird.  
  
## <a name="return-value"></a>Rückgabewert  
 Gibt einen **Long** -Wert zurück, der einer der [ErrorValueEnum](./errorvalueenum.md) -Konstanten entsprechen kann.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die Eigenschaft **Number** , um zu bestimmen, welcher Fehler aufgetreten ist. Der Wert der-Eigenschaft ist eine eindeutige Zahl, die der Fehlerbedingung entspricht.  
  
 Die [Errors](./errors-collection-ado.md) -Auflistung gibt ein HRESULT im Hexadezimal Format (z. b. 0x80004005) oder Long Value (z. b. 2147467259) zurück. Diese HRESULTs können von zugrunde liegenden Komponenten, z. b. OLE DB oder sogar von OLE selbst, ausgelöst werden. Weitere Informationen zu diesen Zahlen finden Sie unter [Errors (OLE DB)](/previous-versions/windows/desktop/ms724533(v=vs.85)) in der [OLE DB Programmer es Reference](/previous-versions/windows/desktop/ms713643(v=vs.85))*.*  
  
## <a name="applies-to"></a>Gilt für  
 [Error-Objekt](./error-object.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für Beschreibung, HelpContext, HelpFile, NativeError, Number, Source und SQLSTATE Properties (VB)](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Beschreibung, HelpContext, HelpFile, NativeError, Number, Source und SQLSTATE Properties Example (VC + +)](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [Description-Eigenschaft](./description-property.md)   
 [HelpContext, HelpFile-Eigenschaften](./helpcontext-helpfile-properties.md)   
 [Source-Eigenschaft (ADO Error)](./source-property-ado-error.md)