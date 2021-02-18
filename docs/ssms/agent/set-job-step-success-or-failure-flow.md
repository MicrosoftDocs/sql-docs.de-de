---
description: Set Job Step Success or Failure Flow
title: Set Job Step Success or Failure Flow
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, action flow logic
- successful jobs [SQL Server]
- failed jobs [SQL Server]
- jobs [SQL Server Agent], action flow logic
ms.assetid: 23041ccf-8a07-41d3-85b9-c449a54b7e1e
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: c5556b9348d705606748e9f2b45b1eef57df2e6d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354854"
---
# <a name="set-job-step-success-or-failure-flow"></a>Set Job Step Success or Failure Flow
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Bei der Erstellung von [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Aufträgen können Sie angeben, welche Aktion von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt werden soll, wenn während der Auftragsausführung ein Fehler auftritt. Bestimmen Sie die Aktion, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nach Erfolg oder Fehlschlagen eines Auftragsschrittes ausgeführt wird. Verwenden Sie anschließend die folgende Prozedur, um mithilfe des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents die Logik der Vorgehensweise für die Auftragsschrittaktion zu konfigurieren.  
  
-   **Vorbereitungen:**  
  
    [Security](#Security)  
  
-   **So legen Sie die Vorgehensweise nach Erfolg oder einem Fehler eines Auftragsschritts fest, und zwar mit**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server Management Objects](#SMO)  
  
## <a name="before-you-begin"></a>Vorbereitungen  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
Ausführliche Informationen finden Sie unter [Implementieren der SQL Server-Agent-Sicherheit](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-set-job-step-success-or-failure-flow"></a>So legen Sie die Vorgehensweise nach Erfolg oder Fehlschlagen eines Auftragsschrittes fest  
  
1.  Erweitern Sie in **Objekt-Explorer** den Eintrag **SQL Server-Agent**, und erweitern Sie anschließend **Aufträge**.  
  
2.  Klicken Sie mit der rechten Maustaste auf den Auftrag, den Sie bearbeiten möchten, und klicken Sie dann auf **Eigenschaften**.  
  
3.  Wählen Sie die Seite **Schritte** aus, und klicken Sie erst auf einen Schritt und dann auf **Bearbeiten**.  
  
4.  Wählen Sie im Dialogfeld **Auftragsschritt-Eigenschaften** die Seite **Erweitert** aus.  
  
5.  Wählen Sie im Dialogfeld **Aktion bei Erfolg** auf die Aktion, die nach erfolgreicher Durchführung des Auftragsschrittes ausgeführt werden soll.  
  
6.  Geben Sie im Feld **Wiederholungsversuche** mit einer Zahl zwischen 0 und 9999 an, wie oft der Auftragsschritt wiederholt werden soll, bevor er als fehlgeschlagen gilt. Wenn Sie im Feld **Wiederholungsversuche** einen Wert größer 0 eingegeben haben, geben Sie im Feld **Wiederholungsintervall (Min)** die Anzahl von Minuten zwischen 1 und 9999 ein, die verstreichen müssen, bevor der Auftragsschritt erneut versucht wird.  
  
7.  Klicken Sie in der Liste **Aktion bei Fehler** auf die Aktion, die nach einem fehlgeschlagenen Auftragsschritt ausgeführt werden soll.  
  
8.  Wenn es sich bei dem Auftrag um ein [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skript handelt, können Sie eine der folgenden Optionen auswählen:  
  
    -   Geben Sie im Feld **Ausgabedatei** den Namen der Ausgabedatei ein, in die die Skriptausgabe geschrieben werden soll. Diese Datei wird standardmäßig bei jeder Ausführung des Auftragsschrittes überschrieben. Wenn Sie nicht möchten, dass die Ausgabedatei überschrieben wird, aktivieren Sie **Ausgabe an vorhandene Datei anfügen**.  
  
    -   Aktivieren Sie **In Tabelle protokollieren** , wenn der Auftragsschritt in einer Datenbanktabelle protokolliert werden soll. Standardmäßig wird der Tabelleninhalt bei jeder Ausführung des Auftragsschrittes überschrieben. Wenn der Tabelleninhalt nicht überschrieben werden soll, aktivieren Sie **Ausgabe an vorhandenen Eintrag in Tabelle anfügen**. Nachdem der Auftragsschritt ausgeführt wurde, können Sie den Inhalt dieser Tabelle anzeigen, indem Sie auf **Anzeigen** klicken.  
  
    -   Aktivieren Sie **Schrittausgabe in Verlauf einschließen** , wenn die Ausgabe in den Schrittverlauf eingeschlossen werden soll. Die Ausgabe wird nur angezeigt, wenn keine Fehler auftraten. Es kann auch vorkommen, dass die Ausgabe abgeschnitten wird.  
  
9. Wenn die Liste **Als Benutzer ausführen** verfügbar ist, wählen Sie daraus das Proxykonto mit den Benutzerinformationen aus, das für den Auftrag verwendet werden wird.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Verwenden von Transact-SQL  
  
#### <a name="to-set-job-step-success-or-failure-flow"></a>So legen Sie die Vorgehensweise nach Erfolg oder Fehlschlagen eines Auftragsschrittes fest  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde_md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'TSQL',  
        @command = N'ALTER DATABASE SALES SET READ_ONLY',   
        @on_success_action = 1;  
    GO  
    ```  
  
Weitere Informationen finden Sie unter [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Verwendung von SQL Server Management Objects  
**So legen Sie die Vorgehensweise nach Erfolg oder Fehlschlagen eines Auftragsschrittes fest**  
  
Verwenden Sie die **JobStep** -Klasse indem Sie eine von Ihnen ausgewählte Programmiersprache, z. B. Visual Basic, Visual C# oder PowerShell verwenden. Weitere Informationen finden Sie unter [SQL Server Management Objects (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md).  
