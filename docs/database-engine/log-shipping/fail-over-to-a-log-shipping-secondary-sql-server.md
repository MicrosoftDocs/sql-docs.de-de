---
title: Failover zu einer sekundären Datenbank für den Protokollversand
description: Hier erfahren Sie, wie Sie mithilfe von SQL Server Management Studio oder Transact-SQL ein Failover auf einen sekundären Protokollversand von SQL Server ausführen.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- primary databases [SQL Server]
- secondary data files [SQL Server], manual fail over
- log shipping [SQL Server], failover
- failover [SQL Server], log shipping
ms.assetid: edfe5d59-4287-49c1-96c9-dd56212027bc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2c6a999ee4c91842025f3f5030339a48a4a5756d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353741"
---
# <a name="fail-over-to-a-log-shipping-secondary-sql-server"></a>Failover zu einer sekundären Datenbank für den Protokollversand (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Wenn die primäre Serverinstanz ausfällt oder gewartet werden muss, kann ein Failover zu einer sekundären Datenbank für den Protokollversand ausgeführt werden.  
  
## <a name="preparing-for-a-controlled-failover"></a>Vorbereitungen für ein kontrolliertes Failover  
 Normalerweise sind die primäre und die sekundäre Datenbank nicht synchronisiert, da die primäre Datenbank nach dem letzten Sicherungsauftrag weiterhin aktualisiert wird. In manchen Fällen wurden möglicherweise auch die aktuellen Sicherungen des Transaktionsprotokolls nicht auf die sekundären Serverinstanzen kopiert, oder die Protokollsicherungen wurden zwar kopiert, jedoch noch nicht vollständig auf die sekundäre Datenbank angewendet. Es wird empfohlen, nach Möglichkeit zunächst alle sekundären Datenbanken mit der primären Datenbank zu synchronisieren.  
  
 Informationen zu Protokollversandaufträgen finden Sie unter [Informationen zum Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md).  
  
## <a name="failing-over"></a>Ausführen eines Failovers  
 So führen Sie ein Failover zu einer sekundären Datenbank aus  
  
1.  Kopieren Sie alle noch nicht kopierten Sicherungsdateien aus der Sicherungsfreigabe in den für den Kopiervorgang verwendeten Zielordner jedes sekundären Servers.  
  
2.  Wenden Sie der Reihe nach alle noch nicht angewendeten Transaktionsprotokollsicherungen auf jede sekundäre Datenbank an. Weitere Informationen finden Sie unter [Anwenden von Transaktionsprotokollsicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
3.  Sichern Sie, wenn auf die primäre Datenbank zugegriffen werden kann, das aktive Transaktionsprotokoll, und wenden Sie die Protokollsicherung auf die sekundären Datenbanken an. Möglicherweise müssen Sie für die Datenbank den Wert [Einzelbenutzermodus](../../relational-databases/databases/set-a-database-to-single-user-mode.md) festlegen, um exklusiven Zugriff zu erhalten, bevor Sie den Wiederherstellungsbefehl ausführen. Danach müssen Sie den Wert für die Datenbank zurück in den Wert für den Modus für mehrere Benutzer ändern, nachdem die Wiederherstellung abgeschlossen ist.  
  
     Wenn die ursprüngliche, primäre Serverinstanz nicht beschädigt ist, sichern Sie das Transaktionsprotokollfragment der primären Datenbank mit der Option WITH NORECOVERY. Dadurch verbleibt die Datenbank im Wiederherstellungsstatus und steht somit Benutzern nicht zur Verfügung. Sie können letztendlich auf diese Datenbank ein Rollforward ausführen, indem Sie Transaktionsprotokollsicherungen aus der primären Ersatzdatenbank anwenden.  
  
     Weitere Informationen finden Sie unter [Transaktionsprotokollsicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md).   
  
4.  Nach der Synchronisierung der sekundären Server können Sie auf Wunsch ein Failover zu einem von ihnen ausführen, indem Sie seine sekundäre Datenbank wiederherstellen und Clients zu dieser Serverinstanz umleiten. Beim Wiederherstellen wird die Datenbank in einen konsistenten Status versetzt und online geschaltet.  
  
    > [!NOTE]  
    >  Wenn Sie eine sekundäre Datenbank verfügbar machen, sollten Sie sicherstellen, dass die Metadaten konsistent mit den Metadaten der ursprünglichen, ersten Datenbank sind. Weitere Informationen finden Sie unter [Verwalten von Metadaten beim Bereitstellen einer Datenbank auf einer anderen Serverinstanz &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
5.  Nachdem Sie die sekundäre Datenbank wiederhergestellt haben, können Sie sie so konfigurieren, dass sie für die anderen sekundären Datenbanken als erste Datenbank dient.  
  
     Wenn keine andere sekundäre Datenbank verfügbar ist, siehe [Konfigurieren des Protokollversands &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Ändern der Rollen zwischen primärem und sekundärem Protokollversandserver &#40;SQL Server&#41;](../../database-engine/log-shipping/change-roles-between-primary-and-secondary-log-shipping-servers-sql-server.md)  
  
-   [Verwaltung von Anmeldenamen und Aufträgen nach einem Rollenwechsel &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Protokollversandtabellen und gespeicherte Prozeduren](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)   
 [Informationen zum Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Sicherungen des Protokollfragments &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)  
  
  
