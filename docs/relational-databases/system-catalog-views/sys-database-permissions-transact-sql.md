---
description: sys.database_permissions (Transact-SQL)
title: sys.database_permissions (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_permissions
- sys.database_permissions_TSQL
- database_permissions_TSQL
- sys.database_permissions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_permissions catalog view
ms.assetid: c1e261f8-6cb0-4759-b5f1-5ec233602655
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a488dcf39ecf3ab0fa6e82264245d38f163f8ed6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209479"
---
# <a name="sysdatabase_permissions-transact-sql"></a>sys.database_permissions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile für jede Berechtigung oder Spaltenausnahmeberechtigung in der Datenbank zurück. Für Spalten gibt es eine Zeile für jede Berechtigung, die von der entsprechenden Berechtigung auf Objektebene abweicht. Wenn die Spalten Berechtigung mit der entsprechenden Objekt Berechtigung übereinstimmt, gibt es dafür keine Zeile, und die angewendete Berechtigung ist das des Objekts.  
  
> [!IMPORTANT]  
>  Berechtigungen auf Spaltenebene überschreiben Berechtigungen auf Objektebene in derselben Entität.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**class**|**tinyint**|Identifiziert die Klasse, für die die Berechtigung vorhanden ist.<br /><br /> 0 = Datenbank<br />1 = Objekt oder Spalte<br />3 = Schema<br />4 = Daten Bank Prinzipal<br />5 = Assembly- **gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br />6 = Typ<br />10 = XML-Schema Auflistung- <br />                      **Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br />15 = Nachrichtentyp: **gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br />16 = Dienstvertrag: **gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br />17 = Dienst: **gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br />18 = Remote Dienst Bindung: **gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br />19 = Route- **gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br />23 = voll Text Katalog- **gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br />24 = symmetrischer Schlüssel: **gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br />25 = Zertifikat- **gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br />26 = asymmetrischer Schlüssel: **gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.|  
|**class_desc**|**nvarchar(60)**|Beschreibung der Klasse, in der die Berechtigung vorhanden ist.<br /><br /> DATABASE<br /><br /> OBJECT_OR_COLUMN<br /><br /> SCHEMA<br /><br /> DATABASE_PRINCIPAL<br /><br /> ASSEMBLY<br /><br /> TYPE<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> MESSAGE_TYPE<br /><br /> SERVICE_CONTRACT<br /><br /> SERVICE<br /><br /> REMOTE_SERVICE_BINDING<br /><br /> ROUTE<br /><br /> FULLTEXT_CATALOG<br /><br /> SYMMETRIC_KEYS<br /><br /> CERTIFICATE<br /><br /> ASYMMETRIC_KEY|  
|**major_id**|**int**|ID des Objekts, für das die Berechtigung vorhanden ist, interpretiert nach der Klasse. In der Regel ist der **major_id** einfach die Art der ID, die für das darstellt, was die Klasse darstellt. <br /><br /> 0 = die Datenbank selbst <br /><br /> >0 = Object-IDs für Benutzer Objekte <br /><br /> \<0 = Object-IDs für Systemobjekte |  
|**minor_id**|**int**|Sekundäre ID des Objekts, für das die Berechtigung vorhanden ist, interpretiert nach der Klasse. Häufig ist der **minor_id** NULL, da keine Unterkategorie für die Klasse des Objekts verfügbar ist. Andernfalls handelt es sich um die Spalten-ID einer Tabelle.|  
|**grantee_principal_id**|**int**|Datenbankprinzipal-ID, der die Berechtigungen erteilt werden.|  
|**grantor_principal_id**|**int**|Datenbankprinzipal-ID des Berechtigenden dieser Berechtigungen.|  
|**type**|**char (4)**|Datenbank-Berechtigungstyp. Eine Liste der Berechtigungstypen finden Sie in der folgenden Tabelle.|  
|**permission_name**|**nvarchar(128)**|Berechtigungsname.|  
|**state**|**char(1)**|Der Berechtigungsstatus:<br /><br /> D = Verweigern<br /><br /> R = Aufheben<br /><br /> G = Erteilen<br /><br /> W = Erteilen mit WITH GRANT OPTION|  
|**state_desc**|**nvarchar(60)**|Beschreibung des Berechtigungsstatus:<br /><br /> VERWEIGERN<br /><br /> REVOKE<br /><br /> GRANT<br /><br /> GRANT_WITH_GRANT_OPTION|  

## <a name="database-permissions"></a>Datenbankberechtigungen   
Die folgenden Berechtigungs Typen sind möglich.
  
|Berechtigungstyp|Berechtigungsname|Betroffenes sicherungsfähiges Element|  
|---------------------|---------------------|--------------------------|  
|AADS |ALTER ANY DATABASE EVENT SESSION |DATABASE |  
|AAMK |ALTER ANY MASK |DATABASE |  
|AEDS |ALTER ANY EXTERNAL DATA SOURCE |DATABASE |  
|AEFF |ALTER ANY EXTERNAL FILE FORMAT |DATABASE |  
|AL|ALTER|APPLICATION ROLE, ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, REMOTE SERVICE BINDING, ROLE, ROUTE, SCHEMA, SERVICE, SYMMETRIC KEY, USER, XML SCHEMA COLLECTION|  
|ALAK|ALTER ANY ASYMMETRIC KEY|DATABASE|  
|ALAR|ALTER ANY APPLICATION ROLE|DATABASE|  
|ALAS|ALTER ANY ASSEMBLY|DATABASE|  
|ALCF|ALTER ANY CERTIFICATE|DATABASE|  
|ALDS|ALTER ANY DATASPACE|DATABASE|  
|ALED|ALTER ANY DATABASE EVENT NOTIFICATION|DATABASE|  
|ALFT|ALTER ANY FULLTEXT CATALOG|DATABASE|  
|ALMT|ALTER ANY MESSAGE TYPE|DATABASE|  
|ALRL|ALTER ANY ROLE|DATABASE|  
|ALRT|ALTER ANY ROUTE|DATABASE|  
|ALSB|ALTER ANY REMOTE SERVICE BINDING|DATABASE|  
|ALSC|ALTER ANY CONTRACT|DATABASE|  
|ALSK|ALTER ANY SYMMETRIC KEY|DATABASE|  
|ALSM|ALTER ANY SCHEMA|DATABASE|  
|ALSV|ALTER ANY SERVICE|DATABASE|  
|ALTG|ALTER ANY DATABASE DDL TRIGGER|DATABASE|  
|ALUS|ALTER ANY USER|DATABASE|  
|AUTH|AUTHENTICATE|DATABASE|  
|BADB|BACKUP DATABASE|DATABASE|  
|BALO|BACKUP LOG|DATABASE|  
|CL|CONTROL|APPLICATION ROLE, ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, REMOTE SERVICE BINDING, ROLE, ROUTE, SCHEMA, SERVICE, SYMMETRIC KEY, TYPE, USER, XML SCHEMA COLLECTION|  
|CO|CONNECT|DATABASE|  
|CORP|CONNECT REPLICATION|DATABASE|  
|CP|CHECKPOINT|DATABASE|  
|CRAG|CREATE AGGREGATE|DATABASE|  
|CRAK|CREATE ASYMMETRIC KEY|DATABASE|  
|CRAS|CREATE ASSEMBLY|DATABASE|  
|CRCF|CREATE CERTIFICATE|DATABASE|  
|CRDB|CREATE DATABASE|DATABASE|  
|CRDF|CREATE DEFAULT|DATABASE|  
|CRED|CREATE DATABASE DDL EVENT NOTIFICATION|DATABASE|  
|CRFN|CREATE FUNCTION|DATABASE|  
|CRFT|CREATE FULLTEXT CATALOG|DATABASE|  
|CRMT|CREATE MESSAGE TYPE|DATABASE|  
|CRPR|CREATE PROCEDURE|DATABASE|  
|CRQU|CREATE QUEUE|DATABASE|  
|CRRL|CREATE ROLE|DATABASE|  
|CRRT|CREATE ROUTE|DATABASE|  
|CRRU|CREATE RULE|DATABASE|  
|CRSB|CREATE REMOTE SERVICE BINDING|DATABASE|  
|CRSC|CREATE CONTRACT|DATABASE|  
|CRSK|CREATE SYMMETRIC KEY|DATABASE|  
|CRSM|CREATE SCHEMA|DATABASE|  
|CRSN|CREATE SYNONYM|DATABASE|  
|CRSO|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> CREATE SEQUENCE|DATABASE|  
|CRSV|CREATE SERVICE|DATABASE|  
|CRTB|CREATE TABLE|DATABASE|  
|CRTY|CREATE TYPE|DATABASE|  
|CRVW|CREATE VIEW|DATABASE|  
|CRXS|**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br /><br /> CREATE XML SCHEMA COLLECTION|DATABASE|  
|DABO |ADMINISTER DATABASE BULK OPERATIONS | DATABASE |
|DL|DELETE|DATABASE, OBJECT, SCHEMA|  
|EAES |EXECUTE ANY EXTERNAL SCRIPT |DATABASE |
|EX|Führen Sie|ASSEMBLY, DATABASE, OBJECT, SCHEMA, TYPE, XML SCHEMA COLLECTION|  
|IM|IMPERSONATE|USER|  
|IN|INSERT|DATABASE, OBJECT, SCHEMA|  
|RC|RECEIVE|OBJECT|  
|RF|REFERENCES|ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, SCHEMA, SYMMETRIC KEY, TYPE, XML SCHEMA COLLECTION|  
|SL|SELECT|DATABASE, OBJECT, SCHEMA|  
|SN|SEND|SERVICE|  
|SPLN|SHOWPLAN|DATABASE|  
|SUQN|SUBSCRIBE QUERY NOTIFICATIONS|DATABASE|  
|TO|TAKE OWNERSHIP|ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, REMOTE SERVICE BINDING, ROLE, ROUTE, SCHEMA, SERVICE, SYMMETRIC KEY, TYPE, XML SCHEMA COLLECTION|  
|UP|UPDATE|DATABASE, OBJECT, SCHEMA|  
|VW|VIEW DEFINITION|APPLICATION ROLE, ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, REMOTE SERVICE BINDING, ROLE, ROUTE, SCHEMA, SERVICE, SYMMETRIC KEY, TYPE, USER, XML SCHEMA COLLECTION|  
|VWCK |VIEW ANY COLUMN ENCRYPTION KEY DEFINITION|DATABASE |  
|VWCM |VIEW ANY COLUMN MASTER KEY DEFINITION|DATABASE |  
|VWCT|VIEW CHANGE TRACKING|TABLE, SCHEMA|  
|VWDS|VIEW DATABASE STATE|DATABASE|  
  
## <a name="permissions"></a>Berechtigungen  
 Jedem Benutzer werden die eigenen Berechtigungen angezeigt. Zum Anzeigen der Berechtigungen für andere Benutzer ist VIEW DEFINITION, ALTER ANY USER oder eine Berechtigung für einen Benutzer erforderlich. Zum Anzeigen benutzerdefinierter Rollen ist ALTER ANY ROLE oder die Mitgliedschaft bei der Rolle erforderlich (z. B. public).  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-listing-all-the-permissions-of-database-principals"></a>A: Auflisten aller Berechtigungen von Datenbankprinzipalen  
 Mit der folgenden Abfrage werden die Berechtigungen aufgelistet, die Datenbankprinzipalen ausdrücklich gewährt oder verweigert wurden.  
  
> [!IMPORTANT]  
>  Die Berechtigungen von festen Datenbankrollen werden nicht in sys.database_permissions aufgeführt. Daher können Datenbankprinzipale über zusätzliche Berechtigungen verfügen, die hier nicht aufgeführt werden.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc, pe.permission_name  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
### <a name="b-listing-permissions-on-schema-objects-within-a-database"></a>G. Auflisten von Berechtigungen für Schemaobjekte in einer Datenbank  
 Die folgende Abfrage verknüpft sys.database_principals und sys.database_permissions mit sys.objects und sys.schemas, um Berechtigungen aufzulisten, die bestimmten Schemaobjekten gewährt oder verweigert wurden.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc,   
    pe.permission_name, s.name + '.' + o.name AS ObjectName  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id  
JOIN sys.objects AS o  
    ON pe.major_id = o.object_id  
JOIN sys.schemas AS s  
    ON o.schema_id = s.schema_id;  
```  
    
  
## <a name="see-also"></a>Weitere Informationen  
 [Securables](../../relational-databases/security/securables.md)   
 [Berechtigungshierarchie &#40;Datenbank-Engine &#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [Sicherheitskatalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  


