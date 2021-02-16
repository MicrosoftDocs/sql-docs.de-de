---
title: Upgraden oder Patchen replizierter Datenbanken | Microsoft-Dokumentation
description: SQL Server unterstützt Upgrades für replizierte Datenbanken von früheren SQL Server-Versionen, ohne die Aktivitäten auf anderen Knoten zu unterbrechen.
ms.custom: ''
ms.date: 07/24/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- merge replication database upgrades [SQL Server replication]
- replication [SQL Server], upgrading
- transactional replication, upgrading databases
- snapshot replication [SQL Server], upgrading databases
- upgrading replicated databases
ms.assetid: 9926a4f7-bcd8-4b9b-9dcf-5426a5857116
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 72b1d22d14cf83d44743d2081ee051bd02faf342
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351019"
---
# <a name="upgrade-or-patch-replicated-databases"></a>Upgraden oder Patchen replizierter Datenbanken

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] unterstützt das Aktualisieren replizierter Datenbanken von früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Während der Aktualisierung eines Knotens müssen die auf anderen Knoten ausgeführten Aktivitäten nicht beendet werden. Stellen Sie sicher, dass die Regeln, die im Hinblick auf die in einer Topologie unterstützten Versionen gelten, eingehalten werden:  
  
-   Für den Verteiler ist jede Version zulässig, die der Verleger-Version entspricht oder höher als diese ist. (In vielen Fällen gehören Verteiler und Verleger derselben Instanz an.)    
-   Für den Verleger ist jede Version zulässig, die der Verteiler-Version entspricht oder niedriger als diese ist.    
-   Die Abonnenten-Version ist vom Veröffentlichungstyp abhängig:    
    - Ein Abonnent einer Transaktionsveröffentlichung kann einer der beiden Versionen im Rahmen der Verlegerversion angehören. Zum Beispiel: Ein SQL Server 2012-Verleger (11.x) kann SQL Server 2014-Abonnenten (12.x) und SQL Server 2016-Abonnenten (13.x) vorweisen. Ein SQL Server 2016-Verleger (13.x) kann SQL Server 2014-Abonnenten (12.x) und SQL Server 2012-Abonnenten (11.x) vorweisen.     
    - Ein Abonnent für eine Mergeveröffentlichung kann alle Versionen, die gleich oder niedriger sind als die Verlegerversion, benutzen. Diese werden gemäß dem Supportzeitraums für den Versionenlebenszyklus unterstützt.  
 
Der Upgradepfad für SQL Server unterscheidet sich je nach Bereitstellungsmuster. Grundsätzlich stehen zwei Upgradepfade für SQL Server zur Verfügung:
- Parallel: Stellen Sie eine parallele Umgebung bereit, und verschieben Sie Datenbanken gemeinsam mit den zugehörigen Objekten auf Instanzebene, z.B. Anmeldenamen, Aufträge usw., in die neue Umgebung. 
- Direktes Upgrade: Führen Sie das Upgrade der vorhandenen SQL Server-Installation mithilfe des SQL Server-Installationsmediums durch. Dazu werden die SQL Server-Bits ersetzt und die Datenbankobjekte aktualisiert. Für Umgebungen, in denen Always On-Verfügbarkeitsgruppen oder Failoverclusterinstanzen ausgeführt werden, werden direkte Upgrades mit einem [parallelen Upgrade](choose-a-database-engine-upgrade-method.md#rolling-upgrade) kombiniert, um die Downtime zu minimieren. 

Das Verschieben von Verleger-Abonnenten-Paaren in Teilen in die neue parallele Umgebung wurde anstelle der Verschiebung der gesamten Topologie als gängige Methode für parallele Upgrades für Replikationstopologien eingeführt. Mithilfe eines solchen inkrementellen Ansatzes lässt sich die Downtime steuern, und die Auswirkungen auf das Unternehmen, das auf die Replikation angewiesen ist, lassen sich zumindest bis zu einem gewissen Grad minimieren.  

Der Großteil dieses Artikels bezieht sich auf das Upgrade der SQL Server-Version. Die Vorgehensweise für direktes Upgrade sollte aber auch beim Patchen von SQL Server mit einem Service Pack oder kumulativen Update verwendet werden. 

 >[!WARNING]
 > Das Upgrade einer Replikationstopologie ist ein aus mehreren Schritten bestehender Vorgang. Es wird empfohlen, ein Upgrade eines Replikats Ihrer Replikationstopologie in einer Testumgebung auszuprobieren, bevor das Upgrade in der eigentlichen Produktionsumgebung durchgeführt wird. Dadurch lässt sich jegliche für eine reibungslose Durchführung des Upgrades erforderliche Dokumentation ganz ohne zusätzliche Kosten und lange Downtimes während des eigentlichen Upgradevorgangs ins Reihe bringen. Kunden konnten die Downtime beim Upgrade ihrer Replikationstopologie mithilfe von Always On-Verfügbarkeitsgruppen und bzw. oder SQL Server-Failoverclusterinstanzen für ihre Produktionsumgebungen deutlich reduzieren. Darüber hinaus wird empfohlen, dass Sie Sicherungen aller Datenbanken anlegen, einschließlich MSDB, Master-, Verteilungs- und Benutzerdatenbanken, die an der Replikation beteiligt sind, bevor Sie das Upgrade ausführen.


## <a name="replication-matrix"></a>Replikationsmatrix

[!INCLUDE[repl matrix](../../includes/replication-compat-matrix.md)]
  
## <a name="run-the-log-reader-agent-for-transactional-replication-before-upgrade"></a>Führen Sie den Protokolllese-Agent für die Transaktionsreplikation vor dem Upgrade aus.  
 Bevor Sie [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)]aktualisieren, müssen Sie sich vergewissern, dass alle Transaktionen mit ausgeführtem Commit von veröffentlichten Tabellen vom Protokolllese-Agent verarbeitet wurden. Um sicherzustellen, dass alle Transaktionen verarbeitet wurden, führen Sie die folgenden Schritte für jede Datenbank aus, die Transaktionsveröffentlichungen enthält:  
  
1.  Stellen Sie sicher, dass der Protokolllese-Agent für die Datenbank ausgeführt wird. Standardmäßig wird der Agent ununterbrochen ausgeführt.    
2.  Beenden Sie die Benutzeraktivität auf veröffentlichten Tabellen.  
3.  Warten Sie eine gewissen Zeit, bis der Protokolllese-Agent die Transaktionen in die Verteilungsdatenbank kopiert hat, und beenden Sie dann den Agent.  
4.  Führen Sie [sp_replcmds](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md) aus, um zu überprüfen, ob alle Transaktionen verarbeitet wurden. Das Resultset dieser Prozedur sollte leer sein.   
5.  Führen Sie [sp_replflush](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md) aus, um die Verbindung von „sp_replcmds“ zu trennen.   
6.  Führen Sie das Serverupgrade auf die neueste Version von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] durch.   
7.  Starten Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent und den Protokolllese-Agent neu, wenn sie nach dem Upgrade nicht automatisch starten.  
  
## <a name="run-agents-for-merge-replication-after-upgrade"></a>Ausführen von Agents nach dem Upgrade für die Mergereplikation  
 Führen Sie nach dem Upgrade für jede Mergeveröffentlichung den Momentaufnahme-Agent und für jedes Abonnement den Merge-Agent aus, um die Replikationsmetadaten zu aktualisieren. Sie müssen die neue Momentaufnahme nicht anwenden, da sie für die erneute Initialisierung der Abonnements nicht benötigt wird. Die Metadaten des Abonnements werden aktualisiert, sobald der Merge-Agent zum ersten Mal nach dem Upgrade ausgeführt wird. Dies bedeutet, dass die Abonnementdatenbank während des Upgrades des Verlegers online und aktiv bleiben kann.  
  
 Die Mergereplikation speichert die Metadaten der Veröffentlichung und des Abonnements in einer Reihe von Systemtabellen in den Veröffentlichungs- und Abonnement-Datenbanken. Bei Ausführung des s werden die Veröffentlichungsmetadaten aktualisiert, und bei Ausführung des Merge-Agents werden die Abonnementmetadaten aktualisiert. Der Agent wird nur benötigt, um eine Momentaufnahme der Veröffentlichung zu generieren. Wenn bei einer Mergeveröffentlichung parametrisierte Filter verwendet werden, gibt es auch für jede Partition eine Momentaufnahme. Diese partitionierten Momentaufnahmen zu aktualisieren, ist nicht erforderlich.  
  
 Die Agents werden in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], im Replikationsmonitor oder in der Befehlszeile ausgeführt. Weitere Informationen zum Ausführen des Momentaufnahme-Agents finden Sie in den folgenden Artikeln:  
  
-   [Erstellen und Anwenden der Anfangsmomentaufnahme](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)
-   [Starten und Beenden eines Replikations-Agents &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)
-   [Erstellen und Anwenden der Anfangsmomentaufnahme](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)
-   [Ausführbare Konzepte für die Programmierung von Replikations-Agent](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  

Weitere Informationen zum Ausführen des Merge-Agents finden Sie in den folgenden Artikeln:
-   [Synchronisieren eines Pullabonnements](../../relational-databases/replication/synchronize-a-pull-subscription.md)
-   [Synchronisieren eines Pushabonnements](../../relational-databases/replication/synchronize-a-push-subscription.md)  
  

Nach dem Upgrade von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in einer Topologie, in der die Mergereplikation verwendet wird, müssen Sie den Kompatibilitätsgrad aller Veröffentlichungen ändern, um neue Funktionen verwenden zu können.  
  
## <a name="upgrading-to-standard-workgroup-or-express-editions"></a>Aktualisieren auf die Standard Edition, Workgroup Edition oder Express Edition  
Bevor eine Edition von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] auf eine andere Edition aktualisiert wird, sollten Sie überprüfen, ob die derzeit verwendete Funktionalität in der Edition, die Ziel des Upgrades ist, unterstützt wird. Weitere Informationen finden Sie im Abschnitt zur Replikation im Thema [Editions and supported features of SQL Server (Editionen und unterstützte Funktionen von SQL Server)](../../sql-server/editions-and-components-of-sql-server-2017.md).  

## <a name="steps-to-upgrade-a-replication-topology"></a>Schritte für das Upgrade einer Replikationstopologie
In diesen Schritten wird die Reihenfolge dargestellt, in der Upgrades für Replikationstopologien durchgeführt werden sollten. Dieselben Schritte gelten sowohl für die Transaktions- als auch für die Mergereplikation. Diese Schritte gelten jedoch nicht für Peer-zu-Peer-Replikation, Abonnements mit verzögertem Update über eine Warteschlange und Abonnements mit sofortigem Update. 

### <a name="in-place-upgrade"></a>Direktes Upgrade 
1. Aktualisieren Sie den Verteiler. 
2. Aktualisieren Sie den Verleger und den Abonnenten. Diese Upgrades können Sie in beliebiger Reihenfolge durchführen. 

 >[!NOTE]
 > Für SQL Server 2008 und 2008 R2 müssen die Upgrades von Verleger und Abonnent gleichzeitig durchgeführt werden, damit sie der Matrix der Replikationstopologie entsprechen. Ein SQL Server 2008- oder 2008 R2-Verleger oder -Abonnent kann über keinen SQL Server 2016-Verleger oder Abonnent (oder höher) verfügen. Wenn ein gleichzeitiges Upgrade nicht möglich ist, verwenden Sie ein Zwischenupgrade auf SQL Server 2014 für die SQL Server-Instanzen, und führen Sie anschließend ein Upgrade auf SQL Server 2016 (oder höher) aus.  

### <a name="side-by-side-upgrade"></a>Paralleles Upgrade
1. Aktualisieren Sie den Verteiler.
1. Konfigurieren Sie die [Verteilung](../../relational-databases/replication/configure-distribution.md) in der neuen SQL Server-Instanz neu.
1. Aktualisieren Sie den Verleger.
1. Aktualisieren Sie den Abonnenten.
1. Konfigurieren Sie alle Verleger-Abonnenten-Paare, einschließlich der erneuten Initialisierung des Abonnenten, neu. 


## <a name="steps-for-side-by-side-migration-of-the-distributor-to-windows-server-2012-r2"></a>Schritte für die parallele Migration des Verteilers zu Windows Server 2012 R2
Wenn Sie ein Upgrade Ihrer SQL Server-Instanz auf SQL Server 2016 (oder höher) planen und Ihr aktuelles Betriebssystem Windows Server 2008 (oder 2008 R2) ist, müssen Sie für das Betriebssystem ein paralleles Upgrade auf Windows Server 2012 R2 oder höher ausführen. Der Grund für dieses zwischengeschaltete Betriebssystemupgrade ist, dass SQL Server 2016 nicht auf einem Server mit Windows Server 2008/2008 R2 installiert werden kann und für Windows Server 2008/2008 R2 kein direktes Upgrade auf Windows Server 2016 möglich ist. Obwohl ein direktes Upgrade von Windows Server 2008/2008 R2 auf Windows Server 2012 und anschließend auf Windows Server 2016 möglich ist, wird dies aufgrund der Ausfallzeit und der höheren Komplexität, die einen einfachen Rollbackpfad verhindern, generell nicht empfohlen. Ein paralleles Upgrade ist der einzige Upgradepfad, der für zu einem Failovercluster gehörige SQL Server-Instanzen verfügbar ist.  Die folgenden Schritte können Sie sowohl in einer eigenständigen SQL Server-Instanz als auch innerhalb einer Always On-Failoverclusterinstanz (FCI) ausführen.

1. Richten Sie eine neue SQL Server-Instanz (entweder eigenständig oder in einem Always On-Failovercluster) mit der gleichen Edition und Version des Windows Server 2012 R2/2016-Verteilers und einem anderen Windows-Cluster sowie SQL Server-FCI-Namen bzw. eigenständigen Hostnamen. Führen Sie keine Änderungen an der Verzeichnisstruktur durch, um sicherzustellen, dass sich die ausführbaren Replikations-Agents, Replikationsordner und Datenbankdateipfade in der neuen Umgebung im gleichen Pfad befinden. Damit werden die erforderlichen Schritte nach der Migration bzw. dem Upgrade reduziert.
1. Stellen Sie sicher, dass Ihre Replikation synchronisiert ist, und beenden Sie dann alle Replikations-Agents. 
1. Beenden Sie die aktuelle SQL Server-Verteilerinstanz. Wenn es sich dabei um eine eigenständige Instanz handelt, fahren Sie den Server herunter. Wenn es sich um eine SQL Server-FCI handelt, stellen Sie die gesamte SQL Server-Rolle und den Netzwerknamen im Cluster-Manager offline. 
1. Entfernen Sie die Einträge für die DNS- und AD-Computerobjekte für die alte Umgebung (aktuelle Verteilerinstanz). 
1. Ändern Sie den Hostnamen des neuen Servers in den des alten Servers.
    1. Wenn es sich dabei um eine SQL Server-FCI handelt, geben Sie der neuen SQL Server-FCI den gleichen virtuellen Servernamen wie der alten Instanz. 
1. Kopieren Sie die Datenbankdateien aus der vorherigen Instanz mithilfe einer SAN-Umleitung, Speicherkopie oder Dateikopie. 
1. Stellen Sie die neue SQL Server-Instanz online. 
1. Starten Sie alle Replikations-Agents neu, und überprüfen Sie, ob die Agents erfolgreich ausgeführt werden.
1. Prüfen Sie, ob die Replikation erwartungsgemäß funktioniert. 
1. Verwenden Sie die SQL Server-Setupmedien zum Ausführen eines direkten Upgrades Ihrer SQL Server-Instanz auf die neue Version von SQL Server. 


  >[!NOTE]
  > Zum Verkürzen der Downtime wird empfohlen, die *parallele Migration* des Verteilers und das *direkte Upgrade auf SQL Server 2016* jeweils als separate Aktivitäten durchzuführen. Damit können Sie eine Vorgehensweise mit Phasen anwenden, das Risiko reduzieren und die Downtime minimieren.

## <a name="web-synchronization-for-merge-replication"></a>Websynchronisierung für die Mergereplikation  
 Bei der Websynchronisierung für die Mergereplikation ist es erforderlich, dass die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Replikationsüberwachung (replisapi.dll) in das virtuelle Verzeichnis auf dem Server mit Internetinformationsdienste (Internet Information Services, IIS) kopiert wird, der für die Synchronisierung verwendet wird. Wenn Sie die Websynchronisierung konfigurieren, wird die Datei vom Assistenten zum Konfigurieren der Websynchronisierung in das virtuelle Verzeichnis kopiert. Wenn Sie die auf dem IIS-Server installierten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Komponenten aktualisieren, müssen Sie replisapi.dll manuell vom Verzeichnis COM in das virtuelle Verzeichnis auf dem IIS-Server kopieren. Weitere Informationen zur Konfiguration der Websynchronisierung finden Sie unter [Konfigurieren der Websynchronisierung](../../relational-databases/replication/configure-web-synchronization.md).  
  
## <a name="restoring-a-replicated-database-from-an-earlier-version"></a>Wiederherstellen einer replizierten Datenbank von einer früheren Version  
 Um sicherzustellen, dass die Replikationseinstellungen beibehalten werden, wenn die Sicherung einer replizierten Datenbank mithilfe einer früheren Version wiederhergestellt wird, stellen Sie die Sicherung auf einem Server und in einer Datenbank wieder her, deren Namen mit den Namen des Servers und der Datenbank übereinstimmen, von dem bzw. der die Sicherung erstellt wurde.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server-Replikation](../../relational-databases/replication/sql-server-replication.md)  
 [Häufig gestellte Fragen für Replikationsadministratoren](../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)   
 [Abwärtskompatibilität von Replikationen](../../relational-databases/replication/replication-backward-compatibility.md)   
 [Unterstützte Versions- und Editionsupgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)   
 [Aktualisieren von SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
 [Upgrading a Replication Topology to SQL Server 2016 (Upgrade der Replikationstopologie auf SQL Server 2016)](/archive/blogs/sql_server_team/upgrading-a-replication-topology-to-sql-server-2016)