---
description: SQLStatistics (Textdateitreiber)
title: SQLStatistics (Text Datei Treiber) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- text file driver [ODBC], SQLStatistics
- SQLStatistics function [ODBC], Text File Driver
ms.assetid: 311afc01-d656-425f-be43-4a8e7cbc9a97
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 82ea614fceb2d3af108fc592aa4c068d355bd1fa
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88500123"
---
# <a name="sqlstatistics-text-file-driver"></a>SQLStatistics (Textdateitreiber)
> [!NOTE]  
>  In diesem Thema werden Treiber spezifische Informationen zu Textdateien bereitstellt. Allgemeine Informationen zu dieser Funktion finden Sie im entsprechenden Thema unter [ODBC-API-Referenz](../../odbc/reference/syntax/odbc-api-reference.md).  
  
|Column|Kommentare|  
|------------|--------------|  
|TABLE_QUALIFIER|Der Pfad zu einem Verzeichnis.<br /><br /> Der Musterabgleich wird im *sztablequalifier* -Argument nicht unterstützt.|  
|TABLE_OWNER|In dieser Spalte wird NULL zurückgegeben, da der Besitzer Name nicht unterstützt wird.|  
|table_name|Nicht durch Trennzeichen getrennter Tabellenname.<br /><br /> Der Musterabgleich wird im Argument " *sztablename* " nicht unterstützt.|  
|INDEX_QUALIFIER|NULL wird immer zurückgegeben.|  
|INDEX_NAME|Index abhängig.|  
|TYPE|Für den Typ werden nur SQL_TABLE_STAT oder SQL_INDEX_OTHER zurückgegeben.|  
|SEQ_IN_INDEX|Index abhängig.|  
|COLUMN_NAME|Index abhängig.|  
|COLLATION|Index abhängig.|  
|PAGES|NULL wird immer zurückgegeben.|  
  
 Das Filtern basiert auf der Eindeutigkeit (dem " *f Unique* "-Argument). Der *fakcurracy* -Parameter wird ignoriert.
