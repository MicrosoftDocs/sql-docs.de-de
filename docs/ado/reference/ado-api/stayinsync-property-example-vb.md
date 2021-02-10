---
description: StayInSync-Eigenschaft – Beispiel (VB)
title: Beispiel für die StayInSync-Eigenschaft (VB) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- StayInSync property [ADO], Visual Basic example
ms.assetid: b682bcc3-04b3-42b0-86f4-c17e0cd29baf
author: rothja
ms.author: jroth
ms.openlocfilehash: dd523383e69d27b3a9c57b4ce99e4be3bc6a1d13
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040390"
---
# <a name="stayinsync-property-example-vb"></a>StayInSync-Eigenschaft – Beispiel (VB)
In diesem Beispiel wird veranschaulicht, wie die [StayInSync](./stayinsync-property.md) -Eigenschaft den Zugriff auf Zeilen in einem hierarchischen [Recordset](./recordset-object-ado.md)erleichtert.  
  
 Die äußere Schleife zeigt den vor-und Nachnamen, den Status und die Identifizierung jedes Autors an. Das angefügte **Recordset** für jede Zeile wird aus der [Fields](./fields-collection-ado.md) -Auflistung abgerufen und automatisch **rsttitleauthor** von der **StayInSync** -Eigenschaft zugewiesen, wenn das übergeordnete **Recordset** in eine neue Zeile verschoben wird. Die innere Schleife zeigt vier Felder aus jeder Zeile im angefügten Recordset an.  
  
```  
'BeginStayInSyncVB  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    Dim Cnxn As ADODB.Connection  
    Dim rst As ADODB.Recordset  
    Dim rstTitleAuthor As New ADODB.Recordset  
    Dim strCnxn As String  
  
    ' Open the connection with Data Shape attributes  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider=MSDataShape;Data Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' Create a recordset  
    Set rst = New ADODB.Recordset  
    rst.StayInSync = True  
    rst.Open "SHAPE {select * from Authors} " & _  
                   "APPEND ( { select * from titleauthor} AS chapTitleAuthor " & _  
                   "RELATE au_id TO au_id) ", Cnxn, , , adCmdText  
  
    Set rstTitleAuthor = rst("chapTitleAuthor").Value  
    Do Until rst.EOF  
        Debug.Print rst!au_fname & " " & rst!au_lname & " " & _  
                   rst!State & " " & rst!au_id  
  
        Do Until rstTitleAuthor.EOF  
            Debug.Print rstTitleAuthor(0) & " " & rstTitleAuthor(1) & " " & _  
                   rstTitleAuthor(2) & " " & rstTitleAuthor(3)  
            rstTitleAuthor.MoveNext  
        Loop  
  
        rst.MoveNext  
    Loop  
  
    ' Clean up  
    rst.Close  
    Cnxn.Close  
    Set rst = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' Clean up  
    If Not rst Is Nothing Then  
        If rst.State = adStateOpen Then rst.Close  
    End If  
    Set rst = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndStayInSyncVB  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Fields-Auflistung (ADO)](./fields-collection-ado.md)   
 [Recordset-Objekt (ADO)](./recordset-object-ado.md)   
 [StayInSync-Eigenschaft](./stayinsync-property.md)