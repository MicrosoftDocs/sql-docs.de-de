---
description: Views-Collection (ADOX)
title: Views-Auflistung (ADOX) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Catalog::Views
- Views
helpviewer_keywords:
- Views collection [ADOX]
ms.assetid: a55d380c-2b7b-4b57-af74-8ba0b3de0db9
author: rothja
ms.author: jroth
ms.openlocfilehash: c4ac183994402714383320305c64d30a27f3acb5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049600"
---
# <a name="views-collection-adox"></a>Views-Collection (ADOX)
Enthält alle [Ansichts](./view-object-adox.md) Objekte eines Katalogs.  
  
## <a name="remarks"></a>Bemerkungen  
 Die [Append](./append-method-adox-views.md) -Methode für eine **views** -Auflistung ist für ADOX eindeutig. Ihre Möglichkeiten:  
  
-   Fügen Sie der Auflistung mithilfe der **Append** -Methode eine neue Ansicht hinzu.  
  
 Die restlichen Eigenschaften und Methoden sind Standard für ADO-Auflistungen. Ihre Möglichkeiten:  
  
-   Greifen Sie mit der [Item](../ado-api/item-property-ado.md) -Eigenschaft auf eine Ansicht in der Auflistung zu.  
  
-   Gibt die Anzahl der in der Auflistung enthaltenen Sichten mit der [count](../ado-api/count-property-ado.md) -Eigenschaft zurück.  
  
-   Entfernen Sie eine Ansicht aus der Auflistung mit der [Delete](./delete-method-adox-collections.md) -Methode.  
  
-   Aktualisieren Sie die Objekte in der Auflistung, um das aktuelle Datenbankschema mit der [Refresh](../ado-api/refresh-method-ado.md) -Methode widerzuspiegeln.  
  
 Dieser Abschnitt enthält das folgende Thema.  
  
-   [Views-Collection – Eigenschaften, Methoden und Ereignisse](./views-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für Sichten und Fields-Auflistungen (VB)](./views-and-fields-collections-example-vb.md)   
 [Beispiele für die Append-Methode (VB)](./views-append-method-example-vb.md)   
 [Views-Auflistung, CommandText-Eigenschafts Beispiel (VB)](./views-collection-commandtext-property-example-vb.md)   
 [Sichten Delete-Methode (Beispiel) (VB)](./views-delete-method-example-vb.md)   
 [Ansichten Aktualisierungs Methode (Beispiel) (VB)](./views-refresh-method-example-vb.md)   
 [Catalog-Objekt (ADOX)](./catalog-object-adox.md)   
 [View-Objekt (ADOX)](./view-object-adox.md)