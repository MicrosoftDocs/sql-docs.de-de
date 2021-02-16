---
title: Erstellen und Ausführen von Aufträgen für SQL Server für Linux
description: Erfahren Sie, wie Sie sowohl mit Transact-SQL als auch mit SQL Server Management Studio (SSMS) einen SQL Server-Agent-Auftrag unter Linux erstellen.
author: VanMSFT
ms.author: vanto
ms.date: 02/20/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1d93d95e-9c89-4274-9b3f-fa2608ec2792
ms.openlocfilehash: bc52f0a2052d0b9abd1700e92af3a30c0e02bad2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346445"
---
# <a name="create-and-run-sql-server-agent-jobs-on-linux"></a>Erstellen und Ausführen eines SQL Server-Agent-Auftrags unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

SQL Server-Aufträge werden verwendet, um regelmäßig dieselbe Befehlssequenz in Ihrer SQL Server-Datenbank auszuführen. Dieses Tutorial enthält ein Beispiel für das Erstellen eines SQL Server-Agent-Auftrags unter Linux mithilfe von Transact-SQL und SQL Server Management Studio (SSMS).

> [!div class="checklist"]
> * Installieren des SQL Server-Agent unter Linux
> * Erstellen eines neuen Auftrags zum Ausführen täglicher Datenbanksicherungen
> * Planen und Ausführen des Auftrags
> * Ausführen der gleichen Schritte in SSMS (optional)

Informationen zu bekannten Problemen mit dem SQL Server-Agent unter Linux finden Sie in den [Versionshinweisen](sql-server-linux-release-notes.md).

## <a name="prerequisites"></a>Voraussetzungen

Zur Durchführung dieses Tutorials ist Folgendes erforderlich:

* Ein Linux-Computer, der folgende Voraussetzungen erfüllt:
  * SQL Server ([RHEL](quickstart-install-connect-red-hat.md), [SLES](quickstart-install-connect-suse.md) oder [Ubuntu](quickstart-install-connect-ubuntu.md)) mit Befehlszeilentools.

Die folgenden Voraussetzungen sind optional:

* Windows-Computer mit SSMS:
  * [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) für optionale SSMS-Schritte.

## <a name="enable-sql-server-agent"></a>Aktivieren des SQL Server-Agents.

Um den SQL Server-Agent unter Linux verwenden zu können, müssen Sie den SQL Server-Agent zuerst auf einem Computer aktivieren, auf dem bereits SQL Server installiert ist.

1. Um den SQL Server-Agent zu aktivieren, führen Sie den folgenden Schritt aus.
  ```bash
  sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
  ```

1. Starten Sie SQL Server mithilfe des folgenden Befehls neu:
  ```bash
  sudo systemctl restart mssql-server
  ```

> [!NOTE]
> Ab SQL Server 2017 CU4 ist der SQL Server-Agent im **mssql-server**-Paket enthalten und standardmäßig deaktiviert. Informationen über das Setup eines Agent vor CU4 finden Sie unter [Installieren des SQL Server-Agent unter Linux](sql-server-linux-setup-sql-agent.md).

## <a name="create-a-sample-database"></a>Erstellen einer Beispieldatenbank

Führen Sie die folgenden Schritte aus, um eine Beispieldatenbank mit dem Namen **SampleDB** zu erstellen. Diese Datenbank wird für den täglichen Sicherungsauftrag verwendet.

1. Öffnen Sie auf Ihrem Linux-Computer eine Bash-Terminalsitzung.

1. Führen Sie mit **sqlcmd** einen Transact- SQL-**CREATE DATABASE**-Befehl aus.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -Q 'CREATE DATABASE SampleDB'
   ```

1. Überprüfen Sie, ob die Datenbank erstellt wurde, indem Sie die Datenbanken auf dem Server auflisten.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -Q 'SELECT Name FROM sys.Databases'
   ```

## <a name="create-a-job-with-transact-sql"></a>Erstellen eines Auftrags mit Transact-SQL

Mit den folgenden Schritten erstellen Sie einen SQL Server-Agent-Auftrag unter Linux mit Transact-SQL-Befehlen. Der Auftrag führt eine tägliche Sicherung der Beispieldatenbank **SampleDB** aus.

> [!TIP]
> Sie können einen beliebigen T-SQL-Client verwenden, um diese Befehle auszuführen. Unter Linux können Sie z.B. [sqlcmd](sql-server-linux-setup-tools.md) oder [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md) verwenden. Von einem Remote-Windows-Server aus können Sie auch Abfragen in SQL Server Management Studio (SSMS) ausführen oder die Benutzeroberflächen-Schnittstelle für die Auftragsverwaltung verwenden, die im nächsten Abschnitt beschrieben wird.

1. Führen Sie [sp_add_job](../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md) aus, um einen Auftrag namens `Daily SampleDB Backup` zu erstellen.

   ```sql
   -- Adds a new job executed by the SQLServerAgent service
   -- called 'Daily SampleDB Backup'
   USE msdb ;
   GO
   EXEC dbo.sp_add_job
      @job_name = N'Daily SampleDB Backup' ;
   GO
   ```

1. Rufen Sie [sp_add_jobstep](../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md) auf, um einen Auftragsschritt zu erstellen, der eine Sicherung der `SampleDB`-Datenbank erstellt.

   ```sql
   -- Adds a step (operation) to the job
   EXEC sp_add_jobstep
      @job_name = N'Daily SampleDB Backup',
      @step_name = N'Backup database',
      @subsystem = N'TSQL',
      @command = N'BACKUP DATABASE SampleDB TO DISK = \
         N''/var/opt/mssql/data/SampleDB.bak'' WITH NOFORMAT, NOINIT, \
         NAME = ''SampleDB-full'', SKIP, NOREWIND, NOUNLOAD, STATS = 10',
      @retry_attempts = 5,
      @retry_interval = 5 ;
   GO
   ```

1. Erstellen Sie dann mit [sp_add_schedule](../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) einen täglichen Zeitplan für Ihren Auftrag.

   ```sql
   -- Creates a schedule called 'Daily'
   EXEC dbo.sp_add_schedule
      @schedule_name = N'Daily SampleDB',
      @freq_type = 4,
      @freq_interval = 1,
      @active_start_time = 233000 ;
   USE msdb ;
   GO
   ```

1. Fügen Sie mit [sp_attach_schedule](../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md) den Auftragszeitplan dem Auftrag an.

   ```sql
   -- Sets the 'Daily' schedule to the 'Daily SampleDB Backup' Job
   EXEC sp_attach_schedule
      @job_name = N'Daily SampleDB Backup',
      @schedule_name = N'Daily SampleDB';
   GO
   ```

1. Verwenden Sie [sp_add_jobserver](../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md), um den Auftrag einem Zielserver zuzuweisen. In diesem Beispiel ist das Ziel der lokale Server.

   ```sql
   EXEC dbo.sp_add_jobserver
      @job_name = N'Daily SampleDB Backup',
      @server_name = N'(LOCAL)';
   GO
   ```
1. Starten Sie den Auftrag mit [sp_start_job](../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md).

   ```sql
   EXEC dbo.sp_start_job N' Daily SampleDB Backup' ;
   GO
   ```

## <a name="create-a-job-with-ssms"></a>Erstellen eines Auftrags mit SSMS

Sie können Aufträge mithilfe von SQL Server Management Studio (SSMS) auch remote unter Windows erstellen und verwalten.

1. Starten Sie SSMS unter Windows, und stellen Sie eine Verbindung mit Ihrer Linux-SQL Server-Instanz her. Weitere Informationen finden Sie unter [Verwenden von SQL Server Management Studio unter Windows zum Verwalten von SQL Server unter Linux](sql-server-linux-manage-ssms.md).

1. Vergewissern Sie sich, dass Sie eine Beispieldatenbank mit dem Namen **SampleDB** erstellt haben.

   <img src="./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-0.png" alt="Create a SampleDB database" style="width: 550px;"/>

1. Stellen Sie sicher, dass der SQL-Agent richtig [installiert](sql-server-linux-setup-sql-agent.md) und konfiguriert wurde. Suchen Sie im Objekt-Explorer das Pluszeichen neben SQL Server-Agent. Wenn SQL Server-Agent nicht aktiviert ist, versuchen Sie, den **mssql-server**-Dienst unter Linux neu zu starten.

   ![Überprüfen, ob SQL Server-Agent installiert wurde](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-1.png)

1. Erstellen Sie einen neuen Auftrag.

   ![Erstellen eines neuen Auftrags](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-2.png)

1. Geben Sie Ihrem Auftrag einen Namen, und erstellen Sie den Auftragsschritt.

   ![Erstellen eines Auftragsschritts](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-3.png)

1. Geben Sie an, welches Subsystem Sie verwenden möchten, und was der Auftragsschritt ausführen soll.

   ![Auftragssubsystem](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-4.png)

   ![Auftragsschrittaktion](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-5.png)

1. Erstellen Sie einen neuen Auftragszeitplan.

   ![Screenshot: Dialogfeld „Neuer Auftrag“ mit hervorgehobener Option „Zeitpläne“ und umrandeter Option „Neu...“](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-6.png)

   ![Screenshot: Dialogfeld „Neuer Auftrag“ mit umrandeter Option „OK“](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-8.png)

1. Starten Sie Ihren Auftrag.

   <img src="./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-9.png" alt="Start the SQL Server Agent job" style="width: 550px;"/>

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Installieren des SQL Server-Agent unter Linux
> * Verwenden von Transact-SQL und gespeicherten Systemprozeduren zum Erstellen von Aufträgen
> * Erstellen eines Auftrags, der tägliche Datenbanksicherungen ausführt
> * Verwenden der SSMS-Benutzeroberfläche zum Erstellen und Verwalten von Aufträgen

Lernen Sie als nächstes weitere Möglichkeiten zum Erstellen und Verwalten von Aufträgen kennen:

> [!div class="nextstepaction"]
>[SQL Server-Agent-Dokumentation](../ssms/agent/sql-server-agent.md)