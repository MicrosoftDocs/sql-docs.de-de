---
description: sys.fn_my_permissions (Transact-SQL)
title: sys.fn_my_permissions (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_my_permissions_TSQL
- fn_my_permissions_TSQL
- sys.fn_my_permissions
- fn_my_permissions
dev_langs:
- TSQL
helpviewer_keywords:
- fn_my_permissions function
- sys.fn_my_permissions function
ms.assetid: 30f97f00-03d8-443a-9de9-9ec420b7699b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 65596436cf44b2c09f344defa1d5e03768db794d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201915"
---
# <a name="sysfn_my_permissions-transact-sql"></a>sys.fn_my_permissions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Liste der Berechtigungen zurück, die dem Prinzipal eines sicherungsfähigen Elements effektiv gewährt wurden. Eine verwandte Funktion ist [HAS_PERMS_BY_NAME](../../t-sql/functions/has-perms-by-name-transact-sql.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn_my_permissions ( securable , 'securable_class' )  
```  
  
## <a name="arguments"></a>Argumente  
 *securable*  
 Der Name des sicherungsfähigen Elements. Wenn das sicherungsfähige Element der Server oder eine Datenbank ist, sollte dieser Wert auf NULL festgelegt werden. *securable* ist ein Skalarausdruck vom Typ **sysname**. *Sicherungs fähigen* kann ein mehrteilige Name sein.  
  
 "*securable_class*"  
 Der Name der Klasse des sicherungsfähigen Elements, für das Berechtigungen aufgelistet werden. *securable_class* ist ein **vom Datentyp sysname**. *securable_class* muss einer der folgenden sein: Anwendungs Rolle, Assembly, asymmetrischer Schlüssel, Zertifikat, Vertrag, Datenbank, Endpunkt, voll Text Katalog, Anmeldung, Nachrichtentyp, Objekt, Remote Dienst Bindung, Rolle, Route, Schema, Server, Dienst, symmetrischer Schlüssel, Typ, Benutzer, XML-Schema Auflistung.  
  
## <a name="columns-returned"></a>Zurückgegebene Spalten  
 In der folgenden Tabelle werden die Spalten aufgelistet, die **fn_my_permissions** zurückgibt. Jede zurückgegebene Zeile beschreibt eine Berechtigung, über die der aktuelle Sicherheitskontext für das sicherungsfähige Element verfügt. Gibt NULL zurück, wenn die Abfrage einen Fehler erzeugt.  
  
|Spaltenname|type|BESCHREIBUNG|  
|-----------------|----------|-----------------|  
|Entitätsname|**sysname**|Der Name des sicherungsfähigen Elements, für das die aufgelisteten Berechtigungen effektiv gewährt wurden.|  
|subentity_name|**sysname**|Der Spaltenname, sofern das sicherungsfähige Element über Spalten verfügt; andernfalls NULL.|  
|permission_name|**nvarchar**|Der Name der Berechtigung.|  
  
## <a name="remarks"></a>Bemerkungen  
 Diese Tabellenwertfunktion gibt eine Liste der gültigen Berechtigungen zurück, über die der aufrufende Prinzipal für ein bestimmtes sicherungsfähiges Element verfügt. Eine gültige Berechtigung ist eine der folgenden:  
  
-   Eine Berechtigung, die dem Prinzipal direkt gewährt und nicht verweigert wurde.  
  
-   Eine Berechtigung, die durch eine Berechtigung auf höherer Ebene des Prinzipals impliziert wird und nicht verweigert wird.  
  
-   Eine Berechtigung, die einer Rolle oder Gruppe erteilt wird, der der Prinzipal als Mitglied angehört, und nicht verweigert wird.  
  
-   Eine Berechtigung einer Rolle oder Gruppe, der der Prinzipal als Mitglied angehört, und die nicht verweigert wird.  
  
 Die Berechtigungsauswertung erfolgt immer im Sicherheitskontext des Aufrufers. Um zu ermitteln, ob ein anderer Prinzipal über eine gültige Berechtigung verfügt, muss der Aufrufer über die IMPERSONATE-Berechtigung für diesen Prinzipal verfügen.  
  
 Für Entitäten auf Schemaebene werden ein-, zwei- oder dreiteilige Namen akzeptiert, die ungleich NULL sind. Bei Entitäten auf Datenbankebene wird ein einteilige Name akzeptiert, bei dem ein NULL-Wert "*Current Database*" heißt. Für den Server selbst ist ein NULL-Wert (steht für "aktueller Server") erforderlich. **fn_my_permissions** können die Berechtigungen auf einem Verbindungs Server nicht überprüfen.  
  
 Die folgende Abfrage gibt eine Liste integrierter sicherungsfähiger Klassen zurück:  
  
```  
SELECT DISTINCT class_desc FROM fn_builtin_permissions(default)  
    ORDER BY class_desc;  
GO  
```  
  
 Wenn Default als Wert von *Sicherungs fähigen* oder *securable_class* angegeben wird, wird der Wert als NULL interpretiert.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-listing-effective-permissions-on-the-server"></a>A. Auflisten der gültigen Berechtigungen auf dem Server  
 Das folgende Beispiel gibt eine Liste der gültigen Berechtigungen des Aufrufers auf dem Server zurück.  
  
```  
SELECT * FROM fn_my_permissions(NULL, 'SERVER');  
GO  
```  
  
### <a name="b-listing-effective-permissions-on-the-database"></a>B. Auflisten der gültigen Berechtigungen für die Datenbank  
 Das folgende Beispiel gibt eine Liste der gültigen Berechtigungen des Aufrufers für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank zurück.  
  
```  
USE AdventureWorks2012;  
SELECT * FROM fn_my_permissions (NULL, 'DATABASE');  
GO  
```  
  
### <a name="c-listing-effective-permissions-on-a-view"></a>C. Auflisten der gültigen Berechtigungen für eine Sicht  
 Das folgende Beispiel gibt eine Liste der gültigen Berechtigungen des Aufrufers für die `vIndividualCustomer`-Sicht im `Sales`-Schema der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank zurück.  
  
```  
USE AdventureWorks2012;  
SELECT * FROM fn_my_permissions('Sales.vIndividualCustomer', 'OBJECT')   
    ORDER BY subentity_name, permission_name ;   
GO   
```  
  
### <a name="d-listing-effective-permissions-of-another-user"></a>D: Auflisten der gültigen Berechtigungen eines anderen Benutzers  
 Das folgende Beispiel gibt eine Liste der gültigen Berechtigungen des Datenbankbenutzers `Wanida` für die `Employee`-Tabelle im `HumanResources`-Schema der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank zurück. Der Aufrufer benötigt die IMPERSONATE-Berechtigung für den Benutzer `Wanida`.  
  
```  
EXECUTE AS USER = 'Wanida';  
SELECT * FROM fn_my_permissions('HumanResources.Employee', 'OBJECT')   
    ORDER BY subentity_name, permission_name ;    
REVERT;  
GO  
```  
  
### <a name="e-listing-effective-permissions-on-a-certificate"></a>E. Auflisten der gültigen Berechtigungen für ein Zertifikat  
 Das folgende Beispiel gibt eine Liste der gültigen Berechtigungen des Aufrufers für ein Zertifikat namens `Shipping47` in der aktuellen Datenbank zurück.  
  
```  
SELECT * FROM fn_my_permissions('Shipping47', 'CERTIFICATE');  
GO  
```  
  
### <a name="f-listing-effective-permissions-on-an-xml-schema-collection"></a>F. Auflisten der gültigen Berechtigungen für eine XML-Schemaauflistung  
 Im folgenden Beispiel wird eine Liste der effektiven Berechtigungen des Aufrufers für eine XML-Schema Auflistung mit dem Namen `ProductDescriptionSchemaCollection` in der-Datenbank zurückgegeben [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
```  
USE AdventureWorks2012;  
SELECT * FROM fn_my_permissions('ProductDescriptionSchemaCollection',  
    'XML SCHEMA COLLECTION');  
GO  
```  
  
### <a name="g-listing-effective-permissions-on-a-database-user"></a>G. Auflisten der gültigen Berechtigungen für einen Datenbankbenutzer  
 Das folgende Beispiel gibt eine Liste der gültigen Berechtigungen des Aufrufers für einen Benutzer namens `MalikAr` in der aktuellen Datenbank zurück.  
  
```  
SELECT * FROM fn_my_permissions('MalikAr', 'USER');  
GO  
```  
  
### <a name="h-listing-effective-permissions-of-another-login"></a>H. Auflisten der gültigen Berechtigungen eines anderen Anmeldenamens  
 Das folgende Beispiel gibt eine Liste der gültigen Berechtigungen des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamens `WanidaBenshoof` für die `Employee`-Tabelle im `HumanResources`-Schema der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank zurück. Der Aufrufer benötigt die IMPERSONATE-Berechtigung für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen `WanidaBenshoof`.  
  
```  
EXECUTE AS LOGIN = 'WanidaBenshoof';  
SELECT * FROM fn_my_permissions('AdventureWorks2012.HumanResources.Employee', 'OBJECT')   
    ORDER BY subentity_name, permission_name ;    
REVERT;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Sicherheitsfunktionen &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
 [Berechtigungen &#40;Datenbank-Engine&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [Berechtigungshierarchie &#40;Datenbank-Engine &#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [Sicherheitskatalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)  
  
  
