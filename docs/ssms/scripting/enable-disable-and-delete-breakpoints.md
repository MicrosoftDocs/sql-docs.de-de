---
title: Aktivieren, Deaktivieren und Löschen von Breakpoints
description: Hier erfahren Sie, wie Sie das Fenster „Haltepunkte“ zum Anzeigen, Löschen, Deaktivieren und Aktivieren von Breakpoints verwenden.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 357b5874-273f-43a9-8e30-83872bdea5dc
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8fec0b9a118326b0afade7d7bab0a26777d8e523
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354646"
---
# <a name="enable-disable-and-delete-breakpoints"></a>Aktivieren, Deaktivieren und Löschen von Breakpoints

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Zum Anzeigen und Verwalten aller offenen Breakpoints können Sie das Fenster **Breakpoints** verwenden. Mithilfe des Fensters können Sie Informationen zu Breakpoints anzeigen und Aktionen ausführen, z. B. Löschen, Deaktivieren und Aktivieren von Breakpoints.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]
  
## <a name="the-breakpoints-window"></a>Fenster "Breakpoints"  
 Das Fenster **Breakpoints** enthält Informationen, z. B. auf welcher Codezeile sich der Breakpoint befindet. Außerdem können Sie im Fenster **Breakpoints** Breakpoints löschen, deaktivieren und aktivieren. Weitere Informationen zum Fenster **Breakpoints** finden Sie unter [Breakpoints Window](./transact-sql-debugger-breakpoints-window.md).  
  
 Durch das Deaktivieren von Breakpoints wird verhindert, dass der Breakpoint die Ausführung anhält, wobei die Definition jedoch beibehalten wird, falls Sie den Breakpoint später aktivieren möchten. Durch das Löschen von Breakpoints werden sie endgültig gelöscht. Sie müssen einen neuen Breakpoint umschalten, um die Ausführung für die Anweisung anzuhalten.  
  
## <a name="to-open-the-breakpoints-window"></a>So öffnen Sie das Fenster "Breakpoints"  
 **To open the Breakpoints window**  
  
 Zum Öffnen des Fensters **Breakpoints** haben Sie folgende Möglichkeiten:  
  
-   Klicken Sie im Menü **Debuggen** auf **Fenster** und dann auf **Breakpoints**.  
  
-   Klicken Sie auf der Symbolleiste **Debuggen** auf die Schaltfläche **Breakpoints** .  
  
-   Drücken Sie STRG+ALT+B.  
  
## <a name="to-disable-a-single-breakpoint"></a>So deaktivieren Sie einen einzelnen Breakpoint  
 **To disable a single breakpoint**  
  
 Sie können einen einzelnen Breakpoint folgendermaßen deaktivieren:  
  
-   Klicken Sie im Fenster „Abfrage-Editor“ mit der rechten Maustaste auf den Breakpoint, und klicken Sie anschließend auf **Breakpoint deaktivieren**.  
  
-   Deaktivieren Sie im Fenster "Breakpoints" das Kontrollkästchen links neben dem Breakpoint.  
  
## <a name="to-disable-all-breakpoints"></a>So deaktivieren Sie alle Breakpoints  
 **To disable all breakpoints**  
  
 Sie können alle Breakpoints folgendermaßen deaktivieren:  
  
-   Klicken Sie im Menü **Debuggen** auf **Alle Breakpoints deaktivieren**.  
  
-   Klicken Sie auf der Symbolleiste des Fensters **Breakpoints** auf die Schaltfläche **Alle Breakpoints deaktivieren** .  
  
## <a name="to-enable-a-single-breakpoint"></a>So aktivieren Sie einen einzelnen Breakpoint  
 **To enable a single breakpoint**  
  
 Sie können einen einzelnen Breakpoint folgendermaßen aktivieren:  
  
-   Klicken Sie im Fenster „Abfrage-Editor“ mit der rechten Maustaste auf den Breakpoint, und klicken Sie anschließend auf **Breakpoint aktivieren**.  
  
-   Aktivieren Sie im Fenster "Breakpoints" das Kontrollkästchen links neben dem Breakpoint.  
  
## <a name="to-enable-all-breakpoints"></a>So aktivieren Sie alle Breakpoints  
 **To enable all breakpoints**  
  
 Sie können alle Breakpoints folgendermaßen aktivieren:  
  
-   Klicken Sie im Menü **Debuggen** auf **Alle Breakpoints aktivieren**.  
  
-   Klicken Sie auf der Symbolleiste des Fensters **Breakpoints** auf die Schaltfläche **Alle Breakpoints aktivieren** .  
  
## <a name="to-delete-a-single-breakpoint"></a>So löschen Sie einen einzelnen Breakpoint  
 **To delete a single breakpoint**  
  
 Sie können einen einzelnen Breakpoint folgendermaßen löschen:  
  
-   Klicken Sie im Fenster „Abfrage-Editor“ mit der rechten Maustaste auf den Breakpoint, und klicken Sie anschließend auf **Breakpoint löschen**.  
  
-   Klicken Sie im Fenster „Breakpoints“ mit der rechten Maustaste auf den Breakpoint, und klicken Sie anschließend im Kontextmenü auf **Löschen** .  
  
-   Wählen Sie im Fenster "Breakpoints" den Breakpoint aus, und drücken Sie dann ENTF.  
  
## <a name="to-delete-all-breakpoints"></a>So löschen Sie alle Breakpoints  
 **To delete all breakpoints**  
  
 Sie können alle Breakpoints folgendermaßen löschen:  
  
-   Klicken Sie im Menü **Debuggen** auf **Alle Haltepunkte löschen**.  
  
-   Klicken Sie auf der Symbolleiste des Fensters **Breakpoints** auf die Schaltfläche **Alle Breakpoints löschen** .  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ein- und Ausschalten eines Breakpoints](./toggle-a-breakpoint.md)  
  
