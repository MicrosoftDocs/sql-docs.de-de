---
title: Vorlagen
titleSuffix: SQl Server Profiler
description: Hier erfahren Sie mehr über die vordefinierten Vorlagen, die der SQL Server Profiler bereitstellt, und deren Verwendung. Sehen Sie sich an, wie Sie benutzerdefinierte Vorlagen erstellen und die Standardvorlage ändern.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: b674e491-dc58-47a1-acdd-7028e9a201fc
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: ef486049c3a50d2e0b9ba216adcfbcc3881b31b1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340548"
---
# <a name="sql-server-profiler-templates"></a>SQL Server Profiler-Vorlagen

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Mit [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] können Sie Vorlagen erstellen, die die Ereignisklassen und Datenspalten definieren, die in Ablaufverfolgungen eingeschlossen werden sollen. Nach dem Definieren und Speichern der Vorlage können Sie eine Ablaufverfolgung ausführen, die die Daten für jede ausgewählte Ereignisklasse aufzeichnet. Eine Vorlage kann für viele Ablaufverfolgungen verwendet werden. Die Vorlage selbst wird nicht ausgeführt.  

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] bietet vordefinierte Ablaufverfolgungsvorlagen, mit denen Sie auf einfache Weise die Ereignisklassen konfigurieren können, die Sie höchstwahrscheinlich für spezifische Ablaufverfolgungen benötigen. Beispielsweise können Sie mithilfe der Standardvorlage eine allgemeine Ablaufverfolgung zum Aufzeichnen von Anmeldungen, Abmeldungen, abgeschlossenen Batches und Verbindungsinformationen erstellen. Diese Vorlage kann unverändert zum Ausführen von Ablaufverfolgungen verwendet werden. Sie kann aber auch als Ausgangspunkt für zusätzliche Vorlagen mit unterschiedlichen Ereigniskonfigurationen dienen.  
  
> [!NOTE]  
>  Neben Ablaufverfolgungen mithilfe vordefinierter Vorlagen können Sie mit [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] Ablaufverfolgungen auch mithilfe einer leeren Vorlage erstellen, die standardmäßig keine Ereignisklassen enthält. Das Verwenden der leeren Ablaufverfolgungsvorlage kann hilfreich sein, wenn eine geplante Ablaufverfolgung keinen Konfigurationen von vordefinierten Vorlagen ähnelt.  
  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] kann für eine Reihe von Servertypen eine Ablaufverfolgung ausführen. Beispielsweise können Sie [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]nachverfolgen.  Die zulässigen Ereignisklassen sind jedoch für die verschiedenen Servertypen nicht identisch. Deshalb verwaltet [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] für die verschiedenen Server unterschiedliche Vorlagen und stellt die betreffende Vorlage zur Verfügung, die dem ausgewählten Servertyp entspricht.  
  
## <a name="predefined-templates"></a>Vordefinierte Vorlagen  
 Neben der Standardvorlage (Standardeinstellung) enthält [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] mehrere vordefinierte Vorlagen zum Überwachen bestimmter Ereignistypen. In der folgenden Tabelle sind die vordefinierten Vorlagen, deren Verwendungszweck und die Ereignisklassen, für die sie Informationen aufzeichnen, aufgeführt.  
  
|Vorlagenname|Vorlagenverwendungszweck|Ereignisklassen|  
|-------------------|----------------------|-------------------|  
|SP_Counts|Zeichnet das Ausführungsverhalten einer gespeicherten Prozedur über einen Zeitraum auf.|**SP:Starting**|  
|Standard|Allgemeiner Ausgangspunkt zum Erstellen einer Ablaufverfolgung. Zeichnet alle gespeicherten Prozeduren und [!INCLUDE[tsql](../../includes/tsql-md.md)] -Batches, die ausgeführt werden, auf. Mit dieser Vorlage überwachen Sie die allgemeine Aktivität des Datenbankservers.|**Audit Login**<br /><br /> **Audit Logout**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Completed**<br /><br /> **SQL:BatchCompleted**<br /><br /> **SQL:BatchStarting**|  
|TSQL|Zeichnet alle [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen, die von Clients an [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] übermittelt werden, sowie den Ausstellungszeitpunkt auf. Mit dieser Vorlage debuggen Sie Clientanwendungen.|**Audit Login**<br /><br /> **Audit Logout**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Starting**<br /><br /> **SQL:BatchStarting**|  
|TSQL_Duration|Zeichnet alle [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen, die von Clients an [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] übermittelt werden, und deren Ausführungszeit (in Millisekunden) auf und gruppiert sie nach der Dauer. Mit dieser Vorlage identifizieren Sie langsame Abfragen.|**RPC:Completed**<br /><br /> **SQL:BatchCompleted**|  
|TSQL_Grouped|Zeichnet alle [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen, die an [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] übermittelt werden, sowie den Ausstellungszeitpunkt auf. Die Informationen werden nach dem Benutzer oder Client, der die Anweisung übermittelt hat, gruppiert. Mit dieser Vorlage analysieren Sie Abfragen von einem bestimmten Client oder Benutzer.|**Audit Login**<br /><br /> **Audit Logout**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Starting**<br /><br /> **SQL:BatchStarting**|  
|TSQL_Locks|Zeichnet alle [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen, die von Clients an [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] übermittelt werden, sowie die Ausnahmesperrereignisse auf. Verwenden Sie dies, um Probleme mit Deadlocks, Sperrtimeouts und Sperrausweitungsereignissen zu beheben.|**Blocked Process Report**<br /><br /> **SP:StmtCompleted**<br /><br /> **SP:StmtStarting**<br /><br /> **SQL:StmtCompleted**<br /><br /> **SQL:StmtStarting**<br /><br /> **Deadlock Graph**<br /><br /> **Lock:Cancel**<br /><br /> **Lock:Deadlock**<br /><br /> **Lock:Deadlock Chain**<br /><br /> **Lock:Escalation**<br /><br /> **Sperre:Timeout (Timeout>0)**|  
|TSQL_Replay|Zeichnet detaillierte Informationen zu [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen auf, die erforderlich sind, falls die Ablaufverfolgung wiedergegeben wird. Verwenden Sie diese Vorlage für iterative Optimierungen, wie z. B. Vergleichstest.|**CursorClose**<br /><br /> **CursorExecute**<br /><br /> **CursorOpen**<br /><br /> **CursorPrepare**<br /><br /> **CursorUnprepare**<br /><br /> **Audit Login**<br /><br /> **Audit Logout**<br /><br /> **Existing Connection**<br /><br /> **RPC Output Parameter**<br /><br /> **RPC:Completed**<br /><br /> **RPC:Starting**<br /><br /> **Exec Prepared SQL**<br /><br /> **Prepare SQL**<br /><br /> **SQL:BatchCompleted**<br /><br /> **SQL:BatchStarting**|  
|TSQL_SPs|Zeichnet detaillierte Informationen zu allen ausgeführten gespeicherten Prozeduren auf. Verwenden Sie diese Vorlage zum Analysieren der Komponentenschritte von gespeicherten Prozeduren. Fügen Sie das **SP:Recompile** -Ereignis hinzu, falls Sie vermuten, dass Prozeduren neu kompiliert werden.|**Audit Login**<br /><br /> **Audit Logout**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Starting**<br /><br /> **SP:Completed**<br /><br /> **SP:Starting**<br /><br /> **SP:StmtStarting**<br /><br /> **SQL:BatchStarting**|  
|Optimierung|Zeichnet Informationen zur Ausführung von gespeicherten Prozeduren und von [!INCLUDE[tsql](../../includes/tsql-md.md)] -Batches auf. Verwenden Sie diese Vorlage zum Erstellen von Ablaufverfolgungsausgaben, die der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Optimierungsratgeber als Arbeitsauslastung verwenden kann, um Datenbanken zu optimieren.|**RPC:Completed**<br /><br /> **SP:StmtCompleted**<br /><br /> **SQL:BatchCompleted**|  
  
 Informationen zu den Ereignisklassen finden Sie unter [SQL Server Event Class Reference](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
## <a name="default-template"></a>Standardvorlage  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] bestimmt automatisch die Vorlage **Standard** , die standardmäßig für neue Ablaufverfolgungen verwendet wird. Sie können jedoch statt der Standardvorlage eine beliebige andere vordefinierte oder benutzerdefinierte Vorlage verwenden. Um die Standardvorlage zu ändern, aktivieren Sie im Dialogfeld **Eigenschaften der Ablaufverfolgungsvorlage** auf der Registerkarte **Allgemein** das Kontrollkästchen **Als Standardvorlage für den ausgewählten Servertyp verwenden** , wenn Sie eine Vorlage erstellen oder bearbeiten.  
  
 Klicken Sie im [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] im Menü **Datei** auf **Vorlagen** und dann auf **Neue Vorlage** oder **Vorlage bearbeiten**, um zum Dialogfeld **Eigenschaften der Ablaufverfolgungsvorlage** zu navigieren.  
  
> [!NOTE]  
>  Die Standardvorlage gilt speziell für einen bestimmten Servertyp. Wenn Sie die Standardvorlage für einen Servertyp ändern, sind andere Servertypen davon nicht betroffen. Weitere Informationen zum Festlegen einer Standardvorlage für einen bestimmten Server finden Sie unter [Festlegen der Standardeinstellungen für Ablaufverfolgungsdefinitionen &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-trace-definition-defaults-sql-server-profiler.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen einer Ablaufverfolgungsvorlage &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-template-sql-server-profiler.md)   
 [Modify a Trace Template &#40;SQL Server Profiler&#41; (Ändern einer Ablaufverfolgungsvorlage &#40;SQL Server Profiler&#41)](./modify-trace-templates.md)   
 [Exportieren einer Ablaufverfolgungsvorlage &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/export-a-trace-template-sql-server-profiler.md)   
 [Importieren einer Ablaufverfolgungsvorlage &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/import-a-trace-template-sql-server-profiler.md)  
  
