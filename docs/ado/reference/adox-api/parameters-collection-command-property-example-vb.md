---
description: Parameters-Collection, Command-Eigenschaft – Beispiel (VB)
title: Parameter Auflistung, Beispiel für Befehls Eigenschaft (VB) | Microsoft-Dokumentation
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
- Command property [ADOX], Visual Basic example
ms.assetid: 7df1089e-69b7-476e-9244-19947c087351
author: rothja
ms.author: jroth
ms.openlocfilehash: 2ec79579b0474db9baa6d128333842a03235388a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053965"
---
# <a name="parameters-collection-command-property-example-vb"></a>Parameters-Collection, Command-Eigenschaft – Beispiel (VB)
Der folgende Code veranschaulicht die Verwendung der [Command](./command-property-adox.md) -Eigenschaft mit dem [Command](../ado-api/command-object-ado.md) -Objekt zum Abrufen von Parameterinformationen für die Prozedur.  
  
```  
' BeginParametersVB  
Sub Main()  
    On Error GoTo ProcedureParametersError  
  
    Dim cnn As New ADODB.Connection  
    Dim cmd As ADODB.Command  
    Dim prm As ADODB.Parameter  
    Dim cat As New ADOX.Catalog  
  
    ' Open the Connection  
    cnn.Open _  
        "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Open the catalog  
    Set cat.ActiveConnection = cnn  
  
    ' Get the command object  
    Set cmd = cat.Procedures("CustomerById").Command  
  
    ' Retrieve Parameter information  
    cmd.Parameters.Refresh  
    For Each prm In cmd.Parameters  
        Debug.Print prm.Name & ":" & prm.Type  
    Next  
  
    'Clean up  
    cnn.Close  
    Set cat = Nothing  
    Set cmd = Nothing  
    Set cnn = Nothing  
    Exit Sub  
  
ProcedureParametersError:  
  
    Set cat = Nothing  
    Set cmd = Nothing  
  
    If Not cnn Is Nothing Then  
        If cnn.State = adStateOpen Then cnn.Close  
    End If  
    Set cnn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndParametersVB  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ActiveConnection-Eigenschaft (ADOX)](./activeconnection-property-adox.md)   
 [Catalog-Objekt (ADOX)](./catalog-object-adox.md)   
 [Command-Eigenschaft (ADOX)](./command-property-adox.md)   
 [Procedure-Objekt (ADOX)](./procedure-object-adox.md)   
 [Procedures-Collection (ADOX)](./procedures-collection-adox.md)