---
description: Item-Eigenschaft (ADO MD Cellset)
title: Item-Eigenschaft (ADO MD Cellset) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Item
- Cellset::Item
helpviewer_keywords:
- Item property [ADO MD]
ms.assetid: 0e93d79b-b12e-4e98-889e-c2dfcca20fd0
author: rothja
ms.author: jroth
ms.openlocfilehash: 0fc6d2650619ef0647f32af9101c3e0c895e300a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054565"
---
# <a name="item-property-ado-md-cellset"></a>Item-Eigenschaft (ADO MD Cellset)
Ruft eine Zelle aus einem [Cellset](./cellset-object-ado-md.md) mithilfe ihrer Koordinaten ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
Set  
Cell = Cellset.Item ( Positions)  
```  
  
## <a name="parameters"></a>Parameter  
 *Positionen*  
 Ein **VariantArray** von Werten, die eine Zelle eindeutig angeben. *Positionen* können eines der folgenden sein:  
  
-   Ein Array von Positionsnummern.  
  
-   Ein Array von Elementnamen.  
  
-   Die Ordinalposition  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **Item** -Eigenschaft, um ein [Cell](./cell-object-ado-md.md) -Objekt in einem [Cellset](./cellset-object-ado-md.md) -Objekt zurückzugeben. Wenn die **Item** -Eigenschaft die Zelle nicht finden kann, die dem *Positions* Argument entspricht, tritt ein Fehler auf.  
  
 Die **Item** -Eigenschaft ist die Standard Eigenschaft für das **Cellset** -Objekt. Die folgenden Syntax Formen sind austauschbar:  
  
```  
  
Cellset.Item ( Positions )Cellset ( Positions )  
```  
  
 Das *Positions* Argument gibt an, welche Zelle zurückgegeben werden soll. Sie können die Zelle anhand der Ordinalposition oder der Position entlang jeder Achse angeben. Wenn Sie die Zelle auf jeder Achse nach Position angeben, können Sie den numerischen Wert der Position oder die Namen der Elemente für jede Position angeben.  
  
 Die Ordinalposition ist eine Zahl, die eine Zelle innerhalb des **Cellsets** eindeutig identifiziert. Konzeptionell werden Zellen in einem **Cellset** nummeriert, als ob das **Cellset** ein *p*-dimensionales Array wäre, wobei *p* für die Anzahl der Achsen steht. Die Zellen werden in zeilengerichteter Reihenfolge adressiert. Im folgenden finden Sie die Formel zum Berechnen der Ordinalzahl einer Zelle:  
  
 Wenn Elementnamen als Zeichen folgen an das **Element** weitergegeben werden, müssen die Member in aufsteigender Reihenfolge der numerischen Achsen Bezeichner aufgelistet werden. Innerhalb einer Achse müssen die Member in aufsteigender Reihenfolge der Dimensions Schachtelung aufgeführt werden, d. h., der Member der äußersten Dimension kommt zuerst an, gefolgt von Membern der inneren Dimensionen. Jede Dimension sollte durch eine separate Zeichenfolge dargestellt werden, und die Liste der Element Zeichenfolgen sollte durch Kommas getrennt werden.  
  
> [!NOTE]
>  Das Abrufen von Zellen nach Elementnamen wird vom Datenanbieter möglicherweise nicht unterstützt. Weitere Informationen finden Sie in der Dokumentation für Ihren Anbieter.  
  
## <a name="applies-to"></a>Gilt für  
 [Cellset-Objekt (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Cell-Objekt (ADO MD)](./cell-object-ado-md.md)   
 [Cellset-Objekt (ADO MD)](./cellset-object-ado-md.md)