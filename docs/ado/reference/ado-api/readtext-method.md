---
description: ReadText-Methode
title: Read-Text-Methode | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_ReadText
- _Stream::ReadText
helpviewer_keywords:
- ReadText method [ADO]
ms.assetid: be5a409e-cf87-4859-9ea5-713401755a77
author: rothja
ms.author: jroth
ms.openlocfilehash: e0d88fba7389c40442fccf006afc03020004d928
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166753"
---
# <a name="readtext-method"></a>ReadText-Methode
Liest die angegebene Anzahl von Zeichen aus einem [Textstreamobjekt](./stream-object-ado.md) .  
  
## <a name="syntax"></a>Syntax  
  
```  
  
String = Stream.ReadText ( NumChars)  
```  
  
#### <a name="parameters"></a>Parameter  
 *NumChars*  
 Optional. Ein **Long** -Wert, der die Anzahl der Zeichen angibt, die aus der Datei gelesen werden sollen, oder ein [streamleseenumerationswert](./streamreadenum.md) . Der Standardwert ist " **ADRead all**".  
  
## <a name="return-value"></a>Rückgabewert  
 Die Read **Text** -Methode liest eine angegebene Anzahl von Zeichen, eine ganze Zeile oder den gesamten Stream aus einem **Streamobjekt** und gibt die resultierende Zeichenfolge zurück.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn *NumChar* größer als die Anzahl der im Stream verbliebenen Zeichen ist, werden nur die verbleibenden Zeichen zurückgegeben. Die gelesene Zeichenfolge wird nicht so aufgefüllt, dass Sie der von *NumChar* angegebenen Länge entspricht. Wenn keine zu lesenden Zeichen vorhanden sind, wird eine Variante zurückgegeben, deren Wert NULL ist. "Read **Text** " kann nicht zum Rück Lesen verwendet werden.  
  
> [!NOTE]
>  Die Read **Text** -Methode wird mit Textstreams verwendet ([Type](./type-property-ado-stream.md) ist **adtypetext**). Verwenden Sie für binäre Datenströme (**Typ** : **adTypeBinary**) den Wert [Lesen](./read-method.md).  
  
 Abfragen, die zu einer großen Menge von XML-Daten führen, die durch die Read **Text** -Methode des ADO-Datenstrom Objekts (ActiveX Data Object) zurückgegeben werden, können viel Zeit in Anspruch nehmen. Wenn dies in einer COM+-Komponente erfolgt, die von einer ASP-Seite aufgerufen wird, kann ein Timeout für die Sitzung des Benutzers auftreten. ADO konvertiert streamobjektdaten von UTF-8-Codierung in Unicode. die häufige Umwandlung von Speicher Belegungen bei einer solchen großen Datenmenge auf einmal ist recht zeitaufwändig. Um das Problem zu beheben, führen Sie wiederholte Aufrufe der Read- **Text** -Methode des ADO-Befehls Objekts aus, und geben Sie eine geringere Anzahl von Zeichen an. Tests haben ergeben, dass ein Wert, der 128 KB (131.072) entspricht, optimal ist. Die Antwortzeit sinkt, wenn dieser Wert verringert wird. Weitere Informationen finden Sie im Knowledge Base-Artikel 280067, "PRB: Abrufen sehr großer XML-Dokumente aus SQL Server 2000 mithilfe der Read Text-Methode des ADO-Streamobjekts ist möglicherweise langsam" in der Microsoft Knowledge Base unter https://support.microsoft.com .  
  
## <a name="applies-to"></a>Gilt für  
 [Stream-Objekt (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Read-Methode](./read-method.md)