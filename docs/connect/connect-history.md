---
title: Treiberhistorie für Microsoft SQL Server | Microsoft-Dokumentation
description: Auf dieser Seite werden die historischen Datenverbindungstechnologien für die Verbindungsherstellung mit SQL Server beschrieben.
ms.custom: ''
ms.date: 12/08/2020
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dee1514230f3e0ce0f0ba4c0d3af904cc90c9720
ms.sourcegitcommit: d983ad60779d90bb1c89a34d7b3d6da18447fdd8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/09/2020
ms.locfileid: "96933809"
---
# <a name="driver-history-for-microsoft-sql-server"></a>Treiberhistorie für Microsoft SQL Server

Auf dieser Seite werden die historischen Datenverbindungstechnologien für die Verbindungsherstellung mit SQL Server beschrieben.

## <a name="odbc"></a>ODBC

Es gibt drei Generationen von Microsoft ODBC-Treibern für SQL Server. Der erste SQL Server-ODBC-Treiber wird weiterhin als Bestandteil der [Windows Data Access Components](#microsoft-or-windows-data-access-components) ausgeliefert. Dieser Treiber wird nicht für neue Entwicklungsprojekte empfohlen. Ab SQL Server 2005 umfasst [SQL Server Native Client](#sql-server-native-client) eine ODBC-Schnittstelle und ist der ODBC-Treiber, der mit SQL Server 2005 bis SQL Server 2012 ausgeliefert wurde. Dieser Treiber wird ebenfalls nicht für neue Entwicklungsprojekte empfohlen. Nach SQL Server 2012 ist [Microsoft ODBC Driver for SQL Server](#microsoft-odbc-driver-for-sql-server) der Treiber, der ab diesem Zeitpunkt mit den neuesten Server-Features aktualisiert wird.

### <a name="sql-server-native-client"></a>SQL Server Native Client

SQL Server Native Client ist eine eigenständige Bibliothek, die sowohl für OLE DB als auch für ODBC verwendet wird. SQL Server Native Client (kurz SNAC) war in SQL Server 2005 bis SQL Server 2012 enthalten. SQL Server Native Client kann für Anwendungen verwendet werden, die die Vorteile der neuen Features von SQL Server 2005 bis SQL Server 2012 nutzen müssen. (Die Microsoft/Windows Data Access Components werden nicht im Hinblick auf diese neuen Features in SQL Server aktualisiert.) Der SQL Server Native Client wird im Hinblick auf neue Features nach SQL Server 2012 nicht mehr aktualisiert. Wechseln Sie zum Microsoft ODBC Driver for SQL Server oder zum Microsoft OLE DB-Treiber für SQL Server, wenn Sie in Zukunft die Vorteile der neuen SQL Server-Features nutzen möchten.

Umfassende Informationen zu SQL Server Native Client finden Sie in der [SQL Server Native Client-Dokumentation](../relational-databases/native-client/sql-server-native-client-programming.md).

### <a name="microsoft-odbc-driver-for-sql-server"></a>Microsoft ODBC Driver for SQL Server

Nach SQL Server 2012 wurde der primäre ODBC-Treiber für SQL Server entwickelt und als „Microsoft ODBC Driver for SQL Server“ veröffentlicht. Weitere Informationen finden Sie in der [Microsoft ODBC Driver for SQL Server-Dokumentation](./odbc/microsoft-odbc-driver-for-sql-server.md).

## <a name="ole-db"></a>OLE DB

Es gibt drei verschiedene Generationen von Microsoft OLE DB-Anbietern für SQL Server. Die erste „Microsoft OLE DB-Anbieter für SQL Server“ (SQLOLEDB) ist weiterhin als Teil von [Windows Data Access Components](#microsoft-or-windows-data-access-components) erhältlich. Dieser Anbieter wird nicht mit neuen Features aktualisiert, und es wird nicht empfohlen, diesen Treiber für neue Entwicklungsprojekte zu verwenden. Ab SQL Server 2005 umfasst [SQL Server Native Client](#sql-server-native-client) eine OLE DB-Anbieterschnittstelle (SQLNCLI) und ist der im Lieferumfang von SQL Server 2005 bis SQL Server 2017 bereitgestellte OLE DB-Anbieter. [Dieser Treiber ist seit 2011 offiziell veraltet](/archive/blogs/sqlnativeclient/microsoft-is-aligning-with-odbc-for-native-relational-data-access), und es wird nicht empfohlen, ihn für neue Entwicklungsprojekte zu verwenden. Im Jahr 2017 wurde die [Einstellung der OLE DB-Datenzugriffstechnologie zurückgenommen und ein neues geplantes Release](/archive/blogs/sqlnativeclient/announcing-the-new-release-of-ole-db-driver-for-sql-server) für 2018 angekündigt. Der neue OLE DB-Anbieter wird als „Microsoft OLE DB Driver for SQL Server“ (MSOLEDBSQL) bezeichnet und ist der aktuell gewartete und unterstützte Anbieter.

## <a name="adonet"></a>ADO.NET

Bei ADO.NET handelt es sich um eine Reihe von Klassen, die eine Schnittstelle für den Zugriff auf jede beliebige Art von Datenquelle definieren, sowohl relationale als auch nicht relationale. ADO.NET wurde mit dem Microsoft .NET Framework eingeführt und wird seither in .NET verbessert und gewartet. Die SqlClient-Bibliothek ist ein ADO.NET-Datenanbieter, der Konnektivität mit SQL Server- und Azure SQL-Datenquellen bietet.

### <a name="systemdatasqlclient"></a>System.Data.SqlClient

System.Data.SqlClient ist im .NET Framework und in .NET Core enthalten. Bis 2019 wurden regelmäßige Featureupdates dafür bereitgestellt. Seit den Ankündigungen zur [Zukunft von .NET Core, des .NET Framework](https://devblogs.microsoft.com/dotnet/net-core-is-the-future-of-net/) und [von .NET allgemein](https://devblogs.microsoft.com/dotnet/introducing-net-5/) musste SqlClient in einem Paket außerhalb von .NET weiterentwickelt werden. System.Data.SqlClient wird weiterhin unterstützt, erhält jedoch keine Featureupdates mehr und wird für neue Entwicklungsprojekte nicht empfohlen.

### <a name="microsoftdatasqlclient"></a>Microsoft.Data.SqlClient

Der [2019 eingeführte](https://devblogs.microsoft.com/dotnet/introducing-the-new-microsoftdatasqlclient/) Microsoft SqlClient-Datenanbieter für SQL Server ist ein ADO.NET-Datenanbieter, der NET Framework-, .NET Core- und .NET Standard-Anwendungen unterstützt. Weitere Informationen zum Microsoft.Data.SqlClient-Namespace finden Sie unter [Microsoft ADO.NET für SQL Server](ado-net/microsoft-ado-net-sql-server.md).

## <a name="jdbc"></a>JDBC

### <a name="microsoft-jdbc-driver-for-sql-server"></a>Microsoft JDBC-Treiber für SQL Server

Der im Jahr 2000 eingeführte Microsoft JDBC-Treiber für SQL Server wird fortlaufend verbessert und gewartet. Im Jahre 2016 wurde er als Open Source-Projekt bereitgestellt. Neueste Informationen finden Sie unter [Übersicht über den JDBC-Treiber](./jdbc/overview-of-the-jdbc-driver.md). Dort können Sie den Treiber auch herunterladen.

## <a name="php"></a>PHP

### <a name="microsoft-drivers-for-php-for-sql-server"></a>Microsoft-Treiber für PHP für SQL Server

Die Microsoft-Treiber für PHP für SQL Server, die 2009 als Open Source-Projekt eingeführt wurden, werden weiterhin verbessert und gewartet. Die neuesten Informationen finden Sie unter [Microsoft-Treiber für PHP für SQL Server](./php/microsoft-php-driver-for-sql-server.md). Dort können Sie die Treiber auch herunterladen.

## <a name="nodejs"></a>Node.js

### <a name="microsoft-driver-for-nodejs-for-sql-server"></a>Microsoft-Treiber für Node.js für SQL Server

Der Microsoft-Treiber für Node.js für SQL Server ermöglicht Node.js-Anwendungen unter Microsoft Windows und Microsoft Azure den Zugriff auf Microsoft SQL Server und Microsoft Azure SQL-Datenbank. Die Entwicklungsbemühungen konzentrieren sich nicht mehr auf diesen Treiber. Es wird nicht empfohlen, neue Anwendungen mit dem Microsoft-Treiber für Node.js für SQL Server zu entwickeln.

Weitere Informationen zum Microsoft-Treiber für Node.js für SQL Server finden Sie unter [WindowsAzure / node-sqlserver](https://github.com/Azure/node-sqlserver).

### <a name="tedious"></a>Tedious

Microsoft beteiligt sich am Open-Source-Tedious-Modul in Node.js für SQL Server-Konnektivität über JavaScript und unterstützt das Projekt. Weitere Informationen finden Sie unter [Node.js-Treiber für SQL Server](./node-js/node-js-driver-for-sql-server.md).

## <a name="microsoft-or-windows-data-access-components"></a>Microsoft/Windows Data Access Components

Die Microsoft/Windows Data Access Components (MDAC/WDAC) werden aus Gründen der Abwärtskompatibilität von Anwendungen mit Windows ausgeliefert und von Windows unterstützt. Sie sind nicht Teil des aktuellen SQL Server-Technologiestapels. Den Komponenten in den MDAC/WDAC werden keine neuen Features hinzugefügt, und es wird nicht empfohlen, sie für die Entwicklung neuer Anwendungen zu verwenden.

Im Rahmen dieses Dokuments kann der MDAC/WDAC-Stapel – basierend auf Technologie und Produkten – in die folgenden Komponenten unterteilt werden:

* **ADO** (einschließlich ADOMD und ADOX)
* **OLE DB** (einschließlich OLE DB Core Services, SQL Server OLE DB-Anbieter, Oracle OLE DB-Anbieter, OLE DB-Anbieter für ODBC-Treiber, Data Shape Provider und Remote Data Provider)
* **ODBC** (einschließlich ODBC-Treiber-Manager, SQL ODBC-Treiber und Oracle ODBC-Treiber)

### <a name="mdacwdac-components"></a>MDAC/WDAC-Komponenten

MDAC/WDAC umfasst die folgenden Komponenten:

* **ODBC:** Die Microsoft ODBC-Schnittstelle (Open Database Connectivity) ist eine Schnittstelle für die Programmiersprache C, mit deren Hilfe Anwendungen auf Daten aus einer Vielzahl verschiedener Datenbank-Managementsysteme (DBMS) zugreifen können. Anwendungen, die diese API verwenden, sind auf den Zugriff auf relationale Datenquellen beschränkt.
* **OLE DB:** Bei OLE DB handelt es sich um eine Reihe von COM-Schnittstellen für den Zugriff auf Daten in verschiedenen Arten von Datenspeichern. Es gibt OLE DB-Anbieter für den Zugriff auf Daten in Datenbanken, Dateisystemen, Nachrichtenspeichern, Verzeichnisdiensten, Workflow- und Dokumentspeichern.
* **ADO:** ActiveX Data Objects (ADO) stellt ein allgemeines Programmiermodell bereit. Wenngleich etwas weniger leistungsfähig als die direkte Codierung in OLE DB oder ODBC, ist ADO einfach zu erlernen und zu verwenden. ADO kann von Skriptsprachen wie z. B. Microsoft Visual Basic Scripting Edition (VBScript) oder Microsoft JScript verwendet werden.
* **ADOMD:** ADO Multi-Dimensional (ADOMD) ist zur Verwendung mit mehrdimensionalen Datenanbietern wie dem Microsoft OLAP-Anbieter vorgesehen (auch bekannt als Microsoft Analysis Services-Anbieter). Seit MDAC 2.0 wurden keine größeren Featureverbesserungen vorgenommen.
* **ADOX:** ADOX (ADO Extensions for DDL and Security) ermöglicht die Erstellung und Änderung der Definitionen einer Datenbank, Tabelle, eines Index oder einer gespeicherten Prozedur. Sie können ADOX mit einem beliebigen Anbieter verwenden. Der Microsoft Jet OLE DB-Anbieter unterstützt ADOX vollständig, während der Microsoft SQL Server OLE DB-Anbieter nur begrenzte Unterstützung bietet.
* **Microsoft SQL Server-Netzwerkbibliotheken:** Die SQL Server-Netzwerkbibliotheken ermöglichen SQLOLEDB und SQLODBC die Kommunikation mit der SQL Server-Datenbank. Die folgenden SQL Server-Netzwerkbibliotheken wurden in MDAC/WDAC-Versionen eingestellt: Banyan Vines, AppleTalk, ServerNet, IPX/SPX, Giganet und RPC. TCP/IP und Named Pipes werden weiterhin unterstützt und sind für die 64-Bit-Version des Windows-Betriebssystems verfügbar.
* **MSDASQL:** Mit dem Microsoft OLE DB-Anbieter für ODBC (MSDASQL) können auf OLE DB und ADO (das intern OLEDB verwendet) basierende Anwendungen über einen ODBC-Treiber auf Datenquellen zugreifen. MSDASQL ist ein OLEDB-Anbieter, der anstelle einer Datenbankverbindung eine Verbindung mit ODBC herstellt. Dies ist als Brücke von OLE DB zu einem ODBC-Treiber gedacht, wenn kein direkter OLE DB-Anbieter für eine Datenquelle vorhanden ist. MSDASQL wird mit dem Windows-Betriebssystem ausgeliefert, und Windows Server 2008 und Vista SP1 waren die ersten Windows-Versionen, die eine 64-Bit-Version der Technologie umfassten.

### <a name="deprecated-mdacwdac-components"></a>Eingestellte MDAC/WDAC-Komponenten

Diese Komponenten werden in der aktuellen Version von MDAC/WDAC noch unterstützt, aber sind in zukünftigen Versionen möglicherweise nicht mehr enthalten. Microsoft empfiehlt, bei der Entwicklung neuer Anwendungen die Verwendung dieser Komponenten zu vermeiden. Wenn Sie bestehende Anwendungen aktualisieren oder modifizieren, entfernen Sie jede Abhängigkeit von diesen Komponenten.

* **SQLOLEDB:** Der Microsoft OLE DB-Anbieter für SQL Server (SQLOLEDB), der den Zugriff auf Microsoft SQL Server unterstützt, wurde als veraltet eingestuft. In zukünftigen Versionen von SQL Server wird er möglicherweise nicht mehr unterstützt. Die Möglichkeit zur Verbindung mit Versionen vor SQL Server 7 wird nach Windows 7 aus dem Betriebssystem entfernt. Neue Anwendungen sollten den Microsoft OLE DB Driver for SQL Server (MSOLEDBSQL) verwenden, der neue SQL Server-Features unterstützt. Vorhandene Anwendungen sollten ebenfalls zum Microsoft OLE DB Driver for SQL Server migriert werden, um eine bessere Leistung, Zuverlässigkeit und Unterstützung zu erzielen. Weitere Informationen finden Sie unter [Aktualisieren einer Anwendung auf den OLE DB-Treiber für SQL Server über MDAC](oledb/applications/updating-an-application-to-oledb-driver-for-sql-server-from-mdac.md).
* **SQLODBC:** Der Microsoft SQL Server-ODBC-Treiber (SQLODBC), der den Zugriff auf Microsoft SQL Server unterstützt, wurde als veraltet eingestuft. In zukünftigen Versionen von SQL Server wird er möglicherweise nicht mehr unterstützt. Die Möglichkeit zur Verbindung mit Versionen vor SQL Server 7 wird nach Windows 7 aus dem Betriebssystem entfernt. Neue Anwendungen sollten unter Windows den Microsoft ODBC Driver for SQL Server verwenden, der neue SQL Server-Features unterstützt. Vorhandene Anwendungen sollten ebenfalls zum Microsoft ODBC Driver for SQL Server migriert werden, um eine bessere Leistung, Zuverlässigkeit und Unterstützung zu erzielen. Relevante Informationen finden Sie unter [Aktualisieren einer Anwendung von MDAC auf SQL Server Native Client](../relational-databases/native-client/applications/updating-an-application-to-sql-server-native-client-from-mdac.md).
* **Microsoft Jet-Datenbankengine 4.0:** Ab Version 2.6 umfasst MDAC keine Jet-Komponenten mehr. Anders ausgedrückt: MDAC 2.6, 2.7 und 2.8 enthalten nicht Microsoft Jet und auch nicht den Microsoft Jet OLE DB-Anbieter, die ODBC-Desktop-Datenbanktreiber und Jet-Datenzugriffsobjekte (Data Access Objects, DAO). 

  Es gibt keine 64-Bit-Version der Jet-Datenbank-Engine, des Jet OLE DB-Treibers, der Jet ODBC-Treiber oder der Jet-DAO. Weitere Informationen finden Sie im Knowledge Base-Artikel [957570](https://support.microsoft.com/kb/957570). Unter 64-Bit-Versionen von Windows wird eine 32-Bit-Version von Jet unter dem Windows WOW64-Subsystem ausgeführt. Weitere Informationen zu WOW64 finden Sie in der [MSDN WOW64-Dokumentation](/windows/desktop/WinProg64/wow64-implementation-details). Native 64-Bit-Anwendungen können nicht mit den 32-Bit-Jet-Treibern kommunizieren, die in WOW64 ausgeführt werden.

  Anstelle von Microsoft Jet empfiehlt Microsoft die Verwendung von [Microsoft SQL Server Express Edition](https://www.microsoft.com/sql-server/sql-server-editions-express) bei der Entwicklung neuer Nicht-Access-Anwendungen, die einen relationalen Datenspeicher benötigen. Diese neuen oder konvertierten Jet-Anwendungen können weiterhin Jet verwenden, um Dateien aus Microsoft Office 2003 und früher (.mdb und .xls) für nicht primäre Datenspeicher zu verwenden. Für diese Anwendungen sollten Sie jedoch eine Migration von Jet zur Microsoft Access-Datenbankengine planen. Sie können die [Microsoft Access-Datenbankengine herunterladen](https://www.microsoft.com/download/details.aspx?id=54920), mit der Sie Lese- und Schreibvorgänge in bereits vorhandenen Dateien im Office 2003- (.mdb und .xls) oder Office 2007-Dateiformat (*.accdb, *.xlsm, *.xlsx und *.xlsb) ausführen können.

  > [!IMPORTANT]
  > Lesen Sie den Office 2007-Endbenutzer-Lizenzvertrag, um sich über spezifische Nutzungseinschränkungen zu informieren.

  > [!NOTE]
  > SQL Server-Anwendungen können über den Office 2007-Systemtreiber auch auf Dateien aus Office 2007 und früher sowie Dateien aus SQL Server-Diensten für heterogene Datenkonnektivität und Integration Services zugreifen. Zusätzlich können 64-Bit-SQL Server-Anwendungen auf 32-Bit-Jet- und Office 2007-Systemdateien zugreifen, indem sie 32-Bit-SQL Server Integration Services (SSIS) unter 64-Bit-Windows verwenden.

* **Microsoft OLE DB-Anbieter für die Datenstrukturierung (Microsoft OLE DB Provider for Data Shaping, MSDADS):** Mit dem MSDADS können Sie hierarchische Beziehungen zwischen Schlüsseln, Feldern oder Rowsets in einer Anwendung erstellen. Seit MDAC 2.1 wurden keine größeren Featureverbesserungen vorgenommen. Dieser Anbieter ist veraltet. Microsoft empfiehlt, anstelle von MSDADS XML zu verwenden.
* **Oracle ODBC und Oracle OLE DB:** Der Microsoft Oracle ODBC-Treiber (Oracle ODBC) und der Microsoft OLE DB-Anbieter für Oracle (Oracle OLE DB) bieten Zugriff auf Oracle-Datenbankserver. Sie wurden mit Oracle Call Interface 7 (OCI) erstellt und bieten vollständige Unterstützung für Oracle 7. Darüber hinaus wird die Oracle 7-Emulation verwendet, um eingeschränkte Unterstützung für Oracle 8-Datenbanken bereitzustellen. Oracle hat die Unterstützung von Anwendungen eingestellt, die OCI-Aufrufe der Version 7 verwenden. Diese Technologien sind veraltet. Wenn Sie Oracle-Datenquellen verwenden, sollten Sie eine Migration zu einem von Oracle bereitgestellten Treiber und Anbieter durchführen.
* **Remotedatendienste (Remote Data Services, RDS):** RDS sind ein proprietärer Microsoft-Mechanismus für den Zugriff auf ADO-Recordset-Remoteobjekte über das Internet oder ein Intranet. RDS ist veraltet, seit MDAC 2.1 wurden keine größeren Featureverbesserungen an RDS vorgenommen. Microsoft hat das .NET-Framework veröffentlicht, das umfangreiche SOAP-Fähigkeiten umfasst und RDS-Komponenten ersetzt. Alle RDS-Serverkomponenten werden nach Windows 7 aus dem Betriebssystem entfernt.
* **Jet-Replikationsobjekte (Jet Replication Objects, JRO):** JRO sind veraltet. JRO wird innerhalb von ADO mit Jet-Datenbanken ( *.mdb) verwendet, um Jet-Datenbanken zu erstellen und zu komprimieren und die Jet-Replikationsverwaltung durchzuführen. MDAC 2.7 ist die letzte zugehörige Version. JRO werden in der 64-Bit-Version des Windows-Betriebssystems nicht verfügbar sein. JRO werden im Microsoft Access 2007-Dateiformat (* .accdb) nicht unterstützt.
* **16-Bit-ODBC-Unterstützung:** Wenn Sie 16-Bit-Anwendungen verwenden, sollten Sie eine Migration zu einer 32-Bit-Anwendung durchführen. Die 16-Bit-Funktionalität wird eingestellt und aus 64-Bit-Betriebssystemen entfernt. Weitere Informationen finden Sie im [Knowledge Base-Artikel 896458](https://support.microsoft.com/kb/896458).
* **OLEDB Simple Provider (MSDAOSP):** Der OLEDB Simple Provider bietet ein Framework für die schnelle Entwicklung von OLE DB-Anbietern über einfache Daten. MSDAOSP ist veraltet.
* **ODBC-Cursorbibliothek:** Die ODBC-Cursorbibliothek (ODBCCR32.dll) bietet eingeschränkte clientseitige Datencursor. Die ODBC-Cursorbibliothek wurde eingestellt. Ihre Anwendung kann serverseitige Cursorimplementierungen als Ersatz verwenden.
* **OLE DB-Remoteschnittstelle für die prozessexterne Ausführung:** Die OLEDB-Remoteschnittstelle (msdaps.dll) war ein Versuch, OLE DB-Anbietern eine prozessexterne Ausführung zu ermöglichen. Die OLE DB-Remoteschnittstelle für die prozessexterne Ausführung ist veraltet.
* **AppleTalk- und Banyan Vines-SQL-Netzwerkbibliotheken:** Die Banyan Vines-, AppleTalk-, ServerNet-, IPX/SPX-, Giganet- und RPC-SQL-Netzwerkbibliotheken sind veraltet. Wenn Sie eine dieser Technologien verwenden, sollten Sie Ihre Anwendungen so anpassen, dass eine andere Netzwerkbibliothek verwendet wird, z. B. TCP/IP oder Named Pipe.

### <a name="mdacwdac-releases"></a>MDAC/WDAC-Releases

Hier finden Sie eine Liste der Unterstützungsszenarios früherer MDAC/WDAC-Releases, beginnend mit dem frühesten:

* **MDAC 1.5, MDAC 2.0 und MDAC 2.1:** Diese Versionen von MDAC waren unabhängige Releases, die über das Windows NT Option Pack, das Microsoft Windows Platform SDK oder die MDAC-Website veröffentlicht wurden. Diese Versionen von MDAC werden nicht länger unterstützt.
* **MDAC 2.5:** Diese Version von MDAC war im Windows 2000-Betriebssystem enthalten. Service Packs von MDAC 2.5 waren in den entsprechenden Windows 2000 Service Packs enthalten.
* **MDAC 2.6:** MDAC 2.6 RTM, SP1 und SP2 waren in Microsoft SQL Server 2000 RTM, SP1 bzw. SP2 enthalten. Darüber hinaus wurden diese MDAC-Service-Packs gemäß dem Service-Pack-Releasezeitplan für Microsoft SQL Server 2000 auf der MDAC-Website veröffentlicht. Sie können diese Version von MDAC und die zugehörigen Service Packs auf Windows 2000-, Windows Millennium Edition-, Windows NT-, Windows 95- und Windows 98-Plattformen installieren. Diese Version von MDAC wird nicht länger unterstützt.
* **MDAC 2.7:** Diese Version von MDAC war in Microsoft Windows XP RTM- und SP1-Betriebssystemen enthalten. Sie können diese Version von MDAC und zugehörige Service Packs auf Windows 2000-, Windows Millennium-, Windows NT- und Windows 98-Plattformen installieren. Sie können diese Version auf der Windows XP-Plattform nur über das Betriebssystem oder entsprechende Service Packs installieren. Diese Version von MDAC wird nicht länger unterstützt.
* **MDAC 2.8:** Diese Version von MDAC war in Windows Server 2003 und Windows XP SP2 und höher enthalten. Sie können diese Version von MDAC und zugehörige Service Packs unter Windows 2000 installieren.

  * Die 32-Bit-Version von MDAC 2.8 wurde zeitgleich mit der Veröffentlichung von Windows Server 2003 für den Kunden auf der MDAC-Website veröffentlicht.
  * Die 64-Bit-Version von MDAC 2.8 wurde mit der 64-Bit-Version von Windows Server 2003 und Windows XP veröffentlicht.

* **Windows Data Access Components (WDAC):** Ab Windows Vista und Windows Server 2008 wurde der Name MDAC in WDAC – „Windows Data Access Components“ geändert. Die WDAC sind Teil des Betriebssystems und stehen nicht separat zur Weiterverteilung zur Verfügung. Die Wartung von WDAC unterliegt dem Lebenszyklus des Betriebssystems.

  32-Bit- und 64-Bit-Versionen von WDAC werden mit den 32-Bit- und 64-Bit-Versionen des Windows-Betriebssystems veröffentlicht.

## <a name="obsolete-data-access-technologies"></a>Veraltete Datenzugriffstechnologien

Veraltete Technologien sind Technologien, die in mehreren Produktversionen nicht mehr erweitert oder aktualisiert wurden und in zukünftige Produktversionen nicht mehr eingeschlossen werden. Verwenden Sie diese Technologien nicht, wenn Sie neue Anwendungen schreiben. Wenn Sie vorhandene Anwendungen modifizieren, die mit diesen Technologien geschrieben wurden, sollten Sie eine Migration dieser Anwendungen auf ADO.NET oder eine andere aktuelle Technologie in Erwägung ziehen.

Die folgenden Komponenten werden als veraltet betrachtet:

* **DB-Library:** DB-Library ist ein SQL Server-spezifisches Programmiermodell, das C-APIs umfasst. Seit SQL Server 6.5 wurden keine Featureverbesserungen an DB-Library vorgenommen. Das letzte Release wurde mit SQL Server 2000 veröffentlicht, und es findet keine Portierung auf die 64-Bit-Version des Windows-Betriebssystems statt.
* **Embedded SQL (E-SQL):** E-SQL ist ein SQL Server-spezifisches Programmiermodell, das die Einbettung von Transact-SQL-Anweisungen in Visual C-Code ermöglicht. Seit SQL Server 6.5 wurden keine Featureverbesserungen an E-SQL vorgenommen. Das letzte Release wurde mit SQL Server 2000 veröffentlicht, und es findet keine Portierung auf die 64-Bit-Version des Windows-Betriebssystems statt.
* **Data Access Objects (DAO):** DAO bietet Zugriff auf JET-Datenbanken (Access). Diese API kann aus Microsoft Visual Basic, Microsoft Visual C++ und Skriptsprachen verwendet werden. Sie war in Microsoft Office 2000 und Office XP enthalten. DAO 3.6 ist die letzte Version dieser Technologie. DAO werden in der 64-Bit-Version des Windows-Betriebssystems nicht verfügbar sein.
* **Remote Data Objects (RDO):** RDO wurde speziell für den Zugriff auf relationale ODBC-Remotedatenquellen entwickelt und erleichterte die Verwendung von ODBC ohne komplexen Anwendungscode. RDO war in den Versionen 4, 5 und 6 von Microsoft Visual Basic enthalten. RDO 2.0 war die letzte Version dieser Technologie.

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
