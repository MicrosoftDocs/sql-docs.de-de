---
title: Vorlagen und Berechtigungen in SQL Server Profiler
titleSuffix: SQL Server Profiler
description: In diesem Artikel erfahren Sie, wie der SQL Server Profiler funktioniert, wie er verwendet werden kann, um eine Ablaufverfolgung für Ereignisse durchzuführen und wo Sie weitere Informationen zu den dazugehörigen Features erhalten.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 6d00378a-5d74-463b-9ed6-a2685306a9d2
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 679b8c0b120b3cea55a2cf8621e5619bc6c282bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342002"
---
# <a name="sql-server-profiler-templates-and-permissions"></a>Vorlagen und Berechtigungen in SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] zeigt an, wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Abfragen intern auflöst. Auf diese Weise können Administratoren genau erfassen, welche [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen oder MDX-Ausdrücke (Multidimensional Expressions) an den Server übermittelt werden und wie der Server auf die Datenbank oder den Cube zugreift, um Resultsets zurückzugeben.  
  
 Mit [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]können Sie folgende Aktionen ausführen:  
  
-   Erstellen einer Ablaufverfolgung, die auf einer wiederverwendbaren Vorlage basiert.  
  
-   Beobachten der Ablaufverfolgungsergebnisse, während die Ablaufverfolgung ausgeführt wird.  
  
-   Speichern der Ablaufverfolgungsergebnisse in einer Tabelle.  
  
-   Starten, Beenden, Anhalten und Ändern der Ablaufverfolgungsergebnisse nach Bedarf.  
  
-   Wiedergeben der Ablaufverfolgungsergebnisse.  
  
 Überwachen Sie mit [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ausschließlich die für Sie interessanten Ereignisse. Wenn Ablaufverfolgungen zu umfangreich werden, können Sie diese anhand der gewünschten Informationen filtern, damit nur eine Teilmenge der Ereignisdaten gesammelt wird. Wenn zu viele Ereignisse überwacht werden, nimmt der Verwaltungsaufwand für den Server und den Überwachungsvorgang zu, und die Ablaufverfolgungsdatei oder -tabelle kann sehr groß werden, vor allem, wenn über längere Zeit überwacht wird.  
  
> [!NOTE]  
>  Nachverfolgungsspaltenwerte über 1 GB geben einen Fehler zurück und werden in der Nachverfolgungsausgabe abgeschnitten.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
|Thema|BESCHREIBUNG|  
|-----------|-----------------|  
|[SQL Server Profiler-Vorlagen](../../tools/sql-server-profiler/sql-server-profiler-templates.md)|Enthält Informationen zu den vordefinierten Ablaufverfolgungsvorlagen, die im Lieferumfang von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]enthalten sind.|  
|[Erforderliche Berechtigungen zum Ausführen von SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)|Enthält Informationen zu den Berechtigungen, die zum Ausführen von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]erforderlich sind.|  
|[Speichern von Ablaufverfolgungen und Ablaufverfolgungsvorlagen](../../tools/sql-server-profiler/save-traces-and-trace-templates.md)|Enthält Informationen zum Speichern von Ablaufverfolgungsausgaben und zum Speichern von Ablaufverfolgungsdefinitionen in einer Vorlage.|  
|[Ändern von Ablaufverfolgungsvorlagen](../../tools/sql-server-profiler/modify-trace-templates.md)|Enthält Informationen zum Ändern von Ablaufverfolgungsvorlagen mithilfe von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] oder mithilfe von [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|[Starten einer Ablaufverfolgung](../../tools/sql-server-profiler/start-a-trace.md)|Enthält Informationen zu den Vorgängen beim Starten, Anhalten oder Beenden einer Ablaufverfolgung.|  
|[Korrelieren einer Ablaufverfolgung mit Windows-Leistungsprotokolldaten](../../tools/sql-server-profiler/correlate-a-trace-with-windows-performance-log-data.md)|Enthält Informationen zur Korrelation von Windows-Leistungsprotokolldaten mit einer Ablaufverfolgung mithilfe von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler.|  
|[Anzeigen und Analysieren von Ablaufverfolgungen mit SQL Server Profiler](../../tools/sql-server-profiler/view-and-analyze-traces-with-sql-server-profiler.md)|Enthält Informationen zum Verwenden von Ablaufverfolgungen zur Problembehandlung bei Daten, zum Anzeigen von Objektnamen in einer Ablaufverfolgung und zum Finden von Ereignissen in einer Ablaufverfolgung.|  
|[Analysieren von Deadlocks mit SQL Server Profiler](../../tools/sql-server-profiler/analyze-deadlocks-with-sql-server-profiler.md)|Enthält Informationen zum Verwenden von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] zum Identifizieren der Ursache eines Deadlocks.|  
|[Analysieren von Abfragen mit SHOWPLAN-Ergebnissen in SQL Server Profiler](../../tools/sql-server-profiler/analyze-queries-with-showplan-results-in-sql-server-profiler.md)|Enthält Informationen zum Verwenden von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] zum Sammeln und Anzeigen von Ergebnissen für Showplan und Showplanstatistiken.|  
|[Filtern von Ablaufverfolgungen mit SQL Server Profiler](../../tools/sql-server-profiler/filter-traces-with-sql-server-profiler.md)|Enthält Informationen zum Festlegen von Filtern für Datenspalten zum Filtern von Ablaufverfolgungsausgaben mithilfe von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|  
|[Wiedergeben von Ablaufverfolgungen](../../tools/sql-server-profiler/replay-traces.md)|Erklärt, was das Wiedergeben einer Ablaufverfolgung bedeutet und was erforderlich ist, um eine Ablaufverfolgung wiederzugeben.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
 [Starten des SQL Server Profilers](../../tools/sql-server-profiler/start-sql-server-profiler.md)  
  
  
