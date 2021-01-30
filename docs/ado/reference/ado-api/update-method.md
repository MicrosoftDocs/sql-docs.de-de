---
description: Update-Methode
title: Update-Methode | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Update
helpviewer_keywords:
- Update method [ADO]
ms.assetid: 6b2a9c31-1a7e-40db-8a53-30720d0f6cc1
author: rothja
ms.author: jroth
ms.openlocfilehash: 279d0e82bff4d71a2c3b18bbdc7ff88f0b9581a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172427"
---
# <a name="update-method"></a>Update-Methode
Speichert alle Änderungen, die Sie an der aktuellen Zeile eines [Recordset](./recordset-object-ado.md) -Objekts vornehmen, oder die [Fields](./fields-collection-ado.md) -Auflistung eines [Datensatz](./record-object-ado.md) -Objekts.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
recordset.Update Fields, Values  
record.Fields.Update  
```  
  
#### <a name="parameters"></a>Parameter  
 *Fields*  
 Optional. Eine **Variante** , die einen einzelnen Namen darstellt, oder ein **Variant** -Array, das Namen oder Ordinalpositionen der Felder darstellt, die Sie ändern möchten.  
  
 *Werte*  
 Optional. Eine **Variante** , die einen einzelnen Wert darstellt, oder ein **Variant** -Array, das Werte für das Feld bzw. die Felder im neuen Datensatz darstellt.  
  
## <a name="remarks"></a>Bemerkungen  
  
## <a name="recordset"></a>Recordset  
 Verwenden Sie die **Update** -Methode, um alle Änderungen zu speichern, die Sie an dem aktuellen Datensatz eines **Recordset** -Objekts vornehmen, seit Sie die [AddNew](./addnew-method-ado.md) -Methode aufrufen oder die Feldwerte in einem vorhandenen Datensatz ändern. Das **Recordset** -Objekt muss Updates unterstützen.  
  
 Führen Sie einen der folgenden Schritte aus, um Feldwerte festzulegen:  
  
-   Weisen Sie der [value](./value-property-ado.md) -Eigenschaft eines [Feld](./field-object.md) Objekts Werte zu, und nennen Sie die **Update** -Methode.  
  
-   Übergeben Sie einen Feldnamen und einen Wert als Argumente mit dem **Update** -Befehl.  
  
-   Übergeben Sie ein Array von Feldnamen und ein Array von Werten mit dem **Update** -Befehl.  
  
 Wenn Sie Arrays von Feldern und Werten verwenden, muss die gleiche Anzahl von Elementen in beiden Arrays vorhanden sein. Außerdem muss die Reihenfolge der Feldnamen mit der Reihenfolge der Feldwerte identisch sein. Wenn die Anzahl und Reihenfolge der Felder und Werte nicht identisch sind, tritt ein Fehler auf.  
  
 Wenn das **Recordset** -Objekt die Batch Aktualisierung unterstützt, können Sie mehrere Änderungen an einem oder mehreren Datensätzen lokal zwischenspeichern, bis Sie die [UpdateBatch](./updatebatch-method.md) -Methode aufrufen. Wenn Sie den aktuellen Datensatz bearbeiten oder einen neuen Datensatz hinzufügen, wenn Sie die **UpdateBatch** -Methode aufrufen, ruft ADO automatisch die **Update** -Methode auf, um ausstehende Änderungen am aktuellen Datensatz zu speichern, bevor die Batch Änderungen an den Anbieter übertragen werden.  
  
 Wenn Sie von dem Datensatz wechseln, den Sie vor dem Aufrufen der **Update** -Methode hinzufügen oder bearbeiten, wird von ADO automatisch **Update** aufgerufen, um die Änderungen zu speichern. Sie müssen die [CancelUpdate](./cancelupdate-method-ado.md) -Methode aufzurufen, wenn Sie Änderungen an dem aktuellen Datensatz abbrechen oder einen neu hinzugefügten Datensatz verwerfen möchten.  
  
 Der aktuelle Datensatz bleibt aktuell, nachdem Sie die **Update** -Methode aufgerufen haben.  
  
## <a name="record"></a>Record  
 Die **Update** -Methode schließt Ergänzungen, Löschungen und Aktualisierungen von Feldern in der [Fields](./fields-collection-ado.md) -Auflistung eines **Datensatz** -Objekts ab.  
  
 Beispielsweise werden mit der **Delete** -Methode gelöschte Felder sofort zum Löschen markiert, bleiben jedoch in der Auflistung. Die **Update** -Methode muss aufgerufen werden, um diese Felder tatsächlich aus der Auflistung des Anbieters zu löschen.  
  
## <a name="applies-to"></a>Gilt für  

:::row:::
    :::column:::
        [Fields-Collection (ADO)](./fields-collection-ado.md)  
    :::column-end:::
    :::column:::
        [Recordset-Objekt (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für Update-und CancelUpdate-Methoden (VB)](./update-and-cancelupdate-methods-example-vb.md)   
 [Beispiel für Update-und CancelUpdate-Methoden (VC + +)](./update-and-cancelupdate-methods-example-vc.md)   
 [AddNew-Methode (ADO)](./addnew-method-ado.md)   
 [CancelUpdate-Methode (ADO)](./cancelupdate-method-ado.md)   
 [EditMode-Eigenschaft](./editmode-property.md)   
 [UpdateBatch-Methode](./updatebatch-method.md)