---
title: Multilookup-Funktion (Berichts-Generator) | Microsoft-Dokumentation
description: Die Multilookup-Funktion gibt im Berichts-Generator mehrere der ersten übereinstimmenden Werte für bestimmte Namen aus einem Dataset zurück, das Name-Wert-Paare enthält.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 1fec079e-33b3-4e4d-92b3-6b4d06a49a77
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b8db1d2a7fe18264c81d7585e02babef65b3346d
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364572"
---
# <a name="report-builder-functions---multilookup-function"></a>Funktionen des Berichts-Generators: Multilookup-Funktion
  Gibt den Satz der ersten übereinstimmenden Werte für den angegebenen Satz von Namen aus einem Dataset mit Name-Wert-Paaren zurück.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="syntax"></a>Syntax  
  
```  
  
Multilookup(source_expression, destination_expression, result_expression, dataset)  
```  
  
#### <a name="parameters"></a>Parameter  
 *source_expression*  
 ( **VariantArray** ) Ein Ausdruck, der im aktuellen Bereich ausgewertet wird, und der den Satz der zu suchenden Namen oder Schlüssel angibt. Beispiel für einen mehrwertigen Parameter: `=Parameters!IDs.value`.  
  
 *destination_expression*  
 ( **Variant** ) Ein Ausdruck, der für jede Zeile in einem Dataset ausgewertet wird, und der den Namen oder den Schlüssel für die Übereinstimmung angibt. Beispiel: `=Fields!ID.Value`.  
  
 *result_expression*  
 ( **Variant** ) Ein Ausdruck, der für die Zeile im Dataset ausgewertet wird, für die *source_expression* = *destination_expression* gilt, und der den abzurufenden Wert angibt. Beispiel: `=Fields!Name.Value`.  
  
 *Dataset (dataset)*  
 Eine Konstante, die den Namen eines Datasets im Bericht angibt. Beispiel: "Colors".  
  
## <a name="return"></a>Rückgabewert  
 Gibt einen Wert vom Typ **VariantArray** zurück; gibt **Nothing** zurück, wenn keine Übereinstimmung vorhanden ist.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie **Multilookup** , um eine Wertemenge aus einem Dataset für Name-Wert-Paare abzurufen, in dem jedes Paar über eine 1:1-Beziehung verfügt. **MultiLookup** ist mit dem Aufrufen von **Lookup** für eine Menge von Namen oder Schlüsseln vergleichbar. Beispiel: Für einen mehrwertigen Parameter, der auf Primärschlüsselbezeichnern basiert, können Sie **Multilookup** in einem Ausdruck in einem Textfeld in einer Tabelle verwenden, um zugeordnete Werte aus einem Dataset abzurufen, das nicht an den Parameter oder die Tabelle gebunden ist.  
  
 Mit **Multilookup** wird Folgendes ausgeführt:  
  
-   Der Quellausdruck wird im aktuellen Bereich ausgewertet, und ein Array von Variant-Objekten wird generiert.  
  
-   Für jedes Objekt im Array wird die [Lookup-Funktion (Berichts-Generator und SSRS)](../../reporting-services/report-design/report-builder-functions-lookup-function.md) aufgerufen und dem Rückgabearray das Ergebnis hinzugefügt.  
  
-   Der Satz von Ergebnissen wird zurückgegeben.  
  
 Verwenden Sie die [Lookup-Funktion (Berichts-Generator und SSRS)](../../reporting-services/report-design/report-builder-functions-lookup-function.md), um einen einzelnen Wert aus einem Dataset mit Name-Wert-Paaren für einen angegebenen Namen abzurufen, wenn eine 1:1-Beziehung vorhanden ist. Verwenden Sie die [LookupSet-Funktion (Berichts-Generator und SSRS)](../../reporting-services/report-design/report-builder-functions-lookupset-function.md), um mehrere Werte aus einem Dataset mit Name-Wert-Paaren für einen Namen abzurufen, wenn eine 1:n-Beziehung besteht.  
  
 Es gelten folgende Einschränkungen:  
  
-   **Multilookup** wird ausgewertet, nachdem alle Filterausdrücke angewendet wurden.  
  
-   Nur eine Suchebene wird unterstützt. Ein Quell-, Ziel- oder Ergebnisausdruck kann keinen Verweis auf eine Suchfunktion einschließen.  
  
-   Quell- und Zielausdrücke müssen den gleichen Datentyp ergeben.  
  
-   Quell-, Ziel- und Ergebnisausdrücke können keine Verweise auf Berichts- oder Gruppenvariablen einschließen.  
  
-   **Multilookup** kann nicht als Ausdruck für die folgenden Berichtselemente verwendet werden:  
  
    -   Dynamische Verbindungszeichenfolgen für eine Datenquelle.  
  
    -   Berechnete Felder in einem Dataset.  
  
    -   Abfrageparameter in einem Dataset.  
  
    -   Filter in einem Dataset.  
  
    -   Berichtsparameter.  
  
    -   Die Eigenschaft „Report.Language“.  
  
 Weitere Informationen finden Sie in der [Aggregatfunktionsreferenz (Berichts-Generator und SSRS)](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md) und unter [Ausdrucksbereich für Gesamtwerte, Aggregate und integrierte Auflistungen (Berichts-Generator und SSRS)](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
## <a name="examples"></a>Beispiele

### <a name="a-use-multilookup-function"></a>A. Verwenden der MultiLookup-Funktion
 Angenommen, ein Dataset mit dem Namen "Category" enthält das Feld "CategoryList", das eine durch Trennzeichen getrennte Liste von Kategoriebezeichnern enthält, wie z. B. "2, 4, 2, 1".  
  
 Das Dataset "CategoryNames" enthält den Kategoriebezeichner und den Kategorienamen, wie in der folgenden Tabelle gezeigt.  
  
|id|Name|  
|--------|----------|  
|1|Accessories|  
|2|Bikes|  
|3|Clothing|  
|4|Komponenten|  
  
 Verwenden Sie für die Suche der Namen, die der Liste der Bezeichner entsprechen, **Multilookup**. Sie müssen zuerst die Liste in ein Zeichenfolgenarray aufteilen, **Multilookup** aufrufen, um die Kategorienamen abzurufen, und die Ergebnisse zu einer Zeichenfolge verketten.  
  
 Wenn der folgende Ausdruck in ein Textfeld in einem Datenbereich platziert wird, der an das Dataset "Category" gebunden ist, wird "Bikes, Components, Bikes, Accessories" angezeigt:  
  
```  
=Join(MultiLookup(Split(Fields!CategoryList.Value,","),  
   Fields!CategoryID.Value,Fields!CategoryName.Value,"Category")),  
   ", ")  
```  
  
### <a name="b-use-multilookup-with-multivalue-parameter"></a>B. Verwenden von MultiLookup mit mehrwertigen Parametern  
 Angenommen, ein Dataset "ProductColors" enthält ein Farbbezeichnerfeld "ColorID" und ein Farbwertfeld "Color", wie in der folgenden Tabelle gezeigt.  
  
|ColorID|Color|  
|-------------|-----------|  
|1|Red|  
|2|Blau|  
|3|Grün|  
  
 Angenommen, der mehrwertige Parameter *MyColors* ist nicht an ein Dataset für seine verfügbaren Werte gebunden. Die Standardwerte für den Parameter sind auf 2 und 3 festgelegt. Wenn der folgende Ausdruck in einem Textfeld in einer Tabelle platziert wird, werden die ausgewählten Werte für den Parameter zu einer durch Trennzeichen getrennten Liste verkettet, und es wird "Blue, Green" angezeigt.  
  
```  
=Join(MultiLookup(Parameters!MyColors.Value,Fields!ColorID.Value,Fields!Color.Value,"ProductColors"),", ")  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ausdrucksverwendungen in Berichten &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Beispiele für Ausdrücke &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Datentypen in Ausdrücken (Berichts-Generator und SSRS)](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)   
 [Ausdrucksbereich für Gesamtwerte, Aggregate und integrierte Sammlungen &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)  
  
  
