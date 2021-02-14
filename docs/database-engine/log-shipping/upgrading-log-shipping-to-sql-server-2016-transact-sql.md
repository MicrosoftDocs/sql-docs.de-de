---
title: Upgraden des Protokollversands auf SQL Server 2016 und höher
description: Informieren Sie sich über die korrekte Reihenfolge, um Ihre Protokollversandlösung für die Notfallwiederherstellung bei einem Upgrade von einer früheren Version auf SQL Server 2016 und höher beizubehalten.
ms.custom: seo-lt-2019
ms.date: 02/01/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], upgrading
ms.assetid: b1289cc3-f5be-40bb-8801-0e3eed40336e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9f384313f14f19a80aef0bd8ee60e37386b3d6b2
ms.sourcegitcommit: 58e7069b5b2b6367e27b49c002ca854b31b1159d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2021
ms.locfileid: "99552600"
---
# <a name="upgrading-log-shipping-to-sql-server-2016-transact-sql"></a>Aktualisieren des Protokollversands auf SQL Server 2016 (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Um die Lösung für die Notfallwiederherstellung für den Protokollversand beizubehalten, führen Sie das Upgrade bzw. die Wartungsupdates in der richtigen Reihenfolge durch. Wartungsupdates umfassen Service Packs oder kumulative Updates.  
  
> [!NOTE]  
> Die[Sicherungskomprimierung](../../relational-databases/backup-restore/backup-compression-sql-server.md) wurde in [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)]eingeführt. In einer aktualisierten Protokollversandkonfiguration wird durch die Serverkonfigurationsoption **Komprimierungsstandard für Sicherung** bestimmt, ob die Transaktionsprotokoll-Sicherungsdateien mithilfe der Sicherungskomprimierung komprimiert werden. Das Verhalten für die Sicherungskomprimierung der Protokollsicherung kann für jede Protokollversandkonfiguration festgelegt werden. Weitere Informationen finden Sie unter [Konfigurieren des Protokollversands &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)eingeführt.  
  
 **In diesem Thema:**  
  
-   [Voraussetzungen](#Prerequisites)  
  
-   [Datensicherung vor dem Upgrade](#ProtectData)  
  
-   [Aktualisieren der Überwachungsserverinstanz](#UpgradeMonitor)  
  
-   [Aktualisieren der sekundären Serverinstanzen](#UpgradeSecondaries)  
  
-   [Aktualisieren der primären Instanz](#UpgradePrimary)  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
 Lesen Sie die folgenden wichtigen Informationen, bevor Sie beginnen:  
  
-   [Unterstützte Versions- und Editionsupgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md): Überprüfen Sie, ob ein Upgrade von Ihrer Version des Windows-Betriebssystems und Ihrer SQL Server-Version auf SQL Server 2016 möglich ist. Sie können beispielsweise nicht direkt von einer SQL Server 2005-Instanz auf [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)]upgraden.  
  
-   [Auswählen einer Upgrademethode für die Datenbank-Engine](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md): Wählen Sie basierend auf Ihrer Prüfung der unterstützten Versions- und Editionsupgrades sowie basierend auf den anderen in Ihrer Umgebung installierten Komponenten die passende Upgrademethode und die passenden Upgradeschritte aus, um das Upgrade der Komponenten in der richtigen Reihenfolge durchzuführen.  
  
-   [Planen und Testen des Upgradeplans für die Datenbank-Engine](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md): Überprüfen Sie die Anmerkungen zu dieser Version, die bekannten Upgradeprobleme und die Prüfliste vor dem Upgrade. Entwickeln und testen Sie dann den Upgradeplan.  
  
-   [Hardware- und Softwareanforderungen für die Installation von SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md):  Überprüfen Sie die Softwareanforderungen für die Installation von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Falls zusätzliche Software erforderlich ist, installieren Sie diese auf jedem Knoten, bevor Sie mit dem Upgradevorgang beginnen, um die Downtime zu minimieren.  
  
##  <a name="protect-your-data-before-the-upgrade"></a><a name="ProtectData"></a> Datensicherung vor dem Upgrade  
 Als bewährte Methode wird empfohlen, dass Sie die Daten vor einem Protokollversandupgrade schützen.  
  
 **So schützen Sie die Daten**  
  
1.  Führen Sie für jede primäre Datenbank eine vollständige Datenbanksicherung aus.  
  
     Weitere Informationen finden Sie unter [Erstellen einer vollständigen Datenbanksicherung &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
2.  Führen Sie den [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) -Befehl für jede primäre Datenbank aus.  
  
> [!IMPORTANT]  
>  Stellen Sie sicher, dass auf Ihrem primären Server für die erwartete Dauer des Upgrades der sekundären Instanzen ausreichend Speicherplatz für die Protokollsicherungskopien verfügbar ist.  Bei einem Failover zu einer sekundären Instanz gilt dasselbe für die sekundäre Instanz (die neue primäre Instanz).  
  
##  <a name="upgrading-the-optional-monitor-server-instance"></a><a name="UpgradeMonitor"></a> Aktualisieren der (optionalen) Überwachungsserverinstanz  
 Die Überwachungsserverinstanz, sofern vorhanden, kann jederzeit aktualisiert werden. Der optionale Überwachungsserver muss jedoch nicht aktualisiert werden, wenn Sie die primären und sekundären Server aktualisieren.  
  
 Während der Aktualisierung des Überwachungsservers ist die Protokollversandkonfiguration weiterhin in Betrieb, ihr Status wird allerdings nicht in den Tabellen auf dem Überwachungsserver aufgezeichnet. Warnungen, die ggf. konfiguriert wurden, werden während der Aktualisierung des Überwachungsservers nicht ausgelöst. Nach der Aktualisierung können Sie die Überwachungstabellen aktualisieren, indem Sie die gespeicherte Systemprozedur [sp_refresh_log_shipping_monitor](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md) ausführen.   Weitere Informationen zu einem Überwachungsserver finden Sie unter [Informationen zum Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md).  
  
##  <a name="upgrading-the-secondary-server-instances"></a><a name="UpgradeSecondaries"></a> Aktualisieren der sekundären Serverinstanzen  

Der Upgradevorgang erfordert die Aktualisierung der sekundären Serverinstanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], bevor die primäre Serverinstanz aktualisiert wird. Aktualisieren Sie immer zuerst die sekundären [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanzen. Der Protokollversand wird während des Upgradevorgangs weiter ausgeführt, weil die aktualisierten sekundären Serverinstanzen die Protokollsicherungen der primären Serverinstanz weiterhin wiederherstellen. Wenn die primäre Serverinstanz vor der sekundären Serverinstanz aktualisiert wird, tritt beim Protokollversand ein Fehler auf, weil eine auf einer neueren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellte Sicherung nicht auf einer älteren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]wiederhergestellt werden kann. Sie können die sekundären Instanzen gleichzeitig oder nacheinander aktualisieren. Es müssen jedoch alle sekundären Instanzen aktualisiert worden sein, bevor die primäre Instanz aktualisiert wird, um einen Fehler beim Protokollversand zu vermeiden.  
  
Während des Upgrades der sekundären Serverinstanz werden die Kopier- und Wiederherstellungsaufträge des Protokollversands nicht ausgeführt. Nicht wiederhergestellte Sicherungen von Transaktionsprotokollen sammeln sich daher auf der primären Instanz an. Daher muss ausreichend Speicherplatz für diese nicht wiederhergestellten Sicherungen vorhanden sein. Die Menge der angehäuften Daten hängt von der Häufigkeit der geplanten Sicherungen auf der primären Serverinstanz und der Reihenfolge ab, mit der die sekundären Instanzen aktualisiert werden. Wenn ein getrennter Überwachungsserver konfiguriert wurde, werden möglicherweise Warnungen ausgegeben, die anzeigen, dass über einen längeren Zeitraum als das konfigurierte Intervall hinweg keine Wiederherstellungsvorgänge ausgeführt wurden.  
  
 Nachdem die sekundären Serverinstanzen aktualisiert wurden, werden die Protokollversandaufträge der Agents fortgesetzt, und die Kopier- und Wiederherstellungsvorgänge für Protokollsicherungen der primären Serverinstanz auf den sekundären Serverinstanzen werden weiter ausgeführt. Die erforderliche Zeit, die die sekundären Serverinstanzen benötigen, um die sekundäre Datenbank zu aktualisieren, variiert abhängig von der Zeit, die die Aktualisierung der sekundären Serverinstanz in Anspruch nahm, und der Häufigkeit der Sicherungen auf dem primären Server.  
  
> [!NOTE]  
> Während des Serverupgrades wird die sekundäre Datenbank selbst nicht auf die neue Version aktualisiert. Die Datenbank wird nur aktualisiert, wenn sie durch das Initiieren eines Failovers der Protokollversand-Datenbank online geschaltet wird. Theoretisch könnte dieser Zustand für einen unbegrenzten Zeitraum beibehalten werden. Der erforderliche Zeitaufwand für das Upgrade der Datenbank-Metadaten beim Initiieren eines Failovers ist gering.  
  
> [!IMPORTANT]  
>  Die RESTORE WITH STANDBY-Option wird für Datenbanken, die eine Aktualisierung erfordern, nicht unterstützt. Wenn eine aktualisierte sekundäre Datenbank mithilfe von RESTORE WITH STANDBY konfiguriert wurde, können Transaktionsprotokolle nach einer Aktualisierung nicht wiederhergestellt werden. Um den Protokollversand auf dieser sekundären Datenbank fortzusetzen, müssen Sie ihn auf diesem Standbyserver erneut einrichten. Weitere Informationen über die STANDBY-Option finden Sie unter [Wiederherstellen einer Transaktionsprotokollsicherung &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md).  
  
##  <a name="upgrading-the-primary-server-instance"></a><a name="UpgradePrimary"></a> Aktualisieren der primären Serverinstanz  
 Da der Protokollversand in erster Linie eine Notfallwiederherstellungslösung ist, wird im einfachsten und gängigsten Szenario ein direktes Upgrade der primären Instanz durchgeführt, wobei die Datenbank während dieses Upgrades nicht verfügbar ist. Sobald der Server aktualisiert wurde, wird die Datenbank automatisch online geschaltet. Daraufhin wird sie automatisch aktualisiert. Nachdem die Datenbank aktualisiert wurde, werden die Protokollversandaufträge fortgesetzt.  
  
> [!NOTE]  
>  Der Protokollversand unterstützt auch die Option zum [Failover zu einer sekundären Datenbank für den Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) sowie zum [Ändern der Rollen zwischen primärem und sekundärem Protokollversandserver &#40;SQL Server&#41;](../../database-engine/log-shipping/change-roles-between-primary-and-secondary-log-shipping-servers-sql-server.md). Da der Protokollversand heute jedoch nur noch selten als Hochverfügbarkeitslösung konfiguriert wird (neuere Optionen sind deutlich stabiler), wird die Downtime durch ein Failover im Allgemeinen nicht minimiert, da Systemdatenbank-Objekte nicht synchronisiert werden. Außerdem ist es äußerst aufwändig, Clients eine problemlose Ermittlung und Verbindung mit einer höher gestuften sekundären Instanz zu ermöglichen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Aktualisieren auf SQL Server 2016 mithilfe des Installations-Assistenten &#40;Setup&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)   
 [Installieren von SQL Server 2016 von der Eingabeaufforderung](../install-windows/install-sql-server-from-the-command-prompt.md)   
 [Konfigurieren des Protokollversands (SQL Server)](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)   
 [Überwachen des Protokollversands (Transact-SQL)](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)   
 [Protokollversandtabellen und gespeicherte Prozeduren](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
