---
description: Append-Methode für Schlüssel, Key Type-, RelatedColumn-, RelatedTable- und UpdateRule-Eigenschaften – Beispiel (VB)
title: Erstellen einer neuen Fremdschlüssel Beziehung zwischen Tabellen Beispiel (VB) | Microsoft-Dokumentation
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
- Key Type property [ADOX], Visual Basic example
- RelatedTable property [ADOX], Visual Basic example
- Keys Append method [ADOX], Visual Basic example
- UpdateRule property [ADOX], Visual Basic example
- RelatedColumn property [ADOX], Visual Basic example
ms.assetid: 13b5b1c3-6af6-439e-bb65-976578ba6bc2
author: rothja
ms.author: jroth
ms.openlocfilehash: a09e3eca2bec72ec61a9811133a65f18af1b7893
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054065"
---
# <a name="keys-append-method-key-type-relatedcolumn-relatedtable-and-updaterule-properties-example-vb"></a>Append-Methode für Schlüssel, Key Type-, RelatedColumn-, RelatedTable- und UpdateRule-Eigenschaften – Beispiel (VB)
Der folgende Code veranschaulicht das Erstellen einer neuen Fremdschlüssel Beziehung zwischen zwei vorhandenen Tabellennamens **Customers** und **Orders**.  
  
```  
' BeginCreateKeyVB  
Sub Main()  
    On Error GoTo CreateKeyError  
  
    Dim kyForeign As New ADOX.Key  
    Dim cat As New ADOX.Catalog  
  
    ' Connect to the catalog.  
    cat.ActiveConnection = "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Define the foreign key.  
    kyForeign.Name = "CustOrder"  
    kyForeign.Type = adKeyForeign  
    kyForeign.RelatedTable = "Customers"  
    kyForeign.Columns.Append "CustomerId"  
    kyForeign.Columns("CustomerId").RelatedColumn = "CustomerId"  
    kyForeign.UpdateRule = adRICascade  
  
    ' Append the foreign key to the keys collection.  
    cat.Tables("Orders").Keys.Append kyForeign  
  
    'Delete the key t demonstrate the Delete method.  
    cat.Tables("Orders").Keys.Delete kyForeign.Name  
  
    'Clean up.  
    Set cat.ActiveConnection = Nothing  
    Set cat = Nothing  
    Set kyForeign = Nothing  
    Exit Sub  
  
CreateKeyError:  
    Set cat = Nothing  
    Set kyForeign = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
  
End Sub  
' EndCreateKeyVB  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Append-Methode (ADOX-Spalten)](./append-method-adox-columns.md)   
 [Append-Methode (ADOX-Schlüssel)](./append-method-adox-keys.md)   
 [Catalog-Objekt (ADOX)](./catalog-object-adox.md)   
 [Column-Objekt (ADOX)](./column-object-adox.md)   
 [Columns-Auflistung (ADOX)](./columns-collection-adox.md)   
 [Key-Objekt (ADOX)](./key-object-adox.md)   
 [Keys-Auflistung (ADOX)](./keys-collection-adox.md)   
 [Name-Eigenschaft (ADOX)](./name-property-adox.md)   
 [RelatedColumn-Eigenschaft (ADOX)](./relatedcolumn-property-adox.md)   
 [RelatedTable-Eigenschaft (ADOX)](./relatedtable-property-adox.md)   
 [Table-Objekt (ADOX)](./table-object-adox.md)   
 [Tables-Auflistung (ADOX)](./tables-collection-adox.md)   
 [Type-Eigenschaft (Key) (ADOX)](./type-property-key-adox.md)   
 [UpdateRule-Eigenschaft (ADOX)](./updaterule-property-adox.md)