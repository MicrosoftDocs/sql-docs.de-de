---
description: 'SQL zu C: GUID'
title: 'SQL zu C: GUID | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], GUID
- data conversions from SQL to C types [ODBC], guid
- GUID data type [ODBC]
ms.assetid: cf56c684-c261-4b89-994a-db14ab2241d6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9b55b88414dfa78b80c49987af6dab82ba4abeda
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203031"
---
# <a name="sql-to-c-guid"></a>SQL zu C: GUID
Der Bezeichner für den GUID ODBC-SQL-Datentyp lautet:  
  
 SQL_GUID  
  
 In der folgenden Tabelle werden die ODBC-C-Datentypen angezeigt, in die GUID-SQL-Daten konvertiert werden können. Eine Erläuterung der Spalten und Begriffe in der Tabelle finden [Sie unter Datentypen von SQL in C-Datentypen](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|C-Typbezeichner|Test|**Targetvalueptr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|Länge des *pufflength* -> Zeichens|Daten|36|–|  
||*BufferLength* < 37|Nicht definiert|Nicht definiert|22003|  
|SQL_C_WCHAR|Länge der *BufferLength* -> Zeichen|Daten|36|–|  
||*BufferLength* < 37|Nicht definiert|Nicht definiert|22003|  
|SQL_C_BINARY|Byte Länge der Daten \< =  *Pufferlänge*|Daten|Länge der Daten in Bytes|–|  
||Byte Länge der Daten > *BufferLength*|Nicht definiert|Nicht definiert|22003|  
|SQL_C_GUID|Keine [a]|Daten|16 [b]|–|  
  
 [a] der Wert von *BufferLength* wird für diese Konvertierung ignoriert. Der Treiber geht davon aus, dass die Größe von **targetvalueptr* die Größe des C-Datentyps ist.  
  
 [b] Dies ist die Größe des entsprechenden C-Datentyps.
