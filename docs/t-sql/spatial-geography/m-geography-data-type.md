---
description: M (geography-Datentyp)
title: M (geography-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- M (geography Data Type)
- M_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- M method
ms.assetid: cdba04f0-4e17-48f6-bafb-b1f918c5a501
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 5c1a8113fbdd4d4406ede4d06f6b036227fdf6b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159634"
---
# <a name="m-geography-data-type"></a>M (geography-Datentyp)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Der **M** (Measure)-Wert der **geography** -Instanz. Die Semantik des Measurewerts ist benutzerdefiniert, beschreibt jedoch im Allgemeinen die Entfernung entlang eines LineString. Zum Beispiel könnte der Measurewert dazu verwendet werden, Kilometersteine an einer Straße mitzuzählen.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
.M  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Typ: **float**  
  
 CLR-Typ: **SqlDouble**  
  
## <a name="remarks"></a>Bemerkungen  
 Der Wert dieser Eigenschaft ist NULL, wenn die **geography** -Instanz kein **Point** ist. Sie ist außerdem NULL für jede **Point** -Instanz, für die sie nicht festgelegt ist.  
  
 Diese Eigenschaft ist schreibgeschützt.  
  
 M-Werte werden in Berechnungen der Bibliothek nicht verwendet und werden nicht in Bibliotheksberechnungen einbezogen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird eine `Point` -Instanz mit Z (Höhe)- und M (Measure)-Werten erstellt und `M` verwendet, um den `M` -Wert der Instanz abzurufen.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POINT(-122.34900 47.65100 10.3 12)', 4326);  
SELECT @g.M;  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Erweiterte Methoden für geography-Instanzen](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [Z &#40;geography-Datentyp&#41;](../../t-sql/spatial-geography/z-geography-data-type.md)  
  
  
