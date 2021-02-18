---
description: SQL Server-Agent
title: SQL Server-Agent
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, about SQL Server Agent
- automatic administration steps
ms.assetid: 8d1dc600-aabb-416f-b3af-fbc9fccfd0ec
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 543491e19d1182d611508797bab2354d483302af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344257"
---
# <a name="sql-server-agent"></a>SQL Server-Agent

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent ist ein Microsoft Windows-Dienst, der geplante administrative Tasks ausführt, die in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] als *Jobs* bezeichnet werden.  

## <a name="benefits-of-sql-server-agent"></a><a name="Benefits"></a>Vorteile des SQL Server-Agents 

Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent verwendet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zum Speichern von Auftragsinformationen. Aufträge enthalten mindestens einen Auftragsschritt. Jeder Schritt umfasst einen eigenen Task, z.B. das Sichern einer Datenbank.  
  
Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent kann einen Auftrag anhand eines Zeitplans, als Reaktion auf ein bestimmtes Ereignis oder bei Bedarf ausführen. Wenn Sie z.B. am Ende jedes Arbeitstages alle Server des Unternehmens sichern möchten, können Sie diesen Task automatisieren. Planen Sie die Sicherung so, dass sie montags bis freitags nach 22:00 Uhr ausgeführt wird. Falls bei der Sicherung ein Problem auftritt, kann der SQL Server-Agent das Ereignis aufzeichnen und Sie benachrichtigen.  
  
> [!NOTE]  
> Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Dienst ist standardmäßig deaktiviert, wenn [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] installiert ist, sofern der Benutzer nicht explizit festgelegt hat, dass der Dienst automatisch gestartet werden soll.  
  
## <a name="sql-server-agent-components"></a><a name="Components"></a>SQL Server Agent Components  
Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent verwendet die folgenden Komponenten, um die auszuführenden Aufgaben, den Zeitpunkt der Ausführung und die Meldung erfolgreicher bzw. fehlgeschlagener Aufgaben zu definieren.  
  
### <a name="jobs"></a>Aufträge  
Ein *Auftrag* umfasst eine angegebene Reihe von Aktionen, die der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent ausführt. Verwenden Sie Aufträge, um eine Verwaltungsaufgabe zu definieren, die einmalig oder mehrmals ausgeführt und deren Erfolgsstatus überwacht werden kann. Ein Auftrag kann auf einem einzelnen lokalen Server oder auf mehreren Remoteservern ausgeführt werden.  
  
> [!IMPORTANT]  
> Aufträge des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agents, die zum Zeitpunkt eines Failoverereignisses auf einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failoverclusterinstanz ausgeführt werden, werden nach dem Failover nicht auf einem anderen Failoverclusterknoten fortgesetzt. Aufträge des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agents, die ausgeführt werden, wenn ein Hyper-V-Knoten angehalten wird, werden nicht fortgesetzt, wenn die Pause ein Failover zu einem anderen Knoten verursacht. Aufträge, die begonnen, aber wegen eines Failoverereignisses nicht abgeschlossen werden, werden als gestartet protokolliert, jedoch werden keine weiteren Protokolleinträge für Abschluss oder Fehler erstellt. Unter diesen Umständen werden die betreffenden Aufträge des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agents scheinbar nie beendet.  
  
Für die Ausführung von Aufträgen gibt es mehrere Möglichkeiten:  
  
-   Ausführung nach einem oder mehreren Zeitplänen.  
  
-   Ausführung als Reaktion auf eine oder mehrere Warnungen.  
  
-   Ausführung im Rahmen der gespeicherten Prozedur sp_start_job.  
  
Die einzelnen Aktionen im Rahmen eines Auftrags werden als *Auftragsschritte* bezeichnet. Ein Auftragsschritt kann beispielsweise in der Ausführung einer [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung, der Ausführung eines [!INCLUDE[ssIS](../../includes/ssis_md.md)]-Pakets oder der Ausgabe eines Befehls an einen Analysis Services-Server bestehen. Auftragsschritte werden als Teil des Auftrags verwaltet.  
  
Jeder Auftragsschritt wird in einem bestimmten Sicherheitskontext ausgeführt. Bei Auftragsschritten, die [!INCLUDE[tsql](../../includes/tsql-md.md)] verwenden, nutzen Sie zum Festlegen des Sicherheitskontexts für den Auftragsschritt die EXECUTE AS-Anweisung. Bei anderen Arten von Auftragsschritten verwenden Sie ein Proxykonto, um den Sicherheitskontext für den Auftragsschritt festzulegen.  
  
### <a name="schedules"></a>Zeitpläne  
Durch einen *Zeitplan* wird angegeben, wann ein Auftrag ausgeführt wird. Mehrere Aufträge können auf dem gleichen Zeitplan basieren, und für einen Auftrag können mehrere Zeitpläne gelten. Ein Zeitplan kann folgende Bedingungen für die Ausführungszeit eines Auftrags definieren:  
  
-   Ausführung beim Start des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agents.  
  
-   Ausführung, wenn sich die CPU-Auslastung des Computers in einem Bereich befindet, den Sie als Leerlauf definiert haben.  
  
-   Einmalige Ausführung zu einem bestimmten Zeitpunkt.  
  
-   Auf der Grundlage einer Zeitplanserie.  
  
Weitere Informationen finden Sie unter [Anlegen und Zuweisen von Zeitplänen zu Aufträgen](../../ssms/agent/create-and-attach-schedules-to-jobs.md).  
  
### <a name="alerts"></a>Alerts  
Eine *Warnung* ist eine automatische Reaktion auf ein bestimmtes Ereignis. Bei einem Ereignis kann es sich z.B. um das Starten eines Auftrags oder das Erreichen eines bestimmten Schwellenwerts bei den Systemressourcen handeln. Sie definieren die Bedingungen, unter denen eine Warnung auftritt.  
  
Eine Warnung kann als Reaktion auf eine der folgenden Bedingungen ausgegeben werden:  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ereignisse  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Leistungsbedingungen  
  
-   Ereignisse in der Microsoft Windows-Verwaltungsinstrumentation (WMI) auf dem Computer, auf dem der SQL Server-Agent ausgeführt wird  
  
Eine Warnung kann die folgenden Aktionen ausführen:  
  
-   Benachrichtigen eines oder mehrerer Operatoren  
  
-   Ausführen eines Auftrags  
  
Weitere Informationen finden Sie unter [Warnungen](../../ssms/agent/alerts.md).  
  
### <a name="operators"></a>Operatoren  
Ein *Operator* definiert die Kontaktinformationen einer Person, die für die Verwaltung einer oder mehrerer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanzen zuständig ist. In einigen Unternehmen werden die Aufgaben eines Operators einer einzelnen Person zugewiesen. In größeren Unternehmen mit mehreren Servern teilen sich mehrere Personen die Aufgaben des Operators. Ein Bediener verfügt über keine Sicherheitsinformationen und definiert keinen Sicherheitsprinzipal.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann Operatoren bei Warnungen folgendermaßen benachrichtigen:  
  
-   E-Mail  
  
-   Pager (per E-Mail)  
  
-   **net send**  
  
> [!NOTE]  
> Um Benachrichtigungen mit **net send** zu senden, muss der Windows Messenger-Dienst auf dem Computer mit dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent gestartet worden sein.  
  
> [!IMPORTANT]  
> Die Pager- und **net send** -Optionen werden in zukünftigen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht mehr im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktionen zurzeit verwenden.  
  
Um Operatoren Benachrichtigungen per E-Mail oder Pager zu senden, müssen Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent so konfigurieren, dass Datenbank-E-Mail verwendet wird. Weitere Informationen finden Sie unter [Datenbank-E-Mail](../../relational-databases/database-mail/database-mail.md).  
  
Ein Operator kann auch als Alias für eine Gruppe von Personen definiert werden. In diesem Fall werden alle Mitglieder dieses Alias zur selben Zeit benachrichtigt. Weitere Informationen finden Sie unter [Operatoren](../../ssms/agent/operators.md).  
  
## <a name="security-for-sql-server-agent-administration"></a><a name="Security"></a>Sicherheit für die Administration mit dem SQL Server-Agent  
Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent verwendet die festen Datenbankrollen **SQLAgentUserRole**, **SQLAgentReaderRole** und **SQLAgentOperatorRole** in der **msdb**-Datenbank, um den Zugriff auf den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent für Benutzer zu steuern, die keine Mitglieder der festen Serverrolle **sysadmin** sind. Neben diesen festen Datenbankrollen können Datenbankadministratoren mithilfe von Subsystemen und Proxys sicherstellen, dass jeder Auftragsschritt mit den mindestens erforderlichen Berechtigungen ausgeführt wird.  
  
### <a name="roles"></a>Rollen  
Mitglieder der festen Datenbankrollen **SQLAgentUserRole**, **SQLAgentReaderRole** und **SQLAgentOperatorRole** in **msdb** und Mitglieder der festen Serverrolle **sysadmin** haben Zugriff auf den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent. Ein Benutzer, der keiner dieser Rollen angehört, kann den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent nicht verwenden. Weitere Informationen zu den Rollen, die vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent verwendet werden, finden Sie unter [Implementieren der SQL Server-Agent-Sicherheit](../../ssms/agent/implement-sql-server-agent-security.md).  
  
### <a name="subsystems"></a>Subsysteme  
Ein Subsystem ist ein vordefiniertes Objekt, das die für einen Auftragsschritt verfügbare Funktionalität darstellt. Jeder Proxy hat Zugriff auf mindestens ein Subsystem. Subsysteme bieten Sicherheit, weil sie den Zugriff auf die für ein Proxykonto verfügbare Funktionalität begrenzen. Jeder Auftragsschritt wird im Kontext eines Proxys ausgeführt. Ausgenommen sind lediglich [!INCLUDE[tsql](../../includes/tsql-md.md)]-Auftragsschritte. Bei [!INCLUDE[tsql](../../includes/tsql-md.md)]-Auftragsschritten wird der Sicherheitskontext mithilfe des EXECUTE AS-Befehls für den Besitzer des Auftrags festgelegt.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definiert die in der folgenden Tabelle aufgeführten Subsysteme:  
  
|Name des Subsystems|BESCHREIBUNG|  
|--------------|-----------|  
|Microsoft ActiveX-Skript|Ausführen eines ActiveX-Skriptauftragsschritts.<br /><br />**Warnung:** Das ActiveX-Skriptsubsystem wird in einer zukünftigen Version von [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent entfernt werden. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden.|  
|Betriebssystem (**CmdExec**)|Ausführen eines ausführbaren Programms.|  
|PowerShell|Ausführen eines PowerShell-Skripterstellungs-Auftragsschritts.|  
|Replikationsverteiler|Ausführen eines Auftragsschritts, der den Replikationsverteilungs-Agent aktiviert.|  
|Replikationsmerge|Ausführen eines Auftragsschritts, der den Replikationsmerge-Agent aktiviert.|  
|Replikation-Warteschlangenleser|Ausführen eines Auftragsschritts, der den Warteschlangenleser-Agent der Microsoft SQL Server-Replikation aktiviert.|  
|Replikationsmomentaufnahme|Ausführen eines Auftragsschritts, der den Replikationsmomentaufnahme-Agent aktiviert.|  
|Replikationstransaktionsprotokoll-Leser|Ausführen eines Auftragsschritts, der den Protokolllese-Agent aktiviert.|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)]-Befehl|Ausführen eines [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)]-Befehls.|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)]-Abfrage|Ausführen einer [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)]-Abfrage.|  
|[!INCLUDE[ssIS](../../includes/ssis_md.md)]-Paketausführung|Ausführen eines [!INCLUDE[ssIS](../../includes/ssis_md.md)]-Pakets.|  
  
> [!NOTE]  
> Da [!INCLUDE[tsql](../../includes/tsql-md.md)] -Auftragsschritte keine Proxys verwenden, gibt es kein Subsystem des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents für [!INCLUDE[tsql](../../includes/tsql-md.md)] -Auftragsschritte.  
  
Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent erzwingt Subsystemeinschränkungen auch dann, wenn der Sicherheitsprinzipal für den Proxy normalerweise über die Berechtigung zum Ausführen des Tasks im Auftragsschritt verfügen würde. Beispielsweise kann ein Proxykonto für einen Benutzer, der Mitglied der festen Serverrolle „sysadmin“ ist, nur einen [!INCLUDE[ssIS](../../includes/ssis_md.md)] -Auftragsschritt ausführen, wenn das Proxykonto Zugriff auf das [!INCLUDE[ssIS](../../includes/ssis_md.md)] -Subsystem hat. Der Benutzer kann jedoch [!INCLUDE[ssIS](../../includes/ssis_md.md)] -Pakete ausführen.  
  
### <a name="proxies"></a>Proxys  
Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent verwendet Proxys zum Verwalten von Sicherheitskontexten. Ein Proxy kann für mehrere Auftragsschritte verwendet werden. Mitglieder der festen Serverrolle **sysadmin** können Proxys erstellen.  
  
Jeder Proxy entspricht einem Satz Sicherheitsanmeldeinformationen. Jeder Proxy kann einer Gruppe von Subsystemen und Anmeldenamen zugeordnet werden. Der Proxy kann nur für Auftragsschritte benutzt werden, die ein dem Proxy zugeordnetes Subsystem verwenden. Um einen Auftragsschritt zu erstellen, der einen bestimmten Proxy verwendet, muss der Auftragsbesitzer einen Anmeldenamen verwenden, der diesem Proxy zugeordnet ist, oder ein Mitglied einer Rolle mit unbeschränktem Zugriff auf Proxys sein. Mitglieder der festen Serverrolle **sysadmin** haben unbeschränkten Zugriff auf Proxys. Mitglieder von **SQLAgentUserRole**, **SQLAgentReaderRole** oder **SQLAgentOperatorRole** können nur Proxys verwenden, für die Ihnen der Zugriff erteilt wurde. Jedem Benutzer, der Mitglied einer dieser festen Datenbankrollen des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agents ist, muss Zugriff auf bestimmte Proxys gewährt werden, damit er Auftragsschritte erstellen kann, bei denen diese Proxys verwendet werden.  
  
## <a name="related-tasks"></a>Related Tasks  
Konfigurieren Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent anhand der folgenden Schritte zur Automatisierung der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verwaltung:  
  
1.  Überprüfen Sie, welche administrativen Tasks oder Serverereignisse regelmäßig auftreten und ob diese Tasks oder Ereignisse programmgesteuert verwaltet werden können. Ein Task eignet sich für die Automatisierung, wenn er eine festgelegte Reihenfolge von Schritten umfasst und zu einem bestimmten Zeitpunkt oder als Reaktion auf ein bestimmtes Ereignis auftreten soll.  
  
2.  Definieren Sie die Aufträge, Zeitpläne, Warnungen und Operatoren mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]-Skripts oder [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects (SMO). Weitere Informationen finden Sie unter [Erstellen von Aufträgen](../../ssms/agent/create-jobs.md).  
  
3.  Führen Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Aufträge aus, die Sie definiert haben.  
  
> [!NOTE]  
> Für die Standardinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erhält der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienst den Namen SQLSERVERAGENT. Für benannte Instanzen erhält der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Dienst den Namen SQLAgent$*instancename*.  
  
Falls Sie mehrere Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausführen, können Sie Tasks, die auf allen Instanzen ausgeführt werden müssen, mithilfe der Multiserververwaltung automatisieren. Weitere Informationen finden Sie unter [Automatisierte Verwaltung in einem Unternehmen](../../ssms/agent/automated-administration-across-an-enterprise.md).  
  
Verwenden Sie für die ersten Schritte mit dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent die folgenden Aufgaben:  
  
|BESCHREIBUNG|Thema|  
|-----------|-----|  
|Beschreibt, wie der SQL Server-Agent konfiguriert wird.|[Konfigurieren des SQL Server-Agents](../../ssms/agent/configure-sql-server-agent.md)|  
|Beschreibt, wie der SQL Server-Agent-Dienst gestartet, beendet und angehalten wird.|[Starten, Beenden oder Anhalten des SQL Server-Agent-Diensts](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)|  
|Beschreibt Überlegungen zum Angeben eines Kontos für den SQL Server-Agent-Dienst.|[Auswählen eines Kontos für den SQL Server-Agent-Dienst](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md)|  
|Beschreibt, wie das SQL Server-Agent-Fehlerprotokoll verwendet wird.|[SQL Server-Agent-Fehlerprotokoll](../../ssms/agent/sql-server-agent-error-log.md)|  
|Beschreibt, wie Leistungsobjekte verwendet werden.|[Verwenden von Leistungsobjekten](../../ssms/agent/use-performance-objects.md)|  
|Beschreibt den Wartungsplanungs-Assistenten. Hierbei handelt es sich um ein Hilfsprogramm, mit dem Sie Aufträge, Warnungen und Operatoren erstellen können, um die Verwaltung einer SQL Server-Instanz zu automatisieren.|[Verwenden des Wartungsplanungs-Assistenten](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)|  
|Beschreibt, wie administrative Aufgaben mit dem SQL Server-Agent automatisiert werden.|[Automatisierte Administrationstasks &#40;SQL Server-Agent&#41;](../../ssms/agent/automated-administration-tasks-sql-server-agent.md)|  
  
## <a name="see-also"></a>Weitere Informationen  
[Oberflächenkonfiguration](../../relational-databases/security/surface-area-configuration.md)  
