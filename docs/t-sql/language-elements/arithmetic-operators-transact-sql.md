---
description: Arithmetische Operatoren (Transact-SQL)
title: Arithmetische Operatoren (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], arithmetic
- arithmetic operators
- math operations [Transact-SQL]
ms.assetid: a41b92a5-1061-4e4d-bb3b-a180b73c88fa
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b51c473bb75f3f2e1c82f96dcd8af743597678fc
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124544"
---
# <a name="arithmetic-operators-transact-sql"></a>Arithmetische Operatoren (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Arithmetische Operatoren führen mathematische Operationen für zwei Ausdrücke eines oder mehrerer Datentypen durch. Sie werden von der numerischen Datentypkategorie aus ausgeführt. Weitere Informationen zu Datentypkategorien finden Sie unter [Transact-SQL-Syntaxkonventionen (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
|Operator|Bedeutung|  
|--------------|-------------|  
|[+ (Addition)](../../t-sql/language-elements/add-transact-sql.md)|Addition|  
|[- (Subtraktion)](../../t-sql/language-elements/subtract-transact-sql.md)|Subtraktion|  
|[* (Multiplikation)](../../t-sql/language-elements/multiply-transact-sql.md)|Multiplikation|  
|[/ (Dividieren)](../../t-sql/language-elements/divide-transact-sql.md)|Division|  
|[% (Modulus)](../../t-sql/language-elements/modulo-transact-sql.md)|Gibt den ganzzahligen Rest einer Division zurück. Beispiel: 12 % 5 = 2 (der Rest von 12 geteilt durch 5 ist 2).|  
  
Mit den Operatoren Plus (+) und Minus (-) können auch arithmetische Operationen für **datetime**- und **smalldatetime**-Werte ausgeführt werden.  
  
Weitere Informationen zu Genauigkeit und Dezimalstellen im Ergebnis einer arithmetischen Operation finden Sie unter [Genauigkeit, Dezimalstellen und Länge &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[Mathematische Funktionen &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)   
[Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
[Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
  
  
