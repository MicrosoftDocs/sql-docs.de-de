---
title: Installieren von SQL Server mit SysPrep | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Sie Images mithilfe von SysPrep in einer SQL Server-Installation vorbereiten und abschließen.
ms.custom: ''
ms.date: 09/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 11f4ed8a-aaa9-417b-bdd5-204f551c6bb6
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8bbb1d668efb9a8531d6a93b084a7315e99072ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348315"
---
# <a name="install-sql-server-with-sysprep"></a>Installieren von SQL Server mit SysPrep

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep können Sie über das Installationscenter zugreifen. Die Seite **Erweitert** des **Installationscenters** enthält zwei Optionen - **Vorbereiten eines Images von einer eigenständigen Instanz von[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** und **Abschließen eines Images von einer vorbereiteten eigenständigen Instanz von[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . In den Abschnitten zum [Vorbereiten](#prepare) und [Abschließen](#complete) wird der Installationsvorgang detailliert beschrieben. Weitere Informationen finden Sie unter [Considerations for Installing SQL Server Using SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md). 
  
Sie können außerdem eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe der Eingabeaufforderung oder einer Konfigurationsdatei vorbereiten und abschließen. Weitere Informationen finden Sie unter  
  
- [Installieren von SQL Server von der Eingabeaufforderung](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)  
  
- [Installieren von SQL Server mithilfe einer Konfigurationsdatei](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md)  
  
## <a name="prerequisites"></a>Voraussetzungen  
Bevor Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installieren, lesen Sie die Artikel in [Planen einer SQL Server-Installation](../../sql-server/install/planning-a-sql-server-installation.md). 
  
Weitere Informationen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen sowie Hardware- und Softwareanforderungen finden Sie unter [Hardware- und Softwareanforderungen für die Installation von SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). 
    
##  <a name="ssnoversion-sysprep-cluster-support"></a>SysPrep-Unterstützung für <a name="sysprep"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Cluster  
 Ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]unterstützt SysPrep gruppierte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanzen in Befehlszeileninstallationen. Weitere Informationen finden Sie unter [Erläuterungen zu SysPrep](https://msdn.microsoft.com/library/cc721940\(v=WS.10\).aspx). 
  
### <a name="to-prepare-a-ssnoversion-failover-cluster-unattended"></a>So bereiten Sie einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Failovercluster vor (unbeaufsichtigt)  
  
1. Bereiten Sie das Image (wie unter [Considerations for Installing SQL Server Using SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md)(Überlegungen zum Installieren von SQL Server mit SysPrep) erläutert) vor, und zeichnen Sie das Windows-Image mithilfe der Generalisierung mit SysPrep. Im folgenden Beispiel wird das Image vorbereitet:  
  
    ```  
    Setup.exe /q /ACTION=PrepareImage l /FEATURES=SQLEngine /InstanceID =<MYINST> /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
     Führen Sie dann Windows SysPrep mit der generalize-Option aus. 
  
2. Stellen Sie das Image bereit, indem Sie Windows SysPrep mit der specialize-Option ausführen. 
  
3. Erstellen Sie den Windows-Failovercluster. 
  
4. Führen Sie „setup.exe“ mit dem Flag **/ACTION=PrepareFailoverCluster** für alle Knoten aus. Beispiel:  
  
    ```  
    setup.exe /q /ACTION=PrepareFailoverCluster /InstanceName=<InstanceName> /Features=SQLEngine  /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx"  /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
### <a name="complete-a-ssnoversion-failover-cluster-unattended"></a>Abschließen eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failoverclusters (unbeaufsichtigt)  
  
1. Führen Sie „setup.exe“ mit dem Flag **/ACTION=CompleteFailoverCluster** für den Knoten aus, der Besitzer der verfügbaren Speichergruppe ist:  
  
    ```  
    setup.exe /q /ACTION=CompleteFailoverCluster /InstanceName=<InstanceName>  /FAILOVERCLUSTERDISKS="<Cluster Disk Resource Name - for example, 'Disk S:'>:" /FAILOVERCLUSTERNETWORKNAME="<Insert FOI Network Name>" /FAILOVERCLUSTERIPADDRESSES="IPv4;xx.xxx.xx.xx;Cluster Network;xxx.xxx.xxx.x" /FAILOVERCLUSTERGROUP="MSSQLSERVER" /INSTALLSQLDATADIR="<Drive>:\<Path>\MSSQLSERVER" /SQLCOLLATION="SQL_Latin1_General_CP1_CS_AS" /SQLSYSADMINACCOUNTS="<DomainName\UserName>"  
    ```  
  
### <a name="adding-a-node-to-an-existing-ssnoversion-failover-cluster-unattended"></a>Hinzufügen eines Knotens zu einem vorhandenen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failovercluster (unbeaufsichtigt)  
  
1. Stellen Sie das Image bereit, indem Sie Windows SysPrep mit der specialize-Option ausführen. 
  
2. Treten Sie dem Windows-Failovercluster bei. 
  
3. Führen Sie „setup.exe“ mit **ACTION=AddNode** auf allen Knoten aus:  
  
    ```  
    setup.exe /q /ACTION=AddNode /InstanceName=<InstanceName> /Features=SQLEngine  /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx"  /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
##  <a name="prepare-image"></a><a name="prepare"></a> Vorbereiten eines Images  
  
### <a name="prepare-a-stand-alone-instance-of-ssnoversion"></a>Bereiten Sie eine eigenständige Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]vor. 
  
1. Legen Sie das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installationsmedium ein. Doppelklicken Sie im Stammordner auf Setup.exe. Wenn Sie eine Installation über eine Netzwerkfreigabe vornehmen möchten, suchen Sie den Stammordner in der Freigabe, und doppelklicken Sie auf setup.exe. 
  
2. Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installationscenter wird vom Installations-Assistenten ausgeführt. Um eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vorzubereiten, klicken Sie auf der Seite Erweitert auf die **Imagevorbereitung einer eigenständigen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** auf der Seite **Erweitert**. 
  
3. Die Systemkonfigurationsprüfung führt einen Ermittlungsvorgang auf dem Computer aus. Klicken Sie zum Fortsetzen des Vorgangs auf **OK**. Sie können die Details auf dem Bildschirm anzeigen, indem Sie auf **Details anzeigen** klicken, oder als HTML-Bericht, indem Sie auf **Detaillierten Bericht anzeigen** klicken. 
  
4. Auf der Seite für Produktupdates werden die neuesten verfügbaren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Produktupdates angezeigt. Wenn Sie die Updates nicht einschließen möchten, deaktivieren Sie das Kontrollkästchen **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Produktupdates einschließen**. Wenn keine Produktupdates ermittelt wurden, zeigt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup diese Seite nicht an und geht automatisch zur Seite **Setupdateien installieren** über. 
  
5. Auf der Seite Setupdateien installieren wird der Status angezeigt, während die Setupdateien heruntergeladen, extrahiert und installiert werden. Wenn ein Update für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup gefunden und angegeben wird, dass das Update eingeschlossen werden soll, wird dieses Update ebenfalls installiert. 
  
6. Die Systemkonfigurationsprüfung überprüft den Systemstatus des Computers, bevor Setup fortgesetzt wird. Sie können die Details auf dem Bildschirm anzeigen, indem Sie auf **Details anzeigen** klicken, oder als HTML-Bericht, indem Sie auf **Detaillierten Bericht anzeigen** klicken. 
  
7. Wählen Sie auf der Seite **Typ der Imagevorbereitung** die Option **Neue Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vorbereiten** aus. 
  
     Die Seite **Typ der Imagevorbereitung** wird nur angezeigt, wenn auf dem Computer eine nicht konfigurierte vorbereitete Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vorhanden ist. Sie können wahlweise eine neue Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vorbereiten oder einer vorhandenen vorbereiteten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz auf dem Computer von SysPrep unterstützte Funktionen hinzufügen. Weitere Informationen zum Hinzufügen von Funktionen zu einer vorbereiteten Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] finden Sie im Thema zum [Hinzufügen von Funktionen zu einer vorbereiteten Instanz](#AddFeatures). 
  
8. Lesen Sie auf der Seite **Lizenzbedingungen** den Lizenzvertrag, und aktivieren Sie dann das Kontrollkästchen, um den Lizenzbestimmungen zuzustimmen. Falls Sie zur Verbesserung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]beitragen möchten, können Sie auch die Option zur Funktionsverwendung aktivieren und Berichte an [!INCLUDE[msCoName](../../includes/msconame-md.md)]senden. 
  
9. Wählen Sie auf der Seite **Funktionsauswahl** die Komponenten für die Installation aus:  
  
    |Installation|Komponenten|  
    |-|-|  
    |[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] SysPrep|[!INCLUDE[ssDE](../../includes/ssde-md.md)]<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Replikation<br /><br /> Volltextfunktionen<br /><br /> Data Quality Services<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] im einheitlichen Modus<br /><br /> [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]<br /><br /> Weitervertreibbare Funktionen<br /><br /> Freigegebene Funktionen|  
  
     Wenn Sie einen Funktionsnamen hervorheben, wird im rechten Bereich eine Beschreibung für die jeweilige Komponentengruppe angezeigt. Sie können jede beliebige Kombination von Kontrollkästchen aktivieren. Weitere Informationen finden Sie unter [Von den SQL Server 2017-Editionen unterstützte Features](../../sql-server/editions-and-components-of-sql-server-2017.md) und [SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md).
  
     Die erforderlichen Komponenten für die ausgewählten Funktionen werden im rechten Bereich angezeigt. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup installiert die erforderlichen Komponenten, die nicht bereits während des Installationsschritts installiert werden, der im weiteren Verlauf dieser Prozedur beschrieben wird. 
  
10. Auf der Seite **Regeln zum Vorbereiten des Images** überprüft die Systemkonfigurationsprüfung den Systemstatus Ihres Computers, bevor Setup fortfährt. Sie können die Details auf dem Bildschirm anzeigen, indem Sie auf **Details anzeigen** klicken, oder als HTML-Bericht, indem Sie auf **Detaillierten Bericht anzeigen** klicken. 
  
11. Geben Sie auf der Instanzkonfigurationsseite die Instanz-ID für die Instanz an. Klicken Sie auf zum Fortfahren auf **Weiter**. 
  
     **Instanz-ID**: Die Instanz-ID wird verwendet, um Installationsverzeichnisse und Registrierungsschlüssel für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu identifizieren. Dies ist der Fall für Standardinstanzen und benannte Instanzen. Wenn die vorbereitete Instanz während des Schritts "Abschließen" als Standardinstanz abgeschlossen wird, wird der Instanzname mit MSSQLSERVER überschrieben. Die Instanz-ID bleibt die gleiche wie angegeben. 
  
     **Instanzstammverzeichnis**: Standardmäßig lautet das Instanzstammverzeichnis [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]. Verwenden Sie das dafür vorgesehene Feld, um ein nicht standardmäßiges Stammverzeichnis anzugeben. Alternativ können Sie auf **Durchsuchen** klicken, um einen Installationsordner zu suchen. Das im Vorbereitungsschritt angegebene Verzeichnis wird während der Konfiguration im Schritt "Abschließen" verwendet. 
  
     Alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Service Packs und Updates werden für jede Komponente einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]übernommen. 
  
     **Installierte Instanzen**: Im Raster werden Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angezeigt, die sich auf dem Computer befinden, auf dem das Setup ausgeführt wird. 
  
12. Auf der Seite **Erforderlicher Speicherplatz** wird der für die angegebenen Funktionen erforderliche Speicherplatz berechnet. Der erforderliche Speicherplatz wird dann mit dem verfügbaren Speicherplatz auf dem Computer verglichen. 
  
13. Die Systemkonfigurationsprüfung führt Regeln für die Imagevorbereitung aus, um die Konfiguration des Computers anhand der von Ihnen angegebenen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Funktionen zu überprüfen. Sie können die Details auf dem Bildschirm anzeigen, indem Sie auf **Details anzeigen** klicken, oder als HTML-Bericht, indem Sie auf **Detaillierten Bericht anzeigen** klicken. 
  
14. Auf der Seite **Das Image kann jetzt vorbereitet werden** wird eine Strukturansicht der Installationsoptionen angezeigt, die in Setup angegeben wurden. Auf dieser Setupseite ist neben der letzten Updateversion auch angegeben, ob die Produktupdatefunktion aktiviert oder deaktiviert ist. Klicken Sie zum Fortsetzen des Vorgangs auf **Vorbereiten**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup installiert zuerst die erforderlichen Komponenten für die ausgewählten Funktionen und dann die Funktionen. 
  
15. Während der Installation wird auf der Seite **Status der Imagevorbereitung** der Status angezeigt, sodass Sie während der Ausführung von Setup den Installationsstatus überwachen können. 
  
16. Nach der Installation bietet die Seite **Abgeschlossen** einen Link zur zusammenfassenden Protokolldatei für die Installation und andere wichtige Hinweise. Klicken Sie auf **Schließen**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , um die Installation von abzuschließen. 
  
17. Starten Sie den Computer neu, falls Sie dazu aufgefordert werden. Wenn Sie den Setupvorgang abgeschlossen haben, sollten Sie unbedingt die vom Installations-Assistenten angezeigte Meldung lesen. Weitere Informationen finden Sie unter [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md). 
  
18. Hiermit wird der Vorbereitungsschritt abgeschlossen. Sie können das Image abschließen oder das vorbereitete Image bereitstellen, wie unter [Considerations for Installing SQL Server Using SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md)beschrieben. 
  
##  <a name="complete-image"></a><a name="complete"></a> Abschließen des Images  
  
### <a name="complete-a-prepared-instance-of-ssnoversion"></a>Abschließen einer vorbereiteten Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Wenn in das Image Ihres Computers eine vorbereitete Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eingeschlossen ist, wird im Startmenü eine Verknüpfung angezeigt. Sie können auch das Installationscenter starten und auf der Seite **Erweitert** auf **Abschließen eines Images von einer vorbereiteten eigenständigen Instanz** klicken. 
  
2. Die Systemkonfigurationsprüfung führt einen Ermittlungsvorgang auf dem Computer aus. Klicken Sie zum Fortsetzen des Vorgangs auf **OK**. Sie können die Details auf dem Bildschirm anzeigen, indem Sie auf **Details anzeigen** klicken, oder als HTML-Bericht, indem Sie auf **Detaillierten Bericht anzeigen** klicken. 
  
3. Klicken Sie auf der Seite **Setup-Unterstützungsdateien** auf **Installieren** , um die Setup-Unterstützungsdateien zu installieren. 
  
4. Die Systemkonfigurationsprüfung überprüft den Systemstatus des Computers, bevor Setup fortgesetzt wird. Nachdem die Prüfung abgeschlossen wurde, klicken Sie auf **Weiter** , um den Vorgang fortzusetzen. Sie können die Details auf dem Bildschirm anzeigen, indem Sie auf **Details anzeigen** klicken, oder als HTML-Bericht, indem Sie auf **Detaillierten Bericht anzeigen** klicken. 
  
5. Wählen Sie auf der Seite **Product Key** ein Optionsfeld aus, um anzugeben, ob Sie eine kostenlose Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]installieren oder ob Sie über eine Produktionsversion des Produkts mit einem PID-Schlüssel verfügen. Weitere Informationen finden Sie unter [Editionen und unterstützte Funktionen von SQL Server](../../sql-server/editions-and-components-of-sql-server-2017.md). Wenn Sie Evaluation Edition installieren, beginnt der 180-Tage-Testzeitraum mit dem Abschließen dieses Schritts. 
  
6. Lesen Sie auf der Seite **Lizenzbedingungen** den Lizenzvertrag, und aktivieren Sie dann das Kontrollkästchen, um den Lizenzbestimmungen zuzustimmen. Falls Sie zur Verbesserung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]beitragen möchten, können Sie auch die Option zur Funktionsverwendung aktivieren und Berichte an [!INCLUDE[msCoName](../../includes/msconame-md.md)]senden. 
  
7. Wählen Sie auf der Seite **Vorbereitete Instanz auswählen** im Dropdownfeld die vorbereitete Instanz aus, die Sie abschließen möchten. Wählen Sie die nicht konfigurierte Instanz aus der Liste **Instanz-ID** aus. 
  
     **Installierte Instanzen:** Dieses Raster zeigt alle Instanzen einschließlich der vorbereiteten Instanzen auf diesem Computer an. 
  
8. Auf der Seite **Überprüfung der Funktionen** werden die ausgewählten Funktionen und Komponenten angezeigt, die während des Vorbereitungsschritts in die Installation eingeschlossen wurden. Wenn Sie der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz weitere Funktionen hinzufügen möchten, die nicht in die vorbereitete Instanz eingeschlossen sind, müssen Sie zunächst diesen Schritt abschließen, um die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz fertig zu stellen, und anschließend die Funktionen im **Installationscenter** im Bereich **Funktionen hinzufügen** hinzufügen. 
  
    > [!NOTE]  
    >  Sie können Funktionen hinzufügen, die für die Produktversion, die Sie installieren, verfügbar sind. Weitere Informationen finden Sie unter [Editionen und unterstützte Funktionen von SQL Server](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
9. Geben Sie auf der Instanzkonfigurationsseite den Instanznamen für die vorbereitete Instanz an. Nach Abschluss der Konfiguration von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ist dies der Name der Instanz. Klicken Sie auf zum Fortfahren auf **Weiter**. 
  
     **Instanz-ID**: Die Instanz-ID wird verwendet, um Installationsverzeichnisse und Registrierungsschlüssel für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu identifizieren. Dies ist der Fall für Standardinstanzen und benannte Instanzen. Wenn die vorbereitete Instanz während des Schritts "Abschließen" als Standardinstanz abgeschlossen wird, wird der Instanzname mit MSSQLSERVER überschrieben. Die Instanz-ID bleibt die gleiche wie während des Vorbereitungsschritts angegeben. 
  
     **Instanzstammverzeichnis**: Das im Vorbereitungsschritt angegebene Verzeichnis wird verwendet und kann in diesem Schritt nicht geändert werden. 
  
     Alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Service Packs und Updates werden für jede Komponente einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]übernommen. 
  
     **Installierte Instanzen** – Im Raster werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanzen angezeigt, die sich auf dem Computer befinden, auf dem Setup ausgeführt wird. 
  
10. Der Ablauf für die weiteren Vorgänge in diesem Artikel richtet sich nach den Funktionen, die Sie während des Vorbereitungsschritts ausgewählt haben. Je nach Auswahl werden möglicherweise nicht alle Seiten angezeigt. 
  
11. Geben Sie auf der Seite **Serverkonfiguration > Dienstkonten** Anmeldekonten für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienste an. Welche Dienste tatsächlich auf dieser Seite konfiguriert werden, ist von den Funktionen abhängig, die Sie für die Installation ausgewählt haben. 
  
     Sie können allen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Diensten dasselbe Anmeldekonto zuweisen, oder Sie können jedes Dienstkonto einzeln konfigurieren. Außerdem können Sie angeben, ob Dienste automatisch starten sollen, manuell gestartet werden oder deaktiviert sind. [!INCLUDE[msCoName](../../includes/msconame-md.md)] , die Dienstkonten einzeln zu konfigurieren, um möglichst geringe Rechte für jeden Dienst bereitzustellen. Dabei erhalten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienste die Berechtigungen, die mindestens erforderlich ist, um ihre Tasks auszuführen. Weitere Informationen finden Sie unter [Serverkonfiguration – Dienstkonten](./install-sql-server.md) und [Konfigurieren von Windows-Dienstkonten und -Berechtigungen](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md). 
  
     Um für alle Dienstkonten in dieser Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]dasselbe Anmeldekonto anzugeben, geben Sie in den Feldern unten auf dieser Seite die entsprechenden Anmeldeinformationen ein. 
  
     **Sicherheitshinweis:** [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
     Wenn Sie die Angabe der Anmeldeinformationen für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienste abgeschlossen haben, klicken Sie auf **Weiter**. 
  
12. Verwenden Sie die Registerkarte **Serverkonfiguration – Sortierung** , um nicht standardmäßige Sortierungen für das [!INCLUDE[ssDE](../../includes/ssde-md.md)] und [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]anzugeben. Weitere Informationen finden Sie unter [Serverkonfiguration – Sortierung](./install-sql-server.md). 
  
13. Geben Sie auf der Seite Konfiguration des [!INCLUDE[ssDE](../../includes/ssde-md.md)] s – Kontenbereitstellung folgende Informationen an:  
  
    - Sicherheitsmodus – Wählen Sie die Windows-Authentifizierung oder die Authentifizierung im gemischten Modus für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]aus. Bei Auswahl des gemischten Authentifizierungsmodus müssen Sie ein sicheres Kennwort für das integrierte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Systemadministratorkonto angeben. 
  
         Nachdem ein Gerät erfolgreich eine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]hergestellt hat, wird für die Windows-Authentifizierung und den gemischten Modus derselbe Sicherheitsmechanismus verwendet. Weitere Informationen finden Sie unter [Konfiguration der Datenbank-Engine – Serverkonfiguration](./install-sql-server.md). 
  
    - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Administratoren – Sie müssen mindestens einen Systemadministrator für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz angeben. Um das Konto hinzuzufügen, unter dem das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup ausgeführt werden soll, klicken Sie auf **Aktuellen Benutzer hinzufügen**. Um Konten zur Liste der Systemadministratoren hinzuzufügen bzw. daraus zu entfernen, klicken Sie auf **Hinzufügen** bzw. **Entfernen**, und bearbeiten Sie anschließend die Liste der Benutzer, Gruppen bzw. Computer, die Administratorrechte für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz haben sollen. Weitere Informationen finden Sie unter [Konfiguration der Datenbank-Engine – Serverkonfiguration](./install-sql-server.md). 
  
     Wenn Sie die Bearbeitung der Liste abgeschlossen haben, klicken Sie auf **OK**. Überprüfen Sie die Liste der Administratoren im Konfigurationsdialogfeld. Sobald die Liste vollständig ist, klicken Sie auf **Weiter**. 
  
14. Geben Sie ggf. auf der Seite [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Konfiguration – Datenverzeichnisse andere Installationsverzeichnisse als das Standardinstallationsverzeichnis an. Wenn die Installation in Standardverzeichnissen erfolgen soll, klicken Sie auf **Weiter**. 
  
    > [!IMPORTANT]  
    >  Wenn Sie keine Standardverzeichnisse angeben, sollten Sie sicherstellen, dass die Installationsordner nur für die Instanz [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verwendet werden. Keines der Verzeichnisse in diesem Dialogfeld sollte gemeinsam mit Verzeichnissen anderer Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]genutzt werden. 
  
     Weitere Informationen finden Sie unter [Konfiguration der Datenbank-Engine – Serverkonfiguration](./install-sql-server.md). 
  
15. Aktivieren Sie auf der Seite „ [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Konfiguration – FILESTREAM“ den FILESTREAM für Ihre Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Weitere Informationen finden Sie unter [Konfiguration der Datenbank-Engine - Filestream](./install-sql-server.md). 
  
16. Auf der Seite für die Konfiguration von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] können Sie die Art der zu erstellenden [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Installation angeben. Weitere Informationen zu [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Konfigurationsmodi finden Sie unter [Reporting Services-Konfigurationsoptionen &#40;SSRS&#41;](./install-sql-server.md). 
  
17. Geben Sie auf der Seite **Fehlerberichterstellung** die Informationen an, die Sie an [!INCLUDE[msCoName](../../includes/msconame-md.md)] senden möchten, um zur Verbesserung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]beizutragen. Die Option für Fehlerberichte ist standardmäßig aktiviert. 
  
18. Auf der Seite **Regeln zum Abschließen des Images** führt die Systemkonfigurationsprüfung alle Imageregeln vollständig aus, um die Konfiguration des Computers anhand der von Ihnen angegebenen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurationen zu überprüfen. Sie können die Details auf dem Bildschirm anzeigen, indem Sie auf **Details anzeigen** klicken, oder als HTML-Bericht, indem Sie auf **Detaillierten Bericht anzeigen** klicken. 
  
19. Auf der Seite **Das Image kann jetzt abgeschlossen werden** wird eine Strukturansicht der Installationsoptionen angezeigt, die in Setup angegeben wurden. Klicken Sie zum Fortsetzen des Vorgangs auf **Installieren**. 
  
20. Während der Installation wird auf der Seite **Status des Imageabschlusses** der Status angezeigt, sodass Sie während der Ausführung von Setup den Installationsstatus überwachen können. 
  
21. Nach der Installation bietet die Seite **Abgeschlossen** einen Link zur zusammenfassenden Protokolldatei für die Installation und andere wichtige Hinweise. Klicken Sie auf **Schließen**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , um die Installation von abzuschließen. 
  
22. Starten Sie den Computer neu, falls Sie dazu aufgefordert werden. Wenn Sie den Setupvorgang abgeschlossen haben, sollten Sie unbedingt die vom Installations-Assistenten angezeigte Meldung lesen. Weitere Informationen finden Sie unter [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md). 
  
23. In diesem Schritt wird die Konfiguration der vorbereiteten Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abgeschlossen. Damit ist die Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]abgeschlossen. 
  
##  <a name="add-features-to-a-prepared-instance"></a><a name="AddFeatures"></a> Add Features to a Prepared Instance  
  
### <a name="add-features-to-a-prepared-instance-of-ssnoversion"></a>Hinzufügen von Funktionen zu einer vorbereiteten Instanz [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Legen Sie das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installationsmedium ein. Doppelklicken Sie im Stammordner auf Setup.exe. Wenn Sie eine Installation über eine Netzwerkfreigabe vornehmen möchten, suchen Sie den Stammordner in der Freigabe, und doppelklicken Sie auf setup.exe. 
  
2. Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installationscenter wird vom Installations-Assistenten ausgeführt. Um einer vorbereiteten Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Funktionen hinzuzufügen, klicken Sie auf der Seite **Erweitert** auf **Vorbereiten eines Images von einer eigenständigen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . 
  
3. Die Systemkonfigurationsprüfung führt einen Ermittlungsvorgang auf dem Computer aus. Klicken Sie zum Fortsetzen des Vorgangs auf **OK**. Sie können die Details auf dem Bildschirm anzeigen, indem Sie auf **Details anzeigen** klicken, oder als HTML-Bericht, indem Sie auf **Detaillierten Bericht anzeigen** klicken. 
  
4. Klicken Sie auf der Seite **Setup-Unterstützungsdateien** auf Installieren, um die Setup-Unterstützungsdateien zu installieren. 
  
5. Wählen Sie auf der Seite **Typ der Imagevorbereitung** die Option **Funktionen zu einer vorhandenen vorbereiteten Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hinzufügen** aus. Wählen Sie in der Dropdownliste verfügbarer vorbereiteter Instanzen die vorbereitete Instanz aus, der Sie Funktionen hinzufügen möchten. 
  
6. Geben Sie auf der Seite **Funktionsauswahl** die Funktionen an, die Sie der angegebenen vorbereiteten Instanz hinzufügen möchten. 
  
     Die erforderlichen Komponenten für die ausgewählten Funktionen werden im rechten Bereich angezeigt. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup installiert die erforderlichen Komponenten, die nicht bereits während des Installationsschritts installiert werden, der im weiteren Verlauf dieser Prozedur beschrieben wird. 
  
7. Auf der Seite **Regeln zum Vorbereiten des Images** überprüft die Systemkonfigurationsprüfung den Systemstatus Ihres Computers, bevor Setup fortfährt. Sie können die Details auf dem Bildschirm anzeigen, indem Sie auf **Details anzeigen** klicken, oder als HTML-Bericht, indem Sie auf **Detaillierten Bericht anzeigen** klicken. 
  
8. Auf der Seite Erforderlicher Speicherplatz wird der für die angegebenen Funktionen erforderliche Speicherplatz berechnet. Der erforderliche Speicherplatz wird dann mit dem verfügbaren Speicherplatz auf dem Computer verglichen. 
  
9. Auf der Seite **Regeln zum Vorbereiten des Images** führt die Systemkonfigurationsprüfung Regeln für die Imagevorbereitung aus, um die Konfiguration des Computers anhand der von Ihnen angegebenen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Funktionen zu überprüfen. Sie können die Details auf dem Bildschirm anzeigen, indem Sie auf **Details anzeigen** klicken, oder als HTML-Bericht, indem Sie auf **Detaillierten Bericht anzeigen** klicken. 
  
10. Auf der Seite **Das Image kann jetzt vorbereitet werden** wird eine Strukturansicht der Installationsoptionen angezeigt, die in Setup angegeben wurden. Klicken Sie zum Fortsetzen des Vorgangs auf **Installieren**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup installiert zuerst die erforderlichen Komponenten für die ausgewählten Funktionen und dann die Funktionen. 
  
11. Während der Installation wird auf der Seite **Status der Imagevorbereitung** der Status angezeigt, sodass Sie während der Ausführung von Setup den Installationsstatus überwachen können. 
  
12. Nach der Installation bietet die Seite **Abgeschlossen** einen Link zur zusammenfassenden Protokolldatei für die Installation und andere wichtige Hinweise. Klicken Sie auf **Schließen**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , um die Installation von abzuschließen. 
  
13. Starten Sie den Computer neu, falls Sie dazu aufgefordert werden. Wenn Sie den Setupvorgang abgeschlossen haben, sollten Sie unbedingt die vom Installations-Assistenten angezeigte Meldung lesen. Weitere Informationen finden Sie unter [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md). 
  
##  <a name="remove-features-from-a-prepare-instance"></a><a name="RemoveFeatures"></a> Entfernen von Funktionen aus einer vorbereiten Instanz  
  
### <a name="removing-features-from-a-prepared-instance-of-ssnoversion"></a>Entfernen von Funktionen aus einer vorbereiteten Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Beginnen Sie den Deinstallationsvorgang, indem Sie im Menü **Start** auf **Systemsteuerung** klicken und danach auf **Programme und Funktionen** doppelklicken. 
  
2. Doppelklicken Sie auf die zu deinstallierende [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Komponente, und klicken Sie dann auf **Entfernen**. 
  
3. Unterstützungsregeln für Setup werden ausgeführt, um die Computerkonfiguration zu überprüfen. Klicken Sie zum Fortsetzen des Vorgangs auf **OK** . 
  
4. Wählen Sie auf der Seite **Instanz auswählen** die vorbereitete Instanz aus, die Sie ändern möchten. Der Name der vorbereiteten Instanz wird als "Nicht konfiguriert PreparedInstanceID" angezeigt, wobei PreparedInstanceID die Instanz ist, die Sie auswählen. 
  
5. Geben Sie auf der Seite **Funktionen auswählen** die Funktionen an, die aus der angegebenen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entfernt werden sollen. Klicken Sie auf zum Fortfahren auf **Weiter**. 
  
6. Entfernungsregeln werden ausgeführt, um zu überprüfen, ob der Vorgang erfolgreich abgeschlossen werden kann. 
  
7. Überprüfen Sie auf der Seite **Deinstallation kann jetzt ausgeführt werden** die Liste der Komponenten und Funktionen, die deinstalliert werden sollen. 
  
8. Auf der Seite **Status des Entfernungsvorgangs** wird der Status des Vorgangs angezeigt. 
  
9. Auf der Seite **Abgeschlossen** können Sie den Abschlussstatus des Vorgangs überprüfen. Klicken Sie auf **Schließen** , um den Installations-Assistenten zu beenden. 
  
##  <a name="uninstalling-a-prepared-instance"></a><a name="Uninstall"></a> Deinstallieren einer vorbereiteten Instanz  
  
### <a name="uninstall-a-prepared-instance-of-ssnoversion"></a>Deinstallieren einer vorbereiteten Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Beginnen Sie den Deinstallationsvorgang, indem Sie im Menü **Start** auf **Systemsteuerung** klicken und danach auf **Programme und Funktionen** doppelklicken. 
  
2. Doppelklicken Sie auf die zu deinstallierende [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Komponente, und klicken Sie dann auf **Entfernen**. 
  
3. Unterstützungsregeln für Setup werden ausgeführt, um die Computerkonfiguration zu überprüfen. Klicken Sie zum Fortsetzen des Vorgangs auf **OK** . 
  
4. Wählen Sie auf der Seite **Instanz auswählen** die vorbereitete Instanz aus, die Sie ändern möchten. Der Name der vorbereiteten Instanz wird als "Nicht konfiguriert PreparedInstanceID" angezeigt, wobei PreparedInstanceID die Instanz ist, die Sie auswählen. 
  
5. Geben Sie auf der Seite **Funktionen auswählen** die Funktionen an, die aus der angegebenen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entfernt werden sollen. Klicken Sie auf zum Fortfahren auf **Weiter**. 
  
6. Auf der Seite **Entfernungsregeln** führt Setup Regeln aus, um zu überprüfen, ob der Vorgang erfolgreich abgeschlossen werden kann. 
  
7. Überprüfen Sie auf der Seite **Deinstallation kann jetzt ausgeführt werden** die Liste der Komponenten und Funktionen, die deinstalliert werden sollen. 
  
8. Auf der Seite **Status des Entfernungsvorgangs** wird der Status des Vorgangs angezeigt. 
  
9. Auf der Seite **Abgeschlossen** können Sie den Abschlussstatus des Vorgangs überprüfen. Klicken Sie auf **Schließen** , um den Installations-Assistenten zu beenden. 
  
10. Wiederholen Sie die Schritte 1 bis 9, bis alle Komponenten von [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] entfernt wurden. 
  
##  <a name="modifying-or-uninstalling-a-completed-instance-of-ssnoversion"></a><a name="bk_Modifying_Uninstalling"></a> Ändern oder Deinstallieren einer abgeschlossenen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 
 Das Verfahren zum Hinzufügen oder Entfernen von Funktionen sowie zum Deinstallieren einer abgeschlossenen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ähnelt dem Verfahren für eine installierte Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Weitere Informationen finden Sie in den folgenden Artikeln:  
  
- [Add Features to an Instance of SQL Server &#40;Setup&#41; (Hinzufügen von Funktionen zu einer Instanz von SQL Server &#40;Setup&#41;)](./add-features-to-an-instance-of-sql-server-setup.md)  
  
- [Vorgehensweise: Deinstallieren einer vorhandenen SQL Server-Instanz &#40;Setup&#41;](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Was ist Windows SysPrep?](/previous-versions/windows/it-pro/windows-vista/cc721940(v=ws.10))   
 [Wie funktioniert Windows SysPrep?](/previous-versions/windows/it-pro/windows-vista/cc766514(v=ws.10))  
  
