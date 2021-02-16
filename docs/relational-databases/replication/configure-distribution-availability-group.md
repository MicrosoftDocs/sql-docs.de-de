---
title: Konfigurieren einer Verteilungsdatenbank in einer Verfügbarkeitsgruppe
description: In diesem Artikel erfahren Sie, wie Sie die Verteilungsdatenbank für die SQL Server-Replikation mit einer Always On-Verfügbarkeitsgruppe konfigurieren.
ms.custom: seo-lt-2019
ms.date: 01/16/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], distribution
- distribution configuration [SQL Server replication]
- remote Distributors [SQL Server replication]
- transactional replication, configuring distribution
- distribution databases [SQL Server replication], sizing
- Distributors [SQL Server replication], configuring
- distribution databases [SQL Server replication], about distribution databases
- distribution databases [SQL Server replication]
- merge replication [SQL Server replication], configuring distribution
ms.assetid: 94d52169-384e-4885-84eb-2304e967d9f7
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 36e2f12793e08211e285c907f26d99e0fcc9262d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348005"
---
# <a name="set-up-replication-distribution-database-in-always-on-availability-group"></a>Einrichten der Verteilungsdatenbank für die Replikation in einer Always On-Verfügbarkeitsgruppe
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Dieser Artikel beschreibt, wie Sie eine SQL Server-Verteilungsdatenbank für die Replikation in einer Always On-Verfügbarkeitsgruppe einrichten.

Ab SQL Server 2017 CU6 und SQL Server 2016 SP2-CU3 werden Verteilungsdatenbanken für die Replikation in Verfügbarkeitsgruppen folgendermaßen unterstützt:

- Die Verfügbarkeitsgruppe der Verteilungsdatenbank muss über einen Listener verfügen. Wenn der Verleger den Verteiler hinzufügt, wird der Listenername als Verteilername verwendet.
- Bei der Erstellung von Replikationsaufträgen dient der Listenername als Verteilername. Replikationsmomentaufnahme-, Protokolllese- und Verteilungs-Agent-Aufträge (Pushabonnementaufträge), die auf dem Verteilungsserver erstellt wurden, werden in allen sekundären Replikaten der Verfügbarkeitsgruppe für die Verteilungsdatenbank erstellt.
 >[!NOTE]
 >Verteilungs-Agent-Aufträge für Pullabonnements werden auf dem Abonnentenserver, nicht auf dem Verteilungsserver erstellt. 
- Ein neuer Auftrag überwacht den Status der Verteilungsdatenbanken (primäres oder sekundäres Replikat in der Verteilungsgruppe) und deaktiviert oder aktiviert die Replikationsaufträge entsprechend.

Nachdem eine Verteilungsdatenbank gemäß der nachfolgenden Anleitung in der Verfügbarkeitsgruppe konfiguriert wurde, können die Replikationskonfiguration und die Aufträge zur Laufzeit ordnungsgemäß vor und nach dem Failover auf die Verteilungsdatenbank ausgeführt werden.

## <a name="supported-scenarios"></a>Unterstützte Szenarios

- Verteilungsdatenbank konfigurieren, die der Verfügbarkeitsgruppe hinzugefügt werden soll
- Replikation, z.B. Veröffentlichungen und Abonnements, vor und nach einem Failover auf die Verfügbarkeitsgruppe konfigurieren
- Replikationsaufträge funktionieren vor und nach einem Failover
- Replikation auf dem Verteiler und Verleger entfernen, wenn sich die Verteilungsdatenbank in der Verfügbarkeitsgruppe befindet
- Knoten zur vorhandenen Verteilungsdatenbank hinzufügen oder daraus entfernen
- Ein Verteiler kann über mehrere Verteilungsdatenbanken verfügen. Jede Verteilungsdatenbank kann sich in einer eigenen oder gar keiner Verfügbarkeitsgruppe befinden. Mehrere Verteilungsdatenbanken können sich eine Verfügbarkeitsgruppe teilen.
- Verleger und Verteiler müssen sich auf separaten SQL Server-Instanzen befinden.
- Wenn der Listener für die Verfügbarkeitsgruppe, die die Verteilungsdatenbank hostet, so konfiguriert ist, dass kein Standardport verwendet wird, müssen Sie einen Alias für den Listener und den Port einrichten.

## <a name="limitations-or-exclusions"></a>Einschränkungen oder Ausschlüsse

- Lokale Verteiler werden nicht unterstützt. Verleger und Verteiler müssen sich beispielsweise auf verschiedenen SQL Server-Instanzen befinden. Diese Instanzen können auf denselben Knotengruppen gehostet werden.  Ein Verleger, der sich selbst als Verteiler nutzt (ein sogenannter lokaler Verteiler) kann keine Verteilungsdatenbanken in Verfügbarkeitsgruppen unterstützen.
- Oracle-Verleger werden nicht unterstützt.
- Die Mergereplikation wird nicht unterstützt.
- Die Transaktionsreplikation mit sofortigen oder verzögerten Updates des Abonnenten wird nicht unterstützt.
- Die Peer-zu-Peer-Replikation wird nicht unterstützt.
- Alle SQL Server 2017-Instanzen, die Verteilungsdatenbank-Replikate hosten, müssen SQL Server 2017 CU 6 oder höher entsprechen. 
- Alle SQL Server 2016-Instanzen, die Verteilungsdatenbank-Replikate hosten, müssen SQL Server 2016 SP2 CU3 oder höher entsprechen.
- Alle SQL Server-Instanzen, die Verteilungsdatenbankreplikate hosten, müssen bis auf das kleine Zeitfenster, in dem das Upgrades stattfindet, jederzeit dieselbe Version aufweisen.
- Die Verteilungsdatenbank muss sich im vollständigen Wiederherstellungsmodus befinden.
- Konfigurieren Sie für die Wiederherstellung und zur Aktivierung der Kürzung von Transaktionsprotokollen vollständige und Transaktionsprotokollsicherungen.
- Für die Verfügbarkeitsgruppe der Verteilungsdatenbank muss ein Listener konfiguriert sein.
- Sekundäre Replikate in einer Verfügbarkeitsgruppe einer Verteilungsdatenbank können synchron oder asynchron sein. Der synchrone Modus wird jedoch empfohlen und bevorzugt.
- Die bidirektionale Transaktionsreplikation wird nicht unterstützt.
- SSMS zeigt für Verteilungsdatenbanken nicht die Status „Wird synchronisiert“ bzw. „Synchronisiert“ an, wenn eine Verteilungsdatenbank zu einer Verfügbarkeitsgruppe hinzugefügt wird.


   >[!NOTE]
   >Vor dem Ausführen von gespeicherten Replikationsprozeduren für das sekundäre Replikat (z.B. `sp_dropdistpublisher`, `sp_dropdistributiondb`, `sp_dropdistributor`, `sp_adddistributiondb`, `sp_adddistpublisher`) sollten Sie sicherstellen, dass das Replikat vollständig synchronisiert wurde.

- Alle sekundären Replikate in der Verfügbarkeitsgruppe einer Verteilungsdatenbank sollten lesbar sein. Wenn ein sekundäres Replikat nicht lesbar ist, ist der Zugriff auf die Verteilereigenschaften für das bestimmte sekundäre Replikat in SQL Server Management Studio nicht möglich. Die Replikation funktioniert jedoch weiterhin ordnungsgemäß. 
- Alle Knoten in der Verfügbarkeitsgruppe einer Verteilungsdatenbank müssen für den SQL Server-Agent dasselbe Domänenkonto verwenden, und das Domänenkonto muss auf jedem Knoten über dieselben Berechtigungen verfügen.
- Wenn einer der Replikations-Agents unter einem Proxykonto ausgeführt wird, muss das Proxykonto auf jedem Knoten in der Verfügbarkeitsgruppe der Verteilungsdatenbank vorhanden sein und muss auf jedem Knoten dieselben Berechtigung haben.
- Nehmen Sie in allen Replikaten, die Teil der Verfügbarkeitsgruppe der Verteilungsdatenbank sind, Änderungen an den Eigenschaften des Verteilers oder der Verteilungsdatenbank vor.
- Nehmen Sie in allen Replikaten, die Teil der Verfügbarkeitsgruppe der Verteilungsdatenbank sind, mithilfe von gespeicherten msdb-Prozeduren oder SQL Server Management Studio Änderungen an Replikationsaufträgen vor.
- Die Konfiguration des Verteilers auf dem Verleger muss mithilfe von Skripts ausgeführt werden. Der Replikations-Assistent kann nicht verwendet werden. Replikations-Assistenten und Eigenschaftenblätter, die anderen Zwecken dienen, werden unterstützt.
- Die Konfiguration von Verfügbarkeitsgruppen für Verteilungsdatenbanken kann nur mithilfe von Skripts erfolgen.
- Die Einrichtung von Verteilungsdatenbanken in einer Verfügbarkeitsgruppe erfordert eine neue Replikationskonfiguration. Das Verschieben einer vorhandenen Verteilungsdatenbank in eine Verfügbarkeitsgruppe wird nicht unterstützt. Sobald eine Verteilungsdatenbank aus einer Verfügbarkeitsgruppe entfernt wurde, kann sie nicht mehr als eine gültige Verteilungsdatenbank fungieren und sollte gelöscht werden.

## <a name="configuration-architecture"></a>Konfigurationsarchitektur

Die folgenden Servernamen und Einstellungen werden in den Beispielen in diesem Artikel verwendet.

- DIST1, DIST2 und DIST3 sind Verteilerserver.
- PUB ist der Verlegerserver.
- Nach der Erstellung der Verfügbarkeitsgruppe der Verteilungsdatenbank lautet der Name des Listeners DISTLISTENER.
- DIST1 ist das erste primäre Replikat der Verfügbarkeitsgruppe der Verteilungsdatenbank.

## <a name="configure-distributor-distribution-database-and-publisher"></a>Konfigurieren von Verteiler, Verteilungsdatenbank und Verteiler

In diesem Beispiel werden ein neuer Verteiler und Verleger konfiguriert, und die Verteilungsdatenbank wird einer Verfügbarkeitsgruppe hinzugefügt.

### <a name="distributors-workflow"></a>Workflow des Verteilers

1. Konfigurieren Sie DIST1, DIST2 und DIST3 mit `sp_adddistributor @@servername` als Verteiler. Geben Sie das Kennwort für `distributor_admin` über `@password` an. `@password` sollte für DIST1, DIST2 und DIST3 identisch sein.
2. Erstellen Sie die Verteilungsdatenbank auf DIST1 mit `sp_adddistributiondb`. Der Name der Verteilungsdatenbank lautet `distribution`. Ändern Sie den Wiederherstellungsmodus der `distribution`-Datenbank von einfach in vollständig.
3. Erstellen Sie eine Verfügbarkeitsgruppe für die `distribution`-Datenbank mit Replikaten auf DIST1, DIST2 und DIST3. Alle Replikate sollten vorzugsweise synchron sein. Konfigurieren Sie die sekundären Replikate so, dass sie lesbar sind oder das Lesen zulassen. Wenn sich die Verteilungsdatenbanken in der Verfügbarkeitsgruppe befinden, ist DIST1 das primäre Replikat und DIST2 und DIST3 sind die sekundären Replikate.
4. Konfigurieren Sie einen Listener namens `DISTLISTENER` für die Verfügbarkeitsgruppe.
5. Konfigurieren Sie für die Wiederherstellung und zur Aktivierung der Kürzung von Transaktionsprotokollen vollständige und Transaktionsprotokollsicherungen.
6. Führen Sie auf DIST2 und DIST3 Folgendes aus:

   ```sql
   sp_adddistributiondb 'distribution'
   ```

1. Um `PUB` als Verleger auf DIST1 hinzuzufügen, führen Sie Folgendes aus:
   
   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   Der Wert von `@working_directory` sollte ein Netzwerkpfad sein, der nicht von DIST1, DIST2 und DIST3 abhängt.

1. Führen Sie auf DIST2 und DIST3 den folgenden Befehl aus, wenn das Replikat als sekundäres Replikat lesbar ist:  

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   Wenn ein Replikat nicht als sekundäres Replikat lesbar ist, führen Sie ein Failover durch, damit es zum primären Replikat wird. Führen Sie dann den folgenden Befehl aus: 

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```
   
   Der Wert von `@working_directory` sollte mit dem im vorherigen Schritt identisch sein.

### <a name="publisher-workflow"></a>Workflow des Verlegers

Um den Verfügbarkeitsgruppenlistener der `distribution`-Datenbank als Verteiler hinzuzufügen, führen Sie auf PUB Folgendes aus: 

   ```sql
   sp_adddistributor @distributor = 'DISTLISTENER', @password = <distributor_admin password> 
   ```

   Der Wert von @password sollte mit dem übereinstimmen, der angegeben wurde, als die Verteiler im Verteiler-Workflow konfiguriert wurden.

## <a name="remove-distributor-and-publisher"></a>Entfernen von Verteiler und Verleger

In diesem Beispiel werden Verleger und Verteiler entfernt, während sich die Verteilungsdatenbank in der Verfügbarkeitsgruppe befindet.

### <a name="publisher-workflow"></a>Workflow des Verlegers

Löschen Sie auf PUB alle Abonnements und Veröffentlichungen für diesen Verleger, und rufen Sie `sp_dropdistributor` auf.

### <a name="distributors-workflow"></a>Workflow des Verteilers

In diesem Beispiel ist DIST1 das aktuelle primäre Replikat in der Verfügbarkeitsgruppe der `distribution`-Datenbank. DIST2 und DIST3 sind die sekundären Replikate.

1. Führen Sie auf DIST2 und DIST3 Folgendes aus:

   ```sql
   sp_dropdistpublisher 'PUB',  @no_checks = 1
   ```

1. Führen Sie auf DIST1 Folgendes aus:

   ```sql
   sp_dropdistpublisher 'PUB'
   ```

1. Löschen Sie die Verfügbarkeitsgruppe.
2. Ändern Sie auf DIST2 und DIST3 die `distribution`-Datenbank in den Modus „read_write“, indem Sie die Datenbank mithilfe der Wiederherstellung wiederherstellen.
   
   ```sql
   RESTORE DATABASE distribution WITH RECOVERY, KEEP_REPLICATION
   ```

1. Um die `distribution`-Datenbank zu löschen und das Momentaufnahmeverzeichnis beizubehalten, führen Sie Folgendes aus: 

   ```sql
   sp_dropdistributiondb 'distribution' , @former_ag_secondary=1
   ```

  Diese Prozedur entfernt alle zurückbleibenden Aufträge auf diesem Replikat.

1. Um die `distribution`-Datenbank auf DIST1 zu löschen, führen Sie Folgendes aus:

   ```sql
   sp_dropdistributiondb 'distribution'
   ``` 

1. Wenn keine anderen Verteilungsdatenbanken in der Verfügbarkeitsgruppe vorhanden sind, führen Sie `sp_dropdistributor` auf DIST1, DIST2 und DIST3 aus.

## <a name="add-a-replica-to-distribution-database-ag"></a>Hinzufügen eines Replikats zu der Verfügbarkeitsgruppe der Verteilungsdatenbank

In diesem Beispiel wird ein neuer Verteiler zu einer vorhandenen Replikationskonfiguration mit einer Verteilungsdatenbank in der Verfügbarkeitsgruppe hinzugefügt. In diesem Beispiel befindet sich in der Verfügbarkeitsgruppe eine vorhandene Verteilungsdatenbank. DIST1 und DIST2 sind die Verteiler, `distribution` ist die Verteilungsdatenbank in der Verfügbarkeitsgruppe, und PUB ist der Verleger. Fügen Sie DIST3 der Verfügbarkeitsgruppe als Replikat hinzu.

### <a name="distributors-workflow"></a>Workflow des Verteilers

1. DIST3 sollte mit `sp_adddistributor @@servername` als Verteiler konfiguriert werden. Das Kennwort für `distributor_admin` sollte mithilfe des @password-Parameters angegeben werden. Das Kennwort sollte mit dem übereinstimmen, was für DIST1 und DIST2 angegeben wurde.
2. Fügen Sie der Verfügbarkeitsgruppe für die aktuelle Verteilungsdatenbank DIST3 hinzu.
3. Führen Sie auf DIST3 Folgendes aus:

   ```sql
   sp_adddistributiondb 'distribution'
   ```

4. Führen Sie auf DIST3 den folgenden Befehl aus, wenn das Replikat als sekundäres Replikat lesbar ist: 

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   Wenn das Replikat nicht als sekundäres Replikat lesbar ist, führen Sie ein Failover durch, damit es zum primären Replikat wird. Führen Sie dann den folgenden Befehl aus:

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```
   
   Der Wert von `@working_directory` sollte mit dem übereinstimmen, was für DIST1 und DIST2 angegeben wurde.

4. Auf DIST3 müssen Sie Verbindungsserver zu den Abonnenten neu erstellen.

## <a name="remove-a-replica-from-distribution-database-ag"></a>Entfernen eines Replikats aus der Verfügbarkeitsgruppe der Verteilungsdatenbank

In diesem Beispiel wird ein Verteiler aus einer aktuellen Verfügbarkeitsgruppe der Verteilungsdatenbank entfernt, wobei die übrigen Replikate in der Verteilungsdatenbank nicht betroffen sind. In diesem Beispiel befindet sich eine Verteilungsdatenbank in der Verfügbarkeitsgruppe. DIST1, DIST2 und DIST3 sind die Verteiler, `distribution` ist die Verteilungsdatenbank der Verfügbarkeitsgruppe, und PUB ist der Verleger. Entfernen Sie DIST3 aus der Verfügbarkeitsgruppe.

### <a name="distributors-workflow"></a>Workflow des Verteilers

1. Stellen Sie sicher, dass DIST3 ein sekundäres Replikat für die Verfügbarkeitsgruppe der `distribution`-Datenbank ist.
2. Entfernen Sie DIST3 aus der Verfügbarkeitsgruppe der `distribution`-Datenbank.
3. Ändern Sie auf DIST3 die `distribution`-Datenbank in den Modus „read_write“, indem Sie die Datenbank mithilfe der Wiederherstellung wiederherstellen. Führen Sie beispielsweise den folgenden Befehl aus:  
   
   ```sql
   RESTORE DATABASE distribution WITH RECOVERY, KEEP_REPLICATION.
   ```
   
1. Um alle verwaisten Aufträge zu entfernen, führen Sie auf DIST3 Folgendes aus: 

   ```sql
   sp_dropdistpublisher 'PUB', @no_checks = 1
   ```

1. Führen Sie auf DIST3 Folgendes aus:

   ```sql
   sp_dropdistributiondb 'distribution', @former_ag_secondary=1
   ```

1. Führen Sie auf DIST3 Folgendes aus: 

   ```sql
   sp_dropdistributor
   ```

## <a name="remove-a-publisher-from-distribution-database-ag"></a>Entfernen eines Verlegers aus der Verfügbarkeitsgruppe der Verteilungsdatenbank

In diesem Beispiel wird ein Verleger aus der aktuellen Verfügbarkeitsgruppe der Verteilungsdatenbank eines Verteilers entfernt, wobei die übrigen Verleger, die von dieser Verteilungsdatenbank bedient werden, nicht betroffen sind. In diesem Beispiel hat eine vorhandene Konfiguration eine Verteilungsdatenbank in einer Verfügbarkeitsgruppe. DIST1, DIST2 und DIST3 sind die Verteiler, `distribution` ist die Verteilungsdatenbank der Verfügbarkeitsgruppe, und PUB1 und PUB2 sind die Verleger, die von der `distribution`-Datenbank bedient werden. In diesem Beispiel wird PUB1 aus diesen Verteilern entfernt.

### <a name="publisher-workflow"></a>Workflow des Verlegers

Löschen Sie auf PUB1 alle Abonnements und Veröffentlichungen für diesen Verleger, und rufen Sie `sp_dropdistributor` auf.

### <a name="distributor-workflow"></a>Workflow des Verteilers

DIST1 ist das aktuelle primäre Replikat in der Verfügbarkeitsgruppe der `distribution`-Datenbank.

1. Führen Sie auf DIST2 und DIST3 Folgendes aus:

   ```sql
   sp_dropdistpublisher 'PUB1',  @no_checks = 1
   ```

1. Führen Sie auf DIST1 Folgendes aus:

   ```sql
   sp_dropdistpublisher 'PUB1'
   ```

1. Zu diesem Zeitpunkt kann es verwaiste Aufträge geben, die im Zusammenhang mit PUB1 auf DIST2 oder DIST3 stehen. Wann immer ein Failover auf DIST2 und DIST3 eintritt, werden alle verwaisten Aufträge, die zu allen Veröffentlichungen von PUB1 gehören, vom `Monitor and sync replication agent jobs`-Auftrag entfernt.

## <a name="add-subscription"></a>Abonnement hinzufügen

In diesem Beispiel geht es um die ordnungsgemäße Konfiguration der Abonnenteninformationen auf Verteilern. In diesem Beispiel wird ein Abonnent hinzugefügt. DIST1 ist das aktuelle primäre Replikat in der Verfügbarkeitsgruppe und DIST2 und DIST3 die entsprechenden sekundären Replikate. Der Name des Abonnenten lautet SUB.

### <a name="publisher-workflow"></a>Workflow des Verlegers

Fügen Sie auf PUB einen Abonnementen so hinzu, wie Sie den Abonnenten `SUB` normalerweise hinzufügen würden.

### <a name="distributor-workflow"></a>Workflow des Verteilers

Fügen Sie auf DIST2 und DIST3 einen Verbindungsserver für SUB hinzu, wenn er zuvor noch nicht für DIST2 oder DIST3 registriert wurde. Es folgt ein Beispiel mit T-SQL-Code für die Erstellung eines Verbindungsservers:

   ```sql 
   EXEC master.dbo.sp_addlinkedserver@server =N'SUB', @srvproduct=N'SQL Server'
   /* For security reasons the linked server remote logins password is changed with ######## */
   EXEC master.dbo.sp_addlinkedsrvlogin@rmtsrvname=N'SUB', @useself=N'True',@locallogin=NULL,@rmtuser=NULL,@rmtpassword=NULL
   ```

## <a name="add-a-pull-subscription"></a>Hinzufügen eines Pullabonnements

### <a name="subscriber-workflow"></a>Workflow des Abonnenten

Ein Pullabonnement für eine Veröffentlichung in der Verteilungsdatenbank in einer Verfügbarkeitsgruppe wird mithilfe des Namens des Verfügbarkeitsgruppenlisteners im `@distributor`-Parameter von `sp_addpullsubscription_agent` eingerichtet.

## <a name="sample-t-sql-create-distribution-db-in-ag"></a>Beispiel-T-SQL-Code für die Erstellung einer Datenbank in einer Verfügbarkeitsgruppe

Das folgende Skript aktiviert eine Verteilungsdatenbank in einer Verfügbarkeitsgruppe. 

```sql
--- WorkFlow to Enable Distribution Database In AG.

-- SECTION 1 ---- CONFIGURE THE DISTRIBUTOR SERVERS

-- Step1 - Configure the Distribution DB nodes (AG Replicas) to act as a distributor
:Connect SQLNode1
sp_adddistributor @distributor = @@ServerName, @password = 'Pass@word1'
Go 
:Connect SQLNode2
sp_adddistributor @distributor = @@ServerName, @password = 'Pass@word1'
Go

-- Step2 - Configure the Distribution Database
:Connect SQLNode1
USE master
EXEC sp_adddistributiondb @database = 'DistributionDB', @security_mode = 1;
GO
Alter Database [DistributionDB] Set Recovery Full
Go
Backup Database [DistributionDB] to Disk = 'Nul'
Go
-- Step 3 - Create AG for the Distribution DB.
:Connect SQLNode1
USE [master]
GO
CREATE ENDPOINT [Hadr_endpoint] 
    STATE=STARTED
    AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)   
    FOR DATA_MIRRORING (ROLE = ALL, AUTHENTICATION = WINDOWS NEGOTIATE
, ENCRYPTION = REQUIRED ALGORITHM AES)
GO

:Connect SQLNode2
USE [master]
GO
CREATE ENDPOINT [Hadr_endpoint] 
    STATE=STARTED
    AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)   
    FOR DATA_MIRRORING (ROLE = ALL, AUTHENTICATION = WINDOWS NEGOTIATE
, ENCRYPTION = REQUIRED ALGORITHM AES)
GO

:Connect SQLNode1
-- Create the Availability Group
CREATE AVAILABILITY GROUP [DistributionDB_AG]
FOR DATABASE [DistributionDB]
REPLICA ON 'SQLNode1'
WITH (ENDPOINT_URL = N'TCP://SQLNode1.contoso.com:5022', 
         FAILOVER_MODE = AUTOMATIC, 
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
         BACKUP_PRIORITY = 50, 
         SECONDARY_ROLE(ALLOW_CONNECTIONS = ALL), 
         SEEDING_MODE = AUTOMATIC),
N'SQLNode2' WITH (ENDPOINT_URL = N'TCP://SQLNode2.contoso.com:5022', 
     FAILOVER_MODE = AUTOMATIC, 
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
     BACKUP_PRIORITY = 50, 
     SECONDARY_ROLE(ALLOW_CONNECTIONS = ALL), 
     SEEDING_MODE = AUTOMATIC);
 GO


:Connect SQLNode2
ALTER AVAILABILITY GROUP [DistributionDB_AG] JOIN
GO  
ALTER AVAILABILITY GROUP [DistributionDB_AG] GRANT CREATE ANY DATABASE
Go

--STEP4 - Create the Listener for the Availability Group. This is very important.
:Connect SQLNode1

USE [master]
GO
ALTER AVAILABILITY GROUP [DistributionDB_AG]
ADD LISTENER N'DistributionDBList' (
WITH IP
((N'10.0.0.8', N'255.255.255.0')) , PORT=1500);
GO

-- STEP 5 - Enable SQLNode2 also as a Distributor
:CONNECT SQLNODE2
EXEC sp_adddistributiondb @database = 'DistributionDB', @security_mode = 1;
GO

--STEP 6 - On all Distributor Nodes Configure the Publisher Details 
:CONNECT SQLNODE1
EXEC sp_addDistPublisher @publisher = 'SQLNode4', @distribution_db = 'DistributionDB', 
    @working_directory = '\\sqlfileshare\Dist_Work_Directory\'
GO
:CONNECT SQLNODE2
EXEC sp_addDistPublisher @publisher = 'SQLNode4', @distribution_db = 'DistributionDB', 
    @working_directory = '\\sqlfileshare\Dist_Work_Directory\'
GO

-- SECTION 2 ---- CONFIGURE THE PUBLISHER SERVER
:CONNECT SQLNODE4
EXEC sp_addDistributor @distributor = 'DistributionDBList', -- Listener for the Distribution DB.    
    @password = 'Pass@word1'
Go

-- SECTION 3 ---- CONFIGURE THE SUBSCRIBERS 
-- On Publisher, create the publication as one would normally do.
-- On the Secondary replicas of the Distribution DB, add the Subscriber as a linked server.
:CONNECT SQLNODE2
EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE5', @srvproduct=N'SQL Server'
 /* For security reasons the linked server remote logins password is changed with ######## */
EXEC master.dbo.sp_addlinkedsrvlogin @rmtsrvname=N'SQLNODE5',@useself=N'True',@locallogin=NULL,@rmtuser=NULL,@rmtpassword=NULL 
```

## <a name="see-also"></a>Weitere Informationen  
 [Veröffentlichen von Daten und Datenbankobjekten](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Schützen des Verteilers](../../relational-databases/replication/security/secure-the-distributor.md)  
  
## <a name="next-steps"></a>Nächste Schritte
 [Anzeigen und Ändern der Verteiler- und Verlegereigenschaften](view-and-modify-distributor-and-publisher-properties.md)  
 [Deaktivieren der Veröffentlichung und Verteilung](disable-publishing-and-distribution.md)  
 [Aktivieren einer Datenbank für die Replikation (SQL Server Management Studio)](enable-a-database-for-replication-sql-server-management-studio.md) 
