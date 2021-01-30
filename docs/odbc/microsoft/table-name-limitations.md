---
description: Einschränkungen für Tabellennamen
title: Einschränkungen für Tabellennamen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, table name limitations
- table name limitations [ODBC]
- Excel driver [ODBC], table name limitations
ms.assetid: d9843e4b-1d05-4d5a-9dc3-ee9ec59edb97
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1b3a13919a20a8092c730ce3dfbe35358d1898df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182552"
---
# <a name="table-name-limitations"></a>Einschränkungen für Tabellennamen
Tabellennamen können beliebige gültige Zeichen (z. b. Leerzeichen) enthalten. Wenn Tabellennamen Zeichen enthalten, ausgenommen Buchstaben, Ziffern und Unterstriche, muss der Name begrenzt werden, indem er in backanführungs Zeichen (') eingeschlossen wird.  
  
 Wenn der Microsoft Excel-Treiber verwendet wird und ein Tabellenname nicht durch einen Daten Bank Verweis qualifiziert ist, wird die Standarddatenbank impliziert. Wenn ein Name in Microsoft Excel das Zeichen "!" enthält, wird er stattdessen automatisch in das Zeichen "$" übersetzt.  
  
 Der Microsoft Excel-Tabellenname, auf den verweist, \<filename> wird für Microsoft Excel 3,0-und 4,0-Dateien unterstützt. Der Microsoft Excel-Tabellenname, auf den verweist, \<workbook-name> wird für Microsoft Excel 5,0-, 7,0-oder 97-Dateien unterstützt.  
  
 Wenn der dBase-Treiber verwendet wird, werden Zeichen mit einem ASCII-Wert größer als 127 in Unterstriche konvertiert.  
  
 Wenn der Microsoft Access-Treiber verwendet wird, ist der Tabellenname auf 64 Zeichen beschränkt.  
  
 Wenn die dBASE-, Microsoft Excel 3,0-oder 4,0-, Paradox-oder Text-Treiber verwendet werden, sollten die speziellen MS-DOS-Schlüsselwörter con, AUX, LPT1 und LPT2 nicht als Tabellennamen verwendet werden.
