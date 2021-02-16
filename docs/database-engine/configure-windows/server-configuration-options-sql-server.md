---
title: Serverkonfigurationsoptionen (SQL Server)
description: Erfahren Sie, wie Sie SQL Server-Ressourcen verwalten und optimieren. Außerdem erfahren Sie mehr über verfügbare Konfigurationsoptionen, mögliche Einstellungen, Standardwerte und Anforderungen für Neustarts.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
keywords:
- Serverkonfiguration (SQL Server)
helpviewer_keywords:
- surface area configuration [SQL Server], sp_configure
- configuration options [SQL Server], when take effect
- server management [SQL Server], configuration options
- SQL Server Management Studio [SQL Server], servers
- servers [SQL Server], configuring
- configuration options [SQL Server], setting
- options [SQL Server], configuration
- RECONFIGURE statement
- performance [SQL Server], servers
- configuration options [SQL Server]
- RECONFIGURE WITH OVERRIDE statement
- SQL Server, configuring
- sp_configure
- stored procedures [SQL Server], configuration options
- server configuration [SQL Server]
- administering SQL Server, configuration options
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: ''
ms.date: 07/20/2020
ms.openlocfilehash: 216e0a9a40466267ab7c7cd5baaef18d9c3e9c38
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341567"
---
# <a name="server-configuration-options-sql-server"></a>Serverkonfigurationsoptionen (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Sie können die Ressourcen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] über Konfigurationsoptionen verwalten und optimieren, indem Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder die gespeicherte Systemprozedur sp_configure verwenden. Die am häufigsten verwendeten Serverkonfigurationsoptionen stehen über [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]zur Verfügung. Mit sp_configure kann auf alle Konfigurationsoptionen zugegriffen werden. Sie sollten vor dem Festlegen dieser Optionen die Auswirkungen auf Ihr System sorgfältig überdenken. Weitere Informationen finden Sie unter [Anzeigen oder Ändern von Servereigenschaften &#40;SQL Server&#41;](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md).

>**WICHTIG!** Erweiterte Optionen sollten ausschließlich von einem erfahrenen Datenbankadministrator oder einem zertifizierten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Techniker geändert werden.

## <a name="categories-of-configuration-options"></a>Kategorien von Konfigurationsoptionen

Konfigurationsoptionen treten wie folgt in Kraft:

- Unmittelbar nach dem Festlegen der Option und dem Ausgeben der **RECONFIGURE** -Anweisung (oder in einigen Fällen der **RECONFIGURE WITH OVERRIDE**-Anweisung). Durch die Neukonfiguration bestimmter Optionen werden Pläne im Plancache für ungültig erklärt, was zum Kompilieren neuer Pläne führt. Weitere Informationen finden Sie unter [DBCC FREEPROCCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md).

  \- oder –

- Nach dem Ausführen der obigen Aktionen und dem Neustarten der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Für Optionen, die einen Neustart von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erfordern, wird der geänderte Wert zunächst nur in der value-Spalte angezeigt. Nach dem Neustart wird der neue Wert sowohl in der value-Spalte als auch in der value_in_use-Spalte angezeigt.

Bei einigen Optionen tritt der neue Konfigurationswert erst nach einem Neustart des Servers in Kraft. Wenn Sie den neuen Wert festlegen und „sp_configure“ ausführen, bevor Sie den Server neu starten, wird der neue Wert in der **value** -Spalte der Konfigurationsoptionen, jedoch nicht in der **value_in_use** -Spalte angezeigt. Nach dem Neustart des Servers wird der neue Wert in der **value_in_use** -Spalte angezeigt.

Selbstkonfigurierende Optionen sind jene Optionen, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gemäß den Anforderungen des Systems angepasst werden. In den meisten Fällen ist es dadurch nicht notwendig, die Werte manuell festzulegen. Beispiele hierfür sind die Option **Max. Anzahl von Arbeitsthreads**  und die Option für Benutzerverbindungen.

## <a name="configuration-options-table"></a>Tabelle der Konfigurationsoptionen
 In der folgenden Tabelle werden alle verfügbaren Konfigurationsoptionen, der Bereich der möglichen Einstellungen und die Standardwerte aufgelistet. Konfigurationsoptionen sind wie folgt mit Buchstabencodes gekennzeichnet:

- A (Advanced) = Erweiterte Optionen, die nur von einem erfahrenen Datenbankadministrator oder einem zertifizierten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Experten geändert werden sollten und das Festlegen von „Erweiterte Optionen anzeigen“ auf 1 erfordern.

- RR (Restart Required) = Optionen, die den Neustart von [!INCLUDE[ssDE](../../includes/ssde-md.md)]erfordern.

- RP (Restart PolyBase) = Optionen, die einen Neustart der PolyBase-Engine erfordern.

- SC (Self-Configuring) = Selbstkonfigurierende Optionen.

| Konfigurationsoption | Mindestwert | Maximalwert | Standard |
|--|--|--|--|
| [AccessCheckCache-Bucketanzahl](../../database-engine/configure-windows/access-check-cache-server-configuration-options.md) (A) | 0 | 16384 | 0 |
| [AccessCheckCache-Kontingent](../../database-engine/configure-windows/access-check-cache-server-configuration-options.md) (A) | 0 | 2147483647 | 0 |
| [Ad Hoc Distributed Queries](../../database-engine/configure-windows/ad-hoc-distributed-queries-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [ADR cleaner retry timeout (min)](../../database-engine/configure-windows/adr-cleaner-retry-timeout-configuration-option.md) (Zeitlimit für Wiederholung der ADR-Bereinigung (Min))<br><br> Eingeführt in SQL Server 2019 | 0 | 32767 | 15 |
| [ADR Preallocation Factor](../../database-engine/configure-windows/adr-preallocation-factor-server-configuration-option.md) (Faktor für die ADR-Vorabzuordnung)<br><br> Eingeführt in SQL Server 2019 | 0 | 32767 | 4 |
| [Affinity I/O Mask](../../database-engine/configure-windows/affinity-input-output-mask-server-configuration-option.md) (A, RR) | -2147483648 | 2147483647 | 0 |
| [Affinity Mask](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md) (A) | -2147483648 | 2147483647 | 0 |
| [Affinity64 I/O Mask](../../database-engine/configure-windows/affinity64-input-output-mask-server-configuration-option.md) (A, nur verfügbar in der 64-Bit-Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) | -2147483648 | 2147483647 | 0 |
| [Affinity64 Mask](../../database-engine/configure-windows/affinity64-mask-server-configuration-option.md) (A, RR), nur verfügbar in der 64-Bit-Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] | -2147483648 | 2147483647 | 0 |
| [Agent XPs](../../database-engine/configure-windows/agent-xps-server-configuration-option.md) (A) | 0 | 1 | 0<br /><br /> (Wird zu 1 geändert, wenn der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent gestartet wird. Der Standardwert ist 0, wenn der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent beim Setup auf automatischen Start festgelegt wurde.) |
| [allow polybase export](../../database-engine/configure-windows/allow-polybase-export.md)<br/><br/> [!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)].| 0 | 1 | 0 |
| [Allow Updates](../../database-engine/configure-windows/allow-updates-server-configuration-option.md) (Veraltet. Darf nicht verwendet werden. Führt beim Neukonfigurieren zu einem Fehler.) | 0 | 1 | 0 |
| [automatic soft-NUMA disabled](soft-numa-sql-server.md) | 0 | 1 | 0 |
| [Standardeinstellung der Sicherungsprüfsumme](../../database-engine/configure-windows/backup-checksum-default.md) | 0 | 1 | 0 |
| [backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md) | 0 | 1 | 0 |
| [Schwellenwert für blockierte Prozesse](../../database-engine/configure-windows/blocked-process-threshold-server-configuration-option.md) (A) | 5 | 86.400 | 0 |
| [C2-Überwachungsmodus](../../database-engine/configure-windows/c2-audit-mode-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) | 0 | 1 | 0 |
| [clr strict security](../../database-engine/configure-windows/clr-strict-security.md) (A) <br /> [!INCLUDE [sqlserver2017](../../includes/applies-to-version/sqlserver2017.md)]. | 0 | 1 | 0 |
| [column encryption enclave type ](../../database-engine/configure-windows/configure-column-encryption-enclave-type.md) (A, RR) | 0 | 1 | 0 |
| [Common Criteria-Kompatibilität aktiviert](../../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [contained database authentication](../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md) | 0 | 1 | 0 |
| [Kostenschwellenwert für Parallelität](../../database-engine/configure-windows/configure-the-cost-threshold-for-parallelism-server-configuration-option.md) (A) | 0 | 32767 | 5 |
| [cross db ownership chaining](../../database-engine/configure-windows/cross-db-ownership-chaining-server-configuration-option.md) | 0 | 1 | 0 |
| [Cursorschwellenwert](../../database-engine/configure-windows/configure-the-cursor-threshold-server-configuration-option.md) (A) | -1 | 2147483647 | -1 |
| [Erweiterte gespeicherte Prozeduren für Datenbank-E-Mail](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [Volltext-Standardsprache](../../database-engine/configure-windows/configure-the-default-full-text-language-server-configuration-option.md) (A) | 0 | 2147483647 | 1033 |
| [default language](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) | 0 | 9999 | 0 |
| [Standardablaufverfolgung aktiviert](../../database-engine/configure-windows/default-trace-enabled-server-configuration-option.md) (A) | 0 | 1 | 1 |
| [Ergebnisse von Triggern nicht zulassen](../../database-engine/configure-windows/disallow-results-from-triggers-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [EKM provider enabled](../../database-engine/configure-windows/ekm-provider-enabled-server-configuration-option.md) | 0 | 1 | 0 |
| [external scripts enabled](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md) (SC) (RR)<br /><br />[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]. | 0 | 1 | 0 |
| [filestream_access_level](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md) | 0 | 2 | 0 |
| [Füllfaktor](../../database-engine/configure-windows/configure-the-fill-factor-server-configuration-option.md) (A, RR) | 0 | 100 | 0 |
| [ft crawl bandwidth (max)](../../database-engine/configure-windows/ft-crawl-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 100 |
| [ft crawl bandwidth (min)](../../database-engine/configure-windows/ft-crawl-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 0 |
| [ft notify bandwidth (max)](../../database-engine/configure-windows/ft-notify-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 100 |
| [ft notify bandwidth (min)](../../database-engine/configure-windows/ft-notify-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 0 |
| [Hadoop-Konnektivität](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md) (RP)<br /><br />[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]. | 0 | 7 | 0 |
| [Lösung für unklare Transaktion](../../database-engine/configure-windows/in-doubt-xact-resolution-server-configuration-option.md) (A) | 0 | 2 | 0 |
| [Speicher für Indexerstellung](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md) (A, SC) | 704 | 2147483647 | 0 |
| [Lightweightpooling](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [Sperren](../../database-engine/configure-windows/configure-the-locks-server-configuration-option.md) (A, RR, SC) | 5\.000 | 2147483647 | 0 |
| [Max. Grad an Parallelität](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) (A) | 0 | 32767 | 0 |
| [Max. Bereich für Volltextdurchforstung](../../database-engine/configure-windows/max-full-text-crawl-range-server-configuration-option.md) (A) | 0 | 256 | 4 |
| [Max. Serverarbeitsspeicher](../../database-engine/configure-windows/server-memory-server-configuration-options.md) (A, SC) | 16 | 2147483647 | 2147483647 |
| [max text repl size](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md) | 0 | 2147483647 | 65536 |
| [Max. Anzahl von Arbeitsthreads](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md) (A) | 128 | 32767<br /><br /> 1024 ist der empfohlene Höchstwert für die 32-Bit-Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], 2048 für die 64-Bit-Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **Hinweis:** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ist die letzte verfügbare Version für 32-Bit-Betriebssysteme. | 0<br /><br /> Bei dem Wert 0 (null) wird automatisch die maximale Anzahl der Arbeitsthreads abhängig von der Anzahl der Prozessoren nach der Formel (256 + ( *\<processors>*  – 4) × 8) für die 32-Bit-Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und (512 + ( *\<processors>*  – 4) × 8) für die 64-Bit-Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] konfiguriert. **Hinweis:** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ist die letzte verfügbare Version für 32-Bit-Betriebssysteme. |
| [Medienbeibehaltung](../../database-engine/configure-windows/configure-the-media-retention-server-configuration-option.md) (A, RR) | 0 | 365 | 0 |
| [Min. Arbeitsspeicher pro Abfrage](../../database-engine/configure-windows/configure-the-min-memory-per-query-server-configuration-option.md) (A) | 512 | 2147483647 | 1024 |
| [Min. Serverarbeitsspeicher](../../database-engine/configure-windows/server-memory-server-configuration-options.md) (A, SC) | 0 | 2147483647 | 0 |
| [nested triggers](../../database-engine/configure-windows/configure-the-nested-triggers-server-configuration-option.md) | 0 | 1 | 1 |
| [Netzwerkpaketgröße](../../database-engine/configure-windows/configure-the-network-packet-size-server-configuration-option.md) (A) | 512 | 32767 | 4096 |
| [OLE-Automatisierungsprozeduren](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [Geöffnete Objekte](../../database-engine/configure-windows/open-objects-server-configuration-option.md) (A, RR, veraltet) | 0 | 2147483647 | 0 |
| [Für Ad-hoc-Arbeitsauslastungen optimieren](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [PH-Timeout](../../database-engine/configure-windows/ph-timeout-server-configuration-option.md) (A) | 1 | 3600 | 60 |
| [polybase enabled](../../relational-databases/polybase/polybase-installation.md#enable) (RR) <br/><br/>[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]| 0 | 1 | 0 |
| [polybase network encryption](../../relational-databases/polybase/polybase-installation.md#enable) (PolyBase-Netzwerkverschlüsselung) | 0 | 1 | 1 |
| [Rang vorausberechnen](../../database-engine/configure-windows/precompute-rank-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [Prioritätserhöhung](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [Kostenbeschränkung der Abfragekontrolle](../../database-engine/configure-windows/configure-the-query-governor-cost-limit-server-configuration-option.md) (A) | 0 | 2147483647 | 0 |
| [Abfragewartezeit](../../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md) (A) | -1 | 2147483647 | -1 |
| [Wiederherstellungsintervall](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md) (A, SC) | 0 | 32767 | 0 |
| [Remotezugriff](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md) (RR) | 0 | 1 | 1 |
| [remote admin connections](../../database-engine/configure-windows/remote-admin-connections-server-configuration-option.md) | 0 | 1 | 0 |
| [remote data archive](../../database-engine/configure-windows/configure-the-remote-data-archive-server-configuration-option.md) | 0 | 1 | 0 |
| [remote login timeout](../../database-engine/configure-windows/configure-the-remote-login-timeout-server-configuration-option.md) | 0 | 2147483647 | 10 |
| [remote proc trans](../../database-engine/configure-windows/configure-the-remote-proc-trans-server-configuration-option.md) | 0 | 1 | 0 |
| [remote query timeout](../../database-engine/configure-windows/configure-the-remote-query-timeout-server-configuration-option.md) | 0 | 2147483647 | 600 |
| [Replication XPs (Option)](../../database-engine/configure-windows/replication-xps-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [Startprozeduren suchen](../../database-engine/configure-windows/configure-the-scan-for-startup-procs-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [server trigger recursion](../../database-engine/configure-windows/server-trigger-recursion-server-configuration-option.md) | 0 | 1 | 1 |
| [Festgelegte Workingsetgröße](../../database-engine/configure-windows/set-working-set-size-server-configuration-option.md) (A, RR, veraltet) | 0 | 1 | 0 |
| [show advanced options](../../database-engine/configure-windows/show-advanced-options-server-configuration-option.md) | 0 | 1 | 0 |
| [Erweiterte gespeicherte Prozeduren für SMO und DMO](../../database-engine/configure-windows/smo-and-dmo-xps-server-configuration-option.md) (A) | 0 | 1 | 1 |
| [Wiederherstellungsmodellfehler unterdrücken](../../database-engine/configure-windows/suppress-recovery-model-errors-server-configuration-option.md) (A) <br/><br/>[!INCLUDE [asdbmi](../../includes/applies-to-version/_asdbmi.md)]| 0 | 1 | 0 |
| [tempdb metadata memory-optimized](../../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata) (A) <br/><br/> [!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)].| 0 | 1 | 0 |
| [Füllwörtertransformation](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [Umstellungsjahr für Angaben mit zwei Ziffern](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) (A) | 1753 | 9999 | 2049 |
| [Benutzerverbindungen](../../database-engine/configure-windows/configure-the-user-connections-server-configuration-option.md) (A, RR, SC) | 0 | 32767 | 0 |
| [user options](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md) | 0 | 32767 | 0 |
| [xp_cmdshell](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md) (A) | 0 | 1 | 0 |  |

## <a name="see-also"></a>Weitere Informationen
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md) [DBCC FREEPROCCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md)


