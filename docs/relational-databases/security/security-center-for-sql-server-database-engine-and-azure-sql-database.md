---
title: Sicherheitsdokumentation für SQL Server und Azure SQL-Datenbank
description: Eine Referenz zu sicherheits- und schutzbezogenen Inhalten für SQL Server und Azure SQL-Datenbank.
ms.custom: seo-lt-2019
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- Security [SQL Server]
helpviewer_keywords:
- SQL Server, security
- security [SQL Server]
- database security [SQL Server]
- databases [SQL Server], security
ms.assetid: dfb39d16-722a-4734-94bb-98e61e014ee7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 22df18f14ddb3eccee24944fa00822cb3176bcfa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340203"
---
# <a name="security-center-for-sql-server-database-engine-and-azure-sql-database"></a>Sicherheitscenter für SQL Server-Datenbank-Engine und Azure SQL-Datenbank
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Diese Seite enthält Links, mit deren Hilfe Sie die erforderlichen Informationen zur Sicherheit und zum Schutz in [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] und [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]finden.  
  
 **Legende**  
  
 ![Screenshot: Legende zur Erklärung der Featureverfügbarkeitssymbole](../performance/media/security-center-legend.PNG "security-center-legend")  
  
##  <a name="authentication-who-are-you"></a><a name="Who"></a> Authentifizierung Wer sind Sie?  
  
|||  
|-|-|  
|**Wer authentifiziert?**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Windows-Authentifizierung<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentifizierung<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Azure Active Directory|Wer authentifiziert? (Windows oder [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])<br /><br /> [Auswählen eines Authentifizierungsmodus](../../relational-databases/security/choose-an-authentication-mode.md)<br /><br /> [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](/azure/azure-sql/database/authentication-aad-overview)|  
|**Wo authentifiziert?**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Bei der master-Datenbank: Anmeldenamen und Datenbankbenutzer<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Bei der Benutzerdatenbank: Eigenständige Datenbankbenutzer|Authentifizierung bei der Masterdatenbank (Anmeldenamen und Datenbankbenutzer)<br /><br /> [Erstellen eines SQL Server-Anmeldenamens](../../relational-databases/security/authentication-access/create-a-login.md)<br /><br /> [Verwalten von Datenbanken und Anmeldungen in Azure SQL-Datenbank](/previous-versions/azure/ee336235(v=azure.100))<br /><br /> [Erstellen eines Datenbankbenutzers](../../relational-databases/security/authentication-access/create-a-database-user.md)<br /><br /> <br /><br /> Authentifizierung bei einer Benutzerdatenbank<br /><br /> [Eigenständige Datenbankbenutzer – Generieren einer portablen Datenbank](../../relational-databases/security/contained-database-users-making-your-database-portable.md)|  
|**Verwenden anderer Identitäten**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Anmeldeinformationen<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Ausführen unter anderem Anmeldenamen<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Ausführen als anderer Datenbankbenutzer|[Anmeldeinformationen &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)<br /><br /> [Ausführen unter anderem Anmeldenamen](../../t-sql/statements/execute-as-transact-sql.md)<br /><br /> [Ausführen als anderer Datenbankbenutzer](../../t-sql/statements/execute-as-transact-sql.md)|  
  
##  <a name="authorization-what-can-you-do"></a><a name="What"></a> Autorisierung Was können Sie tun?  
  
|||  
|-|-|  
|**Gewährung, Widerrufen und Verweigern von Berechtigungen**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Sicherungsfähige Klassen<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Granulare Serverberechtigungen<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Granulare Datenbankberechtigungen|[Berechtigungshierarchie &#40;Datenbank-Engine&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)<br /><br /> [Berechtigungen](../../relational-databases/security/permissions-database-engine.md)<br /><br /> [Sicherungsfähige Elemente](../../relational-databases/security/securables.md)<br /><br /> [Erste Schritte mit Berechtigungen für die Datenbank-Engine](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)|  
|**Sicherheit durch Rollen**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Rollen auf Serverebene<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Rollen auf Datenbankebene|[Rollen auf Serverebene](../../relational-databases/security/authentication-access/server-level-roles.md)<br /><br /> [Rollen auf Datenbankebene](../../relational-databases/security/authentication-access/database-level-roles.md)|  
|**Einschränken des Datenzugriffs auf ausgewählte Datenelemente**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Einschränken des Datenzugriffs mit Ansichten/Verfahren<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Sicherheit auf Zeilenebene<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Dynamische Datenmaskierung<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Signierte Objekte|Einschränken des Datenzugriffs mit [Ansichten](../../relational-databases/views/views.md) und [Verfahren](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)<br /><br /> [Sicherheit auf Zeilenebene (SQL Server)](../../relational-databases/security/row-level-security.md)<br /><br /> [Sicherheit auf Zeilenebene (Azure SQL-Datenbank)](./row-level-security.md)<br /><br /> [Dynamische Datenmaskierung (SQL Server)](../../relational-databases/security/dynamic-data-masking.md)<br /><br /> [Dynamische Datenmaskierung (Azure SQL-Datenbank)](/azure/azure-sql/database/dynamic-data-masking-overview)<br /><br /> [Signierte Objekte](../../t-sql/statements/add-signature-transact-sql.md)|  
  
##  <a name="encryption-storing-secret-data"></a><a name="Encrypt"></a> Verschlüsselung: Speichern geheimer Daten  
  
|||  
|-|-|  
|**Verschlüsseln von Dateien**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: BitLocker-Verschlüsselung (Laufwerksebene)<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: NTFS-Verschlüsselung (Ordnerebene)<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Transparent Data Encryption (Dateiebene)<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Sicherungsverschlüsselung (Dateiebene)|[BitLocker (Laufwerksebene)](https://support.microsoft.com/kb/2855131)<br /><br /> [NTFS-Verschlüsselung (Ordnerebene)](/previous-versions/tn-archive/dd163562(v=technet.10))<br /><br /> [Transparente Datenverschlüsselung (Dateiebene)](../../relational-databases/security/encryption/transparent-data-encryption.md)<br /><br /> [Sicherungsverschlüsselung (Dateiebene)](../../relational-databases/backup-restore/backup-encryption.md)|  
|**Verschlüsseln von Quellen**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Erweiterbares Schlüsselverwaltungsmodul<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: In Azure Key Vault gespeicherte Schlüssel<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Always Encrypted|[Erweiterbares Schlüsselverwaltungsmodul](../../relational-databases/security/encryption/extensible-key-management-ekm.md)<br /><br /> [Im Azure Schlüsseltresor gespeicherte Schlüssel](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)<br /><br /> [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)|  
|**Spalte, Daten und Schlüsselverschlüsselung**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Verschlüsseln durch Zertifikat<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Verschlüsseln mit symmetrischem Schlüssel<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Verschlüsseln mit asymmetrischem Schlüssel<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Verschlüsseln durch Passphrase|[Verschlüsseln durch Zertifikat](../../t-sql/functions/encryptbycert-transact-sql.md)<br /><br /> [Verschlüsseln mit asymmetrischem Schlüssel](../../t-sql/functions/encryptbyasymkey-transact-sql.md)<br /><br /> [Verschlüsseln mit symmetrischem Schlüssel](../../t-sql/functions/encryptbykey-transact-sql.md)<br /><br /> [Verschlüsseln durch Passphrase](../../t-sql/functions/encryptbypassphrase-transact-sql.md)<br /><br /> [Verschlüsseln einer Datenspalte](../../relational-databases/security/encryption/encrypt-a-column-of-data.md)|  
  
##  <a name="connection-security-restricting-and-securing"></a><a name="Connect"></a> Verbindungssicherheit: Einschränken und Sichern  
  
|||  
|-|-|  
|**Firewallschutz**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Windows-Firewalleinstellungen<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Azure Service-Firewalleinstellungen<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Datenbank-Firewalleinstellungen|[Konfigurieren einer Windows-Firewall für Datenbank-Engine-Zugriff](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)<br /><br /> [Azure SQL-Datenbank-Firewalleinstellungen](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)<br /><br /> [Azure Service-Firewalleinstellungen](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)|  
|**Verschlüsseln von Daten in Transit**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Erzwungene SSL-Verbindungen<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Optionale SSL-Verbindungen|[Aktivieren von verschlüsselten Verbindungen mit der Datenbank-Engine](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)<br /><br /> [Aktivieren von verschlüsselten Verbindungen mit der Datenbank-Engine](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md), [Netzwerksicherheit](/azure/sql-database/sql-database-security-best-practice#network-security) <br /><br /> [TLS 1.2-Unterstützung für Microsoft SQL Server](https://support.microsoft.com/kb/3135244)|  
  
##  <a name="auditing-recording-access"></a><a name="Audit"></a> Überwachung: Aufzeichnen des Zugriffs  
  
|||  
|-|-|  
|**Automatisierte Überwachung**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Überwachung (Server- und Datenbankebene)<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Überwachung (Datenbankebene)<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Erkennen von Bedrohungen| <br /><br /> [SQL Server Audit &#40;Datenbank-Engine&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)<br /><br /> [SQL-Datenbanküberwachung](/azure/azure-sql/database/auditing-overview)<br /><br /> [Erste Schritte mit Advanced Threat Protection für Azure SQL-Datenbank](/azure/azure-sql/database/threat-detection-configure) <br /><br /> [Sicherheitsrisikobewertung mit der SQL-Datenbank](/azure/sql-database/sql-vulnerability-assessment) |  
|**Benutzerdefinierte Überwachung**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-both.png":::-Trigger|Benutzerdefinierte Überwachungsimplementierung: Erstellen von [DDL Triggers](../../relational-databases/triggers/ddl-triggers.md) und [DML Triggers](../../relational-databases/triggers/dml-triggers.md)|  
|**Compliance**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: Compliance|SQL Server:<br />                        [Common Criteria](https://go.microsoft.com/fwlink/?LinkId=616319)<br /><br /> SQL-Datenbank:<br />                        [Microsoft Azure Trust Center: Compliance nach Funktion](https://azure.microsoft.com/support/trust-center/services/)|  
  
##  <a name="sql-injection"></a><a name="SQLInjection"></a> SQL Injection  
 Bei der Einschleusung von SQL-Befehlen wird ein bösartiger Code in Zeichenfolgen eingefügt, die später zur Analyse und Ausführung an [!INCLUDE[ssDE](../../includes/ssde-md.md)] übergeben werden. Sie sollten jede Prozedur, die SQL-Anweisungen erstellt, nach Injection-Anfälligkeiten überprüfen, denn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] führt alle empfangenen gültigen Abfragen aus. Alle Datenbanksysteme unterliegen einem gewissen Risiko für die Einschleusung von SQL-Befehlen, und viele der Sicherheitslücken entstehen in der Anwendung, die [!INCLUDE[ssDE](../../includes/ssde-md.md)]abfragt. Sie können Angriffe, die das Ziel haben, SQL-Befehle einzuschleusen, abwehren, indem Sie gespeicherte Prozeduren und parametrisierte Befehle verwenden, dynamisches SQL vermeiden und die Berechtigungen aller Benutzer einschränken.  Weitere Informationen finden Sie unter [Einschleusung von SQL-Befehlen](../../relational-databases/security/sql-injection.md).  
  
 Zusätzliche Links für Anwendungsprogrammierer:  
  
-   [Anwendungssicherheitsszenarios in SQL Server](/dotnet/framework/data/adonet/sql/application-security-scenarios-in-sql-server)  
  
-   [Schreiben von sicherem dynamischem SQL in SQL Server](/dotnet/framework/data/adonet/sql/writing-secure-dynamic-sql-in-sql-server)  
  
-   [Vorgehensweise: How To: Protect From SQL Injection in ASP.NET (Schutz vor Einschleusung von SQL-Befehlen in ASP.NET)](/previous-versions/msp-n-p/ff648339(v=pandp.10))  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erste Schritte mit Berechtigungen für die Datenbank-Engine](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)   
 [Sichern von SQL Server](../../relational-databases/security/securing-sql-server.md)   
 [Prinzipale &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [SQL Server-Zertifikate und asymmetrische Schlüssel](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)   
 [SQL Server-Verschlüsselung](../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Oberflächenkonfiguration](../../relational-databases/security/surface-area-configuration.md)   
 [Sichere Kennwörter](../../relational-databases/security/strong-passwords.md)   
 [TRUSTWORTHY-Datenbankeigenschaft](../../relational-databases/security/trustworthy-database-property.md)   
 [Datenbank-Engine-Funktionen und Tasks](../../sql-server/what-s-new-in-sql-server-ver15.md)  
 [Schutz Ihres geistigen SQL Server-Eigentums](../../relational-databases/security/protecting-your-sql-server-intellectual-property.md)   
  
[!INCLUDE[get-help-security](../../includes/get-help-security.md)]