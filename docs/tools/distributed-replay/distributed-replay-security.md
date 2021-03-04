---
title: Distributed Replay-Sicherheit
description: In diesem Artikel werden die Schritte zur Sicherheitskonfiguration für Distributed Replay von SQL Server und wichtige Überlegungen zum Schutz und Entfernen von Daten beschrieben.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 907fbe327e712c4e9a6aa989a33822baa6122a07
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836877"
---
# <a name="distributed-replay-security"></a>Distributed Replay-Sicherheit

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Bevor Sie das Distributed Replay-Feature von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installieren und verwenden, sollten Sie die wichtigen Sicherheitsinformationen in diesem Artikel lesen. In diesem Thema werden die nach der Installation auszuführenden Sicherheitskonfigurationsschritte beschrieben, die erforderlich sind, bevor Sie Distributed Replay verwenden können. Zudem werden in diesem Thema wichtige Überlegungen im Hinblick auf Datenschutz und wichtige Schritte zum Entfernen von Elementen beschrieben.  
  
## <a name="user-and-service-accounts"></a>Benutzer- und Dienstkonten  
 In der folgenden Tabelle werden die Konten beschrieben, die für Distributed Replay verwendet werden. Nach der Installation von Distributed Replay müssen Sie die Sicherheitsprinzipale zuweisen, unter denen der Controller und der Clientdienst ausgeführt werden. Daher empfiehlt es sich, dass Sie die entsprechenden Domänenbenutzerkonten konfigurieren, bevor Sie die Distributed Replay-Funktionen installieren.  
  
|Benutzerkonto|Requirements (Anforderungen)|  
|------------------|------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Dienstkonto für Distributed Replay Controller|Kann ein Domänenbenutzerkonto oder ein lokales Benutzerkonto sein. Wenn Sie ein lokales Benutzerkonto verwenden, müssen das Verwaltungstool, der Controller und der Client auf demselben Computer ausgeführt werden.<br /><br /> **\*\* Sicherheitshinweis \*\*** Dieses Konto sollte nicht Mitglied der Windows-Gruppe „Administratoren“ sein.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Dienstkonto für Distributed Replay Client|Kann ein Domänenbenutzerkonto oder ein lokales Benutzerkonto sein. Wenn Sie ein lokales Benutzerkonto verwenden, müssen der Controller, der Client und der Ziel-SQL-Server auf demselben Computer ausgeführt werden.<br /><br /> **\*\* Sicherheitshinweis \*\*** Dieses Konto sollte nicht Mitglied der Windows-Gruppe „Administratoren“ sein.|  
|Interaktives Benutzerkonto, das verwendet wird, um das Distributed Replay-Verwaltungstool auszuführen|Kann entweder ein lokales Benutzerkonto oder ein Domänenbenutzerkonto sein. Um ein lokales Benutzerkonto zu verwenden, müssen das Verwaltungstool und der Controller auf demselben Computer ausgeführt werden.|  
  
 **Wichtig**: Wenn Sie den Distributed Replay-Controller konfigurieren, können Sie mindestens ein Benutzerkonto angeben, das zum Ausführen der Distributed Replay-Clientdienste verwendet wird. Die folgenden Kontotypen werden unterstützt:  
  
-   Domänenbenutzerkonto  
  
-   Vom Benutzer erstelltes lokales Benutzerkonto  
  
-   Administrator  
  
-   Virtuelles Konto und verwaltetes Dienstkonto (Managed Service Account, MSA)  
  
-   Netzwerkdienste, lokale Dienste und System  
  
 Gruppenkonten (lokales oder Domänenbenutzerkonto) und andere integrierte Konten (wie "Jeder") werden nicht akzeptiert.  
  
 Nachdem Sie Distributed Replay installiert haben, können Sie das Windows-Tool Dienste zum Festlegen der Dienstkonten oder der zugehörigen Kennwörter verwenden. Um die Dienstkonten zu ändern, die dem Distributed Replay-Controller oder den -Clientdiensten zugeordnet sind, führen Sie folgende Schritte aus:  
  
1.  Führen Sie dazu je nach Betriebssystem eine der folgenden Aktionen aus:  
  
    -   Klicken Sie auf **Start**, geben Sie **services.msc** in das Feld **Suchen** ein, und drücken Sie dann die EINGABETASTE.  
  
    -   Klicken Sie auf **Start**, dann auf **Ausführen**, geben Sie **services.msc** ein, und drücken Sie dann die EINGABETASTE.  
  
2.  Klicken Sie im Dialogfeld **Services** mit der rechten Maustaste auf den Dienst, den Sie konfigurieren möchten, und klicken Sie dann auf **Properties**.  
  
3.  Klicken Sie auf der Registerkarte **Anmelden** auf **Dieses Konto**.  
  
4.  Konfigurieren Sie das Benutzerkonto, das Sie verwenden möchten.  
  
## <a name="file-and-folder-permissions"></a>Datei- und Ordnerberechtigungen  
 Nachdem die Dienstkonten angegeben wurden, müssen Sie diesen Dienstkonten die notwendige Datei- und Ordnerberechtigungen gewähren. Konfigurieren Sie Datei- und Ordnerberechtigungen entsprechend der folgenden Tabelle:  
  
|Konto|Ordnerberechtigungen|  
|-------------|------------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Dienstkonto für Distributed Replay Controller|`<Controller_Installation_Path>\DReplayController` (Lesen, Schreiben, Löschen)<br /><br /> `DReplayServer.xml` Datei (Lesen, Schreiben)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Dienstkonto für Distributed Replay Client|`<Client_Installation_Path>\DReplayClient` (Lesen, Schreiben, Löschen)<br /><br /> `DReplayClient.xml` Datei (Lesen, Schreiben)<br /><br /> Die Arbeits- und Ergebnisverzeichnisse, die in der Clientkonfigurationsdatei durch die Elemente `WorkingDirectory` und `ResultDirectory` angegeben werden. (Lesen, Schreiben)|  
  
## <a name="dcom-permissions"></a>DCOM-Berechtigungen  
 DCOM wird für die RPC-Kommunikation (Remote Procedure Call) zwischen dem Controller und dem Verwaltungstool und zwischen dem Controller und allen Clients verwendet. Sie müssen computerweite und anwendungsspezifische DCOM-Berechtigungen für den Controller konfigurieren, nachdem die Distributed Replay-Funktionen installiert wurden.  
  
 Führen Sie folgende Schritte aus, um die DCOM-Berechtigungen für den Controller zu konfigurieren:  
  
1.  **Öffnen Sie das Snap-In der Komponentendienste, dcomcnfg.exe:** Dabei handelt es sich um das Tool, das für die Konfiguration der DCOM-Berechtigungen verwendet wird.  
  
    1.  Klicken Sie auf dem Controllercomputer auf **Start**.  
  
    2.  Geben Sie **dcomcnfg.exe** in das Feld **Suchen** ein.  
  
    3.  Drücken Sie die EINGABETASTE.  
  
2.  **Konfigurieren Sie computerweite DCOM-Berechtigungen:** Gewähren Sie jedem in der folgenden Tabelle aufgeführten Konto die entsprechenden computerweiten DCOM-Berechtigungen. Weitere Informationen zum Festlegen von computerweiten Berechtigungen finden Sie in [Prüfliste: Verwalten von DCOM-Anwendungen](/windows/win32/com/setting-machine-wide-security-using-dcomcnfg).  
  
3.  **Konfigurieren Sie anwendungsspezifische DCOM-Berechtigungen:** Gewähren Sie jedem in der folgenden Tabelle aufgeführten Konto die entsprechenden anwendungsspezifischen DCOM-Berechtigungen. Der DCOM-Anwendungsname für den Controllerdienst ist **DReplayController**. Weitere Informationen zum Festlegen von anwendungsspezifischen Berechtigungen finden Sie in [Prüfliste: Verwalten von DCOM-Anwendungen](/windows/win32/com/setting-processwide-security-using-dcomcnfg).  
  
 In der folgenden Tabelle wird beschrieben, welche DCOM-Berechtigungen für das interaktive Benutzerkonto für das Verwaltungstool und die Clientdienstkonten erforderlich sind:  
  
|Funktion|Konto|Erfordert DCOM-Berechtigungen für den Controller|  
|-------------|-------------|---------------------------------------------|  
|Distributed Replay–Verwaltungstool|Das interaktive Benutzerkonto|Lokaler Zugriff<br /><br /> Remotezugriff<br /><br /> Lokaler Start<br /><br /> Remotestart<br /><br /> Lokale Aktivierung<br /><br /> Remoteaktivierung|  
|Distributed Replay Client|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Dienstkonto für Distributed Replay Client|Lokaler Zugriff<br /><br /> Remotezugriff<br /><br /> Lokaler Start<br /><br /> Remotestart<br /><br /> Lokale Aktivierung<br /><br /> Remoteaktivierung|  
  
> [!IMPORTANT]  
>  Stellen Sie zum Schutz vor böswilligen Abfragen oder Denial-of-Service-Angriffen sicher, dass nur ein vertrauenswürdiges Benutzerkonto für das Clientdienstkonto verwendet wird. Dieses Konto wird in der Lage sein, eine Verbindung mit der Zielinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]herzustellen und Arbeitsauslastungen für diese Instanz zu simulieren.  
  
## <a name="sql-server-permissions"></a>SQL Server-Berechtigungen  
 Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Clientdienstkonten werden verwendet, um eine Verbindung mit der Zielinstanz der Arbeitsauslastung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]herzustellen. Für diese Verbindungen wird nur die Windows-Authentifizierung unterstützt.  
  
 Nachdem Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay-Clientdienst auf einer Gruppe von Computern installiert haben, muss dem für diese Dienstkonten verwendeten Sicherheitsprinzipal die „sysadmin“-Serverrolle auf der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gewährt werden, für die Sie die Arbeitsauslastung der Ablaufverfolgung wiedergeben möchten. Dieser Schritt wird während des Setups von Distributed Replay nicht automatisch ausgeführt.  
  
## <a name="data-protection"></a>Datenschutz  
 In der Distributed Replay-Umgebung wird den folgenden Benutzerkonten Vollzugriff auf die Zielserverinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], die Eingabedaten der Ablaufverfolgung und die Ergebnisdateien der Ablaufverfolgung gewährt:  
  
-   Interaktives Benutzerkonto, das zum Ausführen des Verwaltungstools verwendet wird.  
  
-   Controllerdienstkonto.  
  
-   Clientdienstkonto.  
  
-   Mitglieder der lokalen Administratorgruppe auf dem Controller.  
  
-   Mitglieder der lokalen Administratorgruppe auf den Clients.  
  
    > [!IMPORTANT]  
    >  Diese Konten enthalten Vollzugriff auf alle persönlich identifizierbare Informationen (PII) oder vertraulichen Informationen in den Datendateien der Ablaufverfolgung, Zwischendateien, Dispatchdateien oder [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datendateien, die von Distributed Replay verwendet wurden.  
  
 Es wird empfohlen, die folgenden Sicherheitsvorsichtsmaßnahmen zu treffen:  
  
-   Speichern Sie die Eingabedaten der Ablaufverfolgung, die Ergebnisdateien der Ablaufverfolgung und Datenbankdateien an einem Speicherort, der das NTFS-Dateisystem (NTFS) verwendet, und wenden Sie die geeigneten Zugriffssteuerungslisten (ACLs) an. Wenn erforderlich, verschlüsseln Sie die Daten, die auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Computer gespeichert werden. Beachten Sie, dass keine ACLs auf Ablaufverfolgungsdateien angewendet werden und keine Datenmaskierung oder Verbergung erfolgt. Sie sollten diese Dateien möglichst bald nach ihrer Verwendung löschen.  
  
-   Wenden Sie die entsprechenden ACLs und Beibehaltungsrichtlinie auf alle Zwischendateien und Dispatchdateien an, die von Distributed Replay generiert werden.  
  
-   Verwenden Sie Transport Layer Security (TLS), früher als Secure Sockets Layer (SSL) bezeichnet, um den Datentransport im Netzwerk zu schützen.  
  
## <a name="important-removal-steps"></a>Wichtige Schritte zum Entfernen von Elementen  
 Es empfiehlt sich, Distributed Replay nur in einer Testumgebung zu verwenden. Führen Sie unbedingt die folgenden Aktionen aus, nachdem Sie die Tests abgeschlossen haben und bevor Sie die betreffenden Computer für eine andere Aufgabe bereitstellen:  
  
-   Deinstallieren Sie die Distributed Replay-Funktionen, und entfernen Sie die zugehörigen Konfigurationsdateien vom Controller und von allen Clients.  
  
-   Löschen Sie alle Ablaufverfolgungsdateien, Zwischendateien, Dispatchdateien und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbankdateien, die für Tests verwendet wurden. Die Zwischendateien und Dispatchdateien werden im Arbeitsverzeichnis auf dem Controller bzw. dem Client gespeichert.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Installieren von Distributed Replay – Übersicht](../../tools/distributed-replay/install-distributed-replay-overview.md)  
  
