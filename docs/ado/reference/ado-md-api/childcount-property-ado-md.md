---
description: ChildCount-Eigenschaft (ADO MD)
title: ChildCount-Eigenschaft (ADO MD) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ChildCount
- Member::ChildCount
helpviewer_keywords:
- ChildCount property [ADO MD]
ms.assetid: 5463be22-ca50-43ea-9c92-468fc8eda280
author: rothja
ms.author: jroth
ms.openlocfilehash: 5515aaf6d3080057a725d40b1bd6cbcbe4c0c945
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100055705"
---
# <a name="childcount-property-ado-md"></a>ChildCount-Eigenschaft (ADO MD)
Gibt die Anzahl der Elemente an, für die [das aktuelle Element](./member-object-ado-md.md) Objekt das übergeordnete Element in einer Hierarchie ist.  
  
## <a name="return-values"></a>Rückgabewerte  
 Gibt eine **lange** ganze Zahl zurück und ist schreibgeschützt.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **childCount** -Eigenschaft, um eine Schätzung der Anzahl der untergeordneten **Elemente eines Members** zurückzugeben. Die untergeordneten **Elemente eines Members können von** der [Children](./children-property-ado-md.md) -Eigenschaft zurückgegeben werden.  
  
 Bei **Element Objekten aus** einem [Positions](./position-object-ado-md.md) Objekt beträgt die maximale Anzahl, die zurückgegeben wird, 65536. Wenn die tatsächliche Anzahl der untergeordneten Elemente 65536 überschreitet, ist der zurückgegebene Wert weiterhin 65536. Daher sollte die Anwendung eine **childCount** von 65536 als gleich oder größer als 65536 untergeordnete Elemente interpretieren.  
  
 Verwenden **Sie für Element** Objekte aus einem [Ebenenobjekt](./level-object-ado-md.md) die Eigenschaft ADO-Sammlungs [Anzahl](../ado-api/count-property-ado.md) für die Auflistung **Children** , um die genaue Anzahl der untergeordneten Elemente zu ermitteln. Die genaue Anzahl von untergeordneten Elementen kann langsam sein, wenn die Anzahl der untergeordneten Elemente in der Auflistung groß ist.  
  
## <a name="applies-to"></a>Gilt für  
 [Member-Objekt (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Children-Eigenschaft (ADO MD)](./children-property-ado-md.md)   
 [Count-Eigenschaft (ADO)](../ado-api/count-property-ado.md)   
 [Members-Collection (ADO MD)](./members-collection-ado-md.md)