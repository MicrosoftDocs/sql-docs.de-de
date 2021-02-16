---
title: Ändern der primären und sekundären Protokollversandrollen
description: Erfahren Sie, wie Sie die sekundäre Datenbank so konfigurieren, dass diese für Ihre Protokollversandlösungen von SQL Server als primäre Datenbank agiert.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], role changes
- secondary data files [SQL Server], roles changed between
- primary databases [SQL Server]
- initial role changes [SQL Server]
- log shipping [SQL Server], failover
- failover [SQL Server], log shipping
ms.assetid: 2d7cc40a-47e8-4419-9b2b-7c69f700e806
author: cawrites
ms.author: chadam
ms.openlocfilehash: 27d97745e5965024a824828009889e368d800b75
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353765"
---
# <a name="change-roles-between-primary-and-secondary-log-shipping-servers-sql-server"></a>Ändern der Rollen zwischen primärem und sekundärem Protokollversandserver (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Nachdem Sie für eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Protokollversandkonfiguration ein Failover zu einem sekundären Server ausgeführt haben, können Sie die sekundäre Datenbank so konfigurieren, dass sie als primäre Datenbank fungiert. Anschließend können Sie primäre und sekundäre Datenbanken je nach Bedarf austauschen.  
  
## <a name="performing-the-initial-role-change"></a>Ausführen der ersten Rollenänderung  
 Bei erstmaligem Failover zu einer sekundären Datenbank und einer anschließenden Konfiguration als neuer primärer Datenbank müssen Sie mehrere Schritte ausführen. Nachdem Sie diese anfänglichen Schritte ausgeführt haben, können Sie die Rollen zwischen der primären und sekundären Datenbank problemlos wechseln.  
  
1.  Führen Sie ein manuelles Failover von der primären Datenbank zu einer sekundären Datenbank aus. Sichern Sie das aktive Transaktionsprotokoll auf dem primären Server mit NORECOVERY. Weitere Informationen finden Sie unter [Failover zu einer sekundären Datenbank für den Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md).  
  
2.  Deaktivieren Sie den Protokollversand-Sicherungsauftrag auf dem ursprünglichen primären Server, und kopieren Sie die Aufträge auf den ursprünglichen sekundären Server, um sie dort wiederherzustellen.  
  
3.  Konfigurieren Sie für die sekundäre Datenbank (die neue primäre Datenbank) den Protokollversand mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Weitere Informationen finden Sie unter [Konfigurieren des Protokollversands &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)eingeführt. Berücksichtigen Sie folgende Punkte:  
  
    1.  Verwenden Sie dieselbe Freigabe zum Erstellen von Sicherungen, die Sie für den ursprünglichen primären Server erstellt haben.  
  
    2.  Geben Sie beim Hinzufügen der sekundären Datenbank im Dialogfeld **Einstellungen für die sekundäre Datenbank** im Feld **Sekundäre Datenbank** den Namen der ursprünglichen primären Datenbank ein.  
  
    3.  Wählen Sie im Dialogfeld **Einstellungen für die sekundäre Datenbank** die Option **Nein, die sekundäre Datenbank ist initialisiert** aus.  
  
4.  Wenn für die frühere Protokollversandkonfiguration die Protokollversandüberwachung aktiviert war, konfigurieren Sie die Protokollversandüberwachung neu, sodass die neue Protokollversandkonfiguration überwacht wird.  Wenn Sie threshold_alert_enabled auf 1 festlegen, wird bei Überschreiten von restore_threshold eine Warnung ausgelöst. Führen Sie die folgenden Befehle aus, und ersetzen Sie *database_name* durch den Namen Ihrer Datenbank:  
  
    1.  **Auf dem neuen primären Server**  
  
         Führen Sie die folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen aus:  
  
        ```sql  
        -- Statement to execute on the new primary server  
        USE msdb  
        GO  
        EXEC master.dbo.sp_change_log_shipping_secondary_database @secondary_database = N'database_name', @threshold_alert_enabled = 1;  
        GO  
        ```  
  
    2.  **Auf dem neuen sekundären Server**  
  
         Führen Sie die folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen aus:  
  
        ```sql  
        -- Statement to execute on the new secondary server  
        USE msdb  
        GO  
        EXEC master.dbo.sp_change_log_shipping_primary_database @database=N'database_name', @threshold_alert_enabled = 1;  
        GO  
        ```  
  
## <a name="swapping-roles"></a>Tauschen der Rollen  
 Nachdem Sie die oben aufgeführten Schritte für den erstmaligen Rollenwechsel ausgeführt haben, können Sie die Rollen zwischen der primären und der sekundären Datenbank mithilfe der im folgenden Abschnitt beschriebenen Schritte tauschen. Führen Sie für einen Rollenwechsel die folgenden allgemeinen Schritte aus:  
  
1.  Schalten Sie die sekundäre Datenbank online. Sichern Sie das Transaktionsprotokoll auf dem primären Server mit NORECOVERY.  
  
2.  Deaktivieren Sie den Protokollversand-Sicherungsauftrag auf dem ursprünglichen primären Server, und kopieren Sie die Aufträge auf den ursprünglichen sekundären Server, um sie dort wiederherzustellen.  
  
3.  Aktivieren Sie den Protokollversand-Sicherungsauftrag auf dem sekundären Server (dem neuen primären Server), und kopieren Sie die Aufträge auf den primären Server (den neuen sekundären Server), um sie dort wiederherzustellen.  
  
> [!IMPORTANT]  
>  Wenn sich der Wechsel zwischen sekundärer und primärer Datenbank für Benutzer und Anwendungen so reibungslos wie möglich gestalten soll, müssen Sie ggf. einige oder alle Metadaten für die Datenbank, wie z. B. Anmeldenamen und Aufträge, auf der neuen primären Serverinstanz neu erstellen. Weitere Informationen finden Sie unter [Verwalten von Metadaten beim Bereitstellen einer Datenbank auf einer anderen Serverinstanz &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Failover zu einer sekundären Datenbank für den Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
-   [Verwaltung von Anmeldenamen und Aufträgen nach einem Rollenwechsel &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Protokollversandtabellen und gespeicherte Prozeduren](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
