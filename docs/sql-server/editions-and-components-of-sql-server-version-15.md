---
description: Editionen und unterstützte Funktionen von [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]
title: Editionen und unterstützte Features von SQL Server 2019 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- 32-bit vs. 64-bit editions [SQL Server]
- default components
- Workgroup Edition [SQL Server]
- Internet servers [SQL Server]
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- client/server applications [SQL Server]
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- 64-bit edition [SQL Server]
- IIS [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 98b630f4471d35259db609a5bf1740ff96b8d604
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336173"
---
# <a name="editions-and-supported-features-of-sssql19-md"></a>Editionen und unterstützte Funktionen von [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx](../includes/applies-to-version/sqlserver.md)]

Dieses Thema enthält detaillierte Informationen zu den von den verschiedenen Editionen von [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] unterstützten Features.

Informationen zu früheren Versionen finden Sie unter:

* [SQL Server 2017](editions-and-components-of-sql-server-2017.md)
* [SQL Server 2016](editions-and-components-of-sql-server-2016.md)

Die Installationsanforderungen variieren je nach den benötigten Anwendungen. Die verschiedenen Editionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tragen den individuellen Leistungs-, Laufzeit- und Preisanforderungen von Organisationen und Einzelpersonen Rechnung. Welche [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Komponenten Sie installieren, hängt auch von den individuellen Anforderungen ab. In den folgenden Abschnitten erfahren Sie, wie Sie die bestmögliche Auswahl unter den in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]verfügbaren Editionen und Komponenten treffen.

Die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Evaluation Edition steht für einen Testzeitraum von 180 Tagen zur Verfügung.

Die neuesten Versionsanmerkungen und Informationen zu Neuerungen finden Sie über die folgenden Links:

* [Versionshinweise zu [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]](../sql-server/sql-server-version-15-release-notes.md)
* [Neuerungen in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2019](../sql-server/what-s-new-in-sql-server-ver15.md)

**Testen Sie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]: [Laden Sie [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] aus dem Evaluation Center](https://www.microsoft.com//evalcenter/evaluate-sql-server) herunter.**

## <a name="ssnoversion-editions"></a>[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Editionen

In der folgenden Tabelle werden die Editionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]beschrieben.

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Edition|Definition|
|---------------------------------------|----------------|
|Enterprise|Das Premium-Angebot, [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise Edition, bietet umfassende hochwertige Rechenzentrumsfunktionen mit blitzschneller Performance, unbegrenzter Virtualisierung<sup>1</sup> und umfassender Business Intelligence. Sie erhalten damit hohe Servicelevel für unternehmenskritische Workloads und Endbenutzerzugriff auf Datenerkenntnisse.|
|Standard|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard Edition stellt eine grundlegende Datenverwaltungs- und Business Intelligence-Datenbank bereit, auf der Abteilungen und kleinere Unternehmen ihre Anwendungen ausführen. Diese unterstützt allgemeine Entwicklungstools für den lokalen Einsatz und die Cloud und ermöglicht eine effektive Datenbankverwaltung mit minimalen IT-Ressourcen.|
|Web|Die [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Web Edition ist eine mit geringen Anschaffungs- und Betriebskosten verbundene Option für Webhoster und Web-VAPs, die kostengünstige Skalierbarkeit und Verwaltungsfunktionen für Webpräsenzen jeder Größe bietet.|
|Entwickler|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer Edition ermöglicht Entwicklern das Erstellen beliebiger Anwendungen auf der Basis von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Sämtliche Funktionen der Enterprise Edition stehen zur Verfügung. Die Lizenz bezieht sich jedoch auf die Verwendung als Entwicklungs- und Testsystem und nicht als Produktionsserver. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer ist eine ideale Option zum Erstellen und Testen von Anwendungen.|
|Express-Editionen|Die Express Edition ist eine kostenlose Datenbank auf Einstiegsebene und eignet sich ideal zum Üben und zum Erstellen von datengesteuerten Anwendungen für Desktopcomputer und kleine Server. Dies ist die beste Wahl für unabhängige Softwareanbieter, Entwickler und Tüftler, die Clientanwendungen erstellen. Wenn Sie erweiterte Datenbankfunktionen benötigen, können Sie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express nahtlos auf höhere Endversionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]aktualisieren. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express LocalDB ist eine neue Light-Version von Express, die sämtliche Programmierbarkeitsfunktionen besitzt, wird im Benutzermodus ausgeführt und stellt eine schnelle, konfigurationsfreie Installation und eine kurze Liste an Voraussetzungen bereit.|

<sup>1</sup> Die unbegrenzte Virtualisierung steht Kunden mit [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) in der Enterprise Edition zur Verfügung. Bereitstellungen müssen die Lizenzierungsrichtlinie einhalten. Weitere Informationen finden Sie auf der Seite zu Preisen und Lizenzierung.

## <a name="using-ssnoversion-with-clientserver-applications"></a>Verwenden von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mit Client-/Server-Anwendungen

Sie können auch nur die Clientkomponenten von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf einem Computer mit Client-/Serveranwendungen installieren und so eine direkte Verbindung mit einer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Instanz herstellen. Die Installation der Clientkomponenten ist auch dann eine gute Wahl, wenn Sie eine Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf einem Datenbankserver verwalten, oder wenn Sie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anwendungen entwickeln möchten.

Die Clienttools-Option installiert die folgenden [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Funktionen: Abwärtskompatibilitätskomponenten, [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], Konnektivitätskomponenten, Verwaltungstools, Software Development Kit und [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Onlinedokumentations-Komponenten. Weitere Informationen finden Sie unter [Installieren von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../database-engine/install-windows/install-sql-server.md).

### <a name="running-with-iis"></a>Ausführen mit IIS

Die Clienttools von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] werden normalerweise auf einem Internetserver installiert, z. B. einem Server mit Internetinformationsdienste (Internet Information Services, IIS). Die Clienttools enthalten die Clientkonnektivitätskomponenten, mit denen eine Anwendung eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]herstellt.

>[!NOTE]
>Die Installation einer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Instanz auf einem Computer mit IIS ist zwar möglich, in der Praxis aber nur bei kleinen Websites üblich, die über einen einzigen Servercomputer verfügen. Bei den meisten Websites befindet sich das IIS-System mittlerer Ebene auf einem Server oder Servercluster, während die Datenbanken auf einem separaten Server oder einem vereinten System gespeichert werden.

## <a name="deciding-among-ssnoversion-components"></a>Auswählen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Komponenten

Auf der Seite Funktionsauswahl des Installationsassistenten für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] können Sie die Komponenten auswählen, die mit [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]installiert werden sollen. Standardmäßig sind in der Struktur keine Funktionen ausgewählt.

Anhand der Informationen aus den folgenden Tabellen können Sie ermitteln, welche Gruppe von Funktionen für Ihre Anforderungen am besten geeignet ist.

|Serverkomponenten|BESCHREIBUNG|
|-----------------------|-----------------|
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] enthält die [!INCLUDE[ssDE](../includes/ssde-md.md)], den Kerndienst zum Speichern, Verarbeiten und Schützen von Daten sowie für Replikation, Volltextsuche, Verwaltungstools für relationale und XML-Daten, Integration in die Datenbankanalyse und PolyBase-Integration für den Zugriff auf Hadoop und andere heterogene Datenquellen sowie Machine Learning Services für die Ausführung von Python- und R-Skripts mit relationalen Daten.|
|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] enthält Tools zum Erstellen und Verwalten der analytischen Onlineverarbeitung (OLAP, Online Analytical Processing) sowie Data Mining-Anwendungen.|
|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] enthält Server- und Clientkomponenten, mit denen Berichte in Form einer Tabelle, Matrix, Grafik oder Freiform erstellt, verwaltet und bereitgestellt werden können. [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] ist gleichzeitig eine erweiterbare Plattform, die Sie zum Entwickeln von Berichtsanwendungen verwenden können.|
|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] stellt eine Gruppe grafischer Tools und programmierbarer Objekte zum Verschieben, Kopieren und Transformieren von Daten bereit. Es beinhaltet außerdem die [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS)-Komponente für [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].|
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] (MDS) ist die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Lösung für die Masterdatenverwaltung. MDS kann konfiguriert werden, um jede Domäne (Produkte, Kunden, Konten) zu verwalten und umfasst Hierarchien, präzise Sicherheit, Transaktionen, Datenversionsverwaltung und Geschäftsregeln sowie einen [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] , der verwendet werden kann, um Daten zu verwalten.|
|Machine Learning-Dienste (datenbankintern)|Machine Learning-Dienste (datenbankintern) unterstützt verteilte, skalierbare Machine Learning-Lösungen mithilfe von Unternehmensdatenquellen. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2016 wurde die R-Sprache unterstützt. [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] unterstützt R und Python.|
|Machine Learning-Server (eigenständig)|Machine Learning Server (eigenständig) unterstützt die Bereitstellung von verteilten, skalierbaren Machine Learning-Lösungen auf mehreren Plattformen und verwendet mehrere Unternehmensdatenquellen, einschließlich Linux und Hadoop. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2016 wurde die R-Sprache unterstützt. [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] unterstützt R und Python.|

|Verwaltungstools|Beschreibung|
|----------------------|-----------------|
|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] (SSMS) ist eine integrierte Umgebung, in der Sie auf [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Komponenten zugreifen sowie diese konfigurieren, verwalten und entwickeln können. SSMS ermöglicht Entwicklern und Administratoren mit unterschiedlichen Fähigkeiten die Verwendung von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Die neueste Edition von SSMS umfasst ein Update für SMO, das auch die [SQL-Bewertungs-API](../tools/sql-assessment-api/sql-assessment-api-overview.md) einschließt.<br /><br/> Herunterladen und Installieren <br />[!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]: [Herunterladen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Konfigurations-Manager|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Konfigurations-Manager stellt eine einfache Konfigurationsverwaltung für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Dienste, Server- und Clientprotokolle sowie Clientaliase bereit.|
|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] stellt eine grafische Benutzeroberfläche zum Überwachen einer Instanz von [!INCLUDE[ssDE](../includes/ssde-md.md)] oder [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]bereit.|
|[!INCLUDE[ssDE](../includes/ssde-md.md)] -Optimierungsratgeber|Der Optimierungsratgeber für[!INCLUDE[ssDE](../includes/ssde-md.md)] unterstützt Sie beim Erstellen einer optimalen Menge von Indizes, indizierten Sichten und Partitionen.|
|Data Quality Client|Stellt eine sehr einfache und intuitive grafische Benutzeroberfläche bereit, um eine Verbindung mit dem DQS-Server herzustellen und Datenbereinigungsvorgänge auszuführen. Diese Komponente ermöglicht außerdem die zentrale Überwachung verschiedener Aktivitäten, die während des Datenbereinigungsvorgangs ausgeführt werden.|
|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] bietet eine IDE zum Erstellen von Lösungen für die Business Intelligence-Komponenten: [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]und [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].<br /><br /> (Früher Business Intelligence Development Studio genannt).<br /><br /> [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] schließt auch "Datenbankprojekte" ein, die eine integrierte Umgebung für Datenbankentwickler bereitstellen, die ihre ganze Datenbankentwurfsarbeit für eine beliebige [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Plattform (sowohl vor Ort als auch remote) in Visual Studio ausführen möchten. Datenbankentwickler können den verbesserten Server-Explorer in Visual Studio verwenden, um Datenbankobjekte und Daten einfach zu erstellen oder zu bearbeiten und Abfragen auszuführen.|
|Konnektivitätskomponenten|Installiert Komponenten für die Kommunikation zwischen Clients und Servern, einschließlich Netzwerkbibliotheken für die DB-Library, ODBC und OLE DB.|

|Dokumentation|BESCHREIBUNG|
|-------------------|-----------------|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Onlinedokumentation|Kerndokumentation für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|

**Developer und Evaluation-Editionen:** Von den Editionen Developer und Evaluation unterstützte Features finden Sie in denen der Enterprise Edition von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in den folgenden Tabellen.

Die Developer Edition unterstützt weiterhin nur einen Client für [[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md).

## <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a> Skalierungsgrenzen

|Funktion|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|-------|--------:|------:|---:|-------------------------------:|-----:|
|Maximale von einer einzelnen Instanz verwendete Rechenkapazität ( [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Maximum des Betriebssystems|Beschränkt auf weniger als 4 Sockets oder 24 Kerne|Beschränkt auf weniger als 4 Sockets oder 16 Kerne|Beschränkt auf weniger als 1 Socket oder 4 Kerne|Beschränkt auf weniger als 1 Socket oder 4 Kerne|
|Maximale von einer einzelnen Instanz verwendete Rechenkapazität ( [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] oder [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)])|Maximum des Betriebssystems|Beschränkt auf weniger als 4 Sockets oder 24 Kerne|Beschränkt auf weniger als 4 Sockets oder 16 Kerne|Beschränkt auf weniger als 1 Socket oder 4 Kerne|Beschränkt auf weniger als 1 Socket oder 4 Kerne|
|Maximaler Arbeitsspeicher für den Pufferpool pro Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Maximum des Betriebssystems|128<nobr/>GB|64<nobr/>GB|1\.410<nobr/>MB|1\.410<nobr/>MB|
|Maximaler Arbeitsspeicher für Columnstore-Segmentcache pro Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Unbegrenzter Arbeitsspeicher| 32<nobr/>GB| 16<nobr/>GB| 352<nobr/>MB| 352<nobr/>MB|
|Maximale speicheroptimierte Datengröße pro Datenbank in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Unbegrenzter Arbeitsspeicher| 32<nobr/>GB| 16<nobr/>GB| 352<nobr/>MB| 352<nobr/>MB|
|Maximaler genutzter Arbeitsspeicher pro Instanz von [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|Maximum des Betriebssystems|16<nobr/>GB<sup>2</sup><br /><br /> 64<nobr/>GB<sup>3</sup>|N/V|N/V|N/V|
|Maximaler genutzter Arbeitsspeicher pro Instanz von [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Maximum des Betriebssystems|64<nobr/>GB|64<nobr/>GB|4<nobr/>GB|N/V|
|Maximale relationale Datenbankgröße|524<nobr/>PB|524<nobr/>PB|524<nobr/>PB|10<nobr/>GB|10<nobr/>GB|

<sup>1</sup> Enterprise Edition mit einer Lizenzierung auf Grundlage von Serverlizenz + Clientzugriffslizenz (CAL) (für neue Verträge nicht verfügbar) ist auf maximal 20 Kerne pro [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Instanz beschränkt. Für das auf Prozessorkernen basierende Serverlizenzierungsmodell gelten keine Beschränkungen. Weitere Informationen finden Sie unter [Rechenkapazitätsgrenzen von bestimmten Editionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).

<sup>2</sup> Tabellarisch

<sup>3</sup> MOLAP

## <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a> RDBMS: Hochverfügbarkeit

|Funktion|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|--------------------------------:|:-----:|
|Server Core-Unterstützung<sup>1</sup>|Ja|Ja|Ja|Ja|Ja|
|Protokollversand|Ja|Ja|Ja|Nein|Nein|
|Datenbankspiegelung|Ja|Ja<sup>2</sup>|Ja<sup>3</sup>|Ja<sup>3</sup>|Ja<sup>3</sup>|
|Sicherungskomprimierung|Ja|Ja|Nein|Nein|Nein |
|Datenbankmomentaufnahme|Ja|Ja|Ja|Ja|Ja|
|Always On-Failoverclusterinstanzen<sup>4</sup>|Ja|Ja|Nein|Nein|Nein|
|Always On-Verfügbarkeitsgruppen<sup>5</sup>|Ja|Nein|Nein|Nein|Nein|
|Basis-Verfügbarkeitsgruppen<sup>6</sup>|Nein|Ja|Nein|Nein|Nein|
|Automatisches Umleiten von Lese-/Schreibverbindungen |Ja|Nein|Nein|Nein|Nein |
|Onlineseiten- und Onlinedateiwiederherstellung|Ja|Nein|Nein|Nein|Nein|
|Onlineindex erstellen und neu erstellen|Ja|Nein|Nein|Nein|Nein |
|Fortsetzbare Neuerstellung von online geschalteten Indizes|Ja|Nein|Nein|Nein|Nein |
|Onlineschemaänderung|Ja|Nein|Nein|Nein|Nein |
|Schnelle Wiederherstellung|Ja|Nein|Nein|Nein|Nein|
|Verbesserte Wiederherstellung von Datenbanken|Ja|Ja|Ja|Nein|Nein |
|Gespiegelte Sicherungen|Ja|Nein|Nein|Nein|Nein |
|Hinzufügen von Speicher im laufenden Systembetrieb und CPU|Ja|Nein|Nein|Nein|Nein|
|Datenbankwiederherstellungsberater|Ja|Ja|Ja|Ja|Ja|
|Verschlüsselte Sicherung|Ja|Ja|Nein|Nein|Nein|
|Hybridsicherung in Windows Azure (Sicherung über URL)|Ja|Ja|Ja|Nein|Nein|
|Clusterlose Verfügbarkeitsgruppe<sup>5,6</sup>|Ja|Ja|Nein|Nein|Nein|
|Failoverserver für die Notfallwiederherstellung<sup>7</sup>|Ja|Ja|Nein|Nein|Nein|
|Failoverserver für Hochverfügbarkeit<sup>7</sup>|Ja|Ja|Nein|Nein|Nein|
|Failoverserver für die Notfallwiederherstellung in Azure<sup>7</sup>|Ja|Ja|Nein|Nein|Nein|

<sup>1</sup> Weitere Informationen zum Installieren von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unter Server Core finden Sie unter [Installieren von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unter Server Core](../database-engine/install-windows/install-sql-server-on-server-core.md).

<sup>2</sup> Nur vollständige Sicherheit

<sup>3</sup> Nur Zeuge

<sup>4</sup> Bei der Enterprise Edition entspricht die Anzahl der Knoten dem Maximum des Betriebssystems. Bei der Standard Edition werden nur zwei Knoten unterstützt.

<sup>5</sup> Bei der Enterprise Edition werden bis zu acht sekundäre Replikate unterstützt, einschließlich fünf synchronen sekundären Replikaten.

<sup>6</sup> Bei der Standard Edition werden Basis-Verfügbarkeitsgruppen unterstützt. Eine Basis-Verfügbarkeitsgruppe unterstützt zwei Replikate mit einer Datenbank. Weitere Informationen über Basis-Verfügbarkeitsgruppen finden Sie unter [Basis-Verfügbarkeitsgruppen](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).

<sup>7</sup> [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) erforderlich.

## <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a> RDBMS: Skalierbarkeit und Leistung

|Funktion|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|------:|:--------:|:------:|:-:|:-------------------------------:|:------:|
|Columnstore<sup>1</sup> <sup>2</sup>|Ja|Ja|Ja|Ja|Ja|
|Große Objektbinärdateien in gruppierten Columnstore-Indizes|Ja|Ja|Ja|Ja|Ja|
|Onlineneuerstellung für nicht gruppierten Columnstore-Index|Ja|Nein|Nein|Nein|Nein|
|In-Memory Database: In-Memory-OLTP<sup>1</sup>|Ja|Ja|Ja|Ja<sup>3</sup>|Ja|
|In-Memory Database: hybrider Pufferpool|Ja|Ja|Nein|Nein|Nein|
|In-Memory Database: speicheroptimierte tempdb-Metadaten|Ja|Nein|Nein|Nein|Nein|
|In-Memory Database: Unterstützung von persistentem Speicher|Ja|Ja|Ja|Ja|Ja|
|Stretch Database|Ja|Ja|Ja|Ja|Ja|
|Unterstützung mehrerer Instanzen|50|50|50|50|50|
|Tabellen- und Indexpartitionierung|Ja|Ja|Ja|Ja|Ja|
|Datenkomprimierung|Ja|Ja|Ja|Ja|Ja|
|Resource Governor|Ja|Nein|Nein|Nein|Nein|
|Parallelverarbeitung für partitionierte Tabellen|Ja|Nein|Nein|Nein|Nein|
|Mehrere Filestreamcontainer|Ja|Ja|Ja|Ja|Ja|
|NUMA-basierter und großer Arbeitsspeicher für umfangreiche Seiten und Zuordnung von Pufferarrays|Ja|Nein|Nein|Nein|Nein|
|Pufferpoolerweiterung|Ja|Ja|Nein|Nein|Nein|
|Ressourcengovernance für E/A-Vorgänge|Ja|Nein|Nein|Nein|Nein|
|Vorauslesen (Read-Ahead)|Ja|Nein|Nein|Nein|Nein|
|Erweitertes Scannen|Ja|Nein|Nein|Nein|Nein|
|Verzögerte Dauerhaftigkeit|Ja|Ja|Ja|Ja|Ja|
|Intelligente Datenbank: automatische Optimierung|Ja|Nein|Nein|Nein|Nein|
|Intelligente Datenbank: Batchmodus für Rowstore<sup>1</sup>|Ja|Nein|Nein|Nein|Nein|
|Intelligente Datenbank: Feedback zur Speicherzuweisung im Zeilenmodus|Ja|Nein|Nein|Nein|Nein|
|Intelligente Datenbank: geschätzte eindeutige Anzahl|Ja|Ja|Ja|Ja|Ja|
|Intelligente Datenbank: verzögerte Kompilierung von Tabellenvariablen|Ja|Ja|Ja|Ja|Ja|
|Intelligente Datenbank: Inlining benutzerdefinierter Skalarfunktionen|Ja|Ja|Ja|Ja|Ja|
|Adaptive Joins im Batchmodus|Ja|Nein|Nein|Nein|Nein|
|Feedback zur Speicherzuweisung im Batchmodus|Ja|Nein|Nein|Nein|Nein|
|Verschachtelte Ausführung mit Tabellenwertfunktionen mit mehreren Anweisungen|Ja|Ja|Ja|Ja|Ja|
|Verbesserungen beim massenhaften Einfügen|Ja|Ja|Ja|Ja|Ja|

<sup>1</sup> Die Größe der In-Memory-OLTP-Daten und des Columnstore-Segmentcaches sind auf die Größe des Arbeitsspeichers beschränkt, die von der Edition im Bereich [Skalierungslimits](#Cross-BoxScaleLimits) festgelegt wird. Der Grad an Parallelität (DOP) für Vorgänge im [Batchmodus](../relational-databases/query-processing-architecture-guide.md#batch-mode-execution) sind für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Standard Edition auf „2“ und für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Web und Express Edition auf „1“ beschränkt. Dies gilt für Columnstore-Indizes, die über datenträgerbasierte Tabellen und speicheroptimierte Tabellen erstellt wurden.

<sup>2</sup> Aggregatweitergabe, Zeichenfolgenprädikatweitergabe und SIMD-Optimierungen sind Erweiterungen der Skalierbarkeit von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition. Weitere Einzelheiten finden Sie unter [Columnstore-Indizes – Neuigkeiten](../relational-databases/indexes/columnstore-indexes-what-s-new.md).

<sup>3</sup> Dieses Feature ist in der LocalDB-Installationsoption nicht enthalten.

## <a name="rdbms-security"></a><a name="RDBMSS"></a> RDBMS: Sicherheit

|Funktion|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|:-----:|:-----------------------:|:-----:|
|Sicherheit auf Zeilenebene|Ja|Ja|Ja|Ja|Ja|
|Always Encrypted|Ja|Ja|Ja|Ja|Ja|
|Always Encrypted mit Secure Enclaves|Ja|Ja|Ja|Ja|Ja|
|Dynamische Datenmaskierung|Ja|Ja|Ja|Ja|Ja|
|Serverüberwachung|Ja|Ja|Ja|Ja|Ja|
|Datenbanküberwachung|Ja|Ja|Ja|Ja|Ja|
|Transparente Datenbankverschlüsselung|Ja|Ja|Ja|Nein|Nein|
|Erweiterbare Schlüsselverwaltung|Ja|Ja|Nein|Nein|Nein |
|Benutzerdefinierte Rollen|Ja|Ja|Ja|Ja|Ja|
|Eigenständige Datenbanken|Ja|Ja|Ja|Ja|Ja|
|Verschlüsselung von Sicherungen|Ja|Ja|Nein|Nein|Nein|
|Datenklassifizierung und -überwachung|Ja|Ja|Ja|Ja|Ja|

## <a name="replication"></a><a name="Replication"></a> Replikation

|Funktion|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|-------|:---------:|:-------:|:--:|:--------------------------------:|:------:|
|Heterogene Abonnenten|Ja|Ja|Nein|Nein|Nein|
|Mergereplikation|Ja|Ja|Ja<sup>1</sup>|Ja<sup>1</sup>|Ja<sup>1</sup>|
|Veröffentlichungen mit Oracle|Ja|Nein|Nein|Nein|Nein|
|Peer-zu-Peer-Transaktionsreplikation|Ja|Nein|Nein|Nein|Nein|
|Momentaufnahmereplikation|Ja|Ja|Ja<sup>1</sup>|Ja<sup>1</sup>|Ja<sup>1</sup>|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Änderungsnachverfolgung|Ja|Ja|Ja|Ja|Ja|
|Transaktionsreplikation|Ja|Ja|Ja<sup>1</sup>|Ja<sup>1</sup>|Ja<sup>1</sup>|
|Transaktionsreplikation zu Azure|Ja|Ja|Nein|Nein|Nein|
|Aktualisierbares Abonnement für Transaktionsreplikation|Ja|Ja|Nein|Nein|Nein|

<sup>1</sup> Nur Abonnent

## <a name="management-tools"></a><a name="SSMS"></a> Verwaltungstools

|Funktion|Enterprise|Standard|Web|Express mit Advanced Services|Express|
|-------|:--------:|:------:|:-:|:----------------------------:|:-----:|
|SQL Management Objects (SMO)|Ja|Ja|Ja|Ja|Ja|
|SQL-Bewertungs-API|Ja|Ja|Ja|Ja|Ja|
|SQL-Sicherheitsrisikobewertung |Ja|Ja|Ja|Ja|Ja|
|SQL-Konfigurations-Manager|Ja|Ja|Ja|Ja|Ja|
|SQL CMD (Command Prompt Tool – Eingabeaufforderungstool)|Ja|Ja|Ja|Ja|Ja|
|Distributed Replay: Administratortool|Ja|Ja|Ja|Ja|Nein|
|Distributed Replay – Client|Ja|Ja|Ja|Nein|Nein|
|Distributed Replay - Controller|Ja<sup>1</sup>|Ja<sup>2</sup>|Ja<sup>2</sup>|Nein|Nein|
|SQL Profiler|Ja|Ja|Nein<sup>3</sup>|Nein<sup>3</sup>|Nein<sup>3</sup>|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Agent|Ja|Ja|Ja|Nein|Nein|
|Microsoft System Center Operations Manager Management Pack|Ja|Ja|Ja|Nein|Nein|
|Datenbankoptimierungsratgeber (DTA)|Ja|Ja<sup>4</sup>|Ja<sup>4</sup>|Nein|Nein|

<sup>1</sup> Bis zu 16 Clients

<sup>2</sup> 1 Client

<sup>3</sup> Für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Web, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express with Tools und [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express with Advanced Services kann eine Profilerstellung mithilfe der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Standard Edition und [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition ausgeführt werden.

<sup>4</sup> Optimierung ist nur in Features der Standard Edition aktiviert.

## <a name="rdbms-manageability"></a><a name="RDBMSM"></a> RDBMS: Verwaltbarkeit

|Funktion|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Benutzerinstanzen|Nein|Nein|Nein|Ja|Ja|
|LocalDB|Nein|Nein|Nein|Ja|Nein|
|Dedizierte Administratorverbindung|Ja|Ja|Ja|Ja<sup>1</sup>|Ja<sup>1</sup>|
|SysPrep-Unterstützung<sup>2</sup>|Ja|Ja|Ja|Ja|Ja|
|PowerShell-Skriptunterstützung<sup>3</sup>|Ja|Ja|Ja|Ja|Ja|
|Unterstützung für Komponentenvorgänge der Datenschichtanwendung: Extrahieren, Bereitstellen, Aktualisieren, Löschen|Ja|Ja|Ja|Ja|Ja|
|Richtlinienautomatisierung (Überprüfung nach Zeitplan und Änderungen)|Ja|Ja|Ja|Nein|Nein |
|Sammler von Leistungsdaten|Ja|Ja|Ja|Nein|Nein|
|Fähigkeit zur Registrierung als verwaltete Instanz in einer Mehrfachinstanzverwaltung|Ja|Ja|Ja|Nein|Nein |
|Standardleistungsberichte|Ja|Ja|Ja|Nein|Nein |
|Planhinweislisten und Planeinfrierung für Planhinweislisten|Ja|Ja|Ja|Nein|Nein |
|Direkte Abfrage von indizierten Sichten (mittels NOEXPAND-Hinweis)|Ja|Ja|Ja|Ja|Ja|
|Direktes Abfragen von SQL Server Analysis Services|Ja|Ja|Nein|Nein|Ja|
|Automatische Wartung für indizierte Sichten|Ja|Ja|Ja|Nein|Nein |
|Verteilte partitionierte Sichten|Ja|Nein|Nein|Nein|Nein |
|Parallele Indexvorgänge|Ja|Nein|Nein|Nein|Nein |
|Automatische Verwendung indizierter Sichten mittels Abfrageoptimierer|Ja|Nein|Nein|Nein|Nein |
|Parallele Konsistenzprüfung|Ja|Nein|Nein|Nein|Nein|
|Steuerungspunkt für das [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Hilfsprogramm|Ja|Nein|Nein|Nein|Nein|
|Pufferpoolerweiterung|Ja|Ja|Nein|Nein|Nein|
|Masterinstanz für Big Data-Cluster|Ja|Ja|Nein|Nein|Nein|
|Kompatibilitätszertifizierung|Ja|Ja|Ja|Ja|Ja|

<sup>1</sup> Mit Ablaufverfolgungsflag

<sup>2</sup> Weitere Informationen finden Sie unter [Überlegungen zur Installation von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mit SysPrep](../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md).

<sup>3</sup> Unter Linux werden PowerShell-Skripts von Windows-Computern unterstützt, die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unter Linux als Ziel verwenden.

## <a name="development-tools"></a><a name="DevTools"></a> Entwicklungstools

|Funktion|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Integration in Microsoft Visual Studio|Ja|Ja|Ja|Ja|Ja|
|IntelliSense (Transact-SQL und MDX)|Ja|Ja|Ja|Ja|Ja|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Data Tools (SSDT)|Ja|Ja|Ja|Ja|Nein|
|MDX-Bearbeitungs-, MDX-Debug- und MDX-Entwurfstools|Ja|Ja|Nein|Nein|Nein |

## <a name="programmability"></a><a name="Programmability"></a> Programmability

|Funktion|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Grundlegende Integration von R<sup>1</sup>|Ja|Ja|Ja|Ja|Nein|
|Erweiterte Integration von R<sup>2</sup>|Ja|Nein|Nein|Nein|Nein|
|Grundlegende Integration von Python|Ja|Ja|Ja|Ja|Nein|
|Erweiterte Integration von Python|Ja|Nein|Nein|Nein|Nein|
|Machine Learning-Server (eigenständig)|Ja|Nein|Nein|Nein|Nein|
|PolyBase-Computeknoten|Ja|Ja<sup>3</sup>|Ja<sup>3</sup>|Ja<sup>3</sup>|Ja<sup>3</sup> |
|PolyBase-Hauptknoten<sup>4</sup>|Ja|Ja|Nein|Nein|Nein|
|JSON|Ja|Ja|Ja|Ja|Ja|
|Abfragespeicher|Ja|Ja|Ja|Ja|Ja|
|Temporal|Ja|Ja|Ja|Ja|Ja|
|Common Language Runtime (CLR)-Integration|Ja|Ja|Ja|Ja|Ja|
|Java Language Runtime-Integration|Ja|Ja|Ja|Ja|Ja|
|Systemeigene XML-Unterstützung|Ja|Ja|Ja|Ja|Ja|
|XML-Indizierung|Ja|Ja|Ja|Ja|Ja|
|MERGE- und UPSERT-Funktionen|Ja|Ja|Ja|Ja|Ja|
|FILESTREAM-Unterstützung|Ja|Ja|Ja|Ja|Ja|
|FileTable|Ja|Ja|Ja|Ja|Ja|
|Datums- und Uhrzeitdatentypen|Ja|Ja|Ja|Ja|Ja|
|Internationalisierungsunterstützung|Ja|Ja|Ja|Ja|Ja|
|Volltextsuche und semantische Suche|Ja|Ja|Ja|Ja|Nein|
|Angabe der Sprache in einer Abfrage|Ja|Ja|Ja|Ja|Nein|
|Service Broker (Messaging)|Ja|Ja|Nein<sup>5</sup>|Nein<sup>5</sup>|Nein<sup>5</sup>|
|Transact-SQL-Endpunkte|Ja|Ja|Ja|Nein|Nein |
|Graph|Ja|Ja|Ja|Ja|Ja|
|Unterstützung für UTF-8|Ja|Ja|Ja|Ja|Ja|

<sup>1</sup> Die grundlegende Integration ist auf 2 Kerne und In-Memory-Datasets beschränkt.

<sup>2</sup> Die erweiterte Integration kann alle verfügbaren Kerne für die Parallelverarbeitung von Datasets beliebiger Größe und unter Berücksichtigung von Hardwarebeschränkungen nutzen.

<sup>3</sup> Horizontale Hochskalierung mit mehreren Computeknoten erfordert einen Hauptknoten.

<sup>4</sup> Vor SQL Server 2019 war für den PolyBase-Hauptknoten eine Enterprise Edition erforderlich.

<sup>5</sup> Nur Client

## <a name="integration-services"></a><a name="IS"></a> Integration Services

Informationen zu den Features von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Integration Services (SSIS), die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Von den Editionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützte Integration Services-Features](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

## <a name="master-data-services"></a><a name="MDS"></a> Master Data Services

Informationen zu den [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]- und Data Quality Services-Features, die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Von den Editionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützte Master Data Services- und Data Quality Services-Features](../master-data-services/master-data-services-and-data-quality-services-features-support.md).

## <a name="data-warehouse"></a><a name="DW"></a> Data Warehouse

|Funktion|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Automatisches Generieren von Staging- und Data Warehouse-Schemas|Ja|Ja|Nein|Nein|Nein|
|Erfassung geänderter Daten|Ja|Ja|Nein|Nein|Nein|
|Optimierung von Sternjoin-Abfragen|Ja|Nein|Nein|Nein|Nein|
|Parallele Abfrageverarbeitung bei partitionierten Tabellen und Indizes|Ja|Nein|Nein|Nein|Nein|
|Globale Batchaggregation|Ja|Nein|Nein|Nein|Nein|

## <a name="analysis-services"></a><a name="SSAS"></a> Analysis Services

Weitere Informationen zu Analysis Services-Features, die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Von den Editionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützte Analysis Services-Features](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md).

## <a name="reporting-services"></a><a name="SSRS"></a> Reporting Services

Weitere Informationen zu Reporting Services-Features, die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Von den Editionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützte Reporting Services-Features](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

## <a name="business-intelligence-clients"></a><a name="BIC"></a> Business Intelligence-Clients

Weitere Informationen zu Business Intelligence-Clientfeatures, die von den einzelnen Editionen von [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Von den Editionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützte Analysis Services-Features](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md) oder [Von den Editionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützte Reporting Services-Features](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

## <a name="spatial-and-location-services"></a><a name="SLS"></a> Räumliche und ortsbezogene Dienste

|Funktionsname|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|-------------|:-------:|:------:|:-:|:-------------------------------:|:-----:|
|Räumlichkeitsindizes|Ja|Ja|Ja|Ja|Ja|
|Planarer und geodätischer Datentyp|Ja|Ja|Ja|Ja|Ja|
|Erweiterte räumliche Bibliotheken|Ja|Ja|Ja|Ja|Ja|
|Importieren/Exportieren räumlicher Industriestandard-Datenformate|Ja|Ja|Ja|Ja|Ja|

## <a name="additional-database-services"></a><a name="ADS"></a> Weitere Datenbankdienste

|Funktionsname|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|------------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Migration Assistant|Ja|Ja|Ja|Ja|Ja|
|Datenbank-E-Mail|Ja|Ja|Ja|Nein|Nein|

## <a name="other-components"></a><a name="Other"></a> Andere Komponenten

|Funktionsname|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|------------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|StreamInsight|StreamInsight Premium-Edition|StreamInsight Standard-Edition|StreamInsight Standard-Edition|Nein|Nein|
|StreamInsight HA|StreamInsight Premium-Edition|Nein|Nein|Nein|Nein|

## <a name="next-steps"></a>Nächste Schritte

[Produktspezifikationen für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](https://msdn.microsoft.com/library/6445fd53-6844-4170-a86b-7fe76a9f64cb)

[Installation für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../database-engine/install-windows/installation-for-sql-server-2016.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
