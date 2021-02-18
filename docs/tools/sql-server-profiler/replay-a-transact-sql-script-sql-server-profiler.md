---
title: Wiedergeben eines Transact-SQL-Skripts
titleSuffix: SQL Server Profiler
description: Hier erfahren Sie, wie Sie den SQL Server Profiler zum Wiedergeben von Transact-SQL-Skripts verwenden, damit Sie verschiedene mögliche Lösungen für ein Leistungsproblem vergleichen können.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 9c0eb222-e6e3-4bc1-a25f-a41e962d361b
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 20252994f90032293730335dda1f6dd689133497
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353435"
---
# <a name="replay-a-transact-sql-script-sql-server-profiler"></a>Wiedergeben eines Transact-SQL-Skripts (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Verwenden Sie beim Testen von möglichen Lösungen für ein Leistungsproblem [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] , um [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skripts wiederzugeben, und vergleichen Sie die Leistung vor und nach dem Ändern.  
  
### <a name="to-replay-a-transact-sql-script"></a>So geben Sie ein Transact-SQL-Skript wieder  
  
1.  Zeigen Sie im Menü **Datei** auf **Öffnen**, und klicken Sie dann auf **Skriptdatei**.  
  
2.  Wählen Sie die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skriptdatei aus, die Sie öffnen möchten. Stellen Sie sicher, dass das [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skript die zum Wiedergeben erforderlichen Ereignisse enthält. Weitere Informationen finden Sie unter [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md).  
  
3.  Klicken Sie im Menü **Wiedergeben** auf **Start**, und stellen Sie eine Verbindung mit dem Server her, auf dem das Skript wiedergegeben werden soll.  
  
4.  Überprüfen Sie im Dialogfeld **Wiedergabekonfiguration** die Einstellungen, und klicken Sie dann auf **OK**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Wiedergeben von Ablaufverfolgungen](../../tools/sql-server-profiler/replay-traces.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
