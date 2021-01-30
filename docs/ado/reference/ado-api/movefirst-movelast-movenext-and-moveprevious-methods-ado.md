---
description: Muvefirst-, muvelast-, muvenext-und muveprevious-Methode (ADO)
title: Muvefirst-, muvelast-, muvenext-und muveprevious-Methoden (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::MoveLast
- Recordset15::MoveNext
- Recordset15::raw_MoveNext
- Recordset15::raw_MovePrevious
- Recordset15::MoveFirst
- Recordset15::raw_MoveLast
- Recordset15::MovePrevious
- Recordset15::raw_MoveFirst
helpviewer_keywords:
- MoveNext method [ADO]
- MoveLast method [ADO]
- MoveFirst method [ADO]
- MovePrevious method [ADO]
ms.assetid: a61a01a7-5b33-4150-9126-21dfa63654cb
author: rothja
ms.author: jroth
ms.openlocfilehash: 2c0f227b7c66928d04980a454828e8d7a80f064f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167098"
---
# <a name="movefirst-movelast-movenext-and-moveprevious-methods-ado"></a>Muvefirst-, muvelast-, muvenext-und muveprevious-Methode (ADO)
Wechselt zum ersten, letzten, nächsten oder vorherigen Datensatz in einem angegebenen [Recordset](./recordset-object-ado.md) -Objekt und legt diesen Datensatz auf den aktuellen Datensatz fest.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
recordset.{MoveFirst | MoveLast | MoveNext | MovePrevious}  
```  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **MoveFirst** -Methode, um die aktuelle Daten Satz Position in den ersten Datensatz im **Recordset** zu verschieben.  
  
 Verwenden Sie die **MoveLast** -Methode, um die aktuelle Daten Satz Position in den letzten Datensatz im **Recordset** zu verschieben. Das **Recordset** -Objekt muss Lesezeichen oder rückwärts Cursor Bewegung unterstützen. Andernfalls generiert der Methoden aufrufsvorgang einen Fehler.  
  
 **Wenn das** **Recordset** leer ist (sowohl **BOF** **als auch** **EOF** sind true), wird ein Fehler generiert.  
  
 Verwenden Sie die **MoveNext** -Methode, um die aktuelle Daten Satz Position einen Datensatz vorwärts (am Ende des **Recordsets**) zu verschieben. Wenn der letzte Datensatz der aktuelle Datensatz ist und Sie die Methode "-Methode" aufgerufen haben, legt **ADO den aktuellen** Datensatz auf die Position hinter dem letzten Datensatz im **Recordset fest** ([EOF](./bof-eof-properties-ado.md) ist **true**). Ein Versuch, vorwärts zu wechseln, wenn die **EOF** -Eigenschaft bereits **true** ist, generiert einen Fehler.  
  
 Wenn in ADO 2,5 und höher das **Recordset** gefiltert oder sortiert wurde und die Daten des aktuellen Datensatzes geändert werden, **verschiebt der Aufruf der Methode "** -Methode" den Cursor auf zwei Datensätze nach oben aus dem aktuellen Datensatz. Dies liegt daran, dass bei einer Änderung des aktuellen Datensatzes der nächste Datensatz zum neuen aktuellen Datensatz wird. Durch den Aufruf von " **muvenext** " nach der Änderung wird der Cursor um einen Datensatz aus dem neuen aktuellen Datensatz verschoben. Dies unterscheidet sich vom Verhalten in ADO 2,1 und früher. In diesen früheren Versionen wird durch das Ändern der Daten eines aktuellen Datensatzes im sortierten oder gefilterten **Recordset** die Position des aktuellen Datensatzes nicht geändert, und der Cursor wird von " **muvenext** " direkt nach dem aktuellen Datensatz auf den nächsten Datensatz verschoben.  
  
 Verwenden Sie die **MovePrevious** -Methode, um die aktuelle Daten Satz Position um einen Datensatz nach unten (am oberen Rand des **Recordsets**) zu verschieben. Das **Recordset** -Objekt muss Lesezeichen oder rückwärts Cursor Bewegung unterstützen. Andernfalls generiert der Methoden aufrufsvorgang einen Fehler. Wenn der erste Datensatz der aktuelle Datensatz ist und Sie die Methode " **muveprevious** " aufzurufen, legt ADO den aktuellen Datensatz auf die Position vor dem ersten Datensatz im **Recordset fest** ([BOF](./bof-eof-properties-ado.md) ist **true**). Ein Versuch, rückwärts zu wechseln, wenn die **BOF** -Eigenschaft bereits **true** ist, generiert einen Fehler. Wenn das **Recordset** -Objekt entweder Lesezeichen oder rückwärts Cursor Bewegung nicht unterstützt, generiert die Methode " **muveprevious** " einen Fehler.  
  
 Wenn das **Recordset** nur vorwärts ist und Sie den vorwärts-und rückwärts Bildlauf unterstützen möchten, können Sie die [CacheSize](./cachesize-property-ado.md) -Eigenschaft verwenden, um einen Daten Satz Cache zu erstellen, der die abwärts Cursor Bewegung durch die [Move](./move-method-ado.md) -Methode unterstützt. Da zwischengespeicherte Datensätze in den Arbeitsspeicher geladen werden, sollten Sie das Caching mehrerer Datensätze vermeiden, als erforderlich sind. Sie **können die Methode** "Methode" in einem vorwärts gerichteten **Recordset** -Objekt abrufen. Dies kann bewirken, dass der Anbieter den Befehl erneut ausführt, der das **Recordset** -Objekt generiert hat.  
  
## <a name="applies-to"></a>Gilt für  
 [Recordset-Objekt (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Muvefirst-, muvelast-, muvenext-und muveprevious-Methoden Beispiel (VB)](./movefirst-movelast-movenext-and-moveprevious-methods-example-vb.md)   
 [Muvefirst-, muvelast-, muvenext-und muveprevious-Methoden Beispiel (VBScript)](./movefirst-movelast-movenext-and-moveprevious-methods-example-vbscript.md)   
 [Muvefirst-, muvelast-, muvenext-und muveprevious-Methoden Beispiel (VC + +)](./movefirst-movelast-movenext-and-moveprevious-methods-example-vc.md)   
 [Move-Methode (ADO)](./move-method-ado.md)   
 ["Muvefirst", "muvelast", "muvenext" und "muveprevious" (RDS)](../rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)   
 [MoveRecord-Methode (ADO)](./moverecord-method-ado.md)