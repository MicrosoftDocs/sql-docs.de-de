---
title: Timeouts für Leaseintegritätsprüfungen für Verfügbarkeitsgruppen
description: Hier finden Sie die Mechanismen und Richtlinien der Timeouts für Leases, Cluster und Integritätsprüfung für Always On-Verfügbarkeitsgruppen.
ms.custom: seo-lt-2019
ms.date: 05/02/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: 787a683eb0fc666eff8f3c72519bbba149dafe96
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100340808"
---
# <a name="mechanics-and-guidelines-of-lease-cluster-and-health-check-timeouts-for-always-on-availability-groups"></a>Mechanismen und Richtlinien der Timeouts für Leases, Cluster und Integritätsprüfung für Always On-Verfügbarkeitsgruppen 

Unterschiedliche Hardware-, Software- und Clusterkonfigurationen sowie unterschiedliche Anwendungsanforderungen in Bezug auf Leistung und Betriebszeit erfordern eine spezifische Konfiguration der Timeoutwerte für Lease, Cluster und Integritätsprüfung. Für bestimmte Anwendungen und Arbeitsauslastungen ist eine striktere Überwachung erforderlich, um Ausfallzeiten infolge schwerer Fehler zu begrenzen. Andere erfordern eine größere Toleranz gegenüber vorübergehenden Netzwerkproblemen und Wartezeiten aufgrund hoher Ressourcennutzung und sind für langsameres Failover geeignet. 

Es werden mehrere Dienste auf den einzelnen Knoten zum Erkennen von Fehlern eingesetzt. Der Clusterdienst kann einen Quorumsverlust erkennen, die Ressourcen-DLL kann ein Problem erkennen, das durch die Always On-Integritätserkennung zum Vorschein kommt, oder es kann ein manuelles Failover direkt auf der primären Instanz initiiert werden. Der Clusterdienst, der Ressourcenhost und die SQL Server-Instanz werden über RPC, Shared Memory und T-SQL miteinander synchronisiert. In den meisten Szenarien findet eine erfolgreiche Kommunikation dieser Dienste statt, doch ist diese Kommunikation selbst zwischen Diensten auf demselben Computer nicht hundertprozentig zuverlässig. Darüber hinaus muss die Verfügbarkeitsgruppe systemweiten Ereignissen wie Netzwerk- und Datenträgerfehlern standhalten können, durch die eine Kommunikation verhindert oder Funktionen unterbrochen werden. Bei Auftreten vieler Fehler und ohne hundertprozentig zuverlässige Kommunikation zwischen den Diensten ist die Verfügbarkeitsgruppe von verschiedenen Erkennungsmechanismen für das Failover abhängig, damit Fehler unabhängig voneinander erkannt werden und darauf reagiert wird, sodass der Clusterstatus immer für alle Knoten konsistent ist.  

## <a name="cluster-node-and-resource-detection"></a>Erkennung von Clusterknoten und -ressourcen 

Auf jedem Knoten im Cluster wird ein einzelner Clusterdienst ausgeführt, der mit dem Failovercluster zusammenarbeitet und alle Clusterressourcen überwacht. Der Ressourcenhost funktioniert als separater Prozess und stellt die Schnittstelle zwischen dem Clusterdienst und den Clusterressourcen dar. Der Ressourcenhost führt Vorgänge für Clusterressourcen aus, wenn er vom Clusterdienst dazu aufgerufen wird. Clusterfähige Anwendungen wie SQL Server bieten benutzerdefinierte Schnittstellen zum Ressourcenmonitor über Ressourcen-DLLs. Die Ressourcen-DLL implementiert Online- und Offlinevorgänge sowie eine Integritätsüberwachung für benutzerdefinierte Ressourcen. Beim Ressourcenhost handelt es sich um einen untergeordneten Prozess des Clusterdiensts, der beendet wird, sobald der Clusterdienst beendet wird. 

Bei SQL Server bestimmt die Ressourcen-DLL der Verfügbarkeitsgruppe den Zustand der Gruppe auf Grundlage des jeweiligen Leasemechanismus und der Always On-Integritätserkennung. Die Ressourcen-DLL der Verfügbarkeitsgruppe stellt die Ressourcenintegrität über den `IsAlive`-Vorgang bereit. Der Ressourcenmonitor fragt `IsAlive` im Taktintervall des Clusters ab, das durch die clusterübergreifenden Werte `CrossSubnetDelay` und `SameSubnetDelay` festgelegt ist. Auf einem primären Knoten initiiert der Clusterdienst immer dann ein Failover, wenn der `IsAlive`-Aufruf an die Ressourcen-DLL zurückgibt, dass die Verfügbarkeitsgruppe nicht fehlerfrei ist. 

Der Clusterdienst sendet Takte an andere Knoten im Cluster und bestätigt Takte, die er von diesen empfängt. Wenn ein Knoten aufgrund einer Reihe unbestätigter Takte einen Kommunikationsfehler erkennt, sendet er eine Nachricht, damit alle erreichbare Knoten ihre Sichten der Clusterknotenintegrität abstimmen. Dieses Ereignis wird *Neugruppierungsereignis* genannt und bewahrt die Konsistenz des Clusterstatus auf allen Knoten. Wenn nach einem Neugruppierungsereignis das Quorum verloren gegangen ist, werden alle Clusterressourcen, einschließlich der Verfügbarkeitsgruppen in dieser Partition, offline geschaltet. Alle Knoten in dieser Partition gehen in einen Auflösungsstatus über. Wenn eine Partition vorhanden ist, die ein Quorum enthält, wird die Verfügbarkeitsgruppe einem Knoten in der Partition zugewiesen und wird zum primären Replikat, während es sich bei allen anderen Knoten um sekundäre Replikate handelt. 

## <a name="always-on-health-detection"></a>Always On-Integritätserkennung 

Die Always On-Ressourcen-DLL überwacht den Status interner SQL Server-Komponenten. `sp_server_diagnostics` meldet die Integrität dieser SQL Server-Komponenten in einem durch `HealthCheckTimeout` gesteuerten Intervall. `sp_server_diagnostics` meldet den Integritätsstatus von fünf Komponenten auf Instanzebene: System, Ressource, Abfrageverarbeitung, E/A-Subsystem und Ereignisse. Außerdem wird die Integrität jeder Verfügbarkeitsgruppe gemeldet. Bei jeder Aktualisierung aktualisiert die Ressourcen-DLL den Integritätsstatus der Verfügbarkeitsgruppenressource basierend auf der Fehlerebene der Verfügbarkeitsgruppe. Bei der Rückgabe von Daten von `sp_server_diagnostics` ist für jede Komponente ein Status vom Typ „Fehlerfrei“, „Warnung“, „Fehler“ oder „Unbekannt“ mit einigen XML-Daten angegeben, die den Status der Komponente beschreiben. Im Falle der Integritätserkennung ergreift die Ressourcen-DLL nur dann Maßnahmen, wenn eine Komponente einen Fehlerstatus aufweist. 

Wenn die Integritätserkennung der Ressourcen-DLL über mehrere Intervalle keine Aktualisierung meldet, wird die Verfügbarkeitsgruppe als fehlerhaft erkannt, und es werden Fehler in `IsAlive`-Aufrufen gemeldet. 

## <a name="lease-mechanism"></a>Leasemechanismus  

Im Gegensatz zu anderen Failovermechanismen spielt die SQL Server-Instanz beim Leasemechanismus eine aktive Rolle. Der Leasemechanismus wird als Looks-Alive-Validierung zwischen dem Clusterressourcenhost und dem SQL Server-Prozess verwendet. Der Mechanismus dient der Sicherstellung, dass die beiden Seiten (Cluster- und SQL Server-Dienst) in häufigem Kontakt stehen, den Zustand des jeweils anderen überprüfen und schließlich ein Split-Brain-Szenario verhindern.  Wenn die Verfügbarkeitsgruppe als primäres Replikat online geschaltet wird, erzeugt die SQL Server-Instanz einen dedizierten Leaseworkerthread für die Verfügbarkeitsgruppe. Der Leaseworker teilt sich einen kleinen Bereich des Speichers mit dem Ressourcenhost, der die Ereignisse für Leaseverlängerung und Leasebeendigung enthält. Der Leaseworker und der Ressourcenhost arbeiten kreisförmig, indem sie das jeweilige Leaseverlängerungsereignis signalisieren und dann im Ruhezustand darauf warten, dass die andere Seite das eigene Leaseverlängerungsereignis oder das Beendigungsereignis signalisiert. Sowohl der Ressourcenhost als auch der SQL Server-Leasethread besitzen einen Wert für die Gültigkeitsdauer, der jedes Mal dann aktualisiert wird, wenn der Thread nach der Signalisierung durch den anderen Thread aktiviert wird. Wenn beim Warten auf das Signal die Gültigkeitsdauer erreicht wird, läuft die Lease ab und das Replikat geht dann in den Auflösungsstatus für die jeweilige Verfügbarkeitsgruppe über. Wenn das Leasebeendigungsereignis signalisiert wird, geht das Replikat in eine Auflösungsrolle über. 

![image](media/availability-group-lease-healthcheck-timeout/image1.png) 

Der Leasemechanismus erzwingt die Synchronisierung zwischen SQL Server und Windows Server-Failovercluster. Bei Ausgabe eines Failoverbefehls sendet der Clusterdienst einen Offlineaufruf an die Ressourcen-DLL des aktuellen primären Replikats. Die Ressourcen-DLL versucht zunächst, die Verfügbarkeitsgruppe anhand einer gespeicherten Prozedur offline zu schalten. Wenn bei dieser gespeicherten Prozedur ein Fehler oder ein Timeout auftritt, wird dies dem Clusterdienst zurückgemeldet, der dann einen Befehl zum Beenden ausgibt. Bei diesem Befehl wird erneut versucht, dieselbe gespeicherte Prozedur auszuführen, doch wartet der Cluster diesmal nicht, dass die Ressourcen-DLL einen Erfolg oder Fehler meldet, bevor die Verfügbarkeitsgruppe auf einem neuen Replikat online geschaltet wird. Wenn dieser zweite Prozeduraufruf fehlschlägt, muss sich der Ressourcenhost darauf verlassen, dass die Instanz vom Leasemechanismus offline geschaltet wird. Wenn die Ressourcen-DLL zum Offlineschalten der Verfügbarkeitsgruppe aufgerufen wird, signalisiert die Ressourcen-DLL das Leasebeendigungsereignis und aktiviert dadurch den SQL Server-Leaseworkerthread, um die Verfügbarkeitsgruppe offline zu schalten. Auch wenn dieses Beendigungsereignis nicht signalisiert wird, läuft die Lease ab und das Replikat geht in den Auflösungsstatus über. 

Die Lease ist in erster Linie ein Synchronisierungsmechanismus zwischen der primären Instanz und dem Cluster, sie kann aber auch dort Fehlerbedingungen erzeugen, wo sonst kein Failover erforderlich wäre. Beispielsweise können hohe CPU-Werte, Bedingungen mit nicht genügend Arbeitsspeicher (wenig virtueller Arbeitsspeicher, Prozesspaging), SQL-Prozesse, die beim Erzeugen eines Speicherabbilds nicht reagieren, ein nicht reagierendes System oder ein Cluster (WSFC), der offline geschaltet wird (z. B. wegen Quorumverlust), die Verlängerung der Leasedauer aus der SQL-Instanz verhindern und einen Neustart oder ein Failover verursachen. 

## <a name="guidelines-for-cluster-timeout-values"></a>Richtlinien für Timeoutwerte des Clusters 

Wägen Sie die Vor- und Nachteile sorgfältig ab, und machen Sie sich die Konsequenzen einer weniger strikten Überwachung des SQL Server-Clusters bewusst. Durch höhere Timeoutwerte des Clusters ergibt sich eine größere Toleranz gegenüber vorübergehenden Netzwerkproblemen, doch verlangsamt sich gleichzeitig die Reaktion auf schwerwiegende Fehler. Werden höhere Timeoutwerte für den Fall einer Ressourcenüberlastung oder hoher geografischer Latenz verwendet, verlängert sich auch die Zeit für die Wiederherstellung nach schwerwiegenden oder nicht behebbaren Fehlern. Zwar ist dies bei vielen Anwendungen akzeptabel, jedoch nicht in allen Fällen die optimale Lösung. 

Die Standardeinstellungen werden für schnelles Reagieren auf Symptome schwerwiegender Fehler und eine Begrenzung von Ausfallzeiten optimiert, doch können diese Einstellungen für bestimmte Arbeitslasten und Konfigurationen zu strikt sein. Es wird nicht empfohlen, `LeaseTimeout`, `CrossSubnetDelay`, `CrossSubnetThreshold`, `SameSubnetDelay`, `SameSubnetThreshold` oder `HealthCheckTimeout` auf Werte unterhalb der Standardwerte zu setzen. Die jeweils richtigen Einstellungen für eine Bereitstellung variieren und ergeben sich wahrscheinlich erst nach einem längeren Optimierungszeitraum. Nehmen Sie Änderungen an diesen Werten nur schrittweise und unter Berücksichtigung der Beziehungen und Abhängigkeiten vor, die zwischen diesen Werten bestehen. 

### <a name="relationship-between-cluster-timeout-and-lease-timeout"></a>Beziehung zwischen Timeouts für Cluster und Lease 

Die primäre Funktion des Leasemechanismus besteht darin, die SQL Server-Ressource dann offline zu nehmen, wenn der Clusterdienst nicht mit der Instanz kommunizieren kann, während ein Failover zu einem anderen Knoten erfolgt. Wenn der Cluster den Offlinevorgang auf der Clusterressource der Verfügbarkeitsgruppe ausführt, sendet der Clusterdienst einen RPC-Aufruf an „rhs.exe“ zum Offlineschalten der Ressource. Die Ressourcen-DLL verwendet gespeicherte Prozeduren, um SQL Server anzuweisen, die Verfügbarkeitsgruppe offline zu schalten, doch kann bei dieser gespeicherten Prozedur ein Fehler oder Timeout auftreten. Der Ressourcenhost beendet während des Offlineaufrufs auch seinen eigenen Leaseverlängerungsthread. Im ungünstigsten Fall bewirkt SQL Server ein Ablaufen der Lease nach ½ \* LeaseTimeout und den Übergang der Instanz in einen Auflösungsstatus. Failover können von mehreren verschiedenen Seiten aus initiiert werden, doch ist es äußerst wichtig, dass die Sicht des Clusterstatus im gesamten Cluster und auf allen SQL Server-Instanzen konsistent ist. Stellen Sie sich beispielsweise ein Szenario vor, in dem die primäre Instanz die Verbindung mit dem restlichen Cluster verliert. Aufgrund der Timeoutwerte des Clusters ermittelt jeder Knoten im Cluster zur gleichen Zeit einen Fehler, doch kann nur der primäre Knoten mit der primären SQL Server-Instanz interagieren, um ein Aufgeben der primären Rolle zu erzwingen. 

Aus Sichtweise des primären Knotens hat der Clusterdienst das Quorum verloren, und der Dienst beginnt sich selbst zu beenden. Der Clusterdienst gibt einen RPC-Aufruf an den Ressourcenhost zum Beenden des Prozesses aus. Dieser Beendigungsaufruf sorgt dafür, dass die Verfügbarkeitsgruppe auf der SQL Server-Instanz offline geschaltet wird. Dieser Offlineaufruf erfolgt über T-SQL, kann jedoch nicht gewährleisten, dass die Verbindung zwischen SQL und der Ressourcen-DLL erfolgreich hergestellt wird. 

Aus Sichtweise des restlichen Clusters steht derzeit kein primäres Replikat bereit, und es wird abgestimmt und ein einzelnes neues primäres Replikat für die verbleibenden Knoten im Cluster erstellt. Wenn bei der von der Ressourcen-DLL aufgerufenen gespeicherten Prozedur ein Fehler oder Timeout auftritt, könnte es beim Cluster zu einem Split-Brain-Szenario kommen. 

Durch das Timeout für die Lease werden Split-Brain-Szenarien bei Kommunikationsfehlern verhindert. Selbst wenn die gesamte Kommunikation fehlschlägt, wird der Prozess der Ressourcen-DLL beendet und die Lease kann nicht aktualisiert werden. Nach Ablauf der Lease wird die Verfügbarkeitsgruppe eigenständig offline geschaltet. Die SQL Server-Instanz muss erkennen, dass sie nicht mehr das primäre Replikat hostet, bevor der Cluster ein neues Replikat erstellt. Da der restliche Cluster, der für das Auswählen eines neuen primären Replikats zuständig ist, keine Möglichkeit zur Koordination mit dem aktuellen primären Replikat hat, wird durch die Timeoutwerte sichergestellt, dass ein neues primäres Replikat erst dann erstellt wird, wenn sich das aktuelle primäre Replikat offline geschaltet hat. 

Bei einem Failover des Clusters muss die SQL Server-Instanz, auf der das vorherige primäre Replikat gehostet ist, in einen Auflösungsstatus übergehen, bevor das neue primäre Replikat online geschaltet wird. Der SQL Server-Leasethread weist zu jedem Zeitpunkt eine verbleibende Gültigkeitsdauer von ½ \* LeaseTimeout auf, da bei jeder Verlängerung der Lease die neue Gültigkeitsdauer auf `LeaseInterval` oder ½ \* LeaseTimeout aktualisiert wird. Wenn der Clusterdienst oder der Ressourcenhost stoppt oder beendet wird, ohne das Leasebeendigungsereignis zu signalisieren, deklariert der Cluster den primären Knoten nach `SameSubnetThreshold`\ `SameSubnetDelay` Millisekunden als inaktiv. Innerhalb dieses Zeitraums muss die Lease ablaufen, damit sichergestellt ist, dass das primäre Replikat offline geschaltet ist. Da die maximale Gültigkeitsdauer für das Timeout der Lease ½ \* `LeaseTimeout` ist, muss ½ \* `LeaseTimeout` weniger als `SameSubnetThreshold` \* `SameSubnetDelay` betragen. 

`SameSubnetThreshold \<= CrossSubnetThreshold` und `SameSubnetDelay \<= CrossSubnetDelay` sollte für alle SQL Server-Cluster zutreffen. 

### <a name="health-check-timeout-operation"></a>Funktionsweise des Timeouts für die Integritätsprüfung 

Das Timeout für die Integritätsprüfung ist flexibler, da kein anderer Failovermechanismus direkt davon abhängt. Beim Standardwert von 30 Sekunden ist das `sp_server_diagnostics`-Intervall auf 10 Sekunden festgelegt, mit einem Mindestwert von 15 Sekunden für das Timeout und einem Intervall von 5 Sekunden. Allgemeiner ausgedrückt: Das `sp_server_diagnositcs`-Aktualisierungsintervall beträgt immer 1/3 \* `HealthCheckTimeout`. Wenn die Ressourcen-DLL in einem Intervall keinen neuen Satz von Integritätsdaten erhält, verwendet sie weiterhin die Integritätsdaten aus dem vorherigen Intervall, um die aktuelle Verfügbarkeitsgruppe und den Zustand der Instanz zu ermitteln. Durch einen höheren Timeoutwert für die Integritätsprüfung, weist das primäre Replikat eine größere Toleranz gegenüber der CPU-Auslastung auf, wodurch `sp_server_diagnostics` daran gehindert werden kann, in jedem Intervall neue Daten bereitzustellen, jedoch werden für einen längeren Zeitraum Integritätsprüfungen auf Grundlage veralteter Daten durchgeführt. Unabhängig vom Timeoutwert geschieht Folgendes: Sobald Daten empfangen werden, die darauf hinweisen, dass das Replikat nicht fehlerfrei ist, wird im nächsten `IsAlive`-Aufruf zurückgegeben, dass die Instanz fehlerhaft ist, und der Clusterdienst initiiert ein Failover. 

Die Fehlerbedingungsebene der Verfügbarkeitsgruppe ändert die Fehlerbedingungen für die Integritätsprüfung. Für jede Fehlerebene gilt: Wenn das Verfügbarkeitsgruppenelement von `sp_server_diagnostics` als fehlerhaft gemeldet wird, schlägt die Integritätsprüfung fehl. Jede Ebene übernimmt alle Fehlerbedingungen der darunter liegenden Ebenen. 


| Ebene | Bedingung, unter der die Instanz als inaktiv betrachtet wird
|:---|---
| 1: OnServerDown | Die Integritätsprüfung führt keine Aktion aus, wenn Fehler bei anderen Ressourcen als der Verfügbarkeitsgruppe auftreten. Wenn innerhalb von 5 Intervallen oder 5/3 \* HealthCheckTimeout keine Verfügbarkeitsgruppendaten eintreffen.
| 2: OnServerUnresponsive | Wenn keine Daten von `sp_server_diagnostics` für HealthCheckTimeout empfangen werden.
| 3: OnCriticalServerError | (Standardeinstellung) Wenn die Systemkomponente einen Fehler meldet.
| 4: OnModerateServerError | Wenn die Ressourcenkomponente einen Fehler meldet. 
| 5:  OnAnyQualifiedFailureConitions |  Wenn die Abfrageverarbeitungskomponente einen Fehler meldet.

## <a name="updating-cluster-and-always-on-timeout-values"></a>Aktualisieren von Timeoutwerten für Cluster und Always On 

### <a name="cluster-values"></a>Clusterwerte 

Es gibt vier Werte in der WSFC-Konfiguration, die zur Bestimmung von Timeoutwerten für den Cluster herangezogen werden: 

  - SameSubnetDelay 
  - SameSubnetThreshold 
  - CrossSubnetDelay 
  - CrossSubnetThreshold 

Die Verzögerungswerte (Delay) bestimmen die Wartezeit zwischen Takten vom Clusterdienst. Die Schwellenwerte (Threshold) legen die Anzahl der Takte fest, die keine Bestätigung vom Zielknoten oder der Ressource empfangen können, bevor das Objekt vom Cluster als inaktiv deklariert wird. Erfolgt für mehr als `SameSubnetDelay \* SameSubnetThreshold` Millisekunden kein erfolgreicher Takt zwischen Knoten im gleichen Subnetz, wird der Knoten als inaktiv erkannt. Dasselbe gilt für die Kommunikation im gesamten Subnetz unter Verwendung subnetzübergreifender Werte. 

Zum Auflisten aller aktuellen Clusterwerte öffnen Sie auf einem beliebigen Knoten im Zielcluster einen PowerShell-Terminal mit erhöhten Rechten. Führen Sie den folgenden Befehl aus:

```PowerShell
 Get-Cluster | fl *
``` 

Um einen dieser Werte zu aktualisieren, führen Sie den folgenden Befehl auf einem PowerShell-Terminal mit erhöhten Rechten aus:

```PowerShell
(Get-Cluster).<ValueName> = <NewValue>
```

Wenn Sie das Produkt aus Verzögerung \* Schwellenwert erhöhen, damit das Timeout für den Cluster eine größere Toleranz aufweist, ist es effektiver, zuerst den Verzögerungswert und erst dann den Schwellenwert zu erhöhen. Durch einen höheren Verzögerungswert wird der Zeitraum zwischen den einzelnen Takten verlängert. Ein längerer Zeitraum zwischen den Takten bietet mehr Zeit, damit sich vorübergehende Netzwerkprobleme von selbst lösen können, und verringert eine Netzwerküberlastung im Vergleich zum Senden von mehr Takten im gleichen Zeitraum. 

### <a name="lease-timeout"></a>Timeout für Lease 

Der Leasemechanismus wird durch einen einzelnen Wert gesteuert, der für jede Verfügbarkeitsgruppe in einem WSFC-Cluster spezifisch ist. Um zu diesem Wert zu navigieren, gehen Sie im Failovercluster-Manager folgendermaßen vor:

1. Suchen Sie auf der Registerkarte „Rollen“ nach der Rolle der Zielverfügbarkeitsgruppe. Klicken Sie auf die Rolle der Zielverfügbarkeitsgruppe. 
2. Klicken Sie mit der rechten Maustaste auf die Verfügbarkeitsgruppenressource am unteren Rand des Fensters, und wählen Sie **Eigenschaften** aus. 

   ![Failovercluster-Manager](media/availability-group-lease-healthcheck-timeout/image2.png) 

3. Navigieren Sie im Popupfenster zur Registerkarte „Eigenschaften“. Dort finden Sie eine Liste spezifischer Werte für diese Verfügbarkeitsgruppe. Klicken Sie auf den LeaseTimeout-Wert, um ihn zu ändern. 

   ![Eigenschaften](media/availability-group-lease-healthcheck-timeout/image3.png) 


   Je nach Konfiguration der Verfügbarkeitsgruppe sind möglicherweise zusätzliche Ressourcen für Listener, freigegebene Datenträger, Dateifreigaben usw. vorhanden. Diese Ressourcen erfordern keine zusätzliche Konfiguration. 

> [!NOTE]  
>  Der neue Wert der Eigenschaft „LeaseTimeout“ wird wirksam, nachdem die Ressource offline und wieder online geschaltet wurde.
   
### <a name="health-check-values"></a>Werte für die Integritätsprüfung 

Zwei Werte steuern die Always On-Integritätsprüfung: FailureConditionLevel und HealthCheckTimeout. FailureConditionLevel gibt die Toleranzstufe für bestimmte Fehlerbedingungen an, die von `sp_server_diagnostics` gemeldet werden. HealthCheckTimeout legt den Zeitraum fest, über den die Ressourcen-DLL keine Aktualisierung von `sp_server_diagnostics` empfangen muss. Das Aktualisierungsintervall für `sp_server_diagnostics` ist immer HealthCheckTimeout / 3. 

Verwenden Sie zum Konfigurieren der Failoverbedingungsebene die Option `FAILURE_CONDITION_LEVEL = <n>` der Anweisung `CREATE` oder `ALTER` `AVAILABILITY GROUP`, wobei `<n>` eine ganze Zahl zwischen 1 und 5 ist. Mit dem folgenden Befehl wird die Fehlerbedingungsebene für die Verfügbarkeitsgruppe „AG1“ auf 1 festgelegt: 

```sql
ALTER AVAILABILITY GROUP AG1 SET (FAILURE_CONDITION_LEVEL = 1); 
```

Verwenden Sie zum Konfigurieren des Timeouts für die Integritätsprüfung die Option `HEALTH_CHECK_TIMEOUT` der Anweisung `CREATE` oder `ALTER` `AVAILABILITY GROUP`. Mit dem folgenden Befehl wird das Timeout für die Integritätsprüfung für die Verfügbarkeitsgruppe „AG1“ auf 60.000 Millisekunden festgelegt: 


```sql
ALTER AVAILABILITY GROUP AG1 SET (HEALTH_CHECK_TIMEOUT =60000);
```

## <a name="summary-of-timeout-guidelines"></a>Zusammenfassung der Richtlinien für Timeouts 

  - Es wird davon abgeraten, Timeouts auf Werte unterhalb der Standardwerte zu setzen. 

  - Das Leaseintervall (½ \* LeaseTimeout) muss kürzer als SameSubnetThreshold \* SameSubnetDelay sein. 

  - SameSubnetThreshold \<= CrossSubnetThreshold 

  - SameSubnetDelay \<= CrossSubnetDelay 
  
 | Timeouteinstellung | Zweck | Zwischen | Verwendung | IsAlive & LooksAlive | Ursachen | Ergebnis 
 | :-------------- | :------ | :------ | :--- | :------------------- | :----- | :------ |
 | Timeout für Lease </br> **Standardwert: 20000** | Split Brain verhindern | Vom primären Replikat auf den Cluster </br> (HADR) | [Windows event objects (Windows-Ereignisobjekte)](/windows/desktop/Sync/event-objects)| Wird in beiden verwendet | Nicht reagierendes Betriebssystems, unzureichender virtueller Arbeitsspeicher, Arbeitssatzpaging, Generieren von Speicherabbildern, voll ausgelastete CPU, WSFC ist offline (Verlust des Quorums) | Verfügbarkeitsgruppenressource von offline zu online geschalten, dann wird ein Failover ausgeführt |  
 | Sitzungstimeout </br> **Standardwert: 10000** | Melden von Kommunikationsproblemen zwischen dem primären und sekundären Replikat | Sekundäres Replikat auf primäres Replikat </br> (HADR) | [TCP Sockets (messages sent via DBM endpoint) (TCP-Sockets (über einen DBM-Endpunkt gesendete Meldungen))](/windows/desktop/WinSock/windows-sockets-start-page-2) | In keinem von beiden verwendet | Netzwerkkommunikation, </br> Probleme auf dem sekundären Replikat (offline), nicht reagierendes Betriebssystem, Ressourcenkonflikte | Verbindung des sekundären Replikats wird getrennt | 
 |HealthCheck-Timeout  </br> **Standardwert: 30000** | Angeben von Timeouts, während die Integrität des primären Replikats ermittelt wird | Cluster auf primäres Replikat </br> (FCI & HADR) | T-SQL [sp_server_diagnostics](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) | Wird in beiden verwendet | Erfüllte Fehlerbedingungen, nicht reagierendes Betriebssystem, unzureichender virtueller Speicher, Kürzung des Arbeitssatzes, WSFC ist offline (Verlust des Quorums), Probleme mit dem Scheduler (Deadlock)| Verfügbarkeitsgruppenressource wird von offline zu online geschaltet, oder es wird ein Failover dafür ausgeführt, FCI-Neustart oder -Failover |  
  | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp;| &nbsp; | &nbsp; | &nbsp; |

## <a name="see-also"></a>Weitere Informationen    

[Aktive sekundäre Replikate: Sicherung auf sekundären Replikaten &#40;Always On-Verfügbarkeitsgruppen&#41;](active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)

[ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)         

