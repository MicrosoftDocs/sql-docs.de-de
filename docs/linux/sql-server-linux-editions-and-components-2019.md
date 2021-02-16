---
description: Editionen und unterstützte Funktionen von SQL Server 2019 unter Linux
title: Editionen und unterstützte Funktionen von SQL Server 2019 – Linux
ms.date: 01/08/2020
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- default components
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.openlocfilehash: 933ce305104358f56246b2c31fd8fe52a07b03ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348148"
---
# <a name="editions-and-supported-features-of-sql-server-2019-on-linux"></a>Editionen und unterstützte Funktionen von SQL Server 2019 unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Dieser Artikel bietet detaillierte Informationen zu den Funktionen, die von den verschiedenen Editionen von SQL Server 2019 für Linux unterstützt werden. Informationen zu den Editionen und unterstützten Funktionen von SQL Server für Windows finden Sie unter [SQL Server 2019 – Windows](../sql-server/editions-and-components-of-sql-server-version-15.md).  
  
Die Installationsanforderungen variieren je nach den benötigten Anwendungen. Die verschiedenen Editionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tragen den individuellen Leistungs-, Laufzeit- und Preisanforderungen von Organisationen und Einzelpersonen Rechnung. Welche [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Komponenten Sie installieren, hängt auch von den individuellen Anforderungen ab. In den folgenden Abschnitten erfahren Sie, wie Sie die bestmögliche Auswahl unter den in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]verfügbaren Editionen und Komponenten treffen.  

Die neuesten Versionsanmerkungen und Informationen zu Neuerungen finden Sie über die folgenden Links:
- [SQL Server 2019 unter Linux: Anmerkungen zu dieser Version](sql-server-linux-release-notes-2019.md)
- [Neuigkeiten in SQL Server 2019 für Linux](sql-server-linux-whats-new-2019.md)

Eine Liste der SQL Server-Funktionen, die unter Linux nicht verfügbar sind, finden Sie unter [Nicht unterstützte Funktionen und Services](#Unsupported).

### <a name="try-sql-server"></a>Testen Sie SQL Server!    
    
[SQL Server 2019 herunterladen](https://www.microsoft.com/sql-server/sql-server-2019)

## <a name="ssnoversion-editions"></a>[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Editionen  
 In der folgenden Tabelle werden die Editionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]beschrieben. 
  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Edition|Definition|  
|---------------------------------------|----------------|  
|Enterprise|Das Premium-Angebot, [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise Edition, stellt umfassende hochwertige Datencenterfunktionen mit atemberaubend schneller Leistung bereit, die hohe Servicelevel für unternehmenswichtige Workloads ermöglicht.|  
|Standard|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard Edition stellt grundlegende Datenverwaltung für Abteilungen und kleinere Organisationen bereit, um deren Anwendungen auszuführen, und unterstützt allgemeine Entwicklungstools für lokale und Cloudanwendungen, wodurch eine effektive Datenbankverwaltung mit minimalen IT-Ressourcen ermöglicht wird.|  
|Web|Die[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Web Edition ist eine mit geringen Anschaffungs- und Betriebskosten verbundene Option für Webhoster und Web-VAPs, die kostengünstige Skalierbarkeit und Verwaltungsfunktionen für Webpräsenzen jeder Größe bietet.|  
|Entwickler|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer Edition ermöglicht Entwicklern das Erstellen beliebiger Anwendungen auf der Basis von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Sämtliche Funktionen der Enterprise Edition stehen zur Verfügung. Die Lizenz bezieht sich jedoch auf die Verwendung als Entwicklungs- und Testsystem und nicht als Produktionsserver. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer ist eine ideale Option zum Erstellen und Testen von Anwendungen.|  
|Express Edition|Die Express Edition ist eine kostenlose Datenbank auf Einstiegsebene und eignet sich ideal zum Üben und zum Erstellen von datengesteuerten Anwendungen für Desktopcomputer und kleine Server. Dies ist die beste Wahl für unabhängige Softwareanbieter, Entwickler und Tüftler, die Clientanwendungen erstellen. Wenn Sie erweiterte Datenbankfunktionen benötigen, können Sie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express nahtlos auf höhere Endversionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]aktualisieren.|  
  
## <a name="using-ssnoversion-with-clientserver-applications"></a>Verwenden von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mit Client-/Server-Anwendungen  

Sie können auch nur die Clientkomponenten von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf einem Computer mit Client-/Serveranwendungen installieren und so eine direkte Verbindung mit einer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Instanz herstellen. Die Installation der Clientkomponenten ist auch dann eine gute Wahl, wenn Sie eine Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf einem Datenbankserver verwalten, oder wenn Sie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anwendungen entwickeln möchten.  
  
## <a name="ssnoversion-components"></a>[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Komponenten  

SQL Server 2019 für Linux unterstützt die SQL Server-Datenbank-Engine. In der folgenden Tabelle sind die Features beschrieben, die es in der Datenbank-Engine gibt.   
  
|Serverkomponenten|BESCHREIBUNG|  
|-----------------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] enthält die [!INCLUDE[ssDE](../includes/ssde-md.md)]. Diese ist der Basisdienst für Speichern, Verarbeiten und Schützen von Daten, Replikation, Volltextsuche, Tools zum Verwalten von relationalen und XML-Daten und Integration von datenbankinternen Analysen.|  

**Developer, Enterprise Core-und Evaluation Edition**  
Informationen zu den Funktionen (Features), die von der Developer, Enterprise Core-und Evaluation Edition unterstützt werden, finden Sie in den Funktionen für die SQL Server Enterprise Edition in den folgenden Tabellen.

Die Developer Edition unterstützt weiterhin nur einen Client für [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md). 
  
##  <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a> Skalierungsgrenzen  
  
|Funktion|Enterprise|Standard|Web|Express| 
|-------------|----------------|--------------|---------|------------------------|
|Maximale von einer einzelnen Instanz verwendete Rechenkapazität ( [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Maximum des Betriebssystems|Beschränkt auf weniger als 4 Sockets oder 24 Kerne|Beschränkt auf weniger als 4 Sockets oder 16 Kerne|Beschränkt auf weniger als 1 Socket oder 4 Kerne| 
|Maximale von einer einzelnen Instanz verwendete Rechenkapazität ( [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] oder [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)])|Maximum des Betriebssystems|Beschränkt auf weniger als 4 Sockets oder 24 Kerne|Beschränkt auf weniger als 4 Sockets oder 16 Kerne|Beschränkt auf weniger als 1 Socket oder 4 Kerne|
|Maximaler Arbeitsspeicher für den Pufferpool pro Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Maximum des Betriebssystems|128 GB|64 GB|1410 MB|
|Maximaler Arbeitsspeicher für Columnstore-Segmentcache pro Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Unbegrenzter Arbeitsspeicher| 32 GB| 16 GB| 352 MB|  
|Maximale speicheroptimierte Datengröße pro Datenbank in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Unbegrenzter Arbeitsspeicher| 32 GB| 16 GB| 352 MB|
|Maximale relationale Datenbankgröße|524 PB|524 PB|524 PB|10 GB|  
  
<sup>1</sup> Die Enterprise Edition mit einer Lizenzierung auf der Grundlage von Serverlizenz + Clientzugriffslizenz (CAL) (für neue Verträge nicht verfügbar) ist auf maximal 20 Kerne pro SQL Server-Instanz beschränkt. Für das auf Prozessorkernen basierende Serverlizenzierungsmodell gelten keine Beschränkungen. Weitere Informationen finden Sie unter [Rechenkapazitätsgrenzen von bestimmten Editionen von SQL Server](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
 
##  <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a> RDBMS: Hochverfügbarkeit  
  
|Funktion|Enterprise|Standard|Web|Express|  
|-------------|----------------|--------------|---------|------------------------|  
|Protokollversand|Ja|Ja|Ja|Nein |  
|Sicherungskomprimierung|Ja|Ja|Nein |Nein | 
|Datenbankmomentaufnahme|Ja|Ja|Nein |Nein |
|Always On-Failoverclusterinstanzen<sup>1</sup>|Ja|Ja|Nein |Nein | 
|AlwaysOn-Verfügbarkeitsgruppen<sup>2</sup>|Ja|Nein |Nein |Nein |
|Basis-Verfügbarkeitsgruppen<sup>3</sup>|Nein |Ja|Nein |Nein |
|Mindestreplikate für Commitverfügbarkeitsgruppen|Ja|Ja|Nein |Nein |
|Verfügbarkeitsgruppe ohne Cluster|Ja|Ja|Nein |Nein |
|Onlineseiten- und Onlinedateiwiederherstellung|Ja|Nein |Nein |Nein |
|Online-Indizierung|Ja|Nein |Nein |Nein |
|Fortsetzbare Neuerstellung von online geschalteten Indizes|Ja|Nein |Nein |Nein |
|Onlineschemaänderung|Ja|Nein |Nein |Nein |
|Schnelle Wiederherstellung|Ja|Nein |Nein |Nein |
|Gespiegelte Sicherungen|Ja|Nein |Nein |Nein |
|Hinzufügen von Speicher im laufenden Systembetrieb und CPU|Ja|Nein |Nein |Nein |
|Verschlüsselte Sicherung|Ja|Ja|Nein |Nein |
|Hybridsicherung in Azure (Sicherung über URL)|Ja|Ja|Nein |Nein |
  
<sup>1</sup> Bei der Enterprise Edition entspricht die Anzahl der Knoten dem Maximum des Betriebssystems. Bei der Standard Edition werden nur zwei Knoten unterstützt. 

<sup>2</sup> Bei der Enterprise Edition werden bis zu acht sekundäre Replikate unterstützt, einschließlich zwei synchronen sekundären Replikaten. 

<sup>3</sup> Bei der Standard Edition werden Basis-Verfügbarkeitsgruppen unterstützt. Eine Basis-Verfügbarkeitsgruppe unterstützt zwei Replikate mit einer Datenbank. Weitere Informationen über Basis-Verfügbarkeitsgruppen finden Sie unter [Basis-Verfügbarkeitsgruppen](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).    

##  <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a> RDBMS: Skalierbarkeit und Leistung  
  
|Funktion|Enterprise|Standard|Web|Express|  
|-------------|----------------|--------------|---------|------------------------| 
|Columnstore <sup>1</sup>|Ja|Ja|Ja|Ja|  
|Große Objektbinärdateien in gruppierten Columnstore-Indizes|Ja|Ja|Ja|Ja|  
|Onlineneuerstellung für nicht gruppierten Columnstore-Index|Ja|Nein |Nein |Nein |
|In-Memory-OLTP <sup>1</sup>|Ja|Ja|Ja|Ja|
|Persistenter Hauptspeicher|Ja|Ja|Ja|Ja|
|Tabellen- und Indexpartitionierung|Ja|Ja|Ja|Ja|  
|Datenkomprimierung|Ja|Ja|Ja|Ja|
|Resource Governor|Ja|Nein |Nein |Nein |  
|Parallelverarbeitung für partitionierte Tabellen|Ja|Nein |Nein |Nein |
|NUMA-basierter und großer Arbeitsspeicher für umfangreiche Seiten und Zuordnung von Pufferarrays|Ja|Nein |Nein |Nein |
|Ressourcenkontrolle für E/A-Vorgänge|Ja|Nein |Nein |Nein |  
|Verzögerte Dauerhaftigkeit|Ja|Ja|Ja|Ja|
|Automatische Optimierung|Ja|Nein |Nein |Nein |
|Adaptive Joins im Batchmodus|Ja|Nein |Nein |Nein |
|Feedback zur Speicherzuweisung im Batchmodus|Ja|Nein |Nein |Nein |
|Verschachtelte Ausführung mit Tabellenwertfunktionen mit mehreren Anweisungen|Ja|Ja|Ja|Ja|
|Verbesserungen beim massenhaften Einfügen|Ja|Ja|Ja|Ja|


<sup>1</sup> Die Größe der In-Memory OLTP-Daten und des Columnstore-Segmentcaches sind auf die Größe des Arbeitsspeichers beschränkt, die von der Edition im Bereich Kapazitätsgrenzen festgelegt wird. Den maximale Grad an Parallelität ist beschränkt. Der Grad an Prozessparallelität (Degree of Parallelism, DOP) für eine Indexerstellung ist auf 2 DOP für die Standard Edition und auf 1 DOP für die Web und die Express Edition beschränkt. Dies gilt für Columnstore-Indizes, die über datenträgerbasierte Tabellen und speicheroptimierte Tabellen erstellt wurden.

##  <a name="rdbms-security"></a><a name="RDBMSS"></a> RDBMS: Sicherheit  
  
|Funktion|Enterprise|Standard|Web|Express|
|-------------|----------------|--------------|---------|------------------------------------| 
|Sicherheit auf Zeilenebene|Ja|Ja|Ja|Ja|  
|Always Encrypted|Ja|Ja|Ja|Ja| 
|Dynamische Datenmaskierung|Ja|Ja|Ja|Ja|   
|Allgemeine Überwachung|Ja|Ja|Ja|Ja| 
|Feine Überwachung|Ja|Ja|Ja|Ja| 
|Transparente Datenbankverschlüsselung|Ja|Ja|Nein |Nein |   
|Benutzerdefinierte Rollen|Ja|Ja|Ja|Ja| 
|Eigenständige Datenbanken|Ja|Ja|Ja|Ja| 
|Verschlüsselung von Sicherungen|Ja|Ja|Nein |Nein |  

##  <a name="rdbms-manageability"></a><a name="RDBMSM"></a> RDBMS: Verwaltbarkeit  
  
|Funktion|Enterprise|Standard|Web|Express|   
|-------------|----------------|--------------|---------|------------------------|  
|Dedizierte Administratorverbindung|Ja|Ja|Ja|Ja, mit Ablaufverfolgungsflag|   
|PowerShell-Skriptunterstützung|Ja|Ja|Ja|Ja| 
|Unterstützung für Komponentenvorgänge der Datenschichtanwendung: Extrahieren, Bereitstellen, Aktualisieren, Löschen|Ja|Ja|Ja|Ja| 
|Richtlinienautomatisierung (Überprüfung nach Zeitplan und Änderungen)|Ja|Ja|Ja|Nein |  
|Sammler von Leistungsdaten|Ja|Ja|Ja|Nein |
|Standardleistungsberichte|Ja|Ja|Ja|Nein |
|Planhinweislisten und Planeinfrierung für Planhinweislisten|Ja|Ja|Ja|Nein | 
|Direkte Abfrage von indizierten Sichten (mittels NOEXPAND-Hinweis)|Ja|Ja|Ja|Ja| 
|Automatische Wartung für indizierte Sichten|Ja|Ja|Ja|Nein |
|Verteilte partitionierte Sichten|Ja|Nein |Nein |Nein | 
|Parallele Indexvorgänge|Ja|Nein |Nein |Nein |  
|Automatische Verwendung indizierter Sichten mittels Abfrageoptimierer|Ja|Nein |Nein |Nein | 
|Parallele Konsistenzprüfung|Ja|Nein |Nein |Nein | 
|SQL Server-Steuerungspunkt für das Hilfsprogramm|Ja|Nein |Nein |Nein |    

##  <a name="programmability"></a><a name="Programmability"></a> Programmability  
  
|Funktion|Enterprise|Standard|Web|Express 
|-------------|----------------|--------------|---------|------------------------|  
|JSON|Ja|Ja|Ja|Ja|   
|Abfragespeicher|Ja|Ja|Ja|Ja|   
|Temporal|Ja|Ja|Ja|Ja|   
|Systemeigene XML-Unterstützung|Ja|Ja|Ja|Ja| 
|XML-Indizierung|Ja|Ja|Ja|Ja| 
|MERGE- und UPSERT-Funktionen|Ja|Ja|Ja|Ja|   
|Datums- und Uhrzeitdatentypen|Ja|Ja|Ja|Ja|  
|Internationalisierungsunterstützung|Ja|Ja|Ja|Ja| 
|Volltextsuche und semantische Suche|Ja|Ja|Ja|Ja|
|Angabe der Sprache in einer Abfrage|Ja|Ja|Ja|Ja|
|Service Broker (Messaging)|Ja|Ja|Nein (nur Client)|Nein (nur Client)|
|Transact-SQL-Endpunkte|Ja|Ja|Ja|Nein |
|Graph|Ja|Ja|Ja|Ja|  


<sup>1</sup> Die Aufskalierung mit mehreren Computeknoten erfordert einen Hauptknoten.

## <a name="integration-services"></a><a name="IS"></a> Integration Services

Informationen über die Features von Integration Services (SSIS), die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Von den SQL Server-Editionen unterstützte Integration Services-Funktionen](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

##  <a name="spatial-and-location-services"></a><a name="SLS"></a> Räumliche und ortsbezogene Dienste  
  
|Funktionsname|Enterprise|Standard|Web|Express|  
|------------------|----------------|--------------|---------|------------------------------------|------------------------|
|Räumlichkeitsindizes|Ja|Ja|Ja|Ja|   
|Planarer und geodätischer Datentyp|Ja|Ja|Ja|Ja| 
|Erweiterte räumliche Bibliotheken|Ja|Ja|Ja|Ja|   
|Importieren/Exportieren räumlicher Industriestandard-Datenformate|Ja|Ja|Ja|Ja|   

## <a name="unsupported-features--services"></a><a name="Unsupported"></a> Nicht unterstützte Features und Dienste

Die folgenden Features und Dienste stehen für SQL Server 2019 für Linux nicht zur Verfügung. Die Unterstützung dieser Features wird im Laufe der Zeit ausgeweitet.

| Bereich | Nicht unterstütztes Feature oder Dienst |
|-----|-----|
| **Datenbank-Engine** | Mergereplikation |
| &nbsp; | Stretch Database |
| &nbsp; | Verteilte Abfrage mit Drittanbieterverbindungen |
| &nbsp; | Verbindungsserver für andere Datenquellen als [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  |
| &nbsp; | Erweiterte gespeicherte Systemprozeduren (XP_CMDSHELL usw.) |
| &nbsp; | Dateitabelle, FILESTREAM |
| &nbsp; | CLR-Assemblys mit festgelegter EXTERNAL_ACCESS- oder UNSAFE-Berechtigung |
| &nbsp; | Pufferpoolerweiterung |
| &nbsp; | Sicherung über URLs: Seitenblob<sup>2</sup> |
| **SQL Server-Agent** |  Subsysteme: CmdExec, PowerShell, Queue Reader, SSIS, SSAS, SSRS |
| &nbsp; | Alerts |
| &nbsp; | Managed Backup |
| **Hochverfügbarkeit** | Datenbankspiegelung  |
| **Security** | Erweiterbare Schlüsselverwaltung |
| &nbsp; | Azure AD-Authentifizierung für Verbindungsserver | 
| &nbsp; | Azure AD-Authentifizierung für Verfügbarkeitsgruppen | 
| **Dienste** | SQL Server-Browser |
| &nbsp; | SQL Server R Services<sup>1</sup> |
| &nbsp; | StreamInsight |
| &nbsp; | Analysis Services |
| &nbsp; | Reporting Services |
| &nbsp; | Data Quality Services |
| &nbsp; | Master Data Services |

<sup>1</sup> SQL Server r wird in SQL Server unterstützt, aber SQL Server R Services als separates Paket wird nicht unterstützt.

<sup>2</sup> Die URL-Sicherung wird für Blockblobs unterstützt, wobei die [Shared Access Signature](../relational-databases/backup-restore/sql-server-backup-to-url.md#SAS) verwendet wird.

## <a name="next-steps"></a>Nächste Schritte
 [Editionen und unterstützten Funktionen von SQL Server 2017 – Linux](sql-server-linux-editions-and-components-2017.md)  
 [Editionen und unterstützte Funktionen von SQL Server 2019 – Windows](../sql-server/editions-and-components-of-sql-server-version-15.md)  
 [Editionen und unterstützte Funktionen von SQL Server 2017 – Windows](../sql-server/editions-and-components-of-sql-server-2017.md)  
 [Editionen und unterstützte Funktionen von SQL Server 2016 – Windows](../sql-server/editions-and-components-of-sql-server-2016.md)  
 [Installation for SQL Server (Installation für SQLServer 2016)](../database-engine/install-windows/install-sql-server.md)  
 [Produktspezifikationen für SQL Server](../sql-server/index.yml)
