---
description: 'Senden der Updates: UpdateBatch-Methode'
title: 'Senden der Updates: UpdateBatch-Methode | Microsoft-Dokumentation'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 87123797-831f-48e0-94b5-f669f9ca194a
author: rothja
ms.author: jroth
ms.openlocfilehash: 88e441fd0ff58f63cf5ac701ec2620c29048b1e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032492"
---
# <a name="sending-the-updates-updatebatch-method"></a>Senden der Updates: UpdateBatch-Methode
Mit dem folgenden Code wird ein Recordset im Batch Modus geöffnet, indem die LockType-Eigenschaft auf adlockbatchoptimiund der Cursor Location auf adUseClient festgelegt wird. Es werden zwei neue Datensätze hinzugefügt und der Wert eines Felds in einem vorhandenen Datensatz geändert. die ursprünglichen Werte werden gespeichert. Anschließend wird UpdateBatch aufgerufen, um die Änderungen an die Datenquelle zurückzusenden.  
  
## <a name="remarks"></a>Bemerkungen  
  
```  
'BeginBatchUpdate  
    strConn = "Provider=SQLOLEDB;Initial Catalog=Northwind;" & _  
              "Data Source=MySQLServer;Integrated Security=SSPI;"  
  
    strSQL = "SELECT ShipperId, CompanyName, Phone FROM Shippers"  
  
    Set objRs1 = New ADODB.Recordset  
    objRs1.CursorLocation = adUseClient  
    objRs1.Open strSQL, strConn, adOpenStatic, adLockBatchOptimistic, adCmdText  
  
    ' Change value of Phone field for first record in Recordset, saving value  
    ' for later restoration.  
    intId = objRs1("ShipperId")  
    sPhone = objRs1("Phone")  
  
    objRs1("Phone") = "(111) 555-1111"  
  
    'Add two new records  
    For i = 0 To 1  
        objRs1.AddNew  
        objRs1(1) = "New Shipper #" & CStr((i + 1))  
        objRs1(2) = "(nnn) 555-" & i & i & i & i  
    Next i  
  
    ' Send the updates  
    objRs1.UpdateBatch  
'EndBatchUpdate  
```  
  
 Wenn Sie den aktuellen Datensatz bearbeiten oder einen neuen Datensatz hinzufügen, wenn Sie die UpdateBatch-Methode aufrufen, ruft ADO automatisch die Update-Methode auf, um ausstehende Änderungen am aktuellen Datensatz zu speichern, bevor die Batch Änderungen an den Anbieter übertragen werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Batchmodus](../../../ado/guide/data/batch-mode.md)
