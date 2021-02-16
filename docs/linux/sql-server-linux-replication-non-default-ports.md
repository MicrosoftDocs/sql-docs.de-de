---
title: Konfigurieren der Replikation eines Momentaufnahmeordners (vom Standard abweichende Ports)
titleSuffix: SQL Server on Linux
description: Hier erfahren Sie, wie Sie Momentaufnahmeordnerfreigaben mit vom Standard abweichenden Ports für die SQL Server-Replikation unter Linux konfigurieren.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 09/24/2018
ms.topic: article
ms.prod: sql
ms.technology: linux
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: f51dd4ebdb3a797a54eb488e8d99c02017dc70e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346484"
---
# <a name="configure-replication-with-non-default-ports-sql-server-linux"></a>Konfigurieren der Replikation mit vom Standard abweichenden Ports (SQL Server für Linux)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Sie können Replikation mit SQL Server für Linux Instanzen konfigurieren, die an einem Port lauschen, der mit der „mssql-conf“-Einstellung „network.tcpport“ konfiguriert wurde. Der Port muss während der Konfiguration an den Servernamen angehängt werden, wenn die folgenden Bedingungen zutreffen:

1. Zur Replikationseinrichtung gehört eine Instanz von SQL Server für Linux.
2. Jede Instanz (Windows oder Linux) lauscht an einem Nicht-Standardport. 

Der Servername einer Instanz kann ermittelt werden, indem @@servername für die Instanz ausgeführt wird. Verwenden Sie nicht die IP-Adresse anstelle des Servernamens. Die Verwendung der IP-Adresse für den Herausgeber, Verteiler oder Abonnenten kann zu einem Fehler führen.

> [!NOTE]
> Das Erstellen einer SQL Server-Replikation unter Linux mit anderen Ports als dem Standardport funktioniert nur mit SQL Server 2019 und höher.

## <a name="examples"></a>Beispiele

„Server1“ lauscht an Port 1500 unter Linux. Um „Server1“ als Verteiler zu konfigurieren, führen Sie `sp_adddistributor` mit `@distributor` aus. Beispiel: 

```sql
exec sp_adddistributor @distributor = 'Server1,1500'
```

„Server1“ lauscht an Port 1500 unter Linux. Um einen Verleger für den Verteiler zu konfigurieren, führen Sie `sp_adddistpublisher` mit `@publisher` aus. Beispiel:

```sql
exec sp_adddistpublisher @publisher = 'Server1,1500' ,  ,  
```

„Server2“ lauscht an Port 6549 unter Linux. Um „Server2“ als Abonnenten zu konfigurieren, führen Sie `sp_addsubscription` mit `@subscriber` aus. Beispiel:

```sql
exec sp_addsubscription @subscriber = 'Server2,6549' ,  ,  
```

„Server3“ lauscht an Port 6549 unter Windows mit dem Servernamen „Server3“ und dem Instanznamen „MSSQL2017“. Um „Server3“ als Abonnenten zu konfigurieren, führen Sie `sp_addsubscription` mit `@subscriber` aus. Beispiel:

```sql
exec sp_addsubscription @subscriber = 'Server3/MSSQL2017,6549',  ,  
```

## <a name="next-steps"></a>Nächste Schritte

[Konzepte: SQL Server-Replikation unter Linux](sql-server-linux-replication.md)

[Gespeicherte Prozeduren für die Replikation](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).

