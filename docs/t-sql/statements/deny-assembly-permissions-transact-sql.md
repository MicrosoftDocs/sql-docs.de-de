---
description: DENY-Assemblyberechtigungen (Transact-SQL)
title: DENY (Assemblyberechtigungen) (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- assemblies [CLR integration], permissions
- DENY statement, assemblies
- denying permissions [SQL Server], assemblies
ms.assetid: be0896b3-f793-44c9-9390-1c6e58f33b2b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 1905b2a0c2eaa017249ac9ab7f86a6523c4366c2
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96131357"
---
# <a name="deny-assembly-permissions-transact-sql"></a>DENY-Assemblyberechtigungen (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Verweigert Berechtigungen für eine Assembly.  
  

  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
DENY { permission [ ,...n ] } ON ASSEMBLY :: assembly_name  
    TO database_principal [ ,...n ]   
        [ CASCADE ]  
        [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *permission*  
 Gibt eine Berechtigung an, die für eine Assembly verweigert werden kann. Unten aufgeführt.  
  
 ON ASSEMBLY **::** _assembly_name_  
 Gibt die Assembly an, für die die Berechtigung verweigert wird. Der Bereichsqualifizierer "::" ist erforderlich.  
  
 *database_principal*  
 Gibt den Prinzipal an, für den die Berechtigung verweigert wird. Einer der folgenden:  
  
-   Datenbankbenutzer  
  
-   Datenbankrolle (database role)  
  
-   Anwendungsrolle  
  
-   Einem Windows-Anmeldenamen zugeordneter Datenbankbenutzer  
  
-   Einer Windows-Gruppe zugeordneter Datenbankbenutzer  
  
-   Einem Zertifikat zugeordneter Datenbankbenutzer  
  
-   Einem asymmetrischen Schlüssel zugeordneter Datenbankbenutzer  
  
-   Keinem Serverprinzipal zugeordneter Datenbankbenutzer.  
  
 CASCADE  
 Gibt an, dass die verweigerte Berechtigung auch anderen Prinzipalen verweigert wird, denen diese Berechtigung von diesem Prinzipal erteilt wurde.  
  
 *denying_principal*  
 Gibt einen Prinzipal an, von dem der Prinzipal, der diese Abfrage ausführt, das Recht zum Verweigern der Berechtigung ableitet. Einer der folgenden:  
  
-   Datenbankbenutzer  
  
-   Datenbankrolle (database role)  
  
-   Anwendungsrolle  
  
-   Einem Windows-Anmeldenamen zugeordneter Datenbankbenutzer  
  
-   Einer Windows-Gruppe zugeordneter Datenbankbenutzer  
  
-   Einem Zertifikat zugeordneter Datenbankbenutzer  
  
-   Einem asymmetrischen Schlüssel zugeordneter Datenbankbenutzer  
  
-   Keinem Serverprinzipal zugeordneter Datenbankbenutzer.  
  
## <a name="remarks"></a>Bemerkungen  
 Eine Assembly ist ein auf der Datenbankebene sicherungsfähiges Element, das in der Datenbank enthalten ist, die das übergeordnete Element in der Berechtigungshierarchie darstellt. Die spezifischsten und restriktivsten Berechtigungen, die für eine Assembly verweigert werden können, sind im Folgenden aufgeführt, zusammen mit den allgemeineren Berechtigungen, die implizit enthalten sind.  
  
|Assemblyberechtigung|Impliziert durch Assemblyberechtigung|Impliziert durch Datenbankberechtigung|  
|-------------------------|------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ASSEMBLY|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die CONTROL-Berechtigung für die Assembly. Wenn Sie die Option AS verwenden, muss der angegebene Prinzipal Besitzer der Assembly sein.  
  
## <a name="see-also"></a>Weitere Informationen  
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [Berechtigungen &#40;Datenbank-Engine&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Prinzipale &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
