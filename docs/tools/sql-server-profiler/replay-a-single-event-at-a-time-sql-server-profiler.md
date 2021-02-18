---
title: Wiedergeben einzelner Ergebnisse
titleSuffix: (SQL Server Profiler
description: Hier entdecken Sie, wie Sie in SQL Server Profiler einzelne Ablaufverfolgungsereignisse wiedergeben, indem Sie eine Datei oder Tabelle für die Ablaufverfolgungswiedergabe durchlaufen.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 220fb192-9636-41a2-b15c-62af6cab8fff
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: b9000da1493b61383f301308d5269fe4349c2e76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353382"
---
# <a name="replay-a-single-event-at-a-time-sql-server-profiler"></a>Wiedergeben von jeweils einem einzelnen Ereignis (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Thema wird beschrieben, wie Sie mithilfe von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]jeweils ein einzelnes Ereignis einer Ablaufverfolgungsdatei oder -tabelle wiedergeben.  
  
### <a name="to-replay-a-single-event-at-a-time"></a>So geben Sie jeweils ein einzelnes Ereignis wieder  
  
1.  Öffnen Sie die Ablaufverfolgungsdatei oder -tabelle, die Sie wiedergeben möchten. Weitere Informationen finden Sie unter [Öffnen einer Ablaufverfolgungsdatei &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md) oder den Optimierungsratgeber von [Öffnen einer Ablaufverfolgungstabelle &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)die richtigen Ereignisse und Spalten aufgezeichnet werden.  
  
     Stellen Sie sicher, dass die geöffnete Ablaufverfolgungsdatei oder -tabelle die Ereignisklassen enthält, die für die Wiedergabe erforderlich sind. Weitere Informationen finden Sie unter [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md).  
  
2.  Klicken Sie im Menü **Wiedergabe** auf **Schritt**, und stellen Sie eine Verbindung mit der Serverinstanz her, auf der die Ablaufverfolgung wiedergegeben werden soll.  
  
3.  Überprüfen Sie im Dialogfeld **Wiedergabekonfiguration** die Einstellungen, und klicken Sie dann auf **OK**. Weitere Informationen zu den Einstellungen im Dialogfeld **Wiedergabekonfiguration** finden Sie unter [Wiedergeben einer Ablaufverfolgungsdatei &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-a-trace-file-sql-server-profiler.md) oder [Wiedergeben einer Ablaufverfolgungstabelle &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-a-trace-table-sql-server-profiler.md).  
  
4.  Klicken Sie im Dialogfeld **Wiedergabekonfiguration** auf **OK** , um das erste Ereignis wiederzugeben.  
  
5.  Um die nachfolgenden Ereignisse wiederzugeben, klicken Sie im Menü **Wiedergabe** auf **Schritt**, oder drücken Sie F10. Klicken Sie für jedes Ereignis entweder auf **Schritt** , oder drücken Sie F10.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Wiedergeben von Ablaufverfolgungen](../../tools/sql-server-profiler/replay-traces.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
