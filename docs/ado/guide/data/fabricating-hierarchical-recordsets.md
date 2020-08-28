---
description: Herstellen hierarchischer Recordsets
title: Fabrizieren von hierarchischen Recordsets | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset fabrication [ADO]
- hierarchical Recordsets [ADO]
- fabricating hierarchical Recordsets [ADO]
- data shaping [ADO], hierarchical Recordsets
ms.assetid: a584e642-a4a3-418e-bc20-3aff81a5625a
author: rothja
ms.author: jroth
ms.openlocfilehash: 24941f8fbf2aedb5fb61cea176ef26d3172012cc
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88991281"
---
# <a name="fabricating-hierarchical-recordsets"></a>Herstellen hierarchischer Recordsets
Im folgenden Beispiel wird gezeigt, wie ein hierarchisches Recordset ohne eine zugrunde liegende Datenquelle mit der Daten Strukturierungs Grammatik zum Definieren von Spalten für übergeordnete, untergeordnete und untergeordnete **Recordsets**von untergeordneten Elementen fabriziert werden kann.  
  
 Zum fabrizieren eines hierarchischen **Recordsets**müssen Sie den Microsoft-Daten Strukturierungs [Dienst für OLE DB (ADO-Dienstanbieter)](../appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) (MSDataShape) angeben, und Sie können im Verbindungs Zeichen folgen Parameter der [Open](../../reference/ado-api/open-method-ado-connection.md) -Methode des [Connection](../../reference/ado-api/connection-object-ado.md) -Objekts den Datenanbieter Wert None angeben. Weitere Informationen finden Sie unter [erforderliche Anbieter für die Daten Strukturierung](./required-providers-for-data-shaping.md).  
  
```  
Dim cn As New ADODB.Connection  
Dim rsCustomers As New ADODB.Recordset  
  
cn.Open "Provider=MSDataShape;Data Provider=NONE;"  
  
strShape = _  
"SHAPE APPEND NEW adInteger AS CustID," & _  
            " NEW adChar(25) AS FirstName," & _  
            " NEW adChar(25) AS LastName," & _  
            " NEW adChar(12) AS SSN," & _  
            " NEW adChar(50) AS Address," & _  
         " ((SHAPE APPEND NEW adChar(80) AS VIN_NO," & _  
                        " NEW adInteger AS CustID," & _  
                        " NEW adChar(20) AS BodyColor, " & _  
                     " ((SHAPE APPEND NEW adChar(80) AS VIN_NO," & _  
                                    " NEW adChar(20) AS Make, " & _  
                                    " NEW adChar(20) AS Model," & _  
                                    " NEW adChar(4) AS Year) " & _  
                        " AS VINS RELATE VIN_NO TO VIN_NO))" & _  
            " AS Vehicles RELATE CustID TO CustID) "  
  
rsCustomers.Open strShape, cn, adOpenStatic, adLockOptimistic, -1  
```  
  
 Sobald das **Recordset** erfunden wurde, kann es aufgefüllt, manipuliert oder persistent in einer Datei gespeichert werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Zugreifen auf Zeilen in einem hierarchischen Recordset](./accessing-rows-in-a-hierarchical-recordset.md)   
 [Formale Form Grammatik](./formal-shape-grammar.md)   
 [Erforderliche Anbieter für die Daten Strukturierung](./required-providers-for-data-shaping.md)   
 [Shape-APPEND-Klausel](./shape-append-clause.md)   
 [Shape-Befehle im Allgemeinen](./shape-commands-in-general.md)