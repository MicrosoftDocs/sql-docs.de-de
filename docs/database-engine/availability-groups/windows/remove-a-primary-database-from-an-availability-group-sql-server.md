---
title: Entfernen einer Datenbank aus einer Verfügbarkeitsgruppe
description: Hier werden die erforderlichen Schritte erläutert, um eine primäre Datenbank mithilfe von Transact-SQL (T-SQL), PowerShell oder SQL Server Management Studio aus einer Always On-Verfügbarkeitsgruppe zu entfernen.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.removeprimarydb.f1
helpviewer_keywords:
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], removing
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: 6d4ca31e-ddf0-44bf-be5e-a5da060bf096
author: cawrites
ms.author: chadam
ms.openlocfilehash: ca813d490f97bf8e501ada801599cef74e2b9c05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351030"
---
# <a name="remove-a-primary-database-from-an-always-on-availability-group"></a>Entfernen einer primären Datenbank aus einer Always On-Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird erläutert, wie sowohl die primäre Datenbank als auch die entsprechenden sekundären Datenbanken aus einer AlwaysOn-Verfügbarkeitsgruppe mit [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]oder PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]entfernt werden.  
  
##  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Voraussetzungen und Einschränkungen  
  
-   Dieser Task wird nur für primäre Replikate unterstützt. Sie müssen mit der Serverinstanz verbunden sein, die das primäre Replikat hostet.  
  
 
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So entfernen Sie eine Verfügbarkeitsdatenbank**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die das primäre Replikat der zu entfernenden Datenbanken hostet, und erweitern Sie die Serverstruktur.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Wählen Sie die Verfügbarkeitsgruppe aus, und erweitern Sie den Knoten **Verfügbarkeitsdatenbanken** .  
  
4.  Dieser Schritt hängt davon ab, ob Sie mehrere Datenbankgruppen oder nur eine Datenbank entfernen möchten:  
  
    -   Verwenden Sie zum Entfernen mehrerer Datenbanken den Bereich **Details zum Objekt-Explorer** , um alle zu entfernenden Datenbanken anzuzeigen und auszuwählen. Weitere Informationen finden Sie unter [Verwenden der Details zum Objekt-Explorer zum Überwachen von Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Wählen Sie zum Entfernen einer einzelnen Datenbank diese im Bereich **Objekt-Explorer** oder **Details zum Objekt-Explorer** aus.  
  
5.  Klicken Sie mit der rechten Maustaste auf die ausgewählten Datenbanken, und wählen Sie im Kontextmenü den Befehl **Datenbank aus Verfügbarkeitsgruppe entfernen** aus.  
  
6.  Klicken Sie zum Entfernen aller aufgelisteten Datenbanken im Dialogfeld **Datenbanken aus Verfügbarkeitsgruppe entfernen** auf **OK**. Wenn Sie nicht alle Datenbanken entfernen wollen, klicken Sie auf **Abbrechen**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So entfernen Sie eine Verfügbarkeitsdatenbank**  
  
1.  Stellen Sie eine Verbindung mit der Serverinstanz her, die das primäre Replikat hostet.  
  
2.  Verwenden Sie die [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) -Anweisung wie folgt:  
  
     ALTER AVAILABILITY GROUP *Gruppenname* REMOVE DATABASE *Verfügbarkeitsdatenbankname*  
  
     Dabei ist *Gruppenname* der Name der Verfügbarkeitsgruppe und *Verfügbarkeitsdatenbankname* der Name der zu entfernenden Datenbank.  
  
     Im folgenden Beispiel wird eine Datenbank namens `Db6` aus der `MyAG` -Verfügbarkeitsgruppe entfernt.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG REMOVE DATABASE Db6;  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> PowerShell  
 **So entfernen Sie eine Verfügbarkeitsdatenbank**  
  
1.  Wechseln Sie mit **cd** in das Verzeichnis der Serverinstanz, die das primäre Replikat hostet.  
  
2.  Verwenden Sie das **Remove-SqlAvailabilityDatabase** -Cmdlet, und geben Sie dabei den Namen der Verfügbarkeitsdatenbank an, die aus der Verfügbarkeitsgruppe entfernt werden soll. Wenn Sie mit der Serverinstanz verbunden sind, die das primäre Replikat hostet, werden die primäre Datenbank und ihre entsprechenden sekundären Datenbanken aus der Verfügbarkeitsgruppe entfernt.  
  
     Beispielsweise wird durch den folgenden Befehl das Verfügbarkeitsdatenbank `MyDb9` von der Verfügbarkeitsgruppe namens `MyAg`entfernt. Da der Befehl auf der Serverinstanz ausgeführt wird, von der das primäre Replikat gehostet wird, werden die primäre Datenbank und alle entsprechenden sekundären Datenbanken aus der Verfügbarkeitsgruppe entfernt. Für diese Datenbank wird auf allen sekundären Replikaten keine Datensynchronisierung mehr ausgeführt.  
  
    ```  
    Remove-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\Sql\PrimaryComputer\InstanceName\AvailabilityGroups\MyAg\AvailabilityDatabases\MyDb9
    ```  
  
    > [!NOTE]  
    >  Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Einrichten und Verwenden des SQL Server PowerShell-Anbieters**  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-removing-an-availability-database-from-an-availability-group"></a><a name="FollowUp"></a> Nachverfolgung: Nach dem Entfernen einer Verfügbarkeitsdatenbank aus einer Verfügbarkeitsgruppe  
 Durch das Entfernen einer Verfügbarkeitsdatenbank aus ihrer Verfügbarkeitsgruppe wird die Datensynchronisierung zwischen der früheren primären Datenbank und den entsprechenden sekundären Datenbanken beendet. Die frühere primäre Datenbank bleibt online. Alle entsprechenden sekundären Datenbanken wechseln in den Status RESTORING.  
  
 Zu diesem Zeitpunkt stehen alternative Methoden zum Umgang mit einer entfernten sekundären Datenbank zur Verfügung:  
  
-   Wenn eine bestimmte sekundäre Datenbank nicht mehr benötigt wird, können Sie sie löschen.  
  
     Weitere Informationen finden Sie unter [Löschen einer Datenbank](../../../relational-databases/databases/delete-a-database.md).  
  
-   Wenn Sie auf eine entfernte sekundäre Datenbank zugreifen möchten, nachdem sie aus der Verfügbarkeitsgruppe entfernt wurde, können Sie die Datenbank wiederherstellen. Wenn Sie jedoch eine entfernte sekundäre Datenbank wiederherstellen, sind zwei voneinander abweichende, unabhängige Datenbanken mit demselben Namen online. Sie müssen sicherstellen, dass Clients nur auf eine von beiden zugreifen können (in der Regel die aktuelle primäre Datenbank).  
  
     Weitere Informationen finden Sie unter [Wiederherstellen einer Datenbank ohne Wiederherstellung von Daten &#40;Transact-SQL&#41;](../../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Entfernen einer sekundären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
