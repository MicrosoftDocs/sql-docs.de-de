---
title: SQL Server-Replikation unter Linux
description: Hier finden Sie Informationen zur Unterstützung der SQL Server-Replikation für Instanzen von SQL Server für Linux durch SQL Server 2017 (14.x) (CU18) und höher.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: vanto
ms.date: 12/09/2019
ms.topic: article
ms.prod: sql
ms.prod_service: database-engine
ms.technology: linux
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=sqlallproducts-allversions'
ms.openlocfilehash: 3e705c463977bf355554ed1d242232649702d276
ms.sourcegitcommit: 3ea082c778f6771b17d90fb597680ed334d3e0ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88088831"
---
# <a name="sql-server-replication-on-linux"></a>SQL Server-Replikation unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[SQL Server 2017](../includes/sssqlv14-md.md)] ([CU18](https://support.microsoft.com/help/4527377)) und später unterstützen SQL Server-Replikation für Instanzen von SQL Server für Linux eingeführt.

Sie konfigurieren Replikation unter Linux mit [gespeicherten Replikationsprozeduren ](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md) von SQL Server Management Studio (SSMS).

Eine Instanz von SQL Server kann an jeder Replikationsrolle mitwirken:

* Herausgeber
* Verteiler
* Subscriber

In einem Replikationsschema können Betriebssystemplattformen gemischt und kombiniert werden. Beispielsweise kann ein Replikationsschema eine Instanz von SQL Server für Linux für Verleger und Verteiler enthalten, und die Abonnenten enthalten Instanzen von SQL Server für Windows und für Linux.

SQL Server-Instanzen für Linux können an jeder Art von Replikation beteiligt werden.

* Transaktionsreplikation
* Momentaufnahme

Ausführliche Informationen über Replikation finden Sie unter [SQL Server-Replikation](../relational-databases/replication/sql-server-replication.md).

## <a name="supported-features"></a>Unterstützte Features

Die folgenden Replikationsfeatures werden unterstützt:

* Momentaufnahmereplikation
* Transaktionsreplikation
* Replikation mit Nicht-Standardports <!--Add link to explanation-->
* Replikation mit AD-Authentifizierung
* Replikationskonfigurationen mit Windows und Linux
* Sofortige Updates für Transaktionsreplikation

## <a name="limitations"></a>Einschränkungen

Folgende Funktionen werden nicht unterstützt:

* Mergereplikation
* Peer-zu-Peer-Replikation
* Veröffentlichungen mit Oracle

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren von SQL Server-Replikation unter Linux](sql-server-linux-replication-tutorial-tsql.md)

[Beispiel: Konfigurieren von SQL Server-Replikation unter Linux](sql-server-linux-replication-configure.md)
