---
description: CacheSize-Eigenschaft (ADO)
title: CacheSize-Eigenschaft (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Recordset15::CacheSize
helpviewer_keywords:
- CacheSize property [ADO]
ms.assetid: 49dc9a49-af7b-433b-be36-7a14ca984fb7
author: rothja
ms.author: jroth
ms.openlocfilehash: d6654adc5cbf5b01435dbc95a2f630cf980cc6d5
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2020
ms.locfileid: "88776359"
---
# <a name="cachesize-property-ado"></a>CacheSize-Eigenschaft (ADO)
Gibt die Anzahl von Datensätzen aus einem [Recordset](./recordset-object-ado.md) -Objekt an, die lokal im Arbeitsspeicher zwischengespeichert werden.  
  
## <a name="settings-and-return-values"></a>Einstellungen und Rückgabewerte  
 Legt einen **Long** -Wert fest, der größer als 0 sein muss, oder gibt ihn zurück. Der Standardwert ist 1.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **CacheSize** -Eigenschaft, um zu steuern, wie viele Datensätze gleichzeitig vom Anbieter in den lokalen Speicher abgerufen werden sollen. Wenn die **CacheSize** z. b. 10 ist, ruft der Anbieter nach dem ersten Öffnen des **Recordset** -Objekts die ersten 10 Datensätze in den lokalen Arbeitsspeicher ab. Wenn Sie durch das **Recordset** -Objekt wechseln, gibt der Anbieter die Daten aus dem lokalen Speicherpuffer zurück. Sobald Sie mit dem letzten Datensatz im Cache fortfahren, ruft der Anbieter die nächsten 10 Datensätze aus der Datenquelle in den Cache ab.  
  
> [!NOTE]
>  **CacheSize** basiert auf der maximalen Anzahl von **Open Rows** -anbieterspezifischen Eigenschaften (in der **Properties** -Auflistung des **Recordset** -Objekts). Sie können **CacheSize** nicht auf einen Wert festlegen, der größer ist als die **Maximale Anzahl offener Zeilen**. Um die Anzahl der Zeilen zu ändern, die vom Anbieter geöffnet werden können, legen Sie die **Maximale Anzahl geöffneter Zeilen**fest.  
  
 Der Wert von **CacheSize** kann während der Lebensdauer des **Recordset** -Objekts angepasst werden. Wenn Sie diesen Wert jedoch ändern, wirkt sich dies nur auf die Anzahl der Datensätze im Cache aus, nachdem nachfolgende Abruf Abbilder aus der Datenquelle abgerufen wurden. Wenn Sie den Eigenschafts Wert alleine ändern, wird der aktuelle Inhalt des Caches nicht geändert.  
  
 Wenn weniger Datensätze abgerufen werden müssen, als **CacheSize** angibt, gibt der Anbieter die verbleibenden Datensätze zurück, und es tritt kein Fehler auf.  
  
 Eine **CacheSize** -Einstellung von 0 (null) ist nicht zulässig und gibt einen Fehler zurück.  
  
 Aus dem Cache abgerufene Datensätze spiegeln keine gleichzeitigen Änderungen wider, die andere Benutzer an den Quelldaten vorgenommen haben. Verwenden Sie die [Resync](./resync-method.md) -Methode, um ein Update aller zwischengespeicherten Daten zu erzwingen.  
  
 Wenn **CacheSize** auf einen Wert größer als 1 festgelegt ist, können die Navigationsmethoden ([Move](./move-method-ado.md), [MoveFirst, MoveLast, MoveNext und MovePrevious](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md)) zur Navigation zu einem gelöschten Datensatz führen, wenn der Löschvorgang nach dem Abrufen der Datensätze erfolgt. Nach dem ersten Abruf werden nachfolgende Löschvorgänge nicht in Ihren Daten Cache übernommen, bis Sie versuchen, auf einen Datenwert aus einer gelöschten Zeile zuzugreifen. Wenn Sie **CacheSize** auf einen solchen Wert festlegen, wird dieses Problem jedoch vermieden, da gelöschte Zeilen nicht abgerufen werden können  
  
## <a name="applies-to"></a>Gilt für  
 [Recordset-Objekt (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [CacheSize-Eigenschaft (Beispiel) (VB)](./cachesize-property-example-vb.md)   
 [Beispiel für CacheSize-Eigenschaft (VC + +)](./cachesize-property-example-vc.md)   
 [CacheSize-Eigenschaft – Beispiel (JScript)](./cachesize-property-example-jscript.md)