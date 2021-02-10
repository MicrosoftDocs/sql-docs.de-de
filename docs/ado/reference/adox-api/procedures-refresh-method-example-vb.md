---
description: Refresh-Methode für Prozeduren – Beispiel (VB)
title: Prozeduren Refresh Method example (VB) | Microsoft-Dokumentation
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
- Refresh method [ADOX], Visual Basic example
ms.assetid: 499679bd-287b-487d-bdfb-3803abffec1c
author: rothja
ms.author: jroth
ms.openlocfilehash: 289cee12f65511cf64a68837043eacb37d2f2763
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054845"
---
# <a name="procedures-refresh-method-example-vb"></a>Refresh-Methode für Prozeduren – Beispiel (VB)
Der folgende Code zeigt, wie die [Prozeduren](./procedures-collection-adox.md) Auflistung eines [Katalogs](./catalog-object-adox.md)aktualisiert wird. Dies ist erforderlich, bevor auf [Prozedur](./procedure-object-adox.md) Objekte aus dem **Katalog** zugegriffen werden kann.  
  
```  
' BeginProceduresRefreshVB  
Sub Main()  
    On Error GoTo ProcedureRefreshError  
  
    Dim cat As New ADOX.Catalog  
  
    ' Open the Catalog  
    cat.ActiveConnection = _  
        "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Refresh the Procedures collection  
    cat.Procedures.Refresh  
  
    'Clean up  
    Set cat.ActiveConnection = Nothing  
    Set cat = Nothing  
    Exit Sub  
  
ProcedureRefreshError:  
    Set cat = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndProceduresRefreshVB  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Catalog-Objekt (ADOX)](./catalog-object-adox.md)   
 [Prozeduren (ADOX)](./procedures-collection-adox.md)   
 [Refresh-Methode (ADO)](../ado-api/refresh-method-ado.md)