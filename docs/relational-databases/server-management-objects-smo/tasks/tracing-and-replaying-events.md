---
description: Verfolgen und Wiedergeben von Ereignissen
title: Ablauf Verfolgung und Wiedergabe von Ereignissen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- replaying events
- traces [SMO]
- events [SMO], replaying
- events [SMO], tracing
ms.assetid: f41b3f85-2f6c-4c3e-9776-8c73d2cc7a53
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8280f8891d4ccd57b4496125a62bd96ba03eb49f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475431"
---
# <a name="tracing-and-replaying-events"></a>Verfolgen und Wiedergeben von Ereignissen
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  In SMO bieten die **Trace** -und **Replay** -Objekte im- <xref:Microsoft.SqlServer.Management.Trace> Namespace programmgesteuerten Zugriff auf die- [!INCLUDE[ssSqlProfiler](../../../includes/sssqlprofiler-md.md)] Funktionalität, die zum Überwachen einer Instanz von oder verwendet wird [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] . Daten über die einzelnen Ereignisse können aufgezeichnet und in einer Datei oder Tabelle zur späteren Analyse gespeichert werden. Beispielsweise können Sie eine Produktionsumgebung überwachen und feststellen, welche Prozeduren langsam ablaufen und dadurch die Leistung beeinträchtigen.  
  
 Die **Ablaufverfolgungs** -und **Wiedergabe** Objekte stellen eine Reihe von-Objekten bereit, die zum Erstellen von Ablauf Verfolgungen für eine Instanz von verwendet werden können [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Diese Objekte können in Ihren eigenen Anwendungen verwendet werden, um manuell Ablaufverfolgungen für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] oder [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] zu erstellen. Außerdem können SMO-Ablauf **Verfolgungs** Objekte zum Lesen von SQL-Ablauf Verfolgungs Dateien und-Tabellen verwendet werden, die durch die Überwachung [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] oder die DTS-Protokollierung erstellt wurden.  
  
 Mit SMO-Ablauf **Verfolgungs** Objekten können Sie die folgenden Funktionen ausführen:  
  
-   Erstellen einer Ablaufverfolgung.  
  
-   Festlegen von Filtern für die Ablaufverfolgung.  
  
-   Festlegen der zu verfolgenden Ereignisse.  
  
-   Stoppen und Starten einer Ablaufverfolgung.  
  
-   Lesen von Ablaufverfolgungsdateien und Ablaufverfolgungstabellen.  
  
-   Abrufen von Informationen zu Ereignissen in einer Ablaufverfolgung.  
  
-   Abrufen von Informationen zu Filtern in einer Ablaufverfolgung.  
  
-   Programmgesteuertes Bearbeiten von Ablaufverfolgungsdaten.  
  
-   Schreiben von Ablaufverfolgungstabellen und Ablaufverfolgungsdateien.  
  
-   Wiedergeben von Ablaufverfolgungsdateien oder Ablaufverfolgungstabellen.  
  
 Die Ablauf Verfolgungs Daten aus den **Trace** -und **Replay** -Objekten können von der SMO-Anwendung verwendet werden, oder Sie können mithilfe [SQL Server Profiler](../../../tools/sql-server-profiler/sql-server-profiler.md)manuell untersucht werden. Die Ablauf Verfolgungs Daten sind auch mit den gespeicherten Prozeduren der [SQL](../../../relational-databases/sql-trace/sql-trace.md) -Ablauf Verfolgung kompatibel, die ebenfalls Ablauf Verfolgungs Funktionen bieten  
  
 Die SMO-Ablaufverfolgungsobjekte befinden sich im <xref:Microsoft.SqlServer.Management.Trace>-Namespace, für den ein Verweis auf die Datei Microsoft.SQLServer.ConnectionInfo.dll erforderlich ist.  
  
 Die **Trace** -und **Replay** -Objekte erfordern ein [Server Connection](/previous-versions/sql/sql-server-2014/ms218641(v=sql.120)) - <xref:Microsoft.SqlServer.Management.Smo.Server.%23ctor%2A> Objekt, um eine Verbindung mit der Instanz von herzustellen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Das [ServerConnection](/previous-versions/sql/sql-server-2014/ms218641(v=sql.120)) -Objekt befindet sich im [Microsoft. SqlServer. Management. Common](/previous-versions/sql/sql-server-2014/ms212673(v=sql.120)) -Namespace, der einen Verweis auf die Microsoft.SQLServer.ConnectionInfo.dll Datei erfordert.  
  
> [!NOTE]  
>  Die **Ablaufverfolgungs** -und **Wiedergabe** Objekte werden auf einer 64-Bit-Plattform nicht unterstützt.  
  
