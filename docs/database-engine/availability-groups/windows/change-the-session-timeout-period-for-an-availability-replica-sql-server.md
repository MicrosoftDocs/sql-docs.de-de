---
title: Ändern des Sitzungstimeouts für ein Verfügbarkeitsgruppenreplikat
description: Beschreibt, wie das Sitzungstimeout eines Replikats in einer Always On-Verfügbarkeitsgruppe konfiguriert wird.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], session timeout
- session timeout [SQL Server]
ms.assetid: e23c6e06-1cd1-4d4a-9bc2-e3e06ab2933d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8aebb45eb25338a2303e607f1a37a877af999ed8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343791"
---
# <a name="modify-the-session-timeout-period-for-an-availability-group-replica"></a>Ändern der Sitzungstimeoutperiode für ein Verfügbarkeitsgruppenreplikat
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie das Sitzungstimeout eines Always On-Verfügbarkeitsreplikats mit [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]oder PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]konfiguriert wird. Das Sitzungstimeout ist eine Replikateigenschaft, die steuert, wie lange (in Sekunden) ein Verfügbarkeitsreplikat auf eine Pingantwort von einem verbundenen Replikat wartet, bevor die Verbindung als fehlgeschlagen betrachtet wird. Standardmäßig wartet ein Replikat 10 Sekunden auf eine Pingantwort. Diese Replikateigenschaft wendet nur die Verbindung zwischen einem angegebenen sekundären Replikat und dem primären Replikat der Verfügbarkeitsgruppe an. Weitere Informationen finden Sie unter [Übersicht über Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
   
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Sie müssen mit der Serverinstanz verbunden sein, die das primäre Replikat hostet.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
 Es wird empfohlen, einen Timeoutzeitraum von 10 Sekunden oder mehr zu wählen. Wenn Sie diesen Wert auf weniger als 10 Sekunden festlegen, verpasst ein stark ausgelastetes System möglicherweise PINGs und meldet einen falschen Fehler.  
  
  
## <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So ändern Sie das Sitzungstimeout für ein Verfügbarkeitsreplikat**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die das primäre Verfügbarkeitsreplikat hostet, und erweitern Sie die Serverstruktur.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Klicken Sie auf die Verfügbarkeitsgruppe, deren Verfügbarkeitsreplikat konfiguriert werden soll.  
  
4.  Klicken Sie mit der rechten Maustaste auf das Replikat, das konfiguriert werden soll, und klicken Sie auf **Eigenschaften**.  
  
5.  Verwenden Sie im Dialogfeld **Eigenschaften des Verfügbarkeitsreplikats** das Feld **Sitzungstimeout (Sekunden)** , um die Anzahl der Sekunden für das Sitzungstimeout für dieses Replikat zu ändern.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So ändern Sie das Sitzungstimeout für ein Verfügbarkeitsreplikat**  
  
1.  Stellen Sie eine Verbindung mit der Serverinstanz her, die das primäre Replikat hostet.  
  
2.  Verwenden Sie die [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) -Anweisung wie folgt:  
  
     ALTER AVAILABILITY GROUP *Gruppenname*  
  
     MODIFY REPLICA ON '*Instanzname*' WITH ( SESSION_TIMEOUT =*Sekunden* )  
  
     Dabei ist *Gruppenname* der Name der Verfügbarkeitsgruppe, *Instanzname* der Name der Serverinstanz, die das zu ändernde Verfügbarkeitsreplikat hostet, und *Sekunden* gibt die Mindestanzahl von Sekunden an, die das Replikat als sekundäres Replikat vor dem Anwenden des Protokolls auf Datenbanken warten muss. Der Standard von 0 Sekunden gibt an, dass es keine Verzögerung gibt.  
  
     Im folgenden Beispiel, eingegeben für das primäre Replikat der `AccountsAG` -Verfügbarkeitsgruppe, wird der Sitzungstimeoutwert in `15` Sekunden für das Replikat geändert, das sich auf der Serverinstanz `INSTANCE09` befindet.  
  
    ```  
    ALTER AVAILABILITY GROUP AccountsAG   
       MODIFY REPLICA ON 'INSTANCE09' WITH (SESSION_TIMEOUT = 15);  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> PowerShell  
 **So ändern Sie das Sitzungstimeout für ein Verfügbarkeitsreplikat**  
  
1.  Wechseln Sie mit **cd** in das Verzeichnis der Serverinstanz, die das primäre Replikat hostet.  
  
2.  Verwenden Sie das Cmdlet **Set-SqlAvailabilityReplica** mit dem **SessionTimeout** -Parameter, um die Anzahl der Sekunden für das Sitzungstimeout für ein angegebenes Verfügbarkeitsreplikat zu ändern.  
  
     Mit dem folgenden Befehl wird der Zeitraum für das Sitzungstimeout z. B. auf 15 Sekunden festgelegt.  
  
    ```  
    Set-SqlAvailabilityReplica -SessionTimeout 15 `   
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
    > [!NOTE]  
    >  Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Einrichten und Verwenden des SQL Server PowerShell-Anbieters**  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht zu AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
