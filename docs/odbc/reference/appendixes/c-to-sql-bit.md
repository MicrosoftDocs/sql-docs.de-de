---
description: 'C zu SQL: Bit'
title: 'C zu SQL: Bit | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from c to SQL types [ODBC], bit
- bit data type [ODBC]
- data conversions from C to SQL types [ODBC], bit
ms.assetid: 267c9fa9-599e-4ee6-b51b-0cae43f09183
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 71b4882a7253b651ff068b67a7a62d496b2971a2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212413"
---
# <a name="c-to-sql-bit"></a>C zu SQL: Bit
Der Bezeichner für den bitodbc-C-Datentyp lautet:  
  
 SQL_C_BIT  
  
 In der folgenden Tabelle werden die ODBC-SQL-Datentypen angezeigt, in die Bit C-Daten konvertiert werden können. Eine Erläuterung der Spalten und Begriffe in der Tabelle finden [Sie unter Datentypen von C in SQL-Datentypen](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|SQL-Typbezeichner|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR<br /><br /> SQL_WCHAR SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Keine|–|  
|SQL_DECIMAL SQL_NUMERIC<br /><br /> SQL_TINYINT SQL_SMALLINT<br /><br /> SQL_INTEGER SQL_BIGINT<br /><br /> SQL_REAL SQL_FLOAT<br /><br /> SQL_DOUBLE|Keine|–|  
|SQL_BIT|Keine|–|  
  
 Der Treiber ignoriert den Längen-/indikatorenwert beim Umrechnen von Daten aus dem Bit-c-Datentyp und geht davon aus, dass die Größe des Daten Puffers die Größe des Bit-c-Datentyps ist Der Wert für die Länge/den Indikator wird im *StrLen_Or_Ind* -Argument in **SQLPutData** und in dem Puffer übergeben, der mit dem *StrLen_or_IndPtr* -Argument in **SQLBindParameter** angegeben wird. Der Datenpuffer wird mit dem *DataPtr* -Argument in **SQLPutData** und dem *ParameterValuePtr* -Argument in **SQLBindParameter** angegeben.
