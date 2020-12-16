---
title: Übersicht über XEvents – SQL Server
description: Bei der Architektur für erweiterte Ereignisse in SQL Server können Sie die Daten sammeln, die erforderlich sind, um Leistungsprobleme zu identifizieren und zu beheben. Die Architektur kann konfiguriert und skaliert werden.
ms.date: 07/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: overview
helpviewer_keywords:
- extended events [SQL Server]
- xe
- XEvents
ms.assetid: bf3b98a6-51ed-4f2d-9c26-92f07f1fa947
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c837902edb7baa43d7fa07cfefc8ec335dc4183e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465531"
---
# <a name="extended-events-overview"></a>Übersicht über erweiterte Ereignisse

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Die Funktionalität von „Erweiterte Ereignisse“ ermöglicht es Benutzern, Daten in dem Umfang zu sammeln, der erforderlich ist, um Leistungsproblem zu erkennen oder zu beheben. „Erweiterte Ereignisse“ kann konfiguriert werden und lässt sich sehr gut skalieren.

Weitere Informationen zu „Erweiterte Ereignisse“ finden Sie unter [Schnellstart: Erweiterte Ereignisse in SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md).

## <a name="benefits-of-ssnoversion-extended-events"></a>Vorteile von erweiterten Ereignissen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  

„Erweiterte Ereignisse“ ist ein schlankes Leistungsüberwachungssystem, das minimale Leistungsressourcen verwendet. „Erweiterte Ereignisse“ stellt zwei grafische Benutzeroberflächen zum Erstellen, Ändern, Anzeigen und Analysieren von Sitzungsdaten bereit. Diese Oberflächen haben folgende Namen:

- Assistent für neue Sitzungen
- Neue Sitzung

## <a name="extended-events-concepts"></a>Konzepte für erweiterte Ereignisse  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] „Erweiterte Ereignisse“ basiert auf vorhandenen Konzepten, etwa einem Ereignis oder einem Ereignisconsumer, verwendet Konzepte aus der Ereignisablaufverfolgung für Windows und führt neue Konzepte ein.  
  
 In der folgenden Tabelle werden die Konzepte von "Erweiterte Ereignisse" beschrieben.  
  
|Thema|BESCHREIBUNG|  
|-----------|-----------------|  
|[Pakete für erweiterte Ereignisse von SQL Server](../../relational-databases/extended-events/sql-server-extended-events-packages.md)|Beschreibt die „Erweiterte Ereignisse“-Pakete, die Objekte enthalten. Diese Objekte werden verwendet, um Daten abzurufen und zu verarbeiten, wenn eine „Erweiterte Ereignisse“-Sitzung ausgeführt wird.|  
|[Ziele für erweiterte Ereignisse von SQL Server](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130))|Beschreibt die Ereignisconsumer, die während einer Ereignissitzung Daten empfangen können.|  
|[Engine für erweiterte Ereignisse von SQL Server](../../relational-databases/extended-events/sql-server-extended-events-engine.md)|Beschreibt die Engine, die eine Sitzung für erweiterte Ereignisse implementiert und verwaltet.|  
|[Sitzungen für erweiterte Ereignisse von SQL Server](../../relational-databases/extended-events/sql-server-extended-events-sessions.md)|Beschreibt die Sitzung für erweiterte Ereignisse.|  
| &nbsp; | &nbsp; |
  
## <a name="extended-events-architecture"></a>Architektur von erweiterten Ereignissen  

„Erweiterte Ereignisse“ ist unser Name für ein allgemeines Ereignisbehandlungssystem für Serversysteme. Die Infrastruktur für erweiterte Ereignisse unterstützt die Korrelation von Daten aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]sowie unter bestimmten Umständen die Korrelation von Daten aus dem Betriebssystem und aus Datenbankanwendungen. Für Daten vom Betriebssystem muss die Ausgabe von „Erweiterte Ereignisse“ an Ereignisablaufverfolgung für Windows (Event Tracing for Windows, ETW) weitergeleitet werden. ETW kann die Ereignisdaten mit Betriebssystem- oder Anwendungsereignisdaten korrelieren.  

Alle Anwendungen weisen Ausführungspunkte auf, die sowohl innerhalb der Anwendung als auch außerhalb nützlich sind. In der Anwendung kann die asynchrone Verarbeitung in die Warteschlange eingereiht werden, wobei Informationen zugrunde gelegt werden, die bei der ersten Ausführung eines Tasks gesammelt wurden. Außerhalb der Anwendung stellen Ausführungspunkte Überwachungshilfsprogrammen Informationen bereit. In den Informationen sind die Verhaltens- und Leistungsmerkmale der überwachten Anwendung beschrieben.  

 Erweiterte Ereignisse unterstützen die Verwendung von Ereignisdaten außerhalb eines Prozesses. Diese Daten werden i. d. R. folgendermaßen verwendet:  
  
-   Von Ablaufverfolgungstools wie der SQL-Ablaufverfolgung und dem Systemmonitor  
  
-   Von Tools für die Protokollierung wie dem Windows-Ereignisprotokoll oder dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Fehlerprotokoll  
  
-   Von Benutzern, die ein Produkt verwalten oder Anwendungen für ein Produkt entwickeln  
  
 Das Design von erweiterten Ereignissen zeichnet sich durch die folgenden zentralen Aspekte aus:  
  
-   Die Engine für erweiterte Ereignisse ist ereignisagnostisch. In der Engine kann jedes beliebige Ereignis an jedes beliebige Ziel gebunden werden, weil die Engine nicht durch den Ereignisinhalt eingeschränkt ist. Weitere Informationen zum Modul für erweiterte Ereignisse finden Sie unter [SQL Server Extended Events Engine](../../relational-databases/extended-events/sql-server-extended-events-engine.md).  
  
-   Ereignisse werden von Ereignisconsumern getrennt, die in erweiterten Ereignissen als *Ziele* bezeichnet werden. Das bedeutet, dass jedes Ziel jedes Ereignis empfangen kann. Zusätzlich kann jedes ausgelöste Ereignis automatisch vom Ziel verarbeitet werden, das dann wiederum die Protokollierung ausführen oder zusätzlichen Ereigniskontext bereitstellen kann. Weitere Informationen finden Sie unter [SQL Server Extended Events Targets](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130)).  
  
-   Ereignisse unterscheiden sich von der Aktion, die ausgeführt werden soll, wenn ein Ereignis auftritt. Dies führt dazu, dass jede beliebige Aktion jedem beliebigen Ereignis zugeordnet werden kann.  
  
-   Mithilfe von Prädikaten kann dynamisch gefiltert werden, wenn Ereignisdaten aufgezeichnet werden sollen. Dynamisches Filtern erhöht die Flexibilität der „Erweiterte Ereignisse“-Infrastruktur. Weitere Informationen finden Sie unter [SQL Server Extended Events Packages](../../relational-databases/extended-events/sql-server-extended-events-packages.md).  
  
 Erweiterte Ereignisse können Ereignisdaten synchron generieren (und asynchron verarbeiten), wodurch eine flexible Lösung für die Ereignisbehandlung bereitgestellt wird. Außerdem bieten erweiterte Ereignisse die folgenden Funktionen:  
  
-   Eine im gesamten Serversystem einheitliche Methode für die Ereignisbehandlung, wobei die Benutzer dennoch die Möglichkeit haben, einzelne Ereignisse für die Problembehandlung zu isolieren.  
  
-   Integration mit und Unterstützung von vorhandenen ETW-Tools  
  
-   Einen vollständig konfigurierbaren Ereignisbehandlungsmechanismus auf Grundlage von [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Die Möglichkeit zur dynamischen Überwachung aktiver Prozesse mit minimaler Beeinträchtigung dieser Prozesse.  
  
-   Eine standardmäßige Systemintegritätssitzung, die ohne merkliche Auswirkungen auf die Leistung ausgeführt wird. In der Sitzung werden Systemdaten erfasst, mit deren Hilfe Sie Leistungsprobleme beheben können. Weitere Informationen finden Sie unter [Verwenden der system_health-Sitzung](../../relational-databases/extended-events/use-the-system-health-session.md).  
  
## <a name="extended-events-tasks"></a>Tasks für erweiterte Ereignisse  

Durch Verwenden von [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)] zum Ausführen von [!INCLUDE[tsql](../../includes/tsql-md.md)] -DDL-Anweisungen (Data Definition Language), dynamischen Verwaltungssichten und -funktionen oder Katalogsichten können Sie einfache oder komplexe Problembehandlungslösungen für erweiterte Ereignisse von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für Ihre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Umgebung erstellen.  
  
|Taskbeschreibung|Thema|  
|----------------------|-----------|  
|Verwenden Sie den **Objekt-Explorer** , um Ereignissitzungen zu verwalten.|[Verwalten von Ereignissitzungen im Objekt-Explorer](../../relational-databases/extended-events/manage-event-sessions-in-the-object-explorer.md)|  
|Beschreibt, wie Sie eine Sitzung für erweiterte Ereignisse erstellen.|[Erstellen einer Sitzung für erweiterte Ereignisse](/previous-versions/sql/sql-server-2016/hh213147(v=sql.130))|  
|Beschreibt, wie Sie Zieldaten anzeigen und aktualisieren.| [Erweiterte Ansicht von Zieldaten aus erweiterten Ereignissen in SQL Server](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md)|  
|Beschreibt, wie Sie die folgenden Tools von erweiterten Ereignissen zum Erstellen und Verwalten von erweiterten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Ereignissitzungen verwenden:|[Tools für erweiterte Ereignisse](../../relational-databases/extended-events/extended-events-tools.md)|  
|Beschreibt, wie Sie eine Sitzung für erweiterte Ereignisse ändern.|[Ändern einer Sitzung für erweiterte Ereignisse](../../relational-databases/extended-events/alter-an-extended-events-session.md)|  
|Beschreibt, wie Sie Informationen zu den den Ereignissen zugeordneten Feldern abrufen.|[Abrufen der Felder für alle Ereignisse](/previous-versions/sql/sql-server-2016/bb677249(v=sql.130))|  
|Beschreibt, wie Sie herausfinden, welche Ereignisse in den registrierten Paketen verfügbar sind.|[Anzeigen der Ereignisse für registrierte Pakete](./selects-and-joins-from-system-views-for-extended-events-in-sql-server.md)|  
|Beschreibt, wie Sie ermitteln, welche Ziele für erweiterte Ereignisse in den registrierten Paketen verfügbar sind.|[Anzeigen der Ziele von erweiterten Ereignissen für registrierte Pakete](/previous-versions/sql/sql-server-2016/bb677247(v=sql.130))|  
|Beschreibt, wie Sie die Ereignisse und Aktionen für erweiterte Ereignisse anzeigen, die den einzelnen SQL-Ablaufverfolgungsereignissen und deren zugeordneten Spalten entsprechen.|[Anzeigen der Entsprechungen von erweiterten Ereignissen für SQL-Ablaufverfolgungsklassen](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md)|  
|Beschreibt, wie Sie die Parameter suchen, die sich festlegen lassen, wenn Sie das ADD TARGET-Argument in CREATE EVENT SESSION oder ALTER EVENT SESSION verwenden.|[Abrufen der konfigurierbaren Parameter für das ADD TARGET-Argument](/previous-versions/sql/sql-server-2016/bb677176(v=sql.130))|  
|Beschreibt, wie Sie ein vorhandenes SQL-Ablaufverfolgungsskript in eine Sitzung für erweiterte Ereignisse konvertieren.|[Konvertieren eines vorhandenen SQL-Ablaufverfolgungsskripts in eine Sitzung für erweiterte Ereignisse](../../relational-databases/extended-events/convert-an-existing-sql-trace-script-to-an-extended-events-session.md)|  
|Beschreibt die Ermittlung der gesperrten Abfragen, des Plans der Abfrage und des [!INCLUDE[tsql](../../includes/tsql-md.md)] -Stapels zum Zeitpunkt der Sperrung.|[Feststellen, welche Abfragen Sperren enthalten](../../relational-databases/extended-events/determine-which-queries-are-holding-locks.md)|  
|Beschreibt, wie Sie die Quelle von Sperren identifizieren, die die Datenbankleistung beeinträchtigen.|[Suchen der Objekte, die über die meisten Sperren verfügen](../../relational-databases/extended-events/find-the-objects-that-have-the-most-locks-taken-on-them.md)|  
|Beschreibt, wie Sie anhand von erweiterten Ereignissen mit der Ereignisablaufverfolgung für Windows die Systemaktivität überwachen.|[Überwachen der Systemaktivität mit erweiterten Ereignissen](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)|  
|Verwenden der Katalogsichten und dynamischen Verwaltungssichten für erweiterte Ereignisse | [SELECT- und JOIN-Anweisungen von Systemsichten für erweiterte Ereignisse in SQL Server](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md) |
| &nbsp; | &nbsp; |

Verwenden Sie die folgende Transact-SQL-Abfrage (T-SQL), um alle möglichen erweiterten Ereignisse und deren Beschreibungen aufzulisten:

```sql
SELECT
     obj1.name as [XEvent-name],
     col2.name as [XEvent-column],
     obj1.description as [Descr-name],
     col2.description as [Descr-column]
  FROM
               sys.dm_xe_objects        as obj1
      JOIN sys.dm_xe_object_columns as col2 on col2.object_name = obj1.name
  ORDER BY
    obj1.name,
    col2.name
```


## <a name="code-examples-can-differ-for-azure-sql-database"></a>Codebeispiele können für Azure SQL-Datenbank abweichend sein.

[!INCLUDE[sql-on-premises-vs-azure-similar-sys-views-include.](../../includes/paragraph-content/sql-on-premises-vs-azure-similar-sys-views-include.md)]

## <a name="see-also"></a>Weitere Informationen

[Datenebenenanwendungen](../../relational-databases/data-tier-applications/data-tier-applications.md)  
[DAC-Unterstützung für SQL Server-Objekte und -Versionen](/previous-versions/sql/sql-server-2012/ee210549(v=sql.110))  
[Bereitstellen einer Datenebenenanwendung](../../relational-databases/data-tier-applications/deploy-a-data-tier-application.md)  
[Überwachen von Datenebenenanwendungen](../../relational-databases/data-tier-applications/monitor-data-tier-applications.md)  
&nbsp;  
[Dynamische Verwaltungssichten für erweiterte Ereignisse](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)  
[Katalogsichten für erweiterte Ereignisse (Transact-SQL)](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)  
&nbsp;  
[XELite: Plattformübergreifende Bibliothek zum Lesen von XEvents aus XEL-Dateien oder SQL-Livestreams](https://www.nuget.org/packages/Microsoft.SqlServer.XEvent.XELite/) (Veröffentlicht: Mai 2019).  
[PowerShell-Cmdlet Read-SQLXEvent](https://www.powershellgallery.com/packages/SqlServer.XEvent) (Veröffentlicht: Juni 2019).  
[SQL Mysteries: Causality tracking vs Event Sequence for XEvent Sessions](https://bobsql.com/sql-mysteries-causality-tracking-vs-event-sequence-for-xevent-sessions/) (Blogbeitrag, der am 1. April 2019 veröffentlicht wurde)