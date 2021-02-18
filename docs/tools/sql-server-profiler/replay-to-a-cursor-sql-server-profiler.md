---
title: Wiedergabe bis zu einem Cursor
titleSuffix: SQL Server Profiler
description: In diesem Artikel wird erläutert, wie Sie die Wiedergabe einer Ablaufverfolgung mithilfe eines Cursors im SQL Server Profiler an einer bestimmten Stelle anhalten. Außerdem erfahren Sie, wie Sie das Debuggen vereinfachen, indem Sie die Wiedergabe bis zu einem Cursor konfigurieren.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 89eadc41-4424-4a1c-ba61-0b52c851cdb1
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: a903e7176b4715dcf54db8a55b28a280a825b3bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353351"
---
# <a name="replay-to-a-cursor-sql-server-profiler"></a>Wiedergeben bis zu einer Cursorposition (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Thema wird beschrieben, wie Ablaufverfolgungsdateien oder -tabellen mithilfe von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]so wiedergegeben werden können, dass sie anhalten, wenn sie eine Cursorposition erreichen. Durch das Anhalten von Ablaufverfolgungen an Cursorpositionen wird das Debugging erleichtert, da lange Ablaufverfolgungsskripts in kürzere Segmente unterteilt und inkrementell analysiert werden können.  
  
### <a name="to-replay-to-the-cursor"></a>So geben Sie bis zur Cursorposition wieder  
  
1.  Öffnen Sie die Ablaufverfolgungsdatei oder -tabelle, die Sie wiedergeben möchten. Weitere Informationen finden Sie unter [Öffnen einer Ablaufverfolgungsdatei &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md) oder den Optimierungsratgeber von [Öffnen einer Ablaufverfolgungstabelle &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)die richtigen Ereignisse und Spalten aufgezeichnet werden.  
  
     Stellen Sie sicher, dass die geöffnete Ablaufverfolgungsdatei oder -tabelle die Ereignisklassen enthält, die für die Wiedergabe erforderlich sind. Weitere Informationen finden Sie unter [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md).  
  
2.  Klicken Sie im Ablaufverfolgungsfenster auf ein Ereignis.  
  
3.  Klicken Sie im Menü **Wiedergeben** auf **Ausführen bis Cursorposition**, und stellen Sie eine Verbindung zu dem Server her, auf dem Sie die Ablaufverfolgung wiedergeben möchten.  
  
4.  Überprüfen Sie im Dialogfeld **Wiedergabekonfiguration** die Einstellungen, und klicken Sie dann auf **OK**.  
  
     Die Wiedergabe beginnt; sie hält an, wenn sie die erste Cursorposition erreicht.  
  
5.  Drücken Sie F5, um die Ablaufverfolgung fortzusetzen.  
  
6.  Wiederholen Sie Schritt 5 bis zum Ende der Ablaufverfolgung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Wiedergeben bis zu einem Breakpoint &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-to-a-breakpoint-sql-server-profiler.md)   
 [Wiedergeben von Ablaufverfolgungen](../../tools/sql-server-profiler/replay-traces.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
