---
description: PageCount-Eigenschaft (ADO)
title: PageCount-Eigenschaft (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::PageCount
helpviewer_keywords:
- PageCount property [ADO]
ms.assetid: b601b56c-0ac4-44ee-bc91-c3d2d104f00a
author: rothja
ms.author: jroth
ms.openlocfilehash: 828a0f9d872ab1040700941d85980e0b5deecf98
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041230"
---
# <a name="pagecount-property-ado"></a>PageCount-Eigenschaft (ADO)
Gibt an, wie viele Datenseiten das [Recordset](./recordset-object-ado.md) -Objekt enthält.  
  
## <a name="return-value"></a>Rückgabewert  
 Gibt einen **Long** -Wert zurück, der die Anzahl der Seiten im **Recordset** angibt.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **PageCount** -Eigenschaft, um zu bestimmen, wie viele Datenseiten im **Recordset** -Objekt vorliegen. *Seiten* sind Gruppen von Datensätzen, deren Größe der [PageSize](./pagesize-property-ado.md) -Eigenschaften Einstellung gleicht. Auch wenn die letzte Seite unvollständig ist, weil weniger Datensätze als der **PageSize** -Wert vorhanden sind, zählt sie als zusätzliche Seite im Wert der **PageCount** . Wenn das **Recordset** -Objekt diese Eigenschaft nicht unterstützt, ist der Wert-1, um anzugeben, dass die **PageCount** -Eigenschaft unbestimmbar ist.  
  
 Weitere Informationen zu Seitenfunktionen finden Sie in den Eigenschaften **PageSize** und [AbsolutePage](./absolutepage-property-ado.md) .  
  
## <a name="applies-to"></a>Gilt für  
 [Recordset-Objekt (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für AbsolutePage, PageCount und pageSize-Eigenschaften (VB)](./absolutepage-pagecount-and-pagesize-properties-example-vb.md)   
 [Beispiel für AbsolutePage, PageCount und pageSize-Eigenschaften (VC + +)](./absolutepage-pagecount-and-pagesize-properties-example-vc.md)   
 [AbsolutePage-Eigenschaft (ADO)](./absolutepage-property-ado.md)   
 [PageSize-Eigenschaft (ADO)](./pagesize-property-ado.md)   
 [RecordCount-Eigenschaft (ADO)](./recordcount-property-ado.md)