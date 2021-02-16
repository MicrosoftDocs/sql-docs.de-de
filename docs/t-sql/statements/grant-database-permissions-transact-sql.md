---
description: GRANT (Datenbankberechtigungen) (Transact-SQL)
title: GRANT (Datenbankberechtigungen) (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server]
- database permissions [SQL Server]
- granting permissions [SQL Server], databases
- permissions [SQL Server], databases
- database permissions [SQL Server], granting
- GRANT statement, databases
ms.assetid: 499e5ed6-945c-4791-ab45-68dec0b9c289
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 185e4f53346d61ec3d53462fec3797b50f20d4c5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338759"
---
# <a name="grant-database-permissions-transact-sql"></a>GRANT (Datenbankberechtigungen) (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Erteilt Berechtigungen für eine Datenbank in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql

GRANT <permission> [ ,...n ]
    TO <database_principal> [ ,...n ] [ WITH GRANT OPTION ]
    [ AS <database_principal> ]

<permission>::=
permission | ALL [ PRIVILEGES ]

<database_principal> ::=
    Database_user
  | Database_role
  | Application_role
  | Database_user_mapped_to_Windows_User
  | Database_user_mapped_to_Windows_Group
  | Database_user_mapped_to_certificate
  | Database_user_mapped_to_asymmetric_key
  | Database_user_with_no_login
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

*permission* Gibt eine Berechtigung an, die für eine Datenbank erteilt werden kann. Eine Liste der Berechtigungen finden Sie im Abschnitt zu den Hinweisen weiter unten in diesem Thema.

ALL Mit dieser Option werden nicht alle möglichen Berechtigungen erteilt. Das Erteilen mit ALL entspricht dem Erteilen der folgenden Berechtigungen: BACKUP DATABASE, BACKUP LOG, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE und CREATE VIEW.

PRIVILEGES Aus Gründen der Kompatibilität mit ANSI-92 eingeschlossen. Ändert das Verhalten von ALL nicht.

WITH GRANT OPTION Gibt an, dass der Prinzipal die angegebene Berechtigung auch anderen Prinzipalen erteilen kann.

AS \<database_principal> Gibt einen Prinzipal an, von dem der Prinzipal, der diese Abfrage ausführt, sein Recht zum Erteilen der Berechtigung ableitet.

*Database_user* Gibt einen Datenbankbenutzer an.

*Database_role* Gibt eine Datenbankrolle an.

*Application_role*
**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

Gibt eine Anwendungsrolle an.

*Database_user_mapped_to_Windows_User*
**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.

Gibt einen Datenbankbenutzer an, der einem Windows-Benutzer zugeordnet ist.

*Database_user_mapped_to_Windows_Group*
**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.

Gibt einen Datenbankbenutzer an, der einer Windows-Gruppe zugeordnet ist.

*Database_user_mapped_to_certificate*
**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.

Gibt einen Datenbankbenutzer an, der einem Zertifikat zugeordnet ist.

*Database_user_mapped_to_asymmetric_key*
**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.

Gibt einen Datenbankbenutzer an, der einem asymmetrischen Schlüssel zugeordnet ist.

*Database_user_with_no_login* Gibt einen Datenbankbenutzer ohne entsprechenden Prinzipal auf Serverebene an.

## <a name="remarks"></a>Bemerkungen

> [!IMPORTANT]
> Durch eine Kombination von ALTER- und REFERENCE-Berechtigungen würde in manchen Fällen zugelassen, dass der Empfänger Daten anzeigt oder Funktionen ausführt, für die er nicht autorisiert ist. Beispiel: Ein Benutzer mit ALTER-Berechtigung für eine Tabelle und REFERENCE-Berechtigung für eine Funktion kann eine berechnete Spalte für eine Funktion erstellen und ausführen lassen. In diesem Fall benötigt der Benutzer auch die SELECT-Berechtigung für die berechnete Spalte.

Eine Datenbank ist ein sicherungsfähiges Element auf dem Server, der das übergeordnete Element in der Berechtigungshierarchie ist. Die spezifischsten und restriktivsten Berechtigungen, die für eine Datenbank erteilt werden können, sind in der folgenden Tabelle aufgeführt. Außerdem enthält die Tabelle die allgemeineren Berechtigungen, die implizit enthalten sind.

|Datenbankberechtigung|Impliziert durch Datenbankberechtigung|Impliziert durch die Serverberechtigung|
|-------------------------|------------------------------------|----------------------------------|
|ADMINISTER DATABASE BULK OPERATIONS<br/>**Gilt für:** [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]|CONTROL|CONTROL SERVER|
|ALTER|CONTROL|ALTER ANY DATABASE|
|ALTER ANY APPLICATION ROLE|ALTER|CONTROL SERVER|
|ALTER ANY ASSEMBLY|ALTER|CONTROL SERVER|
|ALTER ANY ASYMMETRIC KEY|ALTER|CONTROL SERVER|
|ALTER ANY CERTIFICATE|ALTER|CONTROL SERVER|
|ALTER ANY COLUMN ENCRYPTION KEY|ALTER|CONTROL SERVER|
|ALTER ANY COLUMN MASTER KEY DEFINITION|ALTER|CONTROL SERVER|
|ALTER ANY CONTRACT|ALTER|CONTROL SERVER|
|ALTER ANY DATABASE AUDIT|ALTER|ALTER ANY SERVER AUDIT|
|ALTER ANY DATABASE DDL TRIGGER|ALTER|CONTROL SERVER|
|ALTER ANY DATABASE EVENT NOTIFICATION|ALTER|ALTER ANY EVENT NOTIFICATION|
|ALTER ANY DATABASE EVENT SESSION<br />**Gilt für**: [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].|ALTER|ALTER ANY EVENT SESSION|
|ALTER ANY DATABASE SCOPED CONFIGURATION<br /> **Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].|CONTROL|CONTROL SERVER|
|ALTER ANY DATASPACE|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL DATA SOURCE|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL FILE FORMAT|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL LIBRARY <br /> **Gilt für**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)].|CONTROL|CONTROL SERVER |
|ALTER ANY FULLTEXT CATALOG|ALTER|CONTROL SERVER|
|ALTER ANY MASK|CONTROL|CONTROL SERVER|
|ALTER ANY MESSAGE TYPE|ALTER|CONTROL SERVER|
|ALTER ANY REMOTE SERVICE BINDING|ALTER|CONTROL SERVER|
|ALTER ANY ROLE|ALTER|CONTROL SERVER|
|ALTER ANY ROUTE|ALTER|CONTROL SERVER|
|ALTER ANY SCHEMA|ALTER|CONTROL SERVER|
|ALTER ANY SECURITY POLICY<br /> **Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|CONTROL|CONTROL SERVER|
|ALTER ANY SERVICE|ALTER|CONTROL SERVER|
|ALTER ANY SYMMETRIC KEY|ALTER|CONTROL SERVER|
|ALTER ANY USER|ALTER|CONTROL SERVER|
|AUTHENTICATE|CONTROL|AUTHENTICATE SERVER|
|BACKUP DATABASE|CONTROL|CONTROL SERVER|
|BACKUP LOG|CONTROL|CONTROL SERVER|
|CHECKPOINT|CONTROL|CONTROL SERVER|
|CONNECT|CONNECT REPLICATION|CONTROL SERVER|
|CONNECT REPLICATION|CONTROL|CONTROL SERVER|
|CONTROL|CONTROL|CONTROL SERVER|
|CREATE AGGREGATE|ALTER|CONTROL SERVER|
|CREATE ANY EXTERNAL LIBRARY <br /> **Gilt für**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)].|CONTROL|CONTROL SERVER |
|CREATE ASSEMBLY|ALTER ANY ASSEMBLY|CONTROL SERVER|
|CREATE ASYMMETRIC KEY|ALTER ANY ASYMMETRIC KEY|CONTROL SERVER|
|CREATE CERTIFICATE|ALTER ANY CERTIFICATE|CONTROL SERVER|
|CREATE CONTRACT|ALTER ANY CONTRACT|CONTROL SERVER|
|CREATE DATABASE|CONTROL|CREATE ANY DATABASE|
|CREATE DATABASE DDL EVENT NOTIFICATION|ALTER ANY DATABASE EVENT NOTIFICATION|CREATE DDL EVENT NOTIFICATION|
|CREATE DEFAULT|ALTER|CONTROL SERVER|
|CREATE FULLTEXT CATALOG|ALTER ANY FULLTEXT CATALOG|CONTROL SERVER|
|CREATE FUNCTION|ALTER|CONTROL SERVER|
|CREATE MESSAGE TYPE|ALTER ANY MESSAGE TYPE|CONTROL SERVER|
|CREATE PROCEDURE|ALTER|CONTROL SERVER|
|CREATE QUEUE|ALTER|CONTROL SERVER|
|CREATE REMOTE SERVICE BINDING|ALTER ANY REMOTE SERVICE BINDING|CONTROL SERVER|
|CREATE ROLE|ALTER ANY ROLE|CONTROL SERVER|
|CREATE ROUTE|ALTER ANY ROUTE|CONTROL SERVER|
|CREATE RULE|ALTER|CONTROL SERVER|
|CREATE SCHEMA|ALTER ANY SCHEMA|CONTROL SERVER|
|CREATE SERVICE|ALTER ANY SERVICE|CONTROL SERVER|
|CREATE SYMMETRIC KEY|ALTER ANY SYMMETRIC KEY|CONTROL SERVER|
|CREATE SYNONYM|ALTER|CONTROL SERVER|
|CREATE TABLE|ALTER|CONTROL SERVER|
|CREATE TYPE|ALTER|CONTROL SERVER|
|CREATE VIEW|ALTER|CONTROL SERVER|
|CREATE XML SCHEMA COLLECTION|ALTER|CONTROL SERVER|
|Delete|CONTROL|CONTROL SERVER|
|Führen Sie|CONTROL|CONTROL SERVER|
|EXECUTE ANY EXTERNAL SCRIPT <br /> **Gilt für**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)].|CONTROL|CONTROL SERVER|
|EXECUTE EXTERNAL SCRIPT <br /> **Gilt für**: [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)].|EXECUTE ANY EXTERNAL SCRIPT|CONTROL SERVER|
|INSERT|CONTROL|CONTROL SERVER|
|KILL DATABASE CONNECTION<br />**Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|CONTROL|ALTER ANY CONNECTION|
|REFERENCES|CONTROL|CONTROL SERVER|
|SELECT|CONTROL|CONTROL SERVER|
|SHOWPLAN|CONTROL|ALTER TRACE|
|SUBSCRIBE QUERY NOTIFICATIONS|CONTROL|CONTROL SERVER|
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|
|UNMASK|CONTROL|CONTROL SERVER|
|UPDATE|CONTROL|CONTROL SERVER|
|VIEW ANY COLUMN ENCRYPTION KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|
|VIEW ANY COLUMN MASTER KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|
|VIEW DATABASE STATE|CONTROL|VIEW SERVER STATE|
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|

## <a name="permissions"></a>Berechtigungen

Der Berechtigende (oder der mit der AS-Option angegebene Prinzipal) benötigt entweder die Berechtigung selbst mit GRANT OPTION oder eine höhere Berechtigung, die die erteilte Berechtigung impliziert.

Falls die AS-Option verwendet wird, gelten die folgenden zusätzlichen Anforderungen.

|AS *granting_principal*|Zusätzliche Berechtigung erforderlich|
|------------------------------|------------------------------------|
|Datenbankbenutzer|IMPERSONATE-Berechtigung für den Benutzer, Mitgliedschaft in der festen Datenbankrolle db_securityadmin, Mitgliedschaft in der festen Datenbankrolle db_owner oder Mitgliedschaft in der festen Serverrolle sysadmin.|
|Einem Windows-Anmeldenamen zugeordneter Datenbankbenutzer|IMPERSONATE-Berechtigung für den Benutzer, Mitgliedschaft in der festen Datenbankrolle db_securityadmin, Mitgliedschaft in der festen Datenbankrolle db_owner oder Mitgliedschaft in der festen Serverrolle sysadmin.|
|Einer Windows-Gruppe zugeordneter Datenbankbenutzer|Mitgliedschaft in der Windows-Gruppe, Mitgliedschaft in der festen Datenbankrolle db_securityadmin, Mitgliedschaft in der festen Datenbankrolle db_owner oder Mitgliedschaft in der festen Serverrolle sysadmin.|
|Einem Zertifikat zugeordneter Datenbankbenutzer|Mitgliedschaft in der festen Datenbankrolle db_securityadmin, Mitgliedschaft in der festen Datenbankrolle db_owner oder Mitgliedschaft in der festen Serverrolle sysadmin.|
|Einem asymmetrischen Schlüssel zugeordneter Datenbankbenutzer|Mitgliedschaft in der festen Datenbankrolle db_securityadmin, Mitgliedschaft in der festen Datenbankrolle db_owner oder Mitgliedschaft in der festen Serverrolle sysadmin.|
|Einem Serverprinzipal zugeordneter Datenbankbenutzer|IMPERSONATE-Berechtigung für den Benutzer, Mitgliedschaft in der festen Datenbankrolle db_securityadmin, Mitgliedschaft in der festen Datenbankrolle db_owner oder Mitgliedschaft in der festen Serverrolle sysadmin.|
|Datenbankrolle|ALTER-Berechtigung für die Rolle, Mitgliedschaft in der festen Datenbankrolle db_securityadmin, Mitgliedschaft in der festen Datenbankrolle db_owner oder Mitgliedschaft in der festen Serverrolle sysadmin.|
|Anwendungsrolle|ALTER-Berechtigung für die Rolle, Mitgliedschaft in der festen Datenbankrolle db_securityadmin, Mitgliedschaft in der festen Datenbankrolle db_owner oder Mitgliedschaft in der festen Serverrolle sysadmin.|

Objektbesitzer können Berechtigungen für die Objekte erteilen, die sie besitzen. Prinzipale mit CONTROL-Berechtigung für ein sicherungsfähiges Element können die Berechtigung für dieses sicherungsfähige Element erteilen.

Empfänger der CONTROL SERVER-Berechtigung, wie z. B. Mitglieder der festen Serverrolle sysadmin, können jede beliebige Berechtigung für jedes beliebige sicherungsfähige Element auf dem Server erteilen.

## <a name="examples"></a>Beispiele

### <a name="a-granting-permission-to-create-tables"></a>A. Erteilen der Berechtigung zum Erstellen von Tabellen

Im folgenden Beispiel wird die `CREATE TABLE`-Berechtigung für die `AdventureWorks`-Datenbank dem Benutzer `MelanieK` erteilt.

```sql
USE AdventureWorks;
GRANT CREATE TABLE TO MelanieK;
GO
```

### <a name="b-granting-showplan-permission-to-an-application-role"></a>B. Erteilen der SHOWPLAN-Berechtigung für eine Anwendungsrolle

 Im folgenden Beispiel wird die `SHOWPLAN`-Berechtigung für die `AdventureWorks2012`-Datenbank der Anwendungsrolle `AuditMonitor` erteilt.

**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]

```sql
USE AdventureWorks2012;
GRANT SHOWPLAN TO AuditMonitor;
GO
```

### <a name="c-granting-create-view-with-grant-option"></a>C. Erteilen von CREATE VIEW mit GRANT OPTION

 Im folgenden Beispiel wird die `CREATE VIEW`-Berechtigung für die `AdventureWorks2012`-Datenbank dem Benutzer `CarmineEs` mit dem Recht zum Erteilen der `CREATE VIEW`-Berechtigung für andere Prinzipale erteilt.

```sql
USE AdventureWorks2012;
GRANT CREATE VIEW TO CarmineEs WITH GRANT OPTION;
GO
```

### <a name="d-granting-control-permission-to-a-database-user"></a>D: Erteilen einer CONTROL-Berechtigung für einen Datenbankbenutzer

 Im folgenden Beispiel wird die `CONTROL`-Berechtigung für die `AdventureWorks2012`-Datenbank dem Datenbankbenutzer `Sarah` erteilt. Der Benutzer muss in der Datenbank vorhanden und der Kontext muss auf die Datenbank festgelegt sein.

```sql
USE AdventureWorks2012;
GRANT CONTROL ON DATABASE::AdventureWorks2012 TO Sarah;
GO
```

## <a name="see-also"></a>Weitere Informationen

- [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)
- [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [GRANT](../../t-sql/statements/grant-transact-sql.md)
- [Berechtigungen](../../relational-databases/security/permissions-database-engine.md)
- [Principals](../../relational-databases/security/authentication-access/principals-database-engine.md)
