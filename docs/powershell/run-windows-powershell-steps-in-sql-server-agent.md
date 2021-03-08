---
title: Ausführen von Windows PowerShell-Schritten in SQL Server-Agent
description: In diesem Artikel erfahren Sie, wie Sie Windows PowerShell-Schritte in einem SQL Server-Agent-Auftrag ausführen.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: seo-lt-2019
ms.date: 03/16/2017
ms.openlocfilehash: 8029d18dee3dd49342c3c029fc78992900394ed4
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839403"
---
# <a name="run-windows-powershell-steps-in-sql-server-agent"></a>Ausführen von Windows PowerShell-Schritten in SQL Server-Agent

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]

Führen Sie die SQL Server PowerShell-Skripts mithilfe des SQL Server-Agent nach Zeitplan aus.

[!INCLUDE[sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

[!INCLUDE[sql-server-powershell-no-sqlps](../includes/sql-server-powershell-no-sqlps.md)]

## <a name="to-run-powershell-from-sql-server-agent"></a>So führen Sie PowerShell im SQL Server-Agent aus

Es gibt mehrere Typen von Auftragsschritten im SQL Server-Agent. Jeder Typ ist einem Subsystem zugeordnet, das eine bestimmte Umgebung implementiert, wie eine Replikations-Agent- oder Eingabeaufforderungsumgebung. Sie können Windows PowerShell-Skripts schreiben und die Skripts dann mit dem SQL Server-Agent in Aufträge integrieren, die zu festgelegten Zeiten oder in Reaktion auf SQL Server-Ereignisse ausgeführt werden. Windows PowerShell-Skripts können mit entweder einem Eingabeaufforderungs-Auftragsschritt oder einem PowerShell-Auftragsschritt ausgeführt werden.

- Verwenden Sie einen PowerShell-Auftragsschritt, damit das Subsystem des SQL Server-Agents das Hilfsprogramm **sqlps** ausführt, das PowerShell 2.0 startet und das Modul **sqlps**  importiert. Wenn Sie SQL Server 2019 oder höher ausführen, empfiehlt es sich, das Modul **[SqlServer](sql-server-powershell.md#sql-server-agent)** in Ihrem SQL Server-Agent-Auftragsschritt zu verwenden.

- Verwenden Sie einen Auftragsschritt an einer Eingabeaufforderung, um &lt;ui&gt;PowerShell.exe&lt;/ui&gt; auszuführen, und geben Sie ein Skript an, das das **sqlps** -Modul importiert.

### <a name="caution-about-memory-consumption"></a><a name="LimitationsRestrictions"></a> Warnung zur Arbeitsspeichernutzung

Jeder Auftragsschritt des SQL Server-Agents, der PowerShell mit dem Modul **sqlps** ausführt, startet einen Prozess, der etwa **20 MB** Arbeitsspeicher in Anspruch nimmt. Die gleichzeitige Ausführung einer großen Anzahl von Windows PowerShell-Auftragsschritten kann sich negativ auf die Leistung auswirken.

## <a name="create-a-powershell-job-step"></a><a name="PShellJob"></a> Erstellen eines PowerShell-Auftragsschritts

### <a name="to-create-a-powershell-job-step"></a>So erstellen Sie einen PowerShell-Auftragsschritt

1. Erweitern Sie **SQL Server-Agent**, erstellen Sie einen neuen Auftrag, oder klicken Sie mit der rechten Maustaste auf einen vorhandenen Auftrag, und wählen Sie dann **Eigenschaften** aus. Weitere Informationen zum Erstellen eines Auftrags finden Sie unter [Erstellen von Aufträgen](../ssms/agent/create-jobs.md).

2. Wählen Sie im Dialogfeld **Auftragseigenschaften** die Seite **Schritte** und dann auf **Neu** aus.

3. Geben Sie im Dialogfeld **Neuer Auftragsschritt** unter **Schrittname** einen Schrittnamen für den Auftrag ein.

4. Wählen Sie in der Liste **Typ** die Option **PowerShell** aus.

5. Wählen Sie in der Liste **Ausführen als** das Proxykonto mit den Anmeldeinformationen für den Auftrag aus.

6. Geben Sie im Feld **Befehl** die PowerShell-Skriptsyntax ein, die für den Auftragsschritt ausgeführt wird. Wählen Sie alternativ **Öffnen** und dann eine Datei aus, die die Skriptsyntax enthält.

7. Wählen Sie die Seite **Erweitert** aus, um die folgenden Optionen für den Auftragsschritt festzulegen: welche Aktion bei der erfolgreichen oder fehlerhaften Ausführung des Auftragsschrittes jeweils auszuführen ist, wie oft der SQL Server-Agent versuchen soll, den Auftragsschritt auszuführen, und wie viele Wiederholungsversuche unternommen werden sollen.

## <a name="create-a-command-prompt-job-step"></a><a name="CmdExecJob"></a> Erstellen eines Eingabeaufforderungs-Auftragsschritts

### <a name="to-create-a-cmdexec-job-step"></a>So erstellen Sie einen CmdExec-Auftragsschritt

1. Erweitern Sie **SQL Server-Agent**, erstellen Sie einen neuen Auftrag, oder klicken Sie mit der rechten Maustaste auf einen vorhandenen Auftrag, und wählen Sie dann **Eigenschaften** aus. Weitere Informationen zum Erstellen eines Auftrags finden Sie unter [Erstellen von Aufträgen](../ssms/agent/create-jobs.md).

2. Wählen Sie im Dialogfeld **Auftragseigenschaften** die Seite **Schritte** und dann auf **Neu** aus.

3. Geben Sie im Dialogfeld **Neuer Auftragsschritt** unter **Schrittname** einen Schrittnamen für den Auftrag ein.

4. Wählen Sie in der Liste **Typ** den Eintrag **Betriebssystem (CmdExec)** aus.

5. Wählen Sie in der Liste **Ausführen als** das Proxykonto mit den Anmeldeinformationen für den Auftrag aus. Standardmäßig werden CmdExec-Auftragsschritte im Kontext des Kontos des SQL Server-Agent-Dienstes ausgeführt.

6. Geben Sie in das Feld **Prozessexitcode eines erfolgreichen Befehls** einen Wert zwischen 0 und 999999 ein.

7. Geben Sie im Feld **Befehl** "powershell.exe" zusammen mit Parametern, die das auszuführende PowerShell-Skript angeben, ein.

8. Wählen Sie die Seite **Erweitert** aus, um Optionen für Auftragsschritte festzulegen, z. B. welche Aktion bei der erfolgreichen oder fehlerhaften Ausführung des Auftragsschrittes jeweils auszuführen ist, wie oft der SQL Server-Agent versuchen soll, den Auftragsschritt auszuführen, und in welche Datei der SQL Server-Agent die Auftragsschrittausgabe schreiben soll. Nur Mitglieder der festen Serverrolle **sysadmin** können die Auftragsschrittausgabe in eine Betriebssystemdatei schreiben.

## <a name="see-also"></a>Weitere Informationen

- [SQL Server-PowerShell](sql-server-powershell.md)
- [SQL Server-Agent mit PowerShell](sql-server-powershell.md#sql-server-agent)