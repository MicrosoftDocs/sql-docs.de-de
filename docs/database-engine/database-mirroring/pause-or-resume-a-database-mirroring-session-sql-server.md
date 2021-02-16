---
title: Anhalten und Fortsetzen einer Datenbankspiegelungssitzung
description: Hier erfahren Sie, wie Sie eine SQL Server Datenbankspiegelungssitzung mit SQL Server Management Studio oder Transact-SQL (T-SQL) anhalten und fortsetzen.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- resuming database mirroring
- database mirroring [SQL Server], sessions
- database mirroring [SQL Server], pausing
- database mirroring [SQL Server], resuming
- pausing database mirroring
ms.assetid: 05ede3b4-6abe-4442-abb7-9f5aee1d6bc0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 371331c20297f19f46384ac3f742b28448cb7459
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343720"
---
# <a name="pause-or-resume-a-database-mirroring-session-sql-server"></a>Anhalten oder Fortsetzen einer Datenbank-Spiegelungssitzung (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie Sie eine Datenbankspiegelung in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]anhalten oder fortsetzen.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Security](#Security)  
  
-   **So ReplaceThisText mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Nachverfolgung:**  [Nach dem Anhalten oder Fortsetzen der Datenbankspiegelung](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
 Eine Datenbank-Spiegelungssitzung kann jederzeit angehalten werden. Dadurch kann die Leistung bei Engpässen ggf. verbessert werden, und anschließend können Sie die angehaltene Sitzung jederzeit fortsetzen.  
  
> [!CAUTION]  
>  Nach einem erzwungenen Dienst, wenn der ursprüngliche Prinzipalserver die Verbindung wiederherstellt, wird die Spiegelung angehalten. Das Fortsetzen der Spiegelung in dieser Situation könnte auf dem ursprünglichen Prinzipalserver zu Datenverlust führen. Informationen zum Verwalten des potenziellen Datenverlusts finden Sie unter [Rollenwechsel während einer Datenbank-Spiegelungssitzung &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER-Berechtigung für die Datenbank.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 Eine Datenbank-Spiegelungssitzung können Sie auf der Seite **Spiegelung** der Datenbankeigenschaften anhalten bzw. fortsetzen.  
  
#### <a name="to-pause-or-resume-database-mirroring"></a>So halten Sie eine Datenbankspiegelung an bzw. setzen sie fort  
  
1.  Stellen Sie während einer Datenbank-Spiegelungssitzung eine Verbindung mit der Prinzipalserverinstanz her, und klicken Sie im Objekt-Explorer auf den Servernamen, um die Serverstruktur zu erweitern.  
  
2.  Erweitern Sie **Datenbanken**, und wählen Sie die Datenbank aus.  
  
3.  Klicken Sie mit der rechten Maustaste auf die Datenbank, wählen Sie **Tasks** aus, und klicken Sie dann auf **Spiegeln**. Dadurch wird die Seite **Spiegelung** im Dialogfeld **Datenbankeigenschaften** geöffnet.  
  
4.  Klicken Sie zum Anhalten der Sitzung auf **Anhalten**.  
  
     Wenn Sie in der Bestätigungsaufforderung auf **Ja** klicken, wird die Sitzung angehalten und die Schaltfläche erhält die Bezeichnung **Fortsetzen**.  
  
     Weitere Informationen über die Auswirkung des Anhaltens einer Sitzung finden Sie unter [Anhalten und Fortsetzen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/pausing-and-resuming-database-mirroring-sql-server.md).  
  
5.  Klicken Sie zum Fortsetzen der Sitzung auf **Fortsetzen**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-pause-database-mirroring"></a>So halten Sie die Datenbankspiegelung an  
  
1.  Stellen Sie für einen der Partner eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)] her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Führen Sie die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung aus:  
  
     ALTER DATABASE *Datenbankname* SET PARTNER SUSPEND  
  
     wobei *Datenbankname* für die gespiegelte Datenbank steht, deren Sitzung Sie anhalten möchten.  
  
     Im folgenden Beispiel wird die Beispieldatenbank [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] angehalten.  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET PARTNER SUSPEND;  
    ```  
  
##### <a name="to-resume-database-mirroring"></a>So setzen Sie die Datenbankspiegelung fort  
  
1.  Stellen Sie für einen der Partner eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)] her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Führen Sie die folgende Transact-SQL-Anweisung aus:  
  
     ALTER DATABASE *Datenbankname* SET PARTNER RESUME  
  
     wobei *Datenbankname* für die gespiegelte Datenbank steht, deren Sitzung Sie fortsetzen möchten.  
  
     Im folgenden Beispiel wird die Beispieldatenbank [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] angehalten.  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET PARTNER RESUME;  
    ```  
  
##  <a name="follow-up-after-pausing-or-resuming-database-mirroring"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Anhalten oder Fortsetzen der Datenbankspiegelung  
  
-   **Nach dem Anhalten der Datenbankspiegelung**  
  
     Treffen Sie auf der primären Datenbank entsprechende Vorkehrungen, um ein Überlaufen des Transaktionsprotokolls zu verhindern. Weitere Informationen finden Sie unter [Das Transaktionsprotokoll &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
-   **Nach dem Fortsetzen der Datenbankspiegelung**  
  
     Durch das Fortsetzen einer Datenbankspiegelung wird die Spiegeldatenbank in den SYNCHRONIZING-Status gesetzt. Ist die Sicherheitsstufe auf FULL festgelegt, holt der Spiegel den aktuellen Verarbeitungsstand des Prinzipals auf, und die Spiegeldatenbank geht in den SYNCHRONIZED-Status über. Zu diesem Zeitpunkt ist das Ausführen eines Failovers möglich. Ist der Zeuge vorhanden und auf ON festgelegt, ist ein automatisches Failover möglich. Bei Abwesenheit eines Zeugen ist ein manuelles Failover möglich.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Entfernen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
