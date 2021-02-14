---
description: Index-Objekt (ADOX)
title: Index-Objekt (ADOX) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Index
helpviewer_keywords:
- Index object [ADOX]
ms.assetid: 6b9578c0-bc94-46b9-b801-c18e14b04b31
author: rothja
ms.author: jroth
ms.openlocfilehash: 4992a0236553f597c3926fd25c823cd6ba91433c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054125"
---
# <a name="index-object-adox"></a>Index-Objekt (ADOX)
Stellt einen Index aus einer Datenbanktabelle dar.  
  
## <a name="remarks"></a>Bemerkungen  
 Mit dem folgenden Code wird ein neuer **Index** erstellt:  
  
```  
Dim obj As New Index  
```  
  
 Mit den Eigenschaften und Auflistungen eines **Index** Objekts können Sie folgende Aktionen ausführen:  
  
-   Identifizieren Sie den Index mit der [Name](./name-property-adox.md) -Eigenschaft.  
  
-   Greifen Sie auf die Daten Bank Spalten des Indexes mit der [Columns](./columns-collection-adox.md) -Auflistung zu.  
  
-   Geben Sie an, ob die Index Schlüssel mit der [Unique](./unique-property-adox.md) -Eigenschaft eindeutig sein müssen.  
  
-   Geben Sie an, ob der Index der Primärschlüssel für eine Tabelle mit der [PrimaryKey](./primarykey-property-adox.md) -Eigenschaft ist.  
  
-   Geben Sie an, ob Datensätze mit NULL-Werten in ihren Indexfeldern Indexeinträge mit der [IndexNulls](./indexnulls-property-adox.md) -Eigenschaft aufweisen.  
  
-   Geben Sie an, ob der Index mit der [gruppierten](./clustered-property-adox.md) Eigenschaft gruppiert wird.  
  
-   Greifen Sie mit der [Properties](../ado-api/properties-collection-ado.md) -Auflistung auf anbieterspezifische Index Eigenschaften zu.  
  
> [!NOTE]
>  Wenn eine [Spalte](./column-object-adox.md) an die **Columns** -Auflistung eines **Indexes** angefügt wird, tritt ein Fehler auf, wenn die **Spalte** nicht in einem [Tabellen](./table-object-adox.md) Objekt vorhanden ist, das bereits an die [Tabellen](./tables-collection-adox.md) Auflistung angehängt ist.  
  
> [!NOTE]
>  Der Datenanbieter unterstützt möglicherweise nicht alle Eigenschaften von **Index** Objekten. Wenn Sie einen Wert für eine Eigenschaft festgelegt haben, die nicht vom Anbieter unterstützt wird, tritt ein Fehler auf. Bei neuen **Index** Objekten tritt der Fehler auf, wenn das Objekt an die Auflistung angefügt wird. Bei vorhandenen Objekten tritt der Fehler auf, wenn die-Eigenschaft festgelegt wird.  
  
> [!NOTE]
>  Wenn Sie **Index** Objekte erstellen, gewährleistet das vorhanden sein eines entsprechenden Standardwerts für eine optionale Eigenschaft nicht, dass der Anbieter die-Eigenschaft unterstützt. Weitere Informationen zu den Eigenschaften, die der Anbieter unterstützt, finden Sie in der Dokumentation des Anbieters.  
  
 Dieser Abschnitt enthält das folgende Thema.  
  
-   [Index-Objekt – Eigenschaften, Methoden und Ereignisse](./index-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für Index Append-Methode (VB)](./indexes-append-method-example-vb.md)   
 [Beispiel für IndexNulls-Eigenschaft (VB)](./indexnulls-property-example-vb.md)   
 [Beispiel für PrimaryKey und Unique Properties (VB)](./primarykey-and-unique-properties-example-vb.md)   
 [Sortorider-Eigenschafts Beispiel (VB)](./sortorder-property-example-vb.md)   
 [Columns-Auflistung (ADOX)](./columns-collection-adox.md)   
 [Indexes-Auflistung (ADOX)](./indexes-collection-adox.md)   
 [Properties-Collection (ADO)](../ado-api/properties-collection-ado.md)