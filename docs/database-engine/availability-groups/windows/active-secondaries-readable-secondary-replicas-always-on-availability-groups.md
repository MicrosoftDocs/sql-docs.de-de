---
title: Auslagern der Arbeitsauslastung zu sekundären Verfügbarkeitsgruppenreplikaten
description: Erfahren Sie mehr über das Auslagern von schreibgeschützten Abfragen und Berichten auf ein sekundäres Replikat einer Always On-Verfügbarkeitsgruppe in SQL Server.
ms.custom: seo-lt-2019
ms.date: 06/06/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- connection access to availability replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- readable secondary replicas
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 78f3f81a-066a-4fff-b023-7725ff874fdf
author: cawrites
ms.author: chadam
ms.openlocfilehash: 02e07cbb5b0fd6a917d6455c9d41a0fda3f03055
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343949"
---
# <a name="offload-read-only-workload-to-secondary-replica-of-an-always-on-availability-group"></a>Auslagern von schreibgeschützten Arbeitsauslastungen auf ein sekundäres Replikat einer Always On-Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Die Funktionen für aktive sekundäre Replikate in [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] umfassen Unterstützung für den schreibgeschützten Zugriff auf ein oder mehrere sekundäre Replikate (*lesbare sekundäre Replikate*). Ein lesbares sekundäres Replikat kann sich entweder im Verfügbarkeitsmodus mit synchronem oder asynchronem Commit befinden. Lesbare sekundäre Replikate lassen den schreibgeschützten Zugriff auf alle eigenen sekundären Datenbanken zu. Bei lesbaren sekundären Datenbanken ist jedoch kein Schreibschutz festgelegt. Sie sind dynamisch. Eine sekundäre Datenbank wird geändert, wenn Änderungen an der zugehörigen primären Datenbank auf die sekundäre Datenbank angewendet werden. Bei einem typischen sekundären Replikat liegen die Daten in den sekundären Datenbanken nahezu in Echtzeit vor. Dies gilt auch für dauerhafte speicheroptimierte Tabellen. Weiterhin werden Volltextindizes mit den sekundären Datenbanken synchronisiert. In vielen Fällen beträgt die Datenlatenz zwischen einer primären Datenbank und der zugehörigen sekundären Datenbank nur wenige Sekunden.  
  
 Sicherheitseinstellungen, die in den primären Datenbanken auftreten, werden in den sekundären Datenbanken beibehalten. Dazu gehören Benutzer, Datenbankrollen und Anwendungsrollen sowie die jeweiligen zugehörigen Berechtigungen und die transparente Datenverschlüsselung (TDE), wenn diese in der primären Datenbank aktiviert ist.  
  
> [!NOTE]  
>  Sie können zwar keine Daten in sekundäre Datenbanken schreiben, aber in Datenbanken mit Lese-/Schreibzugriff auf der Serverinstanz, auf der die sekundären Replikate gehostet werden, einschließlich Benutzerdatenbanken und Systemdatenbanken, wie **tempdb**.  
  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] unterstützt auch das Umleiten von Verbindungsanforderungen für beabsichtigte Lesevorgänge an ein lesbares sekundäres Replikat (*schreibgeschütztes Routing*). Weitere Informationen zum schreibgeschützten Routing finden Sie unter [Verwenden eines Listeners zum Herstellen einer Verbindung mit einem schreibgeschützten sekundären Replikat (schreibgeschütztes Routing)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md#ConnectToSecondary).  
  
##  <a name="benefits"></a><a name="bkmk_Benefits"></a> Vorteile  
 Die Weiterleitung schreibgeschützter Verbindungen an lesbare sekundäre Replikate bietet die folgenden Vorteile:  
  
-   Entlastet das primäre Replikat von sekundären schreibgeschützten Arbeitsauslastungen, wodurch dessen Ressourcen für unternehmenswichtige Arbeitsauslastungen zur Verfügung stehen. Unternehmenswichtige Lesearbeitsauslastungen oder Arbeitsauslastungen, die keine Latenzen tolerieren, sollten auf dem primären Replikat ausgeführt werden.  
  
-   Verbessert die Rendite von Systemen, auf denen lesbare sekundäre Replikate gehostet werden.  
  
 Außerdem bieten lesbare sekundäre Replikate folgendermaßen stabile Unterstützung für schreibgeschützte Vorgänge:  
  
-   Durch automatische temporäre Statistiken für lesbare sekundäre Datenbanken werden schreibgeschützte Abfragen in datenträgerbasierten Tabellen optimiert. Bei speicheroptimierten Tabellen werden die fehlenden Statistiken automatisch erstellt. Veraltete Statistiken werden jedoch nicht automatisch aktualisiert. Sie müssen die Statistiken auf dem primären Replikat manuell aktualisieren. Weitere Informationen finden Sie unter [Statistiken für Datenbanken mit schreibgeschütztem Zugriff](#Read-OnlyStats)weiter unten in diesem Thema.  
  
-   Schreibgeschützte Arbeitsauslastungen für datenträgerbasierte Tabellen verwenden die Zeilenversionsverwaltung, um Blockierungskonflikte für sekundäre Datenbanken aufzuheben. Alle Abfragen für sekundäre Datenbanken werden automatisch der Momentaufnahme-Transaktionsisolationsstufe zugeordnet, selbst wenn explizit andere Transaktionsisolationsstufen festgelegt wurden. Zudem werden alle Sperrhinweise ignoriert. Dies schließt Leser-/Schreiberkonflikte aus.  
  
-   Schreibgeschützte Arbeitsauslastungen für dauerhafte speicheroptimierte Tabellen greifen unter Verwendung von systemeigenen gespeicherten Prozeduren oder SQL-Interoperabilität, für die hinsichtlich der Transaktionsisolationsstufen dieselben Einschränkungen gelten (siehe [Isolationsstufen in der Datenbank-Engine](/previous-versions/sql/sql-server-2008-r2/ms189122(v=sql.105))), auf genau dieselbe Weise auf Daten wie in der primären Datenbank zu. Auf dem primären Replikat ausgeführte Arbeitsauslastungen für die Berichterstellung oder schreibgeschützte Abfragen können unverändert auf dem sekundären Replikat ausgeführt werden. Analog dazu können auf einem sekundären Replikat ausgeführte Arbeitsauslastungen für die Berichterstellung oder schreibgeschützte Abfragen unverändert auf dem primären Replikat ausgeführt werden.  Ähnlich wie bei datenträgerbasierten Tabellen werden alle Abfragen für sekundäre Datenbanken automatisch der Momentaufnahme-Transaktionsisolationsstufe zugeordnet, selbst wenn explizit andere Transaktionsisolationsstufen festgelegt wurden.  
  
-   DML-Vorgänge sind für Tabellenvariablen datenträgerbasierter und speicheroptimierter Tabellentypen auf dem sekundären Replikat zulässig.  
  
##  <a name="prerequisites-for-the-availability-group"></a><a name="bkmk_Prerequisites"></a> Voraussetzungen für die Verfügbarkeitsgruppe  
  
-   **Lesbare sekundäre Replikate (erforderlich)**  
  
     Der Datenbankadministrator muss mindestens ein Replikat so konfigurieren, dass es bei Ausführung in der sekundären Rolle alle Verbindungen (nur für den schreibgeschützten Zugriff) oder aber nur Verbindungen für beabsichtigte Lesevorgänge zulässt.  
  
    > [!NOTE]  
    >  Optional kann der Datenbankadministrator eines der Verfügbarkeitsreplikate so konfigurieren, dass schreibgeschützte Verbindungen bei Ausführung in der primären Rolle ausgeschlossen werden.
  
     Weitere Informationen finden Sie unter [Informationen zum Clientverbindungszugriff auf Verfügbarkeitsreplikate &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md).  
  
    >[!WARNING]
    >  Nur Replikate desselben Hauptbuilds von SQL Server sind lesbar. Weitere Informationen finden Sie unter [Grundlagen zu parallelen Upgrades](upgrading-always-on-availability-group-replica-instances.md#rolling-upgrade-basics-for-always-on-ags).
  
  
-   **Verfügbarkeitsgruppenlistener**  
  
     Um schreibgeschütztes Routing zu unterstützen, muss eine Verfügbarkeitsgruppe einen [Verfügbarkeitsgruppenlistener](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)besitzen. Der schreibgeschützte Client muss die eigenen Verbindungsanforderungen an diesen Listener weiterleiten, und in der Verbindungszeichenfolge des Clients muss der Anwendungszweck als "schreibgeschützt" angegeben sein. Es muss sich also um *Verbindungsanforderungen für beabsichtigte Lesevorgänge* handeln.  
  
-   **Schreibgeschütztes Routing**  
  
     Als *schreibgeschütztes Routing* wird die Fähigkeit von SQL Server bezeichnet, eingehende Verbindungsanforderungen für beabsichtigte Lesevorgänge, die an einen Verfügbarkeitsgruppenlistener gerichtet sind, an ein verfügbares lesbares sekundäres Replikat weiterzuleiten. Für schreibgeschütztes Routing gelten folgende Voraussetzungen:  
  
    -   Zur Unterstützung von schreibgeschütztem Routing muss für lesbare sekundäre Replikate eine URL für schreibgeschütztes Routing angegeben werden. Diese URL wird nur wirksam, wenn das lokale Replikat unter der sekundären Rolle ausgeführt wird. Die URL für schreibgeschütztes Routing muss nach Bedarf replikatweise angegeben werden. Jede URL für schreibgeschütztes Routing wird zum Weiterleiten von Verbindungsanforderungen für beabsichtigte Lesevorgänge an ein bestimmtes lesbares sekundäres Replikat verwendet. In der Regel wird jedem lesbaren sekundären Replikat eine URL für schreibgeschütztes Routing zugewiesen.  
  
    -   Jedes Verfügbarkeitsreplikat, das als primäres Replikat schreibgeschütztes Routing unterstützen soll, erfordert eine Liste für schreibgeschütztes Routing. Eine Liste für schreibgeschütztes Routing wird nur wirksam, wenn das lokale Replikat unter der primären Rolle ausgeführt wird. Diese Liste muss nach Bedarf replikatweise angegeben werden. Normalerweise enthält jede Liste für schreibgeschütztes Routing jede URL für schreibgeschütztes Routing, wobei die URL des lokalen Replikats am Ende der Liste steht.  
  
        > [!NOTE]  
        >  Für Verbindungsanforderungen beabsichtigter Lesevorgänge kann replikateübergreifend ein Lastenausgleich ausgeführt werden. Weitere Informationen finden Sie unter [Konfigurieren von Lastenausgleich über schreibgeschützte Replikate hinweg](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing).  
  
     Weitere Informationen finden Sie unter [Konfigurieren des schreibgeschützten Routing für eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md).  
  
> [!NOTE]  
>  Weitere Informationen zu Verfügbarkeitsgruppenlistenern und zum schreibgeschützten Routing finden Sie unter [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)besitzen.  
  
##  <a name="limitations-and-restrictions"></a><a name="bkmk_LimitationsRestrictions"></a> Einschränkungen  
 Folgende Vorgänge werden nicht vollständig unterstützt:  
  
-   Sobald für ein lesbares Replikat der Lesezugriff aktiviert ist, können Verbindungsanforderungen für die sekundären Datenbanken angenommen werden. Wenn in einer primären Datenbank jedoch aktive Transaktionen vorhanden sind, sind die Zeilenversionen in der entsprechenden sekundären Datenbank nicht vollständig verfügbar. Für jegliche aktiven Transaktionen, die bei der Konfiguration des sekundären Replikats auf dem primären Replikat vorhanden waren, muss ein Commit oder ein Rollback durchgeführt werden. Bis dieser Prozess abgeschlossen ist, ist die Zuordnung der Transaktionsisolationsstufe auf der sekundären Datenbank unvollständig, und Abfragen werden vorübergehend blockiert.  
  
    > [!WARNING]  
    >  Die Ausführung von Transaktionen mit langer Ausführungszeit wirkt sich darauf aus, wie viele Zeilen mit Versionsangabe für datenträgerbasierte und speicheroptimierte Tabellen beibehalten werden.  
  
-   Obwohl für speicheroptimierte Tabellen immer Zeilenversionen generiert werden, werden bei einer sekundären Datenbank mit speicheroptimierten Tabellen Abfragen blockiert, bis alle aktiven Transaktionen abgeschlossen sind, die sich im primären Replikat befanden, während der Lesezugriff für das sekundäre Replikat aktiviert wurde. Dadurch wird sichergestellt, dass der Arbeitsauslastung für die Berichterstellung und den schreibgeschützten Abfragen sowohl datenträgerbasierte als auch speicheroptimierte Tabellen gleichzeitig zur Verfügung stehen.  
  
-   Die Änderungsnachverfolgung und Change Data Capture werden nicht für sekundäre Datenbanken unterstützt, die zu einem lesbaren sekundären Replikat gehören:  
  
    -   Die Änderungsnachverfolgung ist auf sekundären Datenbanken explizit deaktiviert.  
  
    -   Change Data Capture kann nur für eine sekundäre Replikatdatenbank aktiviert werden. Change Data Capture kann für die primäre Replikatdatenbank aktiviert werden, und die Änderungen können mithilfe der Funktionen für die sekundäre Replikatdatenbank aus den CDC-Tabellen gelesen werden.  
  
-   Da Lesevorgänge der Momentaufnahme-Transaktionsisolationsstufe zugeordnet werden, kann das Cleanup von inaktiven Datensätzen auf dem primären Replikat durch Transaktionen auf einem oder mehreren sekundären Replikaten blockiert werden. Mit der Bereinigungsaufgabe für inaktive Datensätze werden die inaktiven Datensätze für datenträgerbasierte Tabellen auf dem primären Replikat automatisch bereinigt, wenn sie von sekundären Replikaten nicht mehr benötigt werden.  Dies ist analog zur Ausführung von Transaktionen auf dem primären Replikat. Im äußersten Fall müssen Sie Leseabfragen mit langer Laufzeit in der sekundären Datenbank abbrechen, die die Bereinigung der inaktiven Datensätze blockieren. Hinweis: Die Bereinigung des inaktiven Elements kann blockiert werden, wenn das sekundäre Replikat getrennt oder die Datenverschiebung in der sekundären Datenbank angehalten wird. Mit diesem Status wird zudem die Protokollkürzung verhindert. Wenn dieser Status weiterhin auftritt, sollten Sie demnach diese sekundäre Datenbank aus der Verfügbarkeitsgruppe entfernen. Bei speicheroptimierten Tabellen besteht kein Problem mit dem Cleanup inaktiver Datensätze, da die Zeilenversionen im Arbeitsspeicher beibehalten werden und unabhängig von den Zeilenversionen auf dem primären Replikat sind.  
  
-   Beim DBCC SHRINKFILE-Vorgang für Dateien mit datenträgerbasierten Tabellen kann auf dem primären Replikat ein Fehler auftreten, wenn die Datei inaktive Datensätze enthält, die auf einem sekundären Replikat weiterhin benötigt werden.  
  
-   Ab [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] können lesbare sekundäre Replikate selbst dann online verfügbar bleiben, wenn das primäre Replikat aufgrund einer Benutzeraktion oder eines Fehlers offline ist, z. B. weil die Synchronisierung aufgrund eines Benutzerbefehls oder eines Fehlers angehalten wurde oder der Status eines Replikats aufgelöst wurde, da der WSFC offline ist. Allerdings ist in diesem Fall kein schreibgeschütztes Routing möglich, da der Verfügbarkeitsgruppenlistener ebenfalls offline ist. Clients müssen bei schreibgeschützten Arbeitsauslastungen direkt eine Verbindung mit den schreibgeschützten sekundären Replikaten herstellen.  
  
> [!NOTE]  
>  Wenn Sie die dynamische Verwaltungssicht [sys.dm_db_index_physical_stats](../../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) auf einer Serverinstanz abfragen, die ein lesbares sekundäres Replikat hostet, kann ein REDO-Blockierungsproblem auftreten. Dies kommt daher, dass diese dynamische Verwaltungssicht eine IS-Sperre für die angegebene Benutzertabelle oder Sicht erhält, die Anforderungen von einem REDO-Thread für eine X-Sperre dieser Benutzertabelle oder Sicht blockieren kann.  
  
##  <a name="performance-considerations"></a><a name="bkmk_Performance"></a> Leistungsaspekte  
 In diesem Abschnitt werden mehrere Überlegungen zur Leistung für lesbare sekundäre Datenbanken erläutert.  
  
 **In diesem Abschnitt:**  
  
-   [Datenlatenz](#DataLatency)  
  
-   [Auswirkungen auf schreibgeschützte Arbeitsauslastungen](#ReadOnlyWorkloadImpact)  
  
-   [Indizieren](#bkmk_Indexing)  
  
-   [Statistiken für Datenbanken mit schreibgeschütztem Zugriff](#Read-OnlyStats)  
  
###  <a name="data-latency"></a><a name="DataLatency"></a> Datenlatenz  
 Das Implementieren von schreibgeschütztem Zugriff auf sekundäre Replikate ist hilfreich, sofern Ihre schreibgeschützten Arbeitsauslastungen eine gewisse Datenlatenz tolerieren können. Führen Sie in Situationen mit inakzeptabler Datenlatenz ggf. schreibgeschützte Arbeitsauslastungen für das primäre Replikat aus.  
  
 Vom primären Replikat werden Protokolldatensätze der Änderungen in der primären Datenbank an die sekundären Replikate gesendet. In der jeweiligen sekundären Datenbank werden die Protokolldatensätze von einem dedizierten REDO-Thread angewendet. In einer schreibgeschützten sekundären Datenbank erscheint eine angegebene Datenänderung erst in den Abfrageergebnissen, wenn der Protokolldatensatz, der die Änderung enthält, auf die sekundäre Datenbank angewendet und für die Transaktion in der primären Datenbank ein Commit ausgeführt wurde.  
  
 Dies weist auf eine gewisse Latenz zwischen den primären und sekundären Replikaten hin, wobei es sich in der Regel nur um wenige Sekunden handelt. In außergewöhnlichen Fällen, beispielsweise bei Netzwerkproblemen, die den Durchsatz reduzieren, kann die Latenz jedoch signifikant werden. Die Latenz nimmt bei E/A-Engpässen und bei angehaltener Datenverschiebung zu. Zur Überwachung einer angehaltenen Datenverschiebung können Sie das [AlwaysOn-Dashboard](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md) oder die dynamische Verwaltungssicht [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) verwenden.  
  
####  <a name="data-latency-on-databases-with-memory-optimized-tables"></a><a name="bkmk_LatencyWithInMemOLTP"></a> Datenlatenz bei Datenbanken mit speicheroptimierten Tabellen  
 In [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] galten besondere Überlegungen zur Datenlatenz für aktive sekundäre Replikate. Informationen dazu finden Sie unter [[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] Aktive sekundäre Replikate: Lesbare sekundäre Replikate (Always On-Verfügbarkeitsgruppen)](https://technet.microsoft.com/library/ff878253(v=sql.120).aspx). Ab [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] gelten keine Besonderheiten mehr bezüglich der Datenlatenz für speicheroptimierte Tabellen. Die erwartete Datenlatenz für speicheroptimierte Tabellen ist mit der Latenz für datenträgerbasierte Tabellen vergleichbar.  
  
###  <a name="read-only-workload-impact"></a><a name="ReadOnlyWorkloadImpact"></a> Auswirkungen auf schreibgeschützte Arbeitsauslastungen  
 Wenn Sie ein sekundäres Replikat für schreibgeschützten Zugriff konfigurieren, belegen die schreibgeschützten Arbeitsauslastungen in den sekundären Datenbanken Systemressourcen, z. B. CPU und E/A-Vorgänge (für datenträgerbasierte Tabellen) aus REDO-Threads, insbesondere wenn die schreibgeschützten Arbeitsauslastungen für datenträgerbasierte Tabellen äußerst E/A-intensiv sind. Der Zugriff auf speicheroptimierte Tabellen hat keine Auswirkungen auf die E/A-Leistung, weil alle Zeilen im Arbeitsspeicher enthalten sind.  
  
 Schreibgeschützte Arbeitsauslastungen auf den sekundären Replikaten können zudem Änderungen der Datendefinitionssprache (DDL) blockieren, die durch Protokolldatensätze angewendet werden.  
  
-   Obwohl die Lesevorgänge wegen der Zeilenversionsverwaltung über keine gemeinsamen Sperren verfügen, basieren diese Vorgänge auf Schemastabilitätssperren (Sch-S). Dadurch werden u. U. REDO-Vorgänge blockiert, die DDL-Änderungen anwenden. DDL-Vorgänge umfassen ALTER/DROP-Tabellen und -Sichten, aber keine DROP- oder ALTER-Anweisungen gespeicherter Prozeduren. Angenommen, Sie löschen eine datenträgerbasierte oder speicheroptimierte Tabelle auf dem primären Replikat. Wenn der Protokolldatensatz von einem REDO-Thread verarbeitet wird, um die Tabelle zu löschen, muss er eine SCH_M-Sperre für die Tabelle abrufen und kann durch eine laufende Abfrage, die auf die Tabelle zugreift, blockiert werden.  Auf dem primärem Replikat ist das Verhalten identisch, mit der Ausnahme, dass die Tabelle im Rahmen einer Benutzersitzung und nicht in einem REDO-Thread gelöscht wird.  
  
-   Es gibt weitere Gründe für das Blockieren speicheroptimierter Tabellen. Durch das Löschen einer systemeigenen gespeicherten Prozedur kann ein REDO-Thread blockiert werden, wenn die systemeigene gespeicherte Prozedur gleichzeitig auf dem sekundären Replikat ausgeführt wird. Auf dem primärem Replikat ist das Verhalten identisch, mit der Ausnahme, dass die gespeicherte Prozedur im Rahmen einer Benutzersitzung und nicht in einem REDO-Thread gelöscht wird.  
  
 Beachten Sie die bewährten Methoden zum Erstellen von Abfragen, und wenden Sie diese Methoden auf die sekundären Datenbanken an. Planen Sie beispielsweise Abfragen mit langer Laufzeit wie Datenaggregationen in Zeiträumen mit geringer Aktivität.  
  
> [!NOTE]  
>  Wird ein REDO-Thread von Abfragen auf dem sekundären Replikat blockiert, wird das XEvent **sqlserver.lock_redo_blocked** ausgelöst.  
  
###  <a name="indexing"></a><a name="bkmk_Indexing"></a> Indizierung  
 Um schreibgeschützte Arbeitsauslastungen auf den lesbaren sekundären Replikaten zu optimieren, können Sie ggf. Indizes in den Tabellen der sekundären Datenbanken erstellen. Da Sie keine Schema- oder Datenänderungen auf den sekundären Datenbanken vornehmen können, erstellen Sie Indizes in den primären Datenbanken, und lassen Sie die Übertragung der Änderungen auf die sekundäre Datenbank mittels REDO-Prozess zu.  
  
 Zur Überwachung der Indexverwendung auf einem sekundären Replikat fragen Sie die Spalten **user_seeks**, **user_scans** und **user_lookups** der dynamischen Verwaltungssicht [sys.dm_db_index_usage_stats](../../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md) ab.  
  
###  <a name="statistics-for-read-only-access-databases"></a><a name="Read-OnlyStats"></a> Statistiken für Datenbanken mit schreibgeschütztem Zugriff  
 Statistiken zu Spalten von Tabellen und indizierten Sichten werden verwendet, um Abfragepläne zu optimieren. Bei Verfügbarkeitsgruppen werden Statistiken, die in den primären Datenbanken erstellt und verwaltet werden, automatisch in den sekundären Datenbanken beibehalten, wenn die Transaktionsprotokoll-Datensätze angewendet werden. Für die schreibgeschützte Arbeitsauslastung in den sekundären Datenbanken sind jedoch möglicherweise andere Statistiken als die erforderlich, die in den primären Datenbanken erstellt werden. Da sekundäre Datenbanken jedoch auf schreibgeschützten Zugriff beschränkt sind, können für die sekundären Datenbanken keine Statistiken erstellt werden.  
  
 Zur Behebung dieses Problems erstellt und verwaltet das sekundäre Replikat temporäre Statistiken für sekundäre Datenbanken in **tempdb**. Das Suffix „_readonly_database_statistic“ wird an den Namen temporärer Statistiken angefügt, um die temporären Statistiken von den dauerhaften Statistiken zu unterscheiden, die von der primären Datenbank beibehalten werden.  
  
 Nur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] kann temporäre Statistiken erstellen und aktualisieren. Sie können jedoch temporäre Statistiken löschen und ihre Eigenschaften mit den gleichen Tools überwachen, die Sie für dauerhafte Statistiken verwenden:  
  
-   Löschen Sie temporäre Statistiken mit der Anweisung [DROP STATISTICS](../../../t-sql/statements/drop-statistics-transact-sql.md) [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
-   Überwachen Sie Statistiken mit den Katalogsichten **sys.stats** und **sys.stats_columns** . **sys_stats** beinhaltet eine Spalte, **is_temporary**. Damit wird angegeben, welche Statistiken dauerhaft und welche temporär sind.  
  
 Automatische Updates von Statistiken werden für speicheroptimierte Tabellen auf dem primären oder sekundären Replikat nicht unterstützt. Sie müssen die Abfrageleistung und Pläne auf dem sekundären Replikat überwachen und die Statistiken auf dem primären Replikat ggf. manuell aktualisieren. Allerdings werden die fehlenden Statistiken auf dem primären und sekundären Replikat automatisch erstellt.  
  
 Weitere Informationen zu SQL Server-Statistiken finden Sie unter [Statistik](../../../relational-databases/statistics/statistics.md).  
  
 **In diesem Abschnitt:**  
  
-   [Veraltete dauerhafte Statistiken in sekundären Datenbanken](#StalePermStats)  
  
-   [Einschränkungen](#StatsLimitationsRestrictions)  
  
####  <a name="stale-permanent-statistics-on-secondary-databases"></a><a name="StalePermStats"></a> Veraltete dauerhafte Statistiken in sekundären Datenbanken  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] erkennt, wenn dauerhafte Statistiken in einer sekundären Datenbank veraltet sind. Änderungen an den dauerhaften Statistiken können aber nur durch Änderungen an der primären Datenbank vorgenommen werden. Zur Abfrageoptimierung erstellt [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] temporäre Statistiken für datenträgerbasierte Tabellen in der sekundären Datenbank und verwendet diese Statistiken anstelle der veralteten dauerhaften Statistiken.  
  
 Wenn die dauerhaften Statistiken in der primären Datenbank aktualisiert werden, werden sie automatisch in der sekundären Datenbank dauerhaft gespeichert. Dann verwendet [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] die aktualisierten dauerhaften Statistiken, die aktueller als die temporären Statistiken sind.  
  
 Wenn die Verfügbarkeitsgruppe ein Failover ausführt, werden temporäre Statistiken auf allen sekundären Replikaten gelöscht.  
  
####  <a name="limitations-and-restrictions"></a><a name="StatsLimitationsRestrictions"></a> Einschränkungen  
  
-   Da temporäre Statistiken in **tempdb** gespeichert werden, werden durch einen Neustart des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Diensts alle temporären Statistiken entfernt.  
  
-   Das Suffix „_readonly_database_statistic“ ist für von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]generierte Statistiken reserviert. Sie können dieses Suffix nicht verwenden, wenn Sie Statistiken in einer primären Datenbank erstellen. Weitere Informationen finden Sie unter [Verwalten von Statistiken für Tabellen in SQL Data Warehouse](../../../relational-databases/statistics/statistics.md).  
  
##  <a name="accessing-memory-optimized-tables-on-a-secondary-replica"></a><a name="bkmk_AccessInMemTables"></a> Zugreifen auf speicheroptimierte Tabellen auf einem sekundären Replikat  
 Mit speicheroptimierten Tabellen können für primäre und sekundäre Replikate dieselben Transaktionsisolationsstufen verwendet werden. Es wird empfohlen, die Isolationsstufe auf Sitzungsebene auf READ COMMITTED und die Datenbankebenenoption auf MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT auf ON festzulegen. Beispiel:  
  
```sql  
ALTER DATABASE CURRENT SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON  
GO  
SET TRANSACTION ISOLATION LEVEL READ COMMITTED  
GO  
SELECT SUM(UnitPrice*OrderQty)   
FROM Sales.SalesOrderDetail_inmem  
GO  
  
```  
  
##  <a name="capacity-planning-considerations"></a><a name="bkmk_CapacityPlanning"></a> Aspekte der Kapazitätsplanung  
  
-   Bei datenträgerbasierten Tabellen können lesbare sekundäre Replikate aus zwei Gründen Speicherplatz in **tempdb** beanspruchen:  
  
    -   In der Momentaufnahme-Isolationsstufe werden Zeilenversionen in **tempdb** kopiert.  
  
    -   Temporäre Statistiken für sekundäre Datenbanken werden in **tempdb** erstellt und verwaltet. Die temporären Statistiken können einen leichten Anstieg der Größe von **tempdb** verursachen. Weitere Informationen finden Sie unter [Statistiken für Datenbanken mit schreibgeschütztem Zugriff](#Read-OnlyStats)später in diesem Abschnitt.  
  
-   Wenn Sie für eines oder mehrere sekundäre Replikate Lesezugriff konfigurieren, wird von den primären Datenbanken für gelöschte, geänderte oder eingefügte Datenzeilen beim Speichern von Zeigern auf Zeilenversionen in den sekundären Datenbanken für datenträgerbasierte Tabellen ein Mehraufwand von 14 Bytes verursacht. Dieser Mehraufwand von 14 Bytes geht auf die sekundären Datenbanken über. Da der Mehraufwand von 14 Bytes auf die Datenzeilen übertragen wird, können Seitenteilungen auftreten.  
  
     Die Zeilenversionsdaten werden nicht von den primären Datenbanken generiert. Stattdessen werden die Zeilenversionen von den sekundären Datenbanken generiert. Durch die Zeilenversionsverwaltung nimmt jedoch die Datenspeicherung in der primären und in der sekundären Datenbank zu.  
  
     Das Hinzufügen der Zeilenversionsdaten hängt von der Momentaufnahmeisolation oder der Einstellung der READ_COMMITTED_SNAPSHOT-Isolationsstufe (RCSI) in der primären Datenbank ab. In der folgenden Tabelle wird das Verhalten der Versionsverwaltung in einer lesbaren sekundären Datenbank mit unterschiedlichen Einstellungen für datenträgerbasierte Tabellen beschrieben.  
  
    |Lesbares sekundäres Replikat?|Ist Momentaufnahmeisolation oder RCSI-Stufe aktiviert?|Primäre Datenbank|Sekundäre Datenbank|  
    |---------------------------------|-----------------------------------------------|----------------------|------------------------|  
    |Nein|Nein|Keine Zeilenversionen oder 14-Byte-Mehraufwand|Keine Zeilenversionen oder 14-Byte-Mehraufwand|  
    |Nein|Ja|Zeilenversionen und 14-Byte-Mehraufwand|Keine Zeilenversionen, aber 14-Byte-Mehraufwand|  
    |Ja|Nein|Keine Zeilenversionen, aber 14-Byte-Mehraufwand|Zeilenversionen und 14-Byte-Mehraufwand|  
    |Ja|Ja|Zeilenversionen und 14-Byte-Mehraufwand|Zeilenversionen und 14-Byte-Mehraufwand|  
  
##  <a name="related-tasks"></a><a name="bkmk_RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Konfigurieren des schreibgeschützten Zugriffs auf ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Konfigurieren des schreibgeschützten Routing für eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Überwachen von Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
-   [Anzeigen von Verfügbarkeitsreplikateigenschaften &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [Verwenden des Dialogfelds Neue Verfügbarkeitsgruppe &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   [SQL Server Always On Team Blog: The official SQL Server Always On Team Blog (SQL Server Always On-Teamblog: Der offizielle SQL Server Always On-Teamblog)](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Informationen zum Clientverbindungszugriff auf Verfügbarkeitsreplikate &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Statistik](../../../relational-databases/statistics/statistics.md)  
  
