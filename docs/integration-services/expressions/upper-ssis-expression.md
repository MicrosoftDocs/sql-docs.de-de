---
description: UPPER (SSIS-Ausdruck)
title: UPPER (SSIS-Ausdruck) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- UPPER function
- converting lowercase to uppercase
- uppercase characters [Integration Services]
- lowercase characters
ms.assetid: d33985f7-1048-4023-83e4-274090acda78
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bd1a7c2d716f3043693840097dd832dec143cd70
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88484363"
---
# <a name="upper-ssis-expression"></a>UPPER (SSIS-Ausdruck)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Gibt einen Zeichenausdruck zurück, nachdem Kleinbuchstaben in Großbuchstaben konvertiert wurden.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
UPPER(character_expression)  
```  
  
## <a name="arguments"></a>Argumente  
 *character_expression*  
 Ein Zeichenausdruck zum Konvertieren in Großbuchstaben.  
  
## <a name="result-types"></a>Ergebnistypen  
 DT_WSTR  
  
## <a name="remarks"></a>Bemerkungen  
 UPPER kann nur mit dem DT_WSTR-Datentyp verwendet werden. Ein *character_expression* -Argument, das ein Zeichenfolgenliteral oder eine Datenspalte mit dem DT_STR-Datentyp ist, wird implizit in den DT_WSTR-Datentyp umgewandelt, bevor UPPER ausgeführt wird. Andere Datentypen müssen explizit in den DT_WSTR-Datentyp umgewandelt werden. Weitere Informationen finden Sie unter [Integration Services-Datentypen](../../integration-services/data-flow/integration-services-data-types.md) und [CAST &#40;SSIS-Ausdruck&#41;](../../integration-services/expressions/cast-ssis-expression.md).  
  
 UPPER gibt ein NULL-Ergebnis zurück, wenn das Argument NULL ist.  
  
## <a name="expression-examples"></a>Beispiele für Ausdrücke  
 In diesem Beispiel wird ein Zeichenfolgenliteral in Großbuchstaben konvertiert. Als Ergebnis wird "HELLO" zurückgegeben.  
  
```  
UPPER("hello")  
```  
  
 In diesem Beispiel wird das erste Zeichen in der **FirstName** -Spalte in einen Großbuchstaben konvertiert. Wenn **FirstName** gleich anna ist, wird als Ergebnis "A" zurückgegeben. Weitere Informationen finden Sie unter [SUBSTRING &#40;SSIS-Ausdruck&#41;](../../integration-services/expressions/substring-ssis-expression.md).  
  
```  
UPPER(SUBSTRING(FirstName, 1, 1))  
```  
  
 In diesem Beispiel wird der Wert in der **PostalCode** -Variablen in Großbuchstaben konvertiert. Wenn **PostalCode** gleich k4b1s2 ist, wird als Ergebnis "K4B1S2" zurückgegeben.  
  
```  
UPPER(@PostalCode)  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [LOWER &#40;SSIS-Ausdruck&#41;](../../integration-services/expressions/lower-ssis-expression.md)   
 [Funktionen &#40;SSIS-Ausdruck&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
