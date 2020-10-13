---
title: SQL Server-Replikation | Microsoft-Dokumentation
description: Hier lernen Sie die Replikation in SQL Server kennen. Dabei handelt es sich um eine Technologie zum Kopieren und Verteilen von Daten und Datenbankobjekten zwischen Datenbanken und zur Synchronisierung von Datenbanken.
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], about
- replication [SQL Server]
ms.assetid: 3a5f4592-3c61-4b4d-9ceb-39716aeeba41
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 0690ad04a9b4dfdb2edd22c4e882616c2f8bf73f
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91869501"
---
# <a name="sql-server-replication"></a>SQL Server-Replikation
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Bei der Replikation handelt es sich um eine Reihe von Technologien zum Kopieren und Verteilen von Daten und Datenbankobjekten aus einer Datenbank in eine andere und das anschließende Synchronisieren zwischen den Datenbanken, um die Konsistenz der Daten sicherzustellen. Mithilfe von Replikation können Sie Daten an verschiedene Standorte, an Remotebenutzer oder mobile Benutzer über lokale Netzwerke und WANs (Wide Area Network), über DFÜ-Verbindungen, Funkverbindungen oder über das Internet verteilen.  
  
 Die Transaktionsreplikation wird typischerweise in reinen Serverumgebungen verwendet, die einen hohen Durchsatz erfordern, und ist für die folgenden Fälle geeignet: Verbessern der Skalierbarkeit und Verfügbarkeit, Data Warehousing und Berichterstellung, Integrieren von Daten aus mehreren Standorten, Integrieren heterogener Daten und Auslagern der Batchverarbeitung. Die Mergereplikation ist in erster Linie für mobile Anwendungen oder verteilte Serveranwendungen mit möglichen Datenkonflikten konzipiert. Dazu gehören folgende häufige Szenarien: Datenaustausch mit mobilen Benutzern, Point-of-Sale-Anwendungen (POS) und Integrieren von Daten aus mehreren Standorten. Momentaufnahmereplikation wird verwendet, um das Anfangsdataset für Transaktions- und Mergereplikation bereitzustellen. Sie kann auch verwendet werden, wenn vollständige Aktualisierungen von Daten erforderlich sind. Mit diesen drei Replikationstypen stellt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ein leistungsfähiges und flexibles System zum Synchronisieren von Daten im gesamten Unternehmen bereit. Die Replikation in SQLCE 3.5 und SQLCE 4.0 wird unter [!INCLUDE[win8srv](../../includes/win8srv-md.md)] und [!INCLUDE[win8](../../includes/win8-md.md)]unterstützt.  


## <a name="whats-new"></a>Neues 
- Durch SQL Server 2017 wurde der Funktionsumfang der SQL Server-Replikation nicht wesentlich erweitert. 
- Durch SQL Server 2016 wurde der Funktionsumfang der SQL Server-Replikation nicht wesentlich erweitert. 

Informationen zur Abwärtskompatibilität finden Sie unter [Abwärtskompatibilität von Replikationen](replication-backward-compatibility.md). 


 ## <a name="replication-security"></a>Replikationssicherheit
  
-   [Anzeigen und Ändern von Replikationssicherheitseinstellungen](security/view-and-modify-replication-security-settings.md)  
-   [Verwalten von Anmeldungen in der Veröffentlichungszugriffsliste](security/manage-logins-in-the-publication-access-list.md)  
  
## <a name="publishing-and-distribution"></a>Veröffentlichung und Verteilung  
  
-   [Konfigurieren der Veröffentlichung und der Verteilung](configure-publishing-and-distribution.md)   
-   [Anzeigen und Ändern von Veröffentlichungseigenschaften](publish/view-and-modify-publication-properties.md)   
-   [Deaktivieren der Veröffentlichung und Verteilung](disable-publishing-and-distribution.md)  
  
## <a name="publications-and-articles"></a>Veröffentlichungen und Artikel 
  
-   [Erstellen einer Veröffentlichung](publish/create-a-publication.md)    
-   [Definieren eines Artikels](publish/define-an-article.md)   
-   [Anzeigen und Ändern von Veröffentlichungseigenschaften](publish/view-and-modify-publication-properties.md)   
-   [Anzeigen und Ändern von Artikeleigenschaften](publish/view-and-modify-article-properties.md)    
-   [Löschen einer Veröffentlichung](publish/delete-a-publication.md)   
-   [Löschen eines Artikels](publish/delete-an-article.md)    
-   [Erstellen einer Veröffentlichung aus einer Oracle-Datenbank](publish/create-a-publication-from-an-oracle-database.md)   
-   [Festlegen des Ablaufdatums für Abonnements](publish/set-the-expiration-period-for-subscriptions.md)  
-   [Angeben von Schemaoptionen](publish/specify-schema-options.md)  
-   [Replizieren von Schemaänderungen](publish/replicate-schema-changes.md)    
-   [Verwalten von Identitätsspalten](publish/manage-identity-columns.md)   
-   [Festlegen des Kompatibilitätsgrads von Mergeveröffentlichungen](publish/set-the-compatibility-level-for-merge-publications.md)  
  
### <a name="snapshot-options"></a>Momentaufnahmeoptionen  
  
-   [Konfigurieren von Momentaufnahmeeigenschaften](publish/configure-snapshot-properties-replication-transact-sql-programming.md)    
-   [Übermitteln einer Momentaufnahme über FTP](publish/deliver-a-snapshot-through-ftp.md) 
  
### <a name="filter-data"></a>Filtern von Daten  
  
-   [Definieren und Ändern eines Spaltenfilters](publish/define-and-modify-a-column-filter.md)    
-   [Definieren und Ändern eines statischen Zeilenfilters](publish/define-and-modify-a-static-row-filter.md)    
-   [Definieren und Ändern eines parametrisierten Zeilenfilters für einen Mergeartikel](publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)    
-   [Optimieren von parametrisierten Zeilenfiltern](publish/optimize-parameterized-row-filters.md)    
-   [Definieren und Ändern eines Verknüpfungsfilters zwischen Mergeartikeln](publish/define-and-modify-a-join-filter-between-merge-articles.md)  
  
### <a name="transactional-replication-options"></a>Transaktionsreplikationsoptionen  
  
-   [Festlegen der Propagierungsmethode für Datenänderungen an Transaktionsartikeln](publish/set-the-propagation-method-for-data-changes-to-transactional-articles.md)    
-   [Aktivieren des Aktualisierens von Abonnements für Transaktionsveröffentlichungen](publish/enable-updating-subscriptions-for-transactional-publications.md)  
  
### <a name="merge-replication-options"></a>Mergereplikationsoptionen  
  
-   [Definieren einer logische Datensatzbeziehung zwischen Mergetabellenartikeln](publish/define-a-logical-record-relationship-between-merge-table-articles.md)    
-   [Specify Merge Replication properties (Angeben von Mergereplikationseigenschaften)](merge/specify-merge-replication-properties.md)    
-   [Angeben eines Mergeartikelkonfliktlösers](publish/specify-a-merge-article-resolver.md)    

  
## <a name="manage-subscriptions"></a>Verwalten von Abonnements  
  
-   [Erstellen eines Pullabonnements](create-a-pull-subscription.md)    
-   [Anzeigen und Ändern der Eigenschaften von Pullabonnements](view-and-modify-pull-subscription-properties.md)    
-   [Löschen eines Pullabonnements](delete-a-pull-subscription.md)    
-   [Erstellen eines Pushabonnements](create-a-push-subscription.md)   
-   [Anzeigen und Ändern der Eigenschaften von Pushabonnements](view-and-modify-push-subscription-properties.md)   
-   [Löschen eines Pushabonnements](delete-a-push-subscription.md)   
-   [Angeben von Synchronisierungszeitplänen](specify-synchronization-schedules.md)    
-   [Erstellen von aktualisierbaren Abonnements für eine Transaktionsveröffentlichung](publish/create-an-updatable-subscription-to-a-transactional-publication.md)  
-   [Erstellen eines Abonnements für einen Nicht-SQL Server-Abonnenten](create-a-subscription-for-a-non-sql-server-subscriber.md)  
  
## <a name="synchronize-subscriptions"></a>Synchronisieren von Abonnements  
  
-   [Erstellen und Anwenden der Anfangsmomentaufnahme](create-and-apply-the-initial-snapshot.md)   
-   [Erstellen einer Momentaufnahme für eine Mergeveröffentlichung mit parametrisierten Filtern](create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)    
-   [Initialisieren eines Transaktionsabonnements von einer Sicherung &#40;Replikationsprogrammierung mit Transact-SQL&#41;](initialize-a-transactional-subscription-from-a-backup.md)    
-   [Manuelles Initialisieren eines Abonnements](initialize-a-subscription-manually.md)    
-   [Synchronisieren eines Pullabonnements](synchronize-a-pull-subscription.md)    
-   [Synchronisieren eines Pushabonnements](synchronize-a-push-subscription.md)   
-   [Erneutes Initialisieren eines Abonnements](reinitialize-a-subscription.md)    
-   [Ausführen von Skripts während der Synchronisierung &#40;Replikationsprogrammierung mit Transact-SQL&#41;](execute-scripts-during-synchronization-replication-transact-sql-programming.md)    
-   [Implementieren eines Geschäftslogikhandlers für einen Mergeartikel](implement-a-business-logic-handler-for-a-merge-article.md)  
-   [Debuggen eines Geschäftslogikhandlers &#40;Replikationsprogrammierung&#41;](debug-a-business-logic-handler-replication-programming.md)    
-   [Kontrollieren des Verhaltens von Triggern und Einschränkungen während der Synchronisierung &#40;Replikationsprogrammierung mit Transact-SQL&#41;](control-behavior-of-triggers-and-constraints-in-synchronization.md)    
-   [Implementieren eines benutzerdefinierten Konfliktlösers für einen Mergeartikel](implement-a-custom-conflict-resolver-for-a-merge-article.md)  
  
## <a name="administration"></a>Verwaltung 
  
-   [Arbeiten mit Replikations-Agent-Profilen](agents/work-with-replication-agent-profiles.md)   
-   [Überprüfen der Daten am Abonnenten](validate-data-at-the-subscriber.md)    
-   [Verwalten von Partitionen für eine Mergeveröffentlichung mit parametrisierten Filtern](publish/manage-partitions-for-a-merge-publication-with-parameterized-filters.md)    
-   [Massenladen von Daten in Tabellen in einer Mergeveröffentlichung &#40;Replikationsprogrammierung mit Transact-SQL&#41;](bulk-load-data-into-tables-in-a-merge-publication.md)    
-   [Cleanup von Mergemetadaten &#40;Replikationsprogrammierung mit Transact-SQL&#41;](administration/clean-up-merge-metadata-replication-transact-sql-programming.md)    
-   [Ausführen eines Pseudoupdates für einen Mergeartikel &#40;Replikationsprogrammierung mit Transact-SQL&#41;](administration/perform-a-dummy-update-for-a-merge-article-replication-transact-sql-programming.md)    
-   [Anzeigen replizierter Befehle und anderer Informationen in der Verteilungsdatenbank &#40;Replikationsprogrammierung mit Transact-SQL&#41;](monitor/view-replicated-commands-and-information-in-distribution-database.md)    
-   [Aktivieren koordinierter Sicherungen für die Transaktionsreplikation &#40;Replikationsprogrammierung mit Transact-SQL&#41;](administration/enable-coordinated-backups-for-transactional-replication.md)   
-   [Verwalten einer Peer-zu-Peer-Topologie &#40;Replikationsprogrammierung mit Transact-SQL&#41;](administration/administer-a-peer-to-peer-topology-replication-transact-sql-programming.md)    
-   [Versetzen einer Replikationstopologie in einen inaktiven Status &#40;Replikationsprogrammierung mit Transact-SQL&#41;](administration/quiesce-a-replication-topology-replication-transact-sql-programming.md)    
-   [Konfigurieren des Transaktionssatzauftrags für einen Oracle-Verleger &#40;Replikationsprogrammierung mit Transact-SQL&#41;](administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)   
-   [Aktualisieren von Replikationsskripts &#40;Replikationsprogrammierung mit Transact-SQL&#41;](administration/upgrade-replication-scripts-replication-transact-sql-programming.md)  
  
## <a name="monitor"></a>Überwachen
  
-   [Zulassen, dass Nichtadministratoren den Replikationsmonitor verwenden](monitor/allow-non-administrators-to-use-replication-monitor.md)    
-   [Programmgesteuertes Überwachen der Replikation](monitor/programmatically-monitor-replication.md)    
-   [Anzeigen replizierter Befehle und anderer Informationen in der Verteilungsdatenbank &#40;Replikationsprogrammierung mit Transact-SQL&#41;](monitor/view-replicated-commands-and-information-in-distribution-database.md)    
-   [Anzeigen von Konfliktinformationen zu Mergeveröffentlichungen &#40;Replikationsprogrammierung mit Transact-SQL&#41;](./view-and-resolve-data-conflicts-for-merge-publications.md) 
-   [Messen der Latenzzeit und Überprüfen der Verbindungen bei Transaktionsreplikationen](monitor/measure-latency-and-validate-connections-for-transactional-replication.md)  
  
