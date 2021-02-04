---
description: toString-Methode (DateTimeOffset)
title: toString-Methode (DateTimeOffset) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e77b9be3-1a02-4769-8acf-ac71d48d6a76
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4599b8152bbb7041565cc4858199ec88d1bb0510
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158824"
---
# <a name="tostring-method-datetimeoffset"></a>toString-Methode (DateTimeOffset)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Gibt eine Zeichenfolgendarstellung des **DateTimeOffset**-Objekts zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public String toString()  
```  
  
## <a name="return-value"></a>Rückgabewert  
 Eine Zeichenfolgendarstellung des **DateTimeOffset**-Objekts  
  
## <a name="remarks"></a>Bemerkungen  
 Die Zeichenfolge weist folgendes Format auf: `YYYY-MM-DD HH:mm:ss[.fffffff] [+|-]HH:mm`.  
  
 Die Sekundenbruchteile der zurückgegebenen Zeichenfolge werden bis zur angegebenen Genauigkeit mit Nullen aufgefüllt. Ein **datetimeoffset(6)** mit dem Wert „2010-03-10 12:34:56.78 -08:00“ wird beispielsweise von DateTimeOffset.toString wie folgt formatiert: „2010-03-10 12:34:56.780000 -08:00“.  
  
## <a name="see-also"></a>Weitere Informationen  
 [DateTimeOffset-Klasse](../../../connect/jdbc/reference/datetimeoffset-class.md)   
 [DateTimeOffset-Elemente](../../../connect/jdbc/reference/datetimeoffset-members.md)  
  
  
