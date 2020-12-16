---
title: Zu XEvents gehörige Systemobjekte
description: Diese Ressourcen beziehen sich auf erweiterte Ereignisse, einschließlich der Unterstützung durch Systemobjekte, der Verwendung durch SQL Server und spezifische Aspekte von Azure SQL-Datenbank.
ms.date: 03/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: jukoesma
ms.technology: xevents
ms.topic: reference
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6a2d56121276e0a485d0f7a95f45c16c2a87d866
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481281"
---
# <a name="system-objects-that-support-extended-events"></a>Systemobjekte, die erweiterte Ereignisse unterstützen

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Der vorliegende Artikel enthält Links zu weiteren Artikeln, die sich auf erweiterte Ereignisse beziehen. In diesen Artikeln wird Folgendes behandelt:

- Systemobjekte, die Unterstützung für das Feature „Erweiterte Ereignisse“ bieten
- Teile von SQL Server selbst, die erweiterte Ereignisse nutzen
- Aspekte erweiterter Ereignisse, die für Azure SQL-Datenbank in der Cloud spezifisch sind

Die Listen sind nicht unbedingt vollständig.

## <a name="system-tables"></a>Systemtabellen

- [Erweiterte Ereignistabelle: trace_xe_action_map](../system-tables/extended-events-tables-trace-xe-action-map.md)

- [Erweiterte Ereignistabelle: trace_xe_event_map](../system-tables/extended-events-tables-trace-xe-event-map.md)

## <a name="system-catalog-views"></a>Systemkatalogsichten

- [Katalogsichten für erweiterte Ereignisse (Transact-SQL)](../system-catalog-views/extended-events-catalog-views-transact-sql.md)

- [sys.server_event_sessions (Transact-SQL)](../system-catalog-views/sys-server-event-sessions-transact-sql.md)

- [sys.server_event_session_actions (Transact-SQL)](../system-catalog-views/sys-server-event-session-actions-transact-sql.md)

- [sys.server_event_session_events (Transact-SQL)](../system-catalog-views/sys-server-event-session-events-transact-sql.md)

- [sys.server_event_session_fields (Transact-SQL)](../system-catalog-views/sys-server-event-session-fields-transact-sql.md)

- [sys.server_event_session_targets (Transact-SQL)](../system-catalog-views/sys-server-event-session-targets-transact-sql.md)

## <a name="other-system-objects"></a>Weitere Systemobjekte

- [Dynamische Verwaltungssichten für erweiterte Ereignisse](../system-dynamic-management-views/extended-events-dynamic-management-views.md)

## <a name="uses-of-extended-events-by-sql-server-itself"></a>Nutzung erweiterter Ereignisse durch SQL Server selbst

Diese Liste ist nicht vollständig.

- [Zugreifen auf Diagnoseinformationen im Protokoll der erweiterten Ereignisse](../native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md)

- [Konfigurieren erweiterter Ereignisse für Always On-Verfügbarkeitsgruppen](../../database-engine/availability-groups/windows/always-on-extended-events.md)

- [Erweiterte Ereignisse für Stretch Database](../../sql-server/stretch-database/extended-events-for-stretch-database.md)

## <a name="azure-sql-database-and-extended-events"></a>Azure SQL-Datenbank und erweiterte Ereignisse

- [Erweiterte Ereignisse in Azure SQL-Datenbank](/azure/sql-database/sql-database-xevent-db-diff-from-svr)

- [sys.database_event_session_targets (Azure SQL-Datenbank)](../system-catalog-views/sys-database-event-session-targets-azure-sql-database.md)

- [sys.database_event_session_fields (Azure SQL-Datenbank)](../system-catalog-views/sys-database-event-session-fields-azure-sql-database.md)

- [sys.database_event_session_events (Azure SQL-Datenbank)](../system-catalog-views/sys-database-event-session-events-azure-sql-database.md)

- [sys.database_event_session_action (Azure SQL-Datenbank)](../system-catalog-views/sys-database-event-session-actions-azure-sql-database.md)
