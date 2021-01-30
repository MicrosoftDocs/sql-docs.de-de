---
description: DELETE (SQL-Befehl)
title: DELETE-SQL-Befehl | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DELETE [ODBC]
ms.assetid: 0d5bd477-626f-4f22-a05a-f531d9f8c5e7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42c5ff48b0ac330c2a305650141ada2255629c1f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200505"
---
# <a name="delete---sql-command"></a>DELETE (SQL-Befehl)
Markiert Datensätze zum Löschen.  
  
 Der Visual FoxPro-ODBC-Treiber unterstützt die native Visual FoxPro-Sprachsyntax für diesen Befehl. Treiber spezifische Informationen finden Sie in den hinweisen.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
DELETE FROM [DatabaseName!]TableName  
   [WHERE FilterCondition1 [AND | OR FilterCondition2 ...]]  
```  
  
## <a name="arguments"></a>Argumente  
 Aus [ *DatabaseName!*] *TableName*  
 Gibt die Tabelle an, in der Datensätze zum Löschen markiert sind.  
  
 *DatabaseName!* Gibt den Namen einer Datenbank an, die die Tabelle enthält, wenn die enthaltende Datenbank nicht die Datenbank ist, die mit der Datenquelle angegeben ist. Sie müssen den Namen einer Datenbank einschließen, die die Tabelle enthält, wenn es sich bei der Datenbank nicht um die Datenbank handelt, die in der Datenquelle angegeben ist. Schließen Sie das Ausrufezeichen (!) nach dem Datenbanknamen und vor dem Tabellennamen ein.  
  
 Where *FilterCondition1*[und &#124; oder *FilterCondition2*...]  
 Gibt an, dass Visual FoxPro nur bestimmte Datensätze für den Löschvorgang kennzeichnet.  
  
 *Filtercondition* gibt die Kriterien an, die Datensätze erfüllen müssen, um zum Löschen markiert zu werden. Sie können beliebig viele Filterbedingungen einschließen und diese mit dem and-Operator oder or-Operator verbinden. Sie können auch den Not-Operator verwenden, um den Wert eines logischen Ausdrucks umzukehren, oder Sie können **empty**() verwenden, um nach einem leeren Feld zu suchen.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn SET DELETED auf ON festgelegt ist, werden für den Löschvorgang markierte Datensätze von allen Befehlen ignoriert, die einen Bereich enthalten.  
  
 DELETE-SQL verwendet Datensatzsperren, wenn mehrere Datensätze zum Löschen in Tabellen markiert werden, die für den freigegebenen Zugriff geöffnet wurden Dadurch werden Daten Satz Konflikte in mehr Benutzer Situationen reduziert, aber die Leistung kann beeinträchtigt werden. Um die maximale Leistung zu erzielen, öffnen Sie die Tabelle für die exklusive Verwendung.  
  
## <a name="driver-remarks"></a>Hinweise zu Treibern  
 Wenn Ihre Anwendung die ODBC-SQL-Anweisung DELETE an die Datenquelle sendet, konvertiert der Visual FoxPro-ODBC-Treiber den Befehl ohne Übersetzung in den DELETE-Befehl von Visual FoxPro.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SET DELETED-Befehl](../../odbc/microsoft/set-deleted-command.md)
