---
title: Problembehandlung bei Failoverclustern | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die Problembehandlung für Failovercluster, einschließlich der Wiederherstellung nach einem Fehler, der Behebung häufig auftretender Probleme und der Verwendung von erweiterten gespeicherten Prozeduren/COM-Objekten.
ms.custom: ''
ms.date: 10/21/2015
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- troublshooting, failover clustering
- failover clustering, troubleshooting
- cluster troubleshooting
ms.assetid: 84012320-5a7b-45b0-8feb-325bf0e21324
author: cawrites
ms.author: chadam
ms.openlocfilehash: 34fe360248235da3c9a647799a178d9c734ad027
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350042"
---
# <a name="failover-cluster-troubleshooting"></a>Problembehandlung bei Failoverclustern
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema finden Sie Informationen zu den folgenden Punkten:  
  
-   Grundlegende Schritte bei der Problembehandlung.  
  
-   Wiederherstellung bei einem fehlerhaften Failovercluster.  
  
-   Lösungen für die häufigsten Probleme beim Failoverclustering.  
  
-   Verwenden von erweiterten gespeicherten Prozeduren und COM-Objekten.  
  
## <a name="basic-troubleshooting-steps"></a>Grundlegende Schritte bei der Problembehandlung  
 Als erster Schritt für die Diagnose sollte eine neue Überprüfung der Clustervalidierung ausgeführt werden. Details zur Validierung finden Sie unter [Failover Cluster Step-by-Step Guide: Validating Hardware for a Failover Cluster](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732035(v=ws.10)) (Schrittweise Anleitung für Failovercluster: Prüfen der Hardware auf einen Failovercluster).  Diese kann ohne Unterbrechung des Diensts abgeschlossen werden, da sie keine Auswirkungen auf irgendwelche Onlineclusterressourcen hat. Die Validierung kann jederzeit ausgeführt werden, sobald die Failoverclustering-Funktion installiert wurde, auch vor der Clusterbereitstellung, während der Clustererstellung oder -ausführung. Tatsächlich werden, sobald der Cluster verwendet wird, zusätzliche Tests ausgeführt, die prüfen, ob die bewährten Methoden für hoch verfügbare Workloads befolgt wurden. Von diesen Dutzenden Tests wirken sich nur wenige auf ausgeführte Clusterworkloads aus. Diese Tests gehören alle zur Speicherkategorie, das Überspringen dieser gesamten Kategorie stellt somit eine einfache Methode dar, störende Tests zu vermeiden.  
Failoverclustering wird mit einer integrierten Schutzvorrichtung geliefert, um beim Ausführen der Speichertests während der Validierung unbeabsichtigte Downtime zu verhindern. Falls der Cluster bei der Initiierung der Validierung über Onlinegruppen verfügt und die Speichertests ausgewählt bleiben, wird der Benutzer dazu aufgefordert, zu entscheiden, ob alle Tests durchgeführt werden sollen (und Downtime verursacht wird) oder ob die Tests der Datenträger aller Onlinegruppen zur Vermeidung von Downtime übersprungen werden sollen. Falls die gesamte Speicherkategorie von den Tests ausgeschlossen wird, wird diese Aufforderung nicht angezeigt. Dadurch wird die Clustervalidierung ohne Downtime aktiviert.  
  
#### <a name="how-to-revalidate-your-cluster"></a>So validieren Sie Ihren Cluster erneut  
  
1.  Stellen Sie im Failovercluster-Snap-In in der Konsolenstruktur sicher, dass **Failover-Clusterverwaltung** ausgewählt ist, und klicken Sie dann unter **Verwaltung** auf **Konfiguration überprüfen**.  
  
2.  Befolgen Sie die Anleitung im Assistenten, um die Server und Tests anzugeben und die Tests auszuführen. Die Seite **Zusammenfassung** wird angezeigt, nachdem die Tests ausgeführt wurden.  
  
3.  Klicken Sie auf der Seite **Zusammenfassung** auf **Bericht anzeigen** , um die Testergebnisse anzuzeigen.  
  
     Zum Anzeigen der Testergebnisse, nachdem Sie den Assistenten geschlossen haben, wechseln Sie zu **%SystemRoot%\Cluster\Reports\Validation Report date and time.html** , wobei **%SystemRoot%** der Ordner ist, in dem das Betriebssystem installiert ist (zum Beispiel **C:\Windows**).  
  
4.  Zum Anzeigen von Hilfethemen, mit deren Hilfe Sie die Ergebnisse interpretieren können, klicken Sie auf **Weitere Informationen über Clustervalidierungstests**.  
  
 Zum Anzeigen von Hilfethemen über die Clustervalidierung nach Schließen des Assistenten klicken Sie im Failovercluster-Snap-In nacheinander auf **Hilfe**, **Hilfethemen** und die Registerkarte **Inhalt** . Erweitern Sie die Inhalte für die Failoverclusterhilfe, und klicken Sie auf **Konfiguration des Failoverclusters** überprüfen.  Nachdem der Validierungs-Assistent abgeschlossen ist, zeigt der **Zusammenfassungsbericht** die Ergebnisse an. Alle Tests müssen entweder mit einem grünen Häkchen oder in einigen Fällen mit einem gelben Dreieck (Warnung) bestanden werden. Wenn Sie nach Problembereichen suchen (rote X oder gelbe Fragezeichen), klicken Sie in dem Teil des Berichts, der die Testergebnisse zusammenfasst, zum Anzeigen der Details auf einen einzelnen Test. Alle Probleme mit rotem X müssen gelöst werden, bevor [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Probleme behandelt werden.  
  
 **Installieren von Updates**  
  
 Das Installieren von Updates stellt einen wichtigen Teil der Vermeidung von Problemen mit Ihrem System dar. Hilfreiche Links:  
  
-   [Empfohlene Hotfixes und Updates für Windows Server 2012 R2-basierte Failovercluster](https://support.microsoft.com/kb/2920151)  
  
-   [Empfohlene Hotfixes und Updates für Windows Server 2012-basierte Failovercluster](https://support.microsoft.com/kb/2784261)  
  
-   [Empfohlene Hotfixes und Updates für Windows Server 2008 R2-basierte Failovercluster](https://support.microsoft.com/kb/980054)  
  
-   [Empfohlene Hotfixes und Updates für Windows Server 2008-basierte Failovercluster](https://support.microsoft.com/kb/957311)  
  
## <a name="recovering-from-failover-cluster-failure"></a>Wiederherstellen bei einem Failovercluster-Fehler  
 Failovercluster-Fehler sind üblicherweise auf eine von zwei Ursachen zurückzuführen:  
  
-   Hardwarefehler in einem Knoten eines Zwei-Knoten-Clusters. Die Ursache für diesen Hardwarefehler könnte beispielsweise ein Fehler auf der SCSI-Karte oder im Betriebssystem sein.  
  
     Zum Beheben dieses Fehlers entfernen Sie den fehlerhaften Knoten mithilfe des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setupprogramms vom Failovercluster, behandeln Sie den Hardwarefehler, während der Computer offline ist, fahren Sie den Computer wieder hoch, und fügen Sie dann der Failoverclusterinstanz den reparierten Knoten wieder hinzu.  
  
     Weitere Informationen finden Sie unter [Erstellen eines neuen SQL Server-Failoverclusters &#40;Setup&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md) und [Wiederherstellen nach einem Fehler der Failoverclusterinstanz](../../../sql-server/failover-clusters/windows/recover-from-failover-cluster-instance-failure.md).  
  
-   Betriebssystemfehler. In diesem Fall ist der Knoten offline, aber nicht unwiederbringlich beschädigt.  
  
     Um eine Wiederherstellung nach einem Betriebssystemfehler durchzuführen, stellen Sie den Knoten wieder her, und testen Sie das Failover. Wenn für die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz kein ordnungsgemäßes Failover ausgeführt wird, müssen Sie das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setupprogramm zum Entfernen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vom Failovercluster verwenden, notwendige Reparaturen vornehmen, den Computer wieder hochfahren und dann der Failoverclusterinstanz den reparierten Knoten wieder hinzufügen.  
  
     Das Wiederherstellen nach einem Betriebssystemfehler auf diese Art kann Zeit kosten. Falls die Wiederherstellung nach dem Betriebssystemfehler problemlos möglich ist, sollten Sie dieses Verfahren vermeiden.  
  
     Weitere Informationen finden Sie unter [Erstellen eines neuen SQL Server-Failoverclusters &#40;Setup&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md) und [Vorgehensweise: Wiederherstellen nach einem Failoverclusterfehler in Szenario 2](recover-from-failover-cluster-instance-failure.md).  
  
## <a name="resolving-common-problems"></a>Lösen häufig auftretender Probleme  
 In der folgenden Liste werden häufig auftretende Probleme bei der Verwendung sowie deren Behebung beschrieben.  
  
### <a name="problem-incorrect-use-of-command-prompt-syntax-to-install-sql-server"></a>Problem: Falsche Verwendung der Eingabeaufforderungssyntax für die Installation von SQL Server  
 **Problem 1:** Setupprobleme sind schwer zu diagnostizieren, wenn der **/qn** -Schalter an der Eingabeaufforderung verwendet wird, da **/qn** alle Dialogfelder und Fehlermeldungen für das Setup unterdrückt. Wenn der **/qn** -Schalter angegeben ist, werden alle Setupmeldungen, einschließlich aller Fehlermeldungen, in Setupprotokolldateien geschrieben. Weitere Informationen finden Sie unter [Lesen und Anzeigen der Setupprotokolldateien von SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
 **Lösung 1:** Verwenden Sie anstelle des **/qn**-Schalters den **/qb**-Schalter. Wenn Sie den **/qb** -Schalter verwenden, wird bei jedem Schritt die Standardbenutzeroberfläche mit allen Fehlermeldungen angezeigt.  
  
### <a name="problem-sql-server-cannot-log-on-to-the-network-after-it-migrates-to-another-node"></a>Problem: SQL Server kann sich nach der Migration auf einen anderen Knoten nicht am Netzwerk anmelden  
 **Problem 1:** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Dienstkonten können keinen Kontakt mit einem Domänencontroller herstellen.  
  
 **Lösung 1:** Überprüfen Sie die Ereignisprotokolle in Hinblick auf Netzwerkprobleme wie Adapterfehler oder DNS-Probleme. Überprüfen Sie, ob Sie Ihren Domänencontroller pingen können.  
  
 **Problem 2:** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Dienstkontokennwörter stimmen nicht auf allen Clusterknoten überein, oder vom Knoten wird ein [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Dienst, der von einem fehlerhaften Knoten migriert wurde, nicht neu gestartet.  
  
 **Lösung 2:** Ändern Sie die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Dienstkontokennwörter mithilfe von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Configuration Manager. Andernfalls müssen Sie nach dem Ändern der Kennwörter für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienstkonten auf einem Knoten auch die Kennwörter auf allen anderen Knoten ändern. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Der Konfigurations-Manager führt dies automatisch aus.  
  
### <a name="problem-sql-server-cannot-access-the-cluster-disks"></a>Problem: SQL Server kann nicht auf die Clusterdatenträger zugreifen  
 **Problem 1:** Die Firmware oder Treiber sind nicht auf allen Knoten aktualisiert.  
  
 **Lösung 1:** Überprüfen Sie, ob alle Knoten die richtigen Firmwareversionen und die gleichen Treiberversionen verwenden.  
  
 **Problem 2:** Ein Knoten kann Clusterdatenträger nicht wiederherstellen, die von einem fehlerhaften Knoten auf einem freigegebenen Clusterdatenträger mit einem unterschiedlichen Laufwerkbuchstaben migriert wurden.  
  
 **Lösung 2:** Die Laufwerkbuchstaben für die Clusterdatenträger müssen auf beiden Servern identisch sein. Überprüfen Sie andernfalls die ursprüngliche Installation des Betriebssystems und von [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Clusterdienste (MSCS, Microsoft Cluster Service).  
  
### <a name="problem-failure-of-a-sql-server-service-causes-failover"></a>Problem: Ein Fehler eines SQL Server-Dienstes verursacht ein Failover  
 **Lösung:** Um zu verhindern, dass der Fehler bestimmter Dienste zu einem Failover der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Gruppe führt, konfigurieren Sie diese Dienste mithilfe der Clusterverwaltung unter Windows wie folgt:  
  
-   Deaktivieren Sie im Dialogfeld **Volltexteigenschaften** auf der Registerkarte **Erweitert** das Kontrollkästchen **Die Gruppe beeinflussen** . Wenn [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] allerdings ein Failover verursacht, wird die Volltextsuche erneut gestartet.  
  
### <a name="problem-sql-server-does-not-start-automatically"></a>Problem: SQL Server startet nicht automatisch  
 **Lösung:** Verwenden Sie die Clusterverwaltung unter MSCS, um automatisch einen Failovercluster zu starten. Der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienst sollte auf manuelles Starten festgelegt werden, und die Clusterverwaltung sollte in MSCS zum Starten des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Diensts konfiguriert werden. Weitere Informationen finden Sie unter [Verwalten von Diensten](https://msdn.microsoft.com/library/ms178096\(v=sql.105\).aspx).  
  
### <a name="problem-the-network-name-is-offline-and-you-cannot-connect-to-sql-server-using-tcpip"></a>Problem: Die Netzwerknamenressource ist offline, und Sie können keine Verbindung zu SQL Server mithilfe von TCP/IP herstellen  
 **Problem 1:** DNS erzeugt einen Fehler aufgrund einer Clusterressource, die DNS erfordert.  
  
 **Lösung 1:** Beheben Sie die DNS-Probleme.  
  
 **Problem 2:** Im Netzwerk ist ein Name doppelt vorhanden.  
  
 **Lösung 2:** Verwenden Sie „NBTSTAT“, um den doppelten Namen zu finden, und beheben Sie dann das Problem.  
  
 **Problem 3:** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] stellt keine Verbindung mithilfe von Named Pipes her.  
  
 **Lösung 3:** Erstellen Sie mit dem SQL Server-Konfigurations-Manager einen Alias, der eine Verbindung mit dem entsprechenden Computer herstellt, um eine Verbindung mithilfe von Named Pipes herzustellen. Wenn Sie beispielsweise über einen Cluster mit zwei Knoten (**Knoten A** und **Knoten B**) und eine Failoverclusterinstanz (**Virtsql**) mit einer Standardinstanz verfügen, können Sie die Verbindung mit dem Server, dessen Netzwerknamenressource offline ist, wie folgt herstellen:  
  
1.  Ermitteln Sie mithilfe der Clusterverwaltung den Knoten, auf dem die Gruppe mit der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ausgeführt wird. In diesem Beispiel ist es **Knoten A**.  
  
2.  Starten Sie den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienst auf diesem Computer mithilfe von **net start**. Weitere Informationen zum Verwenden von **net start** finden Sie unter [Manuelles Starten von SQL Server](https://msdn.microsoft.com/library/ms191193\(v=sql.105\).aspx).  
  
3.  Starten Sie den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]SQL Server-Konfigurations-Manager auf **Knoten A**. Zeigen Sie den Pipenamen an, der vom Server überwacht wird. Dieser sollte in etwa so aussehen: \\\\.\\$$\VIRTSQL\pipe\sql\query.  
  
4.  Starten Sie den SQL Server-Konfigurations-Manager auf dem Clientcomputer.  
  
5.  Erstellen Sie den Alias SQLTEST1, um über Named Pipes eine Verbindung mit diesem Pipenamen herzustellen. Geben Sie dazu **Knoten A** als Servernamen ein, und ändern Sie den Pipenamen in \\\\.\pipe\\$$\VIRTSQL\sql\query.  
  
6.  Stellen Sie mithilfe des Alias SQLTEST1 als Servernamen eine Verbindung mit dieser Instanz her.  
  
### <a name="problem-sql-server-setup-fails-on-a-cluster-with-error-11001"></a>Problem: SQL Server-Setup führt auf einem Cluster zum Fehler 11001  
 **Problem:** Ein verwaister Registrierungsschlüssel in [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL.X\Cluster]  
  
 **Lösung:** Stellen Sie sicher, dass die „MSSQL.X“-Registrierungsstruktur zurzeit nicht verwendet wird, und löschen Sie dann den Clusterschlüssel.  
  
### <a name="problem-cluster-setup-error-the-installer-has-insufficient-privileges-to-access-this-directory-drivemicrosoft-sql-server-the-installation-cannot-continue-log-on-as-an-administrator-or-contact-your-system-administrator"></a>Problem: Fehler beim Clustersetup: „Die Rechte des Installationsprogramms reichen nicht aus, um auf dieses Verzeichnis zuzugreifen: \<drive>\Microsoft SQL Server. Die Installation kann nicht fortgesetzt werden. Melden Sie sich als Administrator an, oder wenden Sie sich an Ihren Systemadministrator"  
 **Problem:** Dieser Fehler wird durch ein freigegebenes SCSI-Laufwerk verursacht, das nicht ordnungsgemäß partitioniert ist.  
  
 **Lösung:** Erstellen Sie eine neue, einzelne Partition auf dem freigegebenen Datenträger, indem Sie die folgenden Schritte ausführen:  
  
1.  Löschen Sie die Datenträgerressource vom Cluster.  
  
2.  Löschen Sie alle Partitionen vom Datenträger.  
  
3.  Überprüfen Sie in den Datenträgereigenschaften, ob es sich um einen Basisdatenträger handelt.  
  
4.  Erstellen Sie auf dem freigegebenen Datenträger eine Partition, formatieren Sie den Datenträger, und weisen Sie ihm einen Laufwerkbuchstaben zu.  
  
5.  Fügen Sie den Datenträger mithilfe der Clusterverwaltung (cluadmin) dem Cluster hinzu.  
  
6.  Führen Sie [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Setup aus.  
  
### <a name="problem-applications-fail-to-enlist-sql-server-resources-in-a-distributed-transaction"></a>Problem: SQL Server-Ressourcen werden von den Anwendungen nicht in eine verteilte Transaktion eingetragen  
 **Problem:** Weil [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC) in Windows nicht vollständig konfiguriert ist, tragen Anwendungen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Ressourcen in einer verteilten Transaktion möglicherweise nicht ein. Dieses Problem kann sich auf verknüpfte Server, verteilte Abfragen und remote gespeicherte Prozeduren auswirken, die verteilte Transaktionen verwenden. Weitere Informationen zum Konfigurieren von MS DTC finden Sie unter [Vor dem Installieren des Failoverclusterings](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md).  
  
 **Lösung:** Die MS DTC-Dienste müssen auf den Servern vollständig aktiviert werden, auf denen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installiert und MS DTC konfiguriert ist, um solchen Problemen vorzubeugen.  
  
 Führen Sie die folgenden Schritte aus, um MS DTC vollständig zu aktivieren:  
  
1.  Öffnen Sie in der Systemsteuerung die Option **Verwaltung**, und öffnen Sie dann **Computerverwaltung**.  
  
2.  Erweitern Sie im linken Bereich der Computerverwaltung den Knoten **Dienste und Anwendungen**, und klicken Sie dann auf **Dienste**.  
  
3.  Klicken Sie im rechten Bereich der Computerverwaltung mit der rechten Maustaste auf **Distributed Transaction Coordinator**, und wählen Sie **Eigenschaften** aus.  
  
4.  Klicken Sie im Fenster **Distributed Transaction Coordinator** auf die Registerkarte **Allgemein** , und klicken Sie dann auf **Beenden** , um den Dienst zu beenden.  
  
5.  Klicken Sie im Fenster **Distributed Transaction Coordinator** auf die Registerkarte **Anmelden** , und richten Sie das Anmeldekonto NT AUTHORITY\NetworkService ein.  
  
6.  Klicken Sie auf **Anwenden** und **OK** , um das Fenster **Distributed Transaction Coordinator** zu schließen. Schließen Sie das Fenster **Computerverwaltung** . Schließen Sie das Fenster **Verwaltung** .  
  
## <a name="using-extended-stored-procedures-and-com-objects"></a>Verwenden von erweiterten gespeicherten Prozeduren und COM-Objekten  
 Bei Verwendung erweiterter gespeicherter Prozeduren mit einer Failover-Clusterunterstützungskonfiguration müssen alle erweiterten gespeicherten Prozeduren auf einem von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]abhängigen Clusterdatenträger installiert werden. Dadurch wird sichergestellt, dass beim Failover eines Knotens die erweiterten gespeicherten Prozeduren weiterhin verwendet werden können.  
  
 Wenn die erweiterten gespeicherten Prozeduren COM-Komponenten verwenden, muss der Systemadministrator die COM-Komponenten auf jedem Knoten des Clusters registrieren. Die Informationen zum Laden und Ausführen von COM-Komponenten müssen in der Registrierung des aktiven Knotens vorhanden sein, damit die Komponenten erstellt werden. Andernfalls verbleiben die Informationen in der Registrierung des Computers, auf dem die COM-Komponenten zuerst registriert wurden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Lesen und Anzeigen der Setupprotokolldateien von SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)   
 [Funktionsweise erweiterter gespeicherter Prozeduren](../../../relational-databases/extended-stored-procedures-programming/how-extended-stored-procedures-work.md)   
 [Ausführungsmerkmale erweiterter gespeicherter Prozeduren](../../../relational-databases/extended-stored-procedures-programming/execution-characteristics-of-extended-stored-procedures.md)  
  
