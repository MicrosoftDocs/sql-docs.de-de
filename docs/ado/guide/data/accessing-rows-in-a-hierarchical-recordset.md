---
description: Zugreifen auf Zeilen in einem hierarchischen Recordset (Beispiel)
title: Zugreifen auf Zeilen in einem hierarchischen Recordset | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchical Recordsets [ADO]
- data shaping [ADO], hierarchical Recordsets
ms.assetid: 25f1d2a1-6d5e-4457-aa07-5db5c75dee18
author: rothja
ms.author: jroth
ms.openlocfilehash: 36ad54e1768b5164294d5de9767757ef3f376144
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88991781"
---
# <a name="accessing-rows-in-a-hierarchical-recordset-example"></a>Zugreifen auf Zeilen in einem hierarchischen Recordset (Beispiel)
Das folgende Beispiel zeigt die erforderlichen Schritte für den Zugriff auf Zeilen in einem hierarchischen [Recordset](../../reference/ado-api/recordset-object-ado.md):

1.  **Recordset** -Objekte aus den Tabellen " **Authors** " und " **titleauthor** " sind durch die Autoren-ID verknüpft.

2.  Die äußere Schleife zeigt den vor-und Nachnamen, den Status und die Identifizierung jedes Autors an.

3.  Das angefügte **Recordset** für jede Zeile wird aus der [Fields](../../reference/ado-api/fields-collection-ado.md) -Auflistung abgerufen und *rsttitleauthor*zugewiesen.

4.  Die innere Schleife zeigt vier Felder aus jeder Zeile im angefügten **Recordset**an.

 Die [StayInSync](../../reference/ado-api/stayinsync-property.md) -Eigenschaft ist zur Veranschaulichung auf **false** festgelegt, sodass Sie das Kapitel ändern in jeder Iterations Schleife der äußeren Schleife explizit sehen können. Um das Codebeispiel effizienter zu gestalten, können Sie die Zuweisung in Schritt 3 vor der ersten Zeile in Schritt 2 verschieben, sodass die Zuweisung nur einmal ausgeführt wird. Legen Sie dann die [StayInSync](../../reference/ado-api/stayinsync-property.md) -Eigenschaft auf **true**fest, damit *rsttitleauthor* implizit und automatisch in das entsprechende Kapitel wechselt, wenn *RST* zu einer neuen Zeile wechselt.

## <a name="example"></a>Beispiel

```
Sub datashape()
   Dim cnn As New ADODB.Connection
   Dim rst As New ADODB.Recordset
   Dim rstTitleAuthor As New ADODB.Recordset

   cnn.Provider = "MSDataShape"
   cnn.Open    "Data Provider=MSDASQL;" & _
               "Data Source=SRV;Integrated Security=SSPI;Database=Pubs"
' STEP 1
   rst.StayInSync = FALSE
   rst.Open    "SHAPE  {select * from authors} "  & _
               "APPEND ({select * from titleauthor} " & _
               "RELATE au_id TO au_id) AS chapTitleAuthor", _
               cnn
' STEP 2
   While Not rst.EOF
      Debug.Print    rst("au_fname"), rst("au_lname"), _
                     rst("state"), rst("au_id")
' STEP 3
      Set rstTitleAuthor = rst("chapTitleAuthor").Value
' STEP 4
      While Not rstTitleAuthor.EOF
         Debug.Print rstTitleAuthor(0), rstTitleAuthor(1), _
                     rstTitleAuthor(2), rstTitleAuthor(3)
         rstTitleAuthor.MoveNext
      Wend
      rst.MoveNext
   Wend
End Sub
```

## <a name="see-also"></a>Weitere Informationen
 [Übersicht über die Daten Strukturierung](./data-shaping-overview.md) [Field Object](../../reference/ado-api/field-object.md) [Fields Collection (ADO)](../../reference/ado-api/fields-collection-ado.md) [formal Shape Grammar](./formal-shape-grammar.md) [Microsoft Data Strukturierung Service for OLE DB (ADO Service Provider)](../appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) [(ADO)](../../reference/ado-api/recordset-object-ado.md) [erforderliche Anbieter für das Strukturieren von Daten strukturieren von](./required-providers-for-data-shaping.md) [Shape APPEND Clause](./shape-append-clause.md) [Form Befehlen in der allgemeinen](./shape-commands-in-general.md) [Form COMPUTE-Klausel](./shape-compute-clause.md) [Visual Basic for Applications Functions](./visual-basic-for-applications-functions.md)