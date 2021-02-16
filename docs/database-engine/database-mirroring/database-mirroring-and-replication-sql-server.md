---
title: Datenbankspiegelung und Replikation (SQL Server) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Datenbankspiegelung zusammen mit der Replikation verwenden, um die Verfügbarkeit für die Veröffentlichungsdatenbank in SQL Server zu verbessern.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- replication [SQL Server], database mirroring and
ms.assetid: 82796217-02e2-4bc5-9ab5-218bae11a2d6
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 16746834acfe8a53511166a35667f136b561c28a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353182"
---
# <a name="database-mirroring-and-replication-sql-server"></a>Datenbankspiegelung und Replikation (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Die Datenbankspiegelung kann in Verbindung mit der Replikation verwendet werden, um die Verfügbarkeit der Veröffentlichungsdatenbank zu verbessern. Bei der Datenbankspiegelung sind zwei Kopien einer einzigen Datenbank vorhanden, die normalerweise auf verschiedenen Computern gespeichert sind. Für die Clients ist immer nur eine Kopie der Datenbank verfügbar. Diese Kopie wird als Prinzipaldatenbank bezeichnet. Updates, die Clients an der Prinzipaldatenbank vornehmen, werden in der anderen Kopie der Datenbank angewendet, die als Spiegeldatenbank bezeichnet wird. Beim Spiegeln wird das Transaktionsprotokoll von jedem Einfüge-, Update- oder Löschvorgang, der an der Prinzipaldatenbank vorgenommen wird, auf die Spiegeldatenbank angewandt.  
  
 Ein Failover der Replikation zu einer Spiegeldatenbank wird vollständig für Veröffentlichungsdatenbanken unterstützt, und zwar mit einer eingeschränkten Unterstützung für Verteilungs- oder Abonnementdatenbanken. Die Datenbankspiegelung wird für die Verteilerdatenbank nicht unterstützt. Informationen zum Wiederherstellen einer Verteilungsdatenbank oder einer Abonnementdatenbank, ohne dass dazu die Replikation neu konfiguriert werden muss, finden Sie unter [Sichern und Wiederherstellen von replizierten Datenbanken](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md).   
  
> [!NOTE]  
>  Nach einem Failover wird die Spiegeldatenbank zur Prinzipaldatenbank. In diesem Thema bezeichnen die Begriffe "Prinzipal" und "Spiegel" immer die ursprüngliche Prinzipal- bzw. Spiegeldatenbank.  
  
## <a name="requirements-and-considerations-for-using-replication-with-database-mirroring"></a>Anforderungen und Überlegungen zur Verwendung von Replikation mit Datenbankspiegelung  
 Beachten Sie die folgenden Anforderungen und Überlegungen, wenn Sie die Replikation zusammen mit der Datenbankspiegelung verwenden wollen:  
  
-   Die Prinzipal- und die Spiegeldatenbank müssen einen gemeinsamen Verteiler verwenden. Es wird empfohlen, dafür einen Remoteverteiler zu verwenden, der im Falle eines ungeplanten Failovers des Verlegers eine größere Fehlertoleranz aufweist.  
  
-   Die Replikation unterstützt das Spiegeln der Veröffentlichungsdatenbank zur Mergereplikation und zur Transaktionsreplikation mit schreibgeschützten Abonnenten oder Abonnenten mit verzögertem Update über eine Warteschlange. Abonnenten mit sofortigem Update, Oracle-Verleger, Verleger in einer Peer-zu-Peer-Topologie und das erneute Veröffentlichen werden nicht unterstützt.  
  
-   Metadaten und Objekte, die außerhalb der Datenbank vorhanden sind, werden nicht in die Spiegeldatenbank kopiert. Das betrifft Benutzernamen, Aufträge, Verbindungsserver usw. Wenn Sie die Metadaten in der Spiegeldatenbank benötigen, müssen Sie diese manuell kopieren. Weitere Informationen finden Sie unter [Verwaltung von Anmeldenamen und Aufträgen nach einem Rollenwechsel &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md).  
  
## <a name="configuring-replication-with-database-mirroring"></a>Konfigurieren der Replikation mit Datenbankspiegelung  
 Das Konfigurieren der Replikation mit Datenbankspiegelung umfasst fünf Schritte. Jeder dieser Schritte wird im folgenden Abschnitt detailliert beschrieben.  
  
1.  Konfigurieren Sie den Verleger.  
  
2.  Konfigurieren Sie die Datenbankspiegelung.  
  
3.  Konfigurieren Sie die Spiegeldatenbank so, dass sie denselben Verteiler wie die Prinzipaldatenbank verwendet.  
  
4.  Konfigurieren Sie den Replikations-Agent für das Failover.  
  
5.  Fügen Sie die Prinzipal- und die Spiegeldatenbank zum Replikationsmonitor hinzu.  
  
 Schritt 1 und 2 können auch in umgekehrter Reihenfolge durchgeführt werden.  
  
#### <a name="to-configure-database-mirroring-for-a-publication-database"></a>So konfigurieren Sie die Datenbankspiegelung für eine Veröffentlichungsdatenbank  
  
1.  Konfigurieren Sie den Verleger:  
  
    1.  Die Verwendung eines Remoteverteilers wird empfohlen. Weitere Informationen zum Konfigurieren der Verteilung finden Sie unter [Konfigurieren der Verteilung](../../relational-databases/replication/configure-distribution.md).  
  
    2.  Sie können eine Datenbank für die Momentaufnahme- und Transaktionsveröffentlichungen und/oder für Mergeveröffentlichungen aktivieren. Bei gespiegelten Datenbanken, die mehrere Veröffentlichungstypen enthalten, müssen Sie die Datenbanken mit [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)für beide Typen auf demselben Knoten aktivieren. Beispielsweise können Sie die folgenden gespeicherten Prozeduraufrufe für die Prinzipaldatenbank ausführen:  
  
        ```  
        exec sp_replicationdboption @dbname='<PublicationDatabase>', @optname='publish', @value=true;  
        exec sp_replicationdboption @dbname='<PublicationDatabase>', @optname='mergepublish', @value=true;  
        ```  
  
         Weitere Informationen zum Erstellen von Veröffentlichungen finden Sie unter [Veröffentlichen von Daten und Datenbankobjekten](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
2.  Konfigurieren Sie die Datenbankspiegelung. Weitere Informationen finden Sie unter [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md) und [Einrichten der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
3.  Konfigurieren Sie die Verteilung für die Spiegeldatenbank. Geben Sie den Namen der Spiegeldatenbank als Verleger an, und geben Sie denselben Verteiler- und Momentaufnahmeordner an, den auch die Prinzipaldatenbank verwendet. Wenn Sie z. B. eine Replikation mit gespeicherten Prozeduren konfigurieren, führen Sie auf dem Verteiler [sp_adddistpublisher](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md) aus. Führen Sie anschließend auf der Spiegeldatenbank [sp_adddistributor](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) aus. Für **sp_adddistpublisher**:  
  
    -   Legen Sie den Wert des **\@publisher**-Parameters auf den Netzwerknamen der Spiegeldatenbank fest.  
  
    -   Legen Sie den Wert des **\@working_directory**-Parameters auf den Momentaufnahmeordner fest, der von dem Prinzipal verwendet wird.  
  
4.  Geben Sie den Spiegeldatenbanknamen für den Parameter **-PublisherFailoverPartner** des Agents an. Dieser Parameter ist für die folgenden Agents erforderlich, um die Spiegeldatenbank nach einem Failover zu identifizieren:  
  
    -   Momentaufnahme-Agent (für alle Veröffentlichungen)  
  
    -   Protokolllese-Agent (für alle Transaktionsveröffentlichungen)  
  
    -   Warteschlangenlese-Agent (für Transaktionsveröffentlichungen, die Abonnments mit verzögertem Update über eine Warteschlange unterstützen)  
  
    -   Merge-Agent (für Mergeabonnements)  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Replikationsüberwachung (replisapi.dll: für Mergeabonnements, die per Websynchronisierung synchronisiert werden)  
  
    -   SQL Merge-ActiveX-Steuerelement (für Mergeabonnements, die über dieses Steuerelement synchronisiert werden)  
  
     Der Verteilungs-Agent und das Verteilungs-ActiveX-Steuerelement weisen diesen Parameter nicht auf, weil sie keine Verbindung mit dem Verleger herstellen.  
  
     Die Änderungen der Agentparameter treten in Kraft, wenn der Agent das nächste Mal gestartet wird. Wenn der Agent ständig ausgeführt wird, müssen Sie den Agent beenden und neu starten. Parameter können in Agentprofilen und in der Befehlszeile angegeben werden. Weitere Informationen finden Sie unter  
  
    -   [Anzeigen und Ändern von Befehlszeilenparametern des Replikations-Agents &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
    -   [Ausführbare Konzepte für die Programmierung von Replikations-Agent](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  
     Es wird empfohlen, zunächst den Parameter **-PublisherFailoverPartner** zu einem Agent-Profil hinzuzufügen und anschließend den Spiegeldatenbanknamen im Profil anzugeben. Wenn Sie z. B. die Replikation mit gespeicherten Prozeduren konfigurieren:  
  
    ```  
    -- Execute sp_help_agent_profile in the context of the distribution database to get the list of profiles.  
    -- Select the profile id of the profile that needs to be updated from the result set.  
    -- In the agent_type column returned by sp_help_agent_profile:   
    -- 1 = Snapshot Agent; 2 = Log Reader Agent; 3 = Distribution Agent; 4 = Merge Agent; 9 = Queue Reader Agent.  
  
    exec sp_help_agent_profile;  
  
    -- Setting the -PublisherFailoverPartner parameter in the default Snapshot Agent profile (profile 1).  
    -- Execute sp_add_agent_parameter in the context of the distribution database.  
    exec sp_add_agent_parameter @profile_id = 1, @parameter_name = N'-PublisherFailoverPartner', @parameter_value = N'<Failover Partner Name>';  
  
    -- Setting the -PublisherFailoverPartner parameter in the default Merge Agent profile (profile 6).  
    -- Execute sp_add_agent_parameter in the context of the distribution database.  
    exec sp_add_agent_parameter @profile_id = 6, @parameter_name = N'-PublisherFailoverPartner', @parameter_value = N'<Failover Partner Name>';  
    ```  
  
5.  Fügen Sie die Prinzipal- und die Spiegeldatenbank zum Replikationsmonitor hinzu. Weitere Informationen finden Sie unter [Hinzufügen und Entfernen von Verlegern vom Replikationsmonitor aus](../../relational-databases/replication/monitor/add-and-remove-publishers-from-replication-monitor.md).  
  
## <a name="maintaining-a-mirrored-publication-database"></a>Warten einer gespiegelten Veröffentlichungsdatenbank  
 Das Warten einer gespiegelten Veröffentlichungsdatenbank gleicht im Wesentlichen dem Warten einer nicht gespiegelten Datenbank, wobei jedoch die folgenden Überlegungen zu berücksichtigen sind:  
  
-   Die Verwaltung und Überwachung muss auf dem aktiven Server erfolgen. In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]werden Veröffentlichungen nur für den aktiven Server im Ordner **Lokale Veröffentlichungen** angezeigt. Wenn z. B. ein Failover zur Spiegeldatenbank erfolgt ist, werden die Veröffentlichungen für die Spiegeldatenbank und nicht mehr für die Prinzipaldatenbank angezeigt. Wenn ein Failover der Datenbank zur Spiegeldatenbank erfolgt, müssen [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] und der Replikationsmonitor ggf. manuell aktualisiert werden, damit die Änderung berücksichtigt wird.  
  
-   Der Replikationsmonitor zeigt Verlegerknoten in der Objektstruktur für die Prinzipaldatenbank und die Spiegeldatenbank an. Wenn der Prinzipalserver der aktive Server ist, werden die Veröffentlichungsinformationen nur unter dem Prinzipalknoten im Replikationsmonitor angezeigt.  
  
     Wenn der Spiegelserver der aktive Server ist, geschieht Folgendes:  
  
    -   Wenn in einem Agent ein Fehler auftritt, wird der Fehler nur im Prinzipalknoten und nicht im Spiegelknoten angegeben.  
  
    -   Wenn der Prinzipalserver nicht verfügbar ist, werden für den Prinzipal- und den Spiegelknoten identische Listen von Veröffentlichungen angezeigt. Die Überwachung sollte für die Veröffentlichungen unter dem Spiegelknoten erfolgen.  
  
-   Beim Verwenden gespeicherter Prozeduren oder Replikationsverwaltungsobjekte (RMO, Replication Management Objects) zum Verwalten der Replikation in der Spiegeldatenbank müssen Sie in Fällen, in denen Sie den Verlegernamen angeben, den Namen der Instanz angeben, auf der die Datenbank für die Replikation aktiviert wurde. Den entsprechenden Namen ermitteln Sie mithilfe der [publishingservername](../../t-sql/functions/replication-functions-publishingservername.md)-Funktion.  
  
     Wenn eine Veröffentlichungsdatenbank gespiegelt wird, sind die in der gespiegelten Datenbank gespeicherten Replikationsmetadaten identisch mit den Metadaten, die in der Prinzipaldatenbank gespeichert sind. Demzufolge entspricht für Veröffentlichungsdatenbanken, die in der Prinzipaldatenbank zur Replikation aktiviert wurden, der in den Systemtabellen gespeicherte Verlegerinstanzname dem Namen der Prinzipaldatenbank und nicht dem Namen der Spiegeldatenbank. Dies wirkt sich auf die Replikationskonfiguration und -wartung aus, wenn ein Failover der Veröffentlichungsdatenbank zur Spiegeldatenbank erfolgt. Wenn Sie z. B. nach einem Failover zur Spiegeldatenbank die Replikation mit gespeicherten Prozeduren konfigurieren und ein Pullabonnement für eine Veröffentlichungsdatenbank hinzufügen wollen, die in der Prinzipaldatenbank aktiviert wurde, müssen Sie für den **\@publisher**-Parameter von **sp_addpullsubscription** oder **sp_addmergepullsubscription** nicht den Spiegeldatenbanknamen, sondern den Prinzipaldatenbanknamen angeben.  
  
     Wenn Sie nach einem Failover zur Spiegeldatenbank eine Veröffentlichungsdatenbank in der Spiegeldatenbank aktivieren, entspricht der Verlegerinstanzname in den Systemtabellen dem Namen der Spiegeldatenbank. In diesem Fall würden Sie den Namen der Spiegeldatenbank für den **\@publisher**-Parameter verwenden.  
  
    > [!NOTE]  
    >  In einigen Fällen, z.B. bei **sp_addpublication**, wird der **\@publisher**-Parameter nur für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verleger unterstützt. In diesen Fällen ist er nicht relevant für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbankspiegelung.  
  
-   Synchronisieren Sie die Pullabonnements vom Abonnenten und die Pushabonnements vom aktiven Verleger, um ein Abonnement in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] nach einem Failover zu synchronisieren.  
  
### <a name="replication-behavior-if-mirroring-is-removed"></a>Replikationsverhalten nach dem Entfernen der Spiegelung  
 Beachten Sie beim Entfernen der Spiegelung von einer veröffentlichten Datenbank Folgendes:  
  
-   Wenn die Spiegelung der Veröffentlichungsdatenbank in der Prinzipaldatenbank deaktiviert wird, ist die Replikation unverändert für die ursprüngliche Prinzipaldatenbank funktionsfähig.  
  
-   Wenn ein Failover der Veröffentlichungsdatenbank von der Prinzipaldatenbank zur Spiegeldatenbank erfolgt und anschließend die Spiegelungsbeziehung deaktiviert oder entfernt wird, sind die Replikations-Agents nicht für die Spiegeldatenbank funktionsfähig. Wenn die Prinzipaldatenbank dauerhaft verloren geht, deaktivieren Sie die Replikation, und konfigurieren Sie sie anschließend neu, wobei Sie die Spiegeldatenbank als Verleger angeben.  
  
-   Wenn die Datenbankspiegelung vollständig entfernt wird, befindet sich die Spiegeldatenbank in einem Wiederherstellungsstatus und muss wiederhergestellt werden, um funktionsfähig zu werden. Das Verhalten der wiederhergestellten Datenbank im Hinblick auf die Replikation richtet sich danach, ob die KEEP_REPLICATION-Option angegeben wurde. Diese Option weist den Wiederherstellungsvorgang an, die Replikationseinstellungen beizubehalten, wenn eine veröffentlichte Datenbank auf einem Server wiederhergestellt wird, der nicht mit dem Server identisch ist, auf dem die Datenbank erstellt wurde. Verwenden Sie die KEEP_REPLICATION-Option nur dann, wenn die andere Veröffentlichungsdatenbank nicht verfügbar ist. Die Option wird nicht unterstützt, wenn die andere Veröffentlichungsdatenbank noch intakt ist und weiter repliziert. Weitere Informationen zu KEEP_REPLICATION finden Sie unter [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
## <a name="log-reader-agent-behavior"></a>Verhalten des Protokolllese-Agents  
 Die folgende Tabelle beschreibt das Verhalten des Protokolllese-Agents für die verschiedenen Betriebsmodi der Datenbankspiegelung.  
  
|Betriebsmodus|Verhalten des Protokolllese-Agents, wenn die Spiegeldatenbank nicht verfügbar ist|  
|--------------------|------------------------------------------------------------|  
|Modus für hohe Sicherheit mit automatischem Failover|Wenn die Spiegeldatenbank nicht verfügbar ist, gibt der Protokolllese-Agent die Befehle an die Verteilungsdatenbank weiter. Ein Failover der Prinzipaldatenbank zur Spiegeldatenbank ist solange nicht möglich, bis die Spiegeldatenbank wieder online ist und alle Transaktionen von der Prinzipaldatenbank erhalten hat.|  
|Modus mit hoher Leistung|Wenn die Spiegeldatenbank nicht verfügbar ist, läuft sie ungeschützt (das heißt, sie wird nicht gespiegelt). Der Protokolllese-Agent repliziert jedoch nur solche Transaktionen, die in der Spiegeldatenbank festgeschrieben wurden. Wenn der Dienst erzwungen wird und der Spiegelserver die Rolle der Prinzipaldatenbank übernimmt, arbeitet der Protokolllese-Agent für die Spiegeldatenbank und beginnt mit dem Übernehmen der neuen Transaktionen.<br /><br /> Beachten Sie, dass die Replikationslatenzzeit steigt, wenn die Spiegeldatenbank hinter die Prinzipaldatenbank zurückfällt.|  
|Modus für hohe Sicherheit ohne automatisches Failover|Für alle Transaktionen, für die ein Commit ausgeführt wird, wird sichergestellt, dass sie auf dem Datenträger in der Spiegeldatenbank festgeschrieben werden. Der Protokolllese-Agent repliziert jedoch nur solche Transaktionen, die in der Spiegeldatenbank festgeschrieben wurden. Wenn die Spiegeldatenbank nicht verfügbar ist, deaktiviert die Prinzipaldatenbank jede weitere Aktivität in der Datenbank, weshalb der Protokolllese-Agent keine Transaktionen replizieren muss.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server-Replikation](../../relational-databases/replication/sql-server-replication.md)   
 [Protokollversand und Replikation &#40;SQL Server&#41;](../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md)  
  
  
