---
title: Installation und Konfiguration
description: Erfahren Sie, wie Sie Master Data Services auf einem Windows Server 2012 R2-Computer installieren, die MDS-Datenbank und-Website konfigurieren und die Beispiel Modelle und-Daten bereitstellen.
ms.custom: ''
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: quickstart
ms.assetid: f6cd850f-b01b-491f-972c-f966b9fe4190
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 630ee8396c3f4f64e7eee5335484299f5bebfd42
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350707"
---
# <a name="master-data-services-installation-and-configuration"></a>Master Data Services-Installation und -Konfiguration

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dieser Artikel behandelt die Installation von [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] auf einem Computer mit Windows Server 2012 R2, die Einrichtung der MDS-Datenbank und -Website und die Bereitstellung der Beispielmodelle und -daten. [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] (MDS) ermöglicht Ihrer Organisation, eine vertrauenswürdige Version der Daten zu verwalten.   
  
> [!NOTE] 
> Sie können [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] auf einem Windows 10-Computer installieren, wenn Sie die Developer-Edition verwenden, die jetzt [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] unterstützt. 
>>Weitere Informationen zur Betriebssystemunterstützung für unterschiedliche [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)]-Editionen finden Sie unter [Hardware- und Softwareanforderungen für die Installation von SQL Server 2016](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). 

Einen Überblick darüber, wie Sie Daten in [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]organisieren, finden Sie unter [Übersicht über Master Data Services (MDS)](../master-data-services/master-data-services-overview-mds.md).     
  
 Informationen zu den neuen Funktionen in [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] finden Sie unter [Neues in Master Data Services &#40;MDS&#41;](../master-data-services/what-s-new-in-master-data-services-mds.md).  
 
Links zu Videos und Ressourcen zum Erlernen von [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]finden Sie unter [Erlernen von SQL Master Data Services](../master-data-services/learn-sql-server-master-data-services.md). 
  
> **Download**  
> -   Navigieren Sie zum Herunterladen von [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]zum  **[Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-ctp/)** .  
> -   Sie haben ein Azure-Konto?  Wechseln Sie anschließend **[hierhin](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm)**, um einen virtuellen Computer zu starten, auf dem SQL Server bereits installiert ist.  
> 
> **Sie können keine MDS-Website erstellen?**
> >Lesen Sie diesen Microsoft Support-Artikel für Anweisungen zum Lösen dieses Problems.
> [Können Sie keine MDS-Website über ein Konto mit geringen Rechten in SQL Server 2016 erstellen?](https://aka.ms/mdssupport) 

## <a name="internet-explorer-and-silverlight"></a>Internet Explorer und Silverlight
- Wenn Sie [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] auf einem Computer mit Windows Server 2012 installieren, müssen Sie möglicherweise die verstärkte Sicherheit von Internet Explorer konfigurieren, um die Skripterstellung für die Webanwendungswebsite zuzulassen. Andernfalls kann auf dem Servercomputer Website nicht aufgerufen werden.
- Um in der Webanwendung zu arbeiten, muss Silverlight 5 auf dem Clientcomputer installiert sein. Falls Sie nicht über die erforderliche Version von Silverlight verfügen, werden Sie aufgefordert, diese zu installieren, wenn Sie zu einem Bereich der Webanwendung navigieren, in dem sie erforderlich ist. Sie können Silverlight 5 von **[hier](https://www.microsoft.com/silverlight/)** installieren.

## <a name="ssmdsshort_md-on-an-azure-virtual-machine"></a>[!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] auf einem virtuellen Azure-Computer
Wenn Sie einen virtuellen Azure-Computer einrichten, auf dem [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)] bereits installiert ist, [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] wird standardmäßig ebenfalls installiert. 

Als nächstes müssen Sie Internet Information Services (IIS) installieren. Weitere Informationen finden Sie im Abschnitt [Installieren und Konfigurieren von IIS](#InstallIIS). 

Wenn Sie Änderungen an der Installation von [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)] vornehmen möchten, finden Sie die Datei „setup.exe“ am Standardspeicherort `<drive>`:\SQLServer_13.0_Full.
  
## <a name="installing-master-data-services"></a><a name="InstallMDS"></a> Installieren von Master Data Services  
 Zum Installieren von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] verwenden Sie den Installations-Assistenten von [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]-Setup oder eine Eingabeaufforderung.  
  
 **So installieren Sie [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] mithilfe des [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Setups auf einem Computer mit Windows Server 2012 R2**  
  
1.  Doppelklicken Sie auf Setup.exe, und befolgen Sie die Schritte im Installations-Assistenten.  
  
2.  Wählen Sie auf der Seite [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]**Funktionsauswahl** unter **Freigegebene Funktionen** aus.  
  
     Dadurch werden [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)], Assemblys, das Windows PowerShell-Snap-In sowie Ordner und Dateien für Webanwendungen und Dienste installiert.  
  
     ![mds_SQLServer2016Setup_FeatureSelection](../master-data-services/media/mds-sqlserver2016setup-featureselection.png "mds_SQLServer2016Setup_FeatureSelection")  
  
3.  Schließen Sie den Installations-Assistenten ab.  

## <a name="installing-and-configuring-iis"></a><a name="InstallIIS"></a> Installieren und Konfigurieren von IIS
  
1.  Klicken Sie in [!INCLUDE[winblue_server_2](../includes/winblue-server-2-md.md)]auf das Symbol **Server-Manager** auf der Taskleiste auf dem **Desktop**.  
  
     ![Symbol für das Server-Manager in der Windows Server 2012-Taskleiste](../master-data-services/media/mds-windowsservertaskbar-servermanagericon.png "Symbol für das Server-Manager in der Windows Server 2012-Taskleiste")  
  
5.  Klicken Sie im **Server-Manager** im Menü **Verwalten** auf **Rollen und Features hinzufügen** .  
   
     ![Menübefehl "Rollen und Features hinzufügen" in Server Verwaltung](../master-data-services/media/mds-servermanagerdashboard-addrolesfeaturesmenu.png "Menübefehl "Rollen und Features hinzufügen" in Server Verwaltung")  
  
6.  Akzeptieren Sie auf der Seite **Installationstyp** des **Assistenten zum Hinzufügen von Rollen und Features** den Standardwert (**Rollenbasierte oder featurebasierte Installation**), und klicken sie auf **Weiter**.  
  
7.  Klicken Sie auf **Einen Server aus dem Serverpool auswählen** und anschließend auf den Server, auf dem Sie [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]installiert haben.  
  
     ![mds_AddRolesFeaturesWizard_ServerSelectionPage](../master-data-services/media/mds-addrolesfeatureswizard-serverselectionpage.png) 
  
8. Klicken Sie auf der Seite **Serverrollen** auf **Webserver** und anschließend auf **Weiter**. 

   ![mds_AddRolesFeaturesWizard_ServerRolesPage](../master-data-services/media/mds-addrolesfeatureswizard-serverrolespage.png)
   
9. Bestätigen Sie auf der Seite **Funktionen**, dass die folgenden Funktionen ausgewählt sind, und klicken Sie dann auf **Weiter**. Diese Funktionen werden für [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] unter [!INCLUDE[winblue_server_2_md](../includes/winblue-server-2-md.md)] benötigt.
  
    |Funktionen|Funktionen|  
    |--------------|--------------|  
    |![mds_AddRolesFeaturesWizard_FeaturesPage](../master-data-services/media/mds-addrolesfeatureswizard-featurespage.png)|![mds_AddRolesFeaturesWizard_FeaturesPage_WindowsProcActive](../master-data-services/media/mds-addrolesfeatureswizard-featurespage-windowsprocactive.png)|  

10. Klicken Sie im linken Bereich auf **Rolle 'Webserver' (IIS)**, und klicken Sie anschließend auf **Rollendienste**.
11. Bestätigen Sie auf der Seite **Rollendienste**, dass die folgenden Dienste ausgewählt sind, und klicken Sie dann auf **Weiter**. Diese Dienste werden für [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] unter [!INCLUDE[winblue_server_2](../includes/winblue-server-2-md.md)] benötigt.

    > [!WARNING]  
    >  Installieren Sie nicht den Rollendienst WebDAV-Veröffentlichung. Die WebDAV-Veröffentlichung ist nicht mir [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]kompatibel.  
  
     |Rollendienste|Rollendienste|  
    |-----------------------------|-----------------------------|  
    |![mds_AddRolesFeaturesWizard_RoleServicesPage](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage.png)|![mds_AddRolesFeaturesWizard_RoleServicesPage_PerformSecurity](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage-performsecurity.png)|  
    |![mds_AddRolesFeaturesWizard_RoleServicesPage_AppDevsection](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage-appdevsection.png)|![mds_AddRolesFeaturesWizard_RoleServicesPage_ManageToolssection](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage-managetoolssection.png)|  
    |||  
  
     Eine Liste der benötigten Funktionen und Rollendienste bei anderen Betriebssystemen finden Sie unter [Anforderungen für die Webanwendung &#40;Master Data Services&#41;](../master-data-services/install-windows/web-application-requirements-master-data-services.md).   
  
 Weitere Informationen über die Installation [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mithilfe des Setups finden Sie unter [Installieren von SQL Server 2016 vom Installations-Assistenten aus &#40;Setup&#41;](../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md).  
  
 Weitere Informationen zur Installation von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mithilfe einer Eingabeaufforderung finden Sie unter [Installieren von SQL Server 2016 von der Eingabeaufforderung](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md). Wenn Sie eine Eingabeaufforderung verwenden, ist [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] als Funktionsparameter verfügbar.  
  
 Eine kurze Beschreibung mit Links zu weiteren Informationen zur Installationsvorbereitung finden Sie unter [Installieren von Master Data Services](../master-data-services/install-windows/install-master-data-services.md).  
  
##  <a name="setting-up-the-database-and-website"></a><a name="SetUpWeb"></a> Einrichten der Datenbank und Website  
 **So richten Sie Datenbank und Website mithilfe von [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]** ein  

 
> [!WARNING]
>  Sie müssen [IIS installieren](#InstallIIS), bevor Sie den Konfigurations-Manager für [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] starten. Andererseits zeigt der Konfigurations-Manager einen Internetinformationsdienste-Fehler an, und Sie können die [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]-Webanwendung nicht erstellen.  
> 
> **Browseranforderungen**
> >Die [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]-Webanwendung funktioniert nur in Internet Explorer 9 (IE) oder höher. IE 8 und frühere Versionen, Microsoft Edge und Chrome werden nicht unterstützt.    
  
1.  Starten Sie den [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)], und klicken im linken Bereich auf **Datenbankkonfiguration** .  
  
2.  Klicken Sie auf **Datenbank erstellen** und anschließend im **Datenbank erstellen (Assistent)** auf **Weiter**.  
  
3.  Geben Sie auf der Seite **Daten Bank Server** die SQL Server Instanz an. 

    >  [!INCLUDE[sqlv15](../includes/sssql19-md.md)] Fügt Unterstützung für SQL Server verwaltete Instanz hinzu. Legen Sie den Wert **SQL Server Instanz** auf den Host der verwalteten Instanz fest. Beispiel: `xxxxxx.xxxxxx.database.windows.net`.

4. Wählen Sie den **Authentifizierungstyp** aus, und klicken Sie dann auf **Verbindung testen** , um zu bestätigen, dass Sie mithilfe der Anmelde Informationen für den von Ihnen ausgewählten Authentifizierungstyp eine Verbindung Klicken Sie auf **Weiter**.

    >[!INCLUDE[sqlv15](../includes/sssql19-md.md)]Verwenden Sie für das Herstellen einer Verbindung mit der verwalteten Instanz einen der folgenden Authentifizierungs Typen:
    >
    >- Azure Active Directory integrierte Authentifizierung: **Aktueller Benutzer – Active Directory integriert**
    >- SQL Server Authentifizierung: **SQL Server Konto**.
    >
    >In SQL verwaltete Instanz muss der Benutzer Mitglied der `sysadmin` Server Rolle "fester Server" sein.

    > [!NOTE]  
    >  Wenn Sie als Authentifizierungstyp die Option **aktuelle Benutzer integrierte Sicherheit** auswählen, ist das Feld **Benutzername** schreibgeschützt und zeigt den Namen des Windows-Benutzerkontos an, das auf dem Computer angemeldet ist. Wenn Sie [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)] [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] auf einem virtuellen Azure-Computer (VM) ausführen, zeigt das Feld **Benutzername** den VM-Namen sowie den Benutzernamen für das lokale Administratorkonto auf der VM an. 

    ![mds_2016ConfigManager_CreateDatabaseWizard_ServerPage](../master-data-services/media/mds-2016configmanager-createdatabasewizard-serverpage.png)  
  
4.  Geben Sie im Feld **Datenbankname** einen Namen ein. Deaktivieren Sie optional das Kontrollkästchen **SQL Server-Standardsortierung**, um eine Windows-Sortierung auszuwählen und mindestens eine verfügbare Option anzugeben, z.B. **Groß-/Kleinschreibung wird beachtet** . Klicken Sie auf **Weiter**.

    ![mds_2016ConfigManager_CreateDatabaseWizard_DatabasePage](../master-data-services/media/mds-2016configmanager-createdatabasewizard-databasepage.png)  
  
     Weitere Informationen zur Windows-Sortierung finden Sie unter [Name der Windows-Sortierung (Transact-SQL)](../t-sql/statements/windows-collation-name-transact-sql.md).  
  
5.  Geben Sie im Feld **Benutzername** das Windows-Konto des Benutzers an, der der Standardadministrator für Master Data Services sein wird. Ein Administrator verfügt über Zugriff auf alle Funktionsbereiche und kann alle Modelle hinzufügen, löschen oder aktualisieren.  

    ![mds_2016ConfigManager_CreateDatabaseWizard_AdminPage](../master-data-services/media/mds-2016configmanager-createdatabasewizard-adminpage.png)  
  
6.  Klicken Sie auf **weiter** , um eine Zusammenfassung der Einstellungen für die [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] Datenbank anzuzeigen, und klicken Sie dann erneut auf **weiter** , um die Datenbank zu erstellen. Die Seite **Status und Fertig stellen** wird angezeigt.

7. Wenn die Datenbank erstellt und konfiguriert wurde, klicken Sie auf **Fertig stellen**.  
  
     Informationen zu den Einstellungen in **Datenbank erstellen (Assistent)** finden Sie unter [Datenbank erstellen &#40;Assistent im Konfigurations-Manager für Master Data Services&#41;](../master-data-services/create-database-wizard-master-data-services-configuration-manager.md).  
  
7.  Klicken Sie im auf der Seite **Daten Bank Konfiguration** auf [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] **Datenbank auswählen**.  
  
8.  Klicken Sie auf **Verbinden**, und wählen Sie anschließend die [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]-Datenbank, die Sie in Schritt 7 erstellt haben. Klicken Sie anschließend auf **OK**. 

    ![mds_2016ConfigManager_SelectDatabaseButton_ConnectToDatabaseDialog](../master-data-services/media/mds-2016configmanager-selectdatabasebutton-connecttodatabasedialog.png)  
  
     Somit ist die Einrichtung der Datenbank abgeschlossen. Die Seite **Datenbankkonfiguration** zeigt nun die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Instanz an, mit der Sie für [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]eine Verbindung hergestellt haben, der Datenbank, die Sie erstellt haben sowie die aktuelle Datenbankversion.  

    ![mds_2016ConfigManager_DatabaseConfig_Completed](../master-data-services/media/mds-2016configmanager-databaseconfig-completed.png)   
  
9. Klicken Sie im [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]im linken Bereich auf **Webkonfiguration** .  
  
10. Klicken Sie im Listenfeld **Website** auf **Standardwebsite** und anschließend auf **Erstellen** , um eine Webanwendung zu erstellen.  
  
    > [!NOTE]  
    >  Wenn Sie **Standardwebsite** auswählen, müssen Sie eine Webanwendung erstellen. Wenn Sie die Option **Neue Website erstellen** im Listenfeld auswählen, wird die Anwendung automatisch erstellt.  

     ![mds_2016ConfigManager_WebConfig](../master-data-services/media/mds-2016configmanager-webconfig.png)  
  
11. Führen Sie im Abschnitt **Anwendungspool** einen der folgenden Schritte aus.  
  
    -   Geben Sie denselben Benutzernamen ein, den Sie in Schritt 5 für das **Administratorkonto** der Datenbank eingegeben haben. Geben Sie zudem das Kennwort ein, und klicken Sie auf **OK**.  
  
         **Noch**  
  
    -   Geben Sie einen anderen Benutzernamen ein, geben Sie das Kennwort ein, und klicken Sie anschließend auf OK.  
  
         Sie müssen nicht das gleiche Konto verwenden, wenn Sie die Datenbank und die Webanwendung erstellen.  

        ![mds_2016ConfigManager_WebConfig_CreateWebApplication](../master-data-services/media/mds-2016configmanager-webconfig-createwebapplication.png)   
  
     Weitere Informationen zum Dialogfeld **Webanwendung erstellen** finden Sie unter [Webanwendung erstellen (Dialogfeld) &#40;Konfigurations-Manager für Master Data Services&#41;](../master-data-services/create-web-application-dialog-box-master-data-services-configuration-manager.md).  

    > [!NOTE] 
    >  Wenn Ihre Domäne [2020 LDAP-kanalbindung und LDAP-Signierungs Anforderungen für Windows](https://support.microsoft.com/en-us/help/4520412/2020-ldap-channel-binding-and-ldap-signing-requirements-for-windows)implementiert. Das Problem "die Anmelde Informationen konnten in Active Directory nicht überprüft werden" wird angezeigt. Wenn Sie ein Domänen Konto verwenden, um einen Anwendungs Pool zu erstellen. Verwenden Sie für die Problem Umgehung anstelle des Domänen Benutzers einen **lokalen Computerbenutzer**. Dadurch kann die Überprüfung der Anmelde Informationen mit Active Directory umgangen werden. Nachdem Sie die Webanwendung erstellt haben, können Sie die Identität in **Internetinformationsdienste (IIS)-Manager** in Domänen Benutzer ändern.
  
12. Klicken Sie auf der Seite **Webkonfiguration** im Feld **Webanwendung** auf die Anwendung, die Sie erstellt haben, und anschließend im Abschnitt **Zuordnen einer Anwendung zu einer Datenbank** auf  **Auswählen** .  
  
13. Klicken Sie auf **Verbinden**, wählen Sie die [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Datenbank aus, die Sie der Web-Anwendung zuordnen möchten, und klicken Sie anschließend auf **OK**.  
  
     Somit ist die Einrichtung der Website abgeschlossen. Die Seite **Webkonfiguration** zeigt nun die von Ihnen ausgewählte Website an sowie die Webanwendung, die Sie erstellt haben und die [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Datenbank, die der Anwendung zugeordnet ist.  

     ![mds_2016ConfigManager_WebConfig_Completed](../master-data-services/media/mds-2016configmanager-webconfig-completed.png)  
 
     
15. Klicken Sie auf **Anwenden**. Das Meldungsfeld **Konfiguration abgeschlossen** wird angezeigt. Klicken Sie im Meldungsfeld auf **OK**, um die Webanwendung zu starten. Die Website Adresse lautet https://*Servername* / *Webanwendung*/. 


![mds_2016ConfigurationComplete_MessageBox](../master-data-services/media/mds-2016configurationcomplete-messagebox.png) 
  
Weitere Informationen zu den Einstellungen auf der Seite „Webkonfiguration“ finden Sie unter [Webkonfiguration &#40;Seite im Konfigurations-Manager für Master Data Sevices&#41;](../master-data-services/web-configuration-page-master-data-services-configuration-manager.md)  
  
 Alternativ können Sie [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] verwenden, um andere Einstellungen für die der [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Datenbank zugeordneten Webanwendungen und -dienste festzulegen. Beispielsweise können Sie festlegen, wie häufig Daten geladen werden oder wie oft Überprüfungs-E-Mails gesendet werden. Weitere Informationen finden Sie unter [Systemeinstellungen &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
##  <a name="deploying-sample-models-and-data"></a><a name="deploySample"></a> Bereitstellen von Beispielmodellen und -daten  
 Die folgenden drei Beispielmodellpakete sind in  [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]enthalten.   Diese Beispielmodelle enthalten Daten. **Der Standardspeicherort für die Beispielmodellpakete ist: %programfiles%\Microsoft SQL Server\140\Master Data Services\Samples\Packages**.
  
-   chartofaccounts_en.pkg  
-   customer_en.pkg  
-   product_en.pkg  
  
 Stellen Sie diese Pakete mit dem MDSModelDeploy-Tool bereit. Der Standardspeicherort für das MDSModelDeploy-Tool ist *Laufwerk*\Programme\Microsoft SQL Server\140\Master Data Services\Configuration.  
  
 Informationen zu den Voraussetzungen zur Ausführung dieses Tools finden Sie unter [Bereitstellen eines Modellbereitstellungspakets mit MDSModelDeploy](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md).  
  
 Informationen zu Updates für die Daten zur Unterstützung neuer Funktionen in [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] finden Sie unter [SQL Server-Beispiele: Modellbereitstellungspakete (MDS)](../master-data-services/sql-server-samples-model-deployment-packages-mds.md).  
  
 **So stellen Sie die Beispielmodelle bereit**  
  
1.  Kopieren Sie die Beispielmodellpakete in *Laufwerk*:\Programme\Microsoft SQL Server\140\Master Data Services\Configuration.  
  
2.  Öffnen Sie eine Administrator-Eingabeaufforderung, und navigieren Sie zu "MDSModelDeploy.exe", indem Sie folgenden Befehl ausführen.  
  
    ```  
    cd c:\Program Files\Microsoft SQL Server\140\Master Data Services\Configuration  
    ```  
  
3.  Stellen Sie jedes Beispielmodell in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] bereit, indem Sie jeden der folgenden Befehle ausführen.  
  
    > [!IMPORTANT]  
    >  In den folgenden Beispielen wird der `MDS1` -Dienstwert angegeben. Dieser Wert wird verwendet, wenn Sie beim Einrichten der  **-Website** Standardwebsite [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] ausgewählt haben.  Weitere Informationen finden Sie im Abschnitt [Einrichten der Datenbank und der Website](#SetUpWeb) .  
    >   
    >  Wenn Sie eine neue Website erstellt haben oder eine vorhandene Website ausgewählt haben, führen Sie zunächst folgenden Befehl aus, um den korrekten Dienstwert zu bestimmen.  
    >   
    >  `MDSModelDeploy listservices`  
    >   
    >  Der erste Dienstwert in der Liste mit den Werten wird zurückgegeben und dies ist der Wert, den Sie angeben, um ein Modell bereitzustellen.  

    > [!NOTE]
    > Um mehr über die Metadateninformationen der Beispielmodelle zu erfahren, lesen Sie die Infodatei, die an diesem Speicherort vorhanden sind: „C:\Programme\Microsoft SQL Server\140\Master Data Services\Configuration“.
   
     **So stellen Sie das Beispielmodell „chartofaccounts_en.pkg“ bereit**  
  
    ```console
    MDSModelDeploy deploynew -package chartofaccounts_en.pkg -model ChartofAccounts -service MDS1  
    ```  
  
     **So stellen Sie das Beispielmodell „customer_en.pkg“ bereit**  
  
    ```console
    MDSModelDeploy deploynew -package customer_en.pkg -model Customer -service MDS1  
    ```  
  
     **So stellen Sie das Beispielmodell „product_en.pkg“ bereit**  
  
    ```console
    MDSModelDeploy deploynew -package product_en.pkg -model Product -service MDS1  
    ```  
  
     Wenn ein Modell erfolgreich bereitgestellt wurde, wird die Meldung **Der MDSModelDeploy-Vorgang wurde erfolgreich abgeschlossen** angezeigt.  
  
     Die folgende Abbildung zeigt den Befehl zum Bereitstellen des Beispielmodells product_en.pkg.  
  
     ![Befehlszeile für die Bereitstellung des Produktbeispiel Modells](../master-data-services/media/mds-commandprompt-deployingsamplemodel-product.png "Befehlszeile für die Bereitstellung des Produktbeispiel Modells")  
  
4.  Führen Sie folgende Schritte aus, um die Beispielmodelle anzuzeigen.  
  
    1.  Navigieren Sie zur [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Website, die Sie eingerichtet haben. Weitere Informationen finden Sie im Abschnitt [Einrichten der Datenbank und der Website](#SetUpWeb) .  
  
         Die Website Adresse lautet https://*Servername* / *Webanwendung*/.  
  
    2.  Wählen Sie ein Modell aus der Liste **Modell** aus, und klicken Sie auf **Explorer**.  
  
         ![MDS-Website, Startseite.](../master-data-services/media/mds-mdswebsite-homepage-selectsamplemodel.png "MDS-Website, Startseite.")  
  
## <a name="next-step"></a>Nächster Schritt  
 Erstellen Sie ein neues Modell und Entitäten für Ihre Daten. Weitere Informationen finden Sie unter [Erstellen eines Modells &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md) und [Erstellen einer Entität &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md).  
  
 Einen Überblick darüber, wie Sie ein Modell und Entitäten verwenden, um eine Struktur für Ihre Daten in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] zu erstellen, finden Sie unter [Übersicht über Master Data Services &#40;MDS&#41;](../master-data-services/master-data-services-overview-mds.md).  
    
## <a name="see-also"></a>Weitere Informationen  
 [Master Data Services Datenbank](../master-data-services/master-data-services-database.md)   
 [Webanwendung Master Data Manager](../master-data-services/master-data-manager-web-application.md)   
 [Die Seite "Daten Bank Konfiguration" &#40;Konfigurations-Manager für Master Data Services&#41;](../master-data-services/database-configuration-page-master-data-services-configuration-manager.md)   
 [Neues in Master Data Services &#40;MDS&#41;](../master-data-services/what-s-new-in-master-data-services-mds.md)  
  
