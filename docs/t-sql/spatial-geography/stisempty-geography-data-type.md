---
description: STIsEmpty (geography-Datentyp)
title: STIsEmpty (geography-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STIsEmpty_TSQL
- STIsEmpty (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STIsEmpty method
ms.assetid: 4cbc66e3-9035-4ecf-8f5a-6301f168c26c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 434e2ed6bb3c324a2cd3e8b7cf9bf3b4c7b8254c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158886"
---
# <a name="stisempty-geography-data-type"></a>STIsEmpty (geography-Datentyp)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt 1 zurück, wenn eine **geography** -Instanz leer ist. Gibt 0 zurück, wenn eine **geography** -Instanz nicht leer ist.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
.STIsEmpty ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **bit**  
  
 CLR-Rückgabetyp: **SqlBoolean**  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird eine leere `geography` -Instanz erstellt und `STIsEmpty()` verwendet, um zu prüfen, ob die Instanz leer ist.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POLYGON EMPTY', 4326);  
SELECT @g.STIsEmpty();  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [OGC-Methoden für geography-Instanzen](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
