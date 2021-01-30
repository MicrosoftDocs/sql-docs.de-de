---
description: sp_dbfixedrolepermission (Transact-SQL)
title: sp_dbfixedrolepermission (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbfixedrolepermission
- sp_dbfixedrolepermission_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbfixedrolepermission
ms.assetid: b8c30191-f532-49cd-83f3-c271f63ce572
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 78f1381d05c5841ce48e395f8cc1cd477b820e01
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201282"
---
# <a name="sp_dbfixedrolepermission-transact-sql"></a>sp_dbfixedrolepermission (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Zeigt die Berechtigungen einer festen Datenbankrolle an. **sp_dbfixedrolepermission** gibt korrekte Informationen in zurück [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] . Die Ausgabe spiegelt nicht die Änderungen an der Berechtigungs Hierarchie wider, die in implementiert wurden [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Weitere Informationen finden Sie unter [Rollen auf Datenbankebene](../../relational-databases/security/authentication-access/database-level-roles.md#fixed-database-roles), die eine Liste fester Daten bankrollen und zugehöriger Berechtigungen anzeigen.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_dbfixedrolepermission [ [ @rolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @rolename = ] 'role'` Der Name einer gültigen Fixed- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Daten Bank Rolle. *role* ist vom Datentyp **sysname** und hat den Standardwert NULL. Wenn *Role* nicht angegeben wird, werden die Berechtigungen für alle festgelegten Daten bankrollen angezeigt.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**DbFixedRole**|**sysname**|Name der festen Datenbankrolle|  
|**Berechtigung**|**nvarchar (70)**|Mit **DbFixedRole** verknüpfte Berechtigungen|  
  
## <a name="remarks"></a>Bemerkungen  
 Führen Sie **sp_helpdbfixedrole** aus, um eine Liste der fixierten Daten bankrollen anzuzeigen. In der folgenden Tabelle werden die festen Datenbankrollen angezeigt.  
  
|Fixed-Daten Bank Rolle|BESCHREIBUNG|  
|-------------------------|-----------------|  
|**db_owner**|Datenbankbesitzer|  
|**db_accessadmin**|Administratoren für den Datenbankzugriff|  
|**db_securityadmin**|Administratoren für die Datenbanksicherheit|  
|**db_ddladmin**|DDL-Administratoren (Data Definition Language, Datendefinitionssprache) für die Datenbank|  
|**db_backupoperator**|Datenbanksicherungs-Operatoren|  
|**db_datareader**|Datenbank-Datenleser|  
|**db_datawriter**|Datenbank-Datenschreiber|  
|**db_denydatareader**|Datenbank-Verweigerungsdatenleser|  
|**db_denydatawriter**|Datenbank-Verweigerungsdatenschreiber|  
  
 Mitglieder der Daten Bank Rolle **db_owner** verfügen über die Berechtigungen aller anderen Daten bankrollen. Führen Sie **sp_srvrolepermission** aus, um die Berechtigungen für Server Rollen anzuzeigen.  
  
 Das Resultset enthält die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen, die ausgeführt werden können, sowie andere spezielle Aktivitäten, die von Mitgliedern der Datenbankrolle ausgeführt werden können.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **public** -Rolle.  
  
## <a name="examples"></a>Beispiele  
 Die folgende Abfrage gibt die Berechtigungen für alle festen Datenbankrollen zurück, weil keine feste Datenbankrolle angegeben ist.  
  
```  
EXEC sp_dbfixedrolepermission;  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Security Stored Procedures &#40;Transact-SQL&#41; (Gespeicherte Sicherheitsprozeduren (Transact-SQL))](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_droprolemember &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helpdbfixedrole &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-helpdbfixedrole-transact-sql.md)   
 [sp_srvrolepermission &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-srvrolepermission-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
