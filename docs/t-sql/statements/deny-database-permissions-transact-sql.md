---
description: DENY (Datenbankberechtigungen) (Transact-SQL)
title: DENY (Datenbankberechtigungen) (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- DENY statement, databases
- permissions [SQL Server], databases
- database permissions [SQL Server], denying
- denying permissions [SQL Server], databases
ms.assetid: 36cc4e2c-5a24-4975-9920-9305f12c6e7c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1287d86ceeab338a1feaedbd7b0be694021a70fe
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100342544"
---
# <a name="deny-database-permissions-transact-sql"></a>DENY (Datenbankberechtigungen) (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Verweigert Berechtigungen für eine Datenbank in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql
DENY <permission> [ ,...n ]
    TO <database_principal> [ ,...n ] [ CASCADE ]
    [ AS <database_principal> ]

<permission> ::=
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

*permission* Gibt eine Berechtigung an, die für eine Datenbank verweigert werden kann. Eine Liste der Berechtigungen finden Sie im Abschnitt zu den Hinweisen weiter unten in diesem Thema.

ALL Mit dieser Option werden nicht alle möglichen Berechtigungen verweigert. Das Verweigern mit der ALL-Option ist gleichbedeutend mit dem Verweigern der folgenden Berechtigungen: BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE und CREATE VIEW.

PRIVILEGES Aus Gründen der Kompatibilität mit ISO eingeschlossen. Ändert das Verhalten von ALL nicht.

CASCADE Gibt an, dass die Berechtigung auch für andere Prinzipale verweigert wird, denen der angegebene Prinzipal diese Berechtigung erteilt hat.

AS \<database_principal> Gibt einen Prinzipal an, von dem der Prinzipal, der diese Abfrage ausführt, das Recht zum Verweigern der Berechtigung ableitet.

*Database_user* Gibt einen Datenbankbenutzer an.

*Database_role* Gibt eine Datenbankrolle an.

*Application_role*
**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

Gibt eine Anwendungsrolle an.

*Database_user_mapped_to_Windows_User* Gibt einen Datenbankbenutzer an, der einem Windows-Benutzer zugeordnet ist.

*Database_user_mapped_to_Windows_Group* Gibt einen Datenbankbenutzer an, der einer Windows-Gruppe zugeordnet ist.

*Database_user_mapped_to_certificate* Gibt einen Datenbankbenutzer an, der einem Zertifikat zugeordnet ist.

*Database_user_mapped_to_asymmetric_key* Gibt einen Datenbankbenutzer an, der einem asymmetrischen Schlüssel zugeordnet ist.

*Database_user_with_no_login* Gibt einen Datenbankbenutzer ohne entsprechenden Prinzipal auf Serverebene an.

## <a name="remarks"></a>Bemerkungen

Eine Datenbank ist ein sicherungsfähiges Element auf dem Server, der das übergeordnete Element in der Berechtigungshierarchie ist. Die spezifischsten und restriktivsten Berechtigungen, die für eine Datenbank verweigert werden können, sind in der folgenden Tabelle aufgeführt. Außerdem enthält die Tabelle die allgemeineren Berechtigungen, die implizit enthalten sind.

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
|ALTER ANY DATABASE EVENT SESSION<br /> **Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|ALTER|ALTER ANY EVENT SESSION|
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
|ALTER ANY SECURITY POLICY<br /> **Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|CONTROL|CONTROL SERVER|
|ALTER ANY SCHEMA|ALTER|CONTROL SERVER|
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
|INSERT|CONTROL|CONTROL SERVER|
|KILL DATABASE CONNECTION<br /> **Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|CONTROL|ALTER ANY CONNECTION|
|REFERENCES|CONTROL|CONTROL SERVER|
|SELECT|CONTROL|CONTROL SERVER|
|SHOWPLAN|CONTROL|ALTER TRACE|
|SUBSCRIBE QUERY NOTIFICATIONS|CONTROL|CONTROL SERVER|
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|
|UNMASK|CONTROL|CONTROL SERVER|
|UPDATE|CONTROL|CONTROL SERVER|
|VIEW ANY COLUMN ENCRYPTION KEY|CONTROL|VIEW ANY DEFINITION|
|VIEW ANY MASTER KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|
|VIEW DATABASE STATE|CONTROL|VIEW SERVER STATE|
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|

## <a name="permissions"></a>Berechtigungen

Der Prinzipal, der diese Anweisung ausführt (oder der mit der AS-Option angegebene Prinzipal) muss die CONTROL-Berechtigung für die Datenbank oder eine höhere Berechtigung besitzen, die die CONTROL-Berechtigung für die Datenbank impliziert.

Falls die AS-Option verwendet wird, muss der angegebene Prinzipal die Datenbank besitzen.

## <a name="examples"></a>Beispiele

### <a name="a-denying-permission-to-create-certificates"></a>A. Verweigern der Berechtigung zum Erstellen von Zertifikaten

Im folgenden Beispiel wird die `CREATE CERTIFICATE`-Berechtigung für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank für den Benutzer `MelanieK` verweigert.

```sql
USE AdventureWorks2012;
DENY CREATE CERTIFICATE TO MelanieK;
GO
```

### <a name="b-denying-references-permission-to-an-application-role"></a>B. Verweigern der REFERENCES-Berechtigung für eine Anwendungsrolle

Im folgenden Beispiel wird die `REFERENCES`-Berechtigung für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank für die Anwendungsrolle `AuditMonitor` verweigert.

**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

```sql
USE AdventureWorks2012;
DENY REFERENCES TO AuditMonitor;
GO
```

### <a name="c-denying-view-definition-with-cascade"></a>C. Verweigern von VIEW DEFINITION mit CASCADE

Im folgenden Beispiel wird die Berechtigung `VIEW DEFINITION` für die Datenbank [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] für Benutzer `CarmineEs` und für alle Prinzipale verweigert, denen von `CarmineEs` die Berechtigung `VIEW DEFINITION` erteilt wurde.

```sql
USE AdventureWorks2012;
DENY VIEW DEFINITION TO CarmineEs CASCADE;
GO
```

## <a name="see-also"></a>Weitere Informationen

- [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)
- [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [GRANT](../../t-sql/statements/grant-transact-sql.md)
- [Berechtigungen](../../relational-databases/security/permissions-database-engine.md)
- [Principals](../../relational-databases/security/authentication-access/principals-database-engine.md)
