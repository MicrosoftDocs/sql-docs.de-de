---
title: Aktualisieren einer Failoverclusterinstanz
description: Schritte zum Aktualisieren einer SQL Server Always On-Failoverclusterinstanz mithilfe eines Installationsmediums Außerdem erhalten Sie Informationen zu parallelen Upgrades und zum Upgraden eines Clusters mit mehreren Subnetzen.
ms.custom: seo-lt-2019
ms.date: 11/21/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: conceptual
helpviewer_keywords:
- upgrading failover cluster instances
- clusters [SQL Server], upgrading
- failover clustering [SQL Server], upgrading
ms.assetid: daac41fe-7d0b-4f14-84c2-62952ad8cbfa
author: cawrites
ms.author: chadam
ms.openlocfilehash: 358006fa8eb00cdfc52a6d5a630f559ff86553f7
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100350031"
---
# <a name="upgrade-a-failover-cluster-instance"></a>Aktualisieren einer Failoverclusterinstanz 
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] unterstützt das Upgraden eines [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Failoverclusters auf eine neue Version von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], auf ein neues [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Service Pack oder ein kumulatives Update, wobei die Downtime auf ein einzelnes manuelles Failover (oder bei einem Failback auf den ursprünglichen primären Cluster auf zwei manuelle Failovers) beschränkt ist. Das Gleiche gilt, wenn ein neues Windows Service Pack oder kumulatives Windows-Update einzeln auf allen Failoverclusterknoten installiert wird.  

  
 Ein Upgrade für das Windows Server-Betriebssystem eines Knotens, der eine Failoverclusterinstanz enthält, wird für Betriebssysteme vor [!INCLUDE[winblue-server-2-md](../../../includes/winblue-server-2-md.md)] nicht unterstützt. Weitere Informationen zum Upgraden eines Windows Server-Failoverclusterknotens, der unter [!INCLUDE[winblue-server-2-md](../../../includes/winblue-server-2-md.md)] oder höher ausgeführt wird, finden Sie unter [Ausführen eines parallelen Upgrades oder Updates](#perform-a-rolling-upgrade-or-update).  
  
 Informationen zur Unterstützung:  
  
-   Ein [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Upgrade kann über die Benutzeroberfläche und über die Befehlszeile ausgeführt werden. Sie können das Upgrade auf jedem Failoverclusterknoten an der Eingabeaufforderung ausführen oder die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Setupbenutzeroberfläche verwenden, um die einzelnen Clusterknoten zu aktualisieren. Weitere Informationen finden Sie unter:

   - Installieren einer neuen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Failoverclusterinstanz
   - [Installieren von SQL Server von der Eingabeaufforderung](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)

-   Die folgenden Szenarien werden bei einem [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Upgrade nicht unterstützt:  
  
    -   Für eine eigenständige Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] können Sie kein Upgrade auf eine Failoverclusterinstanz ausführen.  
  
    -   Einer Failoverclusterinstanz können Sie keine Features hinzufügen. Beispielsweise können Sie [!INCLUDE[ssDE](../../../includes/ssde-md.md)] keiner vorhandenen ausschließlichen [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]-Failoverclusterinstanz hinzufügen.  
  
    -   Sie können auch kein Downgrade einer Failoverclusterinstanz auf irgendeinem Knoten des Windows Server-Failoverclusters auf eine eigenständige Instanz vornehmen.  
  
    -   Änderungen der Edition der Failoverclusterinstanz sind auf bestimmte Szenarien beschränkt. Weitere Informationen finden Sie unter [Unterstützte Versions- und Editionsupgrades](../../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
-   Während des Upgrades der Failoverclusterinstanz beschränkt sich die Ausfallzeit auf die Failoverzeit und die Ausführungszeit der Upgradeskripts. Wenn Sie ein paralleles Upgrade der Failoverclusterinstanz wie unten beschrieben ausführen und vor Beginn des Upgrades alle Voraussetzungen auf allen Knoten erfüllt sind, ist die Ausfallzeit minimal. Wenn während des Upgrades von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] speicheroptimierte Tabellen verwendet werden, erfordert das Upgrade zusätzliche Zeit. Weitere Informationen finden Sie unter [Planen und Testen des Upgradeplans für die Datenbank-Engine](../../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md).  
  
## <a name="prerequisites"></a>Voraussetzungen  
 Lesen Sie die folgenden wichtigen Informationen, bevor Sie beginnen:  
  
-   [Unterstützte Versions- und Editionsupgrades:](../../../database-engine/install-windows/supported-version-and-edition-upgrades.md) Überprüfen Sie, ob Sie von Ihrer Version des Windows-Betriebssystems und Ihrer Version von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auf die gewünschte Version von [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] upgraden können. Sie können beispielsweise kein direktes Upgrade von einer SQL Server 2005-Failoverclusterinstanz auf [!INCLUDE [sssql14-md](../../../includes/sssql14-md.md)] ausführen und keine Failoverclusterinstanz upgraden, die unter [!INCLUDE[winxpsvr-md](../../../includes/winxpsvr-md.md)] ausgeführt wird.  
  
-   [Auswählen einer Upgrademethode für die Datenbank-Engine](../../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md): Wählen Sie basierend auf Ihrer Prüfung der unterstützten Versions- und Editionsupgrades sowie basierend auf den anderen in Ihrer Umgebung installierten Komponenten die passende Upgrademethode und die passenden Upgradeschritte aus, um das Upgrade der Komponenten in der richtigen Reihenfolge durchzuführen.  
  
-   [Planen und Testen des Upgradeplans für die Datenbank-Engine](../../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md): Überprüfen Sie die Anmerkungen zu dieser Version, die bekannten Upgradeprobleme und die Prüfliste vor dem Upgrade. Entwickeln und testen Sie dann den Upgradeplan.  
  
-   [Hardware- und Softwareanforderungen für die Installation von SQL Server:](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)  Überprüfen Sie die Softwareanforderungen für die Installation von [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Falls zusätzliche Software erforderlich ist, installieren Sie diese auf jedem Knoten, bevor Sie mit dem Upgradevorgang beginnen, um die Downtime zu minimieren.  
  
## <a name="perform-a-rolling-upgrade-or-update"></a>Ausführen eines parallelen Upgrades oder Updates  
 Verwenden Sie für das Upgrade einer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Failoverclusterinstanz das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Setup, um nacheinander für jeden Knoten in der Failoverclusterinstanz das Upgrade auszuführen. Beginnen Sie dabei mit den passiven Knoten. Beim Upgrade der einzelnen Knoten wird der jeweilige Knoten nicht als möglicher Besitzer der Failoverclusterinstanz angezeigt. Im Fall eines unerwarteten Failovers können die aktualisierten Knoten erst dann im Failover verwendet werden, wenn das Setup den Besitz der Windows Server-Failoverclusterrolle zu einem aktualisierten Knoten verschiebt.  
  
 In der Standardeinstellung wird vom [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setup automatisch festgelegt, wann ein Failover zu einem aktualisierten Knoten auszuführen ist. Dies ist von der Gesamtanzahl der Knoten in der Failoverclusterinstanz und der Anzahl der bereits aktualisierten Knoten abhängig. Wenn bereits mindestens die Hälfte der Knoten aktualisiert wurde und Sie das Upgrade für den nächsten Knoten ausführen, löst das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setup einen Failover zu einem aktualisierten Knoten aus. Nach dem Failover zu einem aktualisierten Knoten wird die Clustergruppe in einen aktualisierten Knoten verschoben. Alle aktualisierten Knoten werden in die Liste möglicher Besitzer eingefügt. Alle Knoten, die noch nicht aktualisiert wurden, werden aus der Liste möglicher Besitzer entfernt. Jeder verbleibende Knoten, den Sie aktualisieren, wird der Liste der möglichen Besitzer der Failoverclusterinstanz hinzugefügt.  
  
 Durch diesen Vorgang wird die Ausfallzeit während der gesamten Failoverclusteraktualisierung auf eine Failoverzeit und die Ausführungszeit für Aktualisierungsskripts begrenzt.  
  
 Führen Sie zum Steuern des Failoververhaltens von Clusterknoten während des Upgradevorgangs den Upgradevorgang über die Eingabeaufforderung aus, und verwenden Sie dabei den /FAILOVERCLUSTERROLLOWNERSHIP-Parameter. Weitere Informationen finden Sie unter [Installieren von SQL Server über die Eingabeaufforderung](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).  

 ## <a name="upgrade-with-installation-media"></a>Upgrade mit Installationsmedium 
  
1.  Doppelklicken Sie in den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Installationsmedien für die Edition, die der Edition entspricht, die Sie aktualisieren, im Stammordner auf „setup.exe“. Möglicherweise werden Sie aufgefordert, die erforderlichen Komponenten zu installieren, falls sie nicht bereits installiert wurden.  
  
2.  Nachdem die erforderlichen Komponenten installiert wurden, startet der Installations-Assistent das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Installationscenter. Wählen Sie zum Aktualisieren einer vorhandenen Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Ihre Instanz aus.  
  
3.  Wenn [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setup-Unterstützungsdateien erforderlich sind, werden diese durch das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setup installiert. Wenn Sie zum Neustarten des Computers aufgefordert werden, führen Sie einen Neustart durch, bevor Sie den Vorgang fortsetzen.  
  
4.  Die Systemkonfigurationsprüfung führt einen Ermittlungsvorgang auf dem Computer aus. [!INCLUDE[clickOK](../../../includes/clickok-md.md)], um den Vorgang fortzusetzen.  
  
5.  Geben Sie auf der Seite Product Key den PID-Schlüssel für die neue Versionsedition ein, die der Edition der alten Produktversion entspricht. Wenn Sie z. B. einen Enterprise-Failovercluster aktualisieren möchten, müssen Sie einen PID-Schlüssel für [!INCLUDE[ssEnterprise](../../../includes/ssenterprise-md.md)]angeben. Klicken Sie auf zum Fortfahren auf **Weiter**. Der für ein Failoverclusterupgrade verwendete PID-Schlüssel muss für alle Failoverclusterknoten einer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz einheitlich sein.  
  
6.  Lesen Sie auf der Seite mit den Lizenzbedingungen den Lizenzvertrag, und aktivieren Sie dann das Kontrollkästchen, um den Lizenzbestimmungen zuzustimmen. Falls Sie zur Verbesserung von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]beitragen möchten, können Sie auch die Option zur Funktionsverwendung aktivieren und Berichte an [!INCLUDE[msCoName](../../../includes/msconame-md.md)]senden. **Klicken Sie auf „Weiter“, um den Vorgang fortzusetzen**. Klicken Sie auf **Abbrechen**, wenn Sie den Setupvorgang beenden möchten.  
  
7.  Geben Sie auf der Seite Instanz auswählen die Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] an, die aktualisiert werden soll. **Klicken Sie auf „Weiter“, um den Vorgang fortzusetzen**.  
  
8.  Auf der Seite Funktionsauswahl sind die zu aktualisierenden Funktionen bereits markiert. Nach Auswahl des Funktionsnamens wird im rechten Bereich eine Beschreibung für die einzelnen Komponentengruppen angezeigt. Sie können die zu aktualisierenden Funktionen nicht ändern und während des Aktualisierungsvorgangs keine Funktionen hinzufügen. Wenn Sie einer aktualisierten Instanz von [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] Funktionen hinzufügen möchten, nachdem der Upgradevorgang abgeschlossen wurde, finden Sie entsprechende Informationen unter [Hinzufügen von Funktionen zu einer Instanz von SQL Server 2016 &#40;Setup&#41;](../../../database-engine/install-windows/add-features-to-an-instance-of-sql-server-setup.md).  
  
     Die erforderlichen Komponenten für die ausgewählten Funktionen werden im rechten Bereich angezeigt. SQL Server-Setup installiert die erforderlichen Komponenten, die nicht bereits während des im weiteren Verlauf dieser Prozedur beschriebenen Installationsschritts installiert werden. Um Zeit zu sparen, sollten Sie die erforderlichen Komponenten im Voraus auf jedem Knoten installieren.  
  
9. Auf der Seite Instanzkonfiguration werden die Felder automatisch aus der alten Instanz aufgefüllt. Sie können optional auch den neuen InstanceID-Wert angeben.  
  
     **Instanz-ID** – Standardmäßig wird der Instanzname als Instanz-ID verwendet. So werden Installationsverzeichnisse und Registrierungsschlüssel für die Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]identifiziert. Dies ist der Fall für Standardinstanzen und benannte Instanzen. Bei einer Standardinstanz lauten Instanzname und Instanz-ID MSSQLSERVER. Wenn Sie nicht die Standardinstanz-ID verwenden möchten, aktivieren Sie das Kontrollkästchen **Instanz-ID** , und geben Sie einen Wert ein. Wenn Sie den Standardwert überschreiben, müssen Sie die gleiche Instanz-ID für die Instanz angeben, die in allen Failoverclusterknoten aktualisiert wird. Die Instanz-ID für die aktualisierte Instanz muss in allen Knoten übereinstimmen.  
  
     **Ermittelte Instanzen und Funktionen** : Das Raster zeigt Instanzen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , die sich auf dem Computer befinden, auf dem das Setup ausgeführt wird. **Klicken Sie auf „Weiter“, um den Vorgang fortzusetzen**.  
  
10. Auf der Seite Erforderlicher Speicherplatz wird der für die angegebenen Funktionen erforderliche Speicherplatz berechnet, und die Anforderungen werden mit dem Speicherplatz verglichen, der auf dem Computer verfügbar ist, auf dem Setup ausgeführt wird.  
  
11. Geben Sie auf der Seite Aktualisieren der Volltextsuche die Aktualisierungsoptionen für die Datenbanken an, die aktualisiert werden sollen. Weitere Informationen finden Sie unter [Upgradeoptionen für die Volltextsuche](../../../database-engine/install-windows/install-sql-server.md).  
  
12. Geben Sie auf der Seite **Fehlerberichterstellung** die Informationen an, die Sie an [!INCLUDE[msCoName](../../../includes/msconame-md.md)] senden möchten, um zur Verbesserung von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]beizutragen. Die Option für Fehlerberichte ist standardmäßig aktiviert.  
  
13. Bei der Systemkonfigurationsprüfung wird eine weitere Reihe von Regeln ausgeführt, um die Konfiguration des Computers anhand der von Ihnen angegebenen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Funktionen zu überprüfen, bevor der Aktualisierungsvorgang beginnt.  
  
14. Auf der Seite Clusteraktualisierungsbericht werden die Liste mit den Knoten in der Failoverclusterinstanz und die Instanzversionsinformationen für die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Komponenten in jedem Knoten angezeigt. Der Datenbankskriptstatus und der Replikationsskriptstatus werden angezeigt. Außerdem werden Informationsmeldungen zu den Vorgängen angezeigt, die ausgeführt werden, wenn Sie auf **Weiter** klicken. Abhängig von der Anzahl bereits aktualisierter Failoverclusterknoten und der Gesamtzahl an Knoten zeigt Setup das Failoververhalten an, das ausgeführt wird, wenn Sie auf **Weiter** klicken. Es werden auch Warnungen bei möglicher unnötiger Ausfallzeit angezeigt, wenn die erforderlichen Komponenten noch nicht installiert sind.   
  
15. Auf der Seite Das Upgrade kann jetzt ausgeführt werden wird eine Strukturansicht der Installationsoptionen angezeigt, die während des Setups angegeben wurden. Klicken Sie zum Fortsetzen des Vorgangs auf **Aktualisieren**. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setup installiert zuerst die erforderlichen Komponenten für die ausgewählten Funktionen und dann die Funktionen.  
  
16. Während des Upgrades wird auf der Seite Status der Status angezeigt, sodass Sie während des Setupvorgangs den Upgradefortschritt überwachen können.  
  
17. Nach der Aktualisierung des aktuellen Knotens werden auf der Seite Clusteraktualisierungsbericht Statusinformationen zur Aktualisierung der einzelnen Failoverclusterknoten sowie die Funktionen in jedem Failoverclusterknoten und ihre Versionsinformationen angezeigt. Bestätigen Sie die angezeigten Versionsinformationen, und fahren Sie mit der Aktualisierung der verbleibenden Knoten fort. Wenn das Failover zu aktualisierten Knoten aufgetreten ist, wird dies ebenfalls auf der Statusseite angezeigt. Sie können diese Informationen zur Bestätigung außerdem im Windows-Clusterverwaltungstool überprüfen.  
  
18. Nach der Aktualisierung werden auf der Seite Abgeschlossen ein Link zur zusammenfassenden Protokolldatei für die Installation sowie weitere wichtige Hinweise bereitgestellt. Klicken Sie auf **Schließen**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , um die Installation von abzuschließen.  
  
19. Starten Sie den Computer neu, falls Sie dazu aufgefordert werden. Wenn Sie den Setupvorgang abgeschlossen haben, sollten Sie unbedingt die vom Installations-Assistenten angezeigte Meldung lesen. Weitere Informationen zu Setupprotokolldateien finden Sie unter [Lesen und Anzeigen der Setupprotokolldateien von SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
20. Wiederholen Sie diese Schritte für alle anderen Knoten der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Failoverclusterinstanz, um den Upgradevorgang abzuschließen.  
  
## <a name="upgrade-a-multi-subnet-failover-cluster-instance"></a>Upgrade einer Multisubnetz-Failoverclusterinstanz  

Führen Sie die folgenden Schritte aus, um Ihre Always On-Failoverclusterinstanz in einer Multisubnetzumgebung zu aktualisieren. 
  
### <a name="to-upgrade-to-a-ssnoversion-multi-subnet-failover-cluster-instance-existing-ssnoversion-cluster-is-a-non-multi-subnet-cluster"></a>Upgrade auf eine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Multisubnetz-Failoverclusterinstanz (vorhandener [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Cluster ist kein Multisubnetzcluster)  
  
1.  Führen Sie die oben beschriebenen Schritte für das Upgrade Ihrer Failoverclusterinstanz aus.  
  
2.  Fügen Sie mit der Setupaktion „AddNode“ einen Knoten in einem anderen Subnetz hinzu, und bestätigen Sie auf der Seite **Netzwerkkonfiguration für Cluster** die Änderung der IP-Adressabhängigkeit in „OR“. Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen von Knoten in einer Always On-Failoverclusterinstanz (Setup)](../install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
### <a name="to-upgrade-a-multi-subnet-failover-cluster-instance-currently-using-stretch-vlan-to-use-multi-subnet"></a>Upgrade einer Multisubnetz-Failoverclusterinstanz, die derzeit Stretch-VLAN verwendet, auf die Verwendung von Multisubnetzen  
  
1.  Führen Sie die oben beschriebenen Schritte aus, um Ihren Cluster zu aktualisieren.  
  
2.  Ändern Sie die Netzwerkeinstellungen, um den Remoteknoten in ein anderes Subnetz zu verschieben.  
  
3.  Fügen Sie mit dem Failovercluster-Manager oder mit PowerShell eine neue IP-Adresse für das neue Subnetz hinzu, und legen Sie die IP-Adressabhängigkeit auf „OR“ fest.  
  
## <a name="next-steps"></a>Nächste Schritte  
 Nach dem Upgrade führen Sie die folgenden Tasks aus:  
  
-   [Abschließen des Datenbank-Engine-Upgrades](../../../database-engine/install-windows/complete-the-database-engine-upgrade.md)  
  
-   [Ändern des Datenbank-Kompatibilitätsmodus und Verwenden des Abfragespeichers](../../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)  
  
-   [Nutzen Sie die Vorteile der neuen Features von SQL Server 2016](../../what-s-new-in-sql-server-2017.md)  
  

  
