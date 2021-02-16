---
title: 'Ändern von Metadaten: Clusterübergreifende Migration von Verfügbarkeitsgruppen'
description: Bei einer clusterübergreifenden Migration ändern Sie den Cluster, der die Metadaten für Verfügbarkeitsreplikate in einer Always On-Verfügbarkeitsgruppe verwaltet, indem Sie den HADR-Clusterkontext für eine Instanz von SQL Server ändern.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- Availability replicas [SQL Server], change WSFC cluster context
ms.assetid: ecd99f91-b9a2-4737-994e-507065a12f80
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7c7399af15e32dcc6234cfc62a1cdd08752c2d4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343785"
---
# <a name="change-which-cluster-manages-the-metadata-for-replicas-in-an-always-on-availability-group"></a>Ändern des Clusters, der die Metadaten für Replikate in einer Always On-Verfügbarkeitsgruppe verwaltet

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  In diesem Thema wird beschrieben, wie der HADR-Clusterkontext einer Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mit [!INCLUDE[tsql](../../../includes/tsql-md.md)] in [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)] und höheren Versionen gewechselt wird. Der *HADR-Clusterkontext* bestimmt, welcher Windows Server Failover Clustering-Cluster (WSFC) die Metadaten für von der Serverinstanz gehostete Verfügbarkeitsreplikate verwaltet.  
  
 Wechseln Sie den HADR-Clusterkontext nur während einer clusterübergreifenden Migration von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] zu einer Instanz von [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)] auf einem neuen WSFC-Cluster. Die Kreuzclustermigration von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] unterstützt ein Betriebssystemupgrade auf [!INCLUDE[win8](../../../includes/win8-md.md)] oder [!INCLUDE[win8srv](../../../includes/win8srv-md.md)] mit minimalen Ausfallzeiten der Verfügbarkeitsgruppen. Weitere Informationen finden Sie unter [Lösungen mit hoher Verfügbarkeit (SQL Server)](/previous-versions/sql/sql-server-2012/jj873730(v=msdn.10)).  
  
> [!CAUTION]  
>  Wechseln Sie den HADR-Clusterkontext nur während der clusterübergreifenden Migration von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Bereitstellungen.  
  
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Sie können mit dem HADR-Clusterkontext nur vom lokalen WSFC-Cluster zu einem Remotecluster und dann zurück vom Remotecluster zum lokalen Cluster wechseln. Sie können den HADR-Clusterkontext nicht von einem Remotecluster zu einem anderen Remotecluster umschalten.  
  
-   Der HADR-Clusterkontext kann nur dann zu einem Remotecluster umgeschaltet werden, wenn die Instanz von SQL-Server keine Verfügbarkeitsreplikate hostet.  
  
-   Ein Remote-HADR-Clusterkontext kann jederzeit zurück zum lokalen Cluster wechseln. Der Kontext kann jedoch nicht erneut gewechselt werden, solange die Serverinstanz Verfügbarkeitsreplikate hostet.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Die Serverinstanz, auf der Sie den HADR-Clusterkontext ändern, muss [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)] oder höher (ab Enterprise Edition) ausführen.  
  
-   Die Serverinstanz muss für Always On aktiviert sein. Weitere Informationen finden Sie unter [Aktivieren und Deaktivieren von Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).  
  
-   Damit eine Serverinstanz von einem lokalen Clusterkontext auf einen Remotecluster umgeschaltet werden kann, darf sie keine Verfügbarkeitsreplikate hosten. Die [sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md) -Katalogsicht sollte keine Zeilen zurückgeben.  
  
     Wenn auf der Serverinstanz Verfügbarkeitsreplikate vorhanden sind, bevor Sie den HADR-Clusterkontext ändern können, müssen Sie einen der folgenden Schritte ausführen:  
  
    |Replikatrolle|Aktion|Link|  
    |------------------|------------|----------|  
    |Primär|Schaltet die Verfügbarkeitsgruppe offline.|[Offlineschalten einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/take-an-availability-group-offline-sql-server.md)|  
    |Secondary|Entfernen des Replikats aus der Verfügbarkeitsgruppe|[Entfernen einer sekundären Replikats aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)|  
  
-   Bevor Sie von einem Remotecluster zum lokalen Cluster wechseln können, müssen alle Replikate mit synchronem Commit SYNCHRONIZED sein.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
  
-   Es wird empfohlen, dass Sie den vollständigen Domänennamen angeben. Der Grund hierfür ist, dass ALTER SERVER CONFIGURATION die Ziel-IP-Adresse eines Kurznamens mithilfe einer DNS-Auflösung sucht. In einigen Fällen, abhängig von der DNS-Suchreihenfolge, kann die Verwendung eines Kurznamens Verwirrung verursachen. Betrachten Sie zum Beispiel den folgenden Befehl, der für einen Knoten in der Domäne `abc` ausgeführt wird: (`node1.abc.com`). Der vorgesehene Zielcluster ist der `CLUS01` -Cluster in der Domäne `xyz` (`clus01.xyz.com`). Die lokale Domäne hostet jedoch auch einen Cluster mit dem Namen `CLUS01` (`clus01.abc.com`).  
  
     Wenn der Kurzname des Zielclusters, `CLUS01`, angegeben wird, kann die DNS-Namensauflösung die IP-Adresse des falschen Clusters, `clus01.abc.com`, zurückgeben. Um derartige Verwirrungen zu vermeiden, geben Sie den vollständigen Namen des Zielclusters an, wie im folgende Beispiel dargestellt:  
  
    ```  
    ALTER SERVER CONFIGURATION SET HADR CLUSTER CONTEXT = 'clus01.xyz.com'  
    ```  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
  
-   **SQL Server-Anmeldung**  
  
     Erfordert die CONTROL SERVER-Berechtigung.  
  
-   **SQL Server-Dienstkonto**  
  
     Das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienstkonto der Serverinstanz muss über Folgendes verfügen:  
  
    -   Berechtigung zum Öffnen des WSFC-Zielclusters.  
  
    -   Remote-WSFC-Lese-/Schreibzugriff.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So ändern Sie den WSFC-Clusterkontext eines Verfügbarkeitsreplikats**  
  
1.  Stellen Sie eine Verbindung mit der Serverinstanz her, die entweder das primäre Replikat oder ein sekundäres Replikat der Verfügbarkeitsgruppe hostet.  
  
2.  Verwenden Sie die SET HADR CLUSTER CONTEXT-Klausel der [ALTER SERVER CONFIGURATION](../../../t-sql/statements/alter-server-configuration-transact-sql.md) -Anweisung, wie nachfolgend aufgeführt:  
  
     ALTER SERVER CONFIGURATION SET HADR CLUSTER CONTEXT **=** { **'** _windows\_cluster_ **'** | LOCAL }  
  
     Erläuterungen:  
  
     *Windows-Cluster*  
     Der Clusterobjektname (CON) eines WSFC-Clusters. Sie können entweder den Kurznamen oder den vollständigen Domänennamen angeben. Es wird empfohlen, dass Sie den vollständigen Domänennamen angeben. Weitere Informationen finden Sie weiter oben in diesem Thema unter [Empfehlungen](#Recommendations).  
  
     LOCAL  
     Der lokale WSFC-Cluster.  
  
### <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der HADR-Clusterkontext in einen anderen Cluster geändert. Im Beispiel wird der vollständige Clusterobjektname, `clus01`, angegeben, um den Ziel-WSFC-Cluster `clus01.xyz.com`anzugeben.  
  
```  
ALTER SERVER CONFIGURATION SET HADR CLUSTER CONTEXT = 'clus01.xyz.com';  
```  
  
 Im folgenden Beispiel wird der HADR-Clusterkontext in den lokalen WSFC-Cluster geändert.  
  
```  
ALTER SERVER CONFIGURATION SET HADR CLUSTER CONTEXT = LOCAL;  
```  
  
##  <a name="follow-up-after-switching-the-cluster-context-of-an-availability-replica"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Wechseln des Clusterkontexts eines Verfügbarkeitsreplikats  
 Der neue HADR-Clusterkontext wird sofort wirksam, ohne die Serverinstanz neu starten zu müssen. Die Einstellung für den HADR-Clusterkontext ist eine persistente Einstellung auf Instanzebene, die unverändert bleibt, wenn die Serverinstanz neu gestartet wird.  
  
 Bestätigen Sie den neuen HADR-Clusterkontext, indem Sie die dynamische Verwaltungssicht [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md) wie folgt abfragen:  
  
```  
SELECT cluster_name FROM sys.dm_hadr_cluster  
```  
  
 Diese Abfrage sollte den Namen des Clusters zurückgeben, auf den Sie den HADR-Clusterkontext festgelegt haben.  
  
 Wenn der HADR-Clusterkontext zu einem neuen Cluster gewechselt wird:  
  
-   Die Metadaten werden für alle Verfügbarkeitsreplikate bereinigt, die gerade von der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz gehostet werden.  
  
-   Alle Datenbanken, die zuvor zu einem Verfügbarkeitsreplikat gehörten, befinden sich jetzt im Status RESTORING.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Entfernen eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md)  
  
-   [Offlineschalten einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/take-an-availability-group-offline-sql-server.md)  
  
-   [Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Entfernen einer sekundären Replikats aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   [Technische Artikel zu SQL Server 2012](https://msdn.microsoft.com/library/bb418445\(SQL.10\).aspx)  
  
-   [SQL Server Always On Team Blog: The official SQL Server Always On Team Blog (SQL Server Always On-Teamblog: Der offizielle SQL Server Always On-Teamblog)](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Windows Server-Failoverclustering &#40;WSFC&#41; mit SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)   
 [ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-configuration-transact-sql.md)  
  
