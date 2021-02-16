---
title: Leistungsdashboard | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über das Leistungsdashboard von SQL Server Management Studio, das schnellere Erkenntnisse über SQL Server und Azure SQL Managed Instance bietet.
ms.custom: ''
ms.date: 11/13/2020
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- performance dashboard [SQL Server]
- performance dashboard reports
- perf dashboard
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: pelopes
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 5d1afb7d96f42ecb33f57353a2abcfc24725e51e
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2021
ms.locfileid: "100347717"
---
# <a name="performance-dashboard"></a>Leistungsdashboard
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] Version 17.2 und höher enthält das Leistungsdashboard. Dieses Dashboard wurde entworfen, um schnelle visuelle Einblicke in den Leistungszustand von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) und verwalteter [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)]-Instanzen bereitzustellen. 

Mit dem Leistungsdashboard können Sie schnell herausfinden, ob [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] einen Leistungsengpass aufweisen. Wenn ein Engpass entdeckt wird, können ganz einfach zusätzliche Diagnosedaten erfasst werden, die für das Beheben des Problems erforderlich sein können. Zu den allgemeinen Leistungsproblemen, bei deren Identifizierung das Leistungsdashboard helfen kann, gehören:
-  CPU-Engpässe (bzw. welche Abfragen die meiste CPU-Leistung beanspruchen)
-  E/A-Engpässe (bzw. welche Abfragen die meisten E/A-Vorgänge ausführen)
-  Vom Abfrageoptimierer erzeugte Indexempfehlungen (fehlende Indizes)
-  Blockierung
-  Ressourcenkonflikte (einschließlich Latchkonflikte)

Das Leistungsdashboard hilft auch beim Identifizieren ressourcenintensiver Abfragen, die möglicherweise zuvor ausgeführt wurden. Zudem stehen verschiedene Metriken zur Verfügung, um hohe Kosten festzustellen: CPU, Logische Schreibvorgänge, Logische Lesevorgänge, Dauer, Physische Lesevorgänge und CLR-Zeit.

Das Leistungsdashboard ist in folgende Abschnitte und Unterberichte unterteilt:
-  System-CPU-Auslastung
-  Aktuell wartende Anforderungen
-  Aktuelle Aktivität
   -  Benutzeranforderungen
   -  Benutzersitzungen
   -  Cachetrefferquote
-  Verlaufsinformationen
   -  Wartevorgänge
   -  Latches
   -  I/O Statistics (E/A-Statistik)
   -  Ressourcenintensive Abfragen
- Verschiedene Informationen
  -  Aktive Ablaufverfolgungen
  -  Aktive XEvent-Sitzungen
  -  Datenbanken
  -  Fehlende Indizes

> [!NOTE] 
> Intern verwendet das Leistungsdashboard mit [Ausführung](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md), [Index](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md) und [E/A](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md) zusammenhängende dynamische Verwaltungssichten (Dynamic Management Views, DMVs) und -funktionen (Dynamic Management Functions, DMFs).

## <a name="to-view-the-performance-dashboard"></a>Anzeigen des Leistungsdashboards 
  
Klicken Sie zum Anzeigen des Leistungsdashboards mit der rechten Maustaste auf den Namen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz im Objekt-Explorer, wählen Sie **Berichte** und dann **Standardberichte** aus, und klicken Sie auf **Leistungsdashboard**.  
  
![Leistungsdashboard im Menü](../../relational-databases/performance/media/perf_dashboard_ssms.png "Leistungsdashboard im Menü")  
  
Das Leistungsdashboard wird als neue Registerkarte angezeigt. Im folgenden Beispiel ist ein CPU-Engpass deutlich erkennbar:  
  
![Leistungsdashboard-Hauptbildschirm](../../relational-databases/performance/media/perf_dashboard.png "Leistungsdashboard-Hauptbildschirm")  
  
## <a name="remarks"></a>Bemerkungen
Der Bericht **Fehlende Indizes** zeigt möglicherweise fehlende Indizes an, die der Abfrageoptimierer während der Abfragekompilierung identifiziert hat. Sie müssen jedoch nicht allen Empfehlungen die gleiche Beachtung schenken. Microsoft empfiehlt, die Indizes mit einem höheren Ergebnis als 100.000 für die Erstellung auszuwerten, da bei diesen die meisten Verbesserungen für Benutzerabfragen zu erwarten sind. 

> [!TIP]
> Überprüfen Sie immer, ob ein neuer Indexvorschlag mit einem in derselben Tabelle vorhandenen Index vergleichbar ist. Dann können dieselben praktischen Ergebnisse einfach durch Ändern eines bestehenden Indexes anstatt durch Erstellen eines neuen erzielt werden. Wenn zum Beispiel für die Spalten C1, C2 und C3 ein neuer Index vorgeschlagen wird, sollten Sie zuerst überprüfen, ob ein Index für die Spalten C1 und C2 vorhanden ist. Wenn dies der Fall ist, kann es besser sein, einfach Spalte C3 zum vorhandenen Index hinzuzufügen (unter Beibehaltung der Reihenfolge bereits vorhandener Spalten), um keinen neuen Index erstellen zu müssen.
> Weitere Informationen finden Sie unter [Leitfaden zur Architektur und zum Design von SQL Server-Indizes](../../relational-databases/sql-server-index-design-guide.md).

Der Bericht **Wartevorgänge** filtert alle Wartezeiten im Leerlauf und Ruhezustand heraus. Weitere Informationen zu Wartevorgängen finden Sie unter [sys.dm_os_wait_stats &#40;Transact-SQL&#41; (sys.dm_os_wait_stats &#40;Transact-SQL&#41;)](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) und [SQL Server 2005 Performance Tuning Using Waits and Queues (Leistungsoptimierung von SQL Server 2005 mithilfe von Wartevorgängen und Warteschlangen)](https://download.microsoft.com/download/4/7/a/47a548b9-249e-484c-abd7-29f31282b04d/performance_tuning_waits_queues.doc).

Die Berichte **Ressourcenintensive Abfragen** werden zurückgesetzt, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] neu gestartet wird, da die Daten in den zugrunde liegenden DMVs gelöscht werden. Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] gibt es ausführliche Informationen zu ressourcenintensiven Abfragen im Abfragespeicher. 


> [!NOTE]
> Das Leistungsdashboard wurde für [SQL Server 2005](https://techcommunity.microsoft.com/t5/SQL-Server-Support/SQL-Server-2005-Performance-Dashboard-Reports/ba-p/315415) erstmals als eigenständiger Download freigegeben und später für [SQL Server 2012](https://www.microsoft.com/download/details.aspx?id=29063) aktualisiert. Obwohl der SQL Server Management Studio-Berichtsrenderer keinen Zwischenablagezugriff auf den Text unterstützt, der in den Berichten enthalten ist, kann auf den Text über eigenständige Berichte zugegriffen werden.  Laden Sie die eigenständigen Berichte herunter, wenn Sie Abfragetext aus diesen Berichten kopieren müssen.

## <a name="permissions"></a>Berechtigungen  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden die Berechtigungen `VIEW SERVER STATE` und `ALTER TRACE` benötigt. In [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] ist die Berechtigung `VIEW DATABASE STATE` in der Datenbank erforderlich.

## <a name="see-also"></a>Weitere Informationen  
 [Überwachen und Optimieren der Leistung](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Tools für die Leistungsüberwachung und -optimierung](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Öffnen des Aktivitätsmonitors &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [Aktivitätsmonitor](../../relational-databases/performance-monitor/activity-monitor.md)     
 [Überwachen der Leistung mit dem Abfragespeicher](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
