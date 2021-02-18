---
title: SQL Server PowerShell-Anbieter | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über den SQL Server-Anbieter für Windows PowerShell, der durch Pfade, die den Dateisystempfaden ähneln, Zugriff auf SQL Server-Objekte bietet.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
helpviewer_keywords:
- PowerShell [SQL Server], provider
- PowerShell [SQL Server], SQL Server PowerShell Provider
- Providers [PowerShell]
- SMO [SQL Server], PowerShell
- PowerShell [SQL Server], SMO
- SQL Server Management Objects, PowerShell
ms.assetid: b97acc43-fcd2-4ae5-b218-e183bab916f9
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 07/31/2019
ms.openlocfilehash: 5e62ed5e7482ca6fc1d80eb6e7f19ca1e0da4e7f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350166"
---
# <a name="sql-server-powershell-provider"></a>SQL Server PowerShell-Anbieter

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anbieter für Windows PowerShell macht die Hierarchie von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Objekten auf ähnliche Weise wie in Dateisystempfaden verfügbar. Mithilfe der Pfade können Sie Objekte finden und dann Methoden der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Object-Modelle (SMO) verwenden, um Aktionen für die Objekte auszuführen.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="benefits-of-the-sql-server-powershell-provider"></a>Vorteile des SQL Server PowerShell-Anbieters

Die vom [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anbieter implementierten Pfade ermögliche das einfache und interaktive Überprüfen aller Objekte in einer Instanz von SQL Server. Sie können in den Pfaden navigieren, indem Sie Windows PowerShell-Aliase ähnlich den Befehlen verwenden, die Sie normalerweise zum Navigieren in den Dateisystempfaden verwenden.  
  
## <a name="the-sql-server-powershell-hierarchy"></a>Die SQL Server PowerShell-Hierarchie

Produkte, deren Daten oder Objektmodelle in einer Hierarchie dargestellt werden können, verwenden Windows PowerShell-Anbieter, um die Hierarchien verfügbar zu machen. Die Hierarchie wird mithilfe einer Laufwerks- und Pfadstruktur verfügbar gemacht, die der für das Windows-Dateisystem verwendeten Struktur ähnelt.  
  
 Jeder Windows PowerShell-Anbieter implementiert ein oder mehrere Laufwerke. Jedes Laufwerk ist der Stammknoten einer Hierarchie verwandter Objekte. Der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anbieter implementiert ein Laufwerk mit der Bezeichnung SQLSERVER: Der Anbieter definiert auch einen Satz von primären Ordnern für das SQLSERVER:-Laufwerk. Jeder Ordner und seine Unterordner stellen den Satz von Objekten dar, auf die über ein [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Object-Modell zugegriffen werden kann. Wenn sich der Fokus auf einem Unterordner in einem Pfad befindet, der mit einem dieser primären Ordner beginnt, können Sie die Methoden des zugeordneten Objektmodells verwenden, um Aktionen für das vom Knoten dargestellte Objekt auszuführen. Die vom [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]-Anbieter implementierten Windows PowerShell-Ordner werden in der folgenden Tabelle aufgeführt:  
  
|Ordner|Namespace des SQL Server-Objektmodells|Objekte|  
|------------|---------------------------------------|-------------|  
|`SQLSERVER:\SQL`|<xref:Microsoft.SqlServer.Management.Smo><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.Agent><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.Broker><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.Mail>|Datenbankobjekte, wie Tabellen, Sichten und gespeicherte Prozeduren.|  
|`SQLSERVER:\SQLPolicy`|<xref:Microsoft.SqlServer.Management.Dmf><br /><br /> <xref:Microsoft.SqlServer.Management.Facets>|Richtlinienbasierte Verwaltungsobjekte, z. B. Richtlinien und Facets|  
|`SQLSERVER:\SQLRegistration`|<xref:Microsoft.SqlServer.Management.RegisteredServers><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.RegSvrEnum>|Registrierte Serverobjekte, z. B. Servergruppen und registrierte Server|  
|`SQLSERVER:\Utility`|<xref:Microsoft.SqlServer.Management.Utility>|Hilfsprogrammobjekte, z. B. verwaltete [!INCLUDE[ssDE](../includes/ssde-md.md)]-Instanzen|  
|`SQLSERVER:\DAC`|[Microsoft.SqlServer.Management.Dac](/previous-versions/sql/sql-server-2012/ee212127(v=sql.110))|Datenebenenanwendungs-Objekte z. B. DAC-Pakete und Vorgänge wie das Bereitstellen einer DAC|  
|`SQLSERVER:\DataCollection`|<xref:Microsoft.SqlServer.Management.Collector>|Datensammler-Objekte, z. B. Sammlungssätze und Konfigurationsspeicher|  
|`SQLSERVER:\SSIS`|<xref:Microsoft.SqlServer.Management.IntegrationServices>|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] -Objekte, z. B. Projekte, Pakete und Umgebungen|  
|`SQLSERVER:\XEvent`|<xref:Microsoft.SqlServer.Management.XEvent>|Erweiterte Ereignisse von SQL Server|
|`SQLSERVER:\DatabaseXEvent`|[Microsoft.SqlServer.Management.XEventDbScoped](/dotnet/api/microsoft.sqlserver.management.xeventdbscoped)|Erweiterte Ereignisse von SQL Server|
|`SQLSERVER:\SQLAS`|<xref:Microsoft.AnalysisServices>|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] -Objekte, z. B. Cubes, Aggregationen und Dimensionen|  
  
 Sie können z. B. mit dem Ordner "SQLSERVER:\SQL" Pfade beginnen, die jedes vom SMO-Objektmodell unterstützte Objekt darstellen können. Der erste Teil eines SQLSERVER:\SQL-Pfads ist SQLSERVER:\SQL\\*Computername*\\*Instanzname*. Die Knoten nach dem Instanznamen sind Objektsammlungen (wie *Datenbanken* oder *Sichten*) und Objektnamen (wie AdventureWorks2012). Schemas werden nicht als Objektklassen dargestellt. Wenn Sie den Knoten für ein Objekt der höchsten Ebene in einem Schema angeben, wie beispielsweise eine Tabelle oder eine Sicht, müssen Sie den Objektnamen im Format *Schemaname.Objektname* angeben.  
  
 Im folgenden Beispiel wird der Pfad der Vendor-Tabelle im Purchasing-Schema der AdventureWorks2012-Datenbank in einer Standardinstanz von [!INCLUDE[ssDE](../includes/ssde-md.md)] auf dem lokalen Computer angezeigt:  
  
```powershell
SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Tables\Purchasing.Vendor  
```
  
 Weitere Informationen zur SMO-Objektmodellhierarchie finden Sie unter [SMO Object Model Diagram](../relational-databases/server-management-objects-smo/smo-object-model-diagram.md).  
  
 Auflistungsknoten in einem Pfad werden im zugeordneten Objektmodell einer Auflistung zugeordnet. Objektnamenknoten werden wie in der folgenden Tabelle dargestellt einer Objektklasse im zugeordneten Objektmodell zugeordnet:  
  
|`Path`|SMO-Klasse|  
|----------|---------------|  
|`SQLSERVER:\SQL\MyComputer\DEFAULT\Databases`|<xref:Microsoft.SqlServer.Management.Smo.DatabaseCollection>|  
|`SQLSERVER:\SQL\MyComputer\DEFAULT\Databases\AdventureWorks2012`|<xref:Microsoft.SqlServer.Management.Smo.Database>|  
  
## <a name="sql-server-provider-tasks"></a>SQL Server-Anbietertasks  
  
|Taskbeschreibung|Artikel|  
|----------------------|-----------|  
|Beschreibt, wie Windows PowerShell-Cmdlets verwendet werden, um durch die Knoten in einem Pfad zu navigieren und an jedem Knoten eine Liste der Objekte in diesem Knoten abzurufen.|[Navigieren in SQL Server PowerShell-Pfaden](navigate-sql-server-powershell-paths.md)|  
|Beschreibt, wie die SMO-Methoden und SMO-Eigenschaften verwendet werden, um Berichte über ein Objekt, das von einem Knoten in einem Pfad dargestellt wird, zu erstellen und es zu verarbeiten. Beschreibt darüber hinaus, wie eine Liste der SMO-Methoden und SMO-Eigenschaften für diesen Knoten abgerufen wird.|[Verwenden von SQL Server PowerShell-Pfaden](work-with-sql-server-powershell-paths.md)|  
|Beschreibt, wie ein SMO Uniform Resource Name (URN) in einen SQL Server-Anbieterpfad konvertiert wird.|[Konvertieren von URNs in SQL Server-Anbieterpfade](/powershell/module/sqlserver/Convert-UrnToPath)|  
|Beschreibt, wie SQL Server-Authentifizierungsverbindungen mit dem [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anbieter geöffnet werden. Standardmäßig verwendet der Anbieter Windows-Authentifizierungsverbindungen, die mit den Anmeldeinformationen des Windows-Kontos hergestellt wurden, das die Windows PowerShell-Sitzung ausführt.|[Verwalten der Authentifizierung in PowerShell der Datenbank-Engine](manage-authentication-in-database-engine-powershell.md)|  
  
## <a name="next-steps"></a>Nächste Schritte

[SQL Server-PowerShell](sql-server-powershell.md)