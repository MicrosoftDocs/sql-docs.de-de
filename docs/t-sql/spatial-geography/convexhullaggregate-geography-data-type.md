---
description: ConvexHullAggregate (geography-Datentyp)
title: ConvexHullAggregate (geography-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ConvexHullAggregate_TSQL
- ConvexHullAggregate
dev_langs:
- TSQL
helpviewer_keywords:
- ConvexHullAggregate method (geography)
ms.assetid: 21784c66-2725-471b-9e2d-a8c2e3695197
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: fba09550ac86e630ccf1e29083cbb7565ef2265e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159653"
---
# <a name="convexhullaggregate-geography-data-type"></a>ConvexHullAggregate (geography-Datentyp)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Gibt eine konvexe Hülle für einen angegebenen Satz von **geography** -Objekten zurück.
  
## <a name="syntax"></a>Syntax  
  
```  
  
ConvexHullAggregate ( geography_operand )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *geography_operand*  
 Eine Tabellenspalte vom Typ **geography** , die einen Satz von **geography** -Objekten darstellt.  
  
## <a name="return-types"></a>Rückgabetypen  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **geography**  
  
## <a name="exception"></a>Ausnahme  
 Löst eine `FormatException` aus, wenn ungültige Eingabewerte vorhanden sind. Weitere Informationen finden Sie unter [STIsValid &#40;geography-Datentyp&#41;](../../t-sql/spatial-geography/stisvalid-geography-data-type.md).  
  
## <a name="remarks"></a>Hinweise  
 Diese Methode gibt **null** zurück, wenn die Eingabe leer ist oder andere SRIDs aufweist. Weitere Informationen finden Sie unter [SRIDs &#40;Spatial Reference Identifiers&#41;](../../relational-databases/spatial/spatial-reference-identifiers-srids.md).  
  
 Diese Methode ignoriert **null** -Eingaben.  
  
> [!NOTE]  
>  Diese Methode gibt **null** zurück, wenn alle eingegebenen Werte **null** sind.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die konvexe Hülle des Satzes von **geography** -Objekten zurückgegeben.  
  
 ```
 USE AdventureWorks2012  
 GO  
 SELECT geography::ConvexHullAggregate(SpatialLocation).ToString() AS SpatialLocation  
 FROM Person.Address  
 WHERE City LIKE ('Bothell')
 ```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erweiterte statische geography-Methoden](../../t-sql/spatial-geography/extended-static-geography-methods.md)  
  
  
