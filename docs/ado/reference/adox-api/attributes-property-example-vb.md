---
description: Attributes-Eigenschaft – Beispiel (VB)
title: Beispiel für Attribute-Eigenschaft (VB) | Microsoft-Dokumentation
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
- Attributes property [ADOX], Visual Basic example
ms.assetid: c0ed8195-09af-42c8-99c7-038ecc8a5c9f
author: rothja
ms.author: jroth
ms.openlocfilehash: 2dfc8f93b2c185b530a142dde8206c77799818c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050381"
---
# <a name="attributes-property-example-vb"></a>Attributes-Eigenschaft – Beispiel (VB)
Dieses Beispiel veranschaulicht die [Attribute](./attributes-property-adox.md) -Eigenschaft einer [Spalte](./column-object-adox.md). Wenn Sie es auf **adcolnullable** festlegen, kann der Benutzer den Wert eines [recordsetfelds](../ado-api/recordset-object-ado.md) [](../ado-api/field-object.md) auf eine leere Zeichenfolge festlegen. In dieser Situation kann der Benutzer zwischen einem Datensatz unterscheiden, bei dem keine Daten bekannt sind, und einem Datensatz, in dem die Daten nicht zutreffen.  
  
```  
' BeginAttributesVB  
Sub Main()  
    On Error GoTo AttributesXError  
  
    Dim cnn As New ADODB.Connection  
    Dim cat As New ADOX.Catalog  
    Dim colTemp As New ADOX.Column  
    Dim rstEmployees As New Recordset  
    Dim strMessage As String  
    Dim strInput As String  
    Dim tblEmp As ADOX.Table  
  
    ' Connect the catalog.  
    cnn.Open "Provider='Microsoft.Jet.OLEDB.4.0';data source=" & _  
        "'Northwind.mdb';"  
    Set cat.ActiveConnection = cnn  
  
    Set tblEmp = cat.Tables("Employees")  
  
    ' Create a new Field object and append it to the Fields  
    ' collection of the Employees table.  
    colTemp.Name = "FaxPhone"  
    colTemp.Type = adVarWChar  
    colTemp.DefinedSize = 24  
    colTemp.Attributes = adColNullable  
    cat.Tables("Employees").Columns.Append colTemp.Name, adWChar, 24  
  
    ' Open the Employees table for updating as a Recordset  
    rstEmployees.Open "Employees", cnn, adOpenKeyset, adLockOptimistic, adCmdTable  
  
    With rstEmployees  
        ' Get user input.  
        strMessage = "Enter fax number for " & _  
            !FirstName & " " & !LastName & "." & vbCr & _  
            "[? - unknown, X - has no fax]"  
        strInput = UCase(InputBox(strMessage))  
        If strInput <> "" Then  
            Select Case strInput  
                Case "?"  
                    !FaxPhone = Null  
                Case "X"  
                    !FaxPhone = ""  
                Case Else  
                    !FaxPhone = strInput  
            End Select  
            .Update  
  
            ' Print report.  
            Debug.Print "Name - Fax number"  
            Debug.Print !FirstName & " " & !LastName & " - ";  
  
            If IsNull(!FaxPhone) Then  
                Debug.Print "[Unknown]"  
            Else  
                If !FaxPhone = "" Then  
                    Debug.Print "[Has no fax]"  
                Else  
                    Debug.Print !FaxPhone  
                End If  
            End If  
  
        End If  
  
        .Close  
    End With  
  
    'Clean up  
    tblEmp.Columns.Delete colTemp.Name  
    cnn.Close  
    Set rstEmployees = Nothing  
    Set cat = Nothing  
    Set colTemp = Nothing  
    Set cnn = Nothing  
    Exit Sub  
  
AttributesXError:  
  
    If Not rstEmployees Is Nothing Then  
        If rstEmployees.State = adStateOpen Then rstEmployees.Close  
    End If  
    Set rstEmployees = Nothing  
  
    If Not tblEmp Is Nothing Then tblEmp.Columns.Delete colTemp.Name  
  
    Set cat = Nothing  
    Set colTemp = Nothing  
  
    If Not cnn Is Nothing Then  
        If cnn.State = adStateOpen Then cnn.Close  
    End If  
    Set cnn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
  
End Sub  
' EndAttributesVB  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Attribute-Eigenschaft (ADOX)](./attributes-property-adox.md)   
 [Catalog-Objekt (ADOX)](./catalog-object-adox.md)   
 [Column-Objekt (ADOX)](./column-object-adox.md)