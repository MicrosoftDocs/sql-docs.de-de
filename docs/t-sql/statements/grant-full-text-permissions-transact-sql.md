---
description: GRANT-Berechtigungen für Volltext (Transact-SQL)
title: GRANT-Berechtigungen für Volltext (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], full-text
- full-text search [SQL Server], permissions
- full-text catalogs [SQL Server], permissions
- full-text stoplist [SQL Server], permissions
- GRANT statement, full-text permissions
ms.assetid: fdb64e09-222a-47fe-b08b-999264ca261d
author: MightyPen
ms.author: genemi
ms.openlocfilehash: d966e11f3bb545ec4ac71a412958fc36de6885ee
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "91497744"
---
# <a name="grant-full-text-permissions-transact-sql"></a>GRANT-Berechtigungen für Volltext (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Erteilt Berechtigungen für einen Volltextkatalog oder eine Volltextstoppliste.  
  

  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
GRANT permission [ ,...n ] ON  
    FULLTEXT   
        {  
           CATALOG :: full-text_catalog_name  
           |  
           STOPLIST :: full-text_stoplist_name  
        }  
    TO database_principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
    [ AS granting_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *permission*  
 Der Name einer Berechtigung. Die gültigen Zuordnungen von Berechtigungen zu sicherungsfähigen Elementen werden im Abschnitt mit den Hinweisen weiter unten in diesem Thema beschrieben.  
  
 ON FULLTEXT CATALOG **::** _Volltext-Katalogname_  
 Gibt den Volltextkatalog an, für den die Berechtigung erteilt wird. Der Bereichsqualifizierer **::** ist erforderlich.  
  
 ON FULLTEXT STOPLIST **::** _Volltext-Stopplistenname_  
 Gibt die Volltextstoppliste an, für die die Berechtigung erteilt wird. Der Bereichsqualifizierer **::** ist erforderlich.  
  
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
  
## <a name="remarks"></a>Hinweise  
  
## <a name="fulltext-catalog-permissions"></a>FULLTEXT CATALOG-Berechtigungen  
 Ein Volltextkatalog ist ein auf der Datenbankebene sicherungsfähiges Element, das in der Datenbank enthalten ist, die das übergeordnete Element in der Berechtigungshierarchie darstellt. Die spezifischsten und restriktivsten Berechtigungen, die für einen Volltextkatalog erteilt werden können, sind unten aufgeführt. Auch die allgemeineren Berechtigungen sind aufgeführt, die diese implizit enthalten.  
  
|Berechtigung für Volltextkataloge|Impliziert durch Berechtigung für Volltextkatalog|Impliziert durch Datenbankberechtigung|  
|-----------------------------------|----------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY FULLTEXT CATALOG|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="fulltext-stoplist-permissions"></a>FULLTEXT STOPLIST-Berechtigungen  
 Eine Volltextstoppliste ist ein auf der Datenbankebene sicherungsfähiges Element, das in der Datenbank enthalten ist, die das übergeordnete Element in der Berechtigungshierarchie darstellt. Die spezifischsten und restriktivsten Berechtigungen, die für eine Volltextstoppliste erteilt werden können, sind unten aufgeführt. Auch die allgemeineren Berechtigungen sind aufgeführt, die diese implizit enthalten.  
  
|Berechtigung für Volltextstopplisten|Impliziert durch Berechtigung für Volltextstopplisten|Impliziert durch Datenbankberechtigung|  
|------------------------------------|-----------------------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY FULLTEXT CATALOG|  
|CONTROL|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Berechtigungen  
 Der Berechtigende (oder der mit der AS-Option angegebene Prinzipal) benötigt entweder die Berechtigung selbst mit GRANT OPTION oder eine höhere Berechtigung, die die erteilte Berechtigung impliziert.  
  
 Wenn Sie die Option AS verwenden, gelten die folgenden zusätzlichen Anforderungen.  
  
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
  
 Empfänger der CONTROL SERVER-Berechtigung, wie z. B. Mitglieder der festen Serverrolle sysadmin, können jede beliebige Berechtigung für jedes beliebige sicherungsfähige Element auf dem Server erteilen. Empfänger der CONTROL-Berechtigung in einer Datenbank, z. B. Mitglieder der festen Datenbankrolle db_owner, können jede Berechtigung für ein beliebiges sicherungsfähiges Element in der Datenbank erteilen. Empfänger der CONTROL-Berechtigung für ein Schema können jede beliebige Berechtigung für jedes Objekt innerhalb des Schemas erteilen.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-granting-permissions-to-a-full-text-catalog"></a>A. Gewähren von Berechtigungen für einen Volltextkatalog  
 Im folgenden Beispiel wird `Ted` die `CONTROL`-Berechtigung für den `ProductCatalog`-Volltextkatalog erteilt.  
  
```sql  
GRANT CONTROL  
    ON FULLTEXT CATALOG :: ProductCatalog  
    TO Ted ;  
```  
  
### <a name="b-granting-permissions-to-a-stoplist"></a>B. Gewähren von Berechtigungen für eine Stoppliste  
 Im folgenden Beispiel wird `Mary` die `VIEW DEFINITION`-Berechtigung für die `ProductStoplist`-Volltextstoppliste erteilt.  
  
```sql  
GRANT VIEW DEFINITION  
    ON FULLTEXT STOPLIST :: ProductStoplist  
    TO Mary ;  
```  
  
## <a name="see-also"></a>Siehe auch  
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-stoplist-transact-sql.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [Berechtigungen &#40;Datenbank-Engine&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Prinzipale &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [sys.fulltext_stoplists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)  
  
  
