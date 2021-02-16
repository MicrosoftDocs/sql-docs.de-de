---
title: Konfigurieren von Warnungen für Leistungsmetriken der Datenbankspiegelung
description: 'Leitfaden zum Konfigurieren von Warnungsschwellenwerten und Warnungen für von der Datenbankspiegelung verwendeten Leistungsmetriken '
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- monitoring database mirroring [SQL Server]
- thresholds [SQL Server]
- database mirroring [SQL Server], managing in SQL Server Management Studio
- alerts [SQL Server], database mirroring
- database mirroring [SQL Server], monitoring
- warnings [database mirroring]
ms.assetid: 8cdd1515-0bd7-4f8c-a7fc-a33b575e20f6
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 7b7b014ffdd1377dcda2bfd0c72e4b91b6116a71
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352187"
---
# <a name="use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server"></a>Verwenden von Warnungsschwellenwerten und Warnmeldungen für Spiegelungsleistungsmetriken (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Dieses Thema enthält Informationen über [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Ereignisse, für die Schwellenwerte für Warnungen konfiguriert und die Datenbankspiegelung verwaltet werden können. Sie können den Datenbankspiegelungs-Monitor oder die gespeicherten Prozeduren **sp_dbmmonitorchangealert**, **sp_dbmmonitorhelpalert** und **sp_dbmmonitordropalert** verwenden. Dieses Thema enthält darüber hinaus Informationen über das Konfigurieren von Warnungen für Datenbank-Spiegelungsereignisse.  
  
 Nachdem für eine gespiegelte Datenbank die Überwachung eingerichtet wurde, können vom Systemadministrator für mehrere Schlüsselleistungsmetriken Warnungsschwellenwerte konfiguriert werden. Administratoren können für diese Metriken und andere Datenbank-Spiegelungsereignisse auch Warnungsmeldungen konfigurieren.  
  
  
##  <a name="performance-metrics-and-warning-thresholds"></a><a name="PerfMetricsAndWarningThresholds"></a> Leistungsmetriken und Warnungsschwellenwerte  
 In der folgenden Tabelle werden die Leistungsmetriken, für die Warnungen konfiguriert werden können, zusammen mit den entsprechenden Warnungsschwellenwerten und der entsprechende Bezeichnung des Datenbanküberwachungs-Monitors aufgelistet.  
  
|Leistungsmetrik|Schwellenwert für Warnung|Bezeichnung des Datenbankspiegelungs-Monitors|  
|------------------------|-----------------------|--------------------------------------|  
|Nicht gesendetes Protokoll|Gibt an, bei welcher Menge (in KB) an nicht gesendeten Protokolldaten eine Warnung auf der Prinzipalserverinstanz generiert wird. Anhand dieser Warnung, die speziell für den Modus für hohe Leistung relevant ist, kann der potenzielle Datenverlust in KB gemessen werden. Die Warnung ist aber auch für den Modus für hohe Sicherheit relevant, wenn die Spiegelung angehalten oder unterbrochen wird, weil die Verbindung zwischen den Partnern getrennt wurde.|**Warnhinweis anzeigen, wenn das nicht gesendete Protokoll den Schwellenwert überschreitet.**|  
|Nicht wiederhergestelltes Protokoll|Gibt an, bei welcher Menge (in KB) an nicht wiederhergestellten Protokolldaten eine Warnung auf der Spiegelserverinstanz generiert wird. Diese Warnung ermöglicht die Messung der Failoverzeit. Die *Failoverzeit* besteht hauptsächlich aus der Zeit, die der frühere Spiegelserver benötigt, um ein Rollforward für die Protokolldaten auszuführen, die sich noch in seiner Wiederholungswarteschlange befinden, sowie einer zusätzlichen kurzen Zeitspanne.<br /><br /> Hinweis: Bei einem automatischen Failover hängt die Zeitspanne, die es dauert, bis das System den Fehler bemerkt, nicht von der Failoverzeit ab.<br /><br /> Weitere Informationen finden Sie unter [Einschätzen der Unterbrechung des Diensts während des Rollenwechsels &#40;Datenbankspiegelung&#41;](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md)ausgetauscht werden.|**Warnhinweis anzeigen, wenn das nicht wiederhergestellte Protokoll den Schwellenwert überschreitet.**|  
|Älteste, nicht gesendete Transaktion|Gibt die Menge an Transaktionen (in Anzahl Minuten) an, die sich in der Sendewarteschlange ansammeln dürfen, bevor auf der Prinzipalserverinstanz eine Warnung generiert wird. Anhand dieser Warnung, die speziell für den Modus für hohe Leistung relevant ist, kann der potenzielle Datenverlust im Hinblick auf die Zeit gemessen werden. Die Warnung ist aber auch für den Modus für hohe Sicherheit relevant, wenn die Spiegelung angehalten oder unterbrochen wird, weil die Verbindung zwischen den Partnern getrennt wurde.|**Warnhinweis anzeigen, wenn das Alter der ältesten, nicht gesendeten Transaktion den Schwellenwert überschreitet.**|  
|Spiegelungscommitaufwand|Gibt die durchschnittliche Verzögerung (in Anzahl der Millisekunden) pro Transaktion an, die toleriert wird, bevor auf dem Prinzipalserver eine Warnung generiert wird. Hierbei handelt es sich um die Verzögerung, die entsteht, während die Prinzipalserverinstanz darauf wartet, dass die Spiegelserverinstanz den Transaktionsprotokolldatensatz in die Wiederholungswarteschlange schreibt. Dieser Wert ist nur im Modus für hohe Sicherheit relevant.|**Warnhinweis anzeigen, wenn der Spiegelungscommitaufwand den Schwellenwert überschreitet.**|  
  
 Für jede dieser Leistungsmetrik kann vom Systemadministrator ein Schwellenwert für eine gespiegelte Datenbank angegeben werden. Weitere Informationen finden Sie unter [Einrichten und Verwalten von Schwellenwerten für Warnungen](#SetUpManageWarningThresholds)weiter unten in diesem Thema.  
  
##  <a name="setting-up-and-managing-warning-thresholds"></a><a name="SetUpManageWarningThresholds"></a> Einrichten und Verwalten von Schwellenwerten für Warnungen  
 Ein Systemadministrator kann einen oder mehrere Warnungsschwellenwerte für die wichtigsten Leistungsmetriken für die Spiegelung konfigurieren. Es wird empfohlen, einen Schwellenwert für eine bestimmte Warnung jeweils auf beiden Partnern festzulegen, um sicherzustellen, dass bei einem Failover der Datenbank die Warnung beibehalten wird. Der geeignete Schwellenwert für jeden der Partner hängt von den Leistungsmöglichkeiten des betreffenden Partnersystems ab.  
  
 Warnungsschwellenwerte können mit einem der folgenden Tools konfiguriert und verwaltet werden:  
  
-   Datenbankspiegelungs-Monitor  
  
     Auf der Registerkarte **Warnungen** des Datenbankspiegelungs-Monitors kann der Administrator die aktuelle Konfiguration von Warnungen für eine ausgewählte Datenbank gleichzeitig für die Prinzipal- und die Spiegelserverinstanz anzeigen. Im Dialogfeld **Schwellenwerte für Warnungen festlegen** , das von dieser Registerkarte aus geöffnet werden kann, können dann Schwellenwerte für Warnungen aktiviert und konfiguriert werden.  
  
     Eine Einführung zur Benutzeroberfläche des Datenbankspiegelungs-Monitors finden Sie unter [Database Mirroring Monitor Overview](../../database-engine/database-mirroring/database-mirroring-monitor-overview.md). Informationen zum Starten des Datenbankspiegelungs-Monitors finden Sie unter [Starten des Datenbankspiegelungs-Monitors &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md).  
  
-   Gespeicherte Systemprozeduren  
  
     Mit den folgenden gespeicherten Systemprozeduren kann ein Administrator Warnungsschwellenwerte für die gespiegelten Datenbanken jeweils für einen Partner festlegen.  
  
    |Verfahren|BESCHREIBUNG|  
    |---------------|-----------------|  
    |[sp_dbmmonitorchangealert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)|Mit dieser Prozedur können Warnungsschwellenwerte für eine bestimmte Spiegelungsleistungsmetrik hinzugefügt oder geändert werden.|  
    |[sp_dbmmonitorhelpalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)|Gibt Informationen zu Warnungsschwellenwerten zurück, die für eine oder mehrere der Schlüsselleistungsmetriken für die Überwachung der Datenbankspiegelung festgelegt wurden.|  
    |[sp_dbmmonitordropalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)|Entfernt die Warnung für eine angegebene Leistungsmetrik.|  
  
## <a name="performance-threshold-events-sent-to-the-windows-event-log"></a>Leistungsschwellenwert-Ereignisse, die an das Windows-Ereignisprotokoll gesendet werden  
 Wenn für eine Leistungsmetrik ein Warnungsschwellenwert definiert wurde, wird beim Aktualisieren der Statustabelle der neueste Wert im Vergleich zum Schwellenwert ausgewertet. Wenn der Schwellenwert erreicht wurde, generiert die Updateprozedur, **sp_dbmmonitorupdate**, ein Informationsereignis, ein so genanntes *Leistungsschwellenwert-Ereignis*, für die Metrik und schreibt das Ereignis in das [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Ereignisprotokoll. In der folgenden Tabelle werden die Ereignis-IDs der Leistungsschwellenwert-Ereignisse aufgelistet.  
  
|Leistungsmetrik|Ereignis-ID|  
|------------------------|--------------|  
|Nicht gesendetes Protokoll|32042|  
|Nicht wiederhergestelltes Protokoll|32043|  
|Älteste, nicht gesendete Transaktion|32040|  
|Spiegelungscommitaufwand|32044|  
  
> [!NOTE]  
>  Ein Administrator kann Warnmeldungen für jedes dieser Ereignisse definieren. Weitere Informationen hierzu finden Sie unter [Verwenden von Warnmeldungen für eine gespiegelte Datenbank](#UseAlerts)weiter unten in diesem  
>   
>  Thema.  
  
##  <a name="using-alerts-for-a-mirrored-database"></a><a name="UseAlerts"></a> Verwenden von Warnmeldungen für eine gespiegelte Datenbank  
 Eine wichtige Komponente der Überwachung einer gespiegelten Datenbank ist das Konfigurieren von Warnmeldungen für bedeutsame bei der Datenbankspiegelung auftretende Ereignisse. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generiert die folgenden Typen von Datenbank-Spiegelungsereignissen:  
  
-   Leistungsschwellenwert-Ereignisse  
  
     Weitere Informationen finden Sie unter "Leistungsschwellenwert-Ereignisse, die an das Windows-Ereignisprotokoll gesendet werden" weiter oben in diesem Thema.  
  
-   Statusänderungereignisse  
  
     Hierbei handelt es sich um Ereignisse der Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI), die generiert werden, wenn im internen Status einer Datenbank-Spiegelungssitzung Änderungen auftreten.  
  
    > [!NOTE]  
    >  Weitere Informationen finden Sie unter [Konzepte des WMI-Anbieters für Serverereignisse](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md).  
  
 Ein Systemadministrator kann für diese Ereignisse mithilfe des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents oder anderen Anwendungen, z. B. [!INCLUDE[msCoName](../../includes/msconame-md.md)] Operations Manager, Warnmeldungen konfigurieren.  
  
 Wenn Sie Warnmeldungen für Datenbank-Spiegelungsereignisse definieren, sollten Sie auf beiden Partnerserverinstanzen Warnungsschwellenwerte und Warnmeldungen definieren. Die einzelnen Ereignisse werden jeweils nur auf dem Prinzipalserver oder auf dem Spiegelserver generiert, aber jeder Partner kann dadurch zu jedem Zeitpunkt die Rolle des anderen Partners übernehmen. Soll sichergestellt sein, dass eine Warnmeldung auch nach einem Failover noch funktionsfähig ist, muss die Warnmeldung auf beiden Partnern definiert werden.  
  
> [!IMPORTANT]  
>  Für alle Spiegelungssitzungen wird dringend empfohlen, die Datenbank so zu konfigurieren, dass bei jedem Statusänderungsereignis eine Warnmeldung gesendet wird. Sofern eine Statusänderung nicht als Ergebnis einer manuellen Konfigurationsänderung erwartet wird, muss davon ausgegangen werden, dass ein Ereignis aufgetreten ist, das Ihre Daten gefährden kann. Um den Schutz der Daten sicherzustellen, müssen Sie die Ursache einer unerwarteten Statusänderung herausfinden und beheben.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **So erstellen Sie eine Warnung mit SQL Server Management Studio**  
  
-   [Erstellen einer Warnung mithilfe einer Fehlernummer](../../ssms/agent/create-an-alert-using-an-error-number.md)  
  
-   [Erstellen einer WMI-Ereigniswarnung](../../ssms/agent/create-a-wmi-event-alert.md)  
  
 **So überwachen Sie die Datenbankspiegelung**  
  
-   [Starten des Datenbankspiegelungs-Monitors &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
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
 [Überwachen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)  
  
  
