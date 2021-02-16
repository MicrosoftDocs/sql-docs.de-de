---
title: Suchen nach Fehlern bei der Transaktionsreplikation
description: Hier wird beschrieben, wie Sie Fehler bei der Transaktionsreplikation identifizieren. Der Artikel erläutert auch die Methodik für die Behandlung von Problemen bei der Replikation.
ms.custom: seo-lt-2019
ms.date: 07/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], tutorials
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: d39286c1bc228a9da76a163a39e76dbb2022d807
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348645"
---
# <a name="troubleshooter-find-errors-with-sql-server-transactional-replication"></a>Problembehandlung: Suchen von Fehlern bei SQL Server-Transaktionsreplikationen 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Die Problembehandlung von Replikationsfehlern kann ohne grundlegende Kenntnisse der Transaktionsreplikation frustrierend sein. Der erste Schritt beim Erstellen einer Veröffentlichung besteht darin, den Momentaufnahmen-Agent eine Momentaufnahme erstellen zu lassen und diese im Ordner für Momentaufnahmen zu speichern. Anschließend wendet der Verteilungs-Agent die Momentaufnahme auf den Abonnenten an. 

Bei diesem Vorgang wird die Veröffentlichung erstellt und in den Zustand *Wird synchronisiert* versetzt. Die Synchronisierung erfolgt in drei Phasen:
1. Transaktionen treten bei Objekten auf, die repliziert werden. Diese werden im Transaktionsprotokoll als „Für Replikation“ gekennzeichnet. 
2. Der Protokolllese-Agent durchsucht das Transaktionsprotokoll nach Transaktionen, die als „Für Replikation“ gekennzeichnet sind. Anschließend werden diese Transaktionen in der Verteilungsdatenbank gespeichert. 
3. Der Verteilungs-Agent durchsucht die Verteilungsdatenbank mithilfe des Leserthreads. Anschließend stellt dieser Agent mithilfe des Schreibthreads eine Verbindung zum Abonnenten her, um die Änderung auf diesen anzuwenden.

In jedem Schritt dieses Vorgangs können Fehler auftreten. Diese zu finden kann der schwierigste Aspekt bei der Behandlung von Synchronisierungsproblemen sein. Dieser Vorgang wird durch die Verwendung des Replikationsmonitors jedoch vereinfacht. 

>[!NOTE]
> - In diesem Leitfaden zur Problembehandlung soll die Methodik der Problembehandlung vermittelt werden. Er wurde nicht dafür entworfen, ein bestimmtes Problem zu beheben, sondern stellt einen allgemeinen Leitfaden zum Suchen von Replikationsfehlern dar. Es sind einige spezifische Beispiele enthalten, deren Lösung jedoch abhängig von der Umgebung variieren kann. 
> - Die Fehler, die in diesem Leitfaden als Beispiel dienen, basieren auf dem Tutorial [Konfigurieren der Transaktionsreplikation](../../relational-databases/replication/tutorial-replicating-data-between-continuously-connected-servers.md).



## <a name="troubleshooting-methodology"></a>Problembehandlungsmethoden 

### <a name="questions-to-ask"></a>Wichtige Fragen
1. Wo im Synchronisierungsprozess schlägt die Replikation fehl?
2. Bei welchem Agent ist ein Fehler aufgetreten?
1. Wann wurde eine Replikation zuletzt erfolgreich ausgeführt? Hat sich seitdem etwas verändert?  

### <a name="steps-to-take"></a>Erforderliche Schritte
1. Verwenden Sie den Replikationsmonitor, um zu ermitteln, an welchem Punkt (d.h. bei welchem Agent) ein Fehler bei der Replikation auftritt:
   - Wenn Fehler im Abschnitt **Verleger zu Verteiler** auftreten, tritt der Fehler im Protokolllese-Agent auf: 
   - Wenn Fehler im Abschnitt **Verteiler zu Abonnent** auftreten, tritt der Fehler im Verteilungs-Agent auf:  
2. Durchsuchen Sie den Auftragsverlauf dieses Agents im Auftragsaktivitätsmonitor, um die Details des Fehlers zu ermitteln. Wenn der Auftragsverlauf nicht genügend Details anzeigt, können Sie für diesen Agent die [ausführliche Protokollierung](#enable-verbose-logging-on-any-agent) aktivieren.
3. Versuchen Sie, eine Lösung für den Fehler zu ermitteln.


## <a name="find-errors-with-the-snapshot-agent"></a>Suchen von Fehlern des Momentaufnahmen-Agents
Der Momentaufnahmen-Agent generiert die Momentaufnahme und schreibt diese in den angegebenen Ordner für Momentaufnahmen. 

1. Zeigen Sie den Status des Momentaufnahmen-Agents an:

    a. Erweitern Sie im Objekt-Explorer den Knoten **Lokale Veröffentlichung** unter **Replikation**.

    b. Klicken Sie mit der rechten Maustaste auf Ihre Veröffentlichung **AdvWorksProductTransView**, und wählen Sie **Status des Momentaufnahmen-Agents anzeigen** aus. 

    ![Befehl „Status des Momentaufnahmen-Agents anzeigen“ im Kontextmenü](media/troubleshooting-tran-repl-errors/view-snapshot-agent-status.png)

1. Wenn im Status des Momentaufnahmen-Agents ein Fehler gemeldet wird, finden Sie im Auftragsverlauf des Momentaufnahmen-Agents weitere Einzelheiten:

    a. Erweitern Sie **SQL Server-Agent** im Objekt-Explorer, und öffnen Sie den Auftragsaktivitätsmonitor. 

    b. Legen Sie eine Sortierung nach **Kategorie** fest, und identifizieren Sie den Momentaufnahmen-Agent nach der Kategorie **REPL-Snapshot**.

    c. Klicken Sie mit der rechten Maustaste auf den Momentaufnahmen-Agent, und wählen Sie **Verlauf anzeigen** aus. 

   ![Auswahl zum Öffnen des Verlaufs des Momentaufnahmen-Agents](media/troubleshooting-tran-repl-errors/snapshot-agent-history.png)
    
1. Wählen Sie im Verlauf des Momentaufnahmen-Agents den relevanten Protokolleintrag aus. Dieser befindet sich üblicherweise ein oder zwei Zeilen *vor* dem Eintrag, der den Fehler meldet. (Ein rotes X zeigt Fehler an.) Überprüfen Sie die Meldung in dem Feld unter den Protokollen: 

    ![Fehler „Zugriff verweigert“ im Momentaufnahmen-Agent](media/troubleshooting-tran-repl-errors/snapshot-access-denied.png)

    ```console
    The replication agent had encountered an exception.
    Exception Message: Access to path '\\node1\repldata.....' is denied.
    ```

Wenn Ihre Windows-Berechtigungen für Ihren Momentaufnahmeordner nicht ordnungsgemäß konfiguriert wurden, wird Ihnen beim Momentaufnahmen-Agent der Fehler „Zugriff verweigert“ angezeigt. Sie müssen die Berechtigungen für den Ordner überprüfen, in dem Ihre Momentaufnahme gespeichert ist, und sicherstellen, dass das Konto, das verwendet wird, um den Momentaufnahmen-Agent auszuführen, für den Zugriff auf die Freigabe berechtigt ist.  

## <a name="find-errors-with-the-log-reader-agent"></a>Suchen von Fehlern des Protokolllese-Agents
Der Protokolllese-Agent stellt eine Verbindung mit Ihrer Verlegerdatenbank her und überprüft das Transaktionsprotokoll auf Transaktionen, die mit „Für Replikation“ gekennzeichnet sind. Anschließend fügt er diese Transaktionen zur Verteilungsdatenbank hinzu. 

1.  Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger her. Erweitern Sie den Serverknoten, klicken Sie mit der rechten Maustaste auf den Ordner **Replikation**, und klicken Sie anschließend auf **Replikationsmonitor starten**:  

    ![Menüelement „Replikationsmonitor starten“ im Kontextmenü](media/troubleshooting-tran-repl-errors/launch-repl-monitor.png)
  
    Der Replikationsmonitor wird geöffnet: ![Replikationsmonitor](media/troubleshooting-tran-repl-errors/repl-monitor.png) 
   
2. Das rote X gibt an, dass die Veröffentlichung nicht synchronisiert wurde. Erweitern Sie auf der linken Seite **Meine Verleger** und anschließend die relevanten Verlegerserver.  
  
3.  Wählen Sie auf der linken Seite die Veröffentlichung **AdvWorksProductTrans** aus, und suchen Sie anschließend auf einer der Registerkarten nach dem roten X, um festzustellen, wo das Problem besteht. In diesem Fall befindet sich das rote X auf der Registerkarte **Agents**. Dies bedeutet, dass bei einem der Agents ein Fehler aufgetreten ist: 

    ![Rotes X auf der Registerkarte „Agents“](media/troubleshooting-tran-repl-errors/agent-error.png)

4. Wählen Sie die Registerkarte **Agents** aus, um zu ermitteln, bei welchem Agent der Fehler aufgetreten ist: 

    ![Rotes X beim fehlgeschlagenen Protokolllese-Agent](media/troubleshooting-tran-repl-errors/log-reader-agent-failure.png)


5. In dieser Ansicht werden Ihnen zwei Agents angezeigt: der Momentaufnahmen-Agent und der Protokolllese-Agent. Der Agent, bei dem ein Fehler aufgetreten ist, ist mit einem roten X gekennzeichnet. In diesem Fall ist der Protokolllese-Agent betroffen. 

    Doppelklicken Sie auf die Zeile, in der der Fehler gemeldet wird, um den Verlauf des Protokolllese-Agent zu öffnen. Dieser Verlauf enthält weitere Informationen zu dem Fehler: 
    
    ![Fehlerdetails für den Protokolllese-Agent](media/troubleshooting-tran-repl-errors/log-reader-error.png)

    ```console
    Status: 0, code: 20011, text: 'The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.'.
    The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.
    Status: 0, code: 15517, text: 'Cannot execute as the database principal because the principal "dbo" does not exist, this type of principal cannot be impersonated, or you do not have permission.'.
    Status: 0, code: 22037, text: 'The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.'.        
    ```

6. Dieser Fehler tritt üblicherweise auf, wenn der Besitzer der Verlegerdatenbank nicht ordnungsgemäß festgelegt ist. Dies kann beim Wiederherstellen einer Datenbank geschehen. So überprüfen Sie, ob dies der Fall ist:

    a. Erweitern Sie **Datenbanken** im Objekt-Explorer.

    b. Klicken Sie mit der rechten Maustaste auf **AdventureWorks2012**, und wählen Sie **Eigenschaften** aus. 

    c. Überprüfen Sie, ob auf der Seite **Dateien** ein Besitzer vorhanden ist. Wenn dieses Feld leer ist, ist dies der wahrscheinliche Grund für Ihr Problem. 

   ![Seite „Dateien“ in den Datenbankeigenschaften mit leerem Feld „Besitzer“](media/troubleshooting-tran-repl-errors/db-properties.png)

7. Wenn das Feld „Besitzer“ auf der Seite **Dateien** leer ist, öffnen Sie im Bereich der Datenbank „AdventureWorks2012“ das Fenster **Neue Abfrage**. Führen Sie folgenden T-SQL-Code aus:

    ```sql
    -- set the owner of the database to 'sa' or a specific user account, without the brackets. 
    EXECUTE sp_changedbowner '<useraccount>'
    -- example for sa: exec sp_changedbowner 'sa'
    -- example for user account: exec sp_changedbowner 'sqlrepro\administrator' 
    ```

8. Möglicherweise müssen Sie den Protokolllese-Agent neu starten:

    a. Erweitern Sie den Knoten **SQL Server-Agent** im Objekt-Explorer, und öffnen Sie den Auftragsaktivitätsmonitor.

    b. Legen Sie eine Sortierung nach **Kategorie** fest, und identifizieren Sie den Protokolllese-Agent nach der Kategorie **REPL-LogReader**. 

    c. Klicken Sie mit der rechten Maustaste auf den Auftrag des **Protokolllese-Agents**, und wählen Sie **Auftrag starten bei Schritt...** aus. 

    ![Schritte zum Neustarten des Protokolllese-Agents](media/troubleshooting-tran-repl-errors/start-job-at-step.png)

9. Überprüfen Sie, ob Ihre Veröffentlichung jetzt synchronisiert wird, indem Sie den Replikationsmonitor erneut öffnen. Wenn dieser nicht bereits geöffnet ist, können Sie ihn finden, indem Sie im Objekt-Explorer mit der rechten Maustaste auf **Replikation** klicken. 
10. Wählen Sie die Veröffentlichung **AdvWorksProductTrans** aus, und klicken Sie auf die Registerkarte **Agents**. Doppelklicken Sie auf den Protokolllese-Agent, um den Agent-Verlauf zu öffnen. Nun sollte Ihnen angezeigt werden, dass der Protokolllese-Agent ausgeführt wird und entweder Befehle repliziert oder über „Keine replizierten Transaktionen“ verfügt:

    ![Protokolllese-Agent ohne replizierte Transaktionen](media/troubleshooting-tran-repl-errors/log-reader-running.png)

## <a name="find-errors-with-the-distribution-agent"></a>Suchen von Fehlern des Verteilungs-Agent
Der Verteilungs-Agent sucht Daten in der Verteilungsdatenbank und wendet diese anschließend auf den Abonnenten an. 

1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger her. Erweitern Sie den Serverknoten, klicken Sie mit der rechten Maustaste auf den Ordner **Replikation**, und klicken Sie anschließend auf **Replikationsmonitor starten**.  
2. Wählen Sie im **Replikationsmonitor** die Veröffentlichung **AdvWorksProductTrans** und anschließend die Registerkarte **Alle Abonnements** aus. Klicken Sie mit der rechten Maustaste auf das Abonnement und anschließend auf **Details anzeigen**:

    ![Befehl „Details anzeigen“ im Kontextmenü](media/troubleshooting-tran-repl-errors/view-details.png)

2. Das Dialogfeld **Distributor to Subscriber History** (Verlauf von Verteiler zu Abonnent) wird geöffnet und zeigt an, welches Problem bei dem Agent aufgetreten ist: 

     ![Fehlerdetails des Verteilungs-Agents](media/troubleshooting-tran-repl-errors/dist-history-error.png)

    ```console
    Error messages:
    Agent 'NODE1\SQL2016-AdventureWorks2012-AdvWorksProductTrans-NODE2\SQL2016-7' is retrying after an error. 89 retries attempted. See agent job history in the Jobs folder for more details.
    ```

3. Die Fehlermeldung gibt an, dass der Verteilungs-Agent den Vorgang wiederholt. Überprüfen Sie den Auftragsverlauf des Verteilungs-Agents, um weitere Informationen zu erhalten: 

    a. Erweitern Sie **SQL Server-Agent** im Objekt-Explorer, und öffnen Sie den **Auftragsaktivitätsmonitor**. 
    
    b. Sortieren Sie die Aufträge nach **Kategorie**. 

    c. Identifizieren Sie den Verteilungs-Agent nach der Kategorie **REPL-Verteilung**. Klicken Sie mit der rechten Maustaste auf den Agent und anschließend auf **Verlauf anzeigen**.

    ![Schritte zum Anzeigen des Verlaufs des Verteilungs-Agents](media/troubleshooting-tran-repl-errors/view-dist-agent-history.png)

5. Wählen Sie einen der Fehlereinträge aus, und zeigen Sie die Fehlermeldung im unteren Bereich des Fensters an:  

    ![Fehlertext, der angibt, dass ein falsches Kennwort für den Verteilungs-Agent vorliegt](media/troubleshooting-tran-repl-errors/dist-pw-wrong.png)

    ```console
    Message:
    Unable to start execution of step 2 (reason: Error authenticating proxy NODE1\repl_distribution, system error: The user name or password is incorrect.)
    ```

6. Dieser Fehler gibt an, dass das vom Verteilungs-Agent verwendete Kennwort falsch ist. So beheben Sie dieses Problem:

    a. Erweitern Sie im Objekt-Explorer den Knoten **Replikation**.
    
    b. Klicken Sie mit der rechten Maustaste auf das Abonnement, und wählen Sie **Eigenschaften** aus.
    
    c. Klicken Sie auf die Auslassungspunkte (...) neben dem **Agent-Prozesskonto**, und ändern Sie das Kennwort.

    ![Schritte zum Ändern des Kennworts für den Verteilungs-Agent](media/troubleshooting-tran-repl-errors/dist-agent-pw-change.png)

7. Überprüfen Sie den Replikationsmonitor erneut, indem Sie mit der rechten Maustaste auf **Replikation** im Objekt-Explorer klicken. Ein rotes X unter **Alle Abonnements** gibt an, dass bei dem Verteilungs-Agent weiterhin ein Fehler auftritt. 

    Öffnen Sie den **Distributor to Subscriber History** (Verlauf von Verteiler zu Abonnent), indem Sie mit der rechten Maustaste auf das Abonnement unter **Replikationsmonitor** > **Details anzeigen** klicken. Hier lautet der Fehler nun anders: 

    ![Fehler, der angibt, dass der Verteilungs-Agent keine Verbindung herstellen kann](media/troubleshooting-tran-repl-errors/dist-agent-cant-connect.png)

    ```console
    Connecting to Subscriber 'NODE2\SQL2016'        
    Agent message code 20084. The process could not connect to Subscriber 'NODE2\SQL2016'.
    Number:  18456
    Message: Login failed for user 'NODE2\repl_distribution'.
    ```

8. Dieser Fehler gibt an, dass der Verteilungs-Agent keine Verbindung mit dem Abonnenten herstellen konnte, da die Anmeldung des Benutzers **NODE2\repl_distribution** fehlgeschlagen ist. Sie können dies näher untersuchen, indem Sie eine Verbindung mit dem Abonnenten herstellen und das *aktuelle* SQL-Fehlerprotokoll unter dem Knoten **Verwaltung** im Objekt-Explorer öffnen: 

    ![Fehler, der angibt, dass die Anmeldung des Abonnenten fehlgeschlagen ist](media/troubleshooting-tran-repl-errors/login-failed.png)
    
    Wenn dieser Fehler angezeigt wird, fehlt die Anmeldung des Abonnenten. Weitere Informationen zum Beheben dieses Problems finden Sie unter [Permissions for replication (Berechtigungen für die Replikation)](../../relational-databases/replication/security/security-role-requirements-for-replication.md).

9. Überprüfen Sie den Replikationsmonitor erneut, sobald der Anmeldefehler behoben wurde. Wenn alle Probleme behoben wurden, sollten Ihnen neben dem **Veröffentlichungsnamen** ein grüner Pfeil und unter **Alle Abonnements** der Status **Wird ausgeführt** angezeigt werden. 

    Klicken Sie mit der rechten Maustaste auf Abonnement, um **Distributor to Subscriber History** (Verlauf von Verteiler zu Abonnent) zu öffnen und zu überprüfen, ob die Ausführung erfolgreich war. Wenn der Verteilungs-Agent zum ersten Mal ausgeführt wird, können Sie sehen, dass die Momentaufnahme in den Abonnenten massenkopiert wurde: 

     ![Verteilungs-Agent mit Status „Wird ausgeführt“ und einer Meldung zum Massenkopiervorgang](media/troubleshooting-tran-repl-errors/dist-agent-success.png)   


## <a name="enable-verbose-logging-on-any-agent"></a>Aktivieren der ausführlichen Protokollierung auf einem Agent
Sie können die ausführliche Protokollierung verwenden, um detailliertere Informationen zu Fehlern zu erhalten, die bei einem beliebigen Agent in der Replikationstopologie auftreten. Die Schritte sind für jeden Agent identisch. Achten Sie jedoch darauf, dass Sie den richtigen Agent im Auftragsaktivitätsmonitor auswählen. 

   >[!NOTE]   
   > Die Agents können sich entweder auf dem Verleger oder auf dem Abonnenten befinden, je nachdem, ob es sich um ein Pull- oder um ein Pushabonnement handelt. Wenn Sie den gewünschten Agent auf dem aktuellen Server nicht finden können, suchen Sie auf einem anderen Server.  

1. Entscheiden Sie, wo die ausführliche Protokollierung gespeichert werden soll, und stellen Sie sicher, dass dieser Ordner vorhanden ist. In diesem Beispiel wird C:\Temp verwendet. 
2. Erweitern Sie den Knoten **SQL Server-Agent** im Objekt-Explorer, und öffnen Sie den Auftragsaktivitätsmonitor. 

    ![Befehl „Auftragsaktivitäten anzeigen“ im Kontextmenü des Auftragsaktivitätsmonitors](media/troubleshooting-tran-repl-errors/job-activity-monitor.png)    

1. Sortieren Sie nach **Kategorie**, und ermitteln Sie den entsprechenden Agent. In diesem Beispiel wird der Protokolllese-Agent verwendet. Klicken Sie mit der rechten Maustaste auf den entsprechenden Agent, und wählen Sie **Eigenschaften** aus.

    ![Schritte zum Öffnen der Agent-Eigenschaften](media/troubleshooting-tran-repl-errors/log-agent-properties.png)

1. Wählen Sie die Seite **Schritte** aus, und markieren Sie den Schritt **Agent ausführen**. Wählen Sie **Bearbeiten** aus. 

    ![Schritte zum Bearbeiten des Schritts „Agent ausführen“](media/troubleshooting-tran-repl-errors/edit-steps.png)

1. Beginnen Sie im Feld **Befehl** eine neue Zeile, geben Sie folgenden Text ein, und klicken Sie auf **OK**: 

    ```console
    -Output C:\Temp\OUTPUTFILE.txt -Outputverboselevel 3
    ```

    Sie können den Speicherort und den Ausführlichkeitsgrad beliebig ändern.

    ![Ausführliche Ausgabe in den Eigenschaften des Auftragsschritts](media/troubleshooting-tran-repl-errors/verbose.png)

   > [!NOTE]
   > Durch Folgendes kann der Agent fehlschlagen oder die Ausgabedatei nicht vorhanden sein, wenn Sie die ausführlichen Ausgabeparameter hinzufügen:
   > - Es besteht ein Formatierungsproblem, bei dem ein Halbgeviertstrich zu einem Bindestrich wurde. 
   > - Der Speicherort ist nicht auf dem Datenträger vorhanden, oder das Konto, das den Agent ausführt, verfügt nicht über die Berechtigungen, um am angegebenen Speicherort Schreibvorgänge durchzuführen. 
   > - Zwischen dem letzten Parameter und dem Parameter `-Output` fehlt ein Leerzeichen. 
   > - Verschiedene Agents unterstützen unterschiedliche Ausführlichkeitsgrade. Wenn Sie die ausführliche Protokollierung aktivieren und der Agent darauf hin nicht startet, verringern Sie den angegebenen Ausführlichkeitsgrad um 1. 

1. Starten Sie den Protokolllese-Agent neu, indem Sie mit der rechten Maustaste darauf klicken und **Stop Job at Step** (Auftrag beenden bei Schritt) auswählen. Aktualisieren Sie die Ansicht, indem Sie auf der Symbolleiste auf das **Aktualisierungssymbol** klicken. Klicken Sie mit der rechten Maustaste auf den Agent, und wählen Sie **Auftrag starten bei Schritt...** aus.
2. Überprüfen Sie die Ausgabe auf dem Datenträger. 

    ![Textdatei der Ausgabe](media/troubleshooting-tran-repl-errors/output.png)

    
1. Führen Sie zum Deaktivieren der ausführlichen Protokollierung die vorherigen Schritte erneut durch, um die gesamte Zeile `-Output` zu entfernen, die Sie zuvor hinzugefügt haben. 

Weitere Informationen finden Sie unter [Enabling verbose logging for replication agents (Aktivieren der ausführlichen Protokollierung für Replikations-Agents)](https://support.microsoft.com/help/312292/how-to-enable-replication-agents-for-logging-to-output-files-in-sql-se). 


## <a name="see-also"></a>Weitere Informationen
<br>[Transaktionsreplikation (Übersicht)](../../relational-databases/replication/transactional/transactional-replication.md)
<br>[Tutorials zur Replikation](../../relational-databases/replication/replication-tutorials.md)
<br>[REPLTalk-Blog](https://blogs.msdn.microsoft.com/repltalk)

[!INCLUDE[get-help-options](../../includes/paragraph-content/get-help-options.md)]

