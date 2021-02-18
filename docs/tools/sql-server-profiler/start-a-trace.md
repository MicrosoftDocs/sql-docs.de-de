---
title: Starten einer Ablaufverfolgung
titleSuffix: SQL Server Profiler
description: Erfahren Sie, wie Sie eine Ablaufverfolgung starten und nach den dazugehörigen Daten suchen, nachdem Sie eine neue Ablaufverfolgung definiert oder eine Vorlage im SQL Server Profiler erstellt haben.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: aeeb38eb-229a-4c8b-ad66-57e9ce45fb6a
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: df7bc3602d03cf6467a022f060db6b5a63f1c747
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341991"
---
# <a name="start-a-trace"></a>Starten einer Ablaufverfolgung

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Nachdem Sie mit [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]eine neue Ablaufverfolgung definiert oder eine Vorlage erstellt haben, können Sie die Aufzeichnung von Daten mithilfe der neuen Ablaufverfolgungsdefinition oder Vorlage starten, anhalten oder beenden.  
  
## <a name="starting-a-trace"></a>Starten einer Ablaufverfolgung

Wenn Sie eine Ablaufverfolgung starten und die definierte Quelle eine Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] oder [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]ist, erstellt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine Warteschlange als temporären Speicherort für aufgezeichnete Serverereignisse.  
  
Wenn Sie mit [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] auf die SQL-Ablaufverfolgung zugreifen, wird durch das Starten einer Ablaufverfolgung ein neues Ablaufverfolgungsfenster geöffnet (sofern noch keines geöffnet ist), und die Daten werden sofort aufgezeichnet.  
  
Wenn Sie gespeicherte Systemprozeduren von [!INCLUDE[tsql](../../includes/tsql-md.md)] für den Zugriff auf die SQL-Ablaufverfolgung verwenden, müssen Sie bei jedem Start einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine Ablaufverfolgung starten, damit die Daten aufgezeichnet werden. Nachdem eine Ablaufverfolgung gestartet wurde, können Sie nur den Namen der Ablaufverfolgung ändern.  
  
> [!NOTE]  
>  Bei vorhandenen Ablaufverfolgungen können Sie die Eigenschaften zwar anzeigen, aber nicht ändern. Um die Eigenschaften zu ändern, müssen Sie die Ablaufverfolgung beenden oder anhalten.  
  
## <a name="see-also"></a>Weitere Informationen

[Automatisches Starten einer Ablaufverfolgung nach dem Herstellen einer Verbindung mit einem Server &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/start-a-trace-automatically-after-connecting-to-a-server-sql-server-profiler.md)   

[Anhalten einer Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/pause-a-trace-sql-server-profiler.md)   

[Beenden einer Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/stop-a-trace-sql-server-profiler.md)   

[Löschen des Inhalts eines Ablaufverfolgungsfensters &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/clear-a-trace-window-sql-server-profiler.md)   

[Ausführen einer Ablaufverfolgung, nachdem sie angehalten oder beendet wurde &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/run-a-trace-after-it-has-been-paused-or-stopped-sql-server-profiler.md)