---
description: AllowNullsEnum
title: AllowNullsEnum | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- AllowNullsEnum
helpviewer_keywords:
- AllowNullsEnum enumeration [ADOX]
ms.assetid: 6acf3689-1a7f-4379-9d7f-df452ccbac27
author: rothja
ms.author: jroth
ms.openlocfilehash: c91de05e128468135d846aaa588624295a584b04
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050561"
---
# <a name="allownullsenum"></a>AllowNullsEnum
Gibt an, ob Datensätze mit NULL-Werten indiziert werden.  
  
|Konstante|Wert|BESCHREIBUNG|  
|--------------|-----------|-----------------|  
|**adindexnullsallow**|0|Der Index lässt Einträge zu, bei denen die Schlüssel Spalten NULL sind. Wenn ein NULL-Wert in einer Schlüssel Spalte eingegeben wird, wird der Eintrag in den Index eingefügt.|  
|**adIndexNullsDisallow**|1|Standard. Der Index lässt keine Einträge zu, bei denen die Schlüssel Spalten NULL sind. Wenn ein NULL-Wert in einer Schlüssel Spalte eingegeben wird, tritt ein Fehler auf.|  
|**adindexnullsignore**|2|Der Index fügt keine Einträge ein, die NULL-Schlüssel enthalten. Wenn ein NULL-Wert in einer Schlüssel Spalte eingegeben wird, wird der Eintrag ignoriert, und es tritt kein Fehler auf.|  
|**adindexnullsignoreany**|4|Der Index fügt keine Einträge ein, bei denen einige Schlüssel Spalten einen NULL-Wert haben. Bei einem Index mit einem mehrspaltigen Schlüssel wird der Eintrag ignoriert, wenn ein NULL-Wert in einer Spalte eingegeben wird und kein Fehler auftritt.|  
  
## <a name="applies-to"></a>Gilt für  
 [IndexNulls-Eigenschaft (ADOX)](./indexnulls-property-adox.md)