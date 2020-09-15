---
description: MSReportServer_ConfigurationSetting-Member
title: MSReportServer_ConfigurationSetting-Member | Microsoft-Dokumentation
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: wmi-provider-library-reference
ms.topic: conceptual
apiname:
- MSReportServer_ConfigurationSetting Members
apilocation:
- reportingservices.mof
apitype: MOFDef
helpviewer_keywords:
- WMI provider [Reporting Services], MSReportServer_ConfigurationSetting class
- MSReportServer_ConfigurationSetting class
ms.assetid: 5e21a53a-a2f8-4b35-a8d9-5483519e3857
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7643435954c74285992c6a21ebd1df5d8998164b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88423094"
---
# <a name="msreportserver_configurationsetting-members"></a>MSReportServer_ConfigurationSetting-Member
  Die MSReportServer_ConfigurationSetting-Klasse enthält die folgenden Eigenschaften und Methoden.  
  
## <a name="public-properties"></a>Öffentliche Eigenschaften  
  
|Eigenschaft|BESCHREIBUNG|  
|-|-|  
|[ConnectionPoolSize](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-connectionpoolsize.md)|Gibt die Größe des Verbindungspools zurück, über den der Berichtsserver mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz kommuniziert, die die Berichtsserver-Datenbank hostet. Schreibgeschützt.|  
|[DatabaseLogonAccount](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-databaselogonaccount.md)|Gibt das Anmeldekonto an, über das der Berichtsserver eine Verbindung mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz herstellt, die die Berichtsserver-Datenbank hostet. Schreibgeschützt.|  
|[DatabaseLogonTimeout](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-databaselogontimeout.md)|Gibt die Anzahl von Sekunden an, die gewartet wird, ehe ein Anmeldeversuch bei der Berichtsserver-Datenbank fehlschlägt. Schreibgeschützt.|  
|[DatabaseLogonType](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-databaselogontype.md)|Gibt an, ob der Berichtsserver für den Zugriff auf die Berichtsserver-Datenbank ein Windows-Dienstkonto, ein Windows-Benutzerkonto oder eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldung verwendet. Schreibgeschützt.|  
|[DatabaseName](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-databasename.md)|Gibt den Namen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz an, die die Berichtsserver-Datenbank hostet|  
|[DatabaseQueryTimeout](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-databasequerytimeout.md)|Gibt die Anzahl von Sekunden an, die verstreichen müssen, ehe der Befehl fehlschlägt oder wegen eines Timeouts abgebrochen wird. Der Berichtsserver nimmt die zeitliche Steuerung des Prozesses anhand der Berichtsserver-Datenbank und nicht anhand einer Datenquelle für den Bericht vor.|  
|[DatabaseServerName](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-databaseservername.md)|Gibt den Namen des Servers an, auf dem die Berichtsserver-Datenbank installiert ist|  
|[InstallationID-Eigenschaft](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-installationid.md)|Gibt einen eindeutigen Bezeichner für eine bestimmte Berichtsserverinstanz zurück|  
|[InstanceName](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-instancename.md)|Gibt den Namen einer Berichtsserverinstanz auf einem bestimmten Computer an.|  
|[IsInitialized](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-isinitialized.md)|Gibt an, ob die Berichtsserverinstanz initialisiert wurde. Schreibgeschützt.|  
|[IsSharePointIntegrated](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-issharepointintegrated.md)|Gibt an, ob der Berichtsserver für den integrierten SharePoint-Modus konfiguriert ist.|  
|[IsWebServiceEnabled](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-iswebserviceenabled.md)|Gibt an, ob der Report Server-Webdienst aktiviert ist. Schreibgeschützt.|  
|[IsWindowsServiceEnabled](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-iswindowsserviceenabled.md)|Gibt an, ob der Report Server-Windows-Dienst aktiviert ist. Schreibgeschützt.|  
|[MachineAccountIdentity-Eigenschaft &#40;WMI&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-machineaccountidentity.md)|Ruft die Computerkontoidentität des Computers ab, auf dem der Berichtsserver installiert ist|  
|[PathName](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-pathname.md)|Gibt den Installationspfad zu einer Berichtsserverinstanz an|  
|[SecureConnectionLevel](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-secureconnectionlevel.md)|Gibt die in der Datei RSReportServer.config angegebene sichere Verbindungsebene zurück.|  
|[SenderEmailAddress](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-senderemailaddress.md)|Ruft die Adresse ab, die verwendet wird, um E-Mails vom Berichtsserver zu senden. Schreibgeschützt.|  
|[SendUsingSMTPServer](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-sendusingsmtpserver.md)|Gibt an, ob die SendUsing-Eigenschaft in der E-Mail-Konfiguration auf TRUE festgelegt wurde|  
|[SMTPServer](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-smtpserver.md)|Ruft die SMTP-Server-Eigenschaft aus der Datei RSReportServer.config ab. Schreibgeschützt.|  
|[UnattendedExecutionAccount](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-unattendedexecutionaccount.md)|Gibt das Benutzerkonto für die Anmeldung an, dessen Identität der Berichtsserver bei der unbeaufsichtigten Ausführung von Berichten annimmt. Schreibgeschützt.|  
|[Version](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-version.md)|Gibt die Version des Berichtsservers zurück|  
|[VirtualDirectoryReportManager-Eigenschaft &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-virtualdirectoryreportmanager.md)|Gibt das virtuelle Verzeichnis für die Berichts-Manager-Anwendung zurück|  
|[VirtualDirectoryReportManager-Eigenschaft &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-virtualdirectoryreportserver.md)|Gibt das virtuelle Verzeichnis für die Report Server-Webdienstanwendung zurück|  
|[WindowsServiceIdentityActual](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-windowsserviceidentityactual.md)|Gibt die Identität zurück, unter der der Report Server-Windows-Dienst tatsächlich ausgeführt wird. Schreibgeschützt.|  
|[WindowsServiceIdentityConfigured](../../reporting-services/wmi-provider-library-reference/windowsserviceidentityconfigured-property.md)|Gibt die Identität zurück, unter der der Report Server-Windows-Dienst entsprechend seiner letzten Konfiguration ausgeführt wird. Schreibgeschützt.|  
  
## <a name="public-methods"></a>Öffentliche Methoden  
  
|Methode|BESCHREIBUNG|  
|-|-|  
|[BackupEncryptionKey](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-backupencryptionkey.md)|Sichert den Verschlüsselungsschlüssel für die Instanz. Der Verschlüsselungsschlüssel wird mit einem Kennwort verschlüsselt gespeichert.|  
|[CreateSSLCertificateBinding-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-createsslcertificatebinding.md)|Erstellt eine TLS/SSL-Zertifikatsbindung.|  
|[DeleteEncryptedInformation](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-deleteencryptedinformation.md)|Löscht die verschlüsselten Informationen aus der Berichtsserver-Datenbank|  
|[DeleteEncryptionKey](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-deleteencryptionkey.md)|Löscht die Verschlüsselungsschlüssel aus der Berichtsserver-Datenbank|  
|[GenerateDatabaseCreationScript](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabasecreationscript.md)|Generiert ein SQL-Skript, mit dem die Berichtsserver-Datenbank erstellt werden kann|  
|[GenerateDatabaseRightsScript](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaserightsscript.md)|Generiert ein SQL-Skript, mit dem einem Benutzer Berechtigungen für die Berichtsserver-Datenbank erteilt werden können|  
|[GenerateDatabaseUpgradeScript](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaseupgradescript.md)|Generiert ein SQL-Skript, mit dem die Berichtsserver-Datenbank aktualisiert werden kann|  
|[GetAdminSiteUrl Method &#40;WMI&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-getadminsiteurl.md)|Ruft die absolute URL für die Zentraladministrationswebsite ab|  
|[GetDatabaseVersionDisplayName](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-getdatabaseversiondisplayname.md)|Ruft den Anzeigenamen für eine gegebene Versionszeichenfolge in der Berichtsserver-Datenbank ab|  
|[InitializeReportServer](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-initializereportserver.md)|Initialisiert die angegebene Berichtsserverinstanz.|  
|[ListInstalledSharePointVersions-Methode &#40;WMI&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listinstalledsharepointversions.md)|Gibt mehrere Token zurück, die die Versionen von [!INCLUDE[winSPServ](../../includes/winspserv-md.md)] [!INCLUDE[offSPServ](../../includes/offspserv-md.md)], [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] oder [!INCLUDE[SPS2010](../../includes/sps2010-md.md)] darstellen, die auf dem gleichen Computer wie der Berichtsserver installiert sind.|  
|[ListIPAddresses-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listipaddresses.md)|Listet IP-Adressen für den Computer auf|  
|[ListReportServersInDatabase](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listreportserversindatabase.md)|Gibt eine Liste von Berichtsserverinstallationen zurück, die in der Berichtsserver-Datenbank vorhanden sind. Dies geschieht unabhängig davon, ob diese Installationen Zugriff auf sichere Informationen haben.|  
|[ListReservedURLs-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listreservedurls.md)|Listet für alle Anwendungen auf dem Berichtsserver reservierte URLs auf|  
|[ListSSLCertificateBindings-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listsslcertificatebindings.md)|Listet die in HTTP.SYS vorhandenen und die von „rsreportserver.config“ erwarteten TLS/SSL-Zertifikatsbindungen auf.|  
|[ListSSLCertificates-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listsslcertificates.md)|Listet die auf dem Computer installierten TLS/SSL-Zertifikate auf.|  
|[ReencryptSecureInformation](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-reencryptsecureinformation.md)|Generiert einen neuen Verschlüsselungsschlüssel und verschlüsselt alle sicheren Informationen in der Berichtsserver-Datenbank erneut mit diesem neuen Schlüssel|  
|[RemoveSSLCertificateBindings-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-removesslcertificatebinding.md)|Entfernt eine TLS/SSL-Zertifikatsbindung.|  
|[RemoveUnattendedExecutionAccount](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-removeunattendedexecutionaccount.md)|Löscht den Eintrag für das Konto für die unbeaufsichtigte Ausführung aus der Berichtsserverkonfiguration|  
|[RemoveURL-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-removeurl.md)|Entfernt eine für den Berichtsserver reservierte URL|  
|[ReserveURL-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-reserveurl.md)|Fügt eine URL-Reservierung für eine gegebene Anwendung hinzu|  
|[RestoreEncryptionKey](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-restoreencryptionkey.md)|Wendet den angegebenen Verschlüsselungsschlüssel erneut auf die Berichtsserver-Datenbank an|  
|[SetDatabaseConnection](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setdatabaseconnection.md)|Legt die Berichtsserver-Datenbankverbindung auf eine bestimmte Berichtsserver-Datenbank fest|  
|[SetDatabaseLogonTimeout](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setdatabaselogontimeout.md)|Gibt den Standardtimeoutwert für Anmeldeversuche bei der Berichtsserver-Datenbank an|  
|[SetDatabaseQueryTimeout](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setdatabasequerytimeout.md)|Gibt den Standardtimeoutwert für Verbindungen mit der Berichtsserver-Datenbank an|  
|[SetEmailConfiguration](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setemailconfiguration.md)|Konfiguriert die E-Mail-Übermittlungserweiterung, die vom Berichtsserver zum Senden von E-Mails verwendet wird|  
|[SetSecureConnectionLevel](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setsecureconnectionlevel.md)|Legt die sichere Verbindungsebene des Berichtsservers fest|  
|[SetServiceState](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setservicestate.md)|Aktiviert und deaktiviert den Berichtsserver-Windows-Dienst und den Berichtsserver-Webdienst.|  
|[SetUnattendedExecutionAccount](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setunattendedexecutionaccount.md)|Gibt das Konto an, das verwendet wird, um Berichte unbeaufsichtigt auszuführen|  
|[SetVirtualDirectory-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setvirtualdirectory.md)|Legt das virtuelle Verzeichnis für eine Anwendung fest|  
|[SetWindowsServiceIdentity](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setwindowsserviceidentity.md)|Lässt den Berichtsserver-Windows-Dienst als den angegebenen Windows-Benutzer ausführen und gewährt diesem Konto die erforderlichen Dateisystemberechtigungen, damit der Berichtsserver ausgeführt werden kann.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [MSReportServer_ConfigurationSetting Class (MSReportServer_ConfigurationSetting-Klasse)](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-class.md)  
  
  
