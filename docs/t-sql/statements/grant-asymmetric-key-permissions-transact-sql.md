---
description: GRANT (Berechtigungen für asymmetrische Schlüssel) (Transact-SQL)
title: GRANT (Berechtigungen für asymmetrische Schlüssel) (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], asymmetric keys
- permissions [SQL Server], asymmetric keys
- asymmetric keys [SQL Server], permissions
- GRANT statement, asymmetric keys
ms.assetid: a70e2ee6-59b0-4543-b883-e9cbae6199be
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: c0c2b21dc6c54ecba5d533aa8dc441758f2857cb
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2021
ms.locfileid: "103422011"
---
# <a name="grant-asymmetric-key-permissions-transact-sql"></a>GRANT (Berechtigungen für asymmetrische Schlüssel) (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Erteilt Berechtigungen für einen asymmetrischen Schlüssel.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
GRANT { permission  [ ,...n ] }   
    ON ASYMMETRIC KEY :: asymmetric_key_name   
       TO database_principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
    [ AS granting_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *permission*  
 Gibt eine Berechtigung an, die für einen asymmetrischen Schlüssel erteilt werden kann. Unten aufgeführt.  
  
 ON ASYMMETRIC KEY **::** _asymmetric_key_name_  
 Gibt den asymmetrischen Schlüssel an, für den die Berechtigung erteilt wird. Der Bereichsqualifizierer "::" ist erforderlich.  
  
 *database_principal*  
 Gibt den Prinzipal an, für den die Berechtigung erteilt wird. Einer der folgenden:  
  
-   Datenbankbenutzer  
-   Datenbankrolle (database role)  
-   Anwendungsrolle  
-   Einem Windows-Anmeldenamen zugeordneter Datenbankbenutzer  
-   Einer Windows-Gruppe zugeordneter Datenbankbenutzer  
-   Einem Zertifikat zugeordneter Datenbankbenutzer  
-   Einem asymmetrischen Schlüssel zugeordneter Datenbankbenutzer  
-   Keinem Serverprinzipal zugeordneter Datenbankbenutzer.  
  
GRANT OPTION  
 Gibt an, dass der Prinzipal die angegebene Berechtigung auch anderen Prinzipalen erteilen kann.  
  
AS *granting_principal*  
 Gibt einen Prinzipal an, von dem der Prinzipal, der diese Abfrage ausführt, sein Recht zum Erteilen der Berechtigung ableitet. Einer der folgenden:  
  
-   Datenbankbenutzer  
-   Datenbankrolle (database role)  
-   Anwendungsrolle  
-   Einem Windows-Anmeldenamen zugeordneter Datenbankbenutzer  
-   Einer Windows-Gruppe zugeordneter Datenbankbenutzer  
-   Einem Zertifikat zugeordneter Datenbankbenutzer  
-   Einem asymmetrischen Schlüssel zugeordneter Datenbankbenutzer  
-   Keinem Serverprinzipal zugeordneter Datenbankbenutzer.  
  
## <a name="remarks"></a>Bemerkungen  
 Ein asymmetrischer Schlüssel ist ein sicherungsfähiges Element auf Datenbankebene, das in der Datenbank enthalten ist, die dem Element in der Berechtigungshierarchie übergeordnet ist. Die spezifischsten und restriktivsten Berechtigungen, die für einen asymmetrischen Schlüssel erteilt werden können, sind im Folgenden aufgeführt, zusammen mit den allgemeineren Berechtigungen, die sie implizit enthalten.  
  
|Berechtigung für asymmetrische Schlüssel|Impliziert durch Berechtigung für asymmetrischen Schlüssel|Impliziert durch Datenbankberechtigung|  
|-------------------------------|------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ASYMMETRIC KEY|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Berechtigungen  
 Der Berechtigende (oder der mit der AS-Option angegebene Prinzipal) benötigt entweder die Berechtigung selbst mit GRANT OPTION oder eine höhere Berechtigung, die die erteilte Berechtigung impliziert.  
  
 Wenn Sie die Option AS verwenden, gelten die folgenden zusätzlichen Anforderungen.  
  
|AS *granting_principal*|Zusätzliche Berechtigung erforderlich|  
|------------------------------|------------------------------------|  
|Datenbankbenutzer|IMPERSONATE-Berechtigung für den Benutzer, die Mitgliedschaft in der festen Datenbankrolle **db_securityadmin** und **db_owner** oder die Mitgliedschaft in der festen Serverrolle **sysadmin**.|  
|Einem Windows-Anmeldenamen zugeordneter Datenbankbenutzer|IMPERSONATE-Berechtigung für den Benutzer, die Mitgliedschaft in der festen Datenbankrolle **db_securityadmin** und **db_owner** oder die Mitgliedschaft in der festen Serverrolle **sysadmin**.|  
|Einer Windows-Gruppe zugeordneter Datenbankbenutzer|Mitgliedschaft in der Windows-Gruppe, Mitgliedschaft in der festen Datenbankrolle **db_securityadmin**, Mitgliedschaft in der festen Datenbankrolle **db_owner** oder Mitgliedschaft in der festen Serverrolle **sysadmin**.|  
|Einem Zertifikat zugeordneter Datenbankbenutzer|Mitgliedschaft in der festen Datenbankrolle **db_securityadmin**, Mitgliedschaft in der festen Datenbankrolle **db_owner** oder Mitgliedschaft in der festen Serverrolle **sysadmin**.|  
|Einem asymmetrischen Schlüssel zugeordneter Datenbankbenutzer|Mitgliedschaft in der festen Datenbankrolle **db_securityadmin**, Mitgliedschaft in der festen Datenbankrolle **db_owner** oder Mitgliedschaft in der festen Serverrolle **sysadmin**.|  
|Einem Serverprinzipal zugeordneter Datenbankbenutzer|IMPERSONATE-Berechtigung für den Benutzer, die Mitgliedschaft in der festen Datenbankrolle **db_securityadmin** und **db_owner** oder die Mitgliedschaft in der festen Serverrolle **sysadmin**.|  
|Datenbankrolle|ALTER-Berechtigung für die Rolle, Mitgliedschaft in der festen Datenbankrolle **db_securityadmin**, Mitgliedschaft in der festen Datenbankrolle **db_owner** oder Mitgliedschaft in der festen Serverrolle **sysadmin**.|  
|Anwendungsrolle|ALTER-Berechtigung für die Rolle, Mitgliedschaft in der festen Datenbankrolle **db_securityadmin**, Mitgliedschaft in der festen Datenbankrolle **db_owner** oder Mitgliedschaft in der festen Serverrolle **sysadmin**.|  
  
 Objektbesitzer können Berechtigungen für die Objekte erteilen, die sie besitzen. Prinzipale mit CONTROL-Berechtigung für ein sicherungsfähiges Element können die Berechtigung für dieses sicherungsfähige Element erteilen.  
  
 Empfänger der CONTROL SERVER-Berechtigung, wie z.B. Mitglieder der festen Serverrolle **sysadmin**, können jede beliebige Berechtigung für jedes beliebige sicherungsfähige Element auf dem Server erteilen. Empfänger der CONTROL-Berechtigung für eine Datenbank, wie z.B. Mitglieder der festen Datenbankrolle **db_owner**, können jede beliebige Berechtigung für jedes beliebige sicherungsfähige Element in der Datenbank erteilen. Empfänger der CONTROL-Berechtigung für ein Schema können jede beliebige Berechtigung für jedes Objekt innerhalb des Schemas erteilen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [Berechtigungen &#40;Datenbank-Engine&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Prinzipale &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)  
  
  
