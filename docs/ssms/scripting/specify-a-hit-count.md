---
title: Angeben einer Trefferanzahl
description: In diesem Artikel erfahren Sie, wie Sie eine Trefferanzahl für einen Breakpoint festlegen, sodass der Debugger erst am Breakpoint anhält, wenn die Trefferanzahl erreicht ist.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.debug.breakpt.hitcount
helpviewer_keywords:
- Transact-SQL debugger, breakpoint hit count
ms.assetid: 24836939-94ed-4e57-aa85-5d6938d859e4
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7856ce9244f1a8767a4ce46b889fd39ca6a720f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353911"
---
# <a name="specify-a-hit-count"></a>Angeben einer Trefferanzahl

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Die Breakpoint-Trefferanzahl bildet einen Leistungsindikator, der bei jedem Erreichen des Breakpoints vom [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debugger inkrementiert wird. Wenn die angegebene Trefferanzahl erreicht ist und alle angegebenen Breakpointbedingungen erfüllt sind, führt der Debugger die für den Breakpoint angegebene Aktion aus.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="hit-count-considerations"></a>Überlegungen zur Trefferanzahl

 Standardmäßig wird die Ausführung stets unterbrochen, wenn ein Breakpoint erreicht wird. Folgende Optionen sind verfügbar:  
  
-   Immer anhalten (Standard).  
  
-   Anhalten, wenn die Trefferanzahl gleich einem angegebenen Wert ist.  
  
-   Anhalten, wenn die Trefferanzahl ein Vielfaches ist von einem angegebenen Wert.  
  
-   Anhalten, wenn die Trefferanzahl größer oder gleich einem angegebenen Wert ist.  
  
 Die Breakpoint-Trefferanzahlen werden innerhalb einer Debugsitzung inkrementiert. Zu Beginn jeder Debugsitzung werden alle Trefferanzahlen auf 0 festgelegt.  
  
 Wenn Sie die Häufigkeit nachverfolgen möchten, mit der ein Breakpoint erreicht wurde, ohne die Ausführung zu unterbrechen, geben Sie eine Trefferanzahl mit sehr hohen Wert an, damit beim Breakpoint nie eine Unterbrechung eintritt.  
  
 Die Standardaktion für einen Breakpoint besteht darin, die Ausführung zu unterbrechen, wenn die Trefferanzahl- und die Breakpointbedingung erfüllt sind. Informationen zum Angeben anderer Aktionen finden Sie unter [Angeben einer Breakpointaktion](./specify-a-breakpoint-action.md).  
  
#### <a name="to-specify-a-hit-count"></a>So geben Sie eine Trefferanzahl an  
  
1.  Klicken Sie im Editor-Fenster mit der rechten Maustaste auf das Breakpointsymbol, und klicken Sie dann im Kontextmenü auf **Trefferanzahl** .  
  
     Oder  
  
     Klicken Sie im Fenster **Breakpoints** mit der rechten Maustaste auf das Breakpointsymbol, und klicken Sie dann im Kontextmenü auf **Trefferanzahl** .  
  
2.  Wählen Sie im Dialogfeld **Trefferanzahl für Haltepunkt** im Feld **Wenn der Haltepunkt erreicht wird** das gewünschte Verhalten aus.  
  
     Wenn Sie eine andere Einstellung als **Immer anhalten** auswählen, wird rechts neben der Liste ein Textfeld angezeigt. Geben Sie in diesem Textfeld die gewünschte Trefferanzahl als ganze Zahl ein.  
  
3.  Klicken Sie auf **OK** , um die Änderungen zu implementieren, oder auf **Abbrechen** , um den Vorgang zu beenden, ohne die Änderungen zu übernehmen.  
  
#### <a name="to-view-or-reset-the-current-hit-count"></a>So zeigen Sie die aktuelle Trefferanzahl an oder setzen diese zurück  
  
1.  Klicken Sie im Editor-Fenster mit der rechten Maustaste auf das Breakpointsymbol, und klicken Sie dann im Kontextmenü auf **Trefferanzahl** .  
  
     Oder  
  
     Klicken Sie im Fenster **Breakpoints** mit der rechten Maustaste auf das Breakpointsymbol, und klicken Sie dann im Kontextmenü auf **Trefferanzahl** .  
  
2.  Im Dialogfeld **Trefferanzahl für Haltepunkt** wird direkt über der Schaltfläche **Zurücksetzen** der Wert **Aktuelle Trefferanzahl** angezeigt.  
  
3.  Klicken Sie auf **Zurücksetzen** , wenn Sie die aktuelle Trefferanzahl auf 0 festlegen möchten.  
  
4.  Klicken Sie auf **OK** oder **Abbrechen** , um das Dialogfeld zu schließen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Angeben einer Breakpointbedingung](./specify-a-breakpoint-condition.md)  
  
