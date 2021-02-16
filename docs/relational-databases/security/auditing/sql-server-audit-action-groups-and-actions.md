---
title: SQL Server Audit-Aktionsgruppen und -Aktionen | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über Aktionsgruppen der Server-, Datenbank- und Überwachungsebenen und die individuellen Aktionen der SQL Server-Überwachung.
ms.custom: ''
ms.date: 07/13/2020
ms.prod: sql
ms.prod_service: security
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- audit
helpviewer_keywords:
- audit actions [SQL Server]
- audits [SQL Server], groups
- server-level audit actions [SQL Server]
- SQL Server Audit
- audit-level audit actions [SQL Server]
- database-level audit actions [SQL Server]
- audit action groups [SQL Server]
- audits [SQL Server], actions
ms.assetid: b7422911-7524-4bcd-9ab9-e460d5897b3d
author: DavidTrigano
ms.author: datrigan
ms.openlocfilehash: 0ab7abaaa4473bdc1d4a309ce3ccf0103a33a54a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100344421"
---
# <a name="sql-server-audit-action-groups-and-actions"></a>SQL Server Audit-Aktionsgruppen und -Aktionen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Die Funktion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Audit ermöglicht Ihnen, Ereignisgruppen und einzelne Ereignisse auf Server- und Datenbankebene zu überwachen. Weitere Informationen finden Sie unter [SQL Server Audit &#40;Datenbank-Engine&#41;](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Überwachungen bestehen aus null oder mehr Überwachungsaktionselementen. Bei diesen Überwachungsaktionselementen kann es sich entweder um Aktionsgruppen, wie Server_Object_Change_Group, oder um einzelne Aktionen, wie SELECT-Vorgänge in einer Tabelle, handeln.  
  
> [!NOTE]  
>  Server_Object_Change_Group enthält für jedes Serverobjekt (Datenbank oder Endpunkt) CREATE, ALTER und DROP.  
  
 Überwachungen können in die folgenden Kategorien von Aktionen eingeteilt werden:  
  
-   Serverebene. Diese Aktionen schließen Servervorgänge ein, z. B. Verwaltungsänderungen sowie Anmelde- und Abmeldevorgänge.  
  
-   Datenbankebene. Diese Aktionen umfassen DML- (Data Manipulation Language) und DDL-Vorgänge (Data Definition Language).  
  
-   Überwachungsebene. Diese Aktionen schließen Aktionen im Überwachungsprozess ein.  
  
 Einige Aktionen für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Überwachungskomponenten werden systemintern in einer spezifischen Überwachung überwacht. In diesen Fällen treten Überwachungsereignisse automatisch auf, da das Ereignis für das übergeordnete Objekt eingetreten ist.  
  
 Die folgenden Aktionen werden systemintern überwacht:  
  
-   Statusänderung der Serverüberwachung (Status wird auf EIN oder AUS festgelegt)  
  
 Die folgenden Ereignisse werden nicht systemintern überwacht:  
  
-   CREATE SERVER AUDIT SPECIFICATION  
  
-   ALTER SERVER AUDIT SPECIFICATION  
  
-   DROP SERVER AUDIT SPECIFICATION  
  
-   CREATE DATABASE AUDIT SPECIFICATION  
  
-   ALTER DATABASE AUDIT SPECIFICATION  
  
-   DROP DATABASE AUDIT SPECIFICATION  
  
 Sämtliche Überwachungen werden nach dem ersten Erstellen deaktiviert.  
  
## <a name="server-level-audit-action-groups"></a>Überwachungsaktionsgruppen auf Serverebene  
 Überwachungsaktionsgruppen auf Serverebene sind Aktionen, die Ereignisklassen der Sicherheitsüberwachung in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ähneln. Weitere Informationen finden Sie unter [Ereignisklassen in SQL Server – Referenz](../../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
 In der folgenden Tabelle werden die Überwachungsaktionsgruppen auf Serverebene beschrieben. Hier finden Sie auch, sofern vorhanden, die entsprechende Ereignisklasse in SQL Server.  
  
|Aktionsgruppenname|BESCHREIBUNG|  
|-----------------------|-----------------|  
|APPLICATION_ROLE_CHANGE_PASSWORD_GROUP|Das Ereignis wird ausgelöst, wenn ein Kennwort für eine Anwendungsrolle geändert wird. Entspricht der [Audit App Role Change Password Event Class](../../../relational-databases/event-classes/audit-app-role-change-password-event-class.md).|  
|AUDIT_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn eine Überwachung erstellt, geändert oder gelöscht wird. Das Ereignis wird ausgelöst, wenn eine Überwachungsspezifikation erstellt, geändert oder gelöscht wird. Jede Änderung an einer Überwachung wird in dieser Überwachung überwacht. Entspricht der [Audit Change Audit Event Class](../../../relational-databases/event-classes/audit-change-audit-event-class.md).|  
|BACKUP_RESTORE_GROUP|Das Ereignis wird ausgelöst, wenn ein Sicherungs- oder Wiederherstellungsbefehl ausgegeben wird. Entspricht der [Audit Backup/Restore-Ereignisklasse](../../../relational-databases/event-classes/audit-backup-and-restore-event-class.md).|  
|BATCH_COMPLETED_GROUP|Dieses Ereignis wird ausgelöst, wenn die Ausführung von Batchtexten, gespeicherten Prozeduren oder Transaktionverwaltungsvorgänge abgeschlossen wird. Es wird nach Abschluss des Batches ausgelöst und überwacht den gesamten Text des Batches oder der gespeicherten Prozedur, so wie er vom Client übermittelt wird, einschließlich des Ergebnisses. **Hinzugefügt in SQL Server 2019.**|  
|BATCH_STARTED_GROUP|Dieses Ereignis wird ausgelöst, wenn die Ausführung von Batchtexten, gespeicherten Prozeduren oder Transaktionverwaltungsvorgänge begonnen wird. Es wird vor der Ausführung ausgelöst und überwacht den gesamten Text des Batches oder der gespeicherten Prozedur, so wie er vom Client übermittelt wird. **Hinzugefügt in SQL Server 2019.**|  
|BROKER_LOGIN_GROUP|Das Ereignis wird für Berichtsüberwachungsmeldungen zur Service Broker-Transportsicherheit ausgelöst. Entspricht der [Audit Broker Login Event Class](../../../relational-databases/event-classes/audit-broker-login-event-class.md).|  
|DATABASE_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn eine Datenbank erstellt, geändert oder gelöscht wird. Das Ereignis wird immer dann ausgelöst, wenn eine Datenbank erstellt, geändert oder gelöscht wird. Entspricht der [Audit Database Management Event Class](../../../relational-databases/event-classes/audit-database-management-event-class.md).|  
|DATABASE_LOGOUT_GROUP|Dieses Ereignis wird ausgelöst, wenn sich ein Benutzer einer eigenständigen Datenbank von einer Datenbank abmeldet. Es entspricht der [Ereignisklasse Audit Logout](../../../relational-databases/event-classes/audit-logout-event-class.md).|  
|DATABASE_MIRRORING_LOGIN_GROUP|Das Ereignis wird für Berichtsüberwachungsmeldungen zur Transportsicherheit der Datenbankspiegelung ausgelöst. Entspricht der [Audit Database Mirroring Login Event Class](../../../relational-databases/event-classes/audit-database-mirroring-login-event-class.md).|  
|DATABASE_OBJECT_ACCESS_GROUP|Das Ereignis wird jedes Mal ausgelöst, wenn auf Datenbankobjekte, z. B. Nachrichtentyp, Assembly, Vertrag, zugegriffen wird. Dieses Ereignis wird für jeden Zugriff auf eine Datenbank ausgelöst. Hinweis: Dies kann zu großen Überwachungsdatensätzen führen.<br /><br /> Entspricht der [Audit Database Object Access Event Class](../../../relational-databases/event-classes/audit-database-object-access-event-class.md).|  
|DATABASE_OBJECT_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn eine CREATE-, ALTER- oder DROP-Anweisung für Datenbankobjekte, z. B. Schemas, ausgeführt wird. Dieses Ereignis wird immer dann ausgelöst, wenn ein Datenbankobjekt erstellt, geändert oder gelöscht wird. Hinweis: Dies kann zu einer sehr großen Anzahl an Überwachungsdatensätzen führen.<br /><br /> Entspricht der [Audit Database Object Management Event Class](../../../relational-databases/event-classes/audit-database-object-management-event-class.md).|  
|DATABASE_OBJECT_OWNERSHIP_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn der Besitzer für Objekte im Datenbankbereich geändert wird. Das Ereignis wird für eine Objektbesitzänderung in einer beliebigen Datenbank auf dem Server ausgelöst. Entspricht der [Audit Database Object Take Ownership Event Class](../../../relational-databases/event-classes/audit-database-object-take-ownership-event-class.md).|  
|DATABASE_OBJECT_PERMISSION_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn für Datenbankobjekte, z. B. Assemblys und Schemas, eine GRANT-, REVOKE- oder DENY-Anweisung ausgegeben wurde. Das Ereignis wird für eine Objektberechtigungsänderung für eine beliebige Datenbank auf dem Server ausgelöst. Entspricht der [Audit Database Object GDR Event Class](../../../relational-databases/event-classes/audit-database-object-gdr-event-class.md).|  
|DATABASE_OPERATION_GROUP|Das Ereignis wird ausgelöst, wenn Datenbankvorgänge auftreten, z. B. Prüfpunkt- oder Abonnement-Abfragebenachrichtigungen. Das Ereignis wird bei jedem Datenbankvorgang in einer Datenbank ausgelöst. Entspricht der [Audit Database Operation Event Class](../../../relational-databases/event-classes/audit-database-operation-event-class.md).|  
|DATABASE_OWNERSHIP_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn Sie die ALTER AUTHORIZATION-Anweisung verwenden, um den Besitzer einer Datenbank zu ändern, und die dazu erforderlichen Berechtigungen überprüft werden. Das Ereignis wird für eine Datenbankbesitzänderung in einer beliebigen Datenbank auf dem Server ausgelöst. Entspricht der [Audit Change Database Owner Event Class](../../../relational-databases/event-classes/audit-change-database-owner-event-class.md).|  
|DATABASE_PERMISSION_CHANGE_GROUP|Das Ereignis wird immer dann ausgelöst, wenn ein Prinzipal in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] eine GRANT-, REVOKE- oder DENY-Anweisung für eine Anweisungsberechtigung ausgibt (dies gilt ausschließlich für datenbankspezifische Ereignisse, beispielsweise das Gewähren von Berechtigungen für eine Datenbank).<br /><br /> Das Ereignis wird für eine Datenbankbesitzänderung (GDR) in einer beliebigen Datenbank auf dem Server ausgelöst. Entspricht der [Audit Database Scope GDR Event Class](../../../relational-databases/event-classes/audit-database-scope-gdr-event-class.md).|  
|DATABASE_PRINCIPAL_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn Prinzipale, z. B. Benutzer, in einer Datenbank erstellt oder geändert bzw. aus einer Datenbank gelöscht werden. Entspricht der [Audit Database Principal Management Event Class](../../../relational-databases/event-classes/audit-database-principal-management-event-class.md). (Entspricht ebenfalls der Audit Add DB Principal-Ereignisklasse, die bei den veralteten gespeicherten Prozeduren sp_grantdbaccess, sp_revokedbaccess, sp_addPrincipal und sp_dropPrincipal auftritt.)<br /><br /> Dieses Ereignis wird immer dann ausgelöst, wenn eine Datenbankrolle mit den gespeicherten sp_addrole- und sp_droprole-Prozeduren hinzugefügt bzw. entfernt wird. Das Ereignis wird immer dann ausgelöst, wenn Datenbankprinzipale erstellt, geändert oder aus einer Datenbank gelöscht werden. Entspricht der [Audit Add Role Event Class](../../../relational-databases/event-classes/audit-add-role-event-class.md).|  
|DATABASE_PRINCIPAL_IMPERSONATION_GROUP|Dieses Ereignis wird ausgelöst, wenn es im Datenbankbereich zu einem Identitätswechselvorgang kommt, z. B. bei EXECUTE AS \<principal> oder SETPRINCIPAL. Das Ereignis wird für Identitätswechsel in einer Datenbank ausgelöst. Entspricht der [Audit Database Principal Impersonation Event Class](../../../relational-databases/event-classes/audit-database-principal-impersonation-event-class.md).|  
|DATABASE_ROLE_MEMBER_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn Anmeldedaten hinzugefügt oder aus einer Datenbankrolle entfernt werden. Diese Ereignisklasse wird für die gespeicherten Prozeduren sp_addrolemember, sp_changegroup und sp_droprolemember ausgelöst. Das Ereignis wird bei jeder Änderung der Mitgliedschaft in einer Datenbankrolle in jeder Datenbank ausgelöst. Entspricht der [Audit Add Member to DB Role-Ereignisklasse](../../../relational-databases/event-classes/audit-add-member-to-db-role-event-class.md).|  
|DBCC_GROUP|Das Ereignis wird ausgelöst, wenn ein Prinzipal einen DBCC-Befehl ausgibt. Entspricht der [Audit DBCC Event Class](../../../relational-databases/event-classes/audit-dbcc-event-class.md).|  
|FAILED_DATABASE_AUTHENTICATION_GROUP|Gibt an, dass ein Prinzipal vergeblich versucht hat, sich an einer eigenständigen Datenbank anzumelden. Ereignisse in dieser Klasse werden durch neue Verbindungen oder durch Verbindungen ausgelöst, die aus einem Verbindungspool wiederverwendet werden. Entspricht der [Audit Login Failed Event Class](../../../relational-databases/event-classes/audit-login-failed-event-class.md).|    
|FAILED_LOGIN_GROUP|Gibt an, dass ein Prinzipal versucht hat, sich bei [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] anzumelden, und diese Anmeldung fehlgeschlagen ist. Ereignisse in dieser Klasse werden durch neue Verbindungen oder durch Verbindungen ausgelöst, die aus einem Verbindungspool wiederverwendet werden. Entspricht der [Audit Login Failed Event Class](../../../relational-databases/event-classes/audit-login-failed-event-class.md). Diese Überprüfung gilt nicht für Azure SQL-Datenbank.| 
|FULLTEXT_GROUP|Gibt an, dass ein Volltextereignis aufgetreten ist. Entspricht der [Audit Fulltext Event Class](../../../relational-databases/event-classes/audit-fulltext-event-class.md).|  
|LOGIN_CHANGE_PASSWORD_GROUP|Dieses Ereignis wird ausgelöst, wenn das Anmeldekennwort mit einer ALTER LOGIN-Anweisung oder der gespeicherten sp_password-Prozedur geändert wird. Entspricht der [Audit Login Change Password Event Class](../../../relational-databases/event-classes/audit-login-change-password-event-class.md).|  
|LOGOUT_GROUP|Gibt an, dass ein Prinzipal sich von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]abgemeldet hat. Ereignisse in dieser Klasse werden durch neue Verbindungen oder durch Verbindungen ausgelöst, die aus einem Verbindungspool wiederverwendet werden. Entspricht der [Audit Logout Event Class](../../../relational-databases/event-classes/audit-logout-event-class.md).|  
|SCHEMA_OBJECT_ACCESS_GROUP|Das Ereignis wird immer dann ausgelöst, wenn eine Objektberechtigung im Schema verwendet wurde. Entspricht der [Audit Schema Object Access Event Class](../../../relational-databases/event-classes/audit-schema-object-access-event-class.md).|  
|SCHEMA_OBJECT_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn ein CREATE-, ALTER- oder DROP-Vorgang für ein Schema ausgeführt wird. Entspricht der [Audit Schema Object Management Event Class](../../../relational-databases/event-classes/audit-schema-object-management-event-class.md).<br /><br /> Das Ereignis wird bei Schemaobjekten ausgelöst. Entspricht der [Audit Object Derived Permission Event Class](../../../relational-databases/event-classes/audit-object-derived-permission-event-class.md).<br /><br /> Dieses Ereignis wird immer dann ausgelöst, wenn ein Schema in einer beliebigen Datenbank geändert wird. Entspricht der [Audit Statement Permission Event Class](../../../relational-databases/event-classes/audit-statement-permission-event-class.md).|  
|SCHEMA_OBJECT_OWNERSHIP_CHANGE_GROUP|Dieses Ereignis wird ausgelöst, wenn die Berechtigungen zum Ändern des Besitzers eines Schemaobjekts (z. B. einer Tabelle, Prozedur oder Funktion) überprüft werden. Dies tritt ein, wenn mit der ALTER AUTHORIZATION-Anweisung einem Objekt ein Besitzer zugewiesen wird. Dieses Ereignis wird für eine Schemabesitzänderung für eine beliebige Datenbank auf dem Server ausgelöst. Entspricht der [Audit Schema Object Take Ownership Event Class](../../../relational-databases/event-classes/audit-schema-object-take-ownership-event-class.md).|  
|SCHEMA_OBJECT_PERMISSION_CHANGE_GROUP|Dieses Ereignis wird immer dann ausgelöst, wenn eine Grant-, Deny- oder Revoke-Anweisung für ein Schemaobjekt ausgeführt wird. Entspricht der [Audit Schema Object GDR Event Class](../../../relational-databases/event-classes/audit-schema-object-gdr-event-class.md).|  
|SERVER_OBJECT_CHANGE_GROUP|Das Ereignis wird für CREATE-, ALTER- oder DROP-Vorgänge auf Serverobjekten ausgelöst. Entspricht der [Audit Server Object Management Event Class](../../../relational-databases/event-classes/audit-server-object-management-event-class.md).|  
|SERVER_OBJECT_OWNERSHIP_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn der Besitzer für Objekte im Serverbereich geändert wird. Entspricht der [Audit Server Object Take Ownership Event Class](../../../relational-databases/event-classes/audit-server-object-take-ownership-event-class.md).|  
|SERVER_OBJECT_PERMISSION_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn ein Prinzipal in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]eine GRANT-, REVOKE- oder DENY-Anweisung für eine Serverobjektberechtigung ausgibt. Entspricht der [Audit Server Object GDR Event Class](../../../relational-databases/event-classes/audit-server-object-gdr-event-class.md).|  
|SERVER_OPERATION_GROUP|Das Ereignis wird ausgelöst, wenn Sicherheitsüberwachungsvorgänge verwendet werden, z. B. das Ändern von Einstellungen, Ressourcen, des externen Zugriffs oder von Berechtigungen. Entspricht der [Audit Server Operation Event Class](../../../relational-databases/event-classes/audit-server-operation-event-class.md).|  
|SERVER_PERMISSION_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn eine GRANT-, REVOKE- oder DENY-Anweisung für Berechtigungen im Serverbereich ausgegeben wird. Entspricht der [Audit Server Scope GDR Event Class](../../../relational-databases/event-classes/audit-server-scope-gdr-event-class.md).|  
|SERVER_PRINCIPAL_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn Serverprinzipale erstellt, geändert oder gelöscht werden. Entspricht der [Audit Server Principal Management Event Class](../../../relational-databases/event-classes/audit-server-principal-management-event-class.md).<br /><br /> Dieses Ereignis wird ausgelöst, wenn ein Prinzipal die gespeicherten sp_defaultdb- oder sp_defaultlanguage-Prozeduren oder ALTER LOGIN-Anweisungen ausgibt. Entspricht der [Audit Addlogin Event Class](../../../relational-databases/event-classes/audit-addlogin-event-class.md).<br /><br /> Dieses Ereignis wird für die gespeicherten sp_addlogin- und sp_droplogin-Prozeduren ausgelöst. Entspricht auch der [Audit Login Change Property Event Class](../../../relational-databases/event-classes/audit-login-change-property-event-class.md).<br /><br /> Dieses Ereignis wird für die gespeicherten Prozeduren sp_grantlogin oder sp_revokelogin ausgelöst. Entspricht der [Audit Login GDR Event Class](../../../relational-databases/event-classes/audit-login-gdr-event-class.md).|  
|SERVER_PRINCIPAL_IMPERSONATION_GROUP|Dieses Ereignis wird ausgelöst, wenn es zu einem Identitätswechsel im Serverbereich kommt, z. B. bei EXECUTE AS \<login>. Entspricht der [Audit Server Principal Impersonation Event Class](../../../relational-databases/event-classes/audit-server-principal-impersonation-event-class.md).|  
|SERVER_ROLE_MEMBER_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn Anmeldedaten hinzugefügt werden oder aus einer festen Serverrolle entfernt werden. Dieses Ereignis wird für die gespeicherten Prozeduren sp_addsrvrolemember und sp_dropsrvrolemember ausgelöst. Entspricht der [Audit Add Login to Server Role-Ereignisklasse](../../../relational-databases/event-classes/audit-add-login-to-server-role-event-class.md).|  
|SERVER_STATE_CHANGE_GROUP|Dieses Ereignis wird ausgelöst, wenn der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienststatus geändert wird. Entspricht der [Audit Server Starts and Stops Event Class](../../../relational-databases/event-classes/audit-server-starts-and-stops-event-class.md).|  
|SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP|Gibt an, dass sich ein Prinzipal erfolgreich an einer eigenständigen Datenbank angemeldet hat.|  
|SUCCESSFUL_LOGIN_GROUP|Gibt an, dass ein Prinzipal sich erfolgreich bei [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]angemeldet hat. Ereignisse in dieser Klasse werden durch neue Verbindungen oder durch Verbindungen ausgelöst, die aus einem Verbindungspool wiederverwendet werden. Entspricht der [Audit Login Event Class](../../../relational-databases/event-classes/audit-login-event-class.md).|  
|TRACE_CHANGE_GROUP|Das Ereignis wird für alle Anweisungen ausgelöst, die die ALTER TRACE-Berechtigung überprüfen. Entspricht der [Audit Server Alter Trace Event Class](../../../relational-databases/event-classes/audit-server-alter-trace-event-class.md).|  
|TRANSACTION_GROUP|Dieses Ereignis wird für BEGIN TRANSACTION-, ROLLBACK TRANSACTION- und COMMIT TRANSACTION-Vorgänge ausgelöst, sowohl bei expliziten Aufrufen dieser Anweisungen als auch bei impliziten Transaktionsvorgängen. Dieses Ereignis wird auch für UNDO-Vorgänge für einzelne Anweisungen ausgelöst, die durch das Rollback einer Transaktion verursacht wurden.|  
|USER_CHANGE_PASSWORD_GROUP|Das Ereignis wird immer dann ausgelöst, wenn das Kennwort des Benutzers einer eigenständigen Datenbank mit der ALTER USER-Anweisung geändert wird.|  
|USER_DEFINED_AUDIT_GROUP|Diese Gruppe überwacht ausgelöste Ereignisse mithilfe von [sp_audit_write &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-audit-write-transact-sql.md). In der Regel schließen Trigger oder gespeicherte Prozeduren Aufrufe von **sp_audit_write** ein, um das Überwachen von wichtigen Ereignissen zu aktivieren.|  
  
### <a name="considerations"></a>Überlegungen  
 Aktionsgruppen auf Serverebene umfassen Aktionen auf einer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz. Das Einchecken eines Schemaobjektszugriffs auf eine Datenbank wird beispielsweise aufgezeichnet, wenn die entsprechende Aktionsgruppe der Serverüberwachungsspezifikation hinzugefügt wird. In einer Datenbank-Überwachungsspezifikation werden nur Schemaobjektzugriffe in dieser Datenbank aufgezeichnet.  
  
 Aktionen auf Serverebene ermöglichen keine ausführliche Filterung für Aktionen auf Datenbankebene. Eine Überwachung auf Datenbankebene, z. B. eine Überwachung der SELECT-Aktionen in der Customers-Tabelle für Anmeldedaten in der Employee-Gruppe, ist für die Implementierung einer ausführlichen Aktionsfilterung notwendig. Beziehen Sie in einer Benutzerdatenbank-Überwachungsspezifikation keine Objekte mit Serverbereich ein, wie Systemsichten.  

 > [!NOTE]
 > Da das Aktivieren einer Überwachung auf Transaktionsebene sehr aufwändig ist, ist dies ab [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] SP2 CU3 und [!INCLUDE[ssSQL17](../../../includes/sssql17-md.md)] CU4 standardmäßig deaktiviert, wenn auch die Common Criteria-Kompatibilität deaktiviert ist.  Wenn die Common Criteria-Kompatibilität deaktiviert ist, können Sie zwar trotzdem eine Aktion der TRANSACTION_GROUP zu einer Überwachungsspezifikation hinzufügen, aber dann werden keine Transaktionsdaten erfasst.  Wenn Sie Überwachungsaktionen der TRANSACTION_GROUP konfigurieren möchten, vergewissern Sie sich, dass die Infrastruktur zur Überwachung auf Transaktionsebene aktiviert ist, indem Sie ab [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] SP2 CU3 und [!INCLUDE[ssSQL17](../../../includes/sssql17-md.md)] CU4 und höher die Common Criteria-Kompatibilität aktivieren.  Beachten Sie dabei, dass bei [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] ab SP1 CU2 die Überwachung auf Transaktionsebene auch mit dem Ablaufverfolgungsflag 3427 deaktiviert sein kann.
  
## <a name="database-level-audit-action-groups"></a>Überwachungsaktionsgruppen auf Datenbankebene  
 Überwachungsaktionsgruppen auf Datenbankebene sind Aktionen, die Ereignisklassen der Sicherheitsüberwachung in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ähneln. Weitere Informationen zu Ereignisklassen finden Sie unter [Ereignisklassen in SQL Server – Referenz](../../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
 In der folgenden Tabelle werden die Überwachungsaktionsgruppen auf Datenbankebene beschrieben. Hier finden Sie auch, sofern vorhanden, die entsprechende Ereignisklasse in SQL Server.  
  
|Aktionsgruppenname|BESCHREIBUNG|  
|-----------------------|-----------------|  
|APPLICATION_ROLE_CHANGE_PASSWORD_GROUP|Das Ereignis wird ausgelöst, wenn ein Kennwort für eine Anwendungsrolle geändert wird. Entspricht der [Audit App Role Change Password Event Class](../../../relational-databases/event-classes/audit-app-role-change-password-event-class.md).|  
|AUDIT_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn eine Überwachung erstellt, geändert oder gelöscht wird. Das Ereignis wird ausgelöst, wenn eine Überwachungsspezifikation erstellt, geändert oder gelöscht wird. Jede Änderung an einer Überwachung wird in dieser Überwachung überwacht. Entspricht der [Audit Change Audit Event Class](../../../relational-databases/event-classes/audit-change-audit-event-class.md).|  
|BACKUP_RESTORE_GROUP|Das Ereignis wird ausgelöst, wenn ein Sicherungs- oder Wiederherstellungsbefehl ausgegeben wird. Entspricht der [Audit Backup/Restore-Ereignisklasse](../../../relational-databases/event-classes/audit-backup-and-restore-event-class.md).|  
|BATCH_COMPLETED_GROUP|Dieses Ereignis wird ausgelöst, wenn die Ausführung von Batchtexten, gespeicherten Prozeduren oder Transaktionverwaltungsvorgänge abgeschlossen wird. Es wird nach Abschluss des Batches ausgelöst und überwacht den gesamten Text des Batches oder der gespeicherten Prozedur, so wie er vom Client übermittelt wird, einschließlich des Ergebnisses.|  
|BATCH_STARTED_GROUP|Dieses Ereignis wird ausgelöst, wenn die Ausführung von Batchtexten, gespeicherten Prozeduren oder Transaktionverwaltungsvorgänge begonnen wird. Es wird vor der Ausführung ausgelöst und überwacht den gesamten Text des Batches oder der gespeicherten Prozedur, so wie er vom Client übermittelt wird.|  
|DATABASE_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn eine Datenbank erstellt, geändert oder gelöscht wird. Entspricht der [Audit Database Management Event Class](../../../relational-databases/event-classes/audit-database-management-event-class.md).|  
|DATABASE_LOGOUT_GROUP|Das Ereignis wird ausgelöst, wenn sich der Benutzer einer eigenständigen Datenbank von einer Datenbank abmeldet.|  
|DATABASE_OBJECT_ACCESS_GROUP|Das Ereignis wird immer dann ausgelöst, wenn auf Datenbankobjekte, z. B. Zertifikate und asymmetrische Schlüssel, zugegriffen wird. Entspricht der [Audit Database Object Access Event Class](../../../relational-databases/event-classes/audit-database-object-access-event-class.md).|  
|DATABASE_OBJECT_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn eine CREATE-, ALTER- oder DROP-Anweisung für Datenbankobjekte, z. B. Schemas, ausgeführt wird. Entspricht der [Audit Database Object Management Event Class](../../../relational-databases/event-classes/audit-database-object-management-event-class.md).|  
|DATABASE_OBJECT_OWNERSHIP_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn der Besitzer für Objekte im Datenbankbereich geändert wird. Entspricht der [Audit Database Object Take Ownership Event Class](../../../relational-databases/event-classes/audit-database-object-take-ownership-event-class.md).|  
|DATABASE_OBJECT_PERMISSION_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn für Datenbankobjekte, z. B. Assemblys und Schemas, eine GRANT-, REVOKE- oder DENY-Anweisung ausgegeben wurde. Entspricht der [Audit Database Object GDR Event Class](../../../relational-databases/event-classes/audit-database-object-gdr-event-class.md).|  
|DATABASE_OPERATION_GROUP|Das Ereignis wird ausgelöst, wenn Datenbankvorgänge auftreten, z. B. Prüfpunkt- oder Abonnement-Abfragebenachrichtigungen. Entspricht der [Audit Database Operation Event Class](../../../relational-databases/event-classes/audit-database-operation-event-class.md).|  
|DATABASE_OWNERSHIP_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn Sie die ALTER AUTHORIZATION-Anweisung verwenden, um den Besitzer einer Datenbank zu ändern, und die dazu erforderlichen Berechtigungen überprüft werden. Entspricht der [Audit Change Database Owner Event Class](../../../relational-databases/event-classes/audit-change-database-owner-event-class.md).|  
|DATABASE_PERMISSION_CHANGE_GROUP|Das Ereignis wird immer dann ausgelöst, wenn ein Benutzer in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] eine GRANT-, REVOKE- oder DENY-Anweisung für eine Anweisungsberechtigung ausgibt (dies gilt ausschließlich für datenbankspezifische Ereignisse, beispielsweise das Gewähren von Berechtigungen für eine Datenbank). Entspricht der [Audit Database Scope GDR Event Class](../../../relational-databases/event-classes/audit-database-scope-gdr-event-class.md).|  
|DATABASE_PRINCIPAL_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn Prinzipale, z. B. Benutzer, in einer Datenbank erstellt oder geändert bzw. aus einer Datenbank gelöscht werden. Entspricht der [Audit Database Principal Management Event Class](../../../relational-databases/event-classes/audit-database-principal-management-event-class.md). Entspricht auch der [Audit Add DB User-Ereignisklasse](../../../relational-databases/event-classes/audit-add-db-user-event-class.md), die bei den veralteten gespeicherten Prozeduren sp_grantdbaccess, sp_revokedbaccess, sp_adduser und sp_dropuser auftritt.<br /><br /> Dieses Ereignis wird immer dann ausgelöst, wenn eine Datenbankrolle mit den veralteten gespeicherten Prozeduren sp_addrole und sp_droprole hinzugefügt bzw. entfernt wird. Entspricht der [Audit Add Role Event Class](../../../relational-databases/event-classes/audit-add-role-event-class.md).|  
|DATABASE_PRINCIPAL_IMPERSONATION_GROUP|Dieses Ereignis wird ausgelöst, wenn es zu einem Identitätswechsel im Datenbankbereich kommt, z. B. bei EXECUTE AS \<user>. Entspricht der [Audit Database Principal Impersonation Event Class](../../../relational-databases/event-classes/audit-database-principal-impersonation-event-class.md).|  
|DATABASE_ROLE_MEMBER_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn Anmeldedaten hinzugefügt oder aus einer Datenbankrolle entfernt werden. Diese Ereignisklasse wird mit den gespeicherten Prozeduren sp_addrolemember, sp_changegroup und sp_droprolemember verwendet. Entspricht der [Audit Add Member to DB Role-Ereignisklasse](../../../relational-databases/event-classes/audit-add-member-to-db-role-event-class.md).|  
|DBCC_GROUP|Das Ereignis wird ausgelöst, wenn ein Prinzipal einen DBCC-Befehl ausgibt. Entspricht der [Audit DBCC Event Class](../../../relational-databases/event-classes/audit-dbcc-event-class.md).|  
|FAILED_DATABASE_AUTHENTICATION_GROUP|Gibt an, dass ein Prinzipal vergeblich versucht hat, sich an einer eigenständigen Datenbank anzumelden. Ereignisse in dieser Klasse werden durch neue Verbindungen oder durch Verbindungen ausgelöst, die aus einem Verbindungspool wiederverwendet werden. Das Ereignis wird ausgelöst.|  
|SCHEMA_OBJECT_ACCESS_GROUP|Das Ereignis wird immer dann ausgelöst, wenn eine Objektberechtigung im Schema verwendet wurde. Entspricht der [Audit Schema Object Access Event Class](../../../relational-databases/event-classes/audit-schema-object-access-event-class.md).|  
|SCHEMA_OBJECT_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn ein CREATE-, ALTER- oder DROP-Vorgang für ein Schema ausgeführt wird. Entspricht der [Audit Schema Object Management Event Class](../../../relational-databases/event-classes/audit-schema-object-management-event-class.md).<br /><br /> Das Ereignis wird bei Schemaobjekten ausgelöst. Entspricht der [Audit Object Derived Permission Event Class](../../../relational-databases/event-classes/audit-object-derived-permission-event-class.md). Entspricht auch der [Audit Statement Permission Event Class](../../../relational-databases/event-classes/audit-statement-permission-event-class.md).|  
|SCHEMA_OBJECT_OWNERSHIP_CHANGE_GROUP|Das Ereignis wird ausgelöst, wenn die Berechtigungen zum Ändern des Besitzers eines Schemaobjekts, z. B. einer Tabelle, Prozedur oder Funktion, überprüft werden. Dies tritt ein, wenn mit der ALTER AUTHORIZATION-Anweisung einem Objekt ein Besitzer zugewiesen wird. Entspricht der [Audit Schema Object Take Ownership Event Class](../../../relational-databases/event-classes/audit-schema-object-take-ownership-event-class.md).|  
|SCHEMA_OBJECT_PERMISSION_CHANGE_GROUP|Das Ereignis wird immer dann ausgelöst, wenn eine Grant-, Deny- oder Revoke-Anweisung für ein Schemaobjekt ausgegeben wird. Entspricht der [Audit Schema Object GDR Event Class](../../../relational-databases/event-classes/audit-schema-object-gdr-event-class.md).|  
|SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP|Gibt an, dass sich ein Prinzipal erfolgreich an einer eigenständigen Datenbank angemeldet hat.|  
|USER_CHANGE_PASSWORD_GROUP|Das Ereignis wird immer dann ausgelöst, wenn das Kennwort des Benutzers einer eigenständigen Datenbank mit der ALTER USER-Anweisung geändert wird.|  
|USER_DEFINED_AUDIT_GROUP|Diese Gruppe überwacht ausgelöste Ereignisse mithilfe von [sp_audit_write &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-audit-write-transact-sql.md).|  
  
## <a name="database-level-audit-actions"></a>Überwachungsaktionen auf Datenbankebene  
 Aktionen auf Datenbankebene unterstützen die direkte Überwachung von spezifischen Aktionen für Datenbankenschema- und Schemaobjekte, z. B. Tabellen, Sichten, gespeicherte Prozeduren, Funktionen, erweiterte gespeicherte Prozeduren, Warteschlangen, Synonyme. Typen, XML-Schemaauflistung, Datenbank und Schema werden nicht überwacht. Die Überwachung von Schemaobjekten kann für Schema und Datenbank konfiguriert werden, d. h., es werden alle Ereignisse für alle Schemaobjekte im angegebenen Schema oder in der angegebenen Datenbank überwacht. In der folgenden Tabelle werden Überwachungsaktionen auf Datenbankebene beschrieben.  
  
|Aktion|BESCHREIBUNG|  
|------------|-----------------|  
|SELECT|Dieses Ereignis wird immer dann ausgelöst, wenn SELECT ausgegeben wird.|  
|UPDATE|Dieses Ereignis wird immer dann ausgelöst, wenn UPDATE ausgegeben wird.|  
|INSERT|Dieses Ereignis wird immer dann ausgelöst, wenn INSERT ausgegeben wird.|  
|Delete|Dieses Ereignis wird immer dann ausgelöst, wenn DELETE ausgegeben wird.|  
|Führen Sie|Dieses Ereignis wird immer dann ausgelöst, wenn EXECUTE ausgegeben wird.|  
|RECEIVE|Dieses Ereignis wird immer dann ausgelöst, wenn RECEIVE ausgegeben wird.|  
|REFERENCES|Dieses Ereignis wird immer dann ausgelöst, wenn eine REFERENCES-Berechtigung überprüft wird.|  
  
### <a name="considerations"></a>Überlegungen  
*  Überwachungsaktionen auf Datenbankebene gelten nicht für Spalten.  
  
*  Wenn der Abfrageprozessor die Abfrage parametrisiert, wird der Parameter unter Umständen im Überwachungsereignisprotokoll und nicht in den Spaltenwerten der Abfrage angezeigt. 
 
*  RPC-Anweisungen werden nicht protokolliert. 
  
## <a name="audit-level-audit-action-groups"></a>Überwachungsaktionsgruppen auf Überwachungsebene  
 Sie können auch die Aktionen im Überwachungsprozess überwachen. Dies kann im Serverbereich oder im Datenbankbereich durchgeführt werden. Im Datenbankbereich tritt dies nur bei Datenbank-Überwachungsspezifikationen auf. In der folgenden Tabelle werden Überwachungsaktionsgruppen auf Überwachungsebene beschrieben.  
  
|Aktionsgruppenname|BESCHREIBUNG|  
|-----------------------|-----------------|  
|AUDIT_CHANGE_GROUP|Dieses Ereignis wird immer dann ausgelöst, wenn einer der folgenden Befehle ausgegeben wird:<br /><br /> CREATE SERVER AUDIT<br /><br /> ALTER SERVER AUDIT<br /><br /> DROP SERVER AUDIT<br /><br /> CREATE SERVER AUDIT SPECIFICATION<br /><br /> ALTER SERVER AUDIT SPECIFICATION<br /><br /> DROP SERVER AUDIT SPECIFICATION<br /><br /> CREATE DATABASE AUDIT SPECIFICATION<br /><br /> ALTER DATABASE AUDIT SPECIFICATION<br /><br /> DROP DATABASE AUDIT SPECIFICATION|  
  
## <a name="related-content"></a>Verwandte Inhalte  
 [Erstellen einer Serverüberwachung und einer Serverüberwachungsspezifikation](../../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
 [Erstellen einer Server- und Datenbank-Überwachungsspezifikation](../../../relational-databases/security/auditing/create-a-server-audit-and-database-audit-specification.md)  
  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-transact-sql.md)  
  
 [ALTER SERVER AUDIT  &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-audit-transact-sql.md)  
  
 [DROP SERVER AUDIT  &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-server-audit-transact-sql.md)  
  
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-specification-transact-sql.md)  
  
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-audit-specification-transact-sql.md)  
  
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-server-audit-specification-transact-sql.md)  
  
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-audit-specification-transact-sql.md)  
  
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-database-audit-specification-transact-sql.md)  
  
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-database-audit-specification-transact-sql.md)  
  
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-authorization-transact-sql.md)  
  
 [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)  
  
 [sys.server_audits &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)  
  
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)  
  
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)  
  
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)  
  
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)  
  
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)  
  
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)  
  
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)  
  
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)  
  
  
