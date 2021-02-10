---
description: Erforderliche Anbieter für die Datenstrukturierung
title: Erforderliche Anbieter für die Daten Strukturierung | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], data shaping
- data shaping [ADO], providers required
ms.assetid: d49d48d2-ac2d-4c11-895c-5a149b444620
author: rothja
ms.author: jroth
ms.openlocfilehash: eb3d91f23666c900f59a6ffc8e6b94af9bcdb4d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032532"
---
# <a name="required-providers-for-data-shaping"></a>Erforderliche Anbieter für die Datenstrukturierung
Die Daten Strukturierung erfordert in der Regel zwei Anbieter. Der Dienstanbieter, der Daten Strukturierungs [Dienst für OLE DB](../../../ado/guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md), stellt die Daten Strukturierungs Funktionalität bereit, und ein Datenanbieter, wie z. b. der OLE DB Anbieter für SQL Server, stellt Daten Zeilen bereit, um das geformte [Recordset aufzufüllen](../../../ado/reference/ado-api/recordset-object-ado.md).  
  
 Der Name des Dienstanbieters (MSDataShape) kann als Wert der Eigenschaft [Verbindungs](../../../ado/reference/ado-api/connection-object-ado.md) Objekt [Anbieter](../../../ado/reference/ado-api/provider-property-ado.md) oder Verbindungs Zeichenfolge-Schlüsselwort "Provider = MSDataShape;" angegeben werden.  
  
 Der Name des Datenanbieters kann als Wert der dynamischen Eigenschaft **Datenanbieter** angegeben werden, die der Auflistung der **Verbindungs** Objekt [Eigenschaften](../../../ado/reference/ado-api/properties-collection-ado.md) durch den Daten Strukturierungs Dienst für OLE DB oder das Verbindungs Zeichenfolgen-Schlüsselwort "**Datenanbieter =** _Provider_" hinzugefügt wird.  
  
 Es ist kein Datenanbieter erforderlich, wenn das **Recordset** nicht aufgefüllt ist (z. b. wie in einem fertigen **Recordset** , bei dem Spalten mit dem New-Schlüsselwort erstellt werden). Geben Sie in diesem Fall "**Datenanbieter =** None;" an.  
  
## <a name="example"></a>Beispiel  
  
```  
Dim cnn As New ADODB.Connection  
cnn.Provider = "MSDataShape"  
cnn.Open "Data Provider=SQLOLEDB;Integrated Security=SSPI;Database=Northwind"  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für Daten Strukturierung](../../../ado/guide/data/data-shaping-example.md)   
 [Formale Form Grammatik](../../../ado/guide/data/formal-shape-grammar.md)   
 [Shape-Befehle im Allgemeinen](../../../ado/guide/data/shape-commands-in-general.md)
