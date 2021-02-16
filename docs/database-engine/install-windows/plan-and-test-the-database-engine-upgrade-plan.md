---
title: Planen und Testen des Upgradeplans für die Datenbank-Engine | Microsoft-Dokumentation
description: In diesem Artikel wird die Planung für ein SQL Server-Upgrade beschrieben, einschließlich einer Planungsprüfliste sowie das Entwickeln und Testen eines Upgradeplans.
ms.custom: ''
ms.date: 05/18/2018
ms.prod: sql
ms.technology: install
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 19c5b725-7400-4881-af8f-fd232ca28234
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7a6311f79aa87d93ee980cf4d0c8d00352b14687
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353050"
---
# <a name="plan-and-test-the-database-engine-upgrade-plan"></a>Planen und Testen des Upgradeplans für die Datenbank-Engine

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
 Für eine erfolgreiches Upgrade von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ist unabhängig von der Herangehensweise eine angemessene Planung erforderlich.  
  
## <a name="release-notes-and-known-upgrade-issues"></a>Anmerkungen zu dieser Version und bekannte Upgradeprobleme  
 Bevor Sie auf [!INCLUDE[ssDE](../../includes/ssde-md.md)] aktualisieren, sollten Sie sich folgende Artikel anschauen:

- [Versionsanmerkungen zu SQL Server 2019](../../sql-server/sql-server-version-15-release-notes.md)
- [Versionsanmerkungen zu SQL Server 2017](../../sql-server/sql-server-2017-release-notes.md) 
- [Versionsanmerkungen zu SQL Server 2016](../../sql-server/sql-server-2016-release-notes.md)
- Artikel [Abwärtskompatibilität der SQL Server-Datenbank-Engine](../discontinued-database-engine-functionality-in-sql-server.md)  
  
## <a name="pre-upgrade-planning-checklist"></a>Planungsprüfliste zur Vorbereitung des Upgrades  
 Lesen Sie vor dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Upgrade die folgende Prüfliste und die dazugehörigen Artikel. Der Inhalt dieser Artikel gilt unabhängig von der Upgrademethode für alle Upgrades und hilft Ihnen dabei, die am besten geeignete Upgrademethode zu bestimmen: Paralleles Upgrade, Upgrade durch Neuinstallation oder direktes Upgrade. So kann es beispielsweise geschehen, dass Sie kein direktes oder paralleles Upgrade durchführen können, wenn Sie das Betriebssystem, SQL Server 2005 oder eine 32-Bit-Version von SQL Server upgraden. Eine Entscheidungsstruktur finden Sie unter [Choose a Database Engine Upgrade Method](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md).  
  
-   **Hardware- und Softwareanforderungen:** Lesen Sie das Thema zu den Hardware- und Softwareanforderungen für die Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Diese Anforderungen finden Sie unter: [Hardware- und Softwareanforderungen für die Installation von SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). Es gehört zu jedem Upgradeplanungszyklus, ein Upgrade des Betriebssystems und der Hardware in Betracht zu ziehen, da neuere Hardware schneller ist und den Lizenzbedarf aufgrund der geringeren Anzahl von Prozessoren oder aufgrund der Datenbank- und Serverkonsolidierung reduzieren kann. Diese Art von Hardware- und Softwareänderungen wirken sich darauf aus, welche Upgrademethode Sie wählen.  
  
-   **Aktuelle Umgebung:** Überprüfen Sie die aktuelle Umgebung, um die verwendeten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Komponenten und die mit Ihrer Umgebung verbundenen Clients zu verstehen.  
  
    -   **Client-Anbieter:** Obwohl ein Upgrade kein gleichzeitiges Update des Anbieters für alle Ihre Clients erfordert, steht es Ihnen frei, dies zu tun. Wenn Sie ein Upgrade von [!INCLUDE[sql14](../../includes/sssql14-md.md)] oder älter durchführen, erfordern die folgenden [!INCLUDE[sql15](../../includes/sssql16-md.md)]-Features entweder einen aktualisierten Anbieter für jeden Client oder einen Anbieter, der zusätzliche Funktionen bereitstellt:  
  
       -   [Always Encrypted &#40;Datenbank-Engine&#41;](../../relational-databases/security/encryption/always-encrypted-database-engine.md)  
  
       -   [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
       -   [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)  
  
       -   TLS-Sicherheitsupdate  

   >[!NOTE]
   >Die vorherige Liste gilt auch für [!INCLUDE[sscurrent](../../includes/sscurrent-md.md)].
  
-   **Komponenten von Drittanbietern:** Ermitteln Sie die Kompatibilität von Drittanbieterkomponenten, z.B. für eine integrierte Sicherung.  
  
-   **Zielumgebung:** Stellen Sie sicher, dass Ihre Zielumgebung die Hardware- und Softwareanforderungen erfüllt und die Anforderungen des ursprünglichen Systems unterstützen kann. Ihr Upgrade kann z.B. die Konsolidierung mehrerer SQL Server-Instanzen in eine einzelne, neue [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Instanz bedeuten oder die Virtualisierung Ihrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Umgebung zu einer privaten oder öffentlichen Cloud.  
  
-   **Edition:** Ermitteln Sie die entsprechende Edition von [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] für das Upgrade, und bestimmen Sie die gültigen Upgradepfade für das Upgrade. Ausführliche Informationen finden Sie unter [Supported Version and Edition Upgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md). Bevor Sie eine Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf eine andere Edition aktualisieren, sollten Sie überprüfen, ob die derzeit verwendete Funktionalität in der Edition, die Ziel des Upgrades ist, unterstützt wird.  
  
    > [!NOTE]  
    >  Wenn Sie [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] von einer früheren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition aktualisieren, wählen Sie zwischen „Enterprise Edition: Core-basierte Lizenzierung“ und „Enterprise Edition“ aus. Diese Enterprise Editionen unterscheiden sich nur im Hinblick auf den Lizenzierungsmodus. Weitere Informationen finden Sie unter [Compute Capacity Limits by Edition of SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
  
-   **Abwärtskompatibilität:** Lesen Sie den Artikel zur Abwärtskompatibilität der [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]-Datenbank-Engine, um sich über Änderungen im Verhalten zwischen [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] und der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Version zu informieren, für die Sie das Upgrade durchführen. Siehe [SQL Server Database Engine Backward Compatibility](../discontinued-database-engine-functionality-in-sql-server.md).  
  
-   **Datenmigrations-Assistent:** Führen Sie den Datenmigrations-Assistent aus, um bei der Diagnose von Problemen zu helfen, die den Upgradeprozess blockieren oder aufgrund einer erheblichen Änderung auch Änderungen an vorhandenen Skripts oder Anwendungen erfordern.
    Sie können den Datenmigrations-Assistent [hier herunterladen](https://aka.ms/get-dma).  
  
-   **Systemkonfigurationsprüfung:** Führen Sie die [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]-Systemkonfigurationsprüfung (System Configuration Checker, SCC) aus, um zu ermitteln, ob das SQL Server-Setupprogramm Blockierungsprobleme entdeckt, bevor Sie das Upgrade planen. Weitere Informationen finden Sie unter [Check Parameters for the System Configuration Checker](../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md).  
  
-   **Aktualisieren von speicheroptimierten Tabellen:** Wenn Sie eine SQL Server 2014-Datenbankinstanz mit speicheroptimierten Tabellen auf SQL Server 2016 aktualisieren, benötigt der Upgradeprozess zusätzliche Zeit, um die speicheroptimierten Tabellen in das neue Format auf dem Datenträger zu konvertieren (die Datenbank ist währenddessen offline).   Die Zeitspanne hängt von der Größe der speicheroptimierten Tabellen und der Geschwindigkeit des E/A-Subsystems ab. Das Upgrade erfordert für direkte und neue Installationsupgrades drei Vorgänge hinsichtlich der Datengrößen (Schritt 1 ist für parallele Upgrades nicht notwendig, die Schritte 2 und 3 indes schon):  
  
    1.  Führen Sie die Datenbankwiederherstellung mithilfe des alten Datenträgerformats aus (dabei werden alle speicheroptimierten Tabellen vom Datenträger in den Speicher geladen).  
  
    2.  Serialisieren Sie die Daten auf dem Datenträger im neuen Format auf dem Datenträger  
  
    3.  Führen Sie die Datenbankwiederherstellung mithilfe des neuen Formats aus (dabei werden alle speicheroptimierten Tabellen vom Datenträger in den Speicher geladen).  
  
     Außerdem schlägt die Wiederherstellung fehl, wenn während dem Prozess nicht genügend Speicherplatz auf dem Datenträger verfügbar ist. Stellen Sie sicher, dass ausreichend Speicherplatz auf dem Datenträger verfügbar ist, um die vorhandene Datenbank und die Container in der Dateigruppe MEMORY_OPTIMIZED_DATA zu speichern, um ein direktes Upgrade auszuführen oder eine SQL Server 2014-Datenbank an eine SQL Server 2016-Instanz anzufügen. Verwenden Sie die folgende Abfrage, um den Speicherplatz zu ermitteln, der derzeit für die Dateigruppe MEMORY_OPTIMIZED_DATA erforderlich ist, und infolgedessen auch den Speicherplatz, der erforderlich ist, damit das Upgrade erfolgreich abgeschlossen wird:  
  
    ```  
    select cast(sum(size) as float)*8/1024/1024 'size in GB'   
    from sys.database_files  
    where data_space_id in (select data_space_id from sys.filegroups where type=N'FX')  
    ```  
  
## <a name="develop-and-test-the-upgrade-plan"></a>Entwickeln und Testen des Upgradeplans  
 Die beste Herangehensweise ist, das Upgrade wie jedes andere IT-Projekt zu behandeln. Organisieren Sie ein Upgrade-Team, das die für das Upgrade erforderlichen Qualifikationen für die Datenbankverwaltung, das Netzwerk sowie das Extrahieren, Transformieren und Laden (ETL) besitzt. Das Team muss:  
  
-   **Die Upgrademethode auswählen:** Weitere Informationen finden Sie unter [Auswählen einer Upgrademethode für die Datenbank-Engine](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md).  
  
-   **Einen Wiederherstellungsplan entwickeln:** Mit diesem Plan können Sie Ihre ursprüngliche Umgebung wiederherstellen, wenn Sie sie zurücksetzen müssen.  
  
-   **Akzeptanzkriterien bestimmen:** Überprüfen Sie, ob das Upgrade erfolgreich war, bevor Sie Benutzer auf die aktualisierte Umgebung umstellen.  
  
-   **Den Upgradeplan testen:** Verwenden Sie Microsoft SQL Server Distributed Replay Utility, um die Leistung mit der tatsächlichen Arbeitsauslastung zu testen. Dieses Hilfsprogramm kann Ablaufverfolgungsdaten mithilfe mehrerer Computer wiedergeben, indem es eine für die Unternehmung maßgebliche Arbeitsauslastung simuliert. Durch Ausführen einer Wiedergabe auf einem Testserver vor und nach einem SQL Server-Upgrade können Sie Leistungsunterschiede messen und nach Inkompatibilitäten der Anwendung suchen, die möglicherweise durch das Upgrade verursacht werden. Weitere Informationen finden Sie unter [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md) und [Befehlszeilenoptionen für das Verwaltungstool &#40;Distributed Replay Utility&#41;](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md).  
  
## <a name="next-steps"></a>Nächste Schritte  
[Aktualisieren der Datenbank-Engine](../../database-engine/install-windows/upgrade-database-engine.md) 
  
## <a name="additional-resources"></a>Zusätzliche Ressourcen 
[Leitfaden zur Datenbankmigration](https://aka.ms/datamigration)