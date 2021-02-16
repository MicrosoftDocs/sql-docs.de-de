---
title: Sichern von SQL Server | Microsoft-Dokumentation
description: Verwenden Sie diese Artikel, um einen effektiven Sicherheitsplan in SQL Server zu implementieren. Erfahren Sie mehr über die Plattform, Authentifizierung, Objekte und Anwendungen.
ms.custom: ''
ms.date: 06/21/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- Security [SQL Server]
helpviewer_keywords:
- database objects [SQL Server], security
- SQL Server, security
- operating systems [SQL Server], security
- security [SQL Server], planning
- applications [SQL Server], security
ms.assetid: 4d93489e-e9bb-45b3-8354-21f58209965d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2f3082f83296efe396fa2fca447782e13588df38
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340120"
---
# <a name="securing-sql-server"></a>Sichern von SQL Server

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Das Sichern von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann als eine Reihe von Schritten betrachtet werden, die vier Bereiche betreffen: die Plattform, die Authentifizierung, die Objekte (einschließlich der Daten) und die Anwendungen, die auf das System zugreifen. In den folgenden Themen werden Sie durch das Erstellen und Implementieren eines effektiven Sicherheitsplans geführt.  
  
 Weitere Informationen zur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherheit finden Sie auf der [SQL Server](https://go.microsoft.com/fwlink/?LinkID=31629) -Website. Dazu gehören ein Handbuch mit empfohlenen Vorgehensweisen sowie eine Sicherheitsprüfliste. Auf der Site finden Sie außerdem die neuesten Informationen und Downloads zu Service Packs.  
  
## <a name="platform-and-network-security"></a>Plattform- und Netzwerksicherheit  
 Die Plattform für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] enthält die physische Hardware und die Netzwerksysteme zum Verbinden von Clients mit den Datenbankservern, sowie binäre Dateien zum Verarbeiten von Datenbankanforderungen.  
  
### <a name="physical-security"></a>Physische Sicherheit  
 Durch die empfohlenen Vorgehensweisen für physische Sicherheit wird der Zugriff auf den physischen Server und Hardwarekomponenten beschränkt. Verwenden Sie beispielsweise abgeschlossene Räume mit eingeschränktem Zugang für die Datenbankserverhardware und für Netzwerkgeräte. Beschränken Sie außerdem den Zugriff auf Sicherungsmedien durch Aufbewahren der Medien an einem sicheren, getrennten Ort.  
  
 Das Implementieren der physischen Netzwerksicherheit beginnt mit dem Fernhalten unbefugter Benutzer vom Netzwerk. Die folgende Tabelle enthält weitere Informationen zu Netzwerksicherheitsinformationen.  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|[!INCLUDE[ssEW](../../includes/ssew-md.md)] und Netzwerkzugriff auf andere Editionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|"Konfigurieren und Sichern der Serverumgebung" in der Onlinedokumentation von [!INCLUDE[ssEW](../../includes/ssew-md.md)]|  
| &nbsp; | &nbsp; |
  
### <a name="operating-system-security"></a>Sicherheit des Betriebssystems  
 Service Packs und Upgrades für Betriebssysteme enthalten wichtige Sicherheitserweiterungen. Wenden Sie alle Updates und Upgrades nach dem Testen mit den Datenbankanwendungen auf das Betriebssystem an.  
  
 Firewalls stellen ebenfalls effektive Möglichkeiten zum Implementieren von Sicherheit zur Verfügung. Eine Firewall trennt oder beschränkt logisch den Netzwerkverkehr und kann zum Verstärken der Datensicherheitsrichtlinie der Organisation konfiguriert werden. Wenn Sie eine Firewall verwenden, wird die Sicherheit auf Betriebssystemebene erhöht, indem ein Punkt zur Verfügung gestellt wird, auf den der Sicherheitsmaßstab ausgerichtet werden kann. Die folgende Tabelle enthält weitere Informationen zum Verwenden einer Firewall mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|Konfigurieren einer Firewall für das Funktionieren mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Konfigurieren einer Windows-Firewall für Datenbank-Engine-Zugriff](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)|  
|Konfigurieren einer Firewall für das Funktionieren mit [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|[Integration Services-Dienst &#40;SSIS-Dienst&#41;](../../integration-services/service/integration-services-service-ssis-service.md)|  
|Konfigurieren einer Firewall für das Funktionieren mit [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|[Konfigurieren der Windows-Firewall, um den Zugriff auf Analysis Services zuzulassen](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)|  
|Öffnen bestimmter Ports für eine Firewall zum Zulassen des Zugriffs auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Konfigurieren der Windows-Firewall für den SQL Server-Zugriff](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)|  
|Konfigurieren von Unterstützung für den erweiterten Schutz für die Authentifizierung mit Channelbindung und Dienstbindung|[Herstellen einer Verbindung mit der Datenbank-Engine unter Verwendung von Erweiterter Schutz](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md)|  
| &nbsp; | &nbsp; |
  
 Die Oberflächenreduzierung stellt eine Sicherheitsmaßnahme dar, die das Beenden oder Deaktivieren nicht verwendeter Komponenten beinhaltet. Mithilfe der Oberflächenreduzierung kann die Sicherheit verbessert werden, da weniger Möglichkeiten für Angriffe auf das System vorhanden sind. Der wichtigste Aspekt beim Beschränken des Oberflächenbereichs von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] liegt im Ausführen erforderlicher Dienste mit "geringsten Rechten" durch ausschließliches Erteilen von entsprechenden Rechten an Dienste und Benutzer. Die folgende Tabelle enthält weitere Informationen zu Diensten und Systemzugriff.  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|Für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Konfigurieren von Windows-Dienstkonten und -Berechtigungen](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)|  
| &nbsp; | &nbsp; |
  
 Wenn Ihr [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -System Internetinformationsdienste (Internet Information Services, IIS) verwendet, sind zusätzliche Schritte zum Sichern der Plattformoberfläche erforderlich. Die folgende Tabelle enthält Informationen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und Internetinformationsdiensten.  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|IIS-Sicherheit mit [!INCLUDE[ssEW](../../includes/ssew-md.md)]|"IIS-Sicherheit" in der Onlinedokumentation von [!INCLUDE[ssEW](../../includes/ssew-md.md)]|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Authentifizierung|[Authentifizierung in Reporting Services](../../reporting-services/extensions/security-extension/authentication-in-reporting-services.md)|  
|[!INCLUDE[ssEW](../../includes/ssew-md.md)] und IIS-Zugriff|"Internetinformationsdienste-Sicherheitsflussdiagramm" in der Onlinedokumentation von [!INCLUDE[ssEW](../../includes/ssew-md.md)]|  
| &nbsp; | &nbsp; |
  
### <a name="sql-server-operating-system-files-security"></a>SQL Server-Betriebssystemdateisicherheit  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden Betriebssystemdateien zum Betrieb und zur Datenspeicherung verwendet. Die empfohlene Vorgehensweise für die Dateisicherheit erfordert, den Zugriff auf diese Dateien zu beschränken. Die folgende Tabelle enthält Informationen zu diesen Dateien.  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Programmdateien|[Dateispeicherorte für Standard- und benannte Instanzen von SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md)|  
| &nbsp; | &nbsp; |
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Service Packs und Upgrades stellen erweiterte Sicherheit zur Verfügung. Informationen zum Bestimmen des neuesten für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verfügbaren Service Packs finden Sie auf der Website von [SQL Server](https://go.microsoft.com/fwlink/?LinkID=31629) .  
  
 Mithilfe des folgenden Skripts können Sie das auf dem System installierte Service Pack bestimmen:  
  
```sql
SELECT CONVERT(char(20), SERVERPROPERTY('productlevel'));  
```  
  
## <a name="principals-and-database-object-security"></a>Prinzipale und Datenbankobjektsicherheit  
 Prinzipale sind die Personen, Gruppen und Prozesse, denen Zugriff auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]erteilt wurde. „Sicherungsfähige Elemente“ sind der Server, die Datenbank und Objekte in der Datenbank. Jedes der Elemente verfügt über einen Berechtigungssatz, der zum Reduzieren des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Oberflächenbereichs konfiguriert werden kann. In der folgenden Tabelle sind Informationen zu Prinzipalen und sicherungsfähigen Elementen enthalten.  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|Benutzer, Rollen und Prozesse von Servern und Datenbanken|[Prinzipale &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)|  
|Server- und Datenbankobjektsicherheit|[Sicherungsfähige Elemente](../../relational-databases/security/securables.md)|  
|Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherheitshierarchie|[Berechtigungshierarchie &#40;Datenbank-Engine&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)|  
| &nbsp; | &nbsp; |
  
### <a name="encryption-and-certificates"></a>Verschlüsselung und Zertifikate  
 Durch Verschlüsselung werden keine Probleme der Zugriffssteuerung gelöst. Sie erhöht jedoch die Sicherheit, indem Datenverluste selbst im seltenen Fall einer überbrückten Zugriffssteuerung beschränkt werden. Wenn der Datenbankhostcomputer beispielsweise falsch konfiguriert wurde und ein böswilliger Benutzer Zugriff auf sensible Daten wie Kreditkartennummern gewinnt, sind die gestohlenen Informationen nutzlos, wenn sie verschlüsselt wurden. Die folgende Tabelle enthält weitere Informationen zur Verschlüsselung in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|Die Verschlüsselungshierarchie in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)|  
|Implementieren sicherer Verbindungen|[Aktivieren von verschlüsselten Verbindungen zur Datenbank-Engine &#40;SQL Server-Konfigurations-Manager&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)|  
|Verschlüsselungsfunktionen|[Kryptografiefunktionen &#40;Transact-SQL&#41;](../../t-sql/functions/cryptographic-functions-transact-sql.md)|  
  
 Zertifikate sind "Softwareschlüssel", die für zwei Server freigegeben sind, durch die eine sichere Kommunikation über starke Authentifizierung möglich ist. Zertifikate können in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zum Erweitern der Objekt- und Verbindungssicherheit erstellt und verwendet werden. Die folgende Tabelle enthält Informationen zum Verwenden von Zertifikaten mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|Erstellen eines Zertifikats zum Verwenden mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)|  
|Verwenden eines Zertifikats mit Datenbankspiegelung|[Verwenden von Zertifikaten für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)|  
| &nbsp; | &nbsp; |

## <a name="application-security"></a>Anwendungssicherheit

### <a name="client-programs"></a>Client-Programme

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gehört das Schreiben sicherer Clientanwendungen. Weitere Informationen zum Sichern von Clientanwendungen auf Netzwerkebene finden Sie unter [Client Network Configuration](../../database-engine/configure-windows/client-network-configuration.md).

### <a name="windows-defender-application-control-wdac"></a>Windows Defender Application Control (WDAC)

<!--
This next live paragraph, about Windows Defender Application Control (WDAC), was requested by Bella Brahm, 2019/06/20. (GeneMi)

WDAC can also prevent the kind of highly sophisticated 'Nansh0u' attacks described in 'https://www.guardicore.com/2019/05/nansh0u-campaign-hackers-arsenal-grows-stronger/'. That webpage recommends this present article.
-->

Windows Defender Application Control (WDAC) verhindert die nicht autorisierte Ausführung von Code. WDAC ist eine effektive Methode, um die Bedrohung durch auf ausführbaren Dateien basierende Malware zu minimieren. Weitere Informationen finden Sie in der Dokumentation zu [Windows Defender Application Control](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control).

## <a name="sql-server-security-tools-utilities-views-and-functions"></a>SQL Server-Sicherheitstools, -Hilfsprogramme, -Sichten und -Funktionen  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden Tools, Hilfsprogramme, Sichten und Funktionen zum Konfigurieren und Verwalten der Sicherheit zur Verfügung gestellt.  
  
### <a name="sql-server-security-tools-and-utilities"></a>SQL Server-Sicherheitstools und -Hilfsprogramme  
 Die folgende Tabelle enthält Informationen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Tools und -Hilfsprogrammen zum Konfigurieren und Verwalten der Sicherheit.  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|Herstellen einer Verbindung mit, Konfigurieren und Steuern von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Verwenden von SQL Server Management Studio](../../ssms/sql-server-management-studio-ssms.md)|  
|Herstellen einer Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und Ausführen von Abfragen an der Eingabeaufforderung|[SQLCMD-Hilfsprogramm](../../tools/sqlcmd-utility.md)|  
|Netzwerkkonfiguration und Steuerung für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[SQL Server-Konfigurations-Manager](../../relational-databases/sql-server-configuration-manager.md)|  
|Aktivieren und Deaktivieren von Funktionen mit der richtlinienbasierten Verwaltung|[Verwalten von Servern mit der richtlinienbasierten Verwaltung](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)|  
|Bearbeiten symmetrischer Schlüssel für einen Berichtsserver|[rskeymgmt-Hilfsprogramm &#40;SSRS&#41;](../../reporting-services/tools/rskeymgmt-utility-ssrs.md)|  
| &nbsp; | &nbsp; |

### <a name="sql-server-security-catalog-views-and-functions"></a>SQL Server-Sicherheitskatalogsichten und -Funktionen  
 In [!INCLUDE[ssDE](../../includes/ssde-md.md)] werden Sicherheitsinformationen in zahlreichen Sichten und Funktionen dargestellt, die für Leistung und Nützlichkeit optimiert sind. Die folgende Tabelle enthält Informationen zu Sicherheitssichten und -funktionen.  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sicherheitskatalogsichten, durch die Informationen zu Berechtigungen, Rollen, Prinzipalen usw. auf Datenbankebene und Serverebene zurückgegeben werden. Außerdem sind Katalogsichten mit Informationen zu Verschlüsselungsschlüsseln, Zertifikaten und Anmeldeinformationen vorhanden.|[Sicherheitskatalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherheitsfunktionen, durch die Informationen zum aktuellen Benutzer, zu Berechtigungen und zu Schemas zurückgegeben werden.|[Sicherheitsfunktionen &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherheit.|[Sicherheitsbezogene dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)|  
| &nbsp; | &nbsp; |

## <a name="related-content"></a>Verwandte Inhalte  
 [Überlegungen zur Sicherheit bei SQL Server-Installationen](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
 [Sicherheitscenter für SQL Server-Datenbank-Engine und Azure SQL-Datenbank](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
[SQL Server 2012 Security Best Practices - Operational and Administrative Tasks (Bewährte Sicherheitsmethoden in SQL Server 2012 – betriebs- und administrationsbezogene Aufgaben)](https://download.microsoft.com/download/8/F/A/8FABACD7-803E-40FC-ADF8-355E7D218F4C/SQL_Server_2012_Security_Best_Practice_Whitepaper_Apr2012.docx)   
[SQL Server Security Blog (SQL Server Blog zu Sicherheitsthemen)](/archive/blogs/sqlsecurity/)  
[Security Best Practice and Label Security Whitepapers (Bewährte Sicherheitsmethoden und Sicherheitswhitepaper)](/archive/blogs/sqlsecurity/security-best-practice-and-label-security-whitepapers)  
[Sicherheit auf Zeilenebene](../../relational-databases/security/row-level-security.md)   
[Schutz Ihres geistigen SQL Server-Eigentums](../../relational-databases/security/protecting-your-sql-server-intellectual-property.md)   
