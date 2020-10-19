---
description: Migrieren einer Reporting Services-Installation (einheitlicher Modus)
title: Migrieren einer Reporting Services-Installation (einheitlicher Modus) | Microsoft-Dokumentation
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.date: 05/01/2020
ms.openlocfilehash: d45e00b7d99f87ec3edc9bdd123d5392412dcf73
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91934844"
---
# <a name="migrate-a-reporting-services-installation-native-mode"></a>Migrieren einer Reporting Services-Installation (einheitlicher Modus)

Dieses Thema erklärt die Schritte zum Migrieren einer der folgenden unterstützten Versionen einer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Bereitstellung im einheitlichen Modus zu einer neuen Instanz von SQL Server Reporting Services:  
  
::: moniker range=">=sql-server-2017||=sqlallproducts-allversions"
* [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)]

* [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
* [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
* [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
* [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]
::: moniker-end

::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
* [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
* [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
* [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
* [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]

Informationen zum Migrieren einer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Bereitstellung im SharePoint-Modus finden Sie unter [Migrieren einer Installation von Reporting Services &#40;SharePoint-Modus&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md).  

::: moniker-end
  
 Eine Migration wird als das Verschieben von Anwendungsdatendateien in eine neue SQL Server-Instanz definiert. Die folgenden häufigen Ursachen erfordern das Migrieren der Installation:  
  
* Sie verfügen über eine umfangreiche Implementierung oder benötigen ein hoch verfügbares System.  
  
* Die Hardware oder Topologie der Installation wurde geändert.  
  
* Es ist ein Problem aufgetreten, das Upgrades blockiert.

## <a name="native-mode-migration-overview"></a><a name="bkmk_nativemode_migration_overview"></a> Übersicht über die Migration im einheitlichen Modus

 Der Migrationsprozess für [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] umfasst manuelle und automatisierte Schritte. Die Migration eines Berichtsservers besteht aus den folgenden Teilaufgaben:  
  
* Sichern Sie Datenbank-, Anwendungs- und Konfigurationsdateien.  
  
* Sichern Sie den Verschlüsselungsschlüssel.  
  
* Installieren Sie eine neue Instanz von SQL Server 2012 SP1. Wenn Sie dieselbe Hardware verwenden, können Sie SQL Server parallel zu einer vorhandenen Installation installieren, wenn diese eine der unterstützten Versionen aufweist.  
  
    > [!TIP]  
    >  Bei einer parallelen Installation kann es erforderlich sein, SQL Server als benannte Instanz zu installieren.
  
* Verschieben Sie die Berichtsserver-Datenbank und andere Anwendungsdateien aus der vorhandenen Installation in die neue Installation von SQL Server.  
  
* Verschieben Sie alle benutzerdefinierten Anwendungsdateien in die neue Installation.  
  
* Konfigurieren Sie den Berichtsserver.  
  
* Bearbeiten Sie **RSReportServer.config** , um ggf. vorhandene benutzerdefinierte Einstellungen aus der vorherigen Installation zu übernehmen.  
  
* Sie können optional benutzerdefinierte Zugriffssteuerungslisten (ACLs) für die neue [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Windows-Dienstgruppe konfigurieren.  
  
* Entfernen Sie nicht verwendete Anwendungen und Tools, nachdem Sie sich überzeugt haben, dass die neue Instanz voll funktionstüchtig ist.  
  
 Es gelten Beschränkungen bezüglich der Editionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , auf denen die Berichtsserver-Datenbank gehostet wird. Lesen Sie das folgende Thema, wenn Sie eine Berichtsserver-Datenbank, die in einer früheren Installation erstellt wurde, erneut verwenden.  
  
* [Erstellen einer Berichtsserver-Datenbank](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
  
## <a name="fixed-database-name"></a><a name="bkmk_fixed_database_name"></a> Fester Datenbankname

 Sie können die Berichtsserver-Datenbank nicht umbenennen. Die Identität der Datenbank wird bei der Datenbankerstellung in auf dem Berichtsserver gespeicherten Prozeduren aufgezeichnet. Wenn die primären oder temporären Berichtsserver-Datenbanken umbenannt werden, treten während der Ausführung der Prozeduren Fehler auf, wodurch die Berichtsserverinstallation ungültig wird.  
  
 Wenn der Datenbankname der vorhandenen Installation für die neue Installation ungeeignet ist, sollten Sie eine neue Datenbank mit dem Namen erstellen und die vorhandenen Anwendungsdaten mithilfe der in der folgenden Liste beschriebenen Verfahren laden:  
  
* Schreiben Sie ein [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] -Skript, das SOAP-Methoden des Berichtsserver-Webdiensts aufruft, um Daten von einer Datenbank in eine andere Datenbank zu kopieren. Zum Ausführen des Skripts können Sie das Hilfsprogramm RS.exe verwenden. Weitere Informationen zu diesem Verfahren finden Sie unter [Skripterstellung und PowerShell mit Reporting Services](../../reporting-services/tools/scripting-and-powershell-with-reporting-services.md).  
  
* Schreiben Sie Code, in dem der WMI-Anbieter aufgerufen wird, um Daten von einer Datenbank in eine andere Datenbank zu kopieren. Weitere Informationen zu diesem Verfahren finden Sie unter [Zugreifen auf den Reporting Services-WMI-Anbieter](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md).  
  
* Wenn nur wenige Elemente vorliegen, können Sie die Berichte und freigegebenen Datenquellen vom Berichts-Designer, Modell-Designer und Berichts-Generator aus erneut auf dem neuen Berichtsserver veröffentlichen. Sie müssen Rollenzuweisungen, Abonnements, freigegebene Zeitpläne, Zeitpläne für Berichtmomentaufnahmen, benutzerdefinierte Eigenschaften, die Sie für Berichte und andere Elemente festlegen, Modellelementsicherheit und Eigenschaften, die Sie auf dem Berichtsserver festlegen, erneut erstellen. Bereiten Sie sich darauf vor, dass Sie Ihren Berichtsverlauf und Ihre Protokolldaten zur Berichtsausführung verlieren, wenn Sie diese Aktionen durchführen.
  
## <a name="before-you-start"></a><a name="bkmk_before_you_start"></a> Vorbereitungen

 Auch wenn Sie die Installation nicht aktualisieren, sondern migrieren, sollten Sie die Ausführung von Upgrade Advisor für die vorhandene Installation in Erwägung ziehen, um mögliche Probleme zu ermitteln, die sich auf die Migration auswirken könnten. Dieser Schritt ist besonders dann nützlich, wenn Sie einen Berichtsserver migrieren, den Sie nicht selbst installiert oder konfiguriert haben. Wenn Sie Upgrade Advisor ausführen, können Sie benutzerdefinierte Einstellungen ausfindig machen, die ggf. in einer neuen Installation von SQL Server nicht unterstützt werden.  
  
 Zudem müssen Sie sich mehrerer wichtiger Änderungen in SQL Server Reporting Services bewusst sein, die sich auf Ihre Vorgehensweise beim Migrieren der Installation auswirken:

* Das neue [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] hat den Berichts-Manager ersetzt.
  
* Ab [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]ist IIS keine erforderliche Komponente mehr. Wenn Sie eine Berichtsserverinstallation zu einem neuen Computer migrieren, müssen Sie die Webserverrolle nicht hinzufügen. Zudem unterscheiden sich die Schritte zum Konfigurieren von URLs und Authentifizierung von den in der vorherigen Version erforderlichen Schritten. Dasselbe gilt für Methoden und Tools für die Diagnose und Problembehandlung.  
  
* Der Berichtsserver-Webdienst, das [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]und der Berichtsserver-Windows-Dienst werden unter demselben Konto ausgeführt. Alle drei Anwendungen lesen Konfigurationseinstellungen aus der Datei „RSReportServer.config“.
  
* Das [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] und SQL Server Management Studio sind dazu konzipiert, um überlappende Features zu entfernen. Jedes Tool unterstützt unterschiedliche Aufgaben.
  
* ISAPI-Filter werden in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] und höheren Versionen nicht unterstützt. Wenn Sie ISAPI-Filter verwenden, müssen Sie Ihre Berichtslösung vor der Migration umgestalten.  
  
* IP-Adresseinschränkungen werden in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] und höheren Versionen nicht unterstützt. Wenn Sie IP-Adresseinschränkungen verwenden, müssen Sie Ihre Berichtslösung vor der Migration umgestalten oder eine Technologie wie z. B. eine Firewall, einen Router oder NAT (Network Address Translation) verwenden, um Adressen zu konfigurieren, die nur eingeschränkt auf den Berichtsserver zugreifen dürfen.  
  
* Client-TLS-Zertifikate (Transport Layer Security, früher als Secure Sockets Layer, SSL, bezeichnet), werden in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] und höheren Versionen nicht unterstützt. Wenn Sie Client-TLS-Zertifikate verwenden, müssen Sie Ihre Berichtslösung vor der Migration umgestalten.  
  
* Wenn Sie einen anderen Authentifizierungstyp als die integrierte Windows-Authentifizierung verwenden, müssen Sie das Element `<AuthenticationTypes>` in der Datei **RSReportServer.config** mit einem unterstützten Authentifizierungstyp aktualisieren. Die unterstützten Authentifizierungstypen sind NTLM, Kerberos, Negotiate und Standard. Die anonyme, die .NET Passport- und die Digestauthentifizierung werden in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] und höheren Versionen nicht unterstützt.  
  
* Wenn Sie benutzerdefinierte Cascading Stylesheets in der Berichterstellungsumgebung verwenden, werden sie nicht migriert. Verschieben Sie sie nach der Migration manuell.
  
Weitere Informationen zu Änderungen in Microsoft SQL Server Reporting Services finden Sie in der Dokumentation des Upgraderatgebers und unter [Neues bei Reporting Services](../../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md).  

## <a name="backup-files-and-data"></a><a name="bkmk_backup"></a> Sichern von Dateien und Daten

 Vor dem Installieren einer neuen Instanz von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]müssen Sie alle Dateien der aktuellen Installation sichern.  
  
1. Sichern Sie den Verschlüsselungsschlüssel für die Berichtsserver-Datenbank. Dieser Schritt ist für den Erfolg der Migration entscheidend. Im weiteren Verlauf des Migrationsprozesses müssen Sie den Schlüssel wiederherstellen, damit der Berichtsserver wieder auf verschlüsselte Daten zugreifen kann. Verwenden Sie den Berichtsserver-Konfigurations-Manager, um den Schlüssel zu sichern.  
  
2. Sichern Sie die Berichtsserver-Datenbank mit einer der unterstützten Methoden zum Sichern einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank. Weitere Informationen finden Sie in der Anleitung zum Sichern der Berichtsserver-Datenbank unter [Verschieben von Berichtsserver-Datenbanken auf einen anderen Computer (einheitlicher SSRS-Modus)](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md).  
  
3. Sichern Sie die Konfigurationsdateien des Berichtsservers. Folgende Dateien müssen gesichert werden:  
  
    1. RSReportServer.config  
  
    2. Rswebapplication.config  
  
    3. Rssvrpolicy.config  
  
    4. Rsmgrpolicy.config  
  
    5. Reportingservicesservice.exe.config  
  
    6. „Web.config“ für die Berichtsserver-[!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]-Anwendung.  
  
    7. Machine.config für [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] , wenn Sie die Datei für Berichtsservervorgänge geändert haben  

## <a name="install-sql-server-reporting-services"></a><a name="bkmk_install_ssrs"></a> Installieren von SQL Server Reporting Services

 Installieren Sie eine neue Berichtsserverinstanz im Dateimodus, um sie so konfigurieren zu können, dass Nichtstandardwerte verwendet werden. Verwenden Sie bei der Befehlszeileninstallation das **FilesOnly**-Argument. Wählen Sie im Installations-Assistenten die Option **Server installieren, jedoch nicht konfigurieren**aus.  
  
 Klicken Sie auf einen der folgenden Links, um Anweisungen zum Installieren einer neuen Instanz von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]anzuzeigen.  
  
* [Installieren des Reporting Services 2016-Berichtsservers im einheitlichen Modus](install-reporting-services-native-mode-report-server.md) 
  
* [Installieren von SQL Server von der Eingabeaufforderung](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)  

* [Installieren von SQL Server Reporting Services](install-reporting-services.md)

## <a name="move-the-report-server-database"></a><a name="bkmk_move_database"></a> Verschieben der Berichtsserver-Datenbank

 Die Berichtsserver-Datenbank enthält veröffentlichte Berichte, Modelle, freigegebene Datenquellen, Zeitpläne, Ressourcen, Abonnements und Ordner. Außerdem enthält sie System- und Elementeigenschaften sowie Berechtigungen für den Zugriff auf den Berichtsserverinhalt.  
  
 Wenn die Migration den Wechsel zu einer anderen [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Instanz einschließt, müssen Sie die Berichtsserver-Datenbank in die neue [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Instanz verschieben. Wenn Sie dieselbe [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Instanz verwenden, gehen Sie zum Abschnitt [Verschieben von benutzerdefinierten Assemblys oder Erweiterungen](#bkmk_move_custom).  
  
 Führen Sie die folgenden Schritte aus, um die Berichtsserver-Datenbank zu verschieben:
  
1. Wählen Sie die zu verwendende [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Instanz aus. SQL Server Reporting Services erfordert, dass Sie eine der folgenden Versionen verwenden, um die Berichtsserver-Datenbank zu hosten:  
  
    ::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
    * [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)]

    * [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
    * [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
    * [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
    * [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]
    ::: moniker-end

    ::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
    * [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  

    * [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  

    * [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  

    * [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]
    ::: moniker-end
  
2. Starten Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , und stellen Sie eine Verbindung mit [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
3. Wenn **noch keine Berichtsserver-Datenbank gehostet hat, erstellen Sie die** RSExecRole [!INCLUDE[ssDE](../../includes/ssde-md.md)] in den Systemdatenbanken. Weitere Informationen finden Sie unter [Erstellen der Rolle RSExecRole](../../reporting-services/security/create-the-rsexecrole.md).  
  
4. Befolgen Sie die Anweisungen unter [Verschieben von Berichtsserver-Datenbanken auf einen anderen Computer (einheitlicher SSRS-Modus)](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md).  
  
 Denken Sie daran, dass die Berichtsserver-Datenbank und die temporäre Datenbank voneinander abhängig sind und gemeinsam verschoben werden müssen. Kopieren Sie die Datenbanken nicht; durch das Kopieren werden nicht alle Sicherheitseinstellungen auf die neue Installation übertragen. Verschieben Sie keine SQL Server-Agent-Aufträge für geplante Berichtsservervorgänge. Der Berichtsserver erstellt diese Aufträge automatisch neu.  

## <a name="move-custom-assemblies-or-extensions"></a><a name="bkmk_move_custom"></a> Verschieben von benutzerdefinierten Assemblys oder Erweiterungen

 Wenn die Installation benutzerdefinierte Berichtselemente, Assemblys oder Erweiterungen umfasst, müssen Sie die benutzerdefinierten Komponenten erneut bereitstellen. Wenn Sie keine benutzerdefinierten Komponenten verwenden, fahren Sie mit dem Abschnitt [Konfigurieren des Berichtsservers](#bkmk_configure_reportserver)fort.  
  
 Führen Sie die folgenden Schritte aus, um die benutzerdefinierten Komponenten erneut bereitzustellen:  
  
1. Ermitteln Sie, ob die Assemblys unterstützt werden oder neu kompiliert werden müssen:

    * Benutzerdefinierte Sicherheitserweiterungen müssen mithilfe der [IAuthenticationExtension2](/dotnet/api/microsoft.reportingservices.interfaces.iauthenticationextension2)-Schnittstelle umgeschrieben werden.
  
    * Benutzerdefinierte Renderingerweiterungen für [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] müssen mithilfe des Renderingobjektmodells (ROM) umgeschrieben werden.  
  
    * HTML 3.2- und HTML OWC-Renderer werden in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] und höheren Versionen nicht unterstützt.  
  
    * Andere benutzerdefinierte Assemblys müssen normalerweise nicht neu kompiliert werden.  
  
2. Verschieben Sie die Assemblys auf den neuen Berichtsserver in den Ordner „\bin“. In SQL Server befinden sich die Berichtsserver-Binärdateien an folgendem Speicherort der Standardinstanz des Berichtsservers:  
  
     `\Program files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer\bin`  
  
3. Ändern Sie die Konfigurationsdateien, indem Sie Einträge für die benutzerdefinierte Komponente hinzufügen. Die Einträge sind von der Art der verwendeten Assembly abhängig. Anweisungen dazu, wo Dateien platziert und Konfigurationseinträge hinzugefügt werden, finden Sie unter folgenden Links:  
  
    1. [Bereitstellen einer benutzerdefinierten Assembly](../../reporting-services/custom-assemblies/deploying-a-custom-assembly.md)  
  
    2. [Vorgehensweise: Bereitstellen eines benutzerdefinierten Berichtselements](../../reporting-services/custom-report-items/how-to-deploy-a-custom-report-item.md)  
  
    3. [Bereitstellen von Datenverarbeitungserweiterungen](../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)  
  
    4. [Bereitstellen von Übermittlungserweiterungen](../../reporting-services/extensions/delivery-extension/deploying-a-delivery-extension.md)  
  
    5. [Bereitstellen von Renderingerweiterungen](../../reporting-services/extensions/rendering-extension/deploying-a-rendering-extension.md)  
  
    6. [Implementieren von Sicherheitserweiterungen](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)  

## <a name="configure-the-report-server"></a><a name="bkmk_configure_reportserver"></a> Konfigurieren des Berichtsservers

 Konfigurieren Sie URLs für den Report Server-Webdienst und das Webportal, und konfigurieren Sie die Verbindung mit der Berichtsserver-Datenbank.  
  
 Wenn Sie eine Bereitstellung für horizontales Skalieren migrieren, müssen Sie alle Berichtsserverknoten offline schalten und jeden Server einzeln migrieren. Nach der Migration des ersten Berichtsservers und einem erfolgreichen Verbindungsaufbau mit der Berichtsserver-Datenbank, wird die Version der Berichtsserver-Datenbank automatisch auf die Datenbankversion von SQL Server aktualisiert.  
  
> [!IMPORTANT]
>  Wenn Berichtsserver in der Bereitstellung für die horizontales Skalieren online sind und nicht migriert wurden, könnte die Ausnahme *rsInvalidReportServerDatabase* auftreten, weil sie ein älteres Schema verwenden, wenn sie mit den aktualisierten verbunden werden.  

Wenn der migrierte Berichtsserver als freigegebene Datenbank für eine Bereitstellung für horizontales Skalieren konfiguriert wurde, müssen Sie alle alten Verschlüsselungsschlüssel aus der Tabelle **Keys** in der **ReportServer**-Datenbank vor dem Konfigurieren des Berichtsserverdiensts löschen. Wenn die Schlüssel nicht entfernt werden, versucht der migrierte Berichtsserver, im Bereitstellungsmodus für horizontales Skalieren zu initialisieren. Weitere Informationen finden Sie unter [Hinzufügen und Entfernen von Verschlüsselungsschlüsseln für die Bereitstellung für horizontales Skalieren &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md) und [Konfigurieren und Verwalten von Verschlüsselungsschlüsseln &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md).  

Die Schlüssel für horizontales Skalieren können nicht mit dem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurations-Manager gelöscht werden. Die alten Schlüssel müssen mit SQL Server Management Studio aus der Tabelle **Keys** in der **ReportServer** -Datenbank gelöscht werden. Löschen Sie alle Zeilen in der Keys-Tabelle. Durch diese Aktion wird die Tabelle wie in den folgenden Schritten dargestellt bereinigt und auf das Wiederherstellen des symmetrischen Schlüssels vorbereitet.  

Vor dem Löschen der Schlüssel wird empfohlen, die symmetrischen Verschlüsselungsschlüssel zu sichern. Mit dem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurations-Manager können Sie den Schlüssel sichern. Öffnen Sie den Konfigurations-Manager, klicken Sie auf die Registerkarte „Verschlüsselungsschlüssel“, und klicken Sie dann auf **Sichern**. Sie können auch WMI-Befehle zum Sichern des Verschlüsselungsschlüssels schreiben. Weitere Informationen finden Sie unter [BackupEncryptionKey-Methode (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-backupencryptionkey.md).  
  
1. Starten Sie den Berichtsserver-Konfigurations-Manager, und stellen Sie eine Verbindung mit der installierten [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Instanz her. Weitere Informationen finden Sie unter [Report Server Configuration Manager &#40;Native Mode&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md) (Konfigurations-Manager des Berichtsservers – &#40;einheitlicher Modus&#41;).  
  
2. Konfigurieren Sie URLs für den Berichtsserver und das Webportal. Weitere Informationen finden Sie unter [Konfigurieren einer URL &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md).  
  
3. Konfigurieren Sie die Berichtsserver-Datenbank, und wählen Sie dabei die vorhandene Berichtsserver-Datenbank aus der vorherigen Installation aus. Nach einer erfolgreichen Konfiguration werden die Berichtsserverdienste neu gestartet, und nach Herstellung einer Verbindung mit der Berichtsserver-Datenbank wird die Datenbank automatisch auf SQL Server Reporting Services aktualisiert. Informationen zum Ausführen des Assistenten zum Ändern der Datenbank, mit dem Sie eine Berichtsserver-Datenbank erstellen oder auswählen, finden Sie unter [Erstellen einer Berichtsserver-Datenbank im einheitlichen Modus](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).  
  
4. Stellen Sie die Verschlüsselungsschlüssel wieder her. Dieser Schritt ist erforderlich, um die umkehrbare Verschlüsselung für vorhandene Verbindungszeichenfolgen und Anmeldeinformationen, die sich bereits in der Berichtsserver-Datenbank befinden, zu aktivieren. Weitere Informationen finden Sie unter [Back Up and Restore Reporting Services Encryption Keys](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).  
  
5. Wenn Sie den Berichtsserver auf einem neuen Computer installiert haben und die Windows-Firewall verwenden, muss der TCP-Port, auf dem der Berichtsserver lauscht, geöffnet sein. Standardmäßig ist dies der Port 80. Anweisungen finden Sie unter [Konfigurieren einer Firewall für den Zugriff auf den Berichtsserver](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).  
  
6. Wenn Sie den Berichtsserver im einheitlichen Modus lokal verwalten möchten, müssen Sie das Betriebssystem konfigurieren, um die lokale Verwaltung mit dem Webportal zuzulassen. Weitere Informationen finden Sie unter [Konfigurieren eines Berichtsservers im einheitlichen Modus für die lokale Verwaltung](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).  

## <a name="copy-custom-configuration-settings-to-rsreportserverconfig-file"></a><a name="bkmk_copy_custom_config"></a> Kopieren von benutzerdefinierten Konfigurationseinstellungen in die Datei "RSReportServer.config"

Wenn Sie die Datei RSReportServer.config oder RSWebApplication.config in der vorherigen Installation geändert haben, sollten Sie dieselben Änderungen in der neuen Datei RSReportServer.config vornehmen. In der folgenden Liste sind einige der Gründe aufgeführt, aufgrund derer Sie die vorherige Konfigurationsdatei möglicherweise geändert haben. Außerdem sind Links zu zusätzlichen Informationen zum Konfigurieren derselben Einstellungen in SQL Server 2016 aufgeführt.  
  
|Anpassung|Information|  
|-------------------|-----------------|  
|Berichtsserver-E-Mail-Übermittlung mit benutzerdefinierten Einstellungen|[E-Mail-Einstellungen: einheitlicher Modus von Reporting Services](../../reporting-services/install-windows/e-mail-settings-reporting-services-native-mode-configuration-manager.md)|  
|Geräteinformationseinstellungen|[Anpassen der Parameter für Renderingerweiterungen in der Datei RSReportServer.config](../../reporting-services/customize-rendering-extension-parameters-in-rsreportserver-config.md)|

## <a name="windows-service-group-and-security-acls"></a><a name="bkmk_windowsservice_group"></a> Windows-Dienstgruppe und Sicherheits-ACLs

 In [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] gibt es eine Dienstgruppe, die Windows-Dienstgruppe von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], mit deren Hilfe Sicherheits-ACLs für alle Registrierungsschlüssel, Dateien und Ordner erstellt werden, die mit SQL Server Reporting Services installiert werden. Dieser Windows-Gruppenname wird im Format SQLServerReportServerUser$\<*computer_name*>$\<*instance_name*> angezeigt.  

## <a name="verify-your-deployment"></a><a name="bkmk_verify"></a> Überprüfen der Bereitstellung

1. Testen Sie die virtuellen Verzeichnisse für den Berichtsserver und das [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] , indem Sie einen Browser öffnen und die URL-Adresse eingeben. Weitere Informationen finden Sie unter [Überprüfen einer Installation von Reporting Services](../../reporting-services/install-windows/verify-a-reporting-services-installation.md).  
  
2. Testen Sie Berichte, und überprüfen Sie, ob sie die erwarteten Daten enthalten. Überprüfen Sie, ob in den Datenquelleninformationen noch immer die Datenquellen-Verbindungsinformationen angegeben sind. Der Berichtsserver verwendet beim Verarbeiten und Rendern von Berichten das Berichtsobjektmodell. Die Konstrukte [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] oder [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] werden jedoch nicht durch neue Report Definition Language-Elemente ersetzt. Weitere Informationen zu vorhandenen Berichten, die auf jeder Version Ihres Berichtsservers ausgeführt werden, finden Sie unter [Upgrade Reports (Aktualisieren von Berichten)](../../reporting-services/install-windows/upgrade-reports.md).  

## <a name="remove-unused-programs-and-files"></a><a name="bkmk_remove_unused"></a> Entfernen nicht verwendeter Programme und Dateien

Nachdem Sie Ihren Berichtsserver erfolgreich zu einer neuen Instanz migriert haben, sollten Sie die folgenden Schritte durchführen, um nicht mehr benötigte Programme und Dateien zu entfernen.  
  
1. Deinstallieren Sie die vorherige Version von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , wenn diese nicht mehr benötigt wird. Mit diesem Schritt werden die folgenden Elemente nicht gelöscht, aber Sie können sie manuell entfernen, wenn Sie sie nicht mehr benötigen:  
  
    * Die alte Berichtsserver-Datenbank  
  
    * RSExec-Rolle  
  
    * Berichtsserver-Dienstkonten  
  
    * Anwendungspool für den Berichtsserver-Webdienst  
  
    * Virtuelle Verzeichnisse für den Berichts-Manager und den Berichtsserver  
  
    * Berichtsserver-Protokolldateien  
  
2. Entfernen Sie IIS, wenn das Programm auf diesem Computer nicht mehr benötigt wird.

## <a name="next-steps"></a>Nächste Schritte

* [Migrieren einer Installation von Reporting Services](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md)  
* [Berichtsserver-Datenbank](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)   
* [Aktualisieren und Migrieren von Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
* [Reporting Services-Abwärtskompatibilität](../../reporting-services/reporting-services-backward-compatibility.md)   
* [Berichtsserver-Konfigurations-Manager](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)