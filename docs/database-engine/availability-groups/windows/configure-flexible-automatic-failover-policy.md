---
title: Konfigurieren einer flexiblen Richtlinie für ein automatisches Failover für eine Verfügbarkeitsgruppe
description: Beschreibt, wie Sie eine flexible Failoverrichtlinie für eine Always On-Verfügbarkeitsgruppe mithilfe von Transact-SQL (T-SQL), PowerShell oder SQL Server Management Studio konfigurieren.
ms.date: 11/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], flexible failover policy
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 1ed564b4-9835-4245-ae35-9ba67419a4ce
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.custom: seo-lt-2019
ms.openlocfilehash: 487b155561944e1303aac0fdb7453fc2d494802e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342449"
---
# <a name="configure-a-flexible-automatic-failover-policy-for-an-always-on-availability-group"></a>Konfigurieren einer flexiblen Richtlinie für ein automatischen Failover für eine Always On-Verfügbarkeitsgruppe

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  In diesem Thema wird beschrieben, wie die flexible Failoverrichtlinie für eine Always On-Verfügbarkeitsgruppe mithilfe von [!INCLUDE[tsql](../../../includes/tsql-md.md)] oder PowerShell in SQL Server konfiguriert wird. Eine flexible Failoverrichtlinie ermöglicht eine präzise Kontrolle über die Bedingungen, die ein [automatisches Failover](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) für eine Verfügbarkeitsgruppe verursachen. Durch eine Änderung der Fehlerbedingungen, die ein automatisches Failover und die Häufigkeit von Integritätsprüfungen auslösen, können Sie die Wahrscheinlichkeit für ein automatisches Failover erhöhen oder verringern, um das SLA für Hochverfügbarkeit zu unterstützen.  

   Die flexible Failoverrichtlinie für eine Verfügbarkeitsgruppe wird durch die Fehlerbedingungsebene und einen Schwellenwert für das Integritätsprüfungstimeout definiert. Sobald erkannt wird, dass eine Verfügbarkeitsgruppe ihre Fehlerbedingungsebene oder ihren Schwellenwert für das Integritätsprüfungstimeout überschritten hat, meldet die Ressourcen-DLL der Verfügbarkeitsgruppe dies dem WSFC-Cluster (Windows Server Failover Clustering). Der WSFC-Cluster initiiert dann ein automatisches Failover zum sekundären Replikat.  
 
  > [!NOTE]  
  > Die flexible Failoverrichtlinie für eine Verfügbarkeitsgruppe kann nicht mit [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]konfiguriert werden.  
  
 
## <a name="limitations-on-automatic-failovers"></a><a name="Limitations"></a> Einschränkungen beim automatischen Failover  
  
-   Damit ein automatisches Failover ausgeführt werden kann, müssen das aktuelle primäre Replikat und ein sekundäres Replikat für den Verfügbarkeitsmodus für synchrone Commits und automatischem Failover konfiguriert und das sekundäre Replikat mit dem primären Replikat synchronisiert sein.  
  
-   Für das automatische Failover werden nur drei Replikate unterstützt.  
  
-   Wenn eine Verfügbarkeitsgruppe den Schwellenwert für WSFC-Fehler überschreitet, versucht der WSFC-Cluster nicht, ein automatisches Failover für die Verfügbarkeitsgruppe auszuführen. Außerdem verbleibt die WSFC-Ressourcengruppe der Verfügbarkeitsgruppe so lange in einem fehlerhaften Zustand, bis der Clusteradministrator die fehlerhafte Gruppe manuell online schaltet oder bis der Datenbankadministrator ein manuelles Failover der Verfügbarkeitsgruppe ausführt. Der *WSFC-Fehlerschwellenwert* ist als maximale Anzahl von Fehlern definiert, die während eines bestimmten Zeitraums für die Verfügbarkeitsgruppe unterstützt werden. Der Standardzeitraum beträgt sechs Stunden, und der Standardwert für die maximale Anzahl von Fehlern während dieses Zeitraums entspricht *n*-1, wobei *n* für die Anzahl der WSFC-Knoten steht. Um den Fehlerschwellenwert für eine angegebene Verfügbarkeitsgruppe zu ändern, verwenden Sie die WSFC Failover Manager Console.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Sie müssen mit der Serverinstanz verbunden sein, die das primäre Replikat hostet.  
   
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
  
|Aufgabe|Berechtigungen|  
|----------|-----------------|  
|Konfigurieren der flexiblen Failoverrichtlinie für eine neue Verfügbarkeitsgruppe|Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** und die CREATE AVAILABILITY GROUP-Serverberechtigung, ALTER ANY AVAILABILITY GROUP-Berechtigung oder CONTROL SERVER-Berechtigung.|  
|Ändern der Richtlinie einer vorhandenen Verfügbarkeitsgruppe|Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.|  

##  <a name="health-check-timeout-threshold"></a><a name="HCtimeout"></a> Schwellenwert für das Integritätsprüfungstimeout  
 Die WSFC-Ressourcen-DLL der Verfügbarkeitsgruppe führt eine *Integritätsprüfung* am primären Replikat aus, indem sie die gespeicherte Prozedur [sp_server_diagnostics](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) auf der SQL Server-Instanz aufruft, die das primäre Replikat hostet. **sp_server_diagnostics** gibt Ergebnisse in einem Intervall zurück, das 1/3 des Schwellenwerts für das Integritätsprüfungstimeout für die Verfügbarkeitsgruppe entspricht. Der Standardschwellenwert für das Integritätsprüfungstimeout ist 30 Sekunden, und dies bedeutet, dass **sp_server_diagnostics** in einem Intervall von 10 Sekunden Ergebnisse zurückgibt. Wenn **sp_server_diagnostics** langsam ist oder keine Informationen zurückgibt, wartet die Ressourcen-DLL das gesamte Intervall des durch den Schwellenwert definierten Integritätsprüfungstimeouts ab, bevor festgestellt wird, dass das primäre Replikat nicht reagiert. Wenn das primäre Replikat nicht reagiert, wird ein automatisches Failover initiiert, sofern dies aktuell unterstützt wird.  
  
> [!IMPORTANT]  
>  **sp_server_diagnostics** führt keine Integritätsprüfungen auf Datenbankebene aus.  
  
##  <a name="failure-condition-level"></a><a name="FClevel"></a> Fehlerbedingungsebene  
 Ob die Diagnosedaten und Zustandsinformationen, die von **sp_server_diagnostics** zurückgegeben wurden, ein automatisches Failover garantieren, hängt von der Fehlerbedingungsebene der Verfügbarkeitsgruppe ab. Die *Fehlerbedingungsebene* gibt an, welche Fehlerbedingungen ein automatisches Failover auslösen. Es gibt fünf Fehlerbedingungsebenen, die von der Ebene mit den wenigsten Einschränkungen (Ebene 1) bis zur Ebene mit den meisten Einschränkungen (Ebene 5) reichen. Jede Bedingungsebene umfasst stets auch die weniger restriktiven Ebenen. Daher schließt die strengste Ebene 5 die vier Bedingungsebenen mit weniger Einschränkungen ein usw.  
  
> [!IMPORTANT]  
>  Beschädigte und fehlerverdächtige Datenbanken werden von keiner Fehlerbedingungsebene erkannt. Deshalb werden automatische Failover niemals von Datenbanken ausgelöst, die beschädigt oder fehlerverdächtig sind (entweder aufgrund von Hardwarefehlern, Datenkorruption oder anderen Fehlern).  
  
 In der folgenden Tabelle werden die Fehlerbedingungen beschrieben, die der jeweiligen Ebene entspricht.  
  
|Ebene|Fehlerbedingung|[!INCLUDE[tsql](../../../includes/tsql-md.md)] Wert|PowerShell-Wert|  
|-----------|-----------------------|------------------------------|----------------------|  
|Eine|der Server ausfällt. Gibt an, dass ein automatisches Failover in den folgenden Fällen initiiert wird:<br /><br /> Der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienst ist ausgefallen.<br /><br /> Das Leasing der Verfügbarkeitsgruppe für die Verbindung mit dem WSFC-Cluster läuft ab, da keine ACK-Meldung von der Serverinstanz empfangen wird. Weitere Informationen finden Sie unter [How It Works: SQL Server AlwaysOn Lease Timeout](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268).<br /><br /> <br /><br /> Dies ist die am wenigsten restriktive Ebene.|1|**OnServerDown**|  
|Zwei|der Server nicht reagiert. Gibt an, dass ein automatisches Failover in den folgenden Fällen initiiert wird:<br /><br /> Die Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] stellt keine Verbindung mit dem Cluster her, und der vom Benutzer angegebene Schwellenwert für das Integritätsprüfungstimeout der Verfügbarkeitsgruppe wurde überschritten.<br /><br /> Das Verfügbarkeitsreplikat weist einen fehlerhaften Status auf.|2|**OnServerUnresponsive**|  
|drei|ein kritischer Serverfehler auftritt. Gibt an, dass ein automatisches Failover bei kritischen internen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Fehlern initiiert wird, z. B. verwaisten Spinlocks, ernsten Schreibzugriffsverletzungen oder zu vielen Sicherungen.<br /><br /> Dies ist der Standardebene.|3|**OnCriticalServerError**|  
|4 (vier)|ein mittelschwerer Serverfehler auftritt. Gibt an, dass ein automatisches Failover bei mittelschweren internen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Fehlern initiiert wird, z. B. bei dauerhaft unzureichendem Arbeitsspeicher im internen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Ressourcenpool.|4|**OnModerateServerError**|  
|5 (fünf)|eine qualifizierte Fehlerbedingung auftritt. Gibt an, dass ein automatisches Failover bei sämtlichen qualifizierten Fehlerbedingungen initiiert wird, einschließlich:<br /><br /> Erkennung eines Deadlocks des Schedulers<br /><br /> Erkennung eines unlösbaren Deadlocks.<br /><br /> <br /><br /> Dies ist die restriktivste Ebene.|5|**OnAnyQualifiedFailureConditions**|  
  
> [!NOTE]  
>  Das Fehlen einer Reaktion auf Clientanforderungen durch eine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz ist für Verfügbarkeitsgruppen nicht relevant.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So konfigurieren Sie die flexible Failoverrichtlinie**  
  
1.  Stellen Sie eine Verbindung mit der Serverinstanz her, die das primäre Replikat hostet.  
  
2.  Für eine neue Verfügbarkeitsgruppe verwenden Sie die [-Anweisung](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] . Verwenden Sie zum Ändern einer vorhandenen Verfügbarkeitsgruppe die [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] -Anweisung.  
  
    -   Um die Failover-Bedingungsebene festzulegen, verwenden Sie die Option FAILURE_CONDITION_LEVEL = *n* , wobei *n* für eine ganze Zahl zwischen 1 und 5 steht.  
  
         Beispielsweise wird mit der folgenden [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Anweisung die Fehlerbedingungsebene der vorhandenen Verfügbarkeitsgruppe `AG1`in Ebene 1 geändert:  
  
        ```  
  
        ALTER AVAILABILITY GROUP AG1 SET (FAILURE_CONDITION_LEVEL = 1);  
        ```  
  
         Diese ganzzahligen Werte stehen in folgender Beziehung zu den Fehlerbedingungsebenen:  
  
        |[!INCLUDE[tsql](../../../includes/tsql-md.md)] Wert|Ebene|Automatisches Failover wird initiiert, wenn...|  
        |------------------------------|-----------|-------------------------------------------|  
        |1|Eine|der Server ausfällt. Der SQL Server-Dienst wird aufgrund eines Failovers oder Neustarts beendet.|  
        |2|Zwei|der Server nicht reagiert. Der Wert der Bedingungsebene wird unterschritten, der SQL Server-Dienst ist mit dem Cluster verbunden, und der Schwellenwert für das Timeout der Integritätsprüfung wird überschritten, oder das aktuelle primäre Replikat weist einen fehlerhaften Status auf.|  
        |3|drei|ein kritischer Serverfehler auftritt. Der Wert der Bedingungsebene wird unterschritten, oder es tritt ein interner kritischer Serverfehler auf.<br /><br /> Dies ist der Standardebene.|  
        |4|4 (vier)|ein mittelschwerer Serverfehler auftritt. Der Wert der Bedingungsebene wird unterschritten, oder es tritt ein mittelschwerer Serverfehler auf.|  
        |5|5 (fünf)|eine qualifizierte Fehlerbedingung auftritt. Der Wert der Bedingungsebene wird unterschritten, oder es tritt eine qualifizierte Fehlerbedingung auf.|  
  
         Weitere Informationen zu den Failover-Bedingungsebenen finden Sie unter [Flexible Failoverrichtlinie für automatisches Failover einer Verfügbarkeitsgruppe &#40;SQL Server&#41;]().  
  
    -   Um den Schwellenwert für das Timeout der Integritätsprüfung zu konfigurieren, verwenden Sie die Option HEALTH_CHECK_TIMEOUT = *n* , wobei *n* für eine ganze Zahl zwischen 15000 Millisekunden (15 Sekunden) und 4294967295 Millisekunden steht. Der Standardwert ist 30.000 Millisekunden (oder 30 Sekunden).  
  
         Mit der folgenden [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Anweisung wird z. B. der Schwellenwert für das Timeout der Integritätsprüfung einer vorhandenen Verfügbarkeitsgruppe mit dem Namen `AG1`in 60.000 Millisekunden (eine Minute) geändert.  
  
        ```  
  
        ALTER AVAILABILITY GROUP AG1 SET (HEALTH_CHECK_TIMEOUT = 60000);  
        ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> PowerShell  
 **So konfigurieren Sie die flexible Failoverrichtlinie**  
  
1.  Legen Sie mit (**cd**) die Serverinstanz als Standard fest, die das primäre Replikat hostet.  
  
2.  Verwenden Sie beim Hinzufügen eines Verfügbarkeitsreplikats zu einer Verfügbarkeitsgruppe das Cmdlet **New-SqlAvailabilityGroup** . Verwenden Sie beim Ändern eines vorhandenen Verfügbarkeitsreplikats das Cmdlet **Set-SqlAvailabilityGroup**.  
  
    -   Um die Failover-Bedingungsebene festzulegen, verwenden Sie den **FailureConditionLevel**_level_-Parameter, wobei *level* für einen der folgenden Werte steht:  
  
        |value|Ebene|Automatisches Failover wird initiiert, wenn...|  
        |-----------|-----------|-------------------------------------------|  
        |**OnServerDown**|Eine|der Server ausfällt. Der SQL Server-Dienst wird aufgrund eines Failovers oder Neustarts beendet.|  
        |**OnServerUnresponsive**|Zwei|der Server nicht reagiert. Der Wert der Bedingungsebene wird unterschritten, der SQL Server-Dienst ist mit dem Cluster verbunden, und der Schwellenwert für das Timeout der Integritätsprüfung wird überschritten, oder das aktuelle primäre Replikat weist einen fehlerhaften Status auf.|  
        |**OnCriticalServerError**|drei|ein kritischer Serverfehler auftritt. Der Wert der Bedingungsebene wird unterschritten, oder es tritt ein interner kritischer Serverfehler auf.<br /><br /> Dies ist der Standardebene.|  
        |**OnModerateServerError**|4 (vier)|ein mittelschwerer Serverfehler auftritt. Der Wert der Bedingungsebene wird unterschritten, oder es tritt ein mittelschwerer Serverfehler auf.|  
        |**OnAnyQualifiedFailureConditions**|5 (fünf)|eine qualifizierte Fehlerbedingung auftritt. Der Wert der Bedingungsebene wird unterschritten, oder es tritt eine qualifizierte Fehlerbedingung auf.|  
  
         Weitere Informationen zu den Failover-Bedingungsebenen finden Sie unter [Flexible Failoverrichtlinie für automatisches Failover einer Verfügbarkeitsgruppe &#40;SQL Server&#41;]().  
  
         Beispielsweise wird mit dem folgenden Befehl die Fehlerbedingungsebene der vorhandenen Verfügbarkeitsgruppe `AG1`in Ebene 1 geändert.  
  
        ```  
        Set-SqlAvailabilityGroup `   
        -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg `   
        -FailureConditionLevel OnServerDown  
        ```  
  
    -   Um den Schwellenwert für das Timeout der Integritätsprüfung festzulegen, verwenden Sie den **HealthCheckTimeout**_n_ -Parameter, wobei *n* für eine ganze Zahl zwischen 15000 Millisekunden (15 Sekunden) und 4294967295 Millisekunden steht. Der Standardwert ist 30000 Millisekunden (oder 30 Sekunden).  
  
         Mit dem folgenden Befehl wird z. B. der Schwellenwert für das Timeout der Integritätsprüfung der vorhandenen Verfügbarkeitsgruppe `AG1`in 120.000 Millisekunden (zwei Minuten) geändert.  
  
        ```  
        Set-SqlAvailabilityGroup `   
        -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAG `   
        -HealthCheckTimeout 120000  
        ```  
  
> [!NOTE]  
>  Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Einrichten und Verwenden des SQL Server PowerShell-Anbieters**  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  

##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **To configure automatic failover**  
  
-   [Ändern des Verfügbarkeitsmodus eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md) (automatisches Failover erfordert den Verfügbarkeitsmodus für synchrone Commits)  
  
-   [Ändern des Failovermodus eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Konfigurieren der flexiblen Failoverrichtlinie zum Steuern der Bedingungen für ein automatisches Failover &#40;AlwaysOn-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   [How It Works: SQL Server AlwaysOn Lease Timeout](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verfügbarkeitsmodi &#40;Always On-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Failover und Failovermodi (Always On-Verfügbarkeitsgruppen)](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)   
 [Windows Server-Failoverclustering &#40;WSFC&#41; mit SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)   
 [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)   
 [sp_server_diagnostics (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md)  
  
