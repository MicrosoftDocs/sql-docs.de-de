---
description: Parameterdatentypen
title: Parameter Datentypen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], parameters
- parameter data type [ODBC]
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: fd7e99d8-d26a-408c-9733-6ffccde99f75
author: David-Engel
ms.author: v-daenge
ms.reviewer: ''
ms.openlocfilehash: fbc01ee7f6c4e7cd3ca7dbe4deb6e78e5ff78449
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160830"
---
# <a name="parameter-data-types"></a>Parameterdatentypen
Obwohl jeder Parameter, der mit **SQLBindParameter** angegeben wird, mit einem SQL-Datentyp definiert wird, haben die Parameter in einer SQL-Anweisung keinen systeminternen Datentyp. Daher können Parameter Markierungen nur in eine SQL-Anweisung eingeschlossen werden, wenn Ihre Datentypen von einem anderen Operanden in der Anweisung abgeleitet werden können. Beispielsweise in einem arithmetischen Ausdruck wie? + COLUMN1, der Datentyp des Parameters kann aus dem Datentyp der benannten Spalte abgeleitet werden, die durch COLUMN1 dargestellt wird. Eine Anwendung kann keine Parameter Markierung verwenden, wenn der Datentyp nicht bestimmt werden kann.  
  
 In der folgenden Tabelle wird beschrieben, wie ein-Datentyp in Übereinstimmung mit SQL-92 für verschiedene Typen von Parametern bestimmt wird. Eine umfassendere Spezifikation zum Ableiten des Parameter Typs, wenn andere SQL-Klauseln verwendet werden, finden Sie in der SQL-92-Spezifikation.  
  
|Speicherort des Parameters|Übernommene Datentyp|  
|---------------------------|-----------------------|  
|Ein Operand eines binären arithmetischen oder Vergleichs Operators|Identisch mit dem anderen Operanden|  
|Der erste Operand in einer **between** -Klausel.|Identisch mit dem zweiten Operanden|  
|Der zweite oder dritte Operand in einer **between** -Klausel.|Identisch mit dem ersten Operanden|  
|Ein Ausdruck, **der mit in** verwendet wird|Identisch mit dem ersten Wert oder der Ergebnisspalte der Unterabfrage.|  
|Ein mit **in** verwendeter Wert.|Identisch mit dem Ausdruck oder dem ersten Wert, wenn ein Parametermarker im Ausdruck vorhanden ist.|  
|Ein mit **like** verwendeter Musterwert|VARCHAR|  
|Ein Update Wert, der mit **Update** verwendet wird.|Identisch mit der Update-Spalte|
