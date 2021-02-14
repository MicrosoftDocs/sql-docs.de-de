---
description: Cellset-Objekt (ADO MD)
title: CellSet-Objekt (ADO MD) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cellset
helpviewer_keywords:
- Cellset object [ADO MD]
ms.assetid: 5e2452c0-cac0-49b2-8099-836c35794d50
author: rothja
ms.author: jroth
ms.openlocfilehash: dc64756d2b2ac679e185c4dff8888352a7865f83
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100055725"
---
# <a name="cellset-object-ado-md"></a>Cellset-Objekt (ADO MD)
Stellt die Ergebnisse einer mehrdimensionalen Abfrage dar. Es handelt sich um eine Sammlung von Zellen, die aus Cubes oder anderen Cellsets ausgewählt wurden.  
  
## <a name="remarks"></a>Bemerkungen  
 Daten in einem **Cellset** werden mit direktem, Array ähnlichen Zugriff abgerufen. Sie können einen Drilldown zu einem bestimmten Member durchführen, um Daten zu diesem Element abzurufen. Der folgende Code gibt z. b. die Beschriftung des ersten Elements an der ersten Position auf der ersten Achse eines Cellsets mit dem Namen zurück `cst` :  
  
```  
cst.Axes(0).Positions(0).Members(0).Caption  
```  
  
 Es gibt kein Konzept einer aktiven Zelle in einem CellSet. Stattdessen ruft die [Item](./item-property-ado-md-cellset.md) -Eigenschaft ein bestimmtes [Cell](./cell-object-ado-md.md) -Objekt aus dem Cellset ab. Die Argumente der **Item** -Eigenschaft bestimmen, welche Zelle abgerufen wird. Sie können den eindeutigen Ordinalwert einer Zelle angeben. Sie können auch Zellen abrufen, indem Sie Ihre Positionsnummern entlang der einzelnen Achsen der Cellsets verwenden. Weitere Informationen zum Abrufen von Zellen finden Sie unter der [Item](./item-property-ado-md-cellset.md) -Eigenschaft.  
  
 Mit den Auflistungen, Methoden und Eigenschaften eines **Cellset** -Objekts können Sie folgende Aufgaben ausführen:  
  
-   Ordnen Sie eine geöffnete Verbindung einem **Cellset** -Objekt zu, indem Sie die zugehörige [ActiveConnection](./activeconnection-property-ado-md.md) -Eigenschaft festlegen.  
  
-   Führen Sie aus, und rufen Sie die Ergebnisse einer mehrdimensionalen Abfrage mit der [Open](./open-method-ado-md.md) -Methode ab.  
  
-   Ruft eine **Zelle** aus dem **Cellset** mit der [Item](./item-property-ado-md-cellset.md) -Eigenschaft ab.  
  
-   Gibt die [Achsen](./axis-object-ado-md.md) Objekte zurück, die das **Cellset** mit der [Achsen](./axes-collection-ado-md.md) Auflistung definieren.  
  
-   Rufen Sie Informationen zu den Dimensionen ab, die zum Filtern der Daten im **Cellset** mit der [FilterAxis](./filteraxis-property-ado-md.md) -Eigenschaft verwendet werden.  
  
-   Gibt die Abfrage zurück, die zum Definieren des **Cellsets** mit der [Source](./source-property-ado-md.md) -Eigenschaft verwendet wird.  
  
-   Gibt den aktuellen Status des **Cellsets** (öffnen, schließen, ausführen oder verbinden) mit der [State](./state-property-ado-md.md) -Eigenschaft zurück.  
  
-   Schließen Sie ein geöffnetes **Cellset** mit der [Close](./close-method-ado-md.md) -Methode.  
  
-   Abrufen von anbieterspezifischen Informationen über das **Cellset** mit der standardmäßigen ADO [Properties](../ado-api/properties-collection-ado.md) -Auflistung.  
  
 Dieser Abschnitt enthält das folgende Thema.  
  
-   [Eigenschaften, Methoden und Ereignisse](./cellset-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Cellset-Beispiel (VB)](./cellset-example-vb.md)   
 [Achsen Auflistung (ADO MD)](./axes-collection-ado-md.md)   
 [Cell-Objekt (ADO MD)](./cell-object-ado-md.md)   
 [Verbindungs Objekt (ADO)](../ado-api/connection-object-ado.md)   
 [Properties-Collection (ADO)](../ado-api/properties-collection-ado.md)