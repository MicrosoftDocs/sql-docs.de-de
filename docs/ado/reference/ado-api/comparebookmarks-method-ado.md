---
description: CompareBookmarks-Methode (ADO)
title: CompareBookmarks-Methode (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CompareBookmarks
- Recordset20::CompareBookmarks
- Recordset20::raw_CompareBookmarks
helpviewer_keywords:
- CompareBookmarks method [ADO]
ms.assetid: d0b64286-2cc4-4a22-8f1d-9aefeebbcbc6
author: rothja
ms.author: jroth
ms.openlocfilehash: 63fc23d638a3ffecf29d6f3d3ad56e803d914072
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99155691"
---
# <a name="comparebookmarks-method-ado"></a>CompareBookmarks-Methode (ADO)
Vergleicht zwei Lesezeichen und gibt eine Angabe über das Verhältnis der entsprechenden Werte zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
result = recordset.CompareBookmarks(Bookmark1, Bookmark2)  
```  
  
## <a name="return-value"></a>Rückgabewert  
 Gibt einen [compareenum](./compareenum.md) -Wert zurück, der die relative Zeilen Position von zwei Datensätzen angibt, die durch Ihre Lesezeichen dargestellt werden.  
  
#### <a name="parameters"></a>Parameter  
 *Bookmark1*  
 Das Lesezeichen der ersten Zeile.  
  
 *Bookmark2*  
 Das Lesezeichen der zweiten Zeile.  
  
## <a name="remarks"></a>Bemerkungen  
 Die Lesezeichen müssen auf dasselbe [Recordset](./recordset-object-ado.md) -Objekt oder ein **Recordset** -Objekt und dessen [Klon](./clone-method-ado.md)angewendet werden. Lesezeichen aus unterschiedlichen **recordsetobjekten** können nicht zuverlässig verglichen werden, auch wenn Sie aus derselben Quelle oder demselben Befehl erstellt wurden. Sie können auch keine Lesezeichen für ein **Recordset** -Objekt vergleichen, dessen zugrunde liegender Anbieter keine Vergleiche unterstützt.  
  
 Ein Lesezeichen identifiziert eine Zeile in einem **Recordset** -Objekt eindeutig. Verwenden Sie die [Bookmark](./bookmark-property-ado.md) -Eigenschaft der aktuellen Zeile, um das Lesezeichen zu erhalten.  
  
 Da der Datentyp eines Lesezeichens für jeden Anbieter spezifisch ist, macht ADO ihn als **Variant** verfügbar. SQL Server Lesezeichen sind z. b. vom Typ DBTYPE_R8 (**Double**). ADO würde diesen Typ als **Variant** mit dem Untertyp **Double** verfügbar machen.  
  
 Beim Vergleichen von Lesezeichen versucht ADO nicht, eine Umwandlung durchführen. Die Werte werden einfach an den Anbieter übermittelt, wo der Vergleich erfolgt. Wenn die an die **CompareBookmarks** -Methode übergebenen Lesezeichen in Variablen unterschiedlicher Typen gespeichert werden, kann der folgende Typen Konflikt Fehler generiert werden: "Argumente weisen den falschen Typ auf, sind außerhalb des zulässigen Bereichs oder stehen miteinander in Konflikt."  
  
 Ein Lesezeichen, das ungültig oder falsch formatiert ist, führt zu einem Fehler.  
  
## <a name="applies-to"></a>Gilt für  
 [Recordset-Objekt (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [CompareBookmarks-Methode (Beispiel) (VB)](./comparebookmarks-method-example-vb.md)   
 [CompareBookmarks-Methode (Beispiel) (VC + +)](./comparebookmarks-method-example-vc.md)   
 [Bookmark-Eigenschaft (ADO)](./bookmark-property-ado.md)