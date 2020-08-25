---
description: BeginTrans-, CommitTrans-und RollbackTrans-Methoden Beispiel (VB)
title: BeginTrans-, CommitTrans-und RollbackTrans-Methoden Beispiel (VB) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- RollbackTrans method [ADO], Visual Basic example
- CommitTrans method [ADO], Visual Basic example
- BeginTrans method [ADO], Visual Basic example
ms.assetid: aa7de324-cd71-4bd0-8043-24229f4a785e
author: rothja
ms.author: jroth
ms.openlocfilehash: 9579e5314bb298eed65145548d848e31bf47c1b9
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2020
ms.locfileid: "88776429"
---
# <a name="begintrans-committrans-and-rollbacktrans-methods-example-vb"></a>BeginTrans-, CommitTrans-und RollbackTrans-Methoden Beispiel (VB)
In diesem Beispiel wird der Buchtyp aller Psychologie Bücher in der ***Titel*** Tabelle der-Datenbank geändert. Nachdem die [BeginTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) -Methode eine Transaktion gestartet hat, die alle an der Tabelle " ***Titel*** " vorgenommenen Änderungen isoliert, speichert die [CommitTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) -Methode die Änderungen. Sie können die [RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) -Methode verwenden, um Änderungen rückgängig zu machen, die Sie mithilfe der [Update](./update-method.md) -Methode gespeichert haben.  
  
```  
'BeginBeginTransVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    'recordset and connection variables  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
    Dim rstTitles As ADODB.Recordset  
    Dim strSQLTitles As String  
    'record variables  
    Dim strTitle As String  
    Dim strMessage As String  
  
    ' Open connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Set Cnxn = New ADODB.Connection  
    Cnxn.Open strCnxn  
  
    ' Open recordset dynamic to allow for changes  
    Set rstTitles = New ADODB.Recordset  
    strSQLTitles = "Titles"  
    rstTitles.Open strSQLTitles, Cnxn, adOpenDynamic, adLockPessimistic, adCmdTable  
  
    Cnxn.BeginTrans  
  
    ' Loop through recordset and prompt user  
    ' to change the type for a specified title  
  
    rstTitles.MoveFirst  
  
    Do Until rstTitles.EOF  
        If Trim(rstTitles!Type) = "psychology" Then  
            strTitle = rstTitles!Title  
            strMessage = "Title: " & strTitle & vbCr & _  
            "Change type to self help?"  
  
            ' If yes, change type for the specified title  
            If MsgBox(strMessage, vbYesNo) = vbYes Then  
                rstTitles!Type = "self_help"  
                rstTitles.Update  
            End If  
        End If  
    rstTitles.MoveNext  
    Loop  
  
    ' Prompt user to commit all changes made  
    If MsgBox("Save all changes?", vbYesNo) = vbYes Then  
        Cnxn.CommitTrans  
    Else  
        Cnxn.RollbackTrans  
    End If  
  
    ' Print recordset  
    rstTitles.Requery  
    rstTitles.MoveFirst  
    Do While Not rstTitles.EOF  
        Debug.Print rstTitles!Title & " - " & rstTitles!Type  
        rstTitles.MoveNext  
    Loop  
  
    ' Restore original data as this is a demo  
    rstTitles.MoveFirst  
  
    Do Until rstTitles.EOF  
        If Trim(rstTitles!Type) = "self_help" Then  
            rstTitles!Type = "psychology"  
            rstTitles.Update  
        End If  
        rstTitles.MoveNext  
    Loop  
  
    ' clean up  
    rstTitles.Close  
    Cnxn.Close  
    Set rstTitles = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rstTitles Is Nothing Then  
        If rstTitles.State = adStateOpen Then rstTitles.Close  
    End If  
    Set rstTitles = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
  
'EndBeginTransVB  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [BeginTrans-, CommitTrans-und RollbackTrans-Methode (ADO)](./begintrans-committrans-and-rollbacktrans-methods-ado.md)   
 [Connection-Objekt (ADO)](./connection-object-ado.md)