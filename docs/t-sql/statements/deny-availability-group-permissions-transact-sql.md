---
title: DENY – Verfügbarkeitsgruppenberechtigungen
description: Verweigern von Berechtigungen für eine Always On-Verfügbarkeitsgruppe
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], permissions
- permissions [SQL Server], availability group
- DENY statement, availability groups
- denying permissions, [SQL Server], availability groups
ms.assetid: bda60b36-a0b9-4c20-80c1-6a5cb1d638a5
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: aeb7315bead31911bdd9f62b9ef86a8ee1de9c97
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177789"
---
# <a name="deny-availability-group-permissions-transact-sql"></a>Verweigern von Verfügbarkeitsgruppenberechtigungen mit DENY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Verweigert Berechtigungen für eine Always On-Verfügbarkeitsgruppe in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DENY permission  [ ,...n ] ON AVAILABILITY GROUP :: availability_group_name  
        TO < server_principal >  [ ,...n ]  
    [ CASCADE ]  
    [ AS SQL_Server_login ]   
  
<server_principal> ::=   
        SQL_Server_login  
    | SQL_Server_login_from_Windows_login   
    | SQL_Server_login_from_certificate   
    | SQL_Server_login_from_AsymKey  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *permission*  
 Gibt eine Berechtigung an, die für eine Verfügbarkeitsgruppe verweigert werden kann. Eine Liste der Berechtigungen finden Sie im Abschnitt zu den Hinweisen weiter unten in diesem Thema.  
  
 ON AVAILABILITY GROUP **::** _availability_group_name_  
 Gibt die Verfügbarkeitsgruppe an, der die Berechtigung verweigert wird. Der Bereichsqualifizierer ( **::** ) ist erforderlich.  
  
 TO \<server_principal>  
 Gibt den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen an, für den die Berechtigung verweigert wird.  
  
 *SQL_Server_login*  
 Gibt einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen an.  
  
 *SQL_Server_login_from_Windows_login*  
 Gibt einen aus einem Windows-Anmeldenamen erstellten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen an.  
  
 *SQL_Server_login_from_certificate*  
 Gibt einen einem Zertifikat zugeordneten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen an.  
  
 *SQL_Server_login_from_AsymKey*  
 Gibt einen einem asymmetrischen Schlüssel zugeordneten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen an.  
  
 CASCADE  
 Gibt an, dass die verweigerte Berechtigung auch anderen Prinzipalen verweigert wird, denen diese Berechtigung von diesem Prinzipal erteilt wurde.  
  
 AS *SQL_Server_login*  
 Gibt einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen an, von dem der Prinzipal, der diese Abfrage ausführt, sein Recht zum Verweigern der Berechtigung ableitet.  
  
## <a name="remarks"></a>Bemerkungen  
 Berechtigungen im Serverbereich können nur verweigert werden, wenn **master** als aktuelle Datenbank verwendet wird.  
  
 Informationen zu Verfügbarkeitsgruppen sind in der [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)-Katalogsicht sichtbar. Informationen zu Serverberechtigungen sind in der Katalogsicht [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) sichtbar, Informationen zu Serverprinzipalen sind in der Katalogsicht [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) sichtbar.  
  
 Eine Verfügbarkeitsgruppe ist ein sicherungsfähiges Element auf Serverebene. Die spezifischsten und restriktivsten Berechtigungen, die für eine Verfügbarkeitsgruppe verweigert werden können, sind unten aufgeführt. Auch die allgemeineren Berechtigungen sind aufgeführt, die diese implizit enthalten.  
  
|Verfügbarkeitsgruppenberechtigung|Impliziert durch die Verfügbarkeitsgruppenberechtigung|Impliziert durch die Serverberechtigung|  
|-----------------------------------|----------------------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER ANY AVAILABILITY GROUP|  
|CONNECT|CONTROL|CONTROL SERVER|  
|CONTROL|CONTROL|CONTROL SERVER|  
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die CONTROL-Berechtigung für die Verfügbarkeitsgruppe oder die ALTER ANY AVAILABILITY GROUP-Berechtigung für den Server.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-denying-view-definition-permission-on-an-availability-group"></a>A. Verweigern der VIEW DEFINITION-Berechtigung für eine Verfügbarkeitsgruppe  
 Im folgenden Beispiel wird dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen `VIEW DEFINITION` die `MyAg`-Berechtigung für die Verfügbarkeitsgruppe `ZArifin` verweigert.  
  
```sql  
USE master;  
DENY VIEW DEFINITION ON AVAILABILITY GROUP::MyAg TO ZArifin;  
GO  
```  
  
### <a name="b-denying-take-ownership-permission-with-the-cascade-option"></a>B. Verweigern der TAKE OWNERSHIP-Berechtigung mit der CASCADE-Option  
 Im folgenden Beispiel wird dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Benutzer `TAKE OWNERSHIP` die `MyAg`-Berechtigung für die Verfügbarkeitsgruppe `PKomosinski` mit der `CASCADE`-Option verweigert.  
  
```sql  
USE master;  
DENY TAKE OWNERSHIP ON AVAILABILITY GROUP::MyAg TO PKomosinski   
    CASCADE;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [REVOKE (Verfügbarkeitsgruppenberechtigungen) &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-availability-group-permissions-transact-sql.md)   
 [GRANT (Verfügbarkeitsgruppenberechtigungen) &#40;Transact-SQL&#41;](../../t-sql/statements/grant-availability-group-permissions-transact-sql.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [Katalogsichten Always On-Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Berechtigungen &#40;Datenbank-Engine&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Prinzipale &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
