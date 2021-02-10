---
description: Refresh-Methode für Sichten – Beispiel (VB)
title: Ansichten Aktualisierungs Methode (Beispiel) (VB) | Microsoft-Dokumentation
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
- Refresh method [ADOX]
ms.assetid: cdad2d66-6ade-40dc-9e74-e40cfa9bc127
author: rothja
ms.author: jroth
ms.openlocfilehash: 0d68a27408a6066ec98851c20df3f03b77381e68
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049560"
---
# <a name="views-refresh-method-example-vb"></a>Refresh-Methode für Sichten – Beispiel (VB)
Der folgende Code zeigt, wie die [views](./views-collection-adox.md) -Auflistung eines [Katalogs](./catalog-object-adox.md)aktualisiert wird. Dies ist erforderlich, bevor [auf Sicht](./view-object-adox.md) Objekte aus dem **Katalog** zugegriffen werden kann.  
  
```  
' BeginViewsRefreshVB  
Sub Main()  
    On Error GoTo ProcedureViewsRefreshError  
  
    Dim cat As New ADOX.Catalog  
  
    ' Open the catalog.  
    cat.ActiveConnection = "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Refresh the Procedures collection.  
    cat.Views.Refresh  
  
    'Clean up  
    Set cat = Nothing  
    Exit Sub  
  
ProcedureViewsRefreshError:  
  
    If Not cat Is Nothing Then  
        Set cat = Nothing  
    End If  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndViewsRefreshVB  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Refresh-Methode (ADO)](../ado-api/refresh-method-ado.md)   
 [Views-Collection (ADOX)](./views-collection-adox.md)