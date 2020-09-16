---
description: MSReportServer_ConfigurationSetting-Methoden
title: MSReportServer_ConfigurationSetting-Methoden | Microsoft-Dokumentation
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: wmi-provider-library-reference
ms.topic: conceptual
apiname:
- MSReportServer_ConfigurationSetting Methods
apilocation:
- reportingservices.mof
apitype: MOFDef
helpviewer_keywords:
- WMI provider [Reporting Services], MSReportServer_ConfigurationSetting class
- MSReportServer_ConfigurationSetting class
ms.assetid: a08c2476-5b8e-4792-94da-1360fe231c6e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7b51e79a99f47886627f6479f58b7a2980629d88
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88472608"
---
# <a name="msreportserver_configurationsetting-methods"></a>MSReportServer_ConfigurationSetting-Methoden
  Die MSReportServer_ConfigurationSetting-Klasse des Berichtsserver-WMI-Anbieters stellt die folgenden öffentlichen Methoden bereit.  
  
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
|[ListInstalledSharePointVersions-Methode &#40;WMI&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listinstalledsharepointversions.md)|Diese Methode gibt eine Reihe von Tokens zurück, die die Versionen von Microsoft [!INCLUDE[winSPServ](../../includes/winspserv-md.md)] [!INCLUDE[offSPServ](../../includes/offspserv-md.md)], [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] oder [!INCLUDE[SPS2010](../../includes/sps2010-md.md)] darstellen, die auf dem gleichen Computer wie der Berichtsserver installiert sind.|  
|[ListIPAddresses-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listipaddresses.md)|Listet IP-Adressen für den Computer auf|  
|[ListReportServersInDatabase](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listreportserversindatabase.md)|Gibt eine Liste von Berichtsserverinstallationen zurück, die in der Berichtsserver-Datenbank vorhanden sind. Dies geschieht unabhängig davon, ob diese Installationen Zugriff auf sichere Informationen haben.|  
|[ListReservedURLs-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listreservedurls.md)|Listet für alle Anwendungen auf dem Berichtsserver reservierte URLs auf|  
|[ListSSLCertificateBindings-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listsslcertificatebindings.md)|Listet die in HTTP.SYS vorhandenen und die von RSReportServer.config erwarteten TLS/SSL-Zertifikatsbindungen auf.|  
|[ListSSLCertificates-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listsslcertificates.md)|Listet die auf dem Computer installierten TLS/SSL-Zertifikate auf.|  
|[ReencryptSecureInformation](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-reencryptsecureinformation.md)|Generiert einen neuen Verschlüsselungsschlüssel und verschlüsselt alle sicheren Informationen in der Berichtsserver-Datenbank erneut mit diesem neuen Schlüssel|  
|[RemoveSSLCertificateBindings-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-removesslcertificatebinding.md)|Entfernt eine TLS/SSL-Zertifikatsbindung.|  
|[RemoveUnattendedExecutionAccount](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-removeunattendedexecutionaccount.md)|Löscht den Eintrag für das Konto für die unbeaufsichtigte Ausführung aus der Berichtsserverkonfiguration|  
|[RemoveURL-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-removeurl.md)|Entfernt eine für den Berichtsserver reservierte URL|  
|[ReserveURL-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-reserveurl.md)|Fügt eine URL-Reservierung für eine gegebene Anwendung hinzu|  
|[RestoreEncryptionKey](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-restoreencryptionkey.md)|Wendet den angegebenen Verschlüsselungsschlüssel erneut auf die Berichtsserver-Datenbank an|  
|[SetDatabaseConnection](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setdatabaseconnection.md)|Legt die Berichtsserver-Datenbankverbindung auf eine bestimmte Berichtsserver-Datenbank fest|  
|[SetDatabaseLogonTimeout](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setdatabaselogontimeout.md)|Gibt den Standardtimeoutwert für Anmeldeversuche bei der Berichtsserver-Datenbank an|  
|[SetDatabaseQueryTimeout](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setdatabasequerytimeout.md)|Gibt den Standardtimeoutwert für Berichtsserver-Datenbankabfragen an.|  
|[SetEmailConfiguration](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setemailconfiguration.md)|Konfiguriert die E-Mail-Übermittlungserweiterung, die vom Berichtsserver zum Senden von E-Mails verwendet wird|  
|[SetSecureConnectionLevel](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setsecureconnectionlevel.md)|Legt die sichere Verbindungsebene des Berichtsservers fest|  
|[SetServiceState](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setservicestate.md)|Schaltet den Berichtsserverdienst ein und aus|  
|[SetUnattendedExecutionAccount](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setunattendedexecutionaccount.md)|Gibt das Konto an, das verwendet wird, um Berichte unbeaufsichtigt auszuführen|  
|[SetVirtualDirectory-Methode &#40;WMI: MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setvirtualdirectory.md)|Legt das virtuelle Verzeichnis für eine Anwendung fest|  
|[SetWindowsServiceIdentity](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setwindowsserviceidentity.md)|Lässt den Berichtsserverdienst als den angegebenen Windows-Benutzer ausführen und gewährt diesem Konto die erforderlichen Dateisystemberechtigungen, damit der Berichtsserver ausgeführt werden kann.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [MSReportServer_ConfigurationSetting Class (MSReportServer_ConfigurationSetting-Klasse)](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-class.md)  
  
  
