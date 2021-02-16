---
title: Entfernen einer sekundären Replikats aus einer Verfügbarkeitsgruppe
description: 'Hier werden die erforderlichen Schritte erläutert, um ein sekundäres Replikat mithilfe von Transact-SQL (T-SQL), PowerShell oder SQL Server Management Studio aus einer Always On-Verfügbarkeitsgruppe zu entfernen. '
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.removesecondaryar.f1
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], configuring
ms.assetid: 35ddc8b6-3e7c-4417-9a0a-d4987a09ddf7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 51211eafbc847a5ad7daae6fecc128a489116cd0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344602"
---
# <a name="remove-a-secondary-replica-from-an-availability-group-sql-server"></a>Entfernen einer sekundären Replikats aus einer Verfügbarkeitsgruppe (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie ein sekundäres Replikat aus einer Always On-Verfügbarkeitsgruppe mit [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]oder PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]entfernt wird.  
 
   
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Dieser Task wird nur für das primäre Replikat unterstützt.    
-   Nur ein sekundäres Replikat kann aus einer Verfügbarkeitsgruppe entfernt werden.  
  
## <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Sie benötigen eine Verbindung zur Serverinstanz, die das primäre Replikat der Verfügbarkeitsgruppe hostet.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So entfernen Sie ein sekundäres Replikat**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die das primäre Verfügbarkeitsreplikat hostet, und erweitern Sie die Serverstruktur.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Wählen Sie die Verfügbarkeitsgruppe aus, und erweitern Sie den Knoten **Verfügbarkeitsreplikate** .  
  
4.  Dieser Schritt hängt davon ab, ob Sie mehrere Replikate oder nur ein Replikat entfernen möchten:  
  
    -   Verwenden Sie zum Entfernen mehrerer Replikate den Bereich **Details zum Objekt-Explorer** , um alle zu entfernenden Replikate anzuzeigen und auszuwählen. Weitere Informationen finden Sie unter [Verwenden der Details zum Objekt-Explorer zum Überwachen von Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Wählen Sie zum Entfernen eines einzelnen Replikats dieses im Bereich **Objekt-Explorer** oder **Details zum Objekt-Explorer** aus.  
  
5.  Klicken Sie mit der rechten Maustaste auf die ausgewählten sekundären Replikate, und wählen Sie im Befehlsmenü **Aus Verfügbarkeitsgruppe entfernen** aus.  
  
6.  Klicken Sie zum Entfernen aller aufgeführten sekundären Replikate im Dialogfeld **Sekundäre Replikate aus Verfügbarkeitsgruppe entfernen** auf **OK**. Wenn Sie nicht alle aufgelisteten Replikate entfernen wollen, klicken Sie auf **Abbrechen**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So entfernen Sie ein sekundäres Replikat**  
  
1.  Stellen Sie eine Verbindung mit der Serverinstanz her, die das primäre Replikat hostet.  
  
2.  Verwenden Sie die [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) -Anweisung wie folgt:  
  
     ALTER AVAILABILITY GROUP *Gruppenname* REMOVE REPLICA ON '*Instanzname*' [,...*n*]  
  
     Dabei ist *Gruppenname* der Name der Verfügbarkeitsgruppe und *Instanzname* die Serverinstanz, auf der sich das sekundäre Replikat befindet.  
  
     Im folgenden Codebeispiel wird ein sekundäres Replikat aus der *MyAG* -Verfügbarkeitsgruppe entfernt. Das sekundäre Zielreplikat befindet sich auf der Serverinstanz *HADR_INSTANCE* auf dem Computer *COMPUTER02*.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG REMOVE REPLICA ON 'COMPUTER02\HADR_INSTANCE';  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> PowerShell  
 **So entfernen Sie ein sekundäres Replikat**  
  
1.  Wechseln Sie mit **cd** in das Verzeichnis der Serverinstanz, die das primäre Replikat hostet.  
  
2.  Verwenden Sie das Cmdlet **Remove-SqlAvailabilityReplica** .  
  
     Beispielsweise wird durch den folgenden Befehl das Verfügbarkeitsreplikat auf dem `MyReplica` -Server von der Verfügbarkeitsgruppe namens `MyAg`entfernt.  Dieser Befehl muss auf der Serverinstanz ausgeführt werden, von der das primäre Replikat der Verfügbarkeitsgruppe gehostet wird.  
  
    ```  
    Remove-SqlAvailabilityReplica `   
    -Path SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
    > [!NOTE]  
    >  Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Einrichten und Verwenden des SQL Server PowerShell-Anbieters**  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-removing-a-secondary-replica"></a><a name="PostBestPractices"></a> Nachverfolgung: Nach dem Entfernen eines sekundären Replikats  
 Wenn Sie ein Replikat angeben, das derzeit nicht verfügbar ist, wird beim Onlineschalten des Replikats festgestellt, dass es entfernt wurde.  
  
 Wird ein Replikat entfernt, empfängt es keine Daten mehr. Nachdem für ein sekundäres Replikat bestätigt wurde, dass es aus dem globalen Speicher entfernt wurde, entfernt das Replikat die Verfügbarkeitsgruppeneinstellungen aus seinen Datenbanken, die auf der lokalen Serverinstanz im Status RECOVERING verbleiben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)   
 [Entfernen einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)  
  
