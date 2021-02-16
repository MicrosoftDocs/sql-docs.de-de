---
title: Entfernen der Datenbankspiegelung (SQL Server) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von SQL Server Management Studio oder Transact-SQL in SQL Server eine Datenbankspiegelung von einer Datenbank entfernen.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], removing
- removing database mirroring [SQL Server]
ms.assetid: bbc4d7f7-3bc7-40d6-a822-af195fe7f8c0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1dc0d50f418d0e27e8e3f52d31601b0703151fd6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337869"
---
# <a name="remove-database-mirroring-sql-server"></a>Entfernen der Datenbankspiegelung (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie Sie eine Datenbankspiegelung aus einer Datenbank in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]entfernen.  Der Datenbankbesitzer kann eine Datenbank-Spiegelungssitzung jederzeit manuell beenden, indem er die Spiegelung von der Datenbank entfernt.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Security](#Security)  
  
-   **Entfernen einer Datenbankspiegelung mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Nachverfolgung:**  [Nach dem Entfernen der Datenbankspiegelung](#FollowUp)  
  
-   [Verwandte Aufgaben](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER-Berechtigung für die Datenbank.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-remove-database-mirroring"></a>So entfernen Sie die Datenbankspiegelung  
  
1.  Stellen Sie während einer Datenbank-Spiegelungssitzung eine Verbindung mit der Prinzipalserverinstanz her, und klicken Sie im Objekt-Explorer auf den Servernamen, um die Serverstruktur zu erweitern.  
  
2.  Erweitern Sie **Datenbanken**, und wählen Sie die Datenbank aus.  
  
3.  Klicken Sie mit der rechten Maustaste auf die Datenbank, wählen Sie **Tasks** aus, und klicken Sie dann auf **Spiegeln**. Dadurch wird die Seite **Spiegelung** im Dialogfeld **Datenbankeigenschaften** geöffnet.  
  
4.  Klicken Sie im Bereich **Seite auswählen** auf **Spiegelung**.  
  
5.  Zum Entfernen der Spiegelung klicken Sie auf **Spiegeln entfernen**. Es wird eine Bestätigungsaufforderung angezeigt. Wenn Sie auf **Ja** klicken, wird die Sitzung beendet, und die Spiegelung wird aus der Datenbank entfernt.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 Zum Entfernen der Datenbankspiegelung verwenden Sie die **Datenbankeigenschaften**. Verwenden Sie die Seite **Spiegelung** im Dialogfeld **Datenbankeigenschaften** .  
  
#### <a name="to-remove-database-mirroring"></a>So entfernen Sie die Datenbankspiegelung  
  
1.  Stellen Sie für einen der Spiegelungspartner eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)] her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Führen Sie die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung aus:  
  
    ```  
    ALTER DATABASE database_name SET PARTNER OFF  
    ```  
  
     wobei *Datenbankname* für die gespiegelte Datenbank steht, deren Sitzung Sie entfernen möchten.  
  
     Im folgenden Beispiel wird die Datenbankspiegelung aus der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Beispieldatenbank entfernt.  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET PARTNER OFF;  
    ```  
  
##  <a name="follow-up-removing-database-mirroring"></a><a name="FollowUp"></a>Nächster Schritt: Entfernen der Datenbankspiegelung  
  
> [!NOTE]  
>  Weitere Informationen zu den Auswirkungen des Entfernens der Datenbankspiegelung finden Sie unter [Entfernen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).  
  
-   **Wenn Sie die Datenbankspiegelung erneut starten möchten**  
  
     Vor dem erneuten Starten der Spiegelung müssen alle Protokollsicherungen, die nach dem Entfernen der Spiegelung für die Prinzipaldatenbank erstellt wurden, auf die Spiegeldatenbank angewendet werden.  
  
-   **Wenn Sie die Datenbankspiegelung nicht erneut starten möchten**  
  
     Sie haben auch die Möglichkeit, die frühere Spiegeldatenbank wiederherzustellen. Auf der Serverinstanz, die den Spiegelserver darstellte, können Sie die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung ausführen:  
  
    ```  
    RESTORE DATABASE database_name WITH RECOVERY;  
    ```  
  
    > [!IMPORTANT]  
    >  Wenn Sie diese Datenbank wiederherstellen, sind zwei voneinander abweichende Datenbanken mit demselben Namen online. Deshalb müssen Sie sicherstellen, dass Clients nur auf eine der beiden zugreifen können (in der Regel die aktuellere Prinzipaldatenbank).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Anhalten oder Fortsetzen einer Datenbank-Spiegelungssitzung &#40;SQL Server&#41;](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)  
  
-   [Entfernen des Zeugen aus einer Datenbank-Spiegelungssitzung &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-the-witness-from-a-database-mirroring-session-sql-server.md)  
  
-   [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
-   [Beispiel: Einrichten der Datenbankspiegelung mithilfe von Zertifikaten &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Einrichten der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
