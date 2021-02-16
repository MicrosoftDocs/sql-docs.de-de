---
title: Datenbankspiegelungs-Monitor (Seite Status) | Microsoft-Dokumentation
description: Verwenden Sie diese schreibgeschützte Datenbankspiegelungs-Monitor-Seite, um den Spiegelungsstatus für den Prinzipal und die Spiegelungsserverinstanzen einer ausgewählten Datenbank anzuzeigen.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.status.f1
ms.assetid: 4f64b4e1-89e9-4827-98fa-b92c3dc73b48
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2d912623497cc6dc0baac1c4fbb4c8e794363ce8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353112"
---
# <a name="database-mirroring-monitor-status-page"></a>Datenbankspiegelungs-Monitor (Seite Status)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Diese schreibgeschützte Seite zeigt den neuesten Spiegelungsstatus für die Prinzipal- und die Spiegelserverinstanz der Datenbank an, die zum jetzigen Zeitpunkt in der Navigationsstruktur ausgewählt ist. Wenn Informationen zu einer Instanz zurzeit nicht verfügbar sind, sind einige der Zellen im Raster **Status** , das dieser Instanz entspricht, ausgegraut und zeigen die Option **Unbekannt** an.  
  
 **So verwenden Sie SQL Server Management Studio zum Überwachen der Datenbankspiegelung**  
  
-   [Starten des Datenbankspiegelungs-Monitors &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="options"></a>Tastatur  
 **Status**  
 Zeigt ein Raster an, das den neuesten Spiegelungsstatus höherer Ebene der Prinzipal- und der Spiegelserverinstanz enthält. Die Zeilen des Rasters **Status** liegen in folgender Reihenfolge vor:  
  
-   Prinzipalserverinstanz  
  
-   Spiegelserverinstanz  
  
 Es gibt folgende Spalten:  
  
|Spaltenname|BESCHREIBUNG|  
|-----------------|-----------------|  
|**Serverinstanz**|Name der Serverinstanz, deren Status in der Zeile **Status** angezeigt wird.|  
|**Aktuelle Rolle**|Aktuelle Rolle der Serverinstanz: **Prinzipal** oder **Spiegel**.|  
|**Spiegelungsstatus**|Der von der Serverinstanz gemeldete Spiegelungsstatus und ein Symbol, das den Schweregrad des Status angibt. Im Folgenden sind die möglichen Statuswerte und die zugehörigen Symbole aufgeführt:<br /><br /> Symbol: —, Status **Unbekannt**. Der Monitor ist mit keinem der beiden Partner verbunden. Es sind nur die Informationen verfügbar, die vom Monitor zwischengespeichert wurden.<br /><br /> Symbol: Warnsymbol, Status **Wird synchronisiert**. Der Inhalt der Spiegeldatenbank liegt zeitlich hinter dem Inhalt der Prinzipaldatenbank. Die Prinzipalserverinstanz sendet Protokolldatensätze an die Spiegelserverinstanz, die die Änderungen auf die Spiegeldatenbank anwendet, um ein Rollforward dafür auszuführen. Beim Start einer Datenbank-Spiegelungssitzung befinden sich Spiegel- und Prinzipaldatenbank in diesem Status.<br /><br /> Symbol: Standard-Datenbankzylinder, Status **Synchronisiert**. Wenn der Spiegelserver den Stand des Prinzipalservers erreicht hat, wechselt der Datenbankstatus zu **Synchronisiert**. Die Datenbank verbleibt in diesem Status, solange der Prinzipalserver Änderungen an den Spiegelserver sendet und der Spiegelserver Änderungen auf die Spiegeldatenbank anwendet.  Im Modus für hohe Sicherheit ist sowohl das automatische Failover als auch das manuelle Failover ohne Datenverlust möglich.  Im Modus für hohe Leistung ist immer ein gewisser Datenverlust möglich, sogar im Status **Synchronisiert** .<br /><br /> Symbol: Warnsymbol, Status **Angehalten**. <br />                            Die Prinzipaldatenbank ist verfügbar, sendet aber keine Protokolle an den Spiegelserver.<br /><br /> Symbol: Fehlersymbol, Status **Getrennt**. Die Serverinstanz kann keine Verbindung mit ihrem Partner herstellen.|  
|**Zeugenverbindung**|Der Verbindungsstatus des Zeugen, dem ein Statussymbol vorangestellt ist: **Unbekannt**, **Verbunden** oder **Getrennt**.|  
|**History**|Klicken Sie auf diese Schaltfläche, um den Verlauf der Spiegelung auf der Serverinstanz anzuzeigen. Dadurch wird das Dialogfeld **Datenbankspiegelungsverlauf** geöffnet, das den Verlauf des Spiegelungsstatus und Statistiken für eine gespiegelte Datenbank auf einer bestimmten Serverinstanz anzeigt.<br /><br /> Die Schaltfläche **Verlauf** ist abgeblendet, wenn der Monitor nicht mit der Serverinstanz verbunden ist.|  
  
 **Prinzipalprotokoll (** *\<time>* **)**  
 Status des Protokolls auf der Prinzipalserverinstanz zur Ortszeit auf der Serverinstanz, angegeben durch *\<time>* . Folgende Parameter werden angezeigt:  
  
 **Nicht gesendetes Protokoll**  
 Die Menge der Protokolldaten (in Kilobyte), die sich in der Sendewarteschlange befinden.  
  
 **Älteste, nicht gesendete Transaktion**  
 Alter der ältesten, nicht gesendeten Transaktion in der Sendewarteschlange. Das Alter dieser Transaktion gibt an, wie viele Minuten an Transaktionen noch nicht an die Spiegelserverinstanz gesendet wurden. Dieser Wert hilft, das Datenverlustrisiko in Bezug auf die Zeit zu messen.  
  
 **Zeit zum Senden des Protokolls (geschätzt)**  
 Die ungefähre Dauer der Zeit, die die Prinzipalserverinstanz benötigt, um das derzeit in der Sendewarteschlange befindliche Protokoll an die Spiegelserverinstanz zu senden (die *Senderate*). Da die Rate der eingehenden Transaktionen erheblich schwanken kann, handelt es sich bei der Protokollsendezeit um einen Schätzwert. Die Senderate kann jedoch nützlich sein, um die für ein manuelles Failover erforderliche Zeit grob zu schätzen.  
  
 **Aktuelle Senderate**  
 Rate, in Kilobyte (KB) pro Sekunde, mit der Transaktionen an die Spiegelserverinstanz gesendet werden.  
  
 **Aktuelle Rate neuer Transaktionen**  
 Rate, mit der eingehende Transaktionen in das Protokoll des Prinzipals eingetragen werden (in KB pro Sekunde). Sie können feststellen, ob die Spiegelung zurück liegt, gleich schnell ist oder aufholt, indem Sie diesen Wert mit dem Wert von **Zeit zum Senden des Protokolls (geschätzt)** vergleichen.  
  
 **Spiegelungsprotokoll (** *\<time>* **)**  
 Protokollstatus auf der Spiegelserverinstanz zur Ortszeit auf der Serverinstanz, angegeben durch *\<time>* . Folgende Parameter werden angezeigt:  
  
 **Nicht wiederhergestelltes Protokoll**  
 Die Menge der Protokolldaten in KB, die sich in der Wiederholungswarteschlange befinden.  
  
 **Zeit zum Wiederherstellen des Protokolls (geschätzt)**  
 Die geschätzte Anzahl von Minuten, die erforderlich ist, um das Protokoll, das sich derzeit in der Wiederholungswarteschlange befindet, auf die Spiegeldatenbank anzuwenden.  
  
 **Aktuelle Wiederherstellungsrate**  
 Rate, mit der Transaktionen in der Spiegelserverinstanz wiederhergestellt werden (in KB pro Sekunde).  
  
 **Spiegelungscommitaufwand**  
 Gibt an, wie viele Millisekunden durchschnittlicher Verzögerung pro Transaktion toleriert werden, bevor eine Warnung auf dem Prinzipalserver generiert wird. Hierbei handelt es sich um die Verzögerung, die entsteht, während die Prinzipalserverinstanz darauf wartet, dass die Spiegelserverinstanz den Transaktionsprotokolldatensatz in die Wiederholungswarteschlange schreibt. Dieser Wert ist nur im Modus für hohe Sicherheit relevant.  
  
 **Zeit zum Senden und Wiederherstellen aller aktuellen Protokolle (geschätzt)**  
 Die benötigte Zeit zum Senden und Wiederherstellen des gesamten Protokolls, für das zum aktuellen Zeitpunkt auf dem Prinzipal ein Commit ausgeführt wurde. Diese Zeit kann geringer sein als die Summe der Werte der Felder **Zeit zum Senden des Protokolls (geschätzt)** und **Zeit zum Wiederherstellen des Protokolls (geschätzt)** , da das Senden und Wiederherstellen parallel erfolgen kann. Diese Schätzung sagt die Zeit voraus, die zum Senden und Wiederherstellen neuer Transaktionen benötigt wird, für die beim Bearbeiten von Rückständen in der Sendewarteschlange auf dem Prinzipal ein Commit ausgeführt wurde.  
  
 **Zeugenadresse**  
 Netzwerkadresse der Zeugenserverinstanz. Informationen zum Format dieser Adresse finden Sie unter [Angeben einer Servernetzwerkadresse &#40;Datenbankspiegelung&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
 **Betriebsmodus**  
 Der Betriebsmodus der Datenbank-Spiegelungssitzung:  
  
-   **Hohe Leistung (asynchron)**  
  
-   **Hohe Sicherheit ohne automatisches Failover (synchron)**  
  
-   **Hohe Sicherheit mit automatischem Failover (synchron)**  
  
## <a name="remarks"></a>Bemerkungen  
 Mitglieder der festen Datenbankrolle **dbm_monitor** können den vorhandenen Spiegelungsstatus entweder mithilfe des Datenbankspiegelungs-Monitors oder der gespeicherten Prozedur **sp_dbmmonitorresults** anzeigen. Diese Benutzer können jedoch nicht die Statustabelle aktualisieren. Sie sind abhängig davon, dass der **Auftrag für den Datenbankspiegelungs-Monitor** die Statustabelle regelmäßig aktualisiert. Das Alter des angezeigten Status kann ein Benutzer über die Zeitangaben in den Bezeichnungen **Prinzipalprotokoll (** _\<time>_ **)** und **Spiegelungsprotokoll (** _\<time>_ **)** erfahren.  
  
 Wenn dieser Auftrag nicht vorhanden ist oder der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent beendet wurde, veraltet der Status zunehmend und gibt die Konfiguration der Spiegelungssitzung möglicherweise nicht mehr wieder. So kann z. B. nach einem Failover fälschlicherweise angezeigt werden, dass die Partner dieselbe Rolle haben (Prinzipal oder Spiegel), oder der aktuelle Prinzipalserver wird als Spiegel angezeigt, während der aktuelle Spiegelserver als Prinzipal angezeigt wird.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Starten des Datenbankspiegelungs-Monitors &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Überwachen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Starten des Assistenten zum Konfigurieren der Sicherheit für die Datenbankspiegelung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
