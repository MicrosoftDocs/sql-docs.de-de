---
description: Catalog ActiveConnection-Eigenschaft – Beispiel (VB)
title: Katalog ActiveConnection-Eigenschaft (Beispiel) (VB) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- ActiveConnection property [ADOX], Visual Basic example
ms.assetid: bb3274b1-764d-43a7-a49f-ef55680ecd26
author: rothja
ms.author: jroth
ms.openlocfilehash: 582c558222bec95914e73902b69fe9c7ce46161e
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88985239"
---
# <a name="catalog-activeconnection-property-example-vb"></a>Catalog ActiveConnection-Eigenschaft – Beispiel (VB)
Beim Festlegen der [ActiveConnection](./activeconnection-property-adox.md) -Eigenschaft auf eine gültige, geöffnete Verbindung wird der Katalog geöffnet. Von einem geöffneten Katalog aus können Sie auf die in diesem Katalog enthaltenen Schema Objekte zugreifen.  
  
```  
' BeginOpenConnectionVB  
Sub Main()  
    On Error GoTo OpenConnectionError  
  
    Dim cnn As New ADODB.Connection  
    Dim cat As New ADOX.Catalog  
  
    cnn.Open "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source= 'Northwind.mdb';"  
    Set cat.ActiveConnection = cnn  
    Debug.Print cat.Tables(0).Type  
  
    'Clean up  
    cnn.Close  
    Set cat = Nothing  
    Set cnn = Nothing  
    Exit Sub  
  
OpenConnectionError:  
  
    Set cat = Nothing  
  
    If Not cnn Is Nothing Then  
        If cnn.State = adStateOpen Then cnn.Close  
    End If  
    Set cnn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndOpenConnectionVB  
```  
  
 Wenn die **ActiveConnection** -Eigenschaft auf eine gültige Verbindungs Zeichenfolge festgelegt wird, wird auch der Katalog geöffnet.  
  
```  
Attribute VB_Name = "Catalog"  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ActiveConnection-Eigenschaft (ADOX)](./activeconnection-property-adox.md)   
 [Catalog-Objekt (ADOX)](./catalog-object-adox.md)   
 [Table-Objekt (ADOX)](./table-object-adox.md)   
 [Tables-Auflistung (ADOX)](./tables-collection-adox.md)   
 [Type-Eigenschaft (Tabelle) (ADOX)](./type-property-table-adox.md)