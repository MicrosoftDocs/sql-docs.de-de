---
description: Verwalten von Auftragsschritten
title: Verwalten von Auftragsschritten
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- job steps [SQL Server replication]
- job steps [SQL Server Agent]
- jobs [SQL Server Agent], Integration Services package step
- executable programs as job steps
- operating systems [SQL Server], job steps
- Transact-SQL job step
- job steps [Transact-SQL]
- Integration Services packages, job steps
- replication job steps [SQL Server]
- logs [SQL Server], jobs
- SQL Server Agent jobs, job steps
- ActiveX scripting jobs [SQL Server]
- job steps [Analysis Services]
ms.assetid: 51352afc-a0a4-428b-8985-f9e58bb57c31
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 8892ff46f9c6c9fd382b39284cbc4bc81449a5d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355178"
---
# <a name="manage-job-steps"></a>Verwalten von Auftragsschritten
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Ein Auftragsschritt ist eine Aktion, die der Auftrag auf einer Datenbank oder einem Server ausführt. Jeder Auftrag muss mindestens einen Auftragsschritt aufweisen. Folgende Auftragsschritte sind möglich:  
  
-   Ausführbare Programme und Betriebssystembefehle.  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] Anweisungen, einschließlich gespeicherter Prozeduren und erweiterter gespeicherter Prozeduren.  
  
-   PowerShell-Skripts.  
  
-   [!INCLUDE[msCoName](../../includes/msconame_md.md)] ActiveX-Skripts.  
  
-   Replikationstasks.  
  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] Aufgaben.  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Pakete.  
  
Jeder Auftragsschritt wird in einem bestimmten Sicherheitskontext ausgeführt. Wenn der Auftragsschritt einen Proxy erfordert, wird er im Sicherheitskontext der Anmeldeinformationen des Proxys ausgeführt. Wenn ein Auftragsschritt keinen Proxy erfordert, wird er im Kontext des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Dienstkontos ausgeführt. Nur Mitglieder der festen Serverrolle sysadmin können Aufträge erstellen, die nicht ausdrücklich einen Proxy angeben.  
  
Weil Auftragsschritte im Kontext eines bestimmten [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows-Benutzers ausgeführt werden, muss dieser Benutzer über die Berechtigungen und Konfigurationen verfügen, die für die Ausführung des Auftragsschritts erforderlich sind. Wenn Sie beispielsweise einen Auftrag erstellen, der einen Laufwerkbuchstaben oder einen UNC-Pfad (Universal Naming Convention) erfordert, können die Auftragsschritte unter Ihrem Windows-Benutzerkonto ausgeführt werden, während die Tasks getestet werden. Allerdings muss der Windows-Benutzer für den Auftragsschritt auch über die notwendigen Berechtigungen, die Laufwerkbuchstabenkonfigurationen oder den Zugriff auf das erforderliche Laufwerk verfügen. Andernfalls erzeugt der Auftragsschritt einen Fehler. Um dieses Problem zu verhindern, stellen Sie sicher, dass der Proxy für jeden Auftragsschritt über die notwendigen Berechtigungen für den Task verfügt, von dem der Auftragsschritt ausgeführt wird. Weitere Informationen finden Sie unter [Sicherheit und Schutz (Datenbank-Engine)](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).  
  
## <a name="job-step-logs"></a>Auftragsschrittprotokolle  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent kann die Ausgabe bestimmter Auftragsschritte entweder in eine Betriebssystemdatei oder in die sysjobstepslogs-Tabelle der msdb-Datenbank schreiben. Von den folgenden Auftragsschritttypen können Ausgaben in beide Ziele geschrieben werden:  
  
-   Ausführbare Programme und Betriebssystembefehle.  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen.  
  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] Aufgaben.  
  
Nur von Auftragsschritten, die von Benutzern ausgeführt werden, die Mitglieder der festen Serverrolle sysadmin sind, können Auftragsschrittausgaben in Betriebssystemdateien geschrieben werden. Wenn die Auftragsschritte von Benutzern ausgeführt werden, die Mitglieder der festen Datenbankrolle SQLAgentUserRole, SQLAgentReaderRole oder SQLAgentOperatorRole der msdb-Datenbank sind, kann die Ausgabe dieser Auftragsschritte nur in die sysjobstepslogs-Tabelle geschrieben werden.  
  
Auftragsschritte werden automatisch gelöscht, wenn Aufträge oder Auftragsschritte gelöscht werden.  
  
> [!NOTE]  
> Das Protokollieren von Replikationstasks und [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paketauftragsschritten wird vom jeweiligen Subsystem durchgeführt. Sie können zum Konfigurieren der Auftragsschrittprotokollierung nicht den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent für diese Art von Auftragsschritten verwenden.  
  
## <a name="executable-programs-and-operating-system-commands-as-job-steps"></a>Ausführbare Programme und Betriebssystembefehle als Auftragsschritte  
Ausführbare Programme und Betriebssystembefehle können als Auftragsschritte verwendet werden. Diese Dateien können die Dateierweiterungen BAT, CMD, COM oder EXE aufweisen.  
  
Wenn Sie ein ausführbares Programm oder einen Betriebssystembefehl als Auftragsschritt verwenden, müssen Sie Folgendes angeben:  
  
-   Den Prozessexitcode, der zurückgegeben wird, wenn der Befehl erfolgreich ausgeführt wurde.  
  
-   Den auszuführenden Befehl. Beim Ausführen eines Betriebssystembefehls handelt es sich hierbei einfach um den Befehl selbst. Bei einem externen Programm ist dies der Name des Programms und die Argumente für das Programm, z. B.: **C:\Programme\Microsoft SQL Server\100\Tools\Binn\sqlcmd.exe -e -q "sp_who"**.  
  
    > [!NOTE]  
    > Sie müssen den vollständigen Pfad zur ausführbaren Datei angeben, wenn diese sich nicht in dem Verzeichnis befindet, das im Systempfad oder dem Pfad für den Benutzer angegeben ist, als der der Auftragsschritt ausgeführt wird.  
  
## <a name="transact-sql-job-steps"></a>Transact-SQL-Auftragsschritte  
Beim Erstellen eines [!INCLUDE[tsql](../../includes/tsql-md.md)] -Auftragsschritts müssen Sie folgende Schritte durchführen:  
  
-   Identifizieren der Datenbank, in der Sie den Auftrag ausführen.  
  
-   Eingeben der auszuführenden [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung. Von der Anweisung wird möglicherweise eine gespeicherte Prozedur oder eine erweiterte gespeicherte Prozedur aufgerufen.  
  
Sie können auch eine vorhandene [!INCLUDE[tsql](../../includes/tsql-md.md)] -Datei als Befehl für den Auftragsschritt öffnen.  
  
[!INCLUDE[tsql](../../includes/tsql-md.md)] Auftragsschritte verwenden keine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Proxys. Stattdessen wird der Auftragsschritt als Besitzer des Auftragsschritts bzw. als [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Dienstkonto ausgeführt, wenn der Besitzer des Auftragsschritts Mitglied der festen Serverrolle sysadmin ist. Mitglieder der festen Serverrolle „sysadmin“ können auch angeben, dass [!INCLUDE[tsql](../../includes/tsql-md.md)] -Auftragsschritte unter dem Kontext eines anderen Benutzers ausgeführt werden, indem der *database_user_name* -Parameter der gespeicherten Prozedur „sp_add_jobstep“ verwendet wird. Weitere Informationen finden Sie unter [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md).  
  
> [!NOTE]  
> Ein einzelner [!INCLUDE[tsql](../../includes/tsql-md.md)] -Auftragsschritt kann mehrere Batches enthalten. [!INCLUDE[tsql](../../includes/tsql-md.md)] Auftragsschritte können mehrere eingebettete GO-Befehle enthalten.  
  
## <a name="powershell-scripting-job-steps"></a>PowerShell-Skript-Auftragsschritte  
Wenn Sie einen PowerShell-Skript-Auftragsschritt erstellen, müssen Sie eins von zwei Elementen als Befehl für den Schritt angeben:  
  
-   Den Text eines PowerShell-Skripts.  
  
-   Eine vorhandene PowerShell-Skriptdatei, die geöffnet werden soll.  
  
Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent PowerShell-Subsystem öffnet eine PowerShell-Sitzung und lädt die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell-Snap-Ins. Das als Auftragsschrittbefehl verwendete PowerShell-Skript kann auf den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell-Anbieter und auf Cmdlets verweisen. Weitere Informationen über das Schreiben von PowerShell-Skripts mithilfe von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell-Snap-Ins finden Sie unter [SQL Server PowerShell](../../powershell/sql-server-powershell.md).  
  
## <a name="activex-scripting-job-steps"></a>ActiveX-Skript-Auftragsschritte  
  
> [!IMPORTANT]
> Der ActiveX-Skript-Auftragsschritt wird in einer zukünftigen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus dem [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden.  
  
Wenn Sie einen ActiveX-Skriptauftragsschritt erstellen, ist Folgendes notwendig:  
  
-   Identifizieren der Skriptsprache, in der der Auftragsschritt geschrieben ist.  
  
-   Erstellen des ActiveX-Skripts.  
  
Sie können auch eine vorhandene ActiveX-Skriptdatei als Befehl für den Auftragsschritt öffnen. ActiveX-Skriptbefehle können alternativ auch extern (beispielsweise mithilfe von Microsoft Visual Basic) kompiliert und anschließend als ausführbare Programme ausgeführt werden.  
  
Handelt es sich bei dem Befehl eines Auftragsschritts um ein ActiveX-Skript, können Sie das SQLActiveScriptHost-Objekt verwenden, um Ausgaben in das Verlaufsprotokoll des Auftragsschritts zu drucken oder COM-Objekte zu erstellen. Bei SQLActiveScriptHost handelt es sich um ein globales Objekt, das vom Hostsystem des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents in den Skriptnamensbereich eingefügt wird. Das Objekt verfügt über zwei Methoden („Print“ und „CreateObject“). Das folgende Beispiel veranschaulicht, wie ActiveX-Skripts in Visual Basic Scripting Edition (VBScript) funktionieren.  
  
```  
' VBScript example for ActiveX Scripting job step  
' Create a Dmo.Server object. The object connects to the  
' server on which the script is running.  
  
Set oServer = CreateObject("SQLDmo.SqlServer")  
oServer.LoginSecure = True  
oServer.Connect "(local)"  
'Disconnect and destroy the server object  
oServer.DisConnect  
Set oServer = nothing  
```  
  
## <a name="replication-job-steps"></a>Replikationsauftragsschritte  
Wenn Sie Veröffentlichungen und Abonnements mithilfe der Replikation erstellen, werden standardmäßig Replikationsaufträge erstellt. Der Typ der erstellten Aufträge wird durch den Typ der Replikation (Momentaufnahme, Transaktion oder Merge) und die verwendeten Optionen bestimmt.  
  
Replikationsauftragsschritte aktivieren einen dieser Replikations-Agents:  
  
-   Momentaufnahme-Agent (Momentaufnahmeauftrag)  
  
-   Protokolllese-Agent (Protokollleserauftrag)  
  
-   Verteilungs-Agent (Verteilungsauftrag)  
  
-   Merge-Agent (Mergeauftrag)  
  
-   Warteschlangenlese-Agent (Warteschlangenleser-Auftrag)  
  
Beim Einrichten der Replikation können Sie zwischen drei Ausführungsarten für die Replikations-Agents wählen: immer nach dem Start des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents, bei Bedarf oder gemäß einem Zeitplan. Weitere Informationen zu Replikations-Agents finden Sie unter [Replikations-Agents (Übersicht)](../../relational-databases/replication/agents/replication-agents-overview.md).  
  
## <a name="analysis-services-job-steps"></a>Analysis Services-Auftragsschritte  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent unterstützt zwei unterschiedliche Typen von Analysis Services-Auftragsschritten: Befehlsauftragsschritte und Abfrageauftragsschritte.  
  
### <a name="analysis-services-command-job-steps"></a>Analysis Services-Befehlsauftragsschritte  
Wenn Sie einen [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] -Befehlsauftragsschritt erstellen, müssen Sie folgende Schritte durchführen:  
  
-   Identifizieren der OLAP-Datenbank, in der Sie den Auftragsschritt ausführen.  
  
-   Eingeben der auszuführenden Anweisung. Bei der Anweisung muss es sich um XML-Code für die  **Execute**-Methode in [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] handeln. Die Anweisung darf keinen vollständigen SOAP-Umschlag oder XML-Code für eine  **Discover**-Methode für [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] enthalten. Hinweis: Während [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] vollständige SOAP-Umschläge und die **Discover** -Methode unterstützt, ist das bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Auftragsschritten nicht der Fall.  
  
### <a name="analysis-services-query-job-steps"></a>Analysis Services-Abfrageauftragsschritte  
Wenn Sie einen [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] -Abfrageauftragsschritt erstellen, müssen Sie folgende Schritte durchführen:  
  
-   Identifizieren der OLAP-Datenbank, in der Sie den Auftragsschritt ausführen.  
  
-   Eingeben der auszuführenden Anweisung. Bei der Anweisung muss es sich um eine MDX-Abfrage handeln (Multidimensional Expressions, mehrdimensionale Ausdrücke).  
  
Weitere Informationen zu MDX finden Sie unter [Grundlegendes zur MDX-Anweisung (MDX)](/analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services?viewFallbackFrom=sql-server-ver15).  
  
## <a name="integration-services-packages"></a>Integration Services-Pakete  
Wenn Sie einen [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paketauftragsschritt erstellen, müssen Sie folgende Schritte durchführen:  
  
-   Identifizieren der Paketquelle.  
  
-   Identifizieren des Paketspeicherorts.  
  
-   Identifizieren der Konfigurationsdateien, wenn diese für das Paket erforderlich sind.  
  
-   Identifizieren der Befehlsdateien, wenn diese für das Paket erforderlich sind.  
  
-   Identifizieren der für das Paket zu verwendenden Überprüfung. Sie können beispielsweise angeben, dass das Paket signiert werden oder eine bestimmte Paket-ID aufweisen muss.  
  
-   Identifizieren der Datenquellen für das Paket.  
  
-   Identifizieren der Protokollanbieter für das Paket.  
  
-   Angeben von Variablen und Werten, die vor dem Ausführen des Pakets festgelegt werden.  
  
-   Identifizieren von Ausführungsoptionen.  
  
-   Hinzufügen oder Ändern von Befehlszeilenoptionen.  
  
Hinweis: Wenn Sie das Paket im SSIS-Katalog bereitgestellt und Sie **SSIS-Katalog** als Paketquelle angegeben haben, werden viele dieser Konfigurationsinformationen automatisch vom Paket abgerufen. Auf der Registerkarte **Konfiguration** können Sie die Umgebung, Parameterwerte, Verbindungs-Manager-Werte und Eigenschaftenüberschreibungen angeben und ob das Paket in einer 32-Bit-Laufzeitumgebung ausgeführt wird.  
  
Informationen zum Erstellen von Auftragsschritten, von denen [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Pakete ausgeführt werden, finden Sie unter [Aufträge des SQL Server-Agents für Pakete](../../integration-services/packages/sql-server-agent-jobs-for-packages.md).  
  
## <a name="related-tasks"></a>Related Tasks  
  
|BESCHREIBUNG|Thema|  
|-|-|  
|Beschreibt, wie ein Auftragsschritt mit einem ausführbaren Programm erstellt wird.|[Erstellen eines CmdExec-Auftragsschritts](../../ssms/agent/create-a-cmdexec-job-step.md)|  
|Beschreibt, wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Berechtigungen zurückgesetzt werden.|[Konfigurieren eines Benutzers zum Erstellen und Verwalten von SQL Server-Agent-Aufträgen](../../ssms/agent/configure-a-user-to-create-and-manage-sql-server-agent-jobs.md)|  
|Beschreibt, wie ein [!INCLUDE[tsql](../../includes/tsql-md.md)] -Auftragsschritt erstellt wird.|[Erstellen eines Transact-SQL-Auftragsschritts](../../ssms/agent/create-a-transact-sql-job-step.md)|  
|Beschreibt, wie Optionen für Transact-SQL-Auftragsschritte für den Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent erstellt werden.|[Definieren von Optionen für Transact-SQL-Auftragsschritte](../../ssms/agent/define-transact-sql-job-step-options.md)|  
|Beschreibt, wie ein ActiveX-Skript-Auftragsschritt erstellt wird.|[Create an ActiveX Script Job Step](../../ssms/agent/create-an-activex-script-job-step.md)|  
|Beschreibt, wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Auftragsschritte erstellt und definiert werden, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Analysis Services-Befehle und -Abfragen ausführen.|[Erstellen eines Analysis Services-Auftragsschritts](../../ssms/agent/create-an-analysis-services-job-step.md)|  
|Beschreibt, welche Aktion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausführen sollte, wenn während der Auftragsausführung ein Fehler auftritt.|[Set Job Step Success or Failure Flow](../../ssms/agent/set-job-step-success-or-failure-flow.md)|  
|Beschreibt, wie Auftragsschrittdetails im Dialogfeld Auftragsschritt-Eigenschaften angezeigt werden.|[View Job Step Information](../../ssms/agent/view-job-step-information.md)|  
|Beschreibt, wie ein Auftragsschrittprotokoll des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents gelöscht wird.|[Löschen eines Auftragsschrittprotokolls](../../ssms/agent/delete-a-job-step-log.md)|  
  
## <a name="see-also"></a>Weitere Informationen  
[sysjobstepslogs (Transact-SQL)](../../relational-databases/system-tables/dbo-sysjobstepslogs-transact-sql.md)  
[Erstellen von Aufträgen](../../ssms/agent/create-jobs.md)  
[sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
