---
description: sp_column_privileges (Transact-SQL)
title: sp_column_privileges (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_column_privileges_TSQL
- sp_column_privileges
dev_langs:
- TSQL
helpviewer_keywords:
- sp_column_privileges
ms.assetid: a3784301-2517-4b1d-bbd9-47404483fad0
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 279789c40dbc79dd3d7b2d421d757a936b0e6126
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482401"
---
# <a name="sp_column_privileges-transact-sql"></a>sp_column_privileges (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt Informationen zu Spaltenprivilegien für eine einzelne Tabelle in der aktuellen Umgebung zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_column_privileges [ @table_name = ] 'table_name'   
     [ , [ @table_owner = ] 'table_owner' ]   
     [ , [ @table_qualifier = ] 'table_qualifier' ]   
     [ , [ @column_name = ] 'column' ]  
```  
  
## <a name="arguments"></a>Argumente  
 [ @table_name =] '*table_name*'  
 Die Tabelle zum Zurückgeben von Kataloginformationen. *table_name* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert. Mustervergleiche mit Platzhalterzeichen werden nicht unterstützt.  
  
 [ @table_owner =] '*TABLE_OWNER*'  
 Der Besitzer der Tabelle, die zum Zurückgeben von Kataloginformationen verwendet wird. *TABLE_OWNER* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Mustervergleiche mit Platzhalterzeichen werden nicht unterstützt. Wenn *TABLE_OWNER* nicht angegeben ist, gelten die Standardregeln für die Tabellen Sichtbarkeit des zugrunde liegenden Datenbank-Management Systems (DBMS).  
  
 Wenn der aktuelle Benutzer eine Tabelle mit dem angegebenen Namen besitzt, werden die Spalten dieser Tabelle zurückgegeben. Wenn *TABLE_OWNER* nicht angegeben wird und der aktuelle Benutzer keine Tabelle mit dem angegebenen *table_name* besitzt, sucht sp_column Berechtigungen nach einer Tabelle mit der angegebenen *table_name* , die im Besitz des Daten Bank Besitzers ist. Sofern eine solche Tabelle vorhanden ist, werden die Spalten dieser Tabelle zurückgegeben.  
  
 [ @table_qualifier =] '*TABLE_QUALIFIER*'  
 Der Name des Tabellenqualifizierers. *TABLE_QUALIFIER* ist vom *Datentyp vom Datentyp sysname* und hat den Standardwert NULL. Verschiedene DBMS-Produkte unterstützen eine dreiteilige Benennung für Tabellen (_Qualifizierer_)**.** _Besitzer_**.** _Name_). In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt diese Spalte den Datenbanknamen dar. Bei anderen Produkten stellt sie den Servernamen der Datenbankumgebung für die Tabelle dar.  
  
 [ @column_name =] '*Spalte*'  
 Eine einzelne Spalte, die verwendet wird, wenn nur eine Spalte mit Kataloginformationen empfangen wird. die Spalte ist vom *Datentyp* **nvarchar (** 384 **)** und hat den Standardwert NULL. Wenn *Column* nicht angegeben wird, werden alle Spalten zurückgegeben. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt *Column* den Spaltennamen dar, der in der sys. columns-Tabelle aufgelistet ist. die *Spalte* kann Platzhalter Zeichen enthalten, die Platzhalter übereinstimmende Muster des zugrunde liegenden DBMS verwenden. Für eine optimale Interoperabilität sollte der Gatewayclient nur einen ISO-Standardmustervergleich voraussetzen (die Platzhalterzeichen % und _).  
  
## <a name="result-sets"></a>Resultsets  
 sp_column_privileges entspricht SQLColumnPrivileges in ODBC. Die zurückgegebenen Ergebnisse sind nach den Spalten TABLE_QUALIFIER, TABLE_OWNER, TABLE_NAME, COLUMN_NAME und PRIVILEGE sortiert.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|TABLE_QUALIFIER|**sysname**|Tabellen qualifizierername. Dieses Feld kann den Wert NULL annehmen.|  
|TABLE_OWNER|**sysname**|Der Name des Tabellen Besitzers. Dieses Feld gibt immer einen Wert zurück.|  
|table_name|**sysname**|Tabellenname. Dieses Feld gibt immer einen Wert zurück.|  
|COLUMN_NAME|**sysname**|Der Name der Spalte für jede Spalte des zurückgegebenen TABLE_NAME. Dieses Feld gibt immer einen Wert zurück.|  
|GRANTOR|**sysname**|Der Datenbank-Benutzername, der dem als GRANTEE aufgeführten Prinzipal Berechtigungen für die mit COLUMN_NAME angegebene Spalte erteilt hat. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist diese Spalte stets mit dem TABLE_OWNER identisch. Dieses Feld gibt immer einen Wert zurück.<br /><br /> Die GRANTOR-Spalte kann entweder der Datenbankbesitzer (TABLE_OWNER) oder ein anderer Benutzer sein, dem die Berechtigungen vom Datenbankbesitzer mithilfe der WITH GRANT OPTION-Klausel in der GRANT-Anweisung erteilt wurden.|  
|GRANTEE|**sysname**|Der Datenbank-Benutzername, dem der als GRANTOR aufgeführte Prinzipal Berechtigungen für die mit COLUMN_NAME angegebene Spalte erteilt hat. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] enthält diese Spalte immer einen Datenbankbenutzer aus der sysusers-Tabelle. Dieses Feld gibt immer einen Wert zurück.|  
|PRIVILEGE|**varchar (** 32 **)**|Eine der verfügbaren Spaltenberechtigungen. Spaltenberechtigungen können folgende Werte annehmen (oder auch andere Werte, die von der Datenquelle bei der Definition der Implementierung unterstützt werden):<br /><br /> SELECT = der Berechtigte (GRANTEE) kann Daten für die Spalten abrufen.<br /><br /> INSERT = der Berechtigte (GRANTEE) kann Daten für diese Spalten bereitstellen, wenn neue Zeilen (von diesem GRANTEE) in die Tabelle eingefügt werden.<br /><br /> UPDATE = der Berechtigte (GRANTEE) kann vorhandene Daten in der Spalte ändern.<br /><br /> REFERENCES = der Berechtigte (GRANTEE) kann bei einer Primär-/Fremdschlüssel-Beziehung auf eine Spalte in einer Fremdschlüsseltabelle verweisen. Primär-/Fremdschlüssel-Beziehungen werden mithilfe von Tabelleneinschränkungen definiert.|  
|IS_GRANTABLE|**varchar (** 3 **)**|Zeigt an, ob der Berechtigte (GRANTEE) anderen Benutzern Berechtigungen erteilen darf (bekannt als "Berechtigung mit Recht zum Erteilen"). Dieses Feld kann die Werte YES, NO oder NULL annehmen. Ein unbekannter Wert (oder NULL-Wert) verweist auf eine Datenquelle, für die die "Berechtigung mit Recht zum Erteilen" nicht zutreffend ist.|  
  
## <a name="remarks"></a>Hinweise  
 Bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden Berechtigungen mit der GRANT-Anweisung erteilt und mit der REVOKE-Anweisung aufgehoben.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert SELECT-Berechtigung für das Schema.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden Informationen zum Spaltenprivileg für eine bestimmte Spalte zurückgegeben.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_column_privileges @table_name = 'Employee'   
    ,@table_owner = 'HumanResources'  
    ,@table_qualifier = 'AdventureWorks2012'  
    ,@column_name = 'SalariedFlag';  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
