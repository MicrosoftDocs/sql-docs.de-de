---
description: Type-Eigenschaft (ADO)
title: Type-Eigenschaft (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::Type
- Field20::Type
helpviewer_keywords:
- Type property [ADO]
ms.assetid: 8a4c079f-9f4f-4545-801d-85983b8db71e
author: rothja
ms.author: jroth
ms.openlocfilehash: ac7cb237f0b3c3adb2621e8e7ad15912b907a4d7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170076"
---
# <a name="type-property-ado"></a>Type-Eigenschaft (ADO)
Gibt den Betriebs Typ oder den Datentyp eines [Parameters](./parameter-object.md), [Felds](./field-object.md)oder [Eigenschafts](./property-object-ado.md) Objekts an.  
  
## <a name="settings-and-return-values"></a>Einstellungen und Rückgabewerte  
 Legt einen [datatyanum](./datatypeenum.md) -Wert fest oder gibt ihn zurück.  
  
## <a name="remarks"></a>Bemerkungen  
 Bei **Parameter** Objekten ist die **Type** -Eigenschaft Lese-/Schreibzugriff. Bei neuen **Feld** Objekten, die an die [Fields](./fields-collection-ado.md) -Auflistung eines [Datensatzes](./record-object-ado.md)angehängt wurden, ist der **Typ** Lese-/Schreibzugriff, wenn die [value](./value-property-ado.md) -Eigenschaft für das **Feld** angegeben wurde und der Datenanbieter das neue **Feld** durch Aufrufen der [Update](./update-method.md) -Methode der **Fields** -Auflistung erfolgreich hinzugefügt hat.  
  
 Für alle anderen Objekte ist die **Type** -Eigenschaft schreibgeschützt.  
  
## <a name="applies-to"></a>Gilt für  

:::row:::
    :::column:::
        [Field-Objekt](./field-object.md)  
    :::column-end:::
    :::column:::
        [Parameter-Objekt](./parameter-object.md)  
    :::column-end:::
    :::column:::
        [Property-Objekt (ADO)](./property-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für eine Typeigenschaft (Feld) (VB)](./type-property-example-field-vb.md)   
 [Typeigenschafts Beispiel (Eigenschaft) (VC + +)](./type-property-example-property-vc.md)   
 [RecordType-Eigenschaft (ADO)](./recordtype-property-ado.md)   
 [Type-Eigenschaft (ADO-Stream)](./type-property-ado-stream.md)