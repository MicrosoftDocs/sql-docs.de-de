---
title: 'Always On-Verfügbarkeitsgruppen: Interoperabilität'
description: Informationen zu den verschiedenen Features, die mit Always On-Verfügbarkeitsgruppen (nicht) funktionieren
ms.custom: seodec18
ms.date: 04/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
helpviewer_keywords:
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], interoperability
ms.assetid: daf87f90-2623-42ca-912c-b8f07d210510
author: cawrites
ms.author: chadam
ms.openlocfilehash: fea5426d48b9808612062fccae4cc7c1228a2e1d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343941"
---
# <a name="always-on-availability-groups-interoperability-sql-server"></a>Always On-Verfügbarkeitsgruppen: Interoperabilität (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

In diesem Artikel wird die Interoperabilität von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] mit anderen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Features dokumentiert.

## <a name="features-that-interoperate-with-always-on-availability-groups"></a><a name="Interop"></a> Features, die mit Always On-Verfügbarkeitsgruppen zusammenwirken

In der folgenden Tabelle werden [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Features aufgeführt, die eine Interoperabilität mit [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] aufweisen. Ein Link in der Spalte **Weitere Informationen** weist darauf hin, dass zusätzliche Überlegungen zur Interoperabilität einer bestimmten Funktion vorhanden sind.

|Funktion|Weitere Informationen|
|:------|:---------------|
|Erfassung geänderter Daten|[Replikation, Änderungsnachverfolgung, Change Data Capture und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)|
|Änderungsnachverfolgung|[Replikation, Änderungsnachverfolgung, Change Data Capture und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)|
|Eigenständige Datenbanken|[Eigenständige Datenbanken bei Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/contained-databases-with-always-on-availability-groups-sql-server.md)|
|Datenbankverschlüsselung|[Verschlüsselte Datenbanken bei Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/encrypted-databases-with-always-on-availability-groups-sql-server.md)|
|Datenbank-Momentaufnahmen|[Datenbankmomentaufnahmen bei Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/database-snapshots-with-always-on-availability-groups-sql-server.md)|
|FILESTREAM und FileTable|[FILESTREAM und FileTable bei Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/filestream-and-filetable-with-always-on-availability-groups-sql-server.md)|
|Volltextsuche|Hinweis: Volltextindizes werden mit sekundären Always On-Datenbanken synchronisiert.|
|Protokollversand|[Voraussetzungen für das Migrieren vom Protokollversand zu Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md)|
|Remote Blob Store (RBS)|[Remote Blob Store &#40;RBS&#41; und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remote-blob-store-rbs-and-always-on-availability-groups-sql-server.md)|
|Replikation|[Konfigurieren der Replikation für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)<br /><br /> [Warten einer Always On-Veröffentlichungsdatenbank &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md)<br /><br /> [Replikation, Änderungsnachverfolgung, Change Data Capture und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)<br /><br /> [Replikationsabonnenten und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)|
|Analysis Services|[Analysis Services mit Always On-Verfügbarkeitsgruppen](../../../database-engine/availability-groups/windows/analysis-services-with-always-on-availability-groups.md)|
|Reporting Services|Verwenden Sie schreibgeschützte sekundäre Replikate als Berichtsdatenquelle, und reduzieren Sie die Last des primären Lese-/Schreibreplikats.<br /><br /> [Reporting Services mit Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/reporting-services-with-always-on-availability-groups-sql-server.md)|
|Service Broker|[Service Broker mit Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/service-broker-with-always-on-availability-groups-sql-server.md)|
|SQL Server-Agent|&nbsp;|

## <a name="features-that-interoperate-with-always-on-availability-groups-with-restrictions"></a><a name="restrictions"></a> Features, die mit Einschränkungen mit Always On-Verfügbarkeitsgruppen zusammenwirken

Die folgenden Features wirken mit [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] unter bestimmten Einschränkungen zusammen. Weitere Informationen finden Sie in den verknüpften Themen.

- Datenbankübergreifende Transaktionen/verteilte Transaktionen ([!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] und Windows Server 2016). Weitere Informationen finden Sie unter [Datenbankübergreifende Transaktionen und verteilte Transaktionen für Always On-Verfügbarkeitsgruppen oder Datenbankspiegelung &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).
- Der [Systemdatensammler für Abfragestatistiken](../../../relational-databases/data-collection/system-data-collection-set-reports.md#Query) kann nicht zuverlässig in einer Umgebung mit nicht lesbaren sekundären Replikaten ausgeführt werden. Zur Verwendung des Systemdatensammler für Abfragestatistiken müssen Sie für alle sekundären Verfügbarkeitsgruppenreplikate den [Lesezugriff](configure-read-only-access-on-an-availability-replica-sql-server.md) erlauben. 

## <a name="features-that-do-not-interoperate-with-always-on-availability-groups"></a><a name="NoInterop"></a> Funktionen, die nicht mit Always On-Verfügbarkeitsgruppen zusammenwirken

[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] funktioniert nicht in Verbindung mit folgenden Funktionen:

- Datenbankspiegelung Weitere Informationen finden Sie unter [Datenbankübergreifende Transaktionen und verteilte Transaktionen für Always On-Verfügbarkeitsgruppen oder Datenbankspiegelung &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).

## <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte

- **Blogs:**

  [Migration Guide: Migrating to SQL Server 2012 Failover Clustering and Availability Groups from Prior Clustering and Mirroring Deployments (Leitfaden: Migrieren zu SQL Server 2012-Failoverclustering und -Verfügbarkeitsgruppen von vorherigen Clustering- und Spiegelungsbereitstellungen)](/archive/blogs/sqlalwayson/now-available-migration-guide-migrating-to-sql-server-2012-failover-clustering-and-availability-groups-from-prior-clustering-and-mirroring-deployments)
  [SQL Server Always On Team Blogs: The official SQL Server Always On Team Blog (SQL Server Always On-Teamblog: der offizielle SQL Server Always On-Teamblog)](/archive/blogs/sqlalwayson/)
  [CSS SQL Server Engineers Blogs (Blogs der SQL Server-Engineers für CSS)](/archive/blogs/psssql/)

- **Whitepaper:**

  [Migration Guide: Migrating to Always On Availability Groups from Prior Deployments Combining Database Mirroring and Log Shipping (Leitfaden: Migrieren zu Always On-Verfügbarkeitsgruppen von vorherigen Bereitstellungen mit Datenbankspiegelung und Protokollversand)](/previous-versions/sql/sql-server-2012/jj635217(v=msdn.10))
  [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Leitfaden zu Always On-Lösungen für Microsoft SQL Server zur Bereitstellung von Hochverfügbarkeit und Notfallwiederherstellung)](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))
  [Microsoft White Papers for SQL Server 2012 (Microsoft-Whitepapers zu SQL Server 2012)](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)
  [SQL Server Customer Advisory Team Whitepapers (Whitepapers des Beratungsteams für SQL Server)](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)

## <a name="see-also"></a>Weitere Informationen

[Übersicht über Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)
[Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)