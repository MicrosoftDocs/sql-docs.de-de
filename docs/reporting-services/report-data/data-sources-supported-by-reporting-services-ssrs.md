---
title: Von Reporting Services unterstützte Datenquellen | Microsoft-Dokumentation
description: In diesem Artikel erhalten Sie Informationen zu den verschiedenen von den Reporting Services unterstützten Datenquellen, einschließlich Microsoft SQL Server, Oracle und ODBC.
ms.date: 11/10/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
helpviewer_keywords:
- SQL Server data processing extension [Reporting Services]
- XML data processing extension [Reporting Services]
- reports [Reporting Services], data
- data processing extensions [Reporting Services], data sources
- Oracle [Reporting Services]
- OLE DB data processing extension
- data sources [Reporting Services], about data sources
- ODBC data processing extension
- Reporting Services, data sources
ms.assetid: 9d11d055-a3be-45aa-99a7-46447a94ed42
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a92e47d43e018c467ad53c06aa2943b9d51d1766
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978592"
---
# <a name="data-sources-supported-by-reporting-services-ssrs"></a>Von Reporting Services unterstützte Datenquellen (SSRS)
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] werden Berichtsdaten von Datenquellen über eine modulare und erweiterbare Datenschicht abgerufen, für die Datenverarbeitungserweiterungen verwendet werden. Zum Abrufen von Berichtsdaten von einer Datenquelle müssen Sie eine Datenverarbeitungserweiterung auswählen, die den Typ der Datenquelle, die Version der für die Datenquelle ausgeführten Software und die Plattform der Datenquelle (32-Bit oder 64-Bit [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]) unterstützt.  
  
 Wenn Sie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]bereitstellen, wird eine Reihe von Datenverarbeitungserweiterungen sowohl auf dem Berichterstellungsclient als auch auf dem Berichtsserver automatisch installiert und registriert, um den Zugriff auf verschiedene Datenquellentypen zu ermöglichen. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] werden die folgenden Datenquellentypen installiert:  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] für MDX, DMX, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] und tabellarische Modelle  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]  
  
-   Oracle  
  
-   SAP BW 
  
-   [!INCLUDE[extEssbase](../../includes/extessbase-md.md)]  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] SharePoint-Liste  
  
-   Teradata  
  
-   OLE DB  
  
-   ODBC  
  
-   XML  
  
 Zusätzlich können von Systemadministratoren benutzerdefinierte Datenverarbeitungserweiterungen und [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]-Standarddatenanbieter installiert und registriert werden. Zum Verarbeiten und Anzeigen eines Berichts müssen die Datenverarbeitungserweiterungen und Datenanbieter auf dem Berichtsserver installiert und registriert sein. Zum Anzeigen einer Vorschau für einen Bericht müssen sie auf dem Berichterstellungsclient installiert und registriert sein. Datenverarbeitungserweiterungen und Datenanbieter müssen für die Plattform, auf der sie installiert sind, systemintern kompiliert werden. Wenn Sie mit dem SOAP-Webdienst eine Datenquelle programmgesteuert bereitstellen, müssen Sie die Datenquellenerweiterung definieren. Verwenden Sie Datenerweiterungswerte aus der Datei **RSReportDesigner.config** . Standardmäßig befindet sich die Datei im folgenden Ordner:  
  
```  
<drive letter>\Program Files (x86)\Microsoft Visual Studio 10.0\Common7\IDE\PrivateAssemblies  
```  
  
 Die [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Datenerweiterung lautet z.B. OLEDB-MD.  
  
 Viele [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] -Standarddatenanbieter von Drittanbietern sind im [Microsoft Download Center](https://www.microsoft.com/download/default.aspx) und auf den entsprechenden Drittanbieterseiten als Downloads verfügbar. Sie können auch das öffentliche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Forum nach Informationen zu dritten Datenanbietern durchsuchen.  
  
> [!NOTE]  
>  Es wird von [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] -Standarddatenanbietern nicht notwendigerweise die gesamte Funktionalität unterstützt, die von Datenverarbeitungserweiterungen von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] bereitgestellt wird. Mit einigen OLE DB-Datenanbietern und ODBC-Treibern können Sie außerdem zwar Berichte erstellen und in einer Vorschau anzeigen, doch bieten sie keine Unterstützung für auf einem Berichtsserver veröffentlichte Berichte. Beispielsweise wird der [!INCLUDE[msCoName](../../includes/msconame-md.md)] -OLE DB-Anbieter für Jet auf dem Berichtsserver nicht unterstützt. Weitere Informationen finden Sie unter [Datenverarbeitungserweiterungen und .NET Framework-Datenanbieter &#40;SSRS&#41;](../../reporting-services/report-data/data-processing-extensions-and-net-framework-data-providers-ssrs.md).  
  
 Weitere Informationen zu benutzerdefinierten Datenverarbeitungserweiterungen finden Sie unter [Implementing a Data Processing Extension](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md). Weitere Informationen zu [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] -Standarddatenanbietern finden Sie unter dem <xref:System.Data> -Namespace.   
  
## <a name="platform-support-for-report-data-sources"></a>Plattformunterstützung für Berichtsdatenquellen  
 Die Datenquellen, die Sie in einer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Bereitstellung verwenden können, hängen von der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Edition, der [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Version und von der Plattform ab. Weitere Informationen finden Sie unter [Reporting Services Features Supported by the Editions of SQL Server (Von den SQL Server-Editionen unterstützte Reporting Services-Funktionen)](../../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md). Die weiter unten in diesem Thema dargestellte Tabelle enthält Informationen zu unterstützten Datenquellen, sortiert nach Version und Plattform.  
  
 Für den Berichterstellungsclient und den Berichtsserver gelten unterschiedliche Bedingungen bezüglich der Plattform für [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenquellen.  
  
### <a name="on-the-report-authoring-client"></a>Auf dem Berichterstellungsclient  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] ist eine 32-Bit-Anwendung. [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] wird auf einer Itanium-basierten Plattform nicht unterstützt. Wenn Sie auf einer x64-Plattform im Berichts-Designer Berichte bearbeiten und als Vorschau anzeigen möchten, müssen im (x86)-Verzeichnis der Plattform 32-Bit-Datenanbieter installiert sein.  
  
### <a name="on-the-report-server"></a>Auf dem Berichtsserver  
 Wenn Sie einen Bericht auf einem 64-Bit-Berichtsserver bereitstellen, müssen auf dem Berichtsserver systemintern kompilierte 64-Bit-Datenanbieter installiert sein. Das Einschließen von 32-Bit-Datenanbietern in 64-Bit-Schnittstellen wird nicht unterstützt. Weitere Informationen finden Sie in der Dokumentation des jeweiligen Datenanbieters.  
  
## <a name="supported-data-sources"></a>Unterstützte Datenquellen  
 In der folgenden Tabelle sind [!INCLUDE[msCoName](../../includes/msconame-md.md)] -Datenverarbeitungserweiterungen und Datenanbieter aufgeführt, die Sie zum Abrufen von Daten für Berichtsdatasets und Berichtsmodelle verwenden können. Weitere Informationen zu Erweiterungen oder Datenanbietern erhalten Sie, indem Sie auf den Link in der zweiten Spalte klicken. Die Tabellenspalten enthalten folgende Informationen:  
  
-   Die Quelle der Berichtsdaten: Der Datentyp, auf den zugegriffen wird, z. B. relationale Datenbank, mehrdimensionale Datenbank, Flatfile oder XML. In dieser Spalte wird die folgende Frage beantwortet: „Welche Datentypen kann [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] für einen Bericht verwenden?“  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Datenquellentyp: Einer der Datenquellentypen, die in der Dropdownliste angezeigt werden, wenn Sie in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] eine Datenquelle definieren. Diese Liste enthält die installierten und registrierten Datenverarbeitungserweiterungen und Datenanbieter. In dieser Spalte wird die folgende Frage beantwortet: „Welchen Datenquellentyp wähle ich beim Erstellen einer Berichtsdatenquelle in der Dropdownliste aus?“  
  
-   Name der Datenverarbeitungserweiterung/des Datenanbieters: Die [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Datenverarbeitungserweiterung oder andere Datenanbieter, die dem ausgewählten [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Datenquellentyp entsprechen. In dieser Spalte wird die folgende Frage beantwortet: „Welche Datenverarbeitungserweiterung oder welcher Datenanbieter wird verwendet, wenn ich einen Datenquellentyp auswähle?“  
  
-   Zugrunde liegende Datenanbieterversion (optional): Einige Datenquellentypen unterstützen mehrere Datenanbieter. Dabei kann es sich um unterschiedliche Versionen eines Datenanbieters oder um unterschiedliche Implementierungen von Drittanbietern für einen Typ von Datenanbieter handeln. Der Name des Anbieters wird nach dem Konfigurieren einer Datenquelle häufig in der Verbindungszeichenfolge angegeben. In dieser Spalte wird die folgende Frage beantwortet: „Welchen Datenanbieter wähle ich nach der Auswahl des Datenquellentyps im Dialogfeld **Verbindungseigenschaften** aus?“  
  
-   Datenquelle *\<platform>* : Die von der Datenverarbeitungserweiterung oder dem Datenanbieter für die Zieldatenquelle unterstützte Plattform. In dieser Spalte wird die folgende Frage beantwortet: „Können mit dieser Datenverarbeitungserweiterung oder diesem Datenanbieter Daten von einer Datenquelle auf diesem Plattformtyp abgerufen werden?“  
  
-   Version der Datenquelle: Die Version der Zieldatenquelle, die von der Datenverarbeitungserweiterung oder dem Datenanbieter unterstützt wird. In dieser Spalte wird die folgende Frage beantwortet: „Können mit dieser Datenverarbeitungserweiterung oder diesem Datenanbieter Daten von dieser Version der Datenquelle abgerufen werden?“  
  
-   RS *\<platform>* : Die Plattformen für den Berichtsserver und den Berichterstellungsclient, auf denen Sie eine benutzerdefinierte Datenverarbeitungserweiterung oder einen benutzerdefinierten Datenanbieter installieren können. Die integrierten Datenverarbeitungserweiterungen von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] werden bei jeder Installation von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]bereitgestellt. Eine benutzerdefinierte Datenverarbeitungserweiterung oder ein [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] -Datenanbieter müssen systemintern für eine bestimmte Plattform kompiliert werden. In dieser Spalte wird die folgende Frage beantwortet: „Kann diese Datenverarbeitungserweiterung oder dieser Datenanbieter auf diesem Plattformtyp installiert werden?“  
  
###  <a name="types-of-data-sources"></a><a name="DataSourcesTable"></a> Typen von Datenquellen  
  
|Quelle der<br /><br /> Berichtsdaten|Reporting Services-Datenquellentyp|Name der Datenverarbeitungserweiterung/des Datenanbieters|Zugrunde liegende Datenanbieterversion<br /><br /> (Optional)|Daten<br /><br /> `Source`<br /><br /> Plattform x86|Daten<br /><br /> `Source`<br /><br /> Plattform x64|Version der Datenquelle|RS<br /><br /> Plattform x86|RS<br /><br /> Plattform x64|  
|-------------------------------|-----------------------------------------|------------------------------------------------------|-------------------------------------------------------|--------------------------------------|--------------------------------------|----------------------------|-------------------------|-------------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank|[Microsoft SQL Server](#MicrosoftSQLServer)|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Erweitert System.Data.SqlClient|J|J|SQL Server 2012 und höher.|J|J|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank|OLEDB|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Erweitert System.Data.OledbClient|J|J|SQL Server 2012 und höher.|J|J|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank|[ODBC](#ODBC)|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Erweitert System.Data.OdbcClient|J|J|SQL Server 2012 und höher.|J|J|  
|[!INCLUDE[ssSDS](../../includes/sssds-md.md)]|[Microsoft Azure SQL-Datenbank](#Azure)|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Erweitert System.Data.SqlClient|–|N/V|[!INCLUDE[ssSDS](../../includes/sssds-md.md)]|J|J|
|Azure Synapse Analytics|[Microsoft Azure SQL-Datenbank](#Azure)|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Erweitert System.Data.SqlClient|–|N/V|Azure Synapse Analytics|J|J| 
|[!INCLUDE[ssDW](../../includes/ssdw-md.md)] -Anwendung|[Microsoft Parallel Data Warehouse](#PWD)|Die Unterstützung für die Datenverarbeitungserweiterung [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] wurde eingestellt.|–|–|N/V|[!INCLUDE[ssDWfull](../../includes/ssdwfull-md.md)]|N|N|  
|Mehrdimensionale oder tabellarische [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Datenbank|[Microsoft SQL Server Analysis Services](#AnalysisServices)|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Verwendet ADOMD.NET|J|J|SQL Server 2012 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] und höher|J|J|  
|Power BI Premium-Dataset (ab Reporting Services 2019 und Power BI-Berichtsserver im Januar 2020) |[Microsoft SQL Server Analysis Services](#AnalysisServices)|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Verwendet ADOMD.NET|J|J|SQL Server 2019 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] und höher|J|J|
|Azure Analysis Services |[Microsoft SQL Server Analysis Services](#AnalysisServices)|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Verwendet ADOMD.NET|J|J|SQL Server 2017 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] und höher|J|J| 
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Datenbank|OLEDB|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Erweitert System.Data.OledbClient<br /><br /> Version 10.0|J|J|SQL Server 2012 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|J|J|
|SharePoint-Listen|[Microsoft SharePoint-Liste](#SharePointList)|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Ruft Daten aus Lists.asmx oder den API-Schnittstellen für SharePoint-Objektmodelle ab.<br /><br /> Siehe [Hinweis](#SharePointList).|N|J|Produkte für SharePoint 2013 und höher|J|J|   
|XML|[XML](#XML)|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|XML-Datenquellen sind von keiner bestimmten Plattform abhängig.|–|N/V|[!INCLUDE[vstecwebservices](../../includes/vstecwebservices-md.md)] oder -Dokumente|J|J|  
|Berichtsservermodell|Berichtsmodell|Die Unterstützung für die Datenverarbeitungserweiterung [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] für eine veröffentlichte SMDL-Datei wurde eingestellt.|Für Datenquellen für ein Modell werden integrierte Datenverarbeitungserweiterungen verwendet.<br /><br /> Auf Oracle basierende Modelle erfordern Clientkomponenten für Oracle.<br /><br /> Teradata-basierte Modelle erfordern den .NET-Datenanbieter für Teradata von Teradata.<br /><br /> Informationen zur Plattformunterstützung finden Sie in der Teradata-Dokumentation.|–|N/V|Modelle können in folgenden Versionen erstellt werden:[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] und höher.<br /><br /> [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]<br /><br /> Oracle 9.2.0.3 oder höher<br /><br /> Teradata, Versionen 14, 13, 12 und 6.2|N|N|  
|Mehrdimensionale SAP-Datenbank|SAP BW|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Informationen zur Plattformunterstützung finden Sie in der SAP-Dokumentation.|–|N/V|SAP BW 7.0-7.5|J|–|  
|[!INCLUDE[extEssbase](../../includes/extessbase-md.md)]|[Hyperion Essbase](#Hyperion)|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Informationen zur Plattformunterstützung finden Sie in der Hyperion-Dokumentation.|J|–|[!INCLUDE[extEssbase](../../includes/extessbase-md.md)] 9.3.1|J|–|  
|Relationale Oracle-Datenbank|[Oracle](#OracleClient)|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Erfordert Oracle-Clientkomponenten der Version 12c oder höher.|J|–|Oracle 11g, 11g R2, 12c, 18c und 19c|J|J|  
|Teradata |[Teradata](#Teradata)|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Erweitert den .NET-Datenanbieter für Teradata von Teradata.<br /><br /> Erfordert den .NET-Datenanbieter für Teradata von Teradata.<br /><br /> Informationen zur Plattformunterstützung finden Sie in der Teradata-Dokumentation.|J|–|Teradata, Version 15<br /><br />Teradata, Version 14<br /><br /> Teradata, Version 13|J|N|  
|Relationale DB2-Datenbank|Name der angepassten, registrierten Datenerweiterung||Host Integration (HI) Server 2004<br /><br /> |J|–|N/V|J|N|  
|OLE DB-Standarddatenquelle|OLEDB|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Jede Datenquelle, die OLE DB unterstützt.<br /><br /> Informationen zur Plattformunterstützung finden Sie in der Dokumentation zur Datenquelle.|J|–|Jede Datenquelle, die OLE DB unterstützt. Siehe [Hinweis](#OLEDBStandard).|J|–|  
|ODBC-Standarddatenquelle|[ODBC](#ODBCGeneric)|Integrierte [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung|Jede Datenquelle, die ODBC unterstützt.<br /><br /> Informationen zur Plattformunterstützung finden Sie in der Dokumentation zur Datenquelle.|J|–|Jede Datenquelle, die ODBC unterstützt. Siehe [Hinweis](#ODBCGeneric).|J|J|  
  
 Informationen zur Verwendung externer Datenquellen finden Sie unter [Hinzufügen von Daten aus externen Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/add-data-from-external-data-sources-ssrs.md).  
  
 Viele [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] -Standarddatenanbieter sind von Drittanbietern erhältlich. Weitere Informationen finden Sie auf den Websites oder in den Foren der entsprechenden Drittanbieter.  
  
 Bevor Sie eine benutzerdefinierte Datenverarbeitungserweiterung oder einen [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] -Standarddatenanbieter installieren und registrieren, sollten Sie sich im Referenzmaterial des Datenanbieters unbedingt über diese Vorgänge informieren. Weitere Informationen finden Sie unter [Registrieren eines .NET Framework-Standarddatenproviders &#40;SSRS&#41;](../../reporting-services/report-data/register-a-standard-net-framework-data-provider-ssrs.md).  
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
## <a name="reporting-services-data-processing-extensions"></a>Datenverarbeitungserweiterungen für Reporting Services  
 Die folgenden Datenverarbeitungserweiterungen werden automatisch mit [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] und [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)]installiert. Weitere Informationen und die Möglichkeit, die Installation zu überprüfen, finden Sie unter [RSReportDesigner-Konfigurationsdatei](../../reporting-services/report-server/rsreportdesigner-configuration-file.md) und [RsReportServer.config-Konfigurationsdatei](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
> [!NOTE]
>  Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Datenverarbeitungserweiterung wird derzeit nicht unterstützt.  
  
 Weitere Informationen zu Datenverarbeitungserweiterungen, die vom Berichts-Generator unterstützt werden, finden Sie unter [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).
  
###  <a name="microsoft-sql-server-data-processing-extension"></a><a name="MicrosoftSQLServer"></a> Microsoft SQL Server-Datenverarbeitungserweiterung  
 Beim Datenquellentyp **Microsoft SQL Server** wird der [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] -Datenanbieter für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]umschlossen und erweitert. Diese Datenverarbeitungserweiterung wird für x86- und [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]-basierte Plattformen nativ kompiliert und auf diesen Plattformen ausgeführt.  
  
 In [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] ist der dieser Datenerweiterung zugeordnete Abfrage-Designer der Visual Database Tool-Designer. Wenn Sie den Abfrage-Designer im grafischen Modus verwenden, wird die Abfrage analysiert und möglicherweise umgeschrieben. Wenn Sie die exakte [!INCLUDE[tsql](../../includes/tsql-md.md)] -Syntax für eine Abfrage steuern möchten, verwenden Sie den textbasierten Abfrage-Designer. Weitere Informationen finden Sie unter [Graphical Query Designer User Interface](../../reporting-services/report-data/graphical-query-designer-user-interface.md).  
  
 Weitere Informationen finden Sie unter [SQL Server-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/sql-server-connection-type-ssrs.md).  
  
 Im Berichts-Generator ist der dieser Datenerweiterung zugeordnete Abfrage-Designer der relationale Abfrage-Designer. 
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
###  <a name="microsoft-azure-sql-database-processing-extension"></a><a name="Azure"></a> Microsoft Azure SQL-Datenbank-Verarbeitungserweiterung  
 Beim Datenquellentyp **Microsoft Azure[!INCLUDE[ssSDS](../../includes/sssds-md.md)]** wird der Datenanbieter [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] umschlossen und erweitert.  
  
 In [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] ist der grafische, dieser Datenerweiterung zugeordnete Abfrage-Designer der relationale Abfrage-Designer und nicht der Visual Database Tool-Designer, den Sie mit dem Datenquellentyp **Microsoft SQL Server** verwenden.  
  
 [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] unterscheidet automatisch zwischen **Microsoft Azure[!INCLUDE[ssSDS](../../includes/sssds-md.md)]** - und **Microsoft SQL Server**-Datenquellentypen und öffnet den grafischen, dem Datenquellentyp zugeordneten Abfrage-Designer.  
  
 Wenn Sie den Abfrage-Designer im grafischen Modus verwenden, wird die Abfrage analysiert und möglicherweise umgeschrieben. Ein textbasierter Abfrage-Designer ist ebenfalls für das Schreiben von Abfragen verfügbar. Wenn Sie die exakte [!INCLUDE[tsql](../../includes/tsql-md.md)] -Syntax für eine Abfrage steuern möchten, verwenden Sie den textbasierten Abfrage-Designer.   
  
 Das Abrufen von Daten aus [!INCLUDE[ssSDS](../../includes/sssds-md.md)], Azure Synapse Analytics und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist ähnlich, aber es gibt einige Anforderungen, die nur für [!INCLUDE[ssSDS](../../includes/sssds-md.md)]gelten. Weitere Informationen finden Sie unter [Azure SQL-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/sql-azure-connection-type-ssrs.md).  
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
###  <a name="microsoft-sql-server-parallel-data-warehouse-processing-extension"></a><a name="PWD"></a> Microsoft SQL Server Parallel Data Warehouse-Datenverarbeitungserweiterung  
Die Unterstützung für diese Datenquelle wurde eingestellt. Verwenden Sie den SQL Server-Datenquellentypen, um eine Verbindung zur Microsoft Analytics Platform (APS) herzustellen.
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
###  <a name="microsoft-sql-server-analysis-services-data-processing-extension"></a><a name="AnalysisServices"></a> Microsoft SQL Server Analysis Services-Datenverarbeitungserweiterung  
 Wenn Sie den Datenquellentyp **Microsoft SQL Server Analysis Services** auswählen, müssen Sie eine [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Datenverarbeitungserweiterung auswählen, mit der der [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]-Datenanbieter für [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] erweitert wird. Diese Datenverarbeitungserweiterung wird für x86- und x64-basierte Plattformen systemintern kompiliert und auf diesen Plattformen ausgeführt.  
  
 Für diesen Datenanbieter wird das ADOMD.NET-Objektmodell verwendet, um Abfragen mit XMLA (XML for Analysis), Version 1.0, zu erstellen. Ergebnisse werden als vereinfachtes Rowset zurückgegeben. Weitere Informationen finden Sie unter [Analysis Services Connection Type for MDX (SSRS) (Analysis Services-Verbindungstyp für MDX (SSRS))](../../reporting-services/report-data/analysis-services-connection-type-for-mdx-ssrs.md), [Analysis Services Connection Type for DMX (SSRS) (Analysis Services-Verbindungstyp für DMX (SSRS))](../../reporting-services/report-data/analysis-services-connection-type-for-dmx-ssrs.md), [Analysis Services MDX Query Designer User Interface (Benutzeroberfläche des MDX-Abfrage-Designers für Analysis Services)](../../reporting-services/report-data/analysis-services-mdx-query-designer-user-interface.md) und [Analysis Services DMX Query Designer User Interface (Benutzeroberfläche des DMX-Abfrage-Designers für Analysis Services)](../../reporting-services/report-data/analysis-services-dmx-query-designer-user-interface.md). 
 
 Beachten Sie bei Azure Analysis Services und Power BI Premium-Datasetdatenquellen, dass die mehrstufige Authentifizierung für die Anmeldeinformationen, mit denen eine Verbindung zur Datenquelle hergestellt wird, deaktiviert sein muss. Wenn für Ihre Umgebung die mehrstufige Authentifizierung aktiviert werden muss, ziehen Sie den <a href="/azure/active-directory/conditional-access/overview">bedingten Azure Active Directory-Zugriff</a> als Option in Betracht, um die mehrstufige Authentifizierung für die in der Datenquelle verwendeten Anmeldeinformationen zu deaktivieren.
  
 Wenn Sie ein Power BI Premium-Dataset als Datenquelle verwenden, werden nur der Import-Modus und DirectQuery unterstützt.
  
 Bei Verbindung mit einer [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Datenquelle unterstützt die [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Datenverarbeitungserweiterung mehrwertige Parameter und ordnet Zell- und Elementeigenschaften erweiterten Eigenschaften zu, die von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] unterstützt werden. Weitere Informationen finden Sie unter [Erweiterte Feldeigenschaften für eine Analysis Services-Datenbank (SSRS)](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md).  
  
 Sie können auch Modelle aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Datenquellen erstellen.  
  
###  <a name="ole-db-data-processing-extension"></a><a name="OLEDBAll"></a> OLE DB Data Processing Extension  
 Für die OLE DB-Datenverarbeitungserweiterung ist die Auswahl eines zusätzlichen Datenanbieters in Abhängigkeit von der Version der im Bericht zu verwendenden Datenquelle erforderlich. Wenn Sie keinen bestimmten Datenanbieter auswählen, wird ein Standardwert bereitgestellt. Wählen Sie einen spezifischen Datenanbieter im Dialogfeld **Verbindungseigenschaften** aus, das Sie über das Dialogfeld „Datenquelle“ oder „Freigegebene Datenquelle“ über die Schaltfläche **Bearbeiten** öffnen können.  
  
 Weitere Informationen zum OLE DB-zugeordneten Abfrage-Designer finden Sie unter [Grafische Benutzeroberfläche des Abfrage-Designers](../../reporting-services/report-data/graphical-query-designer-user-interface.md). Weitere Informationen zur Unterstützung bestimmter OLE DB-Anbieter finden Sie in der [Knowledge Base unter](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/811241) Visual Studio .NET-Designer-Tool unterstützt bestimmte OLE DB-Anbieter [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
####  <a name="ole-db-for-sql-server"></a><a name="OLEDBSQL"></a> OLE DB für SQL Server  
 Wenn Sie den Datenquellentyp **OLE DB** auswählen, müssen Sie eine [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung auswählen, mit der der [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] -Datenanbieter für OLE DB erweitert wird. Diese Datenverarbeitungserweiterung wird für x86- und x64-Plattformen systemintern kompiliert und auf diesen Plattformen ausgeführt.  
  
 Weitere Informationen finden Sie unter [OLE DB-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/ole-db-connection-type-ssrs.md).  
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
#### <a name="ole-db-for-olap-70"></a>OLE DB für OLAP 7.0  
 OLE DB-Anbieter für OLAP Services 7.0 wird nicht unterstützt.  
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
####  <a name="ole-db-for-oracle"></a><a name="OracleOLEDB"></a> OLE DB für Oracle  
 Die Datenverarbeitungserweiterung OLE DB für Oracle unterstützt die folgenden Oracle-Datentypen nicht: BLOB, CLOB, NCLOB, BFILE, UROWID.  
  
 Unbenannte, positionsabhängige Parameter werden unterstützt. Benannte Parameter werden von dieser Erweiterung nicht unterstützt. Benannte Parameter können Sie nur mit der [Oracle](#OracleClient) -Datenverarbeitungserweiterung verwenden.  
  
 Weitere Informationen zum Konfigurieren von Oracle als Datenquelle finden Sie unter [Verwenden von Reporting Services zum Konfigurieren und Zugreifen auf eine Oracle-Datenquelle](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/834305). Weitere Informationen zur Konfiguration zusätzlicher Berechtigungen finden Sie in der [Knowledge Base unter](https://mskb.pkisolutions.com/kb/870668) Hinzufügen von Berechtigunen für den NETWORK SERVICE-Sicherheitsprinzipal [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
####  <a name="ole-db-standard-net-framework-data-provider"></a><a name="OLEDBStandard"></a> .NET Framework-Standarddatenanbieter für OLE DB  
 Verwenden Sie zum Abrufen von Daten von einer Datenquelle, die [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] -Datenanbieter für OLE DB unterstützt, den Datenquellentyp **OLE DB** , und wählen Sie den Standarddatenanbieter aus, oder wählen Sie im Dialogfeld **Verbindungszeichenfolge** einen der installierten Datenanbieter aus.  
  
> [!NOTE]  
>  Zwar kann ein Datenanbieter die Vorschau eines Berichts auf dem Berichterstellungsclient unterstützen, doch unterstützen nicht alle OLE DB-Datenanbieter auf einem Berichtsserver veröffentlichte Berichte.  
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
###  <a name="odbc-data-processing-extension"></a><a name="ODBC"></a> ODBC Data Processing Extension  
 Wenn Sie den Datenquellentyp **ODBC** auswählen, müssen Sie eine [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung auswählen, mit der der [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] -Datenanbieter für ODBC erweitert wird. Diese Datenverarbeitungserweiterung wird für x86- und [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)] -Plattformen systemintern kompiliert und auf diesen Plattformen ausgeführt. Mit dieser Erweiterung können Sie mit beliebigen Datenquellen, die über einen ODBC-Anbieter verfügen, eine Verbindung herstellen und von diesen Daten abrufen.  
  
> [!NOTE]  
>  Zwar kann ein Datenanbieter die Vorschau eines Berichts auf dem Berichterstellungsclient unterstützen, doch unterstützen nicht alle ODBC-Datenanbieter auf einem Berichtsserver veröffentlichte Berichte.  
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
####  <a name="odbc-standard-net-framework-data-provider"></a><a name="ODBCGeneric"></a> .NET Framework-Standarddatenanbieter für ODBC  
 Verwenden Sie zum Abrufen von Daten von einer Datenquelle, die einen [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] -Standarddatenanbieter für ODBC unterstützt, den Datenquellentyp **ODBC** , und wählen Sie den Standarddatenanbieter aus, oder wählen Sie im Dialogfeld **Verbindungszeichenfolge** einen der installierten Datenanbieter aus.  
  
> [!NOTE]  
>  Zwar kann ein Datenanbieter die Vorschau eines Berichts auf dem Berichterstellungsclient unterstützen, doch unterstützen nicht alle ODBC-Datenanbieter auf einem Berichtsserver veröffentlichte Berichte.  
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
###  <a name="oracle-data-processing-extension"></a><a name="OracleClient"></a> Oracle-Datenverarbeitungserweiterung  
 Wenn Sie den Datenquellentyp **Oracle** auswählen, entscheiden Sie sich für eine [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Datenverarbeitungserweiterung, die den Oracle-Datenanbieter direkt verwendet, anstatt über den System.Data.OracleClient auszuweichen. Berichtsdaten können von einer Oracle-Datenbank nur abgerufen werden, wenn vom Administrator Oracle-Clienttools installiert wurden. Die Clientanwendung muss in der Version 11g oder höher vorliegen. Diese Tools müssen auf dem Berichterstellungsclient zum Ermöglichen der Vorschau und auf dem Berichtsserver zum Anzeigen veröffentlichter Berichte installiert werden.  
 
Um die Oracle-Clienttools zu installieren, gehen Sie folgendermaßen vor:
 
1.  Navigieren Sie zur [Oracle-Downloadwebsite](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html).
2.  Laden Sie das ODAC 12c-Release 4 (12.1.0.2.4) für Windows (64 Bit für einen Server, 32 Bit für die Tools) herunter.
3.  Installieren Sie den Datenanbieter für .NET 4.
  
 Benannte Parameter werden von dieser Erweiterung unterstützt. Oracle Version 11g oder höher unterstützt Parameter mit mehreren Werten. Verwenden Sie bei unbenannten, positionsabhängigen Parametern die OLE DB-Datenverarbeitungserweiterung mit dem Datenanbieter „ [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB-Anbieter für Oracle“. Weitere Informationen zum Konfigurieren von Oracle als Datenquelle finden Sie unter [Verwenden von Reporting Services zum Konfigurieren und Zugreifen auf eine Oracle-Datenquelle](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/834305). Weitere Informationen zur Konfiguration zusätzlicher Berechtigungen finden Sie in der [Knowledge Base unter](https://mskb.pkisolutions.com/kb/870668) Hinzufügen von Berechtigunen für den NETWORK SERVICE-Sicherheitsprinzipal [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 Sie können von gespeicherten Prozeduren mit mehreren Eingabeparametern Daten abrufen, die gespeicherte Prozedur darf jedoch nur einen Ausgabecursor zurückgeben. Weitere Informationen finden Sie im Abschnitt [Zurückgeben von Ergebnissen mit Oracle-CURSORs vom Typ REF](/dotnet/framework/data/adonet/retrieving-data-using-a-datareader#returning-results-with-oracle-ref-cursors) des Artikels „Abrufen von Daten mithilfe eines DataReader“.
  
 Weitere Informationen finden Sie unter [Oracle-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/oracle-connection-type-ssrs.md). Weitere Informationen zum zugeordneten Abfrage-Designer finden Sie unter [Grafische Benutzeroberfläche des Abfrage-Designers](../../reporting-services/report-data/graphical-query-designer-user-interface.md).  
  
 Sie können auch auf einer Oracle-Datenbank basierende Modelle erstellen.  
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
###  <a name="teradata-data-processing-extension"></a><a name="Teradata"></a> Teradata-Datenverarbeitungserweiterung  
 Wenn Sie den Datenquellentyp **Teradata** auswählen, müssen Sie eine [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Datenverarbeitungserweiterung auswählen, mit der der .NET Framework-Datenanbieter für Teradata erweitert wird. Berichtsdaten können nur von Teradata abgerufen werden, wenn vom Systemadministrator der .NET Framework-Datenanbieter für Teradata auf dem Berichterstellungsclient und auf dem Berichtsserver installiert wurde, sodass auf dem Client Berichte bearbeitet und als Vorschau angezeigt und auf dem Server veröffentlichte Berichte angezeigt werden können.  
  
 Bei Berichtsserverprojekten ist kein grafischer Abfrage-Designer für diese Erweiterung verfügbar. Sie müssen den textbasierten Abfrage-Designer verwenden, um Abfragen zu erstellen.  
  
 In der folgenden Tabelle sind die Versionen des .NET-Datenanbieters für Teradata angegeben, die zum Definieren einer Datenquelle in einer Berichtsdefinition in [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)]unterstützt werden:  
  
|[!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)]-Version|Teradata-Version|.NET Framework-Datenanbieter für Teradata-Version|  
|-----------------------------------|-------------------------------|-------------------------------------------------------|    
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|12.00|12.00.01|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|6.20|12.00.01|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|13.00|13.0.0.1|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|12.00|12.00.01|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|6.20|12.00.01|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|13.00|13.0.0.1|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|6.20|12.00.01|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|12.00|12.00.01|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|13.00|13.0.0.1|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|14.00|14.00.01| 
|SQL Server 2016|13.00|13.0.0.1|  
|SQL Server 2016|14.00|14.00.01|
|SQL Server 2016|15.00|15.00.01| 
  
 Mehrwertige Parameter werden von dieser Erweiterung unterstützt. Makros können in einer Abfrage mit dem Befehl EXECUTE im Abfragemodus TEXT angegeben werden.  
  
 Weitere Informationen finden Sie unter [Teradataverbindungstyp (SSRS)](../../reporting-services/report-data/teradata-connection-type-ssrs.md).  
 
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
###  <a name="sharepoint-list-data-extension"></a><a name="SharePointList"></a> SharePoint-Listendatenerweiterung  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] umfasst die [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Erweiterung für SharePoint-Listendaten, die es Ihnen ermöglicht, SharePoint-Listen in einem Bericht als Datenquelle zu verwenden. Sie können Listendaten aus folgenden Quellen abrufen:  
  
-   SharePoint Server 2016  

-   [!INCLUDE[SPS2013](../../includes/sps2013-md.md)]  
  
 Es gibt drei Implementierungen des SharePoint-Listendatenanbieters.  
  
1.  Bei einer Berichterstellungsumgebung, wie dem Berichts-Generator oder Berichts-Designer in [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)], oder bei einem Berichtsserver, der im einheitlichen Modus konfiguriert ist, stammen die Listendaten aus dem Webdienst Lists.asmx für die SharePoint-Website.  
  
2.  Auf einem Berichtsserver, der im integrierten SharePoint-Modus konfiguriert ist, stammen die Listendaten entweder aus dem entsprechenden Webdienst Lists.asmx oder aus programmgesteuerten Aufrufen der SharePoint-API. In diesem Modus können Listendaten von einer SharePoint-Farm abgerufen werden.  
  
3.  In [!INCLUDE[SPS2013](../../includes/sps2013-md.md)] und SharePoint Server 2016 ermöglicht das [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Add-In für [!INCLUDE[msCoName](../../includes/msconame-md.md)]-SharePoint-Technologien das Abrufen von Listendaten aus einem Lists.asmx-Webdienst für eine SharePoint-Website oder von einer SharePoint-Website, die Teil einer SharePoint-Farm ist. Dieses Szenario wird auch als *lokaler Modus* bezeichnet, da kein Berichtsserver erforderlich ist.  
  
 Die Anmeldeinformationen, die Sie angeben können, hängen von der Implementierung ab, die die Clientanwendung verwendet. Weitere Informationen finden Sie unter [SharePoint-Listenverbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/sharepoint-list-connection-type-ssrs.md).  
  
###  <a name="xml-data-processing-extension"></a><a name="XML"></a> XML-Datenverarbeitungserweiterung  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] besitzt eine XML-Datenverarbeitungserweiterung, sodass die Verwendung von XML-Daten in einem Bericht möglich ist. Die Daten können von einem XML-Dokument, einem Webdienst oder einer webbasierten Anwendung abgerufen werden, auf die mit einer URL zugegriffen wird. Weitere Informationen finden Sie unter [XML-Verbindungstyp (SSRS)](../../reporting-services/report-data/xml-connection-type-ssrs.md). Weitere Informationen zum zugeordneten Abfrage-Designer finden Sie im Abschnitt zum textbasierten Abfrage-Designer unter [Grafische Benutzeroberfläche des Abfrage-Designers](../../reporting-services/report-data/graphical-query-designer-user-interface.md).
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
###  <a name="sap-bw-data-processing-extension"></a><a name="SAPBW"></a> SAP BW-Datenverarbeitungserweiterung  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] enthält eine Datenverarbeitungserweiterung, die die Verwendung von Daten aus einer SAP BW-Datenquelle in Berichten ermöglicht.
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
###  <a name="hyperion-essbase-business-intelligence-data-processing-extension"></a><a name="Hyperion"></a> Hyperion Essbase Business Intelligence-Datenverarbeitungserweiterung  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] enthält eine Datenverarbeitungserweiterung, die die Verwendung von Daten aus einer [!INCLUDE[extEssbase](../../includes/extessbase-md.md)] -Datenquelle in Berichten ermöglicht.  
  
 Weitere Informationen finden Sie unter [Hyperion Essbase-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/hyperion-essbase-connection-type-ssrs.md). Weitere Informationen zum zugeordneten Abfrage-Designer finden Sie unter [Hyperion Essbase Query Designer User Interface](../../reporting-services/report-data/hyperion-essbase-query-designer-user-interface.md).  
  
 Weitere Informationen zu [!INCLUDE[extEssbase](../../includes/extessbase-md.md)] finden Sie unter [Verwenden von SQL Server Reporting Services mit Hyperion Essbase](../../reporting-services/report-data/hyperion-essbase-query-designer-user-interface.md). 
  
 [Zurück zur Datenquellentabelle](#DataSourcesTable)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)   
 [Berichtsdatasets &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)  
Haben Sie dazu Fragen? [Besuchen Sie das Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
  
