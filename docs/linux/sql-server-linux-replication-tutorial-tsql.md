---
title: 'Tutorial: Konfigurieren der Replikation mit T-SQL'
description: Konfigurieren Sie die SQL Server-Momentaufnahmereplikation unter Linux mit zwei SQL Server-Instanzen unter Verwendung von Transact-SQL (T-SQL).
ms.custom: seo-dt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 12/09/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
titleSuffix: SQL Server on Linux
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: 825fec3eeb0abbfacd4bb5f1311e13b6f99189e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346467"
---
# <a name="configure-replication-with-t-sql"></a>Konfigurieren der Replikation mit T-SQL

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)] 

In diesem Tutorial konfigurieren Sie die Replikation von SQL Server-Momentaufnahmen unter Linux mit zwei Instanzen von SQL Server mithilfe von Transact-SQL. Der Herausgeber und der Verteiler sind dieselbe Instanz, und der Abonnent befindet sich auf einer separaten Instanz.

> [!div class="checklist"]
> * Aktivieren von SQL Server-Replikations-Agents unter Linux
> * Erstellen einer Beispieldatenbank
> * Konfigurieren des Momentaufnahmeordners für den Zugriff durch SQL Server-Agents
> * Konfigurieren des Verteilers
> * Konfigurieren des Herausgebers
> * Konfigurieren von Veröffentlichungen und Artikeln
> * Konfigurieren des Abonnenten 
> * Ausführen der Replikationsaufträge

Alle Replikationskonfigurationen können mit [gespeicherten Replikationsprozeduren](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md) konfiguriert werden.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durcharbeiten dieses Tutorials benötigen Sie Folgendes:

- Zwei Instanzen von SQL Server mit der aktuellen Version von SQL Server für Linux
- Ein Tool zum Ausgeben von T-SQL-Abfragen zum Einrichten der Replikation, z.B. SQLCMD oder SSMS

   Siehe [Verwenden von SSMS zum Verwalten von SQL Server für Linux](./sql-server-linux-manage-ssms.md).

   >[!NOTE]
   >[!INCLUDE[SQL Server 2017](../includes/sssql17-md.md)] ([CU18](https://support.microsoft.com/help/4527377)) und später unterstützen SQL Server-Replikation für Instanzen von SQL Server für Linux eingeführt.

## <a name="detailed-steps"></a>Ausführliche Schritte

1. Aktivieren Sie SQL Server-Replikations-Agents unter Linux. Führen Sie auf beiden Hostcomputern die folgenden Befehle im Terminal aus. 

   ```bash
   sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
   sudo systemctl restart mssql-server
   ```

1. Erstellen Sie die Beispieldatenbank und -tabelle. Erstellen Sie auf dem Herausgeber eine Beispieldatenbank und -tabelle, die als Artikel für eine Veröffentlichung fungieren.

   ```sql
   CREATE DATABASE Sales
   GO
   USE [SALES]
   GO 
   CREATE TABLE CUSTOMER([CustomerID] [int] NOT NULL, [SalesAmount] [decimal] NOT NULL)
   GO 
   INSERT INTO CUSTOMER (CustomerID, SalesAmount) VALUES (1,100),(2,200),(3,300)
   ```

   Erstellen Sie auf der anderen SQL Server-Instanz, dem Abonnenten, die Datenbank, um die Artikel zu empfangen.

   ```sql
   CREATE DATABASE Sales
   GO
   ```

1. Erstellen Sie den Momentaufnahmeordner für SQL Server-Agents zum Lesen/Schreiben. Erstellen Sie auf dem Verteiler den Momentaufnahmeordner, und gewähren Sie dem Benutzer „mssql“ Zugriff. 

   ```bash
   sudo mkdir /var/opt/mssql/data/ReplData/
   sudo chown mssql /var/opt/mssql/data/ReplData/
   sudo chgrp mssql /var/opt/mssql/data/ReplData/
   ```

1. Konfigurieren Sie den Verteiler. In diesem Beispiel ist der Herausgeber ebenfalls der Verteiler. Führen Sie die folgenden Befehle auf dem Verleger aus, um die Instanz ebenfalls für die Verteilung zu konfigurieren.

   ```sql
   DECLARE @distributor AS sysname
   DECLARE @distributorlogin AS sysname
   DECLARE @distributorpassword AS sysname
   -- Specify the distributor name. Use 'hostname' command on in terminal to find the hostname
   SET @distributor = N'<distributor instance name>'--in this example, it will be the name of the publisher
   SET @distributorlogin = N'<distributor login>'
   SET @distributorpassword = N'<distributor password>'
   -- Specify the distribution database. 
   
   use master
   exec sp_adddistributor @distributor = @distributor -- this should be the hostname

   -- Log into distributor and create Distribution Database. In this example, our publisher and distributor is on the same host
   exec sp_adddistributiondb @database = N'distribution', @log_file_size = 2, @deletebatchsize_xact = 5000, @deletebatchsize_cmd = 2000, @security_mode = 0, @login = @distributorlogin, @password = @distributorpassword
   GO

   DECLARE @snapshotdirectory AS nvarchar(500)
   SET @snapshotdirectory = N'/var/opt/mssql/data/ReplData/'

   -- Log into distributor and create Distribution Database. In this example, our publisher and distributor is on the same host
   use [distribution] 
   if (not exists (select * from sysobjects where name = 'UIProperties' and type = 'U ')) 
          create table UIProperties(id int) 
   if (exists (select * from ::fn_listextendedproperty('SnapshotFolder', 'user', 'dbo', 'table', 'UIProperties', null, null))) 
          EXEC sp_updateextendedproperty N'SnapshotFolder', @snapshotdirectory, 'user', dbo, 'table', 'UIProperties' 
   else 
         EXEC sp_addextendedproperty N'SnapshotFolder', @snapshotdirectory, 'user', dbo, 'table', 'UIProperties'
   GO
   ```

1. Konfigurieren Sie den Verleger. Führen Sie die folgenden TSQL-Befehle auf dem Herausgeber aus.

   ```sql
   DECLARE @publisher AS sysname
   DECLARE @distributorlogin AS sysname
   DECLARE @distributorpassword AS sysname
   -- Specify the distributor name. Use 'hostname' command on in terminal to find the hostname
   SET @publisher = N'<instance name>' 
   SET @distributorlogin = N'<distributor login>'
   SET @distributorpassword = N'<distributor password>'
   -- Specify the distribution database. 

   -- Adding the distribution publishers
   exec sp_adddistpublisher @publisher = @publisher, 
   @distribution_db = N'distribution', 
   @security_mode = 0, 
   @login = @distributorlogin, 
   @password = @distributorpassword, 
   @working_directory = N'/var/opt/mssql/data/ReplData', 
   @trusted = N'false', 
   @thirdparty_flag = 0, 
   @publisher_type = N'MSSQLSERVER'
   GO
   ```

1. Konfigurieren Sie Veröffentlichungsaufträge. Führen Sie die folgenden TSQL-Befehle auf dem Herausgeber aus.

   ```sql
   DECLARE @replicationdb AS sysname
   DECLARE @publisherlogin AS sysname
   DECLARE @publisherpassword AS sysname
   SET @replicationdb = N'Sales'
   SET @publisherlogin = N'<Publisher login>'
   SET @publisherpassword = N'<Publisher Password>'

   use [Sales]
   exec sp_replicationdboption @dbname = N'Sales', @optname = N'publish', @value = N'true'
   
   -- Add the snapshot publication
   exec sp_addpublication 
   @publication = N'SnapshotRepl', 
   @description = N'Snapshot publication of database ''Sales'' from Publisher ''<PUBLISHER HOSTNAME>''.',
   @retention = 0, 
   @allow_push = N'true', 
   @repl_freq = N'snapshot', 
   @status = N'active', 
   @independent_agent = N'true'

   exec sp_addpublication_snapshot @publication = N'SnapshotRepl', 
   @frequency_type = 1, 
   @frequency_interval = 1, 
   @frequency_relative_interval = 1, 
   @frequency_recurrence_factor = 0, 
   @frequency_subday = 8, 
   @frequency_subday_interval = 1, 
   @active_start_time_of_day = 0,
   @active_end_time_of_day = 235959, 
   @active_start_date = 0, 
   @active_end_date = 0, 
   @publisher_security_mode = 0, 
   @publisher_login = @publisherlogin, 
   @publisher_password = @publisherpassword
   ```

1. Erstellen Sie Artikel aus der Verkaufstabelle. Führen Sie die folgenden TSQL-Befehle auf dem Herausgeber aus.

   ```sql
   use [Sales]
   exec sp_addarticle 
   @publication = N'SnapshotRepl', 
   @article = N'customer', 
   @source_owner = N'dbo', 
   @source_object = N'customer', 
   @type = N'logbased', 
   @description = null, 
   @creation_script = null, 
   @pre_creation_cmd = N'drop', 
   @schema_option = 0x000000000803509D,
   @identityrangemanagementoption = N'manual', 
   @destination_table = N'customer', 
   @destination_owner = N'dbo', 
   @vertical_partition = N'false'
   ```

1. Konfigurieren Sie das Abonnement. Führen Sie die folgenden TSQL-Befehle auf dem Herausgeber aus.

   ```sql
   DECLARE @subscriber AS sysname
   DECLARE @subscriber_db AS sysname
   DECLARE @subscriberLogin AS sysname
   DECLARE @subscriberPassword AS sysname
   SET @subscriber = N'<Instance Name>' -- for example, MSSQLSERVER
   SET @subscriber_db = N'Sales'
   SET @subscriberLogin = N'<Subscriber Login>'
   SET @subscriberPassword = N'<Subscriber Password>'

   use [Sales]
   exec sp_addsubscription 
   @publication = N'SnapshotRepl', 
   @subscriber = @subscriber,
   @destination_db = @subscriber_db, 
   @subscription_type = N'Push', 
   @sync_type = N'automatic', 
   @article = N'all', 
   @update_mode = N'read only', 
   @subscriber_type = 0

   exec sp_addpushsubscription_agent 
   @publication = N'SnapshotRepl', 
   @subscriber = @subscriber,
   @subscriber_db = @subscriber_db, 
   @subscriber_security_mode = 0, 
   @subscriber_login = @subscriberLogin,
   @subscriber_password = @subscriberPassword,
   @frequency_type = 1,
   @frequency_interval = 0, 
   @frequency_relative_interval = 0, 
   @frequency_recurrence_factor = 0, 
   @frequency_subday = 0, 
   @frequency_subday_interval = 0, 
   @active_start_time_of_day = 0, 
   @active_end_time_of_day = 0, 
   @active_start_date = 0, 
   @active_end_date = 19950101
   GO
   ```

1. Führen Sie die Replikations-Agent-Aufträge aus. Führen Sie die folgende Abfrage aus, um eine Liste der Aufträge zu erhalten:

   ```sql
   SELECT name, date_modified FROM msdb.dbo.sysjobs order by date_modified desc
   ```

   Führen Sie den Momentaufnahme- Replikationsauftrag zum Generieren der Momentaufnahme aus:

   ```sql
   USE msdb;   
   --generate snapshot of publications, for example
   EXEC dbo.sp_start_job N'PUBLISHER-PUBLICATION-SnapshotRepl-1'
   GO
   ```

   Führen Sie den Momentaufnahme- Replikationsauftrag zum Generieren der Momentaufnahme aus:

   ```sql
   USE msdb;
   --distribute the publication to subscriber, for example
   EXEC dbo.sp_start_job N'DISTRIBUTOR-PUBLICATION-SnapshotRepl-SUBSCRIBER'
   GO
   ```

1. Stellen Sie eine Verbindung mit dem Abonnenten her, und fragen Sie replizierte Daten ab.    

   Überprüfen Sie auf dem Abonnenten, ob die Replikation funktioniert, indem Sie die folgende Abfrage ausführen:

   ```sql
   SELECT * from [Sales].[dbo].[CUSTOMER]
   ```

In diesem Tutorial konfigurierten Sie die Replikation von SQL Server-Momentaufnahmen unter Linux mit zwei Instanzen von SQL Server mithilfe von Transact-SQL.

> [!div class="checklist"]
> * Aktivieren von SQL Server-Replikations-Agents unter Linux
> * Erstellen einer Beispieldatenbank
> * Konfigurieren des Momentaufnahmeordners für den Zugriff durch SQL Server-Agents
> * Konfigurieren des Verteilers
> * Konfigurieren des Herausgebers
> * Konfigurieren von Veröffentlichungen und Artikeln
> * Konfigurieren des Abonnenten 
> * Ausführen der Replikationsaufträge

## <a name="see-also"></a>Weitere Informationen

Ausführliche Informationen über Replikation finden Sie unter [SQL Server-Replikation](../relational-databases/replication/sql-server-replication.md).

## <a name="next-steps"></a>Nächste Schritte

[Konzepte: SQL Server-Replikation unter Linux](sql-server-linux-replication.md)

[Gespeicherte Prozeduren für die Replikation](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).
