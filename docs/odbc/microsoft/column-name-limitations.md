---
description: Einschränkungen für Spaltennamen
title: Einschränkungen für Spaltennamen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- desktop database drivers [ODBC], column names
- ODBC desktop database drivers [ODBC], column names
ms.assetid: 5a339f61-c52f-40ad-8deb-d785f72753d4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 46719ea0a0ff34e103252678e25242f010d9bd37
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190646"
---
# <a name="column-name-limitations"></a>Einschränkungen für Spaltennamen
Spaltennamen können beliebige gültige Zeichen (z. b. Leerzeichen) enthalten. Wenn Spaltennamen Zeichen enthalten, ausgenommen Buchstaben, Ziffern und Unterstriche, muss der Name begrenzt werden, indem er in backanführungs Zeichen (') eingeschlossen wird.  
  
 Wenn der Microsoft Access-oder Microsoft Excel-Treiber verwendet wird, sind die Spaltennamen auf 64 Zeichen beschränkt, und längere Namen generieren einen Fehler. Wenn der Paradox-Treiber verwendet wird, beträgt der maximale Spaltenname 25 Zeichen. Bei Verwendung des Text Treibers beträgt der maximale Spaltenname 64 Zeichen, und längere Namen werden abgeschnitten.  
  
 Wenn der dBase-Treiber verwendet wird, werden Zeichen mit einem ASCII-Wert größer als 127 in Unterstriche konvertiert.  
  
 Wenn der Microsoft Excel-Treiber verwendet wird, müssen sich die Spaltennamen in der ersten Zeile befinden, wenn Spaltennamen vorhanden sind. Ein Name, der in Microsoft Excel verwendet wird, muss das Zeichen "!" enthalten, das in backanführungs Zeichen (') eingeschlossen werden muss. Das Zeichen "!" wird in das Zeichen "$" konvertiert, da das Zeichen "!" in einem ODBC-Namen nicht zulässig ist, auch wenn der Name in backanführungs Zeichen eingeschlossen ist. Alle anderen gültigen Microsoft Excel-Zeichen (außer der senkrechter Strich (&#124;)) können in einem Spaltennamen verwendet werden, einschließlich Leerzeichen. Ein Begrenzungs Bezeichner muss für einen Microsoft Excel-Spaltennamen verwendet werden, um ein Leerzeichen einzuschließen. Nicht angegebene Spaltennamen werden durch Treiber generierte Namen ersetzt, z. b. "col1" für die erste Spalte.  
  
 Der senkrechter Strich (&#124;) kann nicht in einem Spaltennamen verwendet werden, unabhängig davon, ob der Name in rückzeichen eingeschlossen ist oder nicht.  
  
 Wenn der Text Treiber verwendet wird, stellt der Treiber einen Standardnamen bereit, wenn kein Spaltenname angegeben ist. Der Treiber ruft z. b. die erste Spalte (F1), die zweite Spalte F2 usw. auf.
