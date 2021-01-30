---
description: Prozeduraufruf-Escapesequenz
title: Escapesequenz für Prozedur Aufrufe | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- escape sequences [ODBC], procedure call
- procedure call escape sequence [ODBC]
- ODBC escape sequences [ODBC], procedure call
ms.assetid: 269fbab0-e5f2-4a98-86c0-2d7b647acaae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a3177c134009ed6f8979637980e96f39ab9badd2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205327"
---
# <a name="procedure-call-escape-sequence"></a>Prozeduraufruf-Escapesequenz
ODBC verwendet Escapesequenzen für Prozedur Aufrufe. Die Syntax dieser Escapesequenz lautet wie folgt:  
  
 **{**[? =]**Aufrufen von** *Prozedur Name*[**(**[*Parameter*] [, [*Parameter*]]... **)**]**}**  
  
 In der BNF-Notation lautet die Syntax wie folgt:  
  
 *ODBC-Procedure-Escape* :: =  
  
 &#124; *ODBC-ESC-Initiator* [? =] *-aufrufsprozedur ODBC-ESC-Terminator*  
  
 *Procedure* :: = *Procedure-Name* &#124; *Procedure-Name* (*Procedure-Parameter-List*)  
  
 *Procedure-Identifier* :: = *benutzerdefinierter Name*  
  
 *Procedure-Name* :: = *Procedure-Identifier*  
  
 &#124; *Besitzer Name*. *Prozedur-Bezeichner*  
  
 &#124; Catalog- *Name Catalog-Separator* *Procedure-Identifier*  
  
 &#124; Catalog- *Name Catalog-Separator* [*Owner-Name*]. *Prozedur-Bezeichner*  
  
 (Die dritte Syntax ist nur gültig, wenn die Datenquelle keine Besitzer unterstützt.)  
  
 *Owner-Name* :: = *benutzerdefinierter Name*  
  
 *catalog-Name* :: = *benutzerdefinierter Name*  
  
 *catalog-Separator* :: = {von der *Implementierung definiertes*}  
  
 (Das Katalog Trennzeichen wird von **SQLGetInfo** mit der Option SQL_CATALOG_NAME_SEPARATOR Informationen zurückgegeben.)  
  
 *Procedure-Parameter-List* :: = *Procedure-Parameter*  
  
 &#124; *Procedure-Parameter*, *Procedure-Parameter-List*  
  
 *Procedure-Parameter* :: = *Dynamic-Parameter* &#124; *Literal&#124;* *leere Zeichenfolge*  
  
 *empty-string* :: =  
  
 *ODBC-ESC-Initiator* :: = {  
  
 *ODBC-ESC-Terminator* :: =}  
  
 (Wenn ein Prozedur Parameter eine leere Zeichenfolge ist, verwendet die Prozedur den Standardwert für diesen Parameter.)  
  
 Um zu ermitteln, ob die Datenquelle Prozeduren unterstützt und der Treiber die ODBC-Prozedur Aufruf Syntax unterstützt, kann eine Anwendung **SQLGetInfo** mit dem SQL_PROCEDURES Informationstyp aufrufen.
