---
description: ParentCatalog-Eigenschaft – Beispiel (VB)
title: Beispielcatalog-Eigenschaft (Beispiel) (VB) | Microsoft-Dokumentation
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
- ParentCatalog property [ADOX], Visual Basic example
ms.assetid: 448bc850-7584-4c5f-89f3-5f4fee88b259
author: rothja
ms.author: jroth
ms.openlocfilehash: 66e80c85d1361c99499c91685d46e2df29b59957
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2020
ms.locfileid: "88769809"
---
# <a name="parentcatalog-property-example-vb"></a>ParentCatalog-Eigenschaft – Beispiel (VB)
Der folgende Code veranschaulicht die Verwendung der Eigenschaft ["Eigenschaft"](./parentcatalog-property-adox.md) , um auf eine anbieterspezifische Eigenschaft zuzugreifen, bevor eine Tabelle an einen Katalog angehängt wird. Die-Eigenschaft ist " **AutoIncrement**", wodurch ein AutoIncrement-Feld in einer Microsoft Jet-Datenbank erstellt wird.  
  
```  
' BeginCreateAutoIncrColumnVB  
Sub Main()  
    On Error GoTo CreateAutoIncrColumnError  
  
    Dim cnn As New ADODB.Connection  
    Dim cat As New ADOX.Catalog  
    Dim tbl As New ADOX.Table  
  
    cnn.Open "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
    Set cat.ActiveConnection = cnn  
  
    With tbl  
        .Name = "MyContacts"  
        Set .ParentCatalog = cat  
        ' Create fields and append them to the new Table object.  
        .Columns.Append "ContactId", adInteger  
        ' Make the ContactId column and auto incrementing column  
        .Columns("ContactId").Properties("AutoIncrement") = True  
        .Columns.Append "CustomerID", adVarWChar  
        .Columns.Append "FirstName", adVarWChar  
        .Columns.Append "LastName", adVarWChar  
        .Columns.Append "Phone", adVarWChar, 20  
        .Columns.Append "Notes", adLongVarWChar  
    End With  
  
    cat.Tables.Append tbl  
    Debug.Print "Table 'MyContacts' is added."  
  
    ' Delete the table as this is a demonstration.  
    cat.Tables.Delete tbl.Name  
    Debug.Print "Table 'MyContacts' is delete."  
  
    'Clean up  
    cnn.Close  
    Set cat = Nothing  
    Set tbl = Nothing  
    Set cnn = Nothing  
    Exit Sub  
  
CreateAutoIncrColumnError:  
  
    Set cat = Nothing  
    Set tbl = Nothing  
  
    If Not cnn Is Nothing Then  
        If cnn.State = adStateOpen Then cnn.Close  
    End If  
    Set cnn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
  
End Sub  
' EndCreateAutoIncrColumnVB  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Append-Methode (ADOX-Spalten)](./append-method-adox-columns.md)   
 [Append-Methode (ADOX-Tabellen)](./append-method-adox-tables.md)   
 [Catalog-Objekt (ADOX)](./catalog-object-adox.md)   
 [Column-Objekt (ADOX)](./column-object-adox.md)   
 [Columns-Auflistung (ADOX)](./columns-collection-adox.md)   
 [Name-Eigenschaft (ADOX)](./name-property-adox.md)   
 [Para Catalog-Eigenschaft (ADOX)](./parentcatalog-property-adox.md)   
 [Table-Objekt (ADOX)](./table-object-adox.md)   
 [Type-Eigenschaft (Spalte) (ADOX)](./type-property-column-adox.md)