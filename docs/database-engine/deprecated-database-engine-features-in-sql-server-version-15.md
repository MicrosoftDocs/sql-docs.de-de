---
description: Als veraltet markierte Funktionen der Datenbank-Engine in [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]
title: Als veraltet markierte Features der Datenbank-Engine in SQL Server 2019 | Microsoft-Dokumentation
titleSuffix: SQL Server 2019
ms.custom: seo-lt-2019
ms.date: 02/12/2021
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- deprecated changes 2019 [SQL Server]
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: b6a8b2edf94d74720836e02589ec8e1270f38dac
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525163"
---
# <a name="deprecated-database-engine-features-in-sql-server-2019-15x"></a>Als veraltet markierte Features der Datenbank-Engine in SQL Server 2019 (15.x)

[!INCLUDE[sqlserver2019](../includes/applies-to-version/sqlserver2019.md)]

Von [!INCLUDE [sssql19-md](../includes/sssql19-md.md)] werden keine Features außer den in früheren Versionen veralteten Features als veraltet markiert:

- [[!INCLUDE [sssql17-md](../includes/sssql17-md.md)]](deprecated-database-engine-features-in-sql-server-2017.md)
- [[!INCLUDE [sssql16-md](../includes/sssql16-md.md)]](deprecated-database-engine-features-in-sql-server-2016.md)

## <a name="deprecation-guidelines"></a>Richtlinien für die eingestellte Unterstützung

Wenn eine Funktion als veraltet markiert ist, bedeutet dies:

- Die Funktion ist ausschließlich im Wartungsmodus. Es werden keine weiteren Änderungen vorgenommen, auch solche nicht, die mit der Interoperabilität mit neuen Funktionen zu tun haben.
- Wir bemühen uns, veraltete Funktionen in zukünftigen Versionen zu belassen, um Upgrades zu vereinfachen. In seltenen Fällen kann es jedoch vorkommen, dass eine veraltete Funktion aus [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] entfernt wird, weil sie zukünftige Innovationen beschränkt.
- Für neue Entwicklungen wird empfohlen, in diesen keine veralteten Funktionen zu verwenden.      

Sie können die Nutzung als veraltet markierter Features mithilfe des [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Objektleistungsindikators „Als veraltet markierte Funktion“ oder mit den erweiterten Ereignissen `deprecation_announcement` und `deprecation_final_support` überwachen. Weitere Informationen finden Sie unter [Verwenden von SQL Server-Objekten](../relational-databases/performance-monitor/use-sql-server-objects.md).  

## <a name="query-deprecated-features"></a>Abfragen der als veraltet markierten Features

Die Werte dieser Zähler sind auch durch Ausführung der folgenden Anweisung verfügbar:  

```sql
SELECT * FROM sys.dm_os_performance_counters
WHERE object_name = 'SQLServer:Deprecated Features';
```

### <a name="see-also"></a>Weitere Informationen

- [Wichtige Änderungen an Funktionen der Datenbank-Engine in SQL Server 2019](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-version-15.md)
- [Nicht mehr unterstützte Datenbank-Engine-Funktionalität in SQL Server](../database-engine/discontinued-database-engine-functionality-in-sql-server.md)
- [Abwärtskompatibilität der SQL Server-Datenbank-Engine](./discontinued-database-engine-functionality-in-sql-server.md)