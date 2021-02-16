---
title: Spiegelungsstatus (SQL Server) | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die Datenbankzustände in einer Datenbankspiegelungssitzung in SQL Server. Der Zustand spiegelt den Kommunikationsstatus, den Datenfluss und den Unterschied der Daten wider.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- states [SQL Server], database mirroring
- PENDING_FAILOVER state
- mirroring states [SQL Server]
- DISCONNECTED state
- SYNCHRONIZING state
- SYNCHRONIZED state
- SUSPENDED state
- database mirroring [SQL Server], states
ms.assetid: 90062917-74f9-471b-b49e-bc153ae1a468
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: bec627b89d9aa1265fa70627185a5f18967562ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343295"
---
# <a name="mirroring-states-sql-server"></a>Spiegelungsstatus (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Während einer Datenbank-Spiegelungssitzung befindet sich eine Datenbank stets in einem bestimmten Status (dem *Spiegelungsstatus*). Der Status der Datenbank spiegelt den Kommunikationsstatus, den Datenfluss und den Unterschied in den Daten zwischen den Partnern wider. Die Datenbank-Spiegelungssitzung nimmt denselben Status an wie die Prinzipaldatenbank.  
  
 Während der Datenbank-Spiegelungssitzung überwachen sich die Serverinstanzen gegenseitig. Die Partner verwenden den Spiegelungsstatus für die Überwachung der Datenbank. Mit Ausnahme des PENDING_FAILOVER-Status befinden sich die Prinzipal- und die Spiegeldatenbank immer im gleichen Status. Falls ein Zeuge für die Sitzung festgelegt wurde, überwacht jeder Partner den Zeugen mithilfe seines Verbindungsstatus (CONNECTED oder DISCONNECTED).  
  
 Eine Datenbank kann u. a. den folgenden Spiegelungsstatus annehmen:  
  
|Spiegelungsstatus|BESCHREIBUNG|  
|---------------------|-----------------|  
|SYNCHRONIZING|Der Inhalt der Spiegeldatenbank liegt zeitlich hinter dem Inhalt der Prinzipaldatenbank. Der Prinzipalserver sendet Protokolleinträge an den Spiegelserver, der die Änderungen auf die Spiegeldatenbank anwendet, um ein Rollforward dafür auszuführen.<br /><br /> Beim Start einer Datenbank-Spiegelungssitzung befindet sich die Datenbank im SYNCHRONIZING-Status. Der Prinzipalserver bedient die Datenbank, während der Spiegelserver versucht, dessen Stand zu erreichen.|  
|SYNCHRONIZED|Wenn der Spiegelserver den Prinzipalserver weitgehend eingeholt hat, wechselt der Spiegelungsstatus zu SYNCHRONIZED. Die Datenbank behält diesen Status so lange bei, wie der Prinzipalserver Änderungen an den Spiegelserver sendet und der Spiegelserver Änderungen auf die Spiegeldatenbank anwendet.<br /><br /> Ist die Transaktionssicherheit auf FULL festgelegt, werden sowohl das automatische als auch das manuelle Failover im Status SYNCHRONIZED unterstützt; nach einem Failover kommt es zu keinem Datenverlust.<br /><br /> Ist die Transaktionssicherheit auf OFF festgelegt, kann es auch im Status SYNCHRONIZED zu Datenverlust kommen.<br /><br /> In SQL Server Management Studio wird der Status der Datenbank als „Wird wiederhergestellt“ angezeigt. Fragen Sie die Spalte `mirroring_state_desc` in [sys.database_mirroring](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md) ab, um den tatsächlichen Status zu erhalten. |  
|SUSPENDED|Die Spiegelkopie der Datenbank ist nicht verfügbar. Die Prinzipaldatenbank wird ausgeführt, ohne Protokolle an den Spiegelserver zu senden. Diese Bedingung wird als *ungeschützt laufen* bezeichnet. Dies ist der Status nach einem Failover.<br /><br /> Eine Sitzung kann auch aufgrund von Fehlern beim Wiederholen oder wenn der Administrator die Sitzung anhält in den SUSPENDED-Status gelangen.<br /><br /> SUSPENDED ist ein persistenter Status, der das Herunterfahren und Neustarten der Partner überdauert.|  
|PENDING_FAILOVER|Dieser Status ist nur auf dem Prinzipalserver zu finden, wenn ein Failover zwar begonnen hat, der Server jedoch die Rolle eines Spiegels noch nicht übernommen hat.<br /><br /> Nach dem Initiieren des Failovers wechselt die Prinzipaldatenbank in den Status PENDING_FAILOVER, beendet schnell alle Benutzerverbindungen und übernimmt kurz danach die Rolle des Spiegels.|  
|DISCONNECTED|Die Kommunikation des Partners mit dem anderen Partner wurde unterbrochen.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Überwachen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)  
  
  
