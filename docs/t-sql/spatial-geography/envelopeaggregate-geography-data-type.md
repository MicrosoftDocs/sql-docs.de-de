---
description: EnvelopeAggregate (geography-Datentyp)
title: EnvelopeAggregate (geography-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- EnvelopeAggregate_TSQL
- EnvelopeAggregate
dev_langs:
- TSQL
helpviewer_keywords:
- EnvelopeAggregate method (geography)
ms.assetid: 4947797f-edb8-490f-beca-37df9ec06954
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 9b14db5a616f41049b5cabc845cd822356e34522
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96114994"
---
# <a name="envelopeaggregate-geography-data-type"></a>EnvelopeAggregate (geography-Datentyp)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Gibt für einen angegebenen Satz von **geography** -Objekten ein umgebendes Objekt zurück. Das resultierende **geography** -Objekt enthält mehrere Kreisbogensegmente.
  
## <a name="syntax"></a>Syntax  
  
```  
  
EnvelopeAggregate ( geography_operand )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *geography_operand*  
 Eine Tabellenspalte vom Typ **geography** , die den Satz von **geography** -Objekten enthält, für den eine Umschlagaggregierung ausgeführt werden soll.  
  
## <a name="return-types"></a>Rückgabetypen  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **geography**  
  
## <a name="remarks"></a>Bemerkungen  
 Ein **FullGlobe** -Objekt wird zurückgegeben, wenn das resultierende umgebende Objekt größer als eine Hemisphäre ist. Diese Methode ist nicht exakt.  
  
 Die Methode gibt **null** zurück, wenn die Eingabe abweichende SRIDs aufweist. Weitere Informationen finden Sie unter [SRIDs &#40;Spatial Reference Identifiers&#41;](../../relational-databases/spatial/spatial-reference-identifiers-srids.md).  
  
 Diese Methode ignoriert **null** -Eingaben.  
  
> [!NOTE]  
>  Diese Methode gibt **null** zurück, wenn alle eingegebenen Werte **null** sind.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird ein `EnvelopeAggregate` für eine Reihe von **geography** -Standorten in einer Stadt durchgeführt.  
  
 ```
 USE AdventureWorks2012  
 GO  
 SELECT City,  
 geography::EnvelopeAggregate(SpatialLocation) AS SpatialLocation  
 FROM Person.Address  
 WHERE PostalCode LIKE('981%')  
 GROUP BY City;
 ```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erweiterte statische geography-Methoden](../../t-sql/spatial-geography/extended-static-geography-methods.md)  
  
  
