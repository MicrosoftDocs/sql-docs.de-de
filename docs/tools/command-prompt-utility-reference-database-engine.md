---
title: Hilfsprogramme für die SQL-Eingabeaufforderung (Datenbank-Engine)
description: Eingabeaufforderungs-Hilfsprogramme ermöglichen Ihnen, SQL Server-Vorgänge einem Skript hinzuzufügen. In diesem Artikel werden viele der Eingabeaufforderungs-Hilfsprogramme aufgelistet, die im Lieferumfang von SQL Server enthalten sind.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server]
- command prompt utilities [SQL Server], about command prompt utilities
- command prompt [SQL Server]
- utilities [SQL Server], command prompt
- command prompt [SQL Server], utilities
ms.assetid: 48364bd9-6ea7-45e9-a332-acf3d81bbfae
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: e948ad2c4d9e08d3fb4c5da91f7bfd76a563c64a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349964"
---
# <a name="sql-command-prompt-utilities-database-engine"></a>Hilfsprogramme für die SQL-Eingabeaufforderung (Datenbank-Engine)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Eingabeaufforderungs-Hilfsprogramme versetzen Sie in die Lage, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Vorgänge einem Skript hinzuzufügen. Die folgende Tabelle enthält eine Liste vieler Eingabeaufforderung-Hilfsprogramme, die im Lieferumfang von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] enthalten sind.

Weitere Informationen zur *Haupt*-GUI für SQL und den Befehlszeilentools finden Sie in der [Übersicht zu den SQL-Tools](overview-sql-tools.md).

|**Hilfsprogramm**|**Beschreibung**|**Installiert in**|  
|-----------------|---------------------|----------------------|  
|[bcp (Hilfsprogramm)](../tools/bcp-utility.md)|Wird verwendet, um Daten in einem benutzerdefinierten Format von einer [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Instanz in eine Datendatei zu kopieren|\<*drive*:>\Programme\\[!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]\Client SDK\ODBC\110\Tools\Binn|  
|[dta (Hilfsprogramm)](../tools/dta/dta-utility.md)|Wird verwendet, um eine Arbeitsauslastung zu analysieren und Empfehlungen zu physischen Entwurfsstrukturen auszugeben, um die Serverleistung für diese Arbeitsauslastung zu optimieren.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[dtexec (Hilfsprogramm)](../integration-services/packages/dtexec-utility.md)|Wird verwendet, um ein [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] -Paket zu konfigurieren und auszuführen. Unter dem Namen **DTExecUI** steht eine Version dieses Hilfsprogramms mit Benutzeroberfläche zur Verfügung, über die das Paketausführungsprogramm aufgerufen wird.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[dtutil (Hilfsprogramm)](../integration-services/dtutil-utility.md)|Wird für die Verwaltung von SSIS-Paketen verwendet.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[Bereitstellen von Modelllösungen mit dem Bereitstellungsprogramm](/analysis-services/multidimensional-models/deploy-model-solutions-with-the-deployment-utility)|Wird verwendet, um [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] -Projekte für [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]-Instanzen bereitzustellen.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn\VShell\Common7\IDE|   
|[osql (Hilfsprogramm)](../tools/osql-utility.md)|Ermöglicht es Ihnen, [!INCLUDE[tsql](../includes/tsql-md.md)] -Anweisungen, Systemprozeduren und Skriptdateien an der Eingabeaufforderung einzugeben.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Profiler-Hilfsprogramm](../tools/profiler-utility.md)|Wird verwendet, um [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] von der Eingabeaufforderung zu starten.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Hilfsprogramm RS.exe (SSRS)](../reporting-services/tools/rs-exe-utility-ssrs.md)|Wird verwendet, um Skripts für die Verwaltung von [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] -Berichtsservern auszuführen.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[rsconfig-Hilfsprogramm (SSRS)](../reporting-services/tools/rsconfig-utility-ssrs.md)|Wird zum Konfigurieren einer Berichtsserververbindung verwendet.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[rskeymgmt-Hilfsprogramm &#40;SSRS&#41;](../reporting-services/tools/rskeymgmt-utility-ssrs.md)|Wird für die Verwaltung von Verschlüsselungsschlüsseln auf einem Berichtsserver verwendet.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[sqlagent90 (Anwendung)](../tools/sqlagent90-application.md)|Wird verwendet, um den [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Agent von der Eingabeaufforderung zu starten.|\<drive>:\Programme\Microsoft SQL Server\\<*Instanzname*>\MSSQL\Binn|  
|[SQLCMD-Hilfsprogramm](../tools/sqlcmd-utility.md)|Ermöglicht es Ihnen, [!INCLUDE[tsql](../includes/tsql-md.md)] -Anweisungen, Systemprozeduren und Skriptdateien an der Eingabeaufforderung einzugeben.|\<*drive*:>\Programme\\[!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]\Client SDK\ODBC\110\Tools\Binn|  
|[SQLdiag (Hilfsprogramm)](../tools/sqldiag-utility.md)|Wird zum Sammeln von Diagnoseinformationen für den [!INCLUDE[msCoName](../includes/msconame-md.md)] -Kundenservice und -support verwendet.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Anwendung sqllogship](../tools/sqllogship-application.md)|Wird von Anwendungen zum Ausführen von Sicherungs-, Kopier- und Wiederherstellungsvorgängen und zugeordneten Cleanups für eine Protokollversandkonfiguration verwendet, ohne Sicherungs-, Kopier- und Wiederherstellungsaufträge auszuführen.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[SqlLocalDB-Hilfsprogramm](../tools/sqllocaldb-utility.md)|Ein Ausführungsmodus von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , der speziell für Programmentwickler konzipiert wurde.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[sqlmaint (Hilfsprogramm)](../tools/sqlmaint-utility.md)|Wird verwendet, um Datenbank-Wartungspläne auszuführen, die in früheren Versionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]erstellt wurden.|\<drive>:\Programme\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn|  
|[sqlps (Hilfsprogramm)](../tools/sqlps-utility.md)|Wird zum Ausführen von PowerShell-Befehlen und -Skripts verwendet. Lädt und registriert den [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell-Anbieter sowie cmdlets.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[sqlservr](../tools/sqlservr-application.md)|Wird verwendet, um eine Instanz von [!INCLUDE[ssDE](../includes/ssde-md.md)] zur Problembehandlung von der Eingabeaufforderung aus zu starten und zu beenden.|\<drive>:\Programme\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn|  
|[Ssms-Hilfsprogramm](../ssms/ssms-utility.md)|Wird verwendet, um [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] von der Eingabeaufforderung zu starten.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn\VSShell\Common7\IDE|  
|[tablediff (Hilfsprogramm)](../tools/tablediff-utility.md)|Wird verwendet, um Daten in zwei Tabellen im Hinblick auf Nichtkonvergenz zu vergleichen, was im Rahmen der Problembehandlung in einer Replikationstopologie hilfreich sein kann.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]COM|  

## <a name="command-prompt-utilities-syntax-conventions"></a>Syntaxkonventionen für die Befehlszeilen-Hilfsprogramme  
  
|**Konvention**|**Verwendung**|  
|--------------------|------------------|  
|GROSSBUCHSTABEN|Anweisungen und Ausdrücke, die auf Betriebssystemebene verwendet werden.|  
|`monospace`|Beispielbefehle und Programmcode.|  
|*Kursiv*|Vom Benutzer angegebene Parameter.|  
|**Fett**|Befehle, Parameter und sonstige Syntaxelemente, die genau wie angezeigt eingegeben werden müssen.|  

## <a name="see-also"></a>Weitere Informationen

* [Replication Distribution Agent](../relational-databases/replication/agents/replication-distribution-agent.md)
* [Replikationsprotokolllese-Agent](../relational-databases/replication/agents/replication-log-reader-agent.md)
* [Replication Merge Agent](../relational-databases/replication/agents/replication-merge-agent.md)
* [Replication Queue Reader Agent](../relational-databases/replication/agents/replication-queue-reader-agent.md)
* [Replication Snapshot Agent](../relational-databases/replication/agents/replication-snapshot-agent.md)