---
title: Richtlinien zum Anzeigen der Verfügbarkeitsgruppenintegrität
description: Verwenden Sie die Always On-Richtlinien oder PowerShell, um die Betriebsintegrität einer Always On-Verfügbarkeitsgruppe zu bestimmen.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 6f1bcbc3-1220-4071-8e53-4b957f5d3089
author: cawrites
ms.author: chadam
ms.openlocfilehash: b9b06dac9dc00fd405fd28f1428a0833d968fabe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340833"
---
# <a name="use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server"></a>Verwenden von AlwaysOn-Richtlinien zum Anzeigen des Zustands einer Verfügbarkeitsgruppe (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird beschrieben, wie der Betriebsstatus einer AlwaysOn-Verfügbarkeitsgruppe in [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] mithilfe einer AlwaysOn-Richtlinie in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]bzw. mit PowerShell geändert wird. Informationen zur Verwaltung auf Basis von AlwaysOn-Richtlinien finden Sie unter [AlwaysOn-Richtlinien für Betriebsprobleme mit AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)bzw. mit PowerShell geändert wird.  
  
> [!IMPORTANT]  
>  Für AlwaysOn-Richtlinien werden die Kategorienamen als IDs verwendet. Durch die Änderung des Namens einer AlwaysOn-Kategorie wird deren Funktionalität zur Integritätsüberprüfung unterbrochen. Daher sollte der Name einer AlwaysOn-Kategorie nie geändert werden.  
  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert CONNECT-, VIEW SERVER STATE- und VIEW ANY DEFINITION-Berechtigungen.  
  
##  <a name="using-the-always-on-dashboard"></a><a name="SSMSProcedure"></a> Verwenden des AlwaysOn-Dashboards  
 **So öffnen Sie das AlwaysOn-Dashboard**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die eines der Verfügbarkeitsreplikate hostet. Verwenden Sie zum Anzeigen von Informationen zu allen Verfügbarkeitsreplikaten in einer Verfügbarkeitsgruppe die Serverinstanz, die das primäre Replikat hostet.  
  
2.  Klicken Sie auf den Servernamen, um die Serverstruktur zu erweitern.  
  
3.  Erweitern Sie den Knoten **Always On High Availability** (Always On Hochverfügbarkeit).  
  
     Klicken Sie entweder mit der rechten Maustaste auf den Knoten **Verfügbarkeitsgruppen** , oder erweitern Sie diesen Knoten, und klicken Sie mit der rechten Maustaste auf eine bestimmte Verfügbarkeitsgruppe.  
  
4.  Wählen Sie den Befehl **Dashboard anzeigen** aus.  
  
 Informationen zur Verwendung des Always On-Dashboards finden Sie unter [Use the Always On Dashboard (SQL Server Management Studio) (Verwenden des Always On-Dashboards (SQL Server Management Studio))](~/database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md).  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> PowerShell  
 **Use Always On policies to view the health of an availability group**  
  
1.  Legen Sie mit **cd** eine Serverinstanz als Standard fest, auf der eines der Verfügbarkeitsreplikate gehostet wird. Verwenden Sie zum Anzeigen von Informationen zu allen Verfügbarkeitsreplikaten in einer Verfügbarkeitsgruppe die Serverinstanz, die das primäre Replikat hostet.  
  
2.  Verwenden Sie die folgenden Cmdlets:  
  
     **Test-SqlAvailabilityGroup**  
     Bewertet die Integrität einer Verfügbarkeitsgruppe durch die Auswertung der Richtlinien der richtlinienbasierten SQL Server-Verwaltung. Sie müssen über CONNECT-, VIEW SERVER STATE- und VIEW ANY DEFINITION-Berechtigungen verfügen, um dieses Cmdlet auszuführen.  
  
     Beispielsweise werden durch den folgenden Befehl alle Verfügbarkeitsgruppen im „Error“-Zustand auf der Serverinstanz angezeigt `Computer\Instance`.  
  
    ```  
    Get-ChildItem SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups `   
    | Test-SqlAvailabilityGroup | Where-Object { $_.HealthState -eq "Error" }  
    ```  
  
     **Test-SqlAvailabilityReplica**  
     Bewertet die Integrität von Verfügbarkeitsreplikaten durch die Auswertung der Richtlinien der richtlinienbasierten SQL Server-Verwaltung. Sie müssen über CONNECT-, VIEW SERVER STATE- und VIEW ANY DEFINITION-Berechtigungen verfügen, um dieses Cmdlet auszuführen.  
  
     Durch den folgenden Befehl werden beispielsweise die Integrität des Verfügbarkeitsreplikats `MyReplica` in der Verfügbarkeitsgruppe `MyAg` ausgewertet und eine kurze Zusammenfassung ausgegeben.  
  
    ```  
    Test-SqlAvailabilityReplica `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
     **Test-SqlDatabaseReplicaState**  
     Bewertet die Integrität einer Verfügbarkeitsdatenbank für alle hinzugefügten Verfügbarkeitsreplikate durch die Auswertung der Richtlinien der richtlinienbasierten SQL Server-Verwaltung.  
  
     Durch den folgenden Befehl werden beispielsweise die Integrität aller Verfügbarkeitsdatenbanken in der Verfügbarkeitsgruppe `MyAg` ausgewertet und eine kurze Zusammenfassung für jede Datenbank ausgegeben.  
  
    ```  
    Get-ChildItem SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\DatabaseReplicaStates `   
     | Test-SqlDatabaseReplicaState  
    ```  
  
     Diese Cmdlets akzeptieren die folgenden Optionen:  
  
    |Option|BESCHREIBUNG|  
    |------------|-----------------|  
    |**AllowUserPolicies**|Führt in den AlwaysOn-Richtlinienkategorien gefundene Benutzerrichtlinien aus.|  
    |**InputObject**|Eine Auflistung von Objekten, die abhängig vom verwendeten Cmdlet den Status von Verfügbarkeitsgruppen, Verfügbarkeitsreplikaten oder Verfügbarkeitsdatenbanken darstellen. Das Cmdlet berechnet die Integrität der angegebenen Objekte.|  
    |**NoRefresh**|Wenn dieser Parameter festgelegt wird, werden die vom **-Path** - oder **-InputObject** -Parameter angegebenen Objekte nicht manuell vom Cmdlet aktualisiert.|  
    |**Path**|Abhängig vom verwendeten Cmdlet der Pfad zur Verfügbarkeitsgruppe, zu den Verfügbarkeitsreplikaten oder zum Status des Datenbankreplikatclusters. Dies ist ein optionaler Parameter. Wird dieser Parameter nicht angegeben, wird der Wert standardmäßig auf den aktuellen Arbeitsstandort festgelegt.|  
    |**ShowPolicyDetails**|Zeigt das Ergebnis aller von diesem Cmdlet ausgeführten Richtlinienauswertungen an. Das Cmdlet gibt ein Objekt pro Richtlinienauswertung aus. Dieses Objekt verfügt über Felder, in denen die Ergebnisse der Auswertung beschrieben werden, z. B. ob die Richtlinie eingehalten wurde sowie den Richtliniennamen und die Kategorie.|  
  
     Beispielsweise gibt der folgende **Test-SqlAvailabilityGroup** -Befehl den **-ShowPolicyDetails** -Parameter an, um das Ergebnis aller von diesem Cmdlet ausgeführten Richtlinienauswertungen anzuzeigen, und zwar für jede einzelne Richtlinie der richtlinienbasierten Verwaltung (policy-based management (PBM)) die auf einer Verfügbarkeitsgruppe namens `MyAg`ausgeführt wurde.  
  
    ```  
    Test-SqlAvailabilityGroup `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\AgName `  
    -ShowPolicyDetails  
  
    ```  
  
    > [!NOTE]  
    >  Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Einrichten und Verwenden des SQL Server PowerShell-Anbieters**  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
 **SQL Server Always On Team Blogs-Monitoring Always On Health with PowerShell (SQL Server Always On Team Blogs: Überwachen der Always On-Integrität mit PowerShell):**  
  
-   [Teil 1: Übersicht über grundlegende Cmdlets](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-1-basic-cmdlet-overview)  
  
-   [Teil 2: Verwendung erweiterter Cmdlets](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-2-advanced-cmdlet-usage)  
  
-   [Teil 3: Eine einfache Überwachungsanwendung](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-3-a-simple-monitoring-application)  
  
-   [Teil 4: Integration in SQL Server-Agent](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-4-integration-with-sql-server-agent)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verwaltung einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server.md)   
 [Überwachen von Verfügbarkeitsgruppen (SQL Server)](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [Always On-Richtlinien für Betriebsprobleme mit Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)  
  
