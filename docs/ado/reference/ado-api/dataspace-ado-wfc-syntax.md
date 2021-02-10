---
description: DataSpace (ADO/WFC-Syntax)
title: DataSpace (ADO-WFC-Syntax) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- DataSpace collection [ADO], ADO/WFC syntax
ms.assetid: 950d45d8-07de-467b-b255-f9a7b997204c
author: rothja
ms.author: jroth
ms.openlocfilehash: 40207634f4111852bc280823826c108c49fde070
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034440"
---
# <a name="dataspace-ado---wfc-syntax"></a>DataSpace (ADO/WFC-Syntax)
Die Methode " **kreateobject** " der **DataSpace** -Klasse gibt sowohl ein Geschäftsobjekt an, um Client Anwendungsanforderungen (*ProgID*) als auch das Kommunikationsprotokoll und den Server (*Verbindung*) zu verarbeiten. " **kreateobject** " gibt ein [ObjectProxy](../../../ado/reference/ado-api/objectproxy-ado-wfc-syntax.md) -Objekt zurück, das den Server darstellt.  
  
## <a name="package-commswfcdata"></a>Paket "com. ms. wfc. Data"  
  
### <a name="constructor"></a>Konstruktor  
  
```  
public DataSpace()  
```  
  
### <a name="methods"></a>Methoden  
  
```  
public static ObjectProxy DataSpace.createObject(String  
    progid, String connection)  
```  
  
### <a name="properties"></a>Eigenschaften  
  
```  
public static int getInternetTimeout()  
public static void setInternetTimeout(int plInetTimeout)  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [DataSpace-Objekt (RDS)](../../../ado/reference/rds-api/dataspace-object-rds.md)
