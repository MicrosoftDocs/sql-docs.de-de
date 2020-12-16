---
title: Konfigurieren der Veröffentlichung und der Verteilung | Microsoft Dokumentation
description: Hier erfahren Sie, wie Sie die Veröffentlichung und Verteilung in SQL Server mithilfe von SQL Server Management Studio, Transact-SQL oder Replikationsverwaltungsobjekten konfigurieren.
ms.custom: ''
ms.date: 09/23/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], distribution
- distribution configuration [SQL Server replication]
- publishing [SQL Server replication], configuring
ms.assetid: 3cfc8966-833e-42fa-80cb-09175d1feed7
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 72e6a2ed252e895f1e55a7263c089e8e618ec115
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484912"
---
# <a name="configure-publishing-and-distribution"></a>Konfigurieren der Veröffentlichung und der Verteilung
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
 In diesem Thema wird beschrieben, wie die Veröffentlichung und die Verteilung in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]oder Replikationsverwaltungsobjekten (RMO) konfiguriert werden.

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen 

###  <a name="security"></a><a name="Security"></a> Sicherheit 
Weitere Informationen finden Sie unter [Anzeigen und Ändern von Replikationssicherheitseinstellungen](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).

##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio 
Sie konfigurieren die Verteilung mit dem Assistenten für neue Veröffentlichung oder mit dem Verteilungskonfigurations-Assistenten. Rufen Sie nach der Konfiguration des Verteilers das Dialogfeld **Verteilereigenschaften – \<Distributor>** auf, und ändern Sie die Eigenschaften. Verwenden Sie den Verteilungskonfigurations-Assistenten, wenn Sie einen Verteiler so konfigurieren möchten, dass die Mitglieder der festen Datenbankrollen `db_owner` Veröffentlichungen erstellen können, oder wenn Sie einen Remoteverteiler konfigurieren möchten, bei dem es sich nicht um einen Verleger handelt.

#### <a name="to-configure-distribution"></a>So konfigurieren Sie die Verteilung 

1. Stellen Sie in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]eine Verbindung mit dem Server her, der als Verteiler fungieren soll, und erweitern Sie den Serverknoten (in vielen Fällen handelt es sich beim Verleger und beim Verteiler um denselben Server).

2. Klicken Sie mit der rechten Maustaste auf den Ordner **Replikation** , und klicken Sie dann auf **Verteilung konfigurieren**.

3. Befolgen Sie die Anweisungen im Verteilungskonfigurations-Assistenten, um folgende Vorgänge auszuführen: 

  - Auswählen eines Verteilers. Wählen Sie zum Verwenden eines lokalen Verteilers **Servername als seinen eigenen Verteiler verwenden. SQL Server erstellt eine Verteilungsdatenbank und ein Protokoll.** Aktivieren Sie zum Verwenden eines Remoteverteilers **Folgenden Server als Verteiler verwenden**, und wählen Sie dann einen Server aus. Der Server muss bereits als Verteiler konfiguriert sein, und auf dem Verleger muss die Verwendung des Verteilers aktiviert sein. Weitere Informationen hierzu finden Sie unter [Aktivieren eines Remoteverlegers auf einem Verteiler &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/enable-a-remote-publisher-at-a-distributor-sql-server-management-studio.md).

     Wenn Sie einen Remoteverteiler auswählen, müssen Sie auf der Seite **Administratorkennwort** ein Kennwort für Verbindungen eingeben, die zwischen dem Verleger und dem Verteiler hergestellt werden. Dieses Kennwort muss mit dem Kennwort übereinstimmen, dass bei der Aktivierung des Verlegers auf dem Remoteverteiler angegeben wurde.

  - Geben Sie einen Stammordner der Momentaufnahmen an (für einen lokalen Verteiler). Der Momentaufnahmeordner ist lediglich ein von Ihnen freigegebenes Verzeichnis. Agents, die aus diesem Ordner lesen bzw. in den Ordner schreiben, benötigen ausreichende Zugriffsberechtigungen. Jeder Verleger, der diesen Verteiler verwendet, erstellt einen Unterordner des Stammordners, und jede Veröffentlichung erstellt Unterordner des Verlegerordners, in dem die Momentaufnahmedateien gespeichert werden. Weitere Informationen zum angemessenen Sichern des Ordners finden Sie unter [Sichern des Momentaufnahmeordners](../../relational-databases/replication/security/secure-the-snapshot-folder.md).

  - Geben Sie die Verteilungsdatenbank (für einen lokalen Verteiler) an. In der Verteilungsdatenbank werden Metadaten und Verlaufsdaten für alle Replikations- und Transaktionstypen für die Transaktionsreplikation gespeichert.

  - Optional können Sie weitere Verleger für die Verwendung des Verteilers aktivieren. Wenn die Verwendung des Verteilers auf anderen Verlegern aktiviert ist, müssen Sie auf der Seite **Verteilerkennwort** ein Kennwort für Verbindungen eingeben, die zwischen diesen Verlegern und dem Verteiler hergestellt werden.

  - Erstellen Sie optional ein Skript für die Konfigurationseinstellungen. Weitere Informationen finden Sie unter [Scripting Replication](../../relational-databases/replication/scripting-replication.md).

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL 
Die Replikationsveröffentlichung und -verteilung kann mit gespeicherten Replikationsprozeduren programmgesteuert konfiguriert werden.
### <a name="to-configure-publishing-using-a-local-distributor"></a>So konfigurieren Sie die Veröffentlichung mit einem lokalen Verteiler
1. Führen Sie [sp_get_distributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-get-distributor-transact-sql.md) aus, um zu ermitteln, ob der Server bereits als Verteiler konfiguriert ist.

  - Wenn der Wert von `installed` im Resultset gleich `0` ist, führen Sie [sp_adddistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) auf dem Verteiler für die Masterdatenbank aus.

  - Wenn der Wert von `distribution db installed` im Resultset gleich `0` ist, führen Sie [sp_adddistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md) auf dem Verteiler für die Masterdatenbank aus. Geben Sie den Namen der Verteilungsdatenbank für `@database` an. Optional können Sie die maximale Transaktionsbeibehaltungsdauer für `@max_distretention` und die Verlaufsbeibehaltungsdauer für `@history_retention` angeben. Wenn eine neue Datenbank erstellt wird, geben Sie die gewünschten Eigenschaftenparameter für die Datenbank an.

2. Führen Sie auf dem Verteiler, der zugleich der Verleger ist, [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md) aus, und geben Sie dabei die UNC-Freigabe, die als Standardmomentaufnahmeordner verwendet werden soll, für `@working_directory` an.

   Verwenden Sie ein Azure-Speicherkonto für `@working_directory` und den Speicherzugriffsschlüssel für `@storage_connection_string` als Verteiler in SQL Managed Instance. 

3. Führen Sie [sp_replicationdboption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) auf dem Verleger aus. Geben Sie die zu veröffentlichende Datenbank für `@dbname`, den Replikationstyp für `@optname` und den Wert `true` für `@value` an.

#### <a name="to-configure-publishing-using-a-remote-distributor"></a>So konfigurieren Sie die Veröffentlichung mit einem Remoteverteiler 

1. Führen Sie [sp_get_distributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-get-distributor-transact-sql.md) aus, um zu ermitteln, ob der Server bereits als Verteiler konfiguriert ist.

   - Wenn der Wert von `installed` im Resultset gleich `0` ist, führen Sie [sp_adddistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) auf dem Verteiler für die Masterdatenbank aus. Geben Sie ein starkes Kennwort für `@password` an. Dieses Kennwort für das `distributor_admin`-Konto wird vom Verleger verwendet, wenn er eine Verbindung zum Verteiler herstellt.

   - Wenn der Wert von `distribution db installed` im Resultset gleich `0` ist, führen Sie [sp_adddistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md) auf dem Verteiler für die Masterdatenbank aus. Geben Sie den Namen der Verteilungsdatenbank für `@database` an. Optional können Sie die maximale Transaktionsbeibehaltungsdauer für `@max_distretention` und die Verlaufsbeibehaltungsdauer für `@history_retention` angeben. Wenn eine neue Datenbank erstellt wird, geben Sie die gewünschten Eigenschaftenparameter für die Datenbank an.

2. Führen Sie auf dem Verteiler [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md) aus, und geben Sie dabei die UNC-Freigabe, die als Standardmomentaufnahmeordner verwendet werden soll, für `@working_directory` an. Wenn der Verteiler zum Herstellen der Verbindung mit dem Verleger die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung verwendet, müssen Sie zudem den Wert `0` für `@security_mode` und die [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldeinformationen für `@login` und `@password` angeben.

   Verwenden Sie ein Azure-Speicherkonto für `@working_directory` und den Speicherzugriffsschlüssel für `@storage_connection_string` als Verteiler in SQL Managed Instance. 

3. Führen Sie auf dem Verleger für die master-Datenbank die Prozedur [sp_adddistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) aus. Geben Sie das starke Kennwort an, das in Schritt 1 für `@password` verwendet wurde. Dieses Kennwort wird vom Verleger verwendet, wenn er eine Verbindung zum Verteiler herstellt.

4. Führen Sie [sp_replicationdboption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) auf dem Verleger aus. Geben Sie die zu veröffentlichende Datenbank für `@dbname`, den Replikationstyp für `@optname` und den Wert „true“ für `@value` an.

###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Beispiel (Transact-SQL) 
Im folgenden Beispiel wird veranschaulicht, wie die Veröffentlichung und die Verteilung programmgesteuert konfiguriert werden. In diesem Beispiel werden der Name des Servers, der als Verleger konfiguriert wird, und ein lokaler Verteiler mithilfe von Skriptvariablen bereitgestellt. Die Replikationsveröffentlichung und -verteilung kann mit gespeicherten Replikationsprozeduren programmgesteuert konfiguriert werden.

[!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/configure-publishing-and_1.sql)] 

##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Verwenden von Replikationsverwaltungsobjekten (RMO) 

#### <a name="to-configure-publishing-and-distribution-on-a-single-server"></a>So konfigurieren Sie Veröffentlichung und Verteilung auf einem einzelnen Server 

1. Erstellen Sie eine Verbindung mit dem Server, indem Sie die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Klasse verwenden.

2. Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.ReplicationServer>-Klasse. Übergeben Sie <xref:Microsoft.SqlServer.Management.Common.ServerConnection> von Schritt 1.

3. Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.DistributionDatabase>-Klasse.

4. Legen Sie die <xref:Microsoft.SqlServer.Replication.DistributionDatabase.Name%2A> -Eigenschaft auf den Datenbanknamen fest, und legen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> -Eigenschaft auf die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> aus Schritt 1 fest.

5. Installieren Sie den Verteiler, indem Sie die <xref:Microsoft.SqlServer.Replication.ReplicationServer.InstallDistributor%2A> -Methode aufrufen. Übergeben Sie das <xref:Microsoft.SqlServer.Replication.DistributionDatabase> -Objekt aus Schritt 3.

6. Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.DistributionPublisher>-Klasse.

7. Legen Sie dann die folgenden Eigenschaften von <xref:Microsoft.SqlServer.Replication.DistributionPublisher>fest: 

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Name%2A> &ndash; der Name des Verlegers

  - <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> &ndash; die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> aus Schritt 1.

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.DistributionDatabase%2A> &ndash; der Name der in Schritt 5 erstellten Datenbank.

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.WorkingDirectory%2A> &ndash; die Freigabe, die verwendet wird, um auf Momentaufnahmedateien zuzugreifen.

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.PublisherSecurity%2A> : Der Sicherheitsmodus, der beim Herstellen einer Verbindung mit dem Verleger verwendet wird. <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> wird empfohlen.

8. Rufen Sie die <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Create%2A> -Methode auf.

#### <a name="to-configure-publishing-and-distribution-using-a-remote-distributor"></a>So konfigurieren Sie Veröffentlichung und Verteiler mit einem Remoteverteiler 

1. Erstellen Sie mithilfe der <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Klasse eine Verbindung zum Remoteverteilerserver.

2. Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.ReplicationServer>-Klasse. Übergeben Sie <xref:Microsoft.SqlServer.Management.Common.ServerConnection> von Schritt 1.

3. Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.DistributionDatabase>-Klasse.

4. Legen Sie die <xref:Microsoft.SqlServer.Replication.DistributionDatabase.Name%2A> -Eigenschaft auf den Datenbanknamen fest, und legen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> -Eigenschaft auf die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> aus dem Schritt 1 fest.

5. Installieren Sie den Verteiler, indem Sie die <xref:Microsoft.SqlServer.Replication.ReplicationServer.InstallDistributor%2A> -Methode aufrufen. Geben Sie ein sicheres Kennwort (das vom Verteiler für den Verbindungsaufbau mit dem Remoteverteiler verwendet wird) und das <xref:Microsoft.SqlServer.Replication.DistributionDatabase> -Objekt aus Schritt 3 an. Weitere Informationen finden Sie unter [Schützen des Verteilers](../../relational-databases/replication/security/secure-the-distributor.md).

   > `IMPORTANT!!` Benutzer sollten nach Möglichkeit dazu aufgefordert werden, Sicherheitsanmeldeinformationen zur Laufzeit anzugeben. Wenn Sie Anmeldeinformationen speichern müssen, verwenden Sie die [Kryptografiedienste](/previous-versions/aa719848(v=vs.71)) von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework.

6. Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.DistributionPublisher>-Klasse.

7. Legen Sie dann die folgenden Eigenschaften von <xref:Microsoft.SqlServer.Replication.DistributionPublisher>fest: 

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Name%2A> &ndash; der Name des lokalen Verlegerservers.

  - <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> &ndash; die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> aus Schritt 1.

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.DistributionDatabase%2A> &ndash; der Name der in Schritt 5 erstellten Datenbank.

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.WorkingDirectory%2A> &ndash; die Freigabe, die verwendet wird, um auf Momentaufnahmedateien zuzugreifen.

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.PublisherSecurity%2A> : Der Sicherheitsmodus, der beim Herstellen einer Verbindung mit dem Verleger verwendet wird. <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> wird empfohlen.

8. Rufen Sie die <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Create%2A> -Methode auf.

9. Erstellen Sie mithilfe der <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Klasse eine Verbindung zum lokalen Verlegerserver.

10. Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.ReplicationServer>-Klasse. Übergeben Sie <xref:Microsoft.SqlServer.Management.Common.ServerConnection> von Schritt 9.

11. Rufen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationServer.InstallDistributor%2A> -Methode auf. Übergeben Sie den Namen des Remoteverteilers und das Kennwort des Remoteverteilers, der in Schritt 5 angegeben wurde.

> [!IMPORTANT]
> Benutzer sollten nach Möglichkeit dazu aufgefordert werden, Anmeldeinformationen zur Laufzeit anzugeben. Wenn Sie Anmeldeinformationen speichern müssen, verwenden Sie die [Kryptografiedienste](/previous-versions/aa719848(v=vs.71)) von Windows .NET Framework.

###  <a name="example-rmo"></a><a name="PShellExample"></a> Beispiel (RMO) 
Sie können die Veröffentlichung und Verteilung mithilfe von Replikationsverwaltungsobjekten (ROM) programmgesteuert konfigurieren.

[!code-cs[HowTo#rmo_AddDistPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_adddistpub)] 

[!code-vb[HowTo#rmo_vb_AddDistPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_adddistpub)] 

## <a name="see-also"></a>Weitere Informationen 
[Anzeigen und Ändern der Verteiler- und Verlegereigenschaften](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)  
[Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
[Verteilung konfigurieren](../../relational-databases/replication/configure-distribution.md)  
[Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)  
[Konfigurieren der Replikation für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)