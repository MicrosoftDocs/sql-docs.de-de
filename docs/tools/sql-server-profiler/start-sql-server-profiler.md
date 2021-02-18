---
title: Ausführen von SQL Server Profiler
titleSuffix: SQL Server Profiler
description: Erfahren Sie, welche Programme und Menüs Sie vom SQL Server Profiler aus starten können und welche Verbindungskontexte, Vorlagen und Filter mit der Ablaufverfolgungsausgabe verwendet werden.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 22e57ffa-63b0-4de3-b92e-df297dda1226
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 07/07/2017
ms.openlocfilehash: 9b4bf16638b361059bf82cb090a1705bb8cf103b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354445"
---
# <a name="run-sql-server-profiler"></a>Ausführen von SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Sie können [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] in einer Reihe von Szenarios auf verschiedene Arten ausführen, um das Erfassen der Ablaufverfolgungsausgabe zu unterstützen. Sie können [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] auf unterschiedliche Arten starten: über das **Startmenü** von Windows 10, über das Menü **Extras** im [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Optimierungsratgeber und über verschiedene Speicherorte in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
Wenn Sie [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] zum ersten Mal starten und im Menü **Datei** die Option **Neue Ablaufverfolgung** auswählen, zeigt die Anwendung das Dialogfeld **Verbindung mit Server** herstellen an, in dem Sie eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angeben können, mit der eine Verbindung hergestellt wird.  
## <a name="to-start-sql-server-profiler-from-the-windows-10-start-menu"></a>So starten Sie SQL Server Profiler über das Startmenü von Windows 10  
-  Klicken Sie auf das Windows-Symbol **Start**, oder drücken Sie die Windows-Taste, und geben Sie „SQL Server Profiler 17“ ein. Klicken Sie auf die Kachel **SQL Server Profiler 17**, wenn diese angezeigt wird.   

## <a name="to-start-sql-server-profiler-in-database-engine-tuning-advisor"></a>So starten Sie SQL Server Profiler im Datenbankoptimierungsratgeber  
-  Klicken Sie im [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Optimierungsratgeber im Menü **Extras** auf **SQL Server Profiler**.  

## <a name="to-start-sql-server-profiler-in-sql-server-management-studio"></a>Starten eines SQL Server Profilers in SQL Server Management Studio  
 Sie können [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] von mehreren Speicherorten in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] starten. Wenn [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] gestartet wird, lädt er den Verbindungskontext, die Ablaufverfolgungsvorlage und den Filterkontext seines Startpunkts. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] startet jede SQL Server Profiler-Sitzung in einer eigenen Instanz, und Profiler wird weiterhin ausgeführt, wenn Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] beenden.  
### <a name="to-start-sql-server-profiler-from-the-tools-menu"></a>So starten Sie SQL Server Profiler über das Menü "Extras"  
-  Klicken Sie im [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]-Menü **Extras** auf **SQL Server Profiler**.  

### <a name="to-start-sql-server-profiler-from-the-query-editor"></a>So starten Sie SQL Server Profiler über den Abfrage-Editor  
- Klicken Sie im Abfrage-Editor mit der rechten Maustaste, und wählen Sie dann **Ablaufverfolgungsabfrage in SQL Server Profiler** aus.  

  > [!NOTE]  
  >  Der Verbindungskontext ist die Editor-Verbindung, die Ablaufverfolgungsvorlage lautet TSQL_SPs, und der angewendete Filter ist SPID = Abfragefenster-SPID.  
    
### <a name="to-start-sql-server-profiler-from-activity-monitor"></a>So starten Sie SQL Server Profiler über den Aktivitätsmonitor  
- Klicken Sie im Aktivitätsmonitor auf den Bereich **Prozesse**, klicken Sie mit der rechten Maustaste auf den Prozess, für den Sie ein Profil erstellen möchten, und klicken Sie anschließend auf **Ablaufverfolgungsprozess in SQL Server Profiler**.  

    > [!NOTE]  
    >  Wenn ein Prozess ausgewählt ist, ist der Verbindungskontext die Verbindung für den Objekt-Explorer, und zwar so, wie sie war, als der Aktivitätsmonitor geöffnet wurde. Die Ablaufverfolgungsvorlage ist der auf dem Servertyp basierende Standard, und die SPID entspricht der SPID für den ausgewählten Prozess.  
    
## <a name="net-framework-security"></a>.NET Framework-Sicherheit  
- Im Windows-Authentifizierungsmodus muss das Benutzerkonto, das zum Ausführen von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] verwendet wird, die Berechtigung haben, eine Verbindung zur Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]herzustellen.  
- Zum Ausführen der Ablaufverfolgung mit [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]müssen die Benutzer außerdem die ALTER TRACE-Berechtigung haben.  

## <a name="next-steps"></a>Nächste Schritte  
 [SQL Server Profiler (Übersicht)](../../tools/sql-server-profiler/sql-server-profiler.md)   
 [Verwenden von SQL Server Management Studio](../../ssms/sql-server-management-studio-ssms.md)