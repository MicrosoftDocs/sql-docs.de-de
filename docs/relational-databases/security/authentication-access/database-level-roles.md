---
title: Rollen auf Datenbankebene | Microsoft-Dokumentation
description: Zur Verwaltung der Berechtigungen für Ihre Datenbanken stellt SQL Server mehrere Rollen zur Verfügung. Dabei handelt es sich um Sicherheitsprinzipale, die andere Prinzipale gruppieren.
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, azure-synapse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.roleproperties.database.f1
- sql13.swb.roleproperties.object.f1
- SQL13.SWB.DBROLEPROPERTIES.GENERAL.F1
- sql13.swb.roleproperties.general.f1
helpviewer_keywords:
- db_denydatareader role
- users [SQL Server], database roles
- database-level roles [SQL Server]
- db_denydatawriter role
- roles [SQL Server], database
- principals [SQL Server], database-level
- db_backupoperator role
- credentials [SQL Server], roles
- db_accessadmin role
- schemas [SQL Server], roles
- permissions [SQL Server], roles
- database roles [SQL Server], listed
- db_datareader role
- db_ddladmin role
- db_datawriter role
- db_securityadmin role
- db_owner role
- database roles [SQL Server]
- fixed database roles [SQL Server]
- authentication [SQL Server], roles
- groups [SQL Server], roles
ms.assetid: 7f3fa5f6-6b50-43bb-9047-1544ade55e39
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 86560767d969ef5ee7da97c989da7558421f0302
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344411"
---
# <a name="database-level-roles"></a>Rollen auf Datenbankebene

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Zur einfachen Verwaltung der Berechtigungen für Ihre Datenbanken stellt [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mehrere *Rollen* zur Verfügung. Dies sind Sicherheitsprinzipale, die andere Prinzipale gruppieren. Sie entsprechen den ***Gruppen*** im [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows-Betriebssystem. Der Geltungsbereich der Berechtigungen von Rollen auf Datenbankebene erstreckt sich auf die gesamte Datenbank.  

Zum Hinzufügen und Entfernen von Benutzern zu oder aus einer Datenbankrolle verwenden Sie die Optionen `ADD MEMBER` und `DROP MEMBER` der [ALTER ROLE](../../../t-sql/statements/alter-role-transact-sql.md) -Anweisung. [!INCLUDE[ssPDW_md](../../../includes/sspdw-md.md)] und Azure Synapse unterstützt diese Verwendung von `ALTER ROLE` nicht. Verwenden Sie stattdessen die älteren Prozeduren [sp_addrolemember](../../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) und [sp_droprolemember](../../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md) .
  
 Es gibt zwei Typen von Rollen auf Datenbankebene: *feste Datenbankrollen* , die in der Datenbank vordefiniert sind, und *benutzerdefinierte Datenbankrollen* , die Sie erstellen können.  
  
 Feste Datenbankrollen werden auf der Datenbankebene definiert und sind in jeder Datenbank vorhanden. Mitglieder der Datenbankrolle **db_owner** können die Mitgliedschaft von festen Datenbankrollen ändern. In der msdb-Datenbank gibt es auch einige Datenbankrollen für spezielle Zwecke.  
  
 Sie können in Rollen auf Datenbankebene jedes Datenbankkonto und andere [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Rollen hinzufügen.
  
> [!TIP]  
>  Fügen Sie keine benutzerdefinierten Datenbankrollen als Mitglieder fester Rollen hinzu. Dies könnte zu einer unbeabsichtigten Ausweitung von Privilegien führen.  

Die Berechtigungen von benutzerdefinierten Datenbankrollen können mithilfe der Anweisungen GRANT, DENY und REVOKE angepasst werden. Weitere Informationen finden Sie unter [Berechtigungen (Datenbank-Engine)](../../../relational-databases/security/permissions-database-engine.md).

Eine Liste aller Berechtigungen finden Sie auf dem Poster [Database Engine Permissions (Berechtigungen im Datenbankmodul)](https://aka.ms/sql-permissions-poster) . Datenbankrollen können keine Berechtigungen auf Serverebene erteilt werden. Anmeldungen und andere Prinzipale auf Serverebene (wie etwa Serverrollen) können Datenbankrollen nicht hinzugefügt werden. Verwenden Sie für Sicherheit auf Serverebene in [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)]stattdessen [Serverrollen](../../../relational-databases/security/authentication-access/server-level-roles.md) . Berechtigungen auf Serverebene können nicht mithilfe von Rollen in [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] und Azure Synapse erteilt werden.

## <a name="fixed-database-roles"></a>feste Datenbankrollen
  
 In der folgenden Tabelle sind die festen Datenbankrollen und ihre Möglichkeiten aufgeführt. Diese Rollen sind in allen Datenbanken vorhanden. Außer bei der Datenbankrolle **public** können die den festen Datenbankrollen zugewiesenen Berechtigungen nicht geändert werden.   
  
|Name der festen Datenbankrolle|BESCHREIBUNG|  
|-------------------------------|-----------------|  
|**db_owner**|Mitglieder der festen Datenbankrolle **db_owner** können alle Aktivitäten zur Konfiguration und Wartung an der Datenbank ausführen und die Datenbank in [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)]auch löschen. (In [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] und Azure Synapse sind für einige Wartungsaktivitäten Berechtigungen auf Serverebene erforderlich, und sie können von **db_owners** nicht ausgeführt werden.)|  
|**db_securityadmin**|Mitglieder der festen Datenbankrolle **db_securityadmin** können die Rollenmitgliedschaft nur für benutzerdefinierte Rollen ändern und Berechtigungen verwalten. Mitglieder dieser Rolle können Ihre Berechtigungen möglicherweise erhöhen, und ihre Aktionen sollten überwacht werden.|  
|**db_accessadmin**|Mitglieder der festen Datenbankrolle **db_accessadmin** können den Zugriff auf die Datenbank für Windows-Anmeldungen, Windows-Gruppen und [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Anmeldungen hinzufügen oder entfernen.|  
|**db_backupoperator**|Mitglieder der festen Datenbankrolle **db_backupoperator** können eine Sicherung der Datenbank durchführen.|  
|**db_ddladmin**|Mitglieder der festen Datenbankrolle **db_ddladmin** können in einer Datenbank sämtliche DDL-Befehle (Data Definition Language) ausführen.|  
|**db_datawriter**|Mitglieder der festen Datenbankrolle **db_datawriter** können Daten in allen Benutzertabellen hinzufügen, löschen oder ändern.|  
|**db_datareader**|Mitglieder der festen Datenbankrolle **db_datareader** können alle Daten aller Benutzertabellen lesen.|  
|**db_denydatawriter**|Mitglieder der festen Datenbankrolle **db_denydatareader** können keine Daten in den Benutzertabellen in einer Datenbank hinzufügen, ändern oder löschen.|  
|**db_denydatareader**|Mitglieder der festen Datenbankrolle **db_denydatareader** können keine Daten in den Benutzertabellen in einer Datenbank lesen.|  

Die den festen Datenbankrollen zugewiesenen Berechtigungen können nicht geändert werden. Die folgende Abbildung zeigt die den festen Datenbankrollen zugewiesenen Berechtigungen:

![fixed_database_role_permissions](../../../relational-databases/security/authentication-access/media/permissions-of-database-roles.png)

## <a name="special-roles-for-sssds_md-and-azure-synapse"></a>Besondere Rollen für [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] und Azure Synapse

Diese Datenbankrollen sind nur in der virtuellen Masterdatenbank vorhanden. Ihre Berechtigungen sind auf Aktionen beschränkt, die auf dem Master ausgeführt werden. Nur Datenbankbenutzer im Master können diesen Rollen hinzugefügt werden. Diesen Rollen können keine Anmeldungen hinzugefügt werden, jedoch können Benutzer auf der Grundlage von Anmeldungen erstellt werden, und diese Benutzer können den Rollen dann hinzugefügt werden. Auch eigenständige Datenbankbenutzer im Master können diesen Rollen hinzugefügt werden. Enthaltene Datenbankbenutzer, die der Rolle **dbmanager** im Master hinzugefügt wurden, können jedoch nicht zum Erstellen neuer Datenbanken verwendet werden.

|Rollenname|BESCHREIBUNG|  
|--------------------|-----------------|
|**dbmanager** | Kann Datenbanken erstellen und löschen. Ein Mitglied der dbmanager-Rolle, das eine Datenbank erstellt, wird zum Besitzer der betreffenden Datenbank. Dieser kann dann als dbo-Benutzer eine Verbindung mit der Datenbank herstellen. Der dbo-Benutzer verfügt über alle Datenbankberechtigungen in der Datenbank. Mitglieder der dbmanager-Rolle haben nicht unbedingt die Berechtigung zum Zugriff auf Datenbanken, die sie nicht besitzen.|
|**loginmanager** | Kann Anmeldungen in der virtuellen Masterdatenbank erstellen und löschen.|

> [!NOTE]
> Der Prinzipal auf Serverebene und der Azure Active Directory-Administrator (falls konfiguriert) besitzen alle Berechtigungen in [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] und Azure Synapse, ohne dazu Mitglieder irgendwelcher Rollen sein zu müssen. Weitere Informationen finden Sie unter [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](/azure/azure-sql/database/logins-create-manage). 

Einige Datenbankrollen gelten nicht für Azure SQL oder Synapse SQL:
- **db_backupoperator** ist auf Azure SQL-Datenbank (nicht verwaltete Instanz) und serverlose Synapse SQL-Pools nicht anwendbar, da die T-SQL-Befehle für die Sicherung und Wiederherstellung nicht verfügbar sind.
- **db_datawriter** und **db_denydatawriter** sind nicht auf Synapse SQL Serverless anwendbar, da dort nur externe Daten gelesen werden.
  
## <a name="msdb-roles"></a>msdb-Rollen  
 Die msdb-Datenbank enthält die in der folgenden Tabelle aufgeführten Rollen für spezielle Zwecke.  
  
|Name der msdb-Rolle|BESCHREIBUNG|  
|--------------------|-----------------|  
|**db_ssisadmin**<br /><br /> **db_ssisoperator**<br /><br /> **db_ssisltduser**|Mitglieder dieser Datenbankrollen können [!INCLUDE[ssIS](../../../includes/ssis-md.md)]verwalten und verwenden. Instanzen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], die von einer früheren Version aktualisiert wurden, enthalten möglicherweise eine ältere Version der Rolle, die mit Data Transformation Services (DTS) und nicht mit [!INCLUDE[ssIS](../../../includes/ssis-md.md)] benannt wurde. Weitere Informationen finden Sie unter [Integration Services Roles &#40;SSIS Service&#41;](../../../integration-services/security/integration-services-roles-ssis-service.md) (Integration Services-Rollen [SSIS-Dienst]).|  
|**dc_admin**<br /><br /> **dc_operator**<br /><br /> **dc_proxy**|Mitglieder dieser Datenbankrollen können den Datensammler verwalten und verwenden. Weitere Informationen finden Sie unter [Data Collection](../../../relational-databases/data-collection/data-collection.md).|  
|**PolicyAdministratorRole**|Mitglieder der Datenbankrolle **db_PolicyAdministratorRole** können alle Aktivitäten zur Konfiguration und Wartung für Richtlinien und Bedingungen der richtlinienbasierten Verwaltung ausführen. Weitere Informationen finden Sie unter [Verwalten von Servern mit der richtlinienbasierten Verwaltung](../../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md).|  
|**ServerGroupAdministratorRole**<br /><br /> **ServerGroupReaderRole**|Mitglieder dieser Datenbankrollen können registrierte Servergruppen verwalten und verwenden.|  
|**dbm_monitor**|Wird in der **msdb** -Datenbank erstellt, wenn die erste Datenbank im Datenbankspiegelungs-Monitor registriert wird. Die **dbm_monitor** -Rolle hat keine Mitglieder, bis ein Systemadministrator der Rolle Benutzer zuweist.|  
  
> [!IMPORTANT]  
>  Mitglieder der **db_ssisadmin** -Rolle und der **dc_admin** -Rolle können Ihre Berechtigungen möglicherweise auf sysadmin erhöhen. Diese Ausweitung von Berechtigungen ist möglich, da diese Rollen [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] -Pakete ändern können und [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] -Pakete von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mithilfe des sysadmin-Sicherheitskontexts des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Agents ausgeführt werden können. Konfigurieren Sie als Schutz vor dieser Ausweitung von Berechtigungen beim Ausführen von Wartungsplänen, Datensammlungssätzen und anderen [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] -Paketen Aufträge des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Agents, die Pakete ausführen, für die Verwendung eines Proxykontos mit eingeschränkten Berechtigungen, oder fügen Sie der **db_ssisadmin** -Rolle und der **dc_admin** -Rolle nur **sysadmin** -Mitglieder hinzu.  

## <a name="working-with-database-level-roles"></a>Arbeiten mit Rollen auf Datenbankebene  
 In der folgenden Tabelle werden die Befehle, Sichten und Funktionen zum Arbeiten mit Rollen auf Datenbankebene erklärt.  
  
|Funktion|type|BESCHREIBUNG|  
|-------------|----------|-----------------|  
|[sp_helpdbfixedrole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpdbfixedrole-transact-sql.md)|Metadaten|Gibt eine Liste der festen Datenbankrollen zurück.|  
|[sp_dbfixedrolepermission &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dbfixedrolepermission-transact-sql.md)|Metadaten|Zeigt die Berechtigungen einer festen Datenbankrolle an.|  
|[sp_helprole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)|Metadaten|Gibt Informationen zu den Rollen in der aktuellen Datenbank zurück.|  
|[sp_helprolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)|Metadaten|Gibt Informationen zu den Mitgliedern einer Rolle in der aktuellen Datenbank zurück.|  
|[sys.database_role_members &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)|Metadaten|Gibt eine Zeile für jedes Mitglied jeder Datenbankrolle zurück.|  
|[IS_MEMBER &#40;Transact-SQL&#41;](../../../t-sql/functions/is-member-transact-sql.md)|Metadaten|Zeigt an, ob der aktuelle Benutzer ein Mitglied der angegebenen Microsoft Windows-Gruppe oder der Microsoft SQL Server-Datenbankrolle ist.|  
|[CREATE ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-role-transact-sql.md)|Get-Help|Erstellt eine neue Datenbankrolle in der aktuellen Datenbank.|  
|[ALTER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-role-transact-sql.md)|Get-Help|Ändert den Namen oder die Mitgliedschaft einer Datenbankrolle .|  
|[DROP ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-role-transact-sql.md)|Get-Help|Entfernt eine Rolle aus der Datenbank.|  
|[sp_addrole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md)|Get-Help|Erstellt eine neue Datenbankrolle in der aktuellen Datenbank.|  
|[sp_droprole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-droprole-transact-sql.md)|Get-Help|Entfernt eine Datenbankrolle aus der aktuellen Datenbank.|  
|[sp_addrolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)|Get-Help|Fügt einer Datenbankrolle in der aktuellen Datenbank einen Datenbankbenutzer, eine Datenbankrolle, einen Windows-Anmeldenamen oder eine Windows-Gruppe hinzu. Alle Plattformen außer [!INCLUDE[ssPDW_md](../../../includes/sspdw-md.md)] und Azure Synapse sollten stattdessen `ALTER ROLE` verwenden.|  
|[sp_droprolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)|Get-Help|Entfernt ein Sicherheitskonto aus einer SQL Serverrolle in der aktuellen Datenbank. Alle Plattformen außer [!INCLUDE[ssPDW_md](../../../includes/sspdw-md.md)] und Azure Synapse sollten stattdessen `ALTER ROLE` verwenden.|
|[GRANT](../../../t-sql/statements/grant-transact-sql.md)| Berechtigungen | Fügt einer Rolle eine Berechtigung hinzu.
|[DENY](../../../t-sql/statements/deny-transact-sql.md)| Berechtigungen | Verweigert einer Rolle eine Berechtigung.
|[REVOKE](../../../t-sql/statements/revoke-transact-sql.md)| Berechtigungen | Entfernt eine zuvor erteilte oder verweigerte Berechtigung.
  
  
## <a name="public-database-role"></a>Datenbankrolle public  
 Jeder Datenbankbenutzer gehört der Datenbankrolle **public** an. Wenn einem Benutzer keine bestimmten Berechtigungen für ein sicherungsfähiges Objekt erteilt oder verweigert werden, erbt der Benutzer die Berechtigungen, die der Datenbankrolle **public** für dieses Objekt erteilt wurden. Benutzer können aus der Rolle **öffentlich** nicht entfernt werden. 
  
## <a name="related-content"></a>Verwandte Inhalte  
 [Sicherheitskatalogsichten &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)  
  
 [Gespeicherte Sicherheitsprozeduren &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)  
  
 [Sicherheitsfunktionen &#40;Transact-SQL&#41;](../../../t-sql/functions/security-functions-transact-sql.md)  
  
 [Sichern von SQL Server](../../../relational-databases/security/securing-sql-server.md)  
  
 [sp_helprotect &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)  
  
