---
description: DataSource-Eigenschaft (ADO)
title: DataSource-Eigenschaft (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Recordset20::DataSource
helpviewer_keywords:
- DataSource property [ADO]
ms.assetid: 300a702a-3544-48c5-b759-83b511fe97e0
author: rothja
ms.author: jroth
ms.openlocfilehash: 48c5969df864364cd87d131fce2740a5a0e043f7
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88974261"
---
# <a name="datasource-property-ado"></a>DataSource-Eigenschaft (ADO)
Gibt ein Objekt an, das Daten enthält, die als [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) -Objekt dargestellt werden.  
  
## <a name="remarks"></a>Bemerkungen  
 Diese Eigenschaft wird verwendet, um Daten gebundene Steuerelemente mit der Daten Umgebung zu erstellen. In der Daten Umgebung werden Auflistungen von Daten (Datenquellen) verwaltet, die benannte Objekte (Datenmember) enthalten, die als **Recordset** -Objekt dargestellt werden.  
  
 Die Eigenschaften [DataMember](../../../ado/reference/ado-api/datamember-property.md) und **DataSource** müssen zusammen verwendet werden.  
  
 Das Objekt, auf das verwiesen wird, muss die **IDataSource** -Schnittstelle implementieren und eine **IRowset** -Schnittstelle enthalten.  
  
## <a name="usage"></a>Verwendung  
  
```  
Dim rs as New ADODB.Recordset  
rs.DataMember = "Command"     'Name of the rowset to bind to.  
Set rs.DataSource = myDE      'Name of the object containing an IRowset.  
```  
  
## <a name="applies-to"></a>Gilt für  
 [Recordset-Objekt (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [DataMember-Eigenschaft](../../../ado/reference/ado-api/datamember-property.md)
