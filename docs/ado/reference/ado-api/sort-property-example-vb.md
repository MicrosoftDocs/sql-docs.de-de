---
description: Sort-Eigenschaft – Beispiel (VB)
title: Beispiel für eine Sortier Eigenschaft (VB) | Microsoft-Dokumentation
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
- Sort property [ADO], Visual Basic example
ms.assetid: fc2fd40b-65d6-4023-90a3-90c9a88ef6cf
author: rothja
ms.author: jroth
ms.openlocfilehash: 577ceb894804987ef6ad8b949b9c7d0e8b603180
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040580"
---
# <a name="sort-property-example-vb"></a>Sort-Eigenschaft – Beispiel (VB)
In diesem Beispiel wird die [Sort](./sort-property.md) -Eigenschaft des [Recordset](./recordset-object-ado.md) -Objekts zum Neusortieren der Zeilen eines **Recordsets** verwendet, das von der " **_Authors_*_"-Tabelle der _*_Pubs_** -Datenbank abgeleitet wurde. Eine sekundäre hilfsprogrammroutine druckt jede Zeile.  
  
```  
'BeginSortVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
     ' connection and recordset variables  
    Dim Cnxn As New ADODB.Connection  
    Dim rstAuthors As New ADODB.Recordset  
    Dim strCnxn As String  
    Dim strSQLAuthors As String  
  
    Dim strTitle As String  
  
    ' Open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' open client-side recordset to enable sort method  
    Set rstAuthors = New ADODB.Recordset  
    rstAuthors.CursorLocation = adUseClient  
    strSQLAuthors = "SELECT * FROM Authors"  
    rstAuthors.Open strSQLAuthors, Cnxn, adOpenStatic, adLockReadOnly, adCmdText  
  
     ' sort the recordset last name ascending  
    rstAuthors.Sort = "au_lname ASC, au_fname ASC"  
     ' show output  
    Debug.Print "Last Name Ascending:"  
    Debug.Print "First Name  Last Name" & vbCr  
  
    rstAuthors.MoveFirst  
    Do Until rstAuthors.EOF  
        Debug.Print rstAuthors!au_fname & " " & rstAuthors!au_lname  
        rstAuthors.MoveNext  
    Loop  
  
     ' sort the recordset last name descending  
    rstAuthors.Sort = "au_lname DESC, au_fname ASC"  
     ' show output  
    Debug.Print "Last Name Descending"  
    Debug.Print "First Name  Last Name" & vbCr  
  
    Do Until rstAuthors.EOF  
        Debug.Print rstAuthors!au_fname & " " & rstAuthors!au_lname  
        rstAuthors.MoveNext  
    Loop  
  
    ' clean up  
    rstAuthors.Close  
    Cnxn.Close  
    Set rstAuthors = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rstAuthors Is Nothing Then  
        If rstAuthors.State = adStateOpen Then rstAuthors.Close  
    End If  
    Set rstAuthors = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndSortVB  
```  
  
 Dies ist die sekundäre Dienstprogramm Routine, die den angegebenen Titel und den Inhalt des angegebenen **Recordsets** ausgibt.  
  
```  
Attribute VB_Name = "Sort"  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Recordset-Objekt (ADO)](./recordset-object-ado.md)   
 [Sort-Eigenschaft](./sort-property.md)