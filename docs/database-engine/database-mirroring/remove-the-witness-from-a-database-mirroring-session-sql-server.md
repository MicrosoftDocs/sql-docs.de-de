---
title: Entfernen des Zeugen für die Datenbankspiegelung
description: In diesem Artikel wird beschrieben, wie ein Zeuge aus einer Datenbankspiegelungssitzung mit SQL Server Management Studio (SSMS) oder Transact-SQL (T-SQL) entfernt wird.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- witness [SQL Server], turning off
- witness [SQL Server], removing
- database mirroring [SQL Server], witness
ms.assetid: f3ce7afc-8936-4d35-80ce-d0f8fbc318d3
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 0f74b78045295dc045ea3bbf761adc7955bcdf76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337849"
---
# <a name="remove-the-witness-from-a-database-mirroring-session-sql-server"></a>Entfernen des Zeugen aus einer Datenbank-Spiegelungssitzung (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie Sie einen Zeugen in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]aus einer Datenbankspiegelungssitzung entfernen. Der Datenbankbesitzer kann den Zeugen für eine Datenbank-Spiegelungssitzung jederzeit während einer Datenbank-Spiegelungssitzung deaktivieren.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Security](#Security)  
  
-   **Entfernen des Zeugen mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Nachverfolgung:**  [Nach dem Entfernen des Zeugen](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER-Berechtigung für die Datenbank.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-remove-the-witness"></a>So entfernen Sie den Zeugen  
  
1.  Stellen Sie eine Verbindung zur Prinzipalserverinstanz her, und klicken Sie im Bereich **Objekt-Explorer** auf den Servernamen, um die Serverstruktur zu erweitern.  
  
2.  Erweitern Sie **Datenbanken**, und wählen Sie die Datenbank aus, deren Zeuge entfernt werden soll.  
  
3.  Klicken Sie mit der rechten Maustaste auf die Datenbank, wählen Sie **Tasks** aus, und klicken Sie dann auf **Spiegeln**. Dadurch wird die Seite **Spiegelung** im Dialogfeld **Datenbankeigenschaften** geöffnet.  
  
4.  Zum Entfernen des Zeugen löschen Sie seine Servernetzwerkadresse aus dem Feld **Zeuge** .  
  
    > [!NOTE]  
    >  Wenn Sie vom Modus für hohe Sicherheit mit automatischem Failover zum Modus zur hohe Leistung wechseln, wird das Feld **Zeuge** automatisch gelöscht.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-remove-the-witness"></a>So entfernen Sie den Zeugen  
  
1.  Stellen Sie für eine der Partnerserverinstanzen eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)] her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Führen Sie die folgende Anweisung aus:  
  
     [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md) *Datenbankname* SET WITNESS OFF  
  
     Dabei ist *Datenbankname* der Name der gespiegelten Datenbank.  
  
     Im folgenden Beispiel wird der Zeuge aus der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Datenbank entfernt.  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET WITNESS OFF ;  
    ```  
  
##  <a name="follow-up-after-removing-the-witness"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Entfernen des Zeugen  
 Durch das Deaktivieren des Zeugen ändert sich der [Betriebsmodus](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)entsprechend der Einstellung für die Transaktionssicherheit:  
  
-   Wenn die Transaktionssicherheit auf FULL (Standardeinstellung) festgelegt ist, wird in der Sitzung der synchrone Modus für hohe Sicherheit ohne automatisches Failover verwendet.  
  
-   Wenn die Transaktionssicherheit auf OFF festgelegt ist, wird die Sitzung asynchron (im Modus für hohe Leistung) ausgeführt, ohne dass ein Quorum erforderlich ist. Bei deaktivierter Transaktionssicherheit wird stets dringend empfohlen, den Zeugen ebenfalls zu deaktivieren.  
  
> [!TIP]  
>  Die Transaktionssicherheitseinstellung der Datenbank wird auf jedem Partner in der [sys.database_mirroring](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md)-Katalogsicht in der **mirroring_safety_level**-Spalte und der **mirroring_safety_level_desc**-Spalte aufgezeichnet.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Hinzufügen eines Zeugen für die Datenbankspiegelung mithilfe der Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
-   [Hinzufügen oder Ersetzen eines Datenbank-Spiegelungszeugen &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/add-or-replace-a-database-mirroring-witness-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER DATABASE-Datenbankspiegelung &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
 [Ändern der Transaktionssicherheit in einer Datenbank-Spiegelungssitzung (Transact-SQL)](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md)   
 [Hinzufügen eines Zeugen für die Datenbankspiegelung mithilfe der Windows-Authentifizierung (Transact-SQL)](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)   
 [Datenbank-Spiegelungszeuge](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
