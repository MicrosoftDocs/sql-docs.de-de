---
title: Schrittweises Durchlaufen von Transact-SQL-Code
description: Hier erfahren Sie, wie Sie den Transact-SQL-Debugger verwenden, um zu steuern, welche Transact-SQL-Anweisungen in einem Abfrage-Editor-Fenster der Datenbank-Engine ausgeführt werden.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, debugging code
- Transact-SQL debugger, step over
- Transact-SQL debugger, step out
- Transact-SQL debugger, step into
ms.assetid: e09079b8-c4c9-42b4-821b-4ce81a98a086
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7a48a07ddec4cb6c1f0670a5e6b137690b6f2185
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340683"
---
# <a name="step-through-transact-sql-code"></a>Schrittweises Durchlaufen von Transact-SQL-Code

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Der [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debugger ermöglicht es Ihnen, zu bestimmen, welche [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen in einem [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor-Fenster ausgeführt werden. Sie können den Debugger bei einzelnen Anweisungen unterbrechen und dann den Status der Codeelemente an diesem Punkt anzeigen.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="breakpoints"></a>Breakpoints

Ein Breakpoint signalisiert dem Debugger, die Ausführung bei einer bestimmten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung anzuhalten. Weitere Informationen über Breakpoints finden Sie unter [Verwenden von Transact-SQL-Breakpoints](./transact-sql-breakpoints.md).  
  
## <a name="controlling-statement-execution"></a>Steuern der Anweisungsausführung

Im [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debugger können Sie die folgenden Optionen für das Ausführen der aktuellen Anweisung im [!INCLUDE[tsql](../../includes/tsql-md.md)] -Code angeben:

- Den Vorgang bis zum nächsten Breakpoint ausführen

- Einen Einzelschritt in die nächste Anweisung ausführen  

    Wenn die nächste Anweisung eine gespeicherte [!INCLUDE[tsql](../../includes/tsql-md.md)] -Prozedur, eine Funktion oder einen Trigger aufruft, zeigt der Debugger ein neues Abfrage-Editor-Fenster an, das den Code des Moduls enthält. Das Fenster befindet sich im Debuggingmodus, und die Ausführung hält bei der ersten Anweisung im Modul an. Sie können sich dann durch den Modulcode bewegen, indem Sie z. B. Breakpoints festlegen oder den Code schrittweise durchlaufen.

- Einen Einzelschritt in die nächste Anweisung überspringen

    Die nächste Anweisung wird ausgeführt. Wenn die Anweisung jedoch eine gespeicherte Prozedur, eine Funktion oder einen Trigger aufruft, wird der Modulcode bis zum Ende ausgeführt, und die Ergebnisse werden an den aufrufenden Code zurückgegeben. Wenn Sie sicher sind, dass in der gespeicherten Prozedur keine Fehler vorliegen, können Sie sie überspringen. Die Ausführung hält bei der Anweisung an, die dem Aufruf der gespeicherten Prozedur, der Funktion oder dem Trigger folgt.

- Eine gespeicherte Prozedur, eine Funktion oder einen Trigger bis zum Rücksprung ausführen  

    Die Ausführung hält bei der Anweisung an, die dem Aufruf der gespeicherten Prozedur, der Funktion oder dem Trigger folgt.  

- Den Vorgang von der aktuellen Position bis zur aktuellen Position des Zeigers ausführen und alle Breakpoints ignorieren  

 Die folgende Tabelle enthält die verschiedenen Möglichkeiten, mit denen Sie bestimmen können, wie Anweisungen im [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debugger ausgeführt werden.  
  
|Aktion|Aktion ausführen:|  
|------------|---------------------|  
|Ausführen aller Anweisungen von der aktuellen Anweisung bis zum nächsten Breakpoint|Klicken Sie im Menü **Debuggen** auf **Weiter** .<br /><br /> Klicken Sie auf der Symbolleiste **Debuggen** auf die Schaltfläche **Weiter** .|  
|Ausführen eines Einzelschritts in die nächste Anweisung oder in das nächste Modul|Klicken Sie im Menü **Debuggen** auf **Einzelschritt** .<br /><br /> Klicken Sie auf der Symbolleiste **Debuggen** auf die Schaltfläche **Einzelschritt** .<br /><br /> Drücken Sie F11.|  
|Überspringen der nächsten Anweisung oder des nächsten Moduls|Klicken Sie im Menü **Debuggen** auf **Prozedurschritt** .<br /><br /> Klicken Sie auf der Symbolleiste **Debuggen** auf die Schaltfläche **Prozedurschritt** .<br /><br /> Drücken Sie F10.|  
|Ausführen eines Moduls bis Rücksprung|Klicken Sie im Menü **Debuggen** auf **Rücksprung** .<br /><br /> Klicken Sie auf der Symbolleiste **Debuggen** auf die Schaltfläche **Rücksprung** .<br /><br /> Drücken Sie UMSCHALT+F11.|  
|Ausführen bis zur aktuellen Cursorposition|Klicken Sie mit der rechten Maustaste auf das Abfrage-Editor-Fenster, und klicken Sie dann auf **Ausführen bis Cursorposition**.<br /><br /> Drücken Sie STRG+F10.|  
  
## <a name="see-also"></a>Weitere Informationen

- [Transact-SQL-Debuggerinformationen](./transact-sql-debugger-information.md)