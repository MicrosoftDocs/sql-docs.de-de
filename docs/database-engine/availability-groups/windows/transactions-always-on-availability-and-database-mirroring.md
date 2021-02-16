---
description: 'Transaktionen: Always On-Verfügbarkeitsgruppen und Datenbankspiegelung'
title: 'Transaktionen: Always On-Verfügbarkeitsgruppen und Datenbankspiegelung'
descripton: Learn about cross-database and distributed transaction support for SQL Server Always On availability groups and database mirroring.
ms.custom: seo-lt-2019
ms.date: 12/11/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- cross-database transactions [SQL Server]
- transactions [database mirroring]
- Availability Groups [SQL Server], interoperability
- troubleshooting [SQL Server], cross-database transactions
ms.assetid: 9f7ed895-ad65-43e3-ba08-00d7bff1456d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 69f21f459db5e591b6934bd524515c7ce76cd3e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348998"
---
# <a name="transactions---availability-groups-and-database-mirroring"></a>Transaktionen: Always On-Verfügbarkeitsgruppen und Datenbankspiegelung
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

In diesem Artikel wird die Unterstützung für datenbankübergreifende und verteilte Transaktionen für Always On-Verfügbarkeitsgruppen und die Datenbankspiegelung beschrieben.  

## <a name="support-for-distributed-transactions"></a>Unterstützung für verteilte Transaktionen

SQL Server 2017 unterstützt verteilte Transaktionen für Datenbanken in Verfügbarkeitsgruppen. Diese Unterstützung schließt Datenbanken, die sich auf derselben Instanz von SQL Server befinden, genauso ein wie Datenbanken, die sich auf verschiedenen Instanzen von SQL Server befinden. Verteilte Transaktionen werden für Datenbanken unterstützt, die für die Datenbankspiegelung konfiguriert sind.

> [!NOTE]
> [!INCLUDE[SQL Server 2016](../../../includes/sssql16-md.md)] Service Pack 2 und höher stellt die vollständige Unterstützung für verteilte Transaktionen in Verfügbarkeitsgruppen bereit. 
> 
> In früheren [!INCLUDE[SQL Server 2016](../../../includes/sssql16-md.md)]-Versionen vor Service Pack 2 werden datenbankübergreifende verteilte Transaktionen nicht unterstützt, wenn sie eine Datenbank in einer Verfügbarkeitsgruppe enthalten. Bei diesen Transaktionen werden nur Datenbanken auf der gleichen SQL Server-Instanz verwendet.

Weitere Informationen zum Konfigurieren einer Verfügbarkeitsgruppe für verteilte Transaktionen finden Sie unter [Configure Availability Group for Distributed Transactions (Konfigurieren einer Verfügbarkeitsgruppe für verteilte Transaktionen)](configure-availability-group-for-distributed-transactions.md).

Weitere Informationen finden Sie unter:

- [DTC Administration Guide (DTC-Administratorhandbuch)](/previous-versions/windows/desktop/ms681291(v=vs.85))
- [DTC Developers Guide (DTC-Entwicklerhandbuch)](/previous-versions/windows/desktop/ms679938(v=vs.85))
- [DTC Programmers Reference (DTC-Referenz für Programmierer)](/previous-versions/windows/desktop/ms686108(v=vs.85))

## <a name="sql-server-2016-sp1-and-before-support-for-cross-database-transactions-within-the-same-sql-server-instance"></a>SQL Server 2016 SP1 und früher: Unterstützung für datenbankübergreifende Transaktionen innerhalb derselben SQL Server-Instanz  

In SQL Server 2016 SP1 und früher werden datenbankübergreifende Transaktionen innerhalb derselben SQL Server-Instanz für Verfügbarkeitsgruppen nicht unterstützt. Keine zwei Datenbanken, die an einer datenbankübergreifenden Transaktion beteiligt sind, dürfen von derselben SQL Server-Instanz gehostet werden, wenn sich eine oder beide Datenbanken in einer Verfügbarkeitsgruppe befinden. Diese Einschränkung gilt auch, wenn diese Datenbanken Teil der gleichen Verfügbarkeitsgruppe sind.  
  
Datenbankübergreifende Transaktionen werden für die Datenbankspiegelung ebenfalls nicht unterstützt.  
  
##  <a name="sql-server-2016-sp1-and-before-support-for-distributed-transactions"></a><a name="dtcsupport"></a> SQL Server 2016 SP1 und früher: Unterstützung für verteilte Transaktionen  
Verteilte Transaktion werden von Verfügbarkeitsgruppen unterstützt, wenn Datenbanken von unterschiedlichen SQL Server-Instanzen gehostet werden. Dies gilt auch für verteilte Transaktionen zwischen SQL Server-Instanzen und einem anderen DTC-konformen Server.  
 
Microsoft Distributed Transaction Coordinator (MSDTC oder DTC) ist ein Windows-Dienst, der eine Transaktionsinfrastruktur für verteilte Systeme bereitstellt. Mit MSDTC können Clientanwendungen mehrere Datenquellen in eine Transaktion aufnehmen. Für diese Transaktion wird dann auf allen Servern, die Teil der Transaktion sind, ein Commit ausgeführt. Sie können MSDTC beispielsweise dazu nutzen, um Transaktionen zu koordinieren, die mehrere Datenbanken auf verschiedenen Servern umfassen.

Mit der neuen Funktion von SQL Server 2016 können Sie verteilte Transaktionen selbst dann verwenden, wenn mindestens eine Datenbank in der Transaktion Teil zu einer Verfügbarkeitsgruppe gehören. Vor SQL Server 2016 wurden verteilte Transaktionen für Datenbanken in Verfügbarkeitsgruppen nicht unterstützt. In SQL Server 2016 können Sie einen Ressourcen-Manager pro Datenbank registrieren. Dank dieser neuen Funktion können verteilte Transaktionen jetzt auch Datenbanken in Verfügbarkeitsgruppen berücksichtigen.
  
 Die folgenden Anforderungen müssen erfüllt werden:  
  
-   Verfügbarkeitsgruppen müssen auf Windows Server 2012 R2 oder höher ausgeführt werden. Für Windows Server 2012 R2 müssen Sie das Update in KB3090973 installieren, das unter [https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973) verfügbar ist.  
  
-   Verfügbarkeitsgruppen müssen mit dem Befehl **CREATE AVAILABILITY GROUP** und der Klausel **WITH DTC\_SUPPORT = PER_DB** erstellt werden. Zurzeit können Sie eine vorhandene Verfügbarkeitsgruppe nicht ändern.  

- Alle SQL Server-Instanzen, die in die Verfügbarkeitsgruppe einbezogen werden, müssen SQL Server 2016 oder höher entsprechen.
 
 ## <a name="non-support-for-distributed-transactions"></a>Keine Unterstützung für verteilte Transaktionen
 Besondere Fälle, in denen verteilte Transaktionen nicht unterstützt werden:
 
 - Wenn sich in SQL Server 2016 SP1 und früher mehr als eine Datenbank, die an der Transaktion beteiligt ist, in der gleichen Verfügbarkeitsgruppe befindet.
 
 - Wenn sich in SQL Server 2016 SP1 und früher mindestens eine Datenbank in einer Verfügbarkeitsgruppe befindet und sich eine andere Datenbank auf der gleichen SQL Server-Instanz befindet. 
 
 - Wenn die Verfügbarkeitsgruppe nicht mit der Aktivierung von verteilten Transaktionen erstellt wurde.
 
 - Datenbankspiegelung
 
 > [!IMPORTANT]
 > Bestimmen Sie die geeigneten Standardergebnisse von Transaktionen, die für Ihre Umgebung von DTC nicht aufgelöst werden können.  Informationen zum Konfigurieren des Standardergebnisses finden Sie unter [Lösung für unklare Transaktion (Serverkonfigurationsoption)](../../../database-engine/configure-windows/in-doubt-xact-resolution-server-configuration-option.md).
  
## <a name="example-scenario-with-database-mirroring"></a>Beispielszenario mit Datenbankspiegelung  
 Im folgenden Beispiel zur Datenbankspiegelung wird verdeutlicht, wie eine logische Inkonsistenz auftreten kann. In diesem Beispiel fügt eine Anwendung über eine datenbankübergreifende Transaktion zwei Datenzeilen ein: Eine Zeile wird in eine Tabelle in einer gespiegelten Datenbank (A) eingefügt, und die andere Zeile wird in eine Tabelle in einer anderen Datenbank (B) eingefügt. Datenbank A wird im Modus für hohe Sicherheit mit automatischem Failover gespiegelt. Während des Commits der Transaktion fällt Datenbank A aus. Die Spiegelungssitzung führt automatisch ein Failover zum Spiegel von Datenbank A aus.  
  
 Nach dem Failover kann ein Commit der datenbankübergreifenden Transaktion möglicherweise erfolgreich für Datenbank B ausgeführt werden, jedoch nicht für die Datenbank, für die der Failover ausgeführt wurde. Dies wäre z.B. der Fall, wenn der ursprüngliche Prinzipalserver für Datenbank A nicht das Protokoll für die datenbankübergreifende Transaktion vor dem Ausfall an den Spiegelserver gesendet hätte. Nach dem Failover wäre diese Transaktion auf dem neuen Prinzipalserver nicht vorhanden. Datenbanken A und B würden inkonsistent werden, da die in Datenbank B eingefügten Daten erhalten bleiben, während die in Datenbank A eingefügten Daten verloren gingen.  
  
 Ein vergleichbares Szenario kann bei einer MS DTC-Transaktion auftreten. Angenommen, der neue Prinzipal setzt sich nach dem Failover mit MS DTC in Verbindung. Der neue Prinzipalserver ist MS DTC jedoch nicht bekannt, deshalb beendet er alle Transaktionen, für die "ein Commit vorbereitet wird", für die in anderen Datenbanken jedoch bereits anscheinend ein Commit ausgeführt wurde.  
  
> [!NOTE]  
>  Es besteht keine Unterstützung für die Verwendung der Datenbankspiegelung mit DTC oder die Verwendung von Verfügbarkeitsgruppen mit DTC auf nicht in diesem Artikel genehmigte Weise.  Dies bedeutet nicht, dass Aspekte des Produkts, die nicht mit DTC in Zusammenhang stehen, nicht unterstützt werden; Probleme die aus der unsachgemäßen Verwendung von verteilten Transaktionen herrühren, werden jedoch nicht unterstützt.  
  
## <a name="next-steps"></a>Nächste Schritte  
 [Always On-Verfügbarkeitsgruppen: Interoperabilität &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)  
  
