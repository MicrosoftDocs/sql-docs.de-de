---
title: Versionsanmerkungen zu SQL Server 2012 | Microsoft-Dokumentation
description: In diesem Dokument mit Versionshinweisen werden bekannte Probleme beschrieben, mit denen Sie sich vertraut machen sollten, bevor Sie Microsoft SQL Server 2012 installieren bzw. mit der Problembehandlung beginnen.
ms.prod: sql
ms.technology: release-landing
ms.custom: ''
ms.date: 07/22/2020
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Release Notes, SQL Server
ms.assetid: 9ccb390a-67a9-4593-85ea-2b4c41c4620f
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: e75c713919a53a06c7b54c31f910810f055fac9e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336719"
---
# <a name="sql-server-2012-release-notes"></a>Versionsanmerkungen zu SQL Server 2012
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]
In diesem Dokument mit Versionshinweisen werden bekannte Probleme beschrieben, mit denen Sie sich vertraut machen sollten, bevor Sie [Microsoft SQL Server 2012](https://go.microsoft.com/fwlink/?LinkId=238647) installieren bzw. mit der Problembehandlung beginnen. Die Anmerkungen zu dieser Version sind nur online und nicht auf den Installationsmedien verfügbar und werden regelmäßig aktualisiert.  
  
Informationen zu den ersten Schritten sowie zur Installation von SQL Server 2012 erhalten Sie in der SQL Server 2012-Infodatei. Die Infodatei steht auf den Installationsmedien und auf der [Infodatei](https://download.microsoft.com/download/3/B/D/3BD9DD65-D3E3-43C3-BB50-0ED850A82AD5/ENU/Readme.htm) -Downloadseite zur Verfügung. Weitere Informationen finden Sie auch in der [SQL Server-Onlinedokumentation](/previous-versions/sql/sql-server-2012/ms130214(v=sql.110)) und in den [SQL Server-Foren](https://go.microsoft.com/fwlink/?LinkId=213599).  
  
## <a name="10-before-you-install"></a><a name="Install"></a>1.0 Vor der Installation  
Vor der Installation von [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]sollten folgende Informationen beachtet werden.  
  
### <a name="11-rules-documentation-for-sql-server-2012-setup"></a>1.1 Regeldokumentation für SQL Server 2012-Setup  
**Problem:** SQL Server-Setup überprüft die Computerkonfiguration, bevor der Setupvorgang abgeschlossen wird. Die verschiedenen Regeln, die währen des SQL Server-Setupvorgangs ausgeführt werden, werden in einem Bericht der Systemkonfigurationsprüfung erfasst. Die Dokumentation zu diesen Setupregeln steht nicht länger in der MSDN Library zur Verfügung.  
  
**Problemumgehung:** Der Bericht der Systemkonfigurationsprüfung enthält weitere Einzelheiten zu den Setupregeln. Die Systemkonfigurationsprüfung generiert einen Bericht, der eine kurze Beschreibung für jede ausgeführte Regel sowie den Ausführungsstatus enthält. Der Bericht der Systemkonfigurationsprüfung befindet sich unter %programfiles%\Microsoft SQL Server\110\Setup Bootstrap\Log\\<JJJJMMTT_hhmm>\\.  
  
### <a name="12-adding-a-local-user-account-for-the-distributed-replay-controller-service-might-terminate-setup-unexpectedly"></a>1.2 Das Hinzufügen eines lokalen Benutzerkontos für den Distributed Replay Controller-Dienst kann dazu führen, dass Setup unerwartet beendet wird  
**Problem:** Beim Versuch, auf der Seite **Distributed Replay Controller** von SQL Server-Setup ein lokales Benutzerkonto für den Distributed Replay Controller-Dienst hinzuzufügen, wird das Setup mit der Fehlermeldung „SQL Server-Setupfehler“ unerwartet beendet.  
  
**Problemumgehung:** Fügen Sie während des SQL-Setups keine lokalen Benutzerkonten mit „Aktuellen Benutzer hinzufügen“ oder „Hinzufügen...“ hinzu. Fügen Sie ein lokales Benutzerkonto mithilfe folgender Schritte manuell hinzu, nachdem Setup ausgeführt wurde:  
  
1.  Beenden Sie den SQL Server Distributed Replay Controller-Dienst.  
  
2.  Geben Sie auf dem Controllercomputer, auf dem der Controllerdienst installiert ist, an der Eingabeaufforderung dcomcnfg ein.  
  
3.  Navigieren Sie im Fenster Komponentendienste zu **Konsolenstamm** -> **Komponentendienste** -> **Computer** -> **Arbeitsplatz** -> **Dconfig** ->**DReplayController**.  
  
4.  Klicken Sie mit der rechten Maustaste auf **DReplayController**, und klicken Sie anschließend auf **Eigenschaften**.  
  
5.  Klicken Sie im Fenster **Eigenschaften von DReplayController** auf der Registerkarte **Sicherheit** im Abschnitt **Start- und Aktivierungsberechtigungen** auf **Bearbeiten** .  
  
6.  Erteilen Sie dem lokalen Benutzerkonto die Berechtigungen **Lokale Aktivierung und Remoteaktivierung** , und klicken Sie dann auf **OK**.  
  
7.  Klicken Sie im Abschnitt „Zugriffsberechtigungen“ auf **Bearbeiten** , und erteilen Sie dem lokalen Benutzerkonto die Berechtigungen **Lokaler Zugriff und Remotezugriff** . Klicken Sie dann auf **OK**.  
  
8.  Klicken Sie auf **OK** , um das Fenster **Eigenschaften von DReplayController** zu schließen.  
  
9. Fügen Sie auf dem Controllercomputer das lokale Benutzerkonto zur Gruppe **Distributed COM-Benutzer** hinzu.  
  
10. Starten Sie den SQL Server Distributed Replay Controller-Dienst.  
  
### <a name="13-sql-server-setup-might-fail-while-trying-to-start-the-sql-server-browser-service"></a>1.3 Beim Start des SQL Server-Browserdiensts können in SQL Server-Setup Fehler auftreten  
**Problem:** Beim Versuch, den SQL Server-Browserdienst zu starten, können in SQL Server-Setup Fehler mit etwa folgendem Wortlaut auftreten:  
  
```
The following error has occurred:  
Service 'SQLBrowser' start request failed. Click 'Retry' to retry the failed action, or click 'Cancel' to cancel this action and continue setup.
```
  
oder  
  
```
The following error has occurred:  
SQL Server Browser configuration for feature 'SQL_Browser_Redist_SqlBrowser_Cpu32' was cancelled by user after a previous installation failure. The last attempted step: Starting the SQL Server Browser service 'SQLBrowser', and waiting for up to '900' seconds for the process to complete.
```
  
**Problemumgehung:** Dieser Fehler kann auftreten, wenn SQL Server Engine oder Analysis Services nicht installiert werden können. Informieren Sie sich in den Setupprotokollen von SQL Server, um Probleme mit SQL Server Engine und Analysis Services zu behandeln und den Fehler zu beheben. Weitere Informationen finden Sie unter "Lesen und Anzeigen der Setupprotokolldateien von SQL Server". Weitere Informationen finden Sie unter [View and Read SQL Server Setup Log Files](../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
### <a name="14-sql-server-2008-2008-r2-analysis-services-failover-cluster-upgrade-to-sql-server-2012-might-fail-after-renaming-the-network-name"></a>1.4 Ein Failoverclusterupgrade von SQL Server 2008 oder 2008 R2 Analysis Services auf SQL Server 2012 verursacht nach der Umbenennung des Netzwerks u. U. einen Fehler  
**Problem:** Nachdem der Netzwerkname einer Microsoft SQL Server 2008- oder 2008 R2 Analysis Services-Failoverclusterinstanz mit dem Windows-Clusterverwaltungstool umbenannt wurde, verursacht der Upgradevorgang ggf. einen Fehler.  
  
**Problemumgehung:** Aktualisieren Sie den ClusterName-Registrierungseintrag mithilfe der Anweisungen im Abschnitt „Resolution“ (Problemlösung) [dieses Knowledge Base-Artikels](https://support.microsoft.com/kb/955784), um das Problem zu lösen.  
  
### <a name="15-installing-sql-server-2012-on-windows-server-2008-r2-server-core-service-pack-1"></a>1.5 Installation von SQL Server 2012 unter Windows Server 2008 R2 Server Core Service Pack 1  
Bei der Installation von SQL Server unter Windows Server 2008 R2 Server Core SP1 sind folgende Einschränkungen zu beachten:  
  
-   Microsoft SQL Server 2012 unterstützt kein Setup mit dem Installations-Assistenten unter dem Server Core-Betriebssystem. Beim Installieren unter Server Core unterstützt SQL Server-Setup mithilfe des /Q-Parameters den vollständigen stillen Modus oder mithilfe des /QS-Parameters den einfachen stillen Modus.  
  
-   Das Upgrade einer früheren Version von SQL Server auf Microsoft SQL Server 2012 wird auf Computern mit Windows Server 2008 R2 Server Core SP1 nicht unterstützt.  
  
-   Das Installieren einer 32-Bit-Version der Microsoft SQL Server 2012 Edition wird auf einem Computer mit Windows Server 2008 R2 Server Core SP1 nicht unterstützt.  
  
-   Microsoft SQL Server 2012 kann nicht parallel mit früheren SQL Server-Versionen auf einem Computer installiert werden, auf dem Windows Server 2008 R2 Server Core SP1 ausgeführt wird.  
  
-   Unter dem Server Core-Betriebssystem werden nicht alle Funktionen von SQL Server 2012 unterstützt. Weitere Informationen zu den unterstützten Funktionen sowie zur Installation von SQL Server 2012 unter Server Core finden Sie unter [Installieren von SQL Server 2012 unter Server Core](/previous-versions/sql/sql-server-2012/hh231669(v=sql.110)).  
  
### <a name="16-semantic-search-requires-you-to-install-an-additional-dependency"></a>1.6 Die semantische Suche erfordert die Installation einer zusätzlichen Abhängigkeit  
**Problem:** Die statistische semantische Suche verfügt über eine zusätzliche erforderliche Komponente, die Semantic Language Statistics-Datenbank, die vom SQL Server-Setupprogramm nicht installiert wird.  
  
**Problemumgehung:** Führen Sie zum Einrichten der Semantic Language Statistics-Datenbank als erforderliche Komponente für die semantische Indizierung folgende Schritte aus:  
  
1.  Suchen Sie das Windows Installer-Paket SemanticLanguageDatabase.msi auf den SQL Server-Installationsmedien, und führen Sie es aus, um die Datenbank zu extrahieren. Laden Sie die Semantic Language Statistics-Datenbank für SQL Server 2012 Express aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52681) (https://www.microsoft.com/download/details.aspx?id=52681) ) herunter, und führen Sie dann das Windows Installer-Paket aus.  
  
2.  Verschieben Sie die Datenbank in einen geeigneten Datenordner. Falls Sie die Datenbank im Standardverzeichnis belassen, müssen die Berechtigungen geändert werden, bevor sie erfolgreich angefügt werden kann.  
  
3.  Fügen Sie die extrahierte Datenbank an.  
  
4.  Registrieren Sie die Datenbank, indem Sie die gespeicherte Prozedur **sp_fulltext_semantic_register_language_statistics_db** aufrufen und den Namen angeben, den die Datenbank beim Anfügen erhalten hat.  
  
Falls diese Aufgaben nicht ausgeführt werden, wird beim Erstellen eines semantischen Indexes die folgende Fehlermeldung angezeigt:  
  
```
Msg 41209, Level 16, State 3, Line 1  
A semantic language statistics database is not registered. Full-text indexes using 'STATISTICAL_SEMANTICS' cannot be created or populated.
```
  
### <a name="17-installation-prerequisite-handling-during-sql-server-2012-setup"></a>1.7 Behandeln von erforderlichen Installationskomponenten beim SQL Server 2012-Setup  
Nachfolgend wird das Verhalten der erforderlichen Installationskomponenten beim SQL Server 2012-Setup beschrieben:  
  
-   Die Installation von SQL Server 2012 wird nur unter Windows 7 SP1 oder Windows Server 2008 R2 SP1 unterstützt. Das Setupprogramm verhindert eine Installation von SQL Server 2012 unter Windows 7 oder Windows Server 2008 R2 jedoch nicht.  
  
-   .NET Framework 3.5 SP1 ist für SQL Server 2012 erforderlich, wenn Sie Datenbank-Engine, Replikation, Master Data Services, Reporting Services, Data Quality Services (DQS) oder SQL Server Management Studio auswählen. Das Framework wird vom SQL Server-Setup nicht mehr installiert.  
  
    -   Wenn Sie Setup auf einem Computer mit dem Betriebssystem Windows Vista SP2 oder Windows Server 2008 SP2 ausführen und .NET Framework 3.5 SP1 nicht installiert ist, fordert SQL Server-Setup Sie auf, .NET Framework 3.5 SP1 herunterzuladen und zu installieren, bevor Sie die SQL Server-Installation fortsetzen können. Sie können .NET Framework 3.5 SP1 über Windows Update oder direkt [hier](https://www.microsoft.com/download/details.aspx?id=25150)herunterladen. Um eine Unterbrechung beim SQL Server-Setup zu vermeiden, sollten Sie .NET Framework 3.5 SP1 herunterladen und installieren, bevor Sie SQL Server-Setup ausführen.  
  
    -   Wenn Sie das Setup auf einem Computer mit dem Betriebssystem Windows 7 SP1 oder Windows Server 2008 R2 SP1 ausführen, müssen Sie .NET Framework 3.5 SP1 aktivieren, bevor Sie SQL Server 2012 installieren.  
  
        **Aktivieren Sie .NET Framework 3.5 SP1 unter Windows Server 2008 R2 SP1 mit einer der folgenden Methoden:**  
  
        Methode 1: Verwenden von Server-Manager  
  
        1.  Klicken Sie im Server-Manager auf **Funktionen hinzufügen** , um eine Liste verfügbarer Funktionen anzuzeigen.  
  
        2.  Erweitern Sie auf der Oberfläche **Funktionen auswählen** den Eintrag **.NET Framework 3.5.1-Funktionen** .  
  
        3.  Nachdem Sie **.NET Framework 3.5.1-Funktionen** erweitert haben, werden zwei Kontrollkästchen angezeigt. Ein Kontrollkästchen bezieht sich auf .NET Framework 3.5.1 und ein weiteres auf die WCF-Aktivierung. Wählen Sie **.NET Framework 3.5.1** aus, und klicken Sie anschließend auf **Weiter**. Die .NET Framework 3.5.1-Funktionen können nur zusammen mit den erforderlichen Rollendiensten und -funktionen installiert werden.  
  
        4.  Überprüfen Sie unter **Installationsauswahl bestätigen** Ihre Auswahl, und klicken Sie auf „Installieren“.  
  
        5.  Warten Sie, bis die Installation abgeschlossen ist, und klicken Sie dann auf **Schließen**.  
  
        Methode 2: Verwenden von Windows PowerShell  
  
        1.  Klicken Sie auf **Start** | **Alle Programme** | **Zubehör**.  
  
        2.  Erweitern Sie **Windows PowerShell**, klicken Sie mit der rechten Maustaste auf **Windows PowerShell**, und klicken Sie dann auf **Als Administrator ausführen**. Klicken Sie im Feld **Benutzerkontensteuerung** auf **Ja** .  
  
        3.  Geben Sie an der PowerShell-Eingabeaufforderung die folgenden Befehle ein, und drücken Sie nach jedem Befehl EINGABE:  
  
            ```  
            Import-Module ServerManager  
            Add-WindowsFeature as-net-framework  
            ```  
  
        **Aktivieren Sie .NET Framework 3.5 SP1 unter Windows 7 SP1 mit der folgenden Methode:**  
  
        1.  Klicken Sie auf **Start** | **Systemsteuerung** | **Programme** und dann auf **Windows-Funktionen aktivieren oder deaktivieren**. Wenn Sie zum Eingeben eines Administratorkennworts oder zu einer Bestätigung aufgefordert werden, geben Sie das Kennwort ein, oder nehmen Sie die Bestätigung vor.  
  
        2.  Aktivieren Sie das Kontrollkästchen neben der Funktion, um **Microsoft .NET Framework 3.5.1** zu aktivieren. Um eine Windows-Funktion zu deaktivieren, deaktivieren Sie das Kontrollkästchen.  
  
        3.  Klicken Sie auf **OK**.  
  
        **Aktivieren Sie .NET Framework 3.5 SP1 mit der Imageverwaltung für die Bereitstellung (DISM.exe):**  
  
        Sie können .NET Framework 3.5 SP1 auch mithilfe der Abbildverwaltung für die Bereitstellung (DISM.exe) aktivieren. Weitere Informationen zur Onlineaktivierung von Windows-Funktionen finden Sie unter [Aktivieren oder Deaktivieren von Windows-Funktionen im Onlinemodus](/previous-versions/windows/it-pro/windows-7/dd744582(v=ws.10)). Im Folgenden sind die Anweisungen zur Aktivierung von .NET Framework 3.5 SP1 aufgeführt:  
  
        1.  Geben Sie an der Eingabeaufforderung den folgenden Befehl ein, um alle im Betriebssystem verfügbaren Funktionen aufzulisten.  
  
            ```  
            sm /online /Get-Features  
            ```  
  
        2.  Optional: Geben Sie an der Eingabeaufforderung den folgenden Befehl ein, um Informationen zu einer bestimmten Funktion aufzulisten, die für Sie von Interesse ist.  
  
            ```  
            Dism /online /Get-FeatureInfo /FeatureName:NetFx3  
            ```  
  
        3.  Geben Sie den folgenden Befehl ein, um Microsoft .NET Framework 3.5.1 zu aktivieren.  
  
            ```  
            Dism /online /Enable-Feature /FeatureName:NetFx3  
            ```  
  
-   .NET Framework 4 ist eine Voraussetzung für SQL Server 2012. Das SQL Server-Setup installiert .NET Framework 4 während der Funktionsinstallation.  
  
    .NET Framework 4 wird von SQL Server 2012 Express nicht installiert, wenn die Installation unter dem Betriebssystem Windows Server 2008 R2 SP1 Server Core erfolgt. .NET Framework 4 ist bei der Installation von SQL Server 2012 Express (nur Datenbank) nicht erforderlich, wenn .NET Framework 3.5 SP1 vorhanden ist. Falls .NET Framework 3.5 SP1 nicht vorhanden ist bzw. wenn Sie SQL Server 2012 Management Studio Express, SQL Server 2012 Express mit Tools oder SQL Server 2012 Express mit Advanced Services installieren, müssen Sie .NET Framework 4 vor der Installation von SQL Server2012 Express unter einem Windows Server 2008 R2 SP1 Server Core-Betriebssystem installieren.  
  
-   Damit die Visual Studio-Komponente ordnungsgemäß installiert werden kann, erfordert SQL Server die Installation eines Updates. SQL Server-Setup überprüft, ob dieses Update vorhanden ist, und fordert Sie dann zum Herunterzuladen und Installieren des Updates auf, bevor Sie mit der SQL Server-Installation fortfahren können. Um eine Unterbrechung beim SQL Server-Setup zu vermeiden, können Sie das Update herunterladen und installieren, bevor Sie das SQL Server-Setup ausführen (siehe nachfolgende Ausführungen), oder Sie installieren alle auf Windows Update verfügbaren Updates für .NET Framework 3.5 SP1:  
  
    -   Wenn Sie SQL Server 2012 auf einem Computer mit dem Betriebssystem Windows Vista SP2 oder Windows Server 2008 SP2 installieren, können Sie das erforderliche Update [hier](https://support.microsoft.com/?kbid=956250)abrufen.  
  
    -   Wenn Sie SQL Server 2012 auf einem Computer mit dem Betriebssystem Windows 7 SP1 oder Windows Server 2008 R2 SP1 installieren, ist dieses Update bereits auf dem Computer installiert.  
  
-   Windows PowerShell 2.0 ist für die Installation der SQL Server 2012-Datenbank-Engine-Komponenten und von SQL Server Management Studio erforderlich. Windows PowerShell wird jedoch nicht mehr vom SQL Server-Setup installiert. Wenn PowerShell 2.0 nicht auf dem Computer vorhanden ist, folgen Sie den Anweisungen auf der Seite [Windows Management Framework](https://mskb.pkisolutions.com/kb/968929) , um die Komponente zu aktivieren. Windows PowerShell 2.0 wird abhängig vom verwendeten Betriebssystem auf unterschiedliche Weise zur Verfügung gestellt:  
  
    -   Windows Server 2008 – Windows PowerShell 1.0 ist eine Funktion, die hinzugefügt werden kann. Windows PowerShell 2.0-Versionen werden (in Form eines Betriebssystempatches) heruntergeladen und installiert.  
  
    -   Windows 7/Windows Server 2008 R2 – Windows PowerShell 2.0 wird standardmäßig installiert.  
  
-   Falls Sie beabsichtigen, SQL Server 2012-Funktionen in einer SharePoint-Umgebung zu verwenden, sind SharePoint Server 2010 Service Pack 1 (SP1) und das kumulative Update für SharePoint von August erforderlich. Sie müssen SP1 und das [kumulative Update von August](https://blogs.technet.com/b/stefan_gossner/archive/2010/09/02/august-2010-cumulative-update-for-sharepoint-has-been-released.aspx)für SharePoint, installieren und die Serverfarm vollständig patchen, bevor Sie der Farm SQL Server 2012-Funktionen hinzufügen. Diese Anforderung bezieht sich auf die folgenden SQL Server 2012-Funktionen: Verwenden einer Instanz der Datenbank-Engine als Datenbankserver der Farm, Konfigurieren von PowerPivot für SharePoint oder Bereitstellen von Reporting Services im SharePoint-Modus.  
  
### <a name="18-supported-operating-systems-for-sql-server-2012"></a>1.8 Unterstützte Betriebssysteme für SQL Server 2012  
SQL Server 2012 wird unter den Betriebssystemen Windows Vista SP2, Windows Server 2008 SP2, Windows 2008 R2 SP1 und Windows 7 SP1 unterstützt.  
  
### <a name="19-sync-framework-is-not-included-in-the-installation-package"></a>1.9 Sync Framework ist nicht im Installationspaket enthalten  
**Problem:** Sync Framework ist nicht im SQL Server 2012-Installationspaket enthalten.  
  
**Problemumgehung:** Laden Sie die geeignete Sync Framework-Version von [dieser Microsoft Download Center-Seite](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=23217) herunter.  
  
### <a name="110-if-visual-studio-2010-service-pack-1-is-uninstalled-the-sql-server-2012-instance-must-be-repaired-to-restore-certain-components"></a>1.10 Nachdem Visual Studio 2010 Service Pack 1 deinstalliert wurde, muss die SQL Server 2012-Instanz repariert werden, um bestimmte Komponenten wiederherzustellen  
**Problem:** Die [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]-Installation ist von einigen Komponenten aus Visual Studio 2010 Service Pack 1 abhängig. Wenn Sie Service Pack 1 deinstallieren, wird für einige freigegebene Komponenten ein Downgrade auf die ursprüngliche Version ausgeführt, während einige andere Komponenten vollständig vom Computer entfernt werden.  
  
**Problemumgehung:** Reparieren Sie die [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]-Instanz von den Originalmedien oder von einem Netzwerkinstallationspfad.  
  
1.  Starten Sie das SQL Server-Setupprogramm (setup.exe) vom SQL Server-Installationsmedium.  
  
2.  Nachdem die erforderlichen Komponenten installiert wurden und die Systemüberprüfung durchgeführt wurde, zeigt das Setupprogramm die Seite **SQL Server-Installationscenter** an.  
  
3.  Klicken Sie im linken Navigationsbereich auf **Wartung** , und klicken Sie dann auf **Reparieren** , um den Reparaturvorgang zu starten. Wenn das Installationscenter über das **Startmenü** gestartet wurde, müssen Sie zu dieser Zeit den Speicherort der Installationsmedien angeben.  
  
4.  Es werden Unterstützungsregeln für Setup und Dateiroutinen ausgeführt, um sicherzustellen, dass die erforderlichen Komponenten auf dem System installiert sind und dass der Computer den Setupüberprüfungsregeln entspricht. Klicken Sie zum Fortsetzen auf **OK** oder auf **Installieren** .  
  
5.  Wählen Sie auf der Seite **Instanz auswählen** die zu reparierende Instanz aus, und klicken Sie dann zum Fortsetzen auf **Weiter** .  
  
6.  Die Reparaturregeln werden ausgeführt, um den Vorgang zu überprüfen. Klicken Sie auf **Weiter**, um den Vorgang fortzusetzen.  
  
7.  Wenn die Seite **Bereit zum Reparieren** angezeigt wird, kann der Vorgang fortgesetzt werden. Klicken Sie zum Fortsetzen auf **Reparieren**.  
  
8.  Auf der Seite **Reparaturstatus** wird der Status des Reparaturvorgangs angezeigt. Wenn die Seite **Abgeschlossen** angezeigt wird, wurde der Vorgang abgeschlossen.  
  
Weitere Informationen zum Reparieren einer SQL Server-Instanz finden Sie unter [Reparieren von Fehlern bei einer SQL Server 2012-Installation](../database-engine/install-windows/repair-a-failed-sql-server-installation.md).  
  
### <a name="111-an-instance-of-sql-server-2012-might-fail-after-an-os-upgrade"></a>1.11 Nach einem Betriebssystemupgrade können in einer SQL Server 2012-Instanz Fehler auftreten  
**Problem:** Nachdem das Betriebssystem von Windows Vista auf Windows 7 SP1 aktualisiert wurde, kann in einer SQL Server 2012-Instanz ein Fehler mit etwa folgendem Wortlaut auftreten.  
  
`Setup has detected that the .NET Framework version 4 needs to be repaired. Do not restart your computer until Setup is complete.`  
  
**Problemumgehung**: Reparieren Sie die .NET Framework 4-Installation nach dem Betriebssystemupgrade. Weitere Informationen finden Sie unter [Wie Sie eine vorhandene Installation von .NET Framework reparieren](https://support.microsoft.com/kb/306160).  
  
### <a name="112-sql-server-edition-upgrade-requires-a-restart"></a>1.12 SQL Server-Editionsupgrade erfordert einen Neustart  
**Problem:** Wenn Sie für eine SQL Server 2012-Instanz ein Editionsupgrade ausführen, werden einige Funktionen der neuen Edition ggf. nicht sofort aktiviert.  
  
**Problemumgehung**: Starten Sie den Computer nach dem Editionsupgrade einer SQL Server 2012-Instanz neu. Weitere Informationen zu den in SQL Server 2012 unterstützten Upgrades finden Sie unter [Unterstützte Versions- und Editionsupgrades](../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md).  
  
### <a name="113-database-with-read-only-filegroup-or-files-cannot-be-upgraded"></a>1.13 Datenbanken mit schreibgeschützten Dateigruppen bzw. Dateien können nicht aktualisiert werden  
**Problem:**  Sie können eine Datenbank nicht aktualisieren, indem Sie die Datenbank anfügen oder aus einer Sicherung wiederherstellen, wenn die Datenbank bzw. die darin enthaltenen Dateien/Dateigruppen schreibgeschützt sind.  Fehler 3415 wird zurückgegeben.  Dieses Problem tritt auch bei einem direkten Upgrade einer SQL Server-Instanz auf. Das heißt, Sie versuchen, eine vorhandene SQL Server-Instanz zu ersetzen, indem Sie SQL Server 2012 installieren, und mindestens eine der vorhandenen Datenbanken ist auf schreibgeschützt festgelegt.  
  
**Problemumgehung:** Stellen Sie vor dem Upgrade sicher, dass für die Datenbank sowie die zugehörigen Dateien/Dateigruppen der Lese-/Schreibzugriff aktiviert ist.  
  
### <a name="114-reinstalling-an-instance-of-sql-server-failover-custer-fails-if-you-use-the-same-ip-address"></a>1.14 Die Neuinstallation einer SQL Server-Failoverclusterinstanz verursacht einen Fehler, wenn dieselbe IP-Adresse verwendet wird  
**Problem:** Wenn Sie bei der Installation einer SQL Server-Failoverclusterinstanz eine falsche IP-Adresse angeben, tritt ein Fehler auf. Wenn Sie nach der Deinstallation der fehlerhaften Instanz versuchen, die SQL Server-Failoverclusterinstanz mit demselben Instanznamen und der richtigen IP-Adresse erneut zu installieren, schlägt die Installation fehl. Dies liegt daran, dass noch die doppelte Ressourcengruppe aus der vorherigen Installation vorhanden ist.  
  
**Problemumgehung:** Um dieses Problem zu beheben, verwenden Sie während der Neuinstallation einen anderen Instanznamen, oder löschen Sie die Ressourcengruppe vor der Neuinstallation manuell. Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen von Knoten in einem SQL Server-Failovercluster](failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="20-analysis-services"></a><a name="AS"></a>2.0 Analysis Services  
  
### <a name="21-sql-editor-and-as-editor-cannot-connect-to-their-respective-server-instances-in-the-same-ssms-instance"></a>2.1 Der SQL-Editor und AS-Editor können nicht mit der jeweiligen Serverinstanz in derselben SSMS-Instanz verbunden werden  
**Problem:** Mit dem MDX-/DMX-Editor kann keine Verbindung mit einem Analysis Services-Server hergestellt werden, wenn der SQL-Editor bereits verbunden ist.  
  
Wenn bei Verwendung von SQL Server Management Studio 2012 (SSMS) eine SQL-Datei im Editor geöffnet ist und eine Verbindung mit einer SQL Server-Instanz besteht, kann von einer MDX- oder DMX-Datei keine Verbindung mit einer AS-Serverinstanz hergestellt werden, wenn die Datei in derselben SSMS-Instanz geöffnet wird. Auch wenn eine MDX- oder DMX-Datei bereits im Editor in SSMS geöffnet und mit einer AS-Serverinstanz verbunden ist, kann von einer SQL-Datei, die in derselben SSMS-Instanz geöffnet wird, keine Verbindung mit einer SQL Server-Instanz hergestellt werden.  
  
**Problemumgehung**:  Sie haben folgende Möglichkeiten, um das Problem zu beheben.  
  
-   Starten Sie eine weitere SSMS-Instanz, um die MDX-/DMX-Datei zu öffnen.  
  
-   Trennen Sie den SQL-Editor, und stellen Sie dann eine Verbindung zwischen dem MDX-/DMX-Editor und einem AS-Server her.  
  
### <a name="22-cannot-create-or-open-tabular-projects-when-builtinadministrators-group-name-cannot-be-resolved"></a>2.2 Projekte für tabellarische Modelle können nicht erstellt oder geöffnet werden, wenn der Name der Gruppe "BUILTIN\Administrators" nicht aufgelöst werden kann  
**Problem:** Zum Erstellen oder Öffnen tabellarischer Projekte müssen Sie Administrator einer Arbeitsbereichsdatenbank sein. Ein Benutzer kann der Serveradministratorgruppe durch Hinzufügen des Benutzer- oder Gruppennamens hinzugefügt werden. Wenn Sie Mitglied der Gruppe BUILTIN\Administrators sind, können Sie nur dann BIM-Dateien erstellen oder bearbeiten, wenn der Arbeitsbereichsdatenbankserver mit der Domäne verbunden ist, von der er ursprünglich bereitgestellt wurde. Beim Öffnen oder Erstellen der BIM-Datei treten Fehler auf, und die folgende Fehlermeldung wird angezeigt:  
  
`"The BIM file cannot be opened. The server connected to is not valid. Reason: You are not an administrator of server [server name]."`  
  
**Problemumgehungen:**  
  
-   Verknüpfen Sie den Arbeitsbereichs-Datenbankserver und den Computer mit den SQL Server Data Tools (SSDT) erneut mit der Domäne.  
  
-   Wenn der Arbeitsbereichs-Datenbankserver und/oder SSDT-Computer nicht jederzeit mit der Domäne verbunden sind, fügen Sie einzelne Benutzernamen statt der Gruppe BUILTIN\Administrators als Administratoren auf dem Arbeitsbereichs-Datenbankserver hinzu.  
  
### <a name="23-ssis-components-for-as-tabular-models-do-not-work-as-expected"></a>2.3 SSIS-Komponenten für tabellarische AS-Modelle funktionieren nicht wie erwartet  
SQL Server Integration Services (SSIS)-Komponenten für Analysis Services (AS) funktionieren für tabellarische Modelle nicht wie erwartet. Die folgenden bekannten Probleme können auftreten, wenn Sie versuchen, ein SSIS-Paket zu schreiben, das für tabellarische Modelle verwendet werden soll.  
  
**Problem:** Der AS-Verbindungs-Manager kann ein tabellarisches Modell nicht in derselben Projektmappe wie eine Datenquelle verwenden.  
  
**Problemumgehung:** Vor dem Konfigurieren des AS-Verarbeitungstasks oder des AS-Tasks „DDL ausführen“ muss explizit eine Verbindung mit dem AS-Server hergestellt werden.  
  
Bei der Arbeit mit tabellarischen Modellen verursacht der AS-Verarbeitungstask Probleme:  
  
**Problem:** Anstelle von Datenbanken, Tabellen und Partitionen sehen Sie Cubes, Measuregruppen und Dimensionen. Dies ist eine Einschränkung des Tasks.  
  
**Problemumgehung:** Das tabellarische Modell kann weiterhin mit der aus Cubes/Measuregruppen/Dimensionen bestehenden Struktur verarbeitet werden.  
  
**Problem:** Einige Verarbeitungsoptionen, die von AS im tabellarischen Modus unterstützt werden (z.B. „Defragmentierung verarbeiten“), werden im AS-Verarbeitungstask nicht verfügbar gemacht.  
  
**Problemumgehung:** Verwenden Sie stattdessen den Analysis Services-Task „DDL ausführen“ zum Ausführen eines XMLA-Skripts, das den ProcessDefrag-Befehl enthält.  
  
**Problem:** Einige Konfigurationsoptionen im Tool treffen nicht zu. Beispielsweise sollte Verbundene Objekte verarbeiten beim Verarbeiten von Partitionen nicht verwendet werden, und die Konfigurationsoption Parallelverarbeitung enthält eine ungültige Fehlermeldung, die angibt, dass die Parallelverarbeitung in der Standard-SKU nicht unterstützt wird.  
  
**Problemumgehung:** Keine  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="30-books-online"></a><a name="BOL"></a>3.0 Onlinedokumentation  
  
### <a name="31-help-viewer-for-sql-server-crashes-in-environments-configured-to-run-only-ipv6"></a>3.1 Der Hilfe-Viewer für SQL Server stürzt in Umgebungen ab, die ausschließlich für die Ausführung von IPv6 konfiguriert sind  
**Problem:** Wenn die Umgebung ausschließlich für die Ausführung von IPv6 konfiguriert ist, stürzt Help Viewer von SQL Server 2012 ab, und Sie erhalten die folgende Fehlermeldung:  
  
`HelpLibAgent.exe has stopped working.`  
  
> [!IMPORTANT]  
> Dies gilt für alle Umgebungen, für die ausschließlich IPv6 aktiviert ist. Umgebungen mit IPv4 (bzw. einer Kombination von IPv4 und IPv6) sind nicht betroffen.  
  
**Problemumgehung**: Um das Problem zu vermeiden, aktivieren Sie IPv4, oder folgen Sie den nachfolgenden Anweisungen, um einen Registrierungseintrag und eine Zugriffssteuerungsliste (ACL) zu erstellen, um Help Viewer für IPv6 zu aktivieren:  
  
1.  Erstellen Sie unter HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Help\v1.0 einen Registrierungsschlüssel mit dem Namen „IPv6“ und dem Wert „1 (DWORD(32 Bit))“.  
  
2.  Legen Sie die Sicherheits-ACL-Einstellungen für den Port für IPv6 fest, indem Sie über die Administrator-Eingabeaufforderung folgenden Befehl ausführen:  
  
    ```  
    netsh http add urlacl url=https://[::1]:47873/help/ sddl=D:(A;;GX;;;WD)  
    ```  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="40-data-quality-services"></a><a name="DQS"></a>4.0 Data Quality Services  
  
### <a name="41-dqs-not-supported-in-a-cluster"></a>4.1 DQS wird in einem Cluster nicht unterstützt  
**Problem:** DQS wird in einer SQL Server-Clusterinstallation nicht unterstützt. Wenn Sie eine Clusterinstanz von SQL Server installieren, dürfen die Kontrollkästchen **Data Quality Services** und **Data Quality Client** auf der Seite **Funktionsauswahl** nicht aktiviert werden. Wenn diese Kontrollkästchen während der Installation einer Clusterinstanz aktiviert sind (und Sie die Installation der Data Quality Server-Instanz durch Ausführen der Datei „DQSInstaller.exe“ abschließen), wird DQS zwar auf diesem Knoten installiert, ist auf zusätzlichen Knoten, die Sie dem Cluster hinzufügen, jedoch nicht verfügbar und daher auf diesen zusätzlichen Knoten auch nicht funktionsfähig.  
  
**Problemumgehung:** Installieren Sie das kumulative Update 1 für SQL Server 2012, um das Problem zu beheben. Anweisungen finden Sie unter [https://support.microsoft.com/kb/2674817](https://support.microsoft.com/kb/2674817).  
  
### <a name="42-to-reinstall-data-quality-server-delete-the-dqs-objects-after-uninstalling-data-quality-server"></a>4.2 Zur Neuinstallation der Data Quality Server-Instanz löschen Sie nach der Deinstallation der Data Quality Server-Instanz die DQS-Objekte  
**Problem:** Beim Deinstallieren des Data Quality-Servers werden die DQS-Objekte (DQS-Datenbanken, DQS-Anmeldungen und eine gespeicherte DQS-Prozedur) nicht aus der SQL Server-Instanz gelöscht.  
  
**Problemumgehung:** Zur Neuinstallation des Data Quality-Servers auf demselben Computer und in derselben SQL Server-Instanz müssen Sie die DQS-Objekte manuell aus der SQL Server-Instanz löschen. Außerdem müssen Sie die DQS-Datenbankdateien (DQS_MAIN, DQS_PROJECTS und DQS_STAGING_DATA) aus dem Ordner C:\Programme\Microsoft SQL Server\MSSQL11.<SQL_Server_Instance>\MSSQL\DATA auf dem Computer löschen, bevor Sie den Data Quality-Server neu installieren. Andernfalls schlägt die Installation der Data Quality Server-Instanz fehl. Verschieben Sie die Datenbankdateien, statt sie zu löschen, wenn Daten wie Wissensdatenbanken oder Data Quality-Projekte beibehalten werden sollen. Weitere Informationen zum Entfernen von DQS-Objekten nach Abschluss der Deinstallation finden Sie unter [Entfernen von Data Quality Server-Objekten](./install/remove-data-quality-server-objects.md).  
  
### <a name="43-indication-of-a-terminated-knowledge-discovery-or-interactive-cleansing-activity-is-delayed"></a>4.3 Die Anzeige einer beendeten Wissensermittlung oder interaktiven Bereinigungsaktivität ist zeitverzögert  
**Problem:** Beendet ein Administrator eine Aktivität im Bildschirm „Aktivitätsüberwachung“, wird das Ende der Aktivität einem interaktiven Benutzer, der die Wissensermittlung, Domänenverwaltung oder interaktive Bereinigungsaktivität ausführt, erst dann angezeigt, wenn er den nächsten Vorgang ausführt.  
  
**Problemumgehung:** Keine  
  
### <a name="44-a-cancel-operation-discards-work-from-multiple-activities"></a>4.4 Bei einem Abbruchvorgang kann Arbeit aus mehreren Aktivitäten verworfen werden  
**Problem:** Wenn Sie für eine ausgeführte Wissensermittlungs- oder Domänenverwaltungsaktivität auf **Abbrechen** klicken und andere Aktivitäten zuvor abgeschlossen wurden, ohne dass ein Veröffentlichungsvorgang während der Aktivität ausgeführt wurde, wird die Arbeit aus allen seit der letzten Veröffentlichung ausgeführten Aktivitäten verworfen, nicht nur die der aktuellen Aktivität.  
  
**Problemumgehung:** Um dieses Problem zu vermeiden, veröffentlichen Sie persistent zu speichernde Daten in der Wissensdatenbank, bevor eine neue Aktivität gestartet wird.  
  
### <a name="45-controls-do-not-scale-properly-on-large-font-sizes"></a>4.5 Steuerelemente werden bei großen Schriftgraden nicht richtig skaliert  
**Problem:** Wenn Sie die Textgröße auf „Größer – 150 %“ (in Windows Server 2008 oder Windows 7) ändern oder die benutzerdefinierte DPI-Einstellung auf 200 % (in Windows 7) festlegen, sind die Schaltflächen **Abbrechen** und **Erstellen** auf der Seite **Neue Wissensdatenbank** nicht verfügbar.  
  
**Problemumgehung:** Legen Sie einen kleineren Schriftgrad fest, um das Problem zu beheben.  
  
### <a name="46-screen-resolution-of-800x600-is-not-supported"></a>4.6 Eine Bildschirmauflösung von 800 x 600 wird nicht unterstützt  
**Problem:** Die Data Quality-Clientanwendung wird nicht richtig angezeigt, wenn die Bildschirmauflösung auf 800 x 600 festgelegt ist.  
  
**Problemumgehung:** Um das Problem zu beheben, legen Sie eine höhere Bildschirmauflösung fest.  
  
### <a name="47-map-bigint-column-in-the-source-data-to-a-decimal-domain-to-prevent-data-loss"></a>4.7 Zuordnen einer bigint-Spalte in den Quelldaten zu einer Domäne vom Datentyp "decimal" zur Vermeidung von Datenverlusten  
**Problem:** Wenn eine Spalte in den Quelldaten den Datentyp **bigint** aufweist, müssen Sie in DQS die Spalte einer Domäne vom Datentyp **decimal** anstatt einer Domäne vom Datentyp **integer** zuordnen. Der Datentyp **decimal** repräsentiert eine größere Anzahl von Werten als der Datentyp **int** und kann daher größere Werte speichern.  
  
### <a name="48-nvarcharmax-and-varcharmax-data-types-are-not-supported-in-the-dqs-cleansing-component-in-integration-services"></a>4.8 Der NVARCHAR(MAX)-Datentyp und der VARCHAR(MAX)-Datentyp werden in der Komponente zur DQS-Bereinigung in Integration Services nicht unterstützt  
**Problem:** Datenspalten der Datentypen **nvarchar(max)** und **varchar(max)** werden in der Komponente zur DQS-Bereinigung in Integration Services nicht unterstützt. Aus diesem Grund sind diese Datenspalten auf der Registerkarte Zuordnung des Transformations-Editors für die DQS-Bereinigung nicht für Zuordnungen verfügbar und können folglich nicht bereinigt werden.  
  
**Problemumgehung:** Vor der Verarbeitung dieser Datenspalten mithilfe der Komponente zur DQS-Bereinigung müssen Sie sie mit der Transformation für Datenkonvertierung in den Datentyp **DT_STR** oder den Datentyp **DT_WSTR** konvertieren.  
  
### <a name="49-the-item-to-run-dqsinstallerexe-on-the-start-menu-is-overwritten-on-new-sql-server-instance-installation"></a>4.9 Das Element zum Ausführen von "DQSInstaller.exe" im Startmenü wird bei der Installation einer neuen SQL Server-Instanz überschrieben  
**Problem:** Wenn Sie Data Quality Services in einer SQL Server-Instanz installieren, wird nach Abschluss des SQL Server-Setups im **Startmenü** unter der Programmgruppe **Data Quality Services** ein Eintrag mit dem Namen **Data Quality-Server-Installationsprogramm** erstellt. Wenn Sie jedoch mehrere SQL Server-Instanzen auf demselben Computer installieren, befindet sich trotzdem nur ein Eintrag **Data Quality Server-Installationsprogramm** im **Startmenü** . Durch Klicken auf diesen Eintrag wird die Datei DQSInstaller.exe in der zuletzt installierten SQL Server-Instanz ausgeführt.  
  
### <a name="410-activity-monitoring-displays-incorrect-status-for-failed-integration-services-cleansing-activities"></a>4.10 Bei der Aktivitätsüberwachung wird für fehlerhafte Bereinigungsaktivitäten von Integration Services ein falscher Status angezeigt  
Im Bildschirm „Aktivitätsüberwachung“ wird selbst für Integration Services-basierte Bereinigungsaktivitäten, bei denen ein Fehler aufgetreten ist, in der Spalte **Aktueller Status** der Eintrag **Erfolgreich** angezeigt.  
  
### <a name="411-schema-name-is-not-displayed-as-part-of-tableview-name"></a>4.11 Der Schemaname wird nicht als Teil eines Tabellen-/Sichtnamens angezeigt  
Wenn in einer der DQS-Aktivitäten während der Zuordnungsphase im Data Quality-Client eine SQL Server-Datenquelle ausgewählt wird, wird die Liste der Tabellen und Sichten ohne den Schemanamen angezeigt. Aus diesem Grund können mehrere Tabellen/Sichten mit dem gleichen Namen, aber unterschiedlichen Schemas nur unterschieden werden, indem Sie sie in der Datenvorschau anzeigen bzw. auswählen und dann die für die Zuordnung verfügbaren Felder überprüfen.  
  
### <a name="412-issue-with-cleansing-output-and-export-if-data-source-is-mapped-to-a-composite-domain-containing-a-child-domain-of-date-type"></a>4.12 Problem bei der Ausgabe der Bereinigung und dem Export, wenn eine Datenquelle einer Verbunddomäne zugeordnet ist, die eine untergeordnete Domäne vom Typ "Date" enthält  
Wenn Sie in einem Data Quality-Bereinigungsprojekt ein Feld in den Quelldaten einer Verbunddomäne zugeordnet haben, die über eine untergeordnete Domäne vom Datentyp Date verfügt, weist die Ausgabe der untergeordneten Domäne im Bereinigungsergebnis ein falsches Datumsformat auf, sodass beim Export in die Datenbank ein Fehler verursacht wird.  
  
### <a name="413-error-when-mapping-to-an-excel-sheet-that-contains-a--semicolon-in-its-name"></a>4.13 Fehler beim Zuordnen zu einer Excel-Tabelle mit einem Semikolon (;) im Namen  
**Problem:** Wenn Sie im Data Quality-Client auf der Seite **Zuordnen** einer beliebigen DQS-Aktivität eine Zuordnung zur Excel-Quelltabelle vornehmen, in deren Name ein Semikolon (;) enthalten ist, wird eine Meldung zu einer unbehandelten Ausnahme angezeigt, sobald Sie auf der Seite **Zuordnen** auf **Weiter** klicken.  
  
**Problemumgehung:** Entfernen Sie das Semikolon (;) aus dem Tabellennamen in der Excel-Datei, in der die zuzuordnenden Quelldaten enthalten sind, und wiederholen Sie den Vorgang.  
  
### <a name="414-issue-with-date-or-datetime-values-in-unmapped-source-fields-in-excel-during-cleansing-and-matching"></a>4.14 Problem mit Date-Werten oder DateTime-Werten in nicht zugeordneten Quellfeldern in Excel bei der Bereinigung und beim Abgleich  
**Problem:** Wenn die Quelldaten im Excel-Format vorliegen und Sie die Quellfelder mit Werten vom Datentyp **Date** oder **DateTime** nicht zugeordnet haben, geschieht während Bereinigungs- und Abgleichsaktivitäten Folgendes:  
  
-   Die nicht zugeordneten **Date**-Werte werden im YYYYMMDD-Format angezeigt und exportiert.  
  
-   Der Zeitwert nicht zugeordneter **DateTime**-Werte geht verloren, und sie werden im YYYYMMDD-Format angezeigt und exportiert.  
  
**Problemumgehung:** Sie können die nicht zugeordneten Feldwerte in der Bereinigungsaktivität unten rechts auf der Seite **Ergebnisse verwalten und anzeigen** und in der Abgleichsaktivität auf der Seite **Abgleich** einsehen.  
  
### <a name="415-cannot-import-domain-values-from-an-excel-file-xls-containing-more-than-255-columns-of-data"></a>4.15 Aus einer Excel-Datei (.xls) mit mehr als 255 Datenspalten können keine Domänenwerte importiert werden  
**Problem:** Wenn Sie aus einer in Excel 97-2003 erstellten Datei (XLS-Datei), die mehr als 255 Datenspalten enthält, Werte in eine Domäne importieren, wird eine Ausnahmemeldung angezeigt, und der Import verursacht einen Fehler.  
  
**Problemumgehung:** Führen Sie einen der folgenden Schritte aus, um das Problem zu beheben:  
  
-   Speichern Sie die XLS-Datei als XLSX-Datei, und importieren Sie dann die Werte aus der XLSX-Datei in eine Domäne.  
  
-   Entfernen Sie die Daten aus allen Spalten auf Spalte 255 folgenden Spalten aus der XLS-Datei, speichern Sie die Datei, und importieren Sie dann die Werte aus der XLS-Datei in eine Domäne.  
  
### <a name="416-activity-monitoring-feature-is-unavailable-for-roles-other-than-dqs_administrator"></a>4.16 Die Funktion für die Aktivitätsüberwachung ist für andere Rollen als "dqs_administrator" nicht verfügbar  
Die Funktion für die Aktivitätsüberwachung ist nur für Benutzer mit der Rolle dqs_administrator verfügbar. Wenn Ihrem Benutzerkonto die Rolle dqs_kb_editor oder dqs_kb_operator zugewiesen ist, ist die Funktion für die Aktivitätsüberwachung in der Data Quality-Clientanwendung nicht verfügbar.  
  
### <a name="417-error-on-opening-a-knowledge-base-in-the-recent-knowledge-base-list-for-domain-management"></a>4.17 Wenn eine Wissensdatenbank für die Domänenverwaltung in der Liste "Zuletzt verwendete Wissensdatenbank" geöffnet wird, tritt ein Fehler auf  
Problem: Wenn Sie eine Wissensdatenbank aus der Liste **Zuletzt verwendete Wissensdatenbank** für die Domänenverwaltungsaktivität öffnen, kann die folgende Fehlermeldung auf dem Startbildschirm des Data Quality-Clients angezeigt werden:  
  
`"A configuration with name 'RecentList:KB:<domain>\<username>' already exists in the database."`  
  
Das Problem tritt auf, weil Zeichenfolgen in der SQL Server-Datenbank und in C# von DQS auf unterschiedliche Weise verglichen werden. Beim Zeichenfolgenvergleich in der SQL Server-Datenbank wird die Groß-/Kleinschreibung nicht beachtet, während sie in C# beachtet wird.  
  
Dies wird an einem Beispiel deutlich. Im Beispiel wird der Benutzer "Domain\user1" verwendet. Der Benutzer meldet sich unter Verwendung des Kontos „user1“ beim Data Quality-Client-Computer an und arbeitet mit einer Wissensdatenbank. DQS speichert die zuletzt verwendete Wissensdatenbank für jeden Benutzer als Datensatz in der A_CONFIGURATION-Tabelle der DQS_MAIN-Datenbank. In diesem Fall wird der Datensatz unter dem folgenden Namen gespeichert: RecentList:KB:Domain\user1. Später meldet sich der Benutzer als „User1“ (beachten Sie den Großbuchstaben U) beim Data Quality-Client-Computer an und versucht, die in der Liste **Zuletzt verwendete Wissensdatenbank** enthaltene Wissensdatenbank zu öffnen, um die Domänenverwaltungsaktivität auszuführen. Durch den DQS zugrunde liegenden Code werden die beiden Zeichenfolgen „RecentList:KB:DOMAIN\user1“ und „DOMAIN\User1“ verglichen. Da beim Zeichenfolgenvergleich in C# die Groß-/Kleinschreibung beachtet wird, stimmen die Zeichenfolgen nicht überein, sodass DQS versucht, einen neuen Datensatz für den Benutzer (User1) in die A_CONFIGURATION-Tabelle der DQS_MAIN-Datenbank einzufügen. Da die Groß-/Kleinschreibung beim Zeichenfolgenvergleich in der SQL-Datenbank jedoch nicht beachtet wird, ist die Zeichenfolge in der A_CONFIGURATION-Tabelle der DQS_MAIN-Datenbank folglich schon vorhanden, sodass der Einfügevorgang fehlschlägt.  
  
**Problemumgehung:** Führen Sie einen der folgenden Schritte aus, um das Problem zu beheben:  
  
-   Überprüfen Sie, ob doppelte Einträge vorhanden sind, indem Sie die folgende Anweisung ausführen:  
  
    ```  
    SELECT * FROM DQS_MAIN.dbo.A_CONFIGURATION WHERE NAME like 'RecentList%'  
    ```  
  
    Anschließend können Sie die folgende Anweisung ausführen, um den Datensatz nur für den betroffenen Benutzer zu löschen, indem Sie den Wert in der WHERE-Klausel in die Domäne und den Namen des betroffenen Benutzers ändern.  
  
    ```  
    DELETE DQS_MAIN.dbo.A_Configuration WHERE NAME LIKE 'RecentList%<domain>\<username>'  
    ```  
  
    Alternativ können Sie alle zuletzt verwendeten Elemente für alle Benutzer in DQS entfernen:  
  
    ```  
    DELETE DQS_MAIN.dbo.A_Configuration WHERE NAME LIKE 'RecentList%'  
    ```  
  
-   Verwenden Sie dieselbe Groß-/Kleinschreibung, die Sie bei der letzten Anmeldung beim Data Quality-Clientcomputer für die Angabe des Benutzerkontos verwendet haben.  
  
> [!NOTE]  
> Um dieses Problem zu vermeiden, verwenden Sie zur Angabe des Benutzerkontos für die Anmeldung beim Data Quality-Clientcomputer konsistente Regeln für die Groß-/Kleinschreibung.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="50-database-engine"></a><a name="DE"></a>5.0 Database Engine (Datenbankmodul)  
  
### <a name="51-use-of-distributed-replay-controller-and-distributed-replay-client-features"></a>5.1 Verwenden der Funktion "Distributed Replay Controller" und "Distributed Replay Client"  
**Problem:** Die Funktionen „Distributed Replay Controller“ und „Distributed Replay Client“ werden in der Server Core-SKU von Windows Server 2008, Windows Server 2008 R2 und Windows Server 7 zur Verfügung gestellt, obwohl die beiden Funktionen in der Server Core-SKU nicht unterstützt werden.  
  
**Problemumgehung:** Diese beiden Funktionen sollten in der Server Core-SKU von Windows Server 2008, Windows Server 2008 R2 und Windows Server 7 weder installiert noch verwendet werden.  
  
### <a name="52-sql-server-management-studio-depends-on-visual-studio-2010-sp1"></a>5.2 SQL Server Management Studio ist von Visual Studio 2010 SP1 abhängig  
**Problem:**  Zum ordnungsgemäßen Ausführen von SQL Server 2012 Management Studio ist Visual Studio 2010 SP1 erforderlich. Die Deinstallation von Visual Studio 2010 SP1 kann Funktionsverluste in SQL Server Management Studio verursachen und Management Studio in einen nicht unterstützten Status versetzen. In diesem Fall können folgende Probleme auftreten:  
  
-   Befehlszeilenparameter für ssms.exe funktionieren nicht ordnungsgemäß.  
  
-   Die Hilfeinformationen, die bei der Ausführung von ssms.exe mit dem /?-Schalter angezeigt werden, sind nicht korrekt.  
  
-   Für jede Datei, die im Windows-Explorer durch Doppelklicken geöffnet wird, wird eine neue SSMS-Instanz gestartet, um die Datei zu öffnen.  
  
-   Abfragen können im normalen Benutzermodus nicht debuggt werden.  
  
**Problemumgehung**: Installieren Sie Visual Studio 2010 SP1 erneut, und starten Sie Management Studio neu.  
  
### <a name="53-x64-operating-systems-require-64-bit-powershell-20"></a>5.3 x64-Betriebssysteme benötigen die 64-Bit-Version von PowerShell 2.0  
**Problem:** 32-Bit-Installationen der Windows PowerShell-Erweiterungen für SQL Server werden für SQL Server 2012-Instanzen unter 64-Bit-Betriebssystemen nicht unterstützt.  
  
**Problemumgehungen:**  
  
-   Installieren Sie SQL Server 2012 (64 Bit) zusammen mit den 64-Bit-Verwaltungstools und der 64-Bit-Version der Windows PowerShell Extensions für SQL Server.  
  
-   Oder importieren Sie das SQLPS-Modul über eine Eingabeaufforderung der 32-Bit-Version von Windows PowerShell 2.0.  
  
### <a name="54-an-error-might-occur-when-navigating-in-the-generate-script-wizard"></a>5.4 Beim Navigieren im Assistenten zum Generieren von Skripts kann ein Fehler auftreten  
**Problem:** Nachdem Sie im Assistenten zum Generieren von Skripts ein Skript erstellt haben, indem Sie auf **Skripts speichern oder veröffentlichen** geklickt haben, und dann weiter navigieren, indem Sie auf **Optionen auswählen** oder **Skripterstellungsoptionen festlegen** und dann erneut auf **Skripts speichern oder veröffentlichen** klicken, kann folgender Fehler auftreten:  
  
```
An exception occurred while executing a Transact-SQL statement or batch. (Microsoft.SqlServer.ConnectionInfo)  
------------------------------  
ADDITIONAL INFORMATION:  
Invalid object name 'sys.federations'. (Microsoft SQL Server, Error: 208)
```
  
**Problemumgehung:** Schließen Sie den Assistenten zum Generieren von Skripts, und öffnen Sie ihn erneut.  
  
### <a name="55-new-maintenance-plan-layout-not-compatible-with-earlier-sql-server-tools"></a>5.5 Das neue Layout für Wartungspläne ist mit früheren SQL Server-Tools nicht kompatibel  
**Problem:** Wenn Sie mit den SQL Server 2012-Verwaltungstools einen vorhandenen Wartungsplan aus einer vorherigen Version der SQL Server-Verwaltungstools (SQL Server 2008 R2, SQL Server 2008 oder SQL Server 2005) ändern, wird der Wartungsplan in einem neuen Format gespeichert. Frühere Versionen der SQL Server-Verwaltungstools unterstützen dieses neue Format nicht.  
  
**Problemumgehung**: Keine  
  
### <a name="56-intellisense-has-limitations-when-logged-in-to-a-contained-database"></a>5.6 Die IntelliSense-Funktionalität ist nach der Anmeldung bei einer eigenständigen Datenbank eingeschränkt  
Problem: IntelliSense funktioniert in SQL Server Management Studio (SSMS) und SQL Server Data Tools (SSDT) nicht erwartungsgemäß, wenn eigenständige Benutzer bei eigenständigen Datenbanken angemeldet sind. In diesen Fällen tritt das folgende Verhalten auf:  
  
1.  Ungültige Objekte werden nicht unterstrichen.  
  
2.  Die AutoVervollständigen-Liste wird nicht angezeigt.  
  
3.  Die QuickInfo-Hilfe für integrierte Funktionen wird nicht ausgeführt.  
  
**Problemumgehung**:  Keine  
  
### <a name="57-alwayson-availability-groups"></a>5.7 AlwaysOn-Verfügbarkeitsgruppen  
Bevor Sie eine Verfügbarkeitsgruppe erstellen, lesen Sie den Abschnitt [Voraussetzungen, Einschränkungen und Empfehlungen für AlwaysOn-Verfügbarkeitsgruppen (SQL Server)](https://go.microsoft.com/?linkid=9753168) in der Onlinedokumentation. Eine Einführung in AlwaysOn-Verfügbarkeitsgruppen finden Sie unter [AlwaysOn-Verfügbarkeitsgruppen (SQL Server)](https://go.microsoft.com/?linkid=9753166)in der Onlinedokumentation.  
  
#### <a name="571-client-connectivity-for-alwayson-availability-groups"></a>5.7.1 Clientkonnektivität für AlwaysOn-Verfügbarkeitsgruppen  
**Aktualisiert am:** 13. August 2012  
  
In diesem Abschnitt werden die Treiberunterstützung für AlwaysOn-Verfügbarkeitsgruppen und Problemumgehungen bei der Verwendung nicht unterstützter Treiber beschrieben.  
  
**Treiberunterstützung**  
  
In der folgenden Tabelle ist die Treiberunterstützung für AlwaysOn-Verfügbarkeitsgruppen zusammengefasst:  
  
|Treiber|Multisubnetz-Failover|Application Intent|Schreibgeschütztes Routing|Multisubnetz-Failover: Schnelleres Endpunktfailover in einzelnen Subnetzen|Multisubnetz-Failover: Auflösung benannter Instanzen für SQL-Clusterinstanzen|  
|----------|--------------------------|----------------------|----------------------|------------------------------------------------------------------|---------------------------------------------------------------------------------|  
|SQL Native Client 11.0 ODBC|Ja|Ja|Ja|Ja|Ja|  
|SQL Native Client 11.0 OLEDB|Nein|Ja|Ja|Nein|Nein|  
|ADO.NET mit .NET Framework 4.0 mit Konnektivitätspatch **\&#42;**|Ja|Ja|Ja|Ja|Ja|  
|ADO.NET mit .NET Framework 3.5 SP1 mit Konnektivitätspatch **\&#42;\&#42;**|Ja|Ja|Ja|Ja|Ja|  
|Microsoft JDBC-Treiber 4.0 für SQL Server|Ja|Ja|Ja|Ja|Ja|  
  
**\&#42;** Download des Konnektivitätspatches für ADO.NET mit .NET Framework 4.0: [https://support.microsoft.com/kb/2600211](https://support.microsoft.com/kb/2600211).  
  
**\&#42;\&#42;** Download des Konnektivitätspatches für ADO.NET mit .NET Framework 3.5 SP1: [https://support.microsoft.com/kb/2654347](https://support.microsoft.com/kb/2654347).  
  
**MultiSubnetFailover-Schlüsselwort und zugehörige Funktionen**  
  
MultiSubnetFailover ist ein neues Schlüsselwort für Verbindungszeichenfolgen, das ein schnelleres Failover bei AlwaysOn-Verfügbarkeitsgruppen und AlwaysOn-Failoverclusterinstanzen in SQL Server 2012 ermöglicht. Wenn in der Verbindungszeichenfolge "MultiSubnetFailover=True" festgelegt wird, werden die folgenden drei Teilfunktionen aktiviert:  
  
-   Schnelleres Multisubnetz-Failover auf einen Multisubnetzlistener für eine AlwaysOn-Verfügbarkeitsgruppe oder Failoverclusterinstanzen.  
  
    -   Auflösung benannter Instanzen in eine Multisubnetz-AlwaysOn-Failoverclusterinstanz.  
  
-   Schnelleres Einzelsubnetz-Failover auf einen Einzelsubnetzlistener für eine AlwaysOn-Verfügbarkeitsgruppe oder Failoverclusterinstanzen.  
  
    -   Diese Funktion wird für Verbindungen mit einem Listener verwendet, der über eine einzelne IP in einem einzelnen Subnetz verfügt. Dadurch werden TCP-Verbindungsversuche aggressiver wiederholt, um Failovervorgänge einzelner Subnetze zu beschleunigen.  
  
-   Auflösung benannter Instanzen in eine Multisubnetz-AlwaysOn-Failoverclusterinstanz.  
  
    -   Wird verwendet, um die Auflösung benannter Instanzen für AlwaysOn-Failoverclusterinstanzen mit mehreren Subnetzendpunkten zu unterstützen.  
  
**"MultiSubnetFailover=True" wird von NET Framework 3.5 oder OLE DB nicht unterstützt.**  
  
**Problem:** Wenn die Verfügbarkeitsgruppe oder Failoverclusterinstanz über einen Listenernamen (wird im WSFC-Cluster-Manager als Netzwerkname oder Clientzugriffspunkt bezeichnet) verfügt, der von mehreren IP-Adressen aus unterschiedlichen Subnetzen abhängig ist, und Sie entweder ADO.NET mit .NET Framework 3.5 SP1 oder SQL Native Client 11.0 OLE DB verwenden, tritt bei bis zu 50 % der Clientverbindungsanforderungen an den Listener der Verfügbarkeitsgruppe ein Verbindungstimeout auf.  
  
**Problemumgehungen:** Eine der folgenden Lösungen wird empfohlen.  
  
-   Wenn Sie nicht über die Berechtigung zur Bearbeitung von Clusterressourcen verfügen, ändern Sie den Wert für das Verbindungstimeout in 30 Sekunden (20-sekündiger TCP-Timeoutzeitraum plus Puffer von 10 Sekunden).  
  
    **Vorteile:** Beim Eintreten eines subnetzübergreifenden Failovers ist die Clientwiederherstellungszeit nur kurz.  
  
    **Nachteile:** Für die Hälfte der Clientverbindungen sind mehr als 20 Sekunden erforderlich.  
  
-   Wenn Sie über die notwendigen Berechtigungen zum Bearbeiten der Clusterressourcen verfügen, sollten Sie den Netzwerknamen des Verfügbarkeitsgruppenlisteners auf **RegisterAllProvidersIP**=0 festlegen. Weitere Informationen finden Sie unter "Beispiel-PowerShell-Skript zur Deaktivierung von RegisterAllProvidersIP und zur Reduzierung der Gültigkeitsdauer (TTL)" weiter unten in diesem Abschnitt.  
  
    **Vorteile:** Sie müssen den Clientverbindungs-Timeoutwert nicht erhöhen.  
  
    **Nachteile:** Beim Auftreten eines subnetzübergreifenden Failovers kann die Clientwiederherstellungszeit 15 Minuten oder länger betragen, je nach Einstellung für HostRecordTTL bzw. den siteübergreifenden DNS/AD-Replikationszeitplan.  
  
**Beispiel-PowerShell-Skript zur Deaktivierung von RegisterAllProvidersIP und zur Reduzierung der Gültigkeitsdauer (TTL)**  
  
Das folgende Beispiel-PowerShell-Skript zeigt, wie Sie `RegisterAllProvidersIP` deaktivieren und die Gültigkeitsdauer (TTL) reduzieren. Ersetzen Sie `yourListenerName` durch den Namen des Listeners, den Sie ändern.  
  
```  
Import-Module FailoverClusters  
Get-ClusterResource yourListenerName|Set-ClusterParameter RegisterAllProvidersIP 0  
Get-ClusterResource yourListenerName|Set-ClusterParameter HostRecordTTL 300  
```  
  
#### <a name="572-upgrading-from-ctp3-with-availability-group-configured-is-not-supported"></a>5.7.2 Upgrades von CTP3 mit konfigurierter Verfügbarkeitsgruppe werden nicht unterstützt  
Löschen Sie die Verfügbarkeitsgruppe vor dem Upgrade, und erstellen Sie sie neu. Dies ist eine Einschränkung des CTP3-Builds. Diese Einschränkung wird in zukünftigen Builds behoben sein.  
  
#### <a name="573-side-by-side-installation-of-ctp3-with-later-versions-is-not-supported-if-you-have-an-availability-group-configured-in-your-instance"></a>5.7.3 Die parallele Installation von CTP3 mit späteren Versionen wird nicht unterstützt, wenn Sie in Ihrer Instanz eine Verfügbarkeitsgruppe konfiguriert haben.  
Dies ist eine Einschränkung des CTP3-Builds. Diese Einschränkung wird in zukünftigen Builds behoben sein.  
  
#### <a name="574-side-by-side-installation-of-ctp3-with-later-versions-of-failover-cluster-instances-is-not-supported"></a>5.7.4 Die parallele Installation von CTP3 mit Failoverclusterinstanzen späterer Versionen wird nicht unterstützt.  
Dies ist eine Einschränkung des CTP3-Builds. Diese Einschränkung wird in zukünftigen Builds behoben sein. Um Failoverclusterinstanzen von CTP3 zu aktualisieren, müssen alle Instanzen auf einem Knoten gleichzeitig aktualisiert werden.  
  
#### <a name="575--timeouts-may-occur-when-using-multi-ips-in-the-same-subnet-with-alwayson"></a>5.7.5 Bei Verwendung mehrerer IPs mit AlwaysOn im selben Subnetz können Timeouts auftreten  
**Problem:** Bei Verwendung mehrerer IPs mit AlwaysOn im selben Subnetz können Kunden zeitweise Timeoutfehler angezeigt werden. Dies geschieht, wenn die oberste IP in der Liste fehlerhaft ist.  
  
**Problemumgehung:** Verwenden Sie „multisubnetfailover = true“ in der Verbindungszeichenfolge.  
  
#### <a name="576-failure-to-create-new-availability-group-listeners-because-of-active-directory-quotas"></a>5.7.6 Fehler beim Erstellen neuer Verfügbarkeitsgruppenlistener aufgrund von Active Directory-Kontingenten  
**Problem:** Die Erstellung eines neuen Verfügbarkeitsgruppenlisteners schlägt möglicherweise beim Erstellen fehl, da Sie ein Active Directory-Kontingent für das teilnehmende Clusterknoten-Computerkonto erreicht haben. Weitere Informationen finden Sie unter [Problembehandlung für das Clusterdienstkonto bei Änderung von Computerobjekten](https://support.microsoft.com/kb/307532) und [Active Directory-Kontingente](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc904295(v=ws.10)).  
  
#### <a name="577-netbios-conflicts-because-availability-group-listener-names-use-an-identical-15-character-prefix"></a>5.7.7 NetBIOS-Konflikte, da Namen von Verfügbarkeitsgruppenlistenern ein identisches 15-Zeichen-Präfix verwenden  
Wenn Sie zwei WSFC-Cluster verwenden, die vom gleichen Active Directory gesteuert werden, und Sie versuchen, Verfügbarkeitsgruppenlistener in beiden Clustern mit Namen mit mehr als 15 Zeichen und einem identischen 15-Zeichen-Präfix zu erstellen, erhalten Sie eine Fehlermeldung mit dem Hinweis, dass die VNN-Ressource nicht online geschaltet werden konnte. Informationen zu Präfixbenennungsregeln für DNS-Namen finden Sie unter [Zuweisen von Domänennamen](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731265(v=ws.10)).  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="60-integration-services"></a><a name="IS"></a>6.0 Integration Services  
  
### <a name="61-the-change-data-capture-service-for-oracle-and-the-change-data-capture-designer-console-for-oracle"></a>6.1 Change Data Capture Service für Oracle und Change Data Capture Designer Console für Oracle  
CDC Service für Oracle ist ein Windows-Dienst, der Oracle-Transaktionsprotokolle scannt und Änderungen an relevanten Oracle-Tabellen in SQL Server-Änderungstabellen aufzeichnet. CDC Designer Console wird verwendet, um Oracle CDC-Instanzen zu entwickeln und zu warten. CDC Designer Console ist ein Microsoft Management Console (MMC)-Snap-In.  
  
#### <a name="611-install-the-cdc-service-for-oracle-and-the-cdc-designer-for-oracle"></a>6.1.1 Installieren von CDC Service für Oracle und CDC Designer für Oracle  
**Problem:** CDC Service und CDC Designer werden nicht von SQL Server-Setup installiert. Sie müssen CDC Service oder CDC Designer manuell auf einem Computer installieren, der die in den aktualisierten Hilfedateien beschriebenen Anforderungen und Voraussetzungen erfüllt.  
  
**Problemumgehung:** Führen Sie zum Installieren des CDC Service for Oracle „AttunityOracleCdcService.msi“ manuell von den SQL Server-Installationsmedien aus. Führen Sie zum Installieren von CDC Designer Console AttunityOracleCdcDesigner.msi manuell von den SQL Server-Installationsmedien aus.  Installationspakete für x86 und x64 befinden sich auf den SQL Server-Installationsmedien unter \Tools\AttunityCDCOracle\.  
  
#### <a name="612-f1-help-functionality-points-to-incorrect-documentation-files"></a>6.1.2 Die F1-Hilfefunktion zeigt auf die falschen Dokumentationsdateien  
**Problem:** Über die Dropdownliste der F1-Hilfe oder durch Klicken auf die Schaltfläche „?“ in den Attunity Consoles kann nicht auf die richtige Hilfedokumentation zugegriffen werden. Diese Methoden zeigen auf die falschen CHM-Dateien.  
  
**Problemumgehung:** Die richtigen CHM-Dateien werden zusammen mit CDC Service für Oracle und CDC Designer für Oracle installiert. Um die richtigen Hilfeinhalte anzuzeigen, starten Sie die CHM-Dateien direkt vom folgenden Ort: `%Program Files%\Change Data Capture for Oracle by Attunity\*.chm`.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="70-master-data-services"></a><a name="MDS"></a>7.0 Master Data Services  
  
### <a name="71-fixing-an-mds-installation-in-a-cluster"></a>7.1 Reparieren einer MDS-Installation in einem Cluster  
**Problem:** Wenn Sie eine gruppierte Instanz der RTM-Version von SQL Server 2012 installieren, während das Kontrollkästchen **Master Data Services aktiviert** ist, wird MDS auf einem einzelnen Knoten installiert, ist jedoch auf zusätzlichen Knoten, die Sie dem Cluster hinzufügen, weder verfügbar noch funktionsfähig.  
  
**Problemumgehung**: Um dieses Problem zu beheben, müssen Sie mithilfe der folgenden Schritte die kumulative Version 1 (CU1) für SQL Server 2012 installieren:  
  
1.  Stellen Sie sicher, dass keine SQL-/MDS-Installation vorhanden ist.  
  
2.  Laden Sie SQL Server 2012 CU1 in ein lokales Verzeichnis herunter.  
  
3.  Installieren Sie SQL Server 2012 mit der MDS-Funktion auf dem primären Clusterknoten, und installieren Sie anschließend SQL Server 2012 mit der MDS-Funktion auf zusätzlichen Clusterknoten.  
  
Weitere Informationen zu diesen Problemen sowie zum Ausführen der oben angegebenen Schritte finden Sie unter [https://support.microsoft.com/kb/2683467](https://support.microsoft.com/kb/2683467).  
  
### <a name="72-microsoft-silverlight-5-required"></a>7.2 Microsoft Silverlight 5 ist erforderlich  
Um in der Master Data Manager-Webanwendung arbeiten zu können, muss Silverlight 5.0 auf dem Clientcomputer installiert sein. Falls Sie nicht über die erforderliche Version von Silverlight verfügen, werden Sie aufgefordert, diese zu installieren, wenn Sie zu einem Bereich der Webanwendung navigieren, in dem sie erforderlich ist. Sie können Silverlight 5 von [https://go.microsoft.com/fwlink/?LinkId=243096](https://go.microsoft.com/fwlink/?LinkId=243096) installieren.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="80-reporting-services"></a><a name="RS"></a>8.0 Reporting Services  
  
### <a name="81-reporting-services-connectivity-to-sql-server-pdw-requires-updated-drivers"></a>8.1 Reporting Services-Verbindungen mit SQL Server-PDW erfordern aktualisierte Treiber  
Verbindungen von SQL Server 2012 Reporting Services mit Microsoft SQL Server PDW Appliance Update 2 und höher erfordern, dass die PDW-Verbindungstreiber aktualisiert werden. Weitere Informationen erhalten SQL Server-PDW-Kunden von Microsoft Support Services.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="90-streaminsight"></a><a name="SI"></a>9.0 StreamInsight  
SQL Server 2012 enthält StreamInsight 2.1. StreamInsight 2.1 erfordert eine Microsoft SQL Server 2012-Lizenz sowie .NET Framework 4.0. Die Software umfasst eine Reihe von Leistungsverbesserungen und Programmfehlerbehebungen. Weitere Informationen finden Sie in den [Versionsanmerkungen zu Microsoft StreamInsight 2.1](https://social.technet.microsoft.com/wiki/contents/articles/6539.aspx). Um StreamInsight 2.1 separat herunterzuladen, besuchen Sie im Microsoft Download Center die [Seite zum Download von Microsoft StreamInsight 2.1](https://www.microsoft.com/download/details.aspx?id=30149).  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="100-upgrade-advisor"></a><a name="UA"></a>10.0 Upgrade Advisor  
  
### <a name="101-link-to-install-upgrade-advisor-is-not-enabled-on-chinese-hk-operating-systems"></a>10.1 Link zur Installation von Upgrade Advisor ist auf chinesischen (HK) Betriebssystemen nicht aktiviert  
Problem: Beim Versuch, Upgrade Advisor auf einer unterstützten Windows-Version unter chinesischen (Hongkong, Sonderverwaltungszone) Betriebssystemen zu installieren, ist der Link zur Installation von Upgrade Advisor möglicherweise nicht aktiviert.  
  
**Problemumgehung**: Suchen Sie die Datei **SQLUA.msi** auf den SQL Server 2012-Medien unter `\1028_CHT_LP\x64\redist\Upgrade Advisor` oder `\1028_CHT_LP\x86\redist\Upgrade Advisor`, je nach Architektur des Betriebssystems.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")
