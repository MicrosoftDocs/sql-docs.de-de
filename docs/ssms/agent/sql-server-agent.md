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
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/03/2021
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 751a627530b7d1017bfd8f0f89aa626ea84bc35c
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186537"
---
# <a name="sql-server-agent"></a>SQL Server-Agent

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Der SQL Server-Agent ist ein Microsoft Windows-Dienst, der geplante administrative Aufgaben ausführt, die in SQL Server als *Aufträge* bezeichnet werden.

## <a name="benefits-of-sql-server-agent"></a><a name="Benefits"></a>Vorteile des SQL Server-Agents

Der SQL Server-Agent verwendet SQL Server, um Auftragsinformationen zu speichern. Aufträge enthalten mindestens einen Auftragsschritt. Jeder Schritt umfasst einen eigenen Task, z.B. das Sichern einer Datenbank.

Der SQL Server-Agent kann einen Auftrag anhand eines Zeitplans, als Reaktion auf ein bestimmtes Ereignis oder bei Bedarf ausführen. Wenn Sie z.B. am Ende jedes Arbeitstages alle Server des Unternehmens sichern möchten, können Sie diesen Task automatisieren. Planen Sie die Ausführung der Sicherung von Montag bis Freitag nach 22:00 Uhr. Wenn während der Sicherung ein Problem auftritt, kann der SQL Server-Agent das Ereignis aufzeichnen und Sie benachrichtigen.

> [!NOTE]
> Der SQL Server-Agent-Dienst ist standardmäßig deaktiviert, wenn SQL Server installiert ist, sofern der Benutzer nicht explizit festgelegt hat, dass der Dienst automatisch gestartet werden soll.

## <a name="sql-server-agent-components"></a><a name="Components"></a>SQL Server Agent Components

Der SQL Server-Agent verwendet die folgenden Komponenten, um die auszuführenden Aufgaben, den Zeitpunkt der Ausführung und die Meldung erfolgreicher bzw. fehlgeschlagener Aufgaben zu definieren.

### <a name="jobs"></a>Aufträge

Ein *Auftrag* ist eine festgelegte Reihe von Aktionen, die der SQL Server-Agent ausführt. Verwenden Sie Aufträge, um eine Verwaltungsaufgabe zu definieren, die einmalig oder mehrmals ausgeführt und deren Erfolgsstatus überwacht werden kann. Ein Auftrag kann auf einem einzelnen lokalen Server oder auf mehreren Remoteservern ausgeführt werden.

> [!IMPORTANT]
> Aufträge des SQL Server-Agents, die zum Zeitpunkt eines Failoverereignisses auf einer SQL Server-Failoverclusterinstanz ausgeführt werden, werden nach dem Failover nicht auf einem anderen Failoverclusterknoten fortgesetzt. Aufträge des SQL Server-Agents, die ausgeführt werden, wenn ein Hyper-V-Knoten angehalten wird, werden nicht fortgesetzt, wenn die Pause ein Failover zu einem anderen Knoten verursacht. Aufträge, die begonnen, aber wegen eines Failoverereignisses nicht abgeschlossen werden, werden als gestartet protokolliert, jedoch werden keine weiteren Protokolleinträge für Abschluss oder Fehler erstellt. Unter diesen Umständen werden die betreffenden Aufträge des SQL Server-Agents scheinbar nie beendet.

Für die Ausführung von Aufträgen gibt es mehrere Möglichkeiten:

- Ausführung nach einem oder mehreren Zeitplänen.

- Ausführung als Reaktion auf eine oder mehrere Warnungen.

- Ausführung im Rahmen der gespeicherten Prozedur sp_start_job.

Die einzelnen Aktionen im Rahmen eines Auftrags werden als *Auftragsschritte* bezeichnet. Ein Auftragsschritt kann beispielsweise in der Ausführung einer Transact\-SQL-Anweisung, der Ausführung eines SSIS-Pakets oder der Ausgabe eines Befehls an einen Analysis Services-Server bestehen. Auftragsschritte werden als Teil des Auftrags verwaltet.

Jeder Auftragsschritt wird in einem bestimmten Sicherheitskontext ausgeführt. Bei Auftragsschritten, die Transact\-SQL verwenden, nutzen Sie zum Festlegen des Sicherheitskontexts für den Auftragsschritt die EXECUTE AS-Anweisung. Bei anderen Arten von Auftragsschritten verwenden Sie ein Proxykonto, um den Sicherheitskontext für den Auftragsschritt festzulegen.

### <a name="schedules"></a>Zeitpläne

Durch einen *Zeitplan* wird angegeben, wann ein Auftrag ausgeführt wird. Mehrere Aufträge können auf dem gleichen Zeitplan basieren, und für einen Auftrag können mehrere Zeitpläne gelten. Ein Zeitplan kann folgende Bedingungen für die Ausführungszeit eines Auftrags definieren:

- Bei jedem Start des SQL Server-Agents.

- Ausführung, wenn sich die CPU-Auslastung des Computers in einem Bereich befindet, den Sie als Leerlauf definiert haben.

- Einmalige Ausführung zu einem bestimmten Zeitpunkt.

- Auf der Grundlage einer Zeitplanserie.

Weitere Informationen finden Sie unter [Anlegen und Zuweisen von Zeitplänen zu Aufträgen](../../ssms/agent/create-and-attach-schedules-to-jobs.md).

### <a name="alerts"></a>Alerts

Eine *Warnung* ist eine automatische Reaktion auf ein bestimmtes Ereignis. Bei einem Ereignis kann es sich z.B. um das Starten eines Auftrags oder das Erreichen eines bestimmten Schwellenwerts bei den Systemressourcen handeln. Sie definieren die Bedingungen, unter denen eine Warnung auftritt.

Eine Warnung kann als Reaktion auf eine der folgenden Bedingungen ausgegeben werden:

- SQL Server-Ereignisse

- SQL-Leistungsbedingungen

- Ereignisse in der Microsoft Windows-Verwaltungsinstrumentation (WMI) auf dem Computer, auf dem der SQL Server-Agent ausgeführt wird

Eine Warnung kann die folgenden Aktionen ausführen:

- Benachrichtigen eines oder mehrerer Operatoren

- Ausführen eines Auftrags

Weitere Informationen finden Sie unter [Warnungen](../../ssms/agent/alerts.md).  

### <a name="operators"></a>Operatoren

Ein *Operator* definiert die Kontaktinformationen einer Person, die für die Verwaltung einer oder mehrerer SQL Server-Instanzen zuständig ist. In einigen Unternehmen werden die Aufgaben eines Operators einer einzelnen Person zugewiesen. In größeren Unternehmen mit mehreren Servern teilen sich mehrere Personen die Aufgaben des Operators. Ein Bediener verfügt über keine Sicherheitsinformationen und definiert keinen Sicherheitsprinzipal.  

SQL Server kann Operatoren über folgende Methoden zu Warnungen benachrichtigen:

- E-Mail

- Pager (per E-Mail)

- **net send**

> [!NOTE]
> Der Windows Messenger-Dienst muss auf dem Computer mit dem SQL Server-Agent gestartet worden sein, um Benachrichtigungen mit **net send** zu senden.

> [!IMPORTANT]
> Der Pager und **net send** werden aus dem SQL Server-Agent in einer zukünftigen Version von SQL Server entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktionen zurzeit verwenden.  

Sie müssen den SQL Server-Agent so konfigurieren, dass Datenbank-E-Mail verwendet wird, um Operatoren Benachrichtigungen per E-Mail oder Pager zu senden. Weitere Informationen finden Sie unter [Datenbank-E-Mail](../../relational-databases/database-mail/database-mail.md).

Ein Operator kann auch als Alias für eine Gruppe von Personen definiert werden. In diesem Fall werden alle Mitglieder dieses Alias nicht zur selben Zeit überprüft. Weitere Informationen finden Sie unter [Operatoren](../../ssms/agent/operators.md).

## <a name="security-for-sql-server-agent-administration"></a><a name="Security"></a>Sicherheit für die Administration mit dem SQL Server-Agent

Der SQL Server-Agent verwendet die festen Datenbankrollen **SQLAgentUserRole**, **SQLAgentReaderRole** und **SQLAgentOperatorRole** in der **msdb**-Datenbank, um den Zugriff auf den -Agent für Benutzer zu steuern, die keine Mitglieder der festen Serverrolle **sysadmin** sind. Neben diesen festen Datenbankrollen können Datenbankadministratoren mithilfe von Subsystemen und Proxys sicherstellen, dass jeder Auftragsschritt mit den mindestens erforderlichen Berechtigungen ausgeführt wird.  

### <a name="roles"></a>Rollen

Mitglieder der festen Datenbankrollen **SQLAgentUserRole**, **SQLAgentReaderRole** und **SQLAgentOperatorRole** in **msdb** und Mitglieder der festen Serverrolle **sysadmin** haben Zugriff auf den SQL Server-Agent. Ein Benutzer, der keiner dieser Rollen angehört, kann den SQL Server-Agent nicht verwenden. Weitere Informationen zu den Rollen, die vom SQL Server-Agent verwendet werden, finden Sie unter [Implementieren der SQL Server-Agent-Sicherheit](../../ssms/agent/implement-sql-server-agent-security.md).

### <a name="subsystems"></a>Subsysteme

Ein Subsystem ist ein vordefiniertes Objekt, das die für einen Auftragsschritt verfügbare Funktionalität darstellt. Jeder Proxy hat Zugriff auf mindestens ein Subsystem. Subsysteme bieten Sicherheit, weil sie den Zugriff auf die für ein Proxykonto verfügbare Funktionalität begrenzen. Jeder Auftragsschritt wird im Kontext eines Proxys ausgeführt. Ausgenommen sind lediglich Transact\-SQL-Auftragsschritte. Bei Transact\-SQL-Auftragsschritten wird der Sicherheitskontext mithilfe des EXECUTE AS-Befehls für den Besitzer des Auftrags festgelegt.  

SQL Server definiert die in der folgenden Tabelle aufgeführten Subsysteme:  

|Name des Subsystems|BESCHREIBUNG|
|--------------|-----------|
|Microsoft ActiveX-Skript|Ausführen eines ActiveX-Skriptauftragsschritts.<br /><br />**Warnung:** Das Subsystem ActiveX-Skript wird in einer zukünftigen Version von Microsoft SQL Server aus dem SQL Server-Agent entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden.|  
|Betriebssystem (**CmdExec**)|Ausführen eines ausführbaren Programms.|
|PowerShell|Ausführen eines PowerShell-Skripterstellungs-Auftragsschritts.|
|Replikationsverteiler|Ausführen eines Auftragsschritts, der den Replikationsverteilungs-Agent aktiviert.|
|Replikationsmerge|Ausführen eines Auftragsschritts, der den Replikationsmerge-Agent aktiviert.|
|Replikation-Warteschlangenleser|Ausführen eines Auftragsschritts, der den Warteschlangenleser-Agent der Microsoft SQL Server-Replikation aktiviert.|
|Replikationsmomentaufnahme|Ausführen eines Auftragsschritts, der den Replikationsmomentaufnahme-Agent aktiviert.|
|Replikationstransaktionsprotokoll-Leser|Ausführen eines Auftragsschritts, der den Protokolllese-Agent aktiviert.|
|Analysis Services-Befehl|Führen Sie einen Analysis Services-Befehl aus.|
|Analysis Services-Abfrage|Führen Sie eine Analysis Services-Abfrage aus.|
|SSIS-Paketausführung|Führen Sie ein SSIS-Paket aus.|

> [!NOTE]
> Da Transact\-SQL-Auftragsschritte keine Proxys verwenden, ist kein SQL Server-Agent-Subsystem für Transact\-SQL-Auftragsschritte verfügbar.  

Der SQL Server-Agent erzwingt Subsystemeinschränkungen auch dann, wenn der Sicherheitsprinzipal für den Proxy normalerweise über die Berechtigung zum Ausführen des Tasks im Auftragsschritt verfügen würde. Beispielsweise kann ein Proxykonto für einen Benutzer, der Mitglied der festen Serverrolle „sysadmin“ ist, nur einen SSIS-Auftragsschritt ausführen, wenn das Proxykonto Zugriff auf das SSIS-Subsystem hat. Der Benutzer kann jedoch SSIS-Pakete ausführen.  

### <a name="proxies"></a>Proxys

Der SQL Server-Agent verwendet Proxys zum Verwalten von Sicherheitskontexten. Ein Proxy kann für mehrere Auftragsschritte verwendet werden. Mitglieder der festen Serverrolle **sysadmin** können Proxys erstellen.  

Jeder Proxy entspricht einem Satz Sicherheitsanmeldeinformationen. Jeder Proxy kann einer Gruppe von Subsystemen und Anmeldenamen zugeordnet werden. Der Proxy kann nur für Auftragsschritte benutzt werden, die ein dem Proxy zugeordnetes Subsystem verwenden. Der Auftragsbesitzer muss einen Anmeldenamen verwenden, der diesem Proxy zugeordnet ist, oder ein Mitglied einer Rolle mit unbeschränktem Zugriff auf Proxys sein um einen Auftragsschritt zu erstellen, der einen bestimmten Proxy verwendet. Mitglieder der festen Serverrolle **sysadmin** haben unbeschränkten Zugriff auf Proxys. Mitglieder von **SQLAgentUserRole**, **SQLAgentReaderRole** oder **SQLAgentOperatorRole** können nur Proxys verwenden, für die Ihnen der Zugriff erteilt wurde. Jedem Benutzer, der Mitglied einer dieser festen Datenbankrollen des SQL Server-Agents ist, muss Zugriff auf bestimmte Proxys gewährt werden, damit er Auftragsschritte erstellen kann, bei denen diese Proxys verwendet werden.  

## <a name="related-tasks"></a>Related Tasks

Führen Sie die folgenden Schritte aus, um den SQL Server-Agent so zu konfigurieren, dass die SQL Server-Administration automatisiert wird:

1. Überprüfen Sie, welche administrativen Tasks oder Serverereignisse regelmäßig auftreten und ob diese Tasks oder Ereignisse programmgesteuert verwaltet werden können. Ein Task eignet sich für die Automatisierung, wenn er eine festgelegte Reihenfolge von Schritten umfasst und zu einem bestimmten Zeitpunkt oder als Reaktion auf ein bestimmtes Ereignis auftreten soll.

2. Definieren Sie mehrere Aufträge, Zeitpläne, Warnungen sowie Operatoren, indem Sie SQL Server Management Studio, Transact\-SQL-Skripts oder SQL Server Management Objects (SMO) verwenden. Weitere Informationen finden Sie unter [Erstellen von Aufträgen](../../ssms/agent/create-jobs.md).  

3. Führen Sie die SQL Server-Agent-Aufträge aus, die Sie definiert haben.

> [!NOTE]
> Für die Standardinstanz von SQL Server ist für den SQL Server-Dienst der Name SQLSERVERAGENT festgelegt. Für benannte Instanzen erhält der SQL Server-Agent-Dienst den Namen SQLAgent$*instancename*.

Falls Sie mehrere Instanzen von SQL Server ausführen, können Sie Tasks, die auf allen Instanzen ausgeführt werden müssen, mithilfe der Multiserververwaltung automatisieren. Weitere Informationen finden Sie unter [Automatisierte Verwaltung in einem Unternehmen](../../ssms/agent/automated-administration-across-an-enterprise.md).

Verwenden Sie für die ersten Schritte mit dem SQL Server-Agent die folgenden Aufgaben:

|BESCHREIBUNG|Thema|
|-----------|-----|
|Beschreibt, wie der SQL Server-Agent konfiguriert wird.|[Konfigurieren des SQL Server-Agents](../../ssms/agent/configure-sql-server-agent.md)|  
|Beschreibt, wie der SQL Server-Agent-Dienst gestartet, beendet und angehalten wird.|[Starten, Beenden oder Anhalten des SQL Server-Agent-Diensts](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)|
|Beschreibt Überlegungen zum Angeben eines Kontos für den SQL Server-Agent-Dienst.|[Auswählen eines Kontos für den SQL Server-Agent-Dienst](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md)|
|Beschreibt, wie das SQL Server-Agent-Fehlerprotokoll verwendet wird.|[SQL Server-Agent-Fehlerprotokoll](../../ssms/agent/sql-server-agent-error-log.md)|  
|Beschreibt, wie Leistungsobjekte verwendet werden.|[Verwenden von Leistungsobjekten](../../ssms/agent/use-performance-objects.md)|
|Beschreibt den Wartungsplanungs-Assistenten. Hierbei handelt es sich um ein Hilfsprogramm, mit dem Sie Aufträge, Warnungen und Operatoren erstellen können, um die Verwaltung einer SQL Server-Instanz zu automatisieren.|[Verwenden des Wartungsplanungs-Assistenten](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)|
|Beschreibt, wie administrative Aufgaben mit dem SQL Server-Agent automatisiert werden.|[Automatisierte Administrationstasks &#40;SQL Server-Agent&#41;](../../ssms/agent/automated-administration-tasks-sql-server-agent.md)|

## <a name="nosqlps"></a>NOSQLPS

[!INCLUDE [sql-server-powershell-no-sqlps](../../includes/sql-server-powershell-no-sqlps.md)]

## <a name="see-also"></a>Weitere Informationen

- [Oberflächenkonfiguration](../../relational-databases/security/surface-area-configuration.md)
- [SQL Server-Agent mit PowerShell](../../powershell/sql-server-powershell.md#sql-server-agent)
