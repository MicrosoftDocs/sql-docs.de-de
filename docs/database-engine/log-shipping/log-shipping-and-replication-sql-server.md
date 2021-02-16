---
title: Protokollversand und Replikation (SQL Server) | Microsoft-Dokumentation
description: Erfahren Sie, wie der Protokollversand das Transaktionsprotokoll von jedem Einfüge-, Aktualisierungs- oder Löschvorgang, der an der primären Datenbank vorgenommen wird, auf die sekundäre Datenbank anwendet.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], log shipping and
- log shipping [SQL Server], replication and
ms.assetid: 132bebfd-0206-4d23-829a-b38e5ed17bc9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 96f64ac1ade48962f5f8dd58f0af6f0a7f662b5c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346972"
---
# <a name="log-shipping-and-replication-sql-server"></a>Protokollversand und Replikation (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Beim Protokollversand werden zwei Kopien einer einzigen Datenbank verwendet, die normalerweise auf verschiedenen Computern gespeichert sind. Für die Clients ist immer nur eine Kopie der Datenbank verfügbar. Diese Kopie wird als primäre Datenbank bezeichnet. Updates, die von Clients an der primären Datenbank vorgenommen werden, werden durch den Protokollversand auf die andere Kopie der Datenbank übertragen, die als sekundäre Datenbank bezeichnet wird. Beim Protokollversand wird das Transaktionsprotokoll von jedem Einfüge-, Update- oder Löschvorgang, der an der primären Datenbank vorgenommen wird, auf die sekundäre Datenbank angewandt.  
  
 Der Protokollversand kann zusammen mit der Replikation verwendet werden, wobei das folgende Verhalten auftritt:  
  
-   Die Replikation wird nicht fortgesetzt, nachdem ein Failover des Protokollversands aufgetreten ist. Wenn ein Failover auftritt, stellen die Replikations-Agents keine Verbindung mit der sekundären Datenbank her, sodass keine Replikation der Transaktionen auf die Abonnenten erfolgt. Wenn ein Failback zur primären Datenbank auftritt, wird die Replikation fortgesetzt. Alle Transaktionen, die beim Protokollversand von der sekundären Datenbank zurück zur primären Datenbank kopiert werden, werden zu den Abonnenten repliziert.  
  
-   Wenn die primäre Datenbank dauerhaft verloren geht, kann die sekundäre Datenbank umbenannt werden, sodass die Replikation fortgesetzt werden kann. Im weiteren Verlaufs dieses Themas werden die Anforderungen und Prozeduren zur Behandlung dieses Falls beschrieben. Das bereitgestellte Beispiel bezieht sich auf die Veröffentlichungsdatenbank, weil bei dieser Datenbank der Protokollversand am häufigsten auftritt. Ein gleichartiger Prozess kann jedoch auch auf Abonnement- und Verteilungsdatenbanken angewendet werden.  
  
 Informationen zum Wiederherstellen von Datenbanken, die an der Replikation beteiligt sind, ohne dass dazu die Replikation neu konfiguriert werden muss, finden Sie unter [Sichern und Wiederherstellen von replizierten Datenbanken](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md).  
  
> [!NOTE]  
>  Es wird empfohlen, anstelle des Protokollversands die Datenbankspiegelung zu verwenden, um die Verfügbarkeit der Veröffentlichungsdatenbank sicherzustellen. Weitere Informationen finden Sie unter [Datenbankspiegelung und Replikation &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md).  
  
## <a name="requirements-and-procedures-for-replicating-from-the-secondary-if-the-primary-is-lost"></a>Anforderungen und Prozeduren zum Replizieren von der sekundären Datenbank beim Ausfall der primären Datenbank  
 Beachten Sie die folgenden Anforderungen und Überlegungen:  
  
-   Wenn eine primäre Datenbank mehrere Veröffentlichungsdatenbanken enthält, muss der Protokollversand aller Veröffentlichungsdatenbanken zur selben sekundären Datenbank erfolgen.  
  
-   Der Installationspfad der Sekundärserverinstanz muss dem der primären Datenbank entsprechen. Die Speicherorte der Benutzerdatenbank auf dem Sekundärserver müssen denen auf dem Primärserver entsprechen.  
  
-   Sichern Sie den Diensthauptschlüssel auf der primären Datenbank. Dieser Schlüssel wird in der sekundären Datenbank wiederhergestellt. Weitere Informationen finden Sie unter [BACKUP SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/backup-service-master-key-transact-sql.md).  
  
-   Der Protokollversand garantiert keinen Schutz vor Datenverlust. Ein Ausfall der primären Datenbank kann zum Verlust der Daten führen, die noch nicht gesichert wurden oder deren Sicherungskopien bei dem Ausfall verloren gehen.  
  
### <a name="log-shipping-with-transactional-replication"></a>Protokollversand mit Transaktionsreplikation  
 Bei der Transaktionsreplikation hängt das Verhalten des Protokollversands von der **sync with backup** -Option ab. Diese Option kann für die Veröffentlichungsdatenbank und für die Verteilungsdatenbank festgelegt werden; beim Protokollversand für den Verleger ist nur die Einstellung für die Veröffentlichungsdatenbank relevant.  
  
 Wenn Sie diese Option für die Veröffentlichungsdatenbank festlegen, stellen Sie sicher, dass Transaktionen erst an die Verteilungsdatenbank übermittelt werden, wenn Sie in der Veröffentlichungsdatenbank gesichert wurden. Die neueste Sicherung der Veröffentlichungsdatenbank kann dann auf dem Sekundärserver wiederhergestellt werden, und es besteht keine Möglichkeit, dass die Verteilungsdatenbank Transaktionen enthält, die in der wiederhergestellten Veröffentlichungsdatenbank nicht vorhanden sind. Mit dieser Option wird die Konsistenz zwischen dem Verleger, dem Verteiler und den Abonnenten sichergestellt, wenn ein Failover des Verlegers zu einem Sekundärserver erfolgt. Latenzzeit und Durchsatz sind betroffen, da Transaktionen erst an die Verteilungsdatenbank übermittelt werden können, nachdem sie auf dem Verleger gesichert wurden. Wenn Ihre Anwendung diese Latenzzeit tolerieren kann, sollten Sie diese Option für die Veröffentlichungsdatenbank festlegen. Wenn die **sync with backup** -Option nicht festgelegt ist, können Abonnenten Änderungen empfangen, die nicht mehr in der wiederhergestellten Datenbank auf dem Sekundärserver enthalten sind. Weitere Informationen finden Sie unter [Strategien zum Sichern und Wiederherstellen einer Momentaufnahme- und Transaktionsreplikation](../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md).  
  
 **Konfigurieren der Transaktionsreplikation und des Protokollversands mit der "sync with backup"-Option**  
  
1.  Wenn die "sync with backup"-Option nicht für die Veröffentlichungsdatenbank festgelegt ist, führen Sie `sp_replicationdboption '<publicationdatabasename>', 'sync with backup', 'true'`aus. Weitere Informationen finden Sie unter [sp_replicationdboption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md).  
  
2.  Konfigurieren Sie den Protokollversand für die Veröffentlichungsdatenbank. Weitere Informationen finden Sie unter [Konfigurieren des Protokollversands &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)eingeführt.  
  
3.  Bei einem Ausfall des Verlegers stellen Sie das letzte Protokoll der Datenbank mithilfe der Option KEEP_REPLICATION von RESTORE LOG auf dem Sekundärserver wieder her. Damit werden alle Replikationseinstellungen für die Datenbank beibehalten. Weitere Informationen finden Sie unter [Failover zu einer sekundären Datenbank für den Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) und [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
4.  Stellen Sie die **msdb** -Datenbank und die **master** -Datenbanken von der Primärdatenbank zur sekundären Datenbank wieder her. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von Systemdatenbanken &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). Wenn die primäre Datenbank auch ein Verteiler war, stellen Sie die Verteilungsdatenbank von der primären zur sekundären Datenbank wieder her.  
  
     Diese Datenbanken müssen im Hinblick auf die Replikationskonfiguration und auf die Einstellungen mit der Veröffentlichungsdatenbank auf dem Primärserver konsistent sein.  
  
5.  Benennen Sie auf dem Sekundärserver den Computer um, und benennen Sie dann die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz so um, dass ihr Name zum Primärservernamen passt. Informationen zum Umbenennen des Computers finden Sie in der Windows-Dokumentation. Informationen zum Umbenennen des Servers finden Sie unter [Umbenennen eines Computers, der eine eigenständige Instanz von SQL Server hostet](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) und [Umbenennen einer SQL Server-Failoverclusterinstanz](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md).  
  
6.  Stellen Sie auf dem Sekundärserver den Diensthauptschlüssel wieder her, der von der primären Datenbank gesichert wurde. Weitere Informationen finden Sie unter [RESTORE SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-service-master-key-transact-sql.md).  
  
 **Konfigurieren der Transaktionsreplikation und des Protokollversands ohne die "sync with backup"-Option**  
  
1.  Konfigurieren Sie den Protokollversand für die Veröffentlichungsdatenbank. Weitere Informationen finden Sie unter [Konfigurieren des Protokollversands &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)eingeführt.  
  
2.  Bei einem Ausfall des Verlegers stellen Sie das letzte Protokoll der Datenbank mithilfe der Option KEEP_REPLICATION von RESTORE LOG auf dem Sekundärserver wieder her. Damit werden alle Replikationseinstellungen für die Datenbank beibehalten. Weitere Informationen finden Sie unter [Failover zu einer sekundären Datenbank für den Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) und [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
3.  Stellen Sie die **msdb** -Datenbank und die **master** -Datenbanken von der Primärdatenbank zur sekundären Datenbank wieder her. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von Systemdatenbanken &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). Wenn die primäre Datenbank auch ein Verteiler war, stellen Sie die Verteilungsdatenbank von der primären zur sekundären Datenbank wieder her.  
  
     Diese Datenbanken müssen im Hinblick auf die Replikationskonfiguration und auf die Einstellungen mit der Veröffentlichungsdatenbank auf dem Primärserver konsistent sein.  
  
4.  Benennen Sie auf dem Sekundärserver den Computer um, und benennen Sie dann die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz so um, dass ihr Name zum Primärservernamen passt. Informationen zum Umbenennen des Computers finden Sie in der Windows-Dokumentation. Informationen zum Umbenennen des Servers finden Sie unter [Umbenennen eines Computers, der eine eigenständige Instanz von SQL Server hostet](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) und [Umbenennen einer SQL Server-Failoverclusterinstanz](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md).  
  
     Sie erhalten möglicherweise eine Fehlermeldung vom Protokolllese-Agent, die besagt, dass die Veröffentlichungsdatenbank und die Verteilungsdatenbank nicht synchronisiert sind.  
  
5.  Stellen Sie auf dem Sekundärserver den Diensthauptschlüssel wieder her, der von der primären Datenbank gesichert wurde. Weitere Informationen finden Sie unter [RESTORE SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-service-master-key-transact-sql.md).  
  
6.  Führen Sie **sp_replrestart** aus. Diese gespeicherte Prozedur kann dazu verwendet werden, den Protokolllese-Agent zu zwingen, alle vorhergehenden replizierten Transaktionen im Protokoll der Veröffentlichungsdatenbank zu ignorieren. Transaktionen, die nach dem Beenden der gespeicherten Prozedur angewendet werden, werden vom Protokolllese-Agent verarbeitet. Weitere Informationen finden Sie unter [sp_replrestart &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replrestart-transact-sql.md).  
  
7.  Starten Sie den Protokolllese-Agent nach der erfolgreichen Ausführung der gespeicherten Prozedur neu. Weitere Informationen finden Sie unter [Starten und Beenden eines Replikations-Agents &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
8.  Transaktionen, die bereits an den Abonnenten verteilt wurden, können evtl. auf dem Verleger angewendet werden. Um sicherzustellen, dass der Verteilungs-Agent nicht zu einem Fehler führt, wenn versucht wird, diese Transaktionen erneut auf einen Abonnenten anzuwenden, müssen Sie das Agentprofil **Fortsetzen bei Datenkonsistenzfehlern** angeben.  
  
### <a name="log-shipping-with-merge-replication"></a>Protokollversand mit Mergereplikation  
 Führen Sie die Schritte in der nachstehenden Prozedur aus, um die Mergereplikation und den Protokollversand zu konfigurieren.  
  
 **So konfigurieren Sie die Mergereplikation und den Protokollversand**  
  
1.  Konfigurieren Sie den Protokollversand für die Veröffentlichungsdatenbank. Weitere Informationen finden Sie unter [Konfigurieren des Protokollversands &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)eingeführt.  
  
2.  Bei einem Ausfall des Verlegers benennen Sie den Computer auf dem Sekundärserver um, und weisen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz dann einen Namen zu, der dem Namen des primären Servers entspricht. Informationen zum Umbenennen des Computers finden Sie in der Windows-Dokumentation. Informationen zum Umbenennen des Servers finden Sie unter [Umbenennen eines Computers, der eine eigenständige Instanz von SQL Server hostet](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) und [Umbenennen einer SQL Server-Failoverclusterinstanz](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md).  
  
3.  Stellen Sie das letzte Protokoll der Datenbank mithilfe der Option KEEP_REPLICATION von RESTORE LOG auf dem Sekundärserver wieder her. Damit werden alle Replikationseinstellungen für die Datenbank beibehalten. Weitere Informationen finden Sie unter [Failover zu einer sekundären Datenbank für den Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) und [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
4.  Stellen Sie die **msdb** -Datenbank und die **master** -Datenbanken von der Primärdatenbank zur sekundären Datenbank wieder her. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von Systemdatenbanken &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). Wenn die primäre Datenbank auch ein Verteiler war, stellen Sie die Verteilungsdatenbank von der primären zur sekundären Datenbank wieder her.  
  
     Diese Datenbanken müssen im Hinblick auf die Replikationskonfiguration und auf die Einstellungen mit der Veröffentlichungsdatenbank auf dem Primärserver konsistent sein.  
  
5.  Stellen Sie auf dem Sekundärserver den Diensthauptschlüssel wieder her, der von der primären Datenbank gesichert wurde. Weitere Informationen finden Sie unter [RESTORE SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-service-master-key-transact-sql.md).  
  
6.  Synchronisieren Sie die Veröffentlichungsdatenbank mit einer oder mehreren Abonnementdatenbanken. Das ermöglicht Ihnen das Hochladen jener Änderungen, die zuvor in der Veröffentlichungsdatenbank zwar vorgenommen, aber in der wiederhergestellten Sicherung nicht vorhanden sind. Welche Daten dabei hochgeladen werden können, hängt davon ab, wie die Veröffentlichung gefiltert wird:  
  
    -   Wird die Veröffentlichung gar nicht gefiltert, sollten Sie die Veröffentlichungsdatenbank durch Synchronisieren mit einem aktuellen Abonnenten auf den neuesten Stand bringen.  
  
    -   Wenn die Veröffentlichung gefiltert ist, können Sie möglicherweise die Veröffentlichungsdatenbank nicht auf den aktuellen Stand bringen. Angenommen, eine Tabelle ist so partitioniert, dass jedes Abonnement nur die Kundendaten für eine der folgenden Regionen erhält: Nord, Ost, Süd und West. Wenn für jede Datenpartition mindestens ein Abonnent vorhanden ist, würde es reichen, die Veröffentlichungsdatenbank mit einem Abonnenten für jede Partition zu synchronisieren, um sie auf den neuesten Stand zu bringen. Wenn aber beispielsweise die Daten in der Partition West auf keinen Abonnenten repliziert wurden, können diese Daten auf dem Verleger nicht auf den aktuellen Stand gebracht werden. In diesem sollten Sie alle Abonnements initialisieren, sodass die Daten auf dem Verleger und den Abonnenten zusammengeführt werden. Weitere Informationen finden Sie unter [Erneutes Initialisieren von Abonnements](../../relational-databases/replication/reinitialize-subscriptions.md).  
  
     Wenn Sie die Veröffentlichungsdatenbank mit einem Abonnenten synchronisieren, auf dem eine frühere Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]ausgeführt wird, kann das Abonnement nicht anonym sein – es muss sich um ein Clientabonnement oder ein Serverabonnement handeln (in früheren Versionen als lokales Abonnement bzw. globales Abonnement bezeichnet). Weitere Informationen finden Sie unter [Synchronisieren von Daten](../../relational-databases/replication/synchronize-data.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server-Replikation](../../relational-databases/replication/sql-server-replication.md)   
 [Informationen zum Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Datenbankspiegelung und Replikation &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)  
  
  
