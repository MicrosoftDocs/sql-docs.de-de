---
title: Überwachen der Datenbankspiegelung (SQL Server) | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über den Datenbankspiegelungs-Monitor, gespeicherte Systemprozeduren und die Funktionsweise der Datenbankspiegelungs-Überwachung, einschließlich des Auftrags für den Datenbankspiegelungs-Monitor.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- monitoring [SQL Server], database mirroring
- database mirroring [SQL Server], monitoring
ms.assetid: a7b1b9b0-7c19-4acc-9de3-3a7c5e70694d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 5dc7d451b3e6b26f06bb5273d5f043d9aff0d463
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343307"
---
# <a name="monitoring-database-mirroring-sql-server"></a>Überwachen der Datenbankspiegelung (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Dieser Abschnitt stellt den Datenbankspiegelungs-Monitor und die gespeicherten Prozeduren **sp_dbmmonitor** vor, erklärt die Funktionsweise der Überwachung von Datenbankspiegelungen (mit dem **Auftrag für den Datenbankspiegelungs-Monitor**) und bietet einen Überblick über die Informationen, die Sie im Zusammenhang mit Datenbankspiegelungssitzungen überwachen können. Außerdem erläutert dieser Abschnitt das Definieren von Warnschwellenwerten für eine Reihe vordefinierter Datenbankspiegelungsereignisse sowie das Einrichten von Warnungen für jedes Datenbankspiegelungsereignis.  
  
 Sie können eine gespiegelte Datenbank während einer Spiegelungssitzung überwachen, um zu überprüfen, ob und auf welche Weise ein Datenfluss stattfindet. Zum Einrichten und Verwalten der Überwachung für eine oder mehrere gespiegelte Datenbanken auf einer Serverinstanz können Sie entweder den Datenbanküberwachungs-Monitor oder die gespeicherten Systemprozeduren **sp_dbmmonitor** verwenden.  
  
 Der als **Auftrag für den Datenbankspiegelungs-Monitor** bezeichnete Auftrag für die Überwachung der Datenbankspiegelung wird unabhängig vom Datenbankspiegelungs-Monitor im Hintergrund ausgeführt. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ruft den **Auftrag für den Datenbankspiegelungs-Monitor** in regelmäßigen Intervallen auf (Standard ist einmal pro Minute), und der Auftrag ruft eine gespeicherte Prozedur auf, die den Spiegelstatus aktualisiert. Wenn Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] zum Starten einer Spiegelungssitzung verwenden, wird der **Auftrag für den Datenbankspiegelungs-Monitor** automatisch erstellt. Wenn Sie jedoch nur ALTER DATABASE *<Datenbankname>* SET PARTNER zum Starten der Spiegelung verwenden, müssen Sie den Auftrag erstellen, indem Sie eine gespeicherte Prozedur ausführen.  
  
 **In diesem Thema:**  
  
-   [Überwachen des Spiegelungsstatus](#MonitoringStatus)  
  
-   [Zusätzliche Informationsquellen zu gespiegelten Datenbanken](#AdditionalSources)  
  
-   [Verwandte Aufgaben](#RelatedTasks)  
  
##  <a name="monitoring-mirroring-status"></a><a name="MonitoringStatus"></a> Überwachen des Spiegelungsstatus  
 Zum Einrichten und Verwalten der Überwachung für eine oder mehrere gespiegelte Datenbanken auf einer Serverinstanz können Sie entweder den Datenbanküberwachungs-Monitor oder die gespeicherten Systemprozeduren von **dbmmonitor** verwenden. Sie können eine gespiegelte Datenbank während einer Spiegelungssitzung überwachen, um zu überprüfen, ob und auf welche Weise ein Datenfluss stattfindet.  
  
 Die Überwachung einer gespiegelten Datenbank ermöglicht Ihnen insbesondere Folgendes:  
  
-   Überprüfen, ob die Spiegelung funktionsfähig ist.  
  
     Aus dem Basisstatus erkennen Sie, ob die zwei Serverinstanzen in Betrieb sind, ob zwischen den Servern eine Verbindung besteht und ob das Protokoll vom Prinzipal auf den Spiegel verschoben wird.  
  
-   Bestimmen, ob die Spiegeldatenbank mit der Prinzipaldatenbank Schritt hält.  
  
     Im Modus für hohe Leistung kann auf einem Prinzipalserver ein Rückstand von nicht gesendeten Protokolldatensätzen entstehen, die vom Prinzipalserver noch an den Spiegelserver gesendet werden müssen. In jedem Betriebsmodus kann zudem auf dem Spiegelserver ein Rückstand an noch nicht wiederhergestellten Protokolldatensätzen entstehen, die zwar bereits in die Protokolldatei geschrieben wurden, aber auf der Spiegeldatenbank noch wiederhergestellt werden müssen.  
  
-   Das Ausmaß an Datenverlusten bestimmen, wenn die Prinzipalserverinstanz während des Modus für hohe Leistung unverfügbar wird.  
  
     Sie können den Datenverlust anhand des evtl. noch nicht gesendeten Transaktionsprotokollanteils und des Zeitintervalls ermitteln, in dem für die verlorenen Transaktionen auf dem Prinzipal ein Commit ausgeführt wurde.  
  
-   Vergleichen der aktuellen Leistung mit der vergangenen Leistung.  
  
     Bei Auftreten von Problemen kann ein Datenbankadministrator einen Verlauf der Spiegelungsleistung anzeigen lassen und dadurch u. U. die Ursache des aktuellen Status feststellen. Ein Blick auf die Verlaufsdaten kann es dem Benutzer ermöglichen, Trends im Leistungsverhalten zu erkennen, Muster von Leistungsproblemen zu identifizieren (z. B. zu welchen Tageszeiten das Netzwerk langsam ist oder zu welchen Zeiten eine große Anzahl von Befehlen in das Protokoll geschrieben werden).  
  
-   Feststellen der Ursache eines reduzierten Datenflusses zwischen Spiegelungspartnern.  
  
-   Festlegen von Warnungsschwellenwerten für Schlüsselleistungsmetriken.  
  
     Wenn eine neue Statuszeile einen Wert enthält, der einen Schwellenwert überschreitet, wird ein Informationsereignis an das Windows-Ereignisprotokoll gesendet. Ein Systemadministrator kann dann manuell Warnmeldungen basierend auf diesen Ereignissen konfigurieren. Weitere Informationen finden Sie unter [Verwenden von Warnungsschwellenwerten und Warnmeldungen für Spiegelungsleistungsmetriken &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
###  <a name="tools-for-monitoring-database-mirroring-status"></a><a name="tools_for_monitoring_dbm_status"></a> Tools zum Überwachen des Datenbank-Spiegelungsstatus  
 Der Status der Spiegelung kann entweder mithilfe des Datenbanküberwachungs-Monitors oder der gespeicherten Systemprozedur **sp_dbmmonitorresults** überwacht werden. Mit diesen Tools können sowohl Systemadministratoren, d.h., Mitglieder der festen Serverrolle **sysadmin** , als auch Benutzer, die von einem Systemadministrator der festen Datenbankrolle **dbm_monitor** in der **msdb** -Datenbank hinzugefügt wurden, die Datenbankspiegelung für jede gespiegelte Datenbank auf der lokalen Serverinstanz überwachen. Bei beiden Tools kann ein Systemadministrator den Spiegelungsstatus auch manuell aktualisieren.  
  
> [!NOTE]  
>  Systemadministratoren können auch Warnungsschwellenwerte für wichtige Leistungsmetriken konfigurieren und anzeigen. Weitere Informationen finden Sie unter [Verwenden von Warnungsschwellenwerten und Warnmeldungen für Spiegelungsleistungsmetriken &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
-   Datenbankspiegelungs-Monitor  
  
     Der Datenbankspiegelungs-Monitor verfügt über eine grafische Benutzeroberfläche, mit der Systemadministratoren den Status anzeigen und aktualisieren können und für mehrere Schlüsselleistungsmetriken Warnungsschwellenwerte konfigurieren können. Der Datenbankspiegelungs-Monitor kann auch von Mitgliedern der festen Datenbankrolle **dbm_monitor** verwendet werden, um die jeweils neueste Zeile der Spiegelungsstatustabelle anzuzeigen. Die Statustabelle kann von diesen Benutzern jedoch nicht aktualisiert werden.  
  
     Auf dem Monitor wird der Status einschließlich der Leistungsmetrik für eine ausgewählte Datenbank auf der Seite **Status** im Registerformat angezeigt. Der Inhalt dieser Seite stammt sowohl von der Prinzipal- als auch von der Spiegelserverinstanz. Die Seite wird mit den Statusinformationen, die über jeweils separate Verbindungen zur Prinzipalserver- und Spiegelserverinstanz gesammelt werden, asynchron gefüllt. Der Monitor versucht, die Statustabelle alle 30 Sekunden zu aktualisieren. Das Update ist nur dann erfolgreich, wenn die Tabelle nicht innerhalb von 15 Sekunden aktualisiert worden ist und der Benutzer ein Mitglied der festen Serverrolle **sysadmin** ist. Eine Zusammenfassung der Informationen, die auf der Seite **Status** angegeben werden, finden Sie unter [Auf dem Datenbankspiegelungs-Monitor angezeigter Status](#perf_metrics_of_dbm_monitor)weiter unten in diesem Thema.  
  
     Eine Einführung zur Benutzeroberfläche des Datenbankspiegelungs-Monitors finden Sie unter [Database Mirroring Monitor Overview](../../database-engine/database-mirroring/database-mirroring-monitor-overview.md). Informationen zum Starten des Datenbankspiegelungs-Monitors finden Sie unter [Starten des Datenbankspiegelungs-Monitors &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md).  
  
-   Gespeicherte Systemprozeduren  
  
     Sie können den aktuellen Status auch abrufen oder aktualisieren, indem Sie die gespeicherte Systemprozedur **sp_dbmmonitorresults** ausführen. Andere gespeicherte Prozeduren von dbmmonitor ermöglichen es Ihnen, die Überwachung einzurichten, Überwachungsparameter zu ändern, den aktuellen Updatezeitraum anzuzeigen sowie die Überwachung auf der Serverinstanz zu entfernen.  
  
     In der folgenden Tabelle werden die gespeicherten Prozeduren vorgestellt, mit denen die Datenbankspiegelungsüberwachung unabhängig vom Datenbankspiegelungs-Monitor verwaltet und verwendet werden kann.  
  
    |Verfahren|BESCHREIBUNG|  
    |---------------|-----------------|  
    |[sp_dbmmonitoraddmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md)|Erstellt einen Auftrag, mit dem in regelmäßigen Abständen die Statusinformationen für jede gespiegelte Datenbank auf der Serverinstanz aktualisiert werden.|  
    |[sp_dbmmonitorchangemonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)|Ändert den Wert eines Parameters für die Datenbank-Spiegelungsüberwachung.|  
    |[sp_dbmmonitorhelpmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)|Gibt den aktuellen Updatezeitraum zurück.|  
    |[sp_dbmmonitorresults](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)|Gibt Statuszeilen für eine überwachte Datenbank zurück. Außerdem können Sie damit festlegen, ob die Prozedur den neuesten Status vorzeitig abrufen soll.|  
    |[sp_dbmmonitordropmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)|Beendet und löscht den Auftrag für den Datenbankspiegelungs-Monitor für alle Datenbanken auf der Serverinstanz.|  
  
     Die gespeicherten Systemprozeduren von **dbmmonitor** können als Zusatz zum Datenbankspiegelungs-Monitor verwendet werden So kann z.B. der Status auch dann mit dem Datenbankspiegelungs-Monitor angezeigt werden, wenn die Überwachung mit **sp_dbmmonitoraddmonitoring** konfiguriert wurde.  
  
### <a name="how-monitoring-works"></a>Funktionsweise der Überwachung  
 Dieser Abschnitt enthält Erläuterungen zur Datenbank-Spiegelungsstatustabelle, zum Auftrag für den Datenbankspiegelungs-Monitor sowie zum Datenbankspiegelungs-Monitor. Sie erfahren außerdem, wie Benutzer den Status der Datenbankspiegelung überwachen können und wie der Überwachungsauftrag gelöscht werden kann.  
  
#### <a name="database-mirroring-status-table"></a>Datenbank-Spiegelungsstatustabelle  
 Der Datenbank-Spiegelungsstatus wird in einer internen, nicht dokumentierten Datenbank-Spiegelungsstatustabelle in der **msdb** -Datenbank gespeichert. Diese Statustabelle wird automatisch erstellt, wenn der Spiegelungsstatus zum ersten Mal auf der Serverinstanz aktualisiert wird.  
  
 Die Statustabelle kann entweder automatisch aktualisiert werden oder manuell von einem Systemadministrator unter Einhaltung eines Mindestupdateintervalls von 15 Sekunden. Durch das Minimum von 15 Sekunden wird verhindert, dass Serverinstanzen mit Statusanforderungen überlastet werden.  
  
 Die Statustabelle wird automatisch sowohl vom Datenbankspiegelungs-Monitor als auch vom Auftrag für den Datenbankspiegelungs-Monitor aktualisiert, sofern dieser ausgeführt wird. Der **Auftrag für den Datenbankspiegelungs-Monitor** aktualisiert die Tabelle standardmäßig einmal pro Minute (ein Systemadministrator kann einen Updatezeitraum zwischen 1 und 120 Minuten angeben). Der Datenbankspiegelungs-Monitor hingegen aktualisiert die Tabelle automatisch alle 30 Sekunden. Für diese Updates rufen der **Auftrag für den Datenbankspiegelungs-Monitor** und der Datenbankspiegelungs-Monitor die Prozedur **sp_dbmmonitorupdate** auf.  
  
 Bei der ersten Ausführung von **sp_dbmmonitorupdate** werden die Tabelle des **Datenbankspiegelungsstatus** und die feste Datenbankrolle **dbm_monitor** in der **msdb** -Datenbank erstellt. Mit **sp_dbmmonitorupdate** wird der Spiegelungsstatus in der Regel durch Einfügen einer neuen Zeile in die Statustabelle für jede gespiegelte Datenbank auf der Serverinstanz aktualisiert. Weitere Informationen hierzu finden Sie unter „Datenbank-Spiegelungsstatustabelle“ weiter unten in diesem Thema. Diese Prozedur wertet außerdem die Leistungsmetrik in den neuen Zeilen aus und entfernt Zeilen, die älter sind als die aktuelle Beibehaltungsdauer (die Standardeinstellung ist 7 Tage). Weitere Informationen finden Sie unter [sp_dbmmonitorupdate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md).  
  
> [!NOTE]  
>  Sofern der Datenbankspiegelungs-Monitor nicht aktuell von einem Mitglied der festen Serverrolle **sysadmin** verwendet wird, erfolgt eine automatische Aktualisierung der Statustabelle nur dann, wenn der **Auftrag für den Datenbankspiegelungs-Monitor** vorhanden ist und der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ausgeführt wird.  
  
#### <a name="database-mirroring-monitor-job"></a>Auftrag für den Datenbankspiegelungs-Monitor  
 Der als **Auftrag für den Datenbankspiegelungs-Monitor** bezeichnete Auftrag für die Überwachung der Datenbankspiegelung arbeitet unabhängig vom Datenbankspiegelungs-Monitor. Der **Auftrag für den Datenbankspiegelungs-Monitor** wird nur dann automatisch erstellt, wenn eine Spiegelungssitzung mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] gestartet wird. Wenn zum Starten der Spiegelung immer die ALTER DATABASE *Datenbankname* SET PARTNER-Befehle verwendet werden, ist der Auftrag nur dann vorhanden, wenn der Systemadministrator die gespeicherte Prozedur **sp_dbmmonitoraddmonitoring** ausführt.  
  
 Nachdem der **Auftrag für den Datenbankspiegelungs-Monitor** erstellt wurde und vorausgesetzt, dass der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ausgeführt wird, wird der Auftrag standardmäßig einmal pro Minute aufgerufen. Der Auftrag ruft dann die gespeicherte Systemprozedur **sp_dbmmonitorupdate** auf.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ruft den **Auftrag für den Datenbankspiegelungs-Monitor** standardmäßig einmal pro Minute auf, und der Auftrag ruft seinerseits **sp_dbmmonitorupdate** auf, um die Statustabelle zu aktualisieren. Systemadministratoren können den Updatezeitraum mithilfe der gespeicherten Systemprozedur **sp_dbmmonitorchangemonitoring** aktualisieren. Sie können außerdem mit der gespeicherten Systemprozedur **sp_dbmmonitorchangemonitoring** den aktuellen Updatezeitraum anzeigen lassen. Weitere Informationen finden Sie unter [sp_dbmmonitoraddmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md) und [sp_dbmmonitorchangemonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md).  
  
#### <a name="monitoring-database-mirroring-status-by-system-administrators"></a>Überwachen des Datenbank-Spiegelungsstatus (durch Systemadministratoren)  
 Mitglieder der festen Serverrolle **sysadmin** können die Statustabelle anzeigen und aktualisieren.  
  
-   Verwenden des Datenbankspiegelungs-Monitors  
  
     Mit dem Datenbankspiegelungs-Monitor kann ein Systemadministrator manuell die Seite **Status** , die Navigationsstruktur oder die Seite **Verlauf** aktualisieren. Dabei wird auch die Statustabelle aktualisiert, sofern sie nicht innerhalb der vorhergehenden 15 Sekunden bereits aktualisiert wurde.  
  
     Der Systemadministrator kann auch über die Schaltfläche **Verlauf** für eine Serverinstanz (auf der Seite **Status** ) den Verlauf des Spiegelungsstatus auf einer bestimmten Serverinstanz anzeigen. Der Verlauf wird im Dialogfeld **Datenbankspiegelungsverlauf** angezeigt. Hier kann der Systemadministrator alle oder auch nur einen Teil der Zeilen in der Statustabelle der Serverinstanz einsehen.  
  
     Informationen zu den Metriken auf der Seite **Status** finden Sie unter "Vom Datenbankspiegelungs-Monitor angezeigte Leistungsmetriken" weiter unten in diesem Thema.  
  
-   Verwenden von **sp_dbmmonitorresults**  
  
     Systemadministratoren können mit der gespeicherten Systemprozedur **sp_dbmmonitorresults** die Statustabelle anzeigen und bei Bedarf aktualisieren, sofern sie nicht in den vorhergehenden 15 Sekunden aktualisiert wurde. Diese Prozedur ruft die **sp_dbmmonitorupdate** -Prozedur auf und gibt, abhängig von der im Prozeduraufruf angeforderten Menge, eine oder mehrere Verlaufszeilen zurück. Informationen zu dem in den Resultsets angezeigten Status finden Sie unter [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md).  
  
#### <a name="monitoring-database-mirroring-status-by-dbm_monitor-members"></a>Überwachen des Datenbankspiegelungs-Status (durch dbm_monitor Members)  
 Wie bereits erwähnt, wird beim ersten Ausführen von **sp_dbmmonitorupdate** die feste Datenbankrolle **dbm_monitor** in der **msdb** -Datenbank erstellt. Mitglieder der festen Datenbankrolle **dbm_monitor** können den vorhandenen Spiegelungsstatus entweder mithilfe des Datenbankspiegelungs-Monitors oder der gespeicherten Prozedur **sp_dbmmonitorresults** anzeigen. Diese Benutzer können jedoch nicht die Statustabelle aktualisieren. Wie alt der angezeigte Status ist, geht auf der Seite **Status** aus den in den Bezeichnungen **Prinzipalprotokoll (** _\<time>_ **)** und **Spiegelungsprotokoll (** _\<time>_ **)** angezeigten Uhrzeiten hervor.  
  
 Für Mitglieder der festen Datenbankrolle **dbm_monitor** wird die Statustabelle über den **Auftrag für den Datenbankspiegelungs-Monitor** in regelmäßigen Intervallen aktualisiert. Wenn der Auftrag nicht vorhanden ist oder der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent beendet wird, veraltet der Status zunehmend und gibt möglicherweise nicht mehr die Konfiguration der Spiegelungssitzung wieder. So kann z. B. nach einem Failover fälschlicherweise angezeigt werden, dass die Partner dieselbe Rolle haben (Prinzipal oder Spiegel), oder der aktuelle Prinzipalserver wird als Spiegel angezeigt, während der aktuelle Spiegelserver als Prinzipal angezeigt wird.  
  
#### <a name="dropping-the-database-mirroring-monitor-job"></a>Löschen des Auftrags für den Datenbankspiegelungs-Monitor  
 Der als **Auftrag für den Datenbankspiegelungs-Monitor** bezeichnete Auftrag für die Überwachung der Datenbankspiegelung bleibt so lange bestehen, bis er gelöscht wird. Der Überwachungsauftrag muss vom Systemadministrator verwaltet werden. Der **Auftrag für den Datenbankspiegelungs-Monitor** kann mit **sp_dbmmonitordropmonitoring** gelöscht werden. Weitere Informationen finden Sie unter [sp_dbmmonitordropmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md).  
  
###  <a name="status-displayed-by-the-database-mirroring-monitor"></a><a name="perf_metrics_of_dbm_monitor"></a> Auf dem Datenbankspiegelungs-Monitor angezeigter Status  
 Auf der Seite **Status** des Datenbankspiegelungs-Monitors werden die Partner beschrieben sowie der Status der Spiegelungssitzung. Der Status umfasst Werte der Leistungsmetrik, z. B. den Zustand des Transaktionsprotokolls und weitere Informationen, mit denen gegenwärtig die Zeit zum Abschließen eines Failovers und die Gefahr des Datenverlusts abgeschätzt werden kann, wenn die Sitzung nicht synchronisiert wird. Zusätzlich enthält die Seite **Status** Angaben zum Status der Spiegelungssitzung sowie allgemeine Sitzungsinformationen.  
  
> [!NOTE]  
>  Eine Einführung zum Datenbankspiegelungs-Monitor und zur Seite **Status** finden Sie unter [Tools zum Überwachen des Datenbank-Spiegelungsstatus](#tools_for_monitoring_dbm_status)weiter oben in diesem Thema.  
  
 Die jeweiligen bereitgestellten Informationen werden in den folgenden Abschnitten zusammengefasst.  
  
#### <a name="partners"></a>Partner  
 Auf der Seite **Status** werden für jeden der Partner die folgenden Informationen angezeigt:  
  
-   Serverinstanz  
  
     Name der Serverinstanz, deren Status in der Zeile **Status** angezeigt wird.  
  
-   Aktuelle Rolle  
  
     Aktuelle Rolle der Serverinstanz. Mögliche Zustandswerte:  
  
    -   Prinzipal  
  
    -   Spiegel  
  
-   Spiegelungsstatus  
  
     Mögliche Zustandswerte:  
  
    -   Unknown  
  
    -   Wird synchronisiert  
  
    -   Synchronisiert  
  
    -   Ausgesetzt  
  
    -   Getrennt  
  
-   Zeugenverbindung  
  
     Verbindungsstatus des Zeugen. Mögliche Zustandswerte:  
  
    -   Unknown  
  
    -   Verbunden  
  
    -   Getrennt  
  
#### <a name="log-on-the-principal-server"></a>Protokoll auf dem Prinzipalserver  
 Auf der Seite **Status** werden die folgenden Informationen zum Status des Protokolls auf dem Prinzipalserver entsprechend der angegebenen Zeit angezeigt:  
  
-   Nicht gesendetes Protokoll  
  
     Menge der in der Sendewarteschlange wartenden Protokolldaten (in KB).  
  
-   Älteste, nicht gesendete Transaktion  
  
     Alter der ältesten, nicht gesendeten Transaktion in der Sendewarteschlange. Das Alter dieser Transaktion gibt an, wie viele Minuten an Transaktionen noch nicht an die Spiegelserverinstanz gesendet wurden. Dieser Wert hilft, das Datenverlustrisiko in Bezug auf die Zeit zu messen.  
  
-   Zeit zum Senden des Protokolls (geschätzt)  
  
     Die geschätzte Anzahl von Minuten, die die Prinzipalserverinstanz benötigt, um das Protokoll, das sich derzeit in der Sendewarteschlange befindet, basierend auf der aktuellen Senderate an die Spiegelserverinstanz zu senden. Die zum Senden des Protokolls tatsächlich erforderliche Zeit wird von der Rate der eintreffenden Transaktionen beeinflusst, die erheblichen Schwankungen unterworfen sein kann. Anhand des Werts **Zeit zum Senden des Protokolls (geschätzt)** kann jedoch die für ein manuelles Failover erforderliche Zeit grob geschätzt werden.  
  
-   Aktuelle Senderate  
  
     Rate, mit der Transaktionen an die Spiegelserverinstanz gesendet werden (in KB pro Sekunde).  
  
-   Aktuelle Rate neuer Transaktionen  
  
     Rate, mit der eingehende Transaktionen in das Protokoll des Prinzipals eingetragen werden (in KB pro Sekunde). Sie können feststellen, ob die Spiegelung zurück liegt, gleich schnell ist oder aufholt, indem Sie diesen Wert mit dem Wert von **Zeit zum Senden des Protokolls (geschätzt)** vergleichen.  
  
#### <a name="log-on-the-mirror-server"></a>Protokoll auf dem Spiegelserver  
 Auf der Seite **Status** werden die folgenden Informationen zum Status des Protokolls auf dem Spiegelserver entsprechend der angegebenen Zeit angezeigt:  
  
-   Nicht wiederhergestelltes Protokoll  
  
     Menge der in der Wiederholungswarteschlange wartenden Protokolldaten (in KB).  
  
-   Zeit zum Wiederherstellen des Protokolls (geschätzt)  
  
     Die geschätzte Anzahl von Minuten, die erforderlich ist, um das Protokoll, das sich derzeit in der Wiederholungswarteschlange befindet, auf die Spiegeldatenbank anzuwenden.  
  
-   Aktuelle Wiederherstellungsrate  
  
     Rate, mit der Transaktionen in der Spiegelserverinstanz wiederhergestellt werden (in KB pro Sekunde).  
  
#### <a name="mirroring-session"></a>Spiegelungssitzung  
 Zusätzlich werden auf der Seite **Status** die folgenden Informationen zur Spiegelungssitzung angezeigt:  
  
-   Spiegelungscommitaufwand  
  
     Durchschnittliche Verzögerung pro Transaktion in Millisekunden (nur im Modus für hohe Sicherheit relevant). Hierbei handelt es sich um die Verzögerung, die entsteht, während die Prinzipalserverinstanz darauf wartet, dass die Spiegelserverinstanz den Transaktionsprotokolldatensatz in die Wiederholungswarteschlange schreibt.  
  
-   Zeit zum Senden und Wiederherstellen aller aktuellen Protokolle (geschätzt)  
  
     Geschätzte Zeit, die erforderlich ist, um das gesamte, noch nicht gesendete Protokoll, für das auf dem Prinzipal ein Commit ausgeführt wurde, zu senden, und um das gesamte Protokoll wiederherzustellen, das sich derzeit in der Wiederholungswarteschlange befindet. Der Schätzung ist möglicherweise niedriger als die Summe der Werte in den Feldern **Zeit zum Senden des Protokolls (geschätzt)** und **Zeit zum Wiederherstellen des Protokolls (geschätzt)** , da Sende- und Wiederherstellungsvorgänge parallel ausgeführt werden können.  
  
-   Zeugenadresse  
  
     Netzwerkadresse der Zeugenserverinstanz. Informationen zum Format dieser Adresse finden Sie unter [Angeben einer Servernetzwerkadresse &#40;Datenbankspiegelung&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
-   Betriebsmodus  
  
     Der Betriebsmodus der Datenbank-Spiegelungssitzung:  
  
    -   Hohe Leistung (asynchron)  
  
    -   Hohe Sicherheit ohne automatisches Failover (synchron)  
  
    -   Hohe Sicherheit mit automatischem Failover (synchron)  
  
##  <a name="additional-sources-of-information-about-a-mirrored-database"></a><a name="AdditionalSources"></a> Zusätzliche Informationsquellen zu gespiegelten Datenbanken  
 Zusätzlich zur Verwendung des Datenbankspiegelungs-Monitors und dbmmonitor-basierter gespeicherter Prozeduren zur Überwachung einer gespiegelten Datenbank und zum Einrichten von Warnungen für überwachte Leistungsvariablen bietet [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Katalogsichten, Leistungsindikatoren und Ereignisbenachrichtigungen für die Datenbankspiegelung.  
  
 **In diesem Abschnitt:**  
  
-   [Metadaten für die Datenbankspiegelung](#DbmMetadata)  
  
-   [Leistungsindikatoren für die Datenbankspiegelung](#DbmPerfCounters)  
  
-   [Ereignisbenachrichtigungen für die Datenbankspiegelung](#DbmEventNotif)  
  
###  <a name="database-mirroring-metadata"></a><a name="DbmMetadata"></a> Metadaten für die Datenbankspiegelung  
 Jede Datenbank-Spiegelungssitzung wird in Metadaten beschrieben, die mithilfe der folgenden Katalogsichten oder dynamischen Verwaltungssichten offengelegt werden:  
  
-   **sys.database_mirroring**  
  
     Diese Sicht zeigt die Metadaten für die Datenbankspiegelung für jede gespiegelte Datenbank in einer Serverinstanz an. Weitere Informationen finden Sie unter [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
-   **sys.database_mirroring_endpoints**  
  
     Die **sys.database_mirroring_endpoints** -Katalogsicht zeigt Informationen zum Endpunkt der Datenbankspiegelung der Serverinstanz an. Weitere Informationen finden Sie unter [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
-   **sys.database_mirroring_witnesses**  
  
     Diese Katalogsicht zeigt die Metadaten für die Datenbankspiegelung für jede Sitzung an, bei der eine Serverinstanz Zeuge ist. Weitere Informationen finden Sie unter [sys.database_mirroring_witnesses &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md).  
  
-   **sys.dm_db_mirroring_connections**  
  
     Diese dynamische Verwaltungssicht gibt für jede Datenbankspiegelungs-Netzwerkverbindung eine Zeile zurück.  
  
     Weitere Informationen finden Sie unter [sys.dm_db_mirroring_connections &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md).  
  
###  <a name="database-mirroring-performance-counters"></a><a name="DbmPerfCounters"></a> Leistungsindikatoren für die Datenbankspiegelung  
 Mit Leistungsindikatoren können Sie die Leistung der Datenbankspiegelung überwachen. Beispielsweise können Sie den Indikator **Transaktionsverzögerung** untersuchen, um festzustellen, ob die Datenbankspiegelung Auswirkungen auf die Leistung des Prinzipalservers hat, oder Sie können die Indikatoren **Wiederholungswarteschlange** und **Protokollsende-Warteschlange** untersuchen, um festzustellen, wie gut die Spiegeldatenbank mit der Prinzipaldatenbank Schritt halten kann. Mit dem Leistungsindikator **Gesendete Protokollbytes/Sekunde** können Sie überwachen, wie viele Protokollbytes pro Sekunde gesendet wurden.  
  
 In den Systemmonitoren auf beiden Partnern sind Leistungsindikatoren im Datenbankspiegelungs-Leistungsobjekt verfügbar (**SQLServer:Datenbankspiegelung**). Weitere Informationen finden Sie unter [SQL Server, Database Mirroring Object](../../relational-databases/performance-monitor/sql-server-database-mirroring-object.md).  
  
 **So starten Sie den Systemmonitor**  
  
-   [Starten des Systemmonitors &#40;Windows&#41;](../../relational-databases/performance/start-system-monitor-windows.md)  
  
###  <a name="database-mirroring-event-notifications"></a><a name="DbmEventNotif"></a> Ereignisbenachrichtigungen für die Datenbankspiegelung  
 Bei einer Ereignisbenachrichtigung handelt es sich um eine spezielle Art von Datenbankobjekt. Ereignisbenachrichtigungen werden als Antwort auf eine Vielzahl von Transact-SQL DDL-Anweisungen (Data Definition Language) und Ereignissen der SQL-Ablaufverfolgung ausgeführt; sie senden Informationen zu Server- und Datenbankereignissen an einen [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Dienst.  
  
 Die folgenden Ereignisse stehen für die Datenbankspiegelung zur Verfügung:  
  
-   **Database Mirroring State Change** -Ereignisklasse  
  
     Sie zeigt an, wenn sich der Spiegelungsstatus einer gespiegelten Datenbank ändert. Weitere Informationen finden Sie unter [Database Mirroring State Change Event Class](../../relational-databases/event-classes/database-mirroring-state-change-event-class.md).  
  
-   **Audit Database Mirroring Login** -Ereignisklasse  
  
     Sie meldet Überwachungsmeldungen, die mit der Transportsicherheit bei der Datenbankspiegelung verbunden sind. Weitere Informationen finden Sie unter [Audit Database Mirroring Login Event Class](../../relational-databases/event-classes/audit-database-mirroring-login-event-class.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Verwenden von Warnungsschwellenwerten und Warnmeldungen für Spiegelungsleistungsmetriken &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md)  
  
-   [Starten des Datenbankspiegelungs-Monitors &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
-   [Anzeigen des Status einer gespiegelten Datenbank &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/view-the-state-of-a-mirrored-database-sql-server-management-studio.md)  
  
 **Gespeicherten Prozeduren**  
  
-   [sp_dbmmonitoraddmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorchangealert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)  
  
-   [sp_dbmmonitorchangemonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)  
  
-   [sp_dbmmonitordropalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)  
  
-   [sp_dbmmonitordropmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorhelpalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)  
  
-   [sp_dbmmonitorhelpmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
-   [sp_dbmmonitorupdate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Konzepte des WMI-Anbieters für Serverereignisse](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md)  
  
  
