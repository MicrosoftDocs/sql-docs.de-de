---
title: Lokal (Fenster)
description: In diesem Artikel erfahren Sie, wie Sie das Fenster „Lokal“ des Transact-SQL-Debuggers verwenden, um Ausdrücke aus dem aktuellen Aufrufstapelrahmen anzuzeigen und zu bearbeiten.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Locals Window [Transact-SQL]
ms.assetid: 59bea640-7823-4b4d-832c-f384d83cca2f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dd6fc308bf65904d943b298546f2006b9f259abe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351609"
---
# <a name="transact-sql-debugger---locals-window"></a>Transact-SQL-Debugger – Fenster „Lokal“

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Im Fenster **Lokal** werden Informationen über die lokalen Ausdrücke im aktuellen Bereich des [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debuggers angezeigt. Der Bereich wird auf den aktuellen Aufruflistenrahmen festgelegt, der im Fenster **Aufrufliste** ausgewählt ist. Sie müssen sich im Debugmodus befinden, um die lokalen Ausdrücke anzuzeigen.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="task-list"></a>Aufgabenliste

**So greifen Sie auf das Fenster Lokal zu**
  
-   Klicken Sie im Menü **Debuggen** auf **Fenster**, und klicken Sie dann auf **Lokal**.  
  
 **So ändern Sie den Wert eines Ausdrucks**  
  
-   Klicken mit der rechten Maustaste auf den Ausdruck, und wählen Sie anschließend **Wert bearbeiten** aus.  
  
## <a name="columns"></a>Spalten  
 **Name**  
 Der Name des lokalen Ausdrucks. Der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Debugger führt Variablen, Parameter und die Systemfunktionen, deren Namen mit @@ beginnen, auf.  
  
 **Wert**  
 Zeigt den Wert an, der dem lokalen Ausdruck derzeit zugewiesen ist. Diese Spalte ist leer, wenn dem Ausdruck kein Wert zugewiesen wurde.  
  
 Wenn die Länge eines Ausdrucks größer als die Breite der Spalte **Wert** ist, wird der vollständige Wert in einer QuickInfo angezeigt, wenn Sie den Mauszeiger über die **Wertzelle** für diesen Ausdruck bewegen.  
  
 Ein Lupensymbol in einer **Wertzelle** zeigt an, dass die [!INCLUDE[tsql](../../includes/tsql-md.md)] Debuggerschnellansicht verfügbar ist. In der Liste können Sie **Text-Schnellansicht**, **XML-Schnellansicht** oder **HTML-Schnellansicht** angeben. Um eine Debuggerschnellansicht zu starten, klicken Sie auf das Lupensymbol. Der [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debugger öffnet ein Dialogfeld, in dem die Daten in einem für den Datentyp geeigneten Format angezeigt werden.  
  
 **Typ**  
 Zeigt den Datentyp des Ausdrucks an.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Transact-SQL-Debugger](./transact-sql-debugger.md)   
 [Transact-SQL-Debuggerinformationen](./transact-sql-debugger-information.md)   
 [Überwachung (Fenster)](./transact-sql-debugger-watch-window.md)   
 [Fenster 'Aufrufliste'](./transact-sql-debugger-call-stack-window.md)   
 [Dialogfeld 'Schnellüberwachung'](./transact-sql-debugger-quickwatch-dialog-box.md)   
 [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)