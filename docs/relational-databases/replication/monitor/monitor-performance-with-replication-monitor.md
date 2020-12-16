---
description: Überwachen der Leistung mit dem Replikationsmonitor
title: Überwachen der Leistung mit dem Replikationsmonitor | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- monitoring performance [SQL Server replication], Replication Monitor
- Log Reader Agent, monitoring
- Replication Monitor, performance
- Merge Agent, monitoring
- Queue Reader Agent, monitoring
- Snapshot Agent, monitoring
- Distribution Agent, monitoring
- monitoring performance [SQL Server replication]
ms.assetid: f212397d-1bfd-496b-a246-668952891d09
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 909670345c94444ba4883422f5d02f13757106c5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477861"
---
# <a name="monitor-performance-with-replication-monitor"></a>Überwachen der Leistung mit dem Replikationsmonitor
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Mit dem [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Replikationsmonitor können Sie die Leistung der Transaktionsreplikation und der Mergereplikation auf folgende Weisen überwachen:  
  
-   Festlegen von Warnungen und Schwellenwerten  
  
-   Anzeigen von Leistungsindikatoren  
  
-   Ermitteln der Latenzzeit mit Überwachungstokens (Transaktionsreplikation)  
  
-   Anzeigen detaillierter Synchronisierungsstatistiken (Mergereplikation)  
  
-   Anzeigen von Transaktionen und Bereitstellungszeiten (Transaktionsreplikation)  
  
## <a name="set-warnings-and-thresholds"></a>Festlegen von Schwellenwerten und Warnungen  
 Im Replikationsmonitor können Sie Warnungen für eine Reihe von Leistungszuständen aktivieren. Beim Aktivieren einer Warnung geben Sie einen Schwellenwert an. Wenn dieser Schwellenwert erreicht oder überschritten wird, wird in der **Status** -Spalte eine Warnung für das Abonnement und die Veröffentlichung angezeigt, mit der das Abonnement synchronisiert wird (es sei denn, es muss ein Problem mit einer höheren Priorität angezeigt werden). Neben der Warnung im Replikationsmonitor kann bei Erreichen eines Schwellenwerts auch ein Warnhinweis ausgelöst werden. Warnungen können für die folgenden Leistungszustände aktiviert werden:  
  
-   Die angegebene Latenzzeit (Zeit, die zwischen dem Start einer Transaktion auf dem Verleger und dem Start der entsprechenden Transaktion auf dem Abonnenten vergeht) wird überschritten.  
  
     Betrifft nur Transaktionsreplikationen. Wenn der angegebene Schwellenwert erreicht oder überschritten wird, wird als Status **Leistungskritisch** angezeigt.  
  
-   Die angegebene Synchronisierungszeit wird überschritten.  
  
     Betrifft nur Mergereplikationen. Wenn der angegebene Schwellenwert erreicht oder überschritten wird, wird als Status **Langer Mergevorgang** angezeigt. Sie können für DFÜ- und LAN-Verbindungen (Local Area Network, lokales Netzwerk) unterschiedliche Schwellenwerte angeben.  
  
-   Die angegebene Zahl von Zeilen konnte nicht innerhalb der festgelegten Zeit verarbeitet werden.  
  
     Betrifft nur Mergereplikationen. Wenn der angegebene Schwellenwert erreicht oder überschritten wird, wird als Status **Leistungskritisch** angezeigt. Sie können für DFÜ- und LAN-Verbindungen unterschiedliche Schwellenwerte angeben.  
  
 Weitere Informationen finden Sie unter [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md).  
  
## <a name="view-performance-measurements"></a>Anzeigen von Leistungsindikatoren  
 Der Replikationsmonitor zeigt bei Transaktions- und Mergereplikationen in den Spalten **Aktuelle Durchschnittsleistung** und **Die derzeit schlechteste Leistung** (für Veröffentlichungen) und in der **Leistung** -Spalte (für Abonnements) Leistungswerte an. Die Werte sind:  
  
-   Ausgezeichnet  
  
-   Gut  
  
-   Mittelmäßig  
  
-   Schlecht  
  
-   Kritisch (nur bei Transaktionsreplikation)  
  
 Welcher Wert angezeigt wird, hängt von Folgendem ab:  
  
-   Bei der Transaktionsreplikation richtet sich die Angabe zur Leistungsqualität nach dem Schwellenwert für die Latenzzeit. Wenn dieser Schwellenwert nicht festgelegt ist, wird auch kein Leistungswert angezeigt. Aus der folgenden Tabelle geht der Zusammenhang zwischen dem Schwellenwert und dem Wert für die Leistungsqualität hervor. Wenn für den Schwellenwert z. B. 60 Sekunden festgelegt ist und die tatsächliche Latenzzeit 30 Sekunden beträgt, entspricht diese Latenzzeit 50 % des Schwellenwerts, woraus sich der Wert "Gut" ergibt.  
  
    |Ausgezeichnet|Gut|Mittelmäßig|Schlecht|Kritisch|  
    |---------------|----------|----------|----------|--------------|  
    |0 – 34%|35 – 59%|60 – 84%|85 – 99%|100% +|  
  
-   Bei der Mergereplikation ist der Wert für die Leistungsqualität unabhängig von den beiden Schwellenwerten. (Der Schwellenwert für die Zeilenverarbeitung bestimmt, ob der Wert **Leistungskritisch** in der **Status** -Spalte angezeigt wird). Die Leistungsqualität wird ermittelt, indem die Leistung der einzelnen Abonnements mit der durchschnittlichen bisherigen Leistung der Veröffentlichungsabonnements mit dem gleichen Verbindungstyp (DFÜ oder LAN) verglichen wird. Der Replikationsmonitor zeigt einen Wert an, nachdem fünf Synchronisierungen mit jeweils mindestens 50 Änderungen über denselben Verbindungstyp stattgefunden haben. Wenn weniger als fünf Synchronisierungen mit 50 oder mehr Änderungen ausgeführt wurden oder wenn die letzte Synchronisierung weniger als 50 Änderungen aufweist, zeigt der Replikationsmonitor keinen Wert an.  
  
     Aus der folgenden Tabelle geht der Zusammenhang zwischen der durchschnittlichen Leistung und dem Wert für die Leistungsqualität hervor. Wenn z. B. zehn Abonnenten über eine LAN-Verbindung mit einer Durchschnittsrate von 100 Zeilen pro Sekunde synchronisiert werden und eines der Abonnements dann mit einer Rate von 125 Zeilen pro Sekunde synchronisiert wird, beläuft sich die Leistung für diese Abonnentensynchronisierung auf 125 % des Durchschnitts, was einer guten Leistung entspricht.  
  
    |Ausgezeichnet|Gut|Mittelmäßig|Schlecht|  
    |---------------|----------|----------|----------|  
    |151+%|76 – 150%|26 – 75%|0 – 25%|  
  
 Weitere Informationen zum Anzeigen von Abonnementinformationen finden Sie unter [View Information and Perform Tasks using Replication Monitor (Anzeigen von Informationen und Ausführen von Aufgaben mit dem Replikationsmonitor)](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
## <a name="determine-latency-with-tracer-tokens"></a>Ermitteln der Latenzzeit mit Überwachungstokens  
 Mithilfe der Transaktionsreplikation können Sie die Latenzzeit (Latenz) in einem System messen, indem Sie ein Token (kleine Menge von Daten) in das Transaktionsprotokoll der Veröffentlichungsdatenbank einfügen und aufzeichnen, wie lange dieses Token benötigt, bis es beim Verteiler und den Abonnenten ankommt. Mithilfe des Tokens können Sie auch feststellen, ob Daten den Verteiler oder die Abonnenten gar nicht erreichen. Weitere Informationen finden Sie unter [Messen der Wartezeit und Überprüfen der Verbindungen bei Transaktionsreplikationen](../../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)  
  
## <a name="view-detailed-synchronization-performance-for-merge-replication"></a>Anzeigen von Details zur Synchronisierungsleistung bei der Mergereplikation  
 Bei Mergereplikationen zeigt der Replikationsmonitor detaillierte Statistiken für alle Artikel an, die während einer Synchronisierung verarbeitet werden. So lässt sich diesen Statistiken z. B. die Länge der einzelnen Verarbeitungsphasen (Hochladen von Änderungen, Herunterladen von Änderungen usw.) entnehmen. Auf diese Weise können Sie besser die Tabellen identifizieren, die zu einer Verlangsamung führen, und Sie können hier auch hervorragend Leistungsprobleme im Zusammenhang mit Mergeabonnements diagnostizieren. Weitere Informationen zum Anzeigen detaillierten Statistiken finden Sie unter [View Information and Perform Tasks using Replication Monitor (Anzeigen von Informationen und Ausführen von Aufgaben mit dem Replikationsmonitor)](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
## <a name="view-transactions-and-delivery-time-for-transactional-replication"></a>Anzeigen von Transaktions- und Bereitstellungszeiten bei der Transaktionsreplikation  
 Bei Transaktionsreplikationen zeigt der Replikationsmonitor Informationen zur Anzahl der Transaktionen in der Verteilungsdatenbank an, die noch nicht an einen Abonnenten verteilt wurden, und er gibt an, wie lange die Verteilung dieser Transaktionen schätzungsweise dauert. Weitere Informationen finden Sie unter [View information and perform tasks using Replication Monitor (Anzeigen von Informationen und Ausführen von Aufgaben mit dem Replikationsmonitor)](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Überwachen der Replikation](../../../relational-databases/replication/monitor/monitoring-replication.md)   
 [Festlegen von Schwellenwerten und Warnungen im Replikationsmonitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)  
  
  
