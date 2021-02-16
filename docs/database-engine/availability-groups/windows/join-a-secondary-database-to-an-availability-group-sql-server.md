---
title: Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe
description: Hier werden die erforderlichen Schritte erläutert, um eine sekundäre Datenbank mithilfe von Transact-SQL (T-SQL), PowerShell oder SQL Server Management Studio mit einer Always On-Verfügbarkeitsgruppe zu verknüpfen.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
f1_keywords:
- sql13.swb.availabilitygroup.joindbs.f1
helpviewer_keywords:
- secondary databases [SQL Server], in availability group
- secondary databases [SQL Server]
- Availability Groups [SQL Server], joining
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: fd7efe79-c1f9-497d-bfe7-b2a2b2321cf5
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9e1d98fc820215de35e11cb06843884968dd5b3a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348451"
---
# <a name="join-a-secondary-database-to-an-always-on-availability-group"></a>Verknüpfen einer sekundären Datenbank mit einer Always On-Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie eine sekundäre Datenbank mithilfe von [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]oder PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]mit einer Always On-Verfügbarkeitsgruppe verknüpft wird. Nachdem Sie eine sekundäre Datenbank auf ein sekundäres Replikat vorbereitet haben, müssen Sie die Datenbank so schnell wie möglich mit der Verfügbarkeitsgruppe verknüpfen. So wird die Datenverschiebung aus der entsprechenden primären Datenbank in die sekundäre Datenbank gestartet.  
   
> [!NOTE]  
>  Informationen dazu, was passiert, nachdem eine sekundäre Datenbank mit der Gruppe verknüpft wurde, finden Sie unter [Übersicht über Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)mit einer Always On-Verfügbarkeitsgruppe verknüpft wird.  
   
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Sie müssen mit der Serverinstanz verbunden sein, auf der das sekundäre Replikat gehostet wird.  
  
-   Das sekundäre Replikat muss bereits mit der Verfügbarkeitsgruppe verknüpft sein. Weitere Informationen finden Sie unter [Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)mit einer Always On-Verfügbarkeitsgruppe verknüpft wird.  
  
-   Die sekundäre Datenbank muss vor kurzem vorbereitet worden sein. Weitere Informationen finden Sie unter [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)erstellt und konfiguriert wird.  
  
###  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So verknüpfen Sie eine sekundäre Datenbank mit einer Verfügbarkeitsgruppe**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die das sekundäre Replikat hostet, und erweitern Sie die Serverstruktur.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Erweitern Sie die Verfügbarkeitsgruppe, die Sie ändern möchten, und erweitern Sie den Knoten **Verfügbarkeitsdatenbanken** .  
  
4.  Klicken Sie mit der rechten Maustaste auf die Datenbank, und klicken Sie auf **Verfügbarkeitsgruppe beitreten**.  
  
5.  Das Dialogfeld **Datenbanken mit Verfügbarkeitsgruppe verknüpfen** wird geöffnet. Überprüfen Sie den Namen der Verfügbarkeitsgruppe, der in der Titelleiste angezeigt wird, und den im Raster angezeigten Datenbanknamen bzw. andere Namen, und klicken Sie auf **OK**, oder klicken Sie auf **Abbrechen**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So verknüpfen Sie eine sekundäre Datenbank mit einer Verfügbarkeitsgruppe**  
  
1.  Stellen Sie eine Verbindung mit der Serverinstanz her, die das sekundäre Replikat hostet.  
  
2.  Verwenden Sie die [SET HADR-Klausel der ALTER DATABASE](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) -Anweisung wie folgt:  
  
     ALTER DATABASE *Datenbankname* SET HADR AVAILABILITY GROUP = *Gruppenname*,  
  
     dabei ist *Datenbankname* der Name einer hinzuzufügenden Datenbank und *Gruppenname* der Name der Verfügbarkeitsgruppe.  
  
     Im folgenden Beispiel wird die sekundäre Datenbank `Db1` mit dem lokalen sekundären Replikat der `MyAG`-Verfügbarkeitsgruppe verknüpft.  
  
    ```  
    ALTER DATABASE Db1 SET HADR AVAILABILITY GROUP = MyAG;  
    ```  
  
    > [!NOTE]  
    >  Unter [Erstellen einer Verfügbarkeitsgruppe &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md) können Sie die Verwendung dieser [!INCLUDE[tsql](../../../includes/tsql-md.md)]-Anweisung im Kontext sehen.  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> PowerShell  
 **So verknüpfen Sie eine sekundäre Datenbank mit einer Verfügbarkeitsgruppe**  
  
1.  Wechseln Sie mit **cd** in das Verzeichnis der Serverinstanz, die das sekundäre Replikat hostet.  
  
2.  Verwenden Sie das **Add-SqlAvailabilityDatabase** -Cmdlet, um eine oder mehrere sekundäre Datenbanken mit der Verfügbarkeitsgruppe zu verknüpfen.  
  
     Beispielsweise wird durch den folgenden Befehl die sekundäre Datenbank `Db1`mit der Verfügbarkeitsgruppe `MyAG` in einer der Serverinstanzen verknüpft, von denen ein sekundäres Replikat gehostet wird.  
  
    ```  
    Add-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\SQL\SecondaryServer\InstanceName\AvailabilityGroups\MyAG `   
    -Database "Db1"  
    ```  
  
    > [!NOTE]  
    >  Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Einrichten und Verwenden des SQL Server PowerShell-Anbieters**  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Problembehandlung für die AlwaysOn-Verfügbarkeitsgruppenkonfiguration &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
