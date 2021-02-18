---
title: Problembehandlung bei verwaisten Benutzern
description: Es kommt zu verwaisten Benutzern, wenn die Anmeldedaten eines Datenbankbenutzers nicht mehr in der Masterdatenbank gespeichert sind. In diesem Artikel wird erläutert, wie Sie verwaiste Benutzer identifizieren und entfernen.
ms.custom: seo-lt-2019
ms.date: 07/14/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: high-availability
ms.topic: troubleshooting
helpviewer_keywords:
- orphaned users [SQL Server]
- logins [SQL Server], orphaned users
- troubleshooting [SQL Server], user accounts
- user accounts [SQL Server], orphaned users
- failover [SQL Server], managing metadata
- database mirroring [SQL Server], metadata
- users [SQL Server], orphaned
ms.assetid: 11eefa97-a31f-4359-ba5b-e92328224133
author: cawrites
ms.author: chadam
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016'
ms.openlocfilehash: 2583e238295c87b555c43f25b7fd68b03f3647c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354033"
---
# <a name="troubleshoot-orphaned-users-sql-server"></a>Problembehandlung bei verwaisten Benutzern (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Verwaiste Benutzer treten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf, wenn ein Datenbankbenutzer auf einem Anmeldenamen in der **Masterdatenbank** basiert, aber der Anmeldename nicht mehr in **Master** vorhanden ist. Dies kann auftreten, wenn der Anmeldename gelöscht wird, oder wenn die Datenbank auf einen anderen Server verschoben wird, auf dem der Anmeldename nicht existiert. Dieses Thema beschreibt, wie Sie verwaiste Benutzer finden und sie den Anmeldenamen erneut zuordnen können.  
  
> [!NOTE]  
>  Verringern Sie mögliche verwaiste Benutzer, indem Sie eigenständige Datenbankbenutzer für Datenbanken, die verschoben werden können, verwenden. Weitere Informationen finden Sie unter [Eigenständige Datenbankbenutzer - machen Sie Ihre Datenbank portabel](../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
## <a name="background"></a>Hintergrund  
 Für die Verbindung mit einer Datenbank oder einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe eines Sicherheitsprinzipals (Datenbank-Benutzeridentität) auf Basis eines Anmeldenamens, muss der Prinzipal einen gültigen Anmeldenamen in der **Masterdatenbank** besitzen. Dieser Anmeldename wird bei der Authentifizierung benötigt, bei der die Prinzipalidentität überprüft wird und bestimmt wird, ob der Prinzipal eine Verbindung mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz herstellen darf. Die auf einer Serverinstanz vorhandenen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldenamen werden in der **sys.server_principals** -Katalogsicht und der **sys.sql_logins** -Kompatibilitätssicht angezeigt.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldenamen greifen als „Datenbankbenutzer“ auf individuelle Datenbanken zu, der dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldenamen zugeordnet ist. Es gibt jedoch drei Ausnahmen von dieser Regel:  
  
-   Eigenständige Datenbankbenutzer  
  
     Eigenständige Datenbankbenutzer authentifizieren sich auf Benutzerdatenbankebene und werden keinem Anmeldenamen zugeordnet. Dies wird empfohlen, da die Datenbanken besser portierbar sind und eigenständige Datenbankbenutzer nicht verwaisen können. Sie müssen jedoch für jede Datenbank neu erstellt werden. Möglicherweise ist dies in einer Umgebung mit mehreren Datenbanken nicht sinnvoll.  
  
-   Das **Gastkonto** .  
  
     Wenn dieses Konto in der Datenbank aktiviert ist, erlaubt es [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldenamen, die keinem Datenbankbenutzer zugeordnet sind, die die Datenbank als **Gastbenutzer** verwenden. Das **Gastkonto** wird standardmäßig deaktiviert.  
  
-   Microsoft Windows-Gruppenmitgliedschaften.  
  
     Ein von einem Windows-Benutzer erstellter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldename kann eine Datenbank verwenden, wenn der Windows-Benutzer Mitglied einer Windows-Gruppe ist, die auch ein Benutzer der Datenbank ist.  
  
 Die Informationen für die Zuordnung eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldenamens zu einem Datenbankbenutzer werden in der Datenbank gespeichert. Hierzu zählen der Name des Datenbankbenutzers sowie die Sicherheits-ID (SID) des entsprechenden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldenamens. Die Berechtigungen dieses Datenbankbenutzers werden für die Autorisierung in der Datenbank verwendet.  
  
 Ein Datenbankbenutzer (basierend auf einem Anmeldenamen), für den ein entsprechender [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldename auf einer Serverinstanz nicht oder falsch definiert ist, kann sich bei der Instanz nicht anmelden. Diese Benutzer werden als *verwaiste Benutzer* der Datenbank dieser Serverinstanz bezeichnet. Verwaisungen treten auf, wenn der Datenbankbenutzer einer Anmelde-SID zugeordnet wird, die auf der `master` -Instanz nicht vorhanden ist. Ein Datenbankbenutzer kann anschließend zu einem verwaisten Benutzer werden, wenn die Datenbank wiederhergestellt oder an eine andere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz angefügt wird, wo der Anmeldename nie erstellt wurde. Ein Datenbankbenutzer kann auch zu einem verwaisten Benutzer werden, wenn der entsprechende [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldename gelöscht wird. Selbst wenn der Anmeldename neu erstellt wird, verfügt er über eine andere SID, sodass der Datenbankbenutzer verweist bleibt.  
  
## <a name="detect-orphaned-users"></a>Ermitteln von verwaisten Benutzern  

**Für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und PDW**

Führen Sie die folgende Anweisung in der Benutzerdatenbank aus, um verwaiste Benutzer in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] über eine Suche nach fehlenden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldenamen zu ermitteln:  
  
```  
SELECT dp.type_desc, dp.SID, dp.name AS user_name  
FROM sys.database_principals AS dp  
LEFT JOIN sys.server_principals AS sp  
    ON dp.SID = sp.SID  
WHERE sp.SID IS NULL  
    AND authentication_type_desc = 'INSTANCE';  
```  
  
 Die Ausgabe führt alle Benutzer der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung und entsprechende Sicherheits-IDs (SID), die mit keinem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldenamen verknüpft sind, in der aktuellen Datenbank auf.  

**Für SQL-Datenbank und Azure Synapse Analytics**

Die `sys.server_principals`-Tabelle ist in der SQL-Datenbank oder in Azure Synapse Analytics nicht verfügbar. Ermitteln Sie verwaiste Benutzer in diesen Umgebungen, indem Sie die folgenden Schritte ausführen:

1. Stellen Sie eine Verbindung mit der `master` -Datenbank her, und wählen Sie mit der folgenden Abfrage die SIDs für die Anmeldenamen:
    ```
    SELECT sid 
    FROM sys.sql_logins 
    WHERE type = 'S'; 
    ```

2. Stellen Sie eine Verbindung mit der Benutzerdatenbank her, und überprüfen Sie mit der folgenden Abfrage die SIDs der Benutzer in der `sys.database_principals` -Tabelle:

    ```
    SELECT name, sid, principal_id
    FROM sys.database_principals 
    WHERE type = 'S' 
      AND name NOT IN ('guest', 'INFORMATION_SCHEMA', 'sys')
      AND authentication_type_desc = 'INSTANCE';
    ```

3. Vergleichen Sie die beiden Listen, um festzustellen, ob die Tabelle `sys.database_principals` der Benutzerdatenbank SIDs enthält, für die in der Tabelle `sql_logins` der Masterdatenbank keine entsprechenden Anmelde-SIDs vorhanden sind. 
  
## <a name="resolve-an-orphaned-user"></a>Entfernen eines verwaisten Benutzers  
Verwenden Sie in der Masterdatenbank die Anweisung [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) zusammen mit der SID-Option, um einen fehlenden Anmeldenamen neu zu erstellen und geben Sie die `SID` des Datenbankbenutzers an, die Sie im vorherigen Abschnitt erhalten haben:  
  
```  
CREATE LOGIN <login_name>   
WITH PASSWORD = '<use_a_strong_password_here>',  
SID = <SID>;  
```  
  
 Führen Sie in der Benutzerdatenbank die Anweisung **ALTER USER** aus, und geben Sie den Anmeldenamen ein, um verwaiste Benutzer einem Anmeldenamen zuzuordnen, der bereits im [Master](../../t-sql/statements/alter-user-transact-sql.md) vorhanden ist.  
  
```  
ALTER USER <user_name> WITH Login = <login_name>;  
```  
  
 Wenn Sie einen fehlenden Anmeldenamen neu erstellen, kann der Benutzer mithilfe des angegebenen Kennworts auf die Datenbank zugreifen. Der Benutzer kann dann das Kennwort für das Anmeldekonto mit der ALTER LOGIN-Anweisung ändern.  
  
```  
ALTER LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';  
```  
  
> [!IMPORTANT]  
>  Bei jedem Anmeldenamen kann das eigene Kennwort geändert werden. Nur Anmeldenamen mit der Berechtigung `ALTER ANY LOGIN` können auch die Kennwörter von anderen Benutzern ändern. Allerdings können die Kennwörter von Mitgliedern der **sysadmin** -Rolle nur von Mitgliedern der **sysadmin** -Rolle geändert werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [ALTER USER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-user-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sp_change_users_login &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-users-login-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_password &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-password-transact-sql.md)   
 [sys.sysusers &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysusers-transact-sql.md)   
 [sys.sql_logins](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md) [sys.syslogins &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslogins-transact-sql.md)  
  
  
