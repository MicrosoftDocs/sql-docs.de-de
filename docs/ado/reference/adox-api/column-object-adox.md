---
description: Column-Objekt (ADOX)
title: Column-Objekt (ADOX) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Column
helpviewer_keywords:
- Column object [ADOX]
ms.assetid: 6e772783-1bc8-4ea7-94b2-7d7a52ea5c47
author: rothja
ms.author: jroth
ms.openlocfilehash: 7501381174ddf522b60a596cd5846f012d59d88e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164254"
---
# <a name="column-object-adox"></a>Column-Objekt (ADOX)
Stellt eine Spalte aus einer Tabelle, einem Index oder einem Schlüssel dar.  
  
## <a name="remarks"></a>Bemerkungen  
 Der folgende Code erstellt eine neue **Spalte**:  
  
 `Dim obj As New Column`  
  
 Mit den Eigenschaften und Auflistungen eines **Spalten** Objekts können Sie folgende Aktionen ausführen:  
  
-   Identifizieren Sie die Spalte mit der Eigenschaft [Name Property (ADOX)](./name-property-adox.md) .  
  
-   Geben Sie den Datentyp der Spalte mit der Eigenschaft [Type Property (Key) (ADOX)](./type-property-key-adox.md) an.  
  
-   Bestimmen Sie, ob die Spalte eine festgelegte Länge hat oder ob Sie NULL-Werte mit der Eigenschaft " [Attributeigenschaft" (ADOX)](./attributes-property-adox.md) enthalten kann.  
  
-   Geben Sie die maximale Größe der Spalte mit der Eigenschaft [DefinedSize (ADOX)](./definedsize-property-adox.md) an.  
  
-   Geben Sie für numerische Datenwerte die Skala mit der Eigenschaft " [NumericScale Property (ADOX)](./numericscale-property-adox.md) " an.  
  
-   Geben Sie als Wert für numerische Daten die maximale Genauigkeit mit der Eigenschaft für die [Precision-Eigenschaft (ADOX)](./precision-property-adox.md) an.  
  
-   Geben Sie das [Catalog-Objekt (ADOX)](./catalog-object-adox.md) an, das die Spalte besitzt, mit der Eigenschaft " [Eigenschaft" (ADOX)](./parentcatalog-property-adox.md) .  
  
-   Geben Sie für Schlüssel Spalten den Namen der verknüpften Spalte in der verknüpften Tabelle mit der Eigenschaft [RelatedColumn-Eigenschaft (ADOX)](./relatedcolumn-property-adox.md) an.  
  
-   Geben Sie für Index Spalten an, ob die Sortierreihenfolge aufsteigend oder absteigend mit der Eigenschaft [sortor der Eigenschaft (ADOX)](./sortorder-property-adox.md) ist.  
  
-   Zugreifen auf anbieterspezifische Eigenschaften mit der [Properties Collection (ADO)](../ado-api/properties-collection-ado.md) -Auflistung.  
  
> [!NOTE]
>  Nicht alle Eigenschaften von **Spalten** Objekten können von Ihrem Datenanbieter unterstützt werden. Wenn Sie einen Wert für eine Eigenschaft festgelegt haben, die vom Anbieter nicht unterstützt wird, tritt ein Fehler auf. Bei neuen **Spalten** Objekten tritt der Fehler auf, wenn das Objekt an die Auflistung angefügt wird. Bei vorhandenen Objekten tritt der Fehler auf, wenn die-Eigenschaft festgelegt wird.  
>   
>  Beim Erstellen von **Spalten** Objekten garantiert das vorhanden sein eines entsprechenden Standardwerts für eine optionale Eigenschaft nicht, dass der Anbieter die-Eigenschaft unterstützt. Weitere Informationen zu den Eigenschaften, die der Anbieter unterstützt, finden Sie in der Dokumentation des Anbieters.  
  
 Dieser Abschnitt enthält das folgende Thema.  
  
-   [Column-Objekt – Eigenschaften, Methoden und Ereignisse](./column-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Methoden und Tabellen Append-Methoden, Name Property example (VB)](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [Connection Close-Methode, Table Type-Eigenschafts Beispiel (VB)](./connection-close-method-table-type-property-example-vb.md)   
 [Keys Append-Methode, Schlüsseltyp, RelatedColumn, RelatedTable und UpdateRule Properties-Beispiel (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [ADOX-Code Beispiel: Beispiel für NumericScale und Precision Properties (VB)](./adox-code-example-numericscale-and-precision-properties-example-vb.md)   
 [Beispiel für eine Beispiel Katalog Eigenschaft (VB)](./parentcatalog-property-example-vb.md)   
 [Sortorider-Eigenschafts Beispiel (VB)](./sortorder-property-example-vb.md)   
 [Columns-Auflistung (ADOX)](./columns-collection-adox.md)   
 [Properties-Collection (ADO)](../ado-api/properties-collection-ado.md)