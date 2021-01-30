---
description: managed_backup managed_backup.fn_backup_instance_config (Transact-SQL)
title: managed_backup managed_backup.fn_backup_instance_config (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_backup_instance_config
- smart_admin.fn_backup_instance_config_TSQL
- fn_backup_instance_config_TSQL
- smart_admin.fn_backup_instance_config
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_backup_instance_config
- fn_backup_instance_config
ms.assetid: 2382a547-c0c9-4e1d-87c9-d8526192eb5a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 534b1417536b233cafd0c7f3499e6ff801f0fa42
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207413"
---
# <a name="managed_backupfn_backup_instance_config-transact-sql"></a>managed_backup managed_backup.fn_backup_instance_config (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Gibt 1 Zeile mit den [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]-Standardkonfigurationseinstellungen für die SQL Server-Instanz zurück.  
  
 Verwenden Sie diese gespeicherte Prozedur, um die aktuellen [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]-Standardkonfigurationseinstellungen für die Instanz von SQL Server zu überprüfen oder zu bestimmen.  
  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```sql  
managed_backup.fn_backup_db_config ()  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a>Argumente  
 Keine  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|is_smart_backup_enabled|INT|Zeigt 1 an, wenn [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] aktiviert ist, und 0, wenn [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] deaktiviert ist.|  
|credential_name|SYSNAME|Verwendete Standard SQL-Anmeldeinformationen für die Authentifizierung beim Speicher.|  
|retention_days|INT|Die auf Instanzebene festgelegte Standardbeibehaltungsdauer.|  
|storage_url|Nvarchar (1024)|Die standardmäßige, auf Instanzebene festgelegte Speicherkonto-URL.|  
|encryption_algorithm|SYSNAME|Der Name des Verschlüsselungsalgorithmus. Wird auf NULL festgelegt, wenn keine Verschlüsselung angegeben ist.|  
|encryptor_type|Nvarchar (32)|Verwendeter Verschlüsselungstyp: Zertifikat oder asymmetrischer Schlüssel. Wird auf NULL festgelegt, wenn keine Verschlüsselung angegeben ist.|  
|encryptor_name|SYSNAME|Der Name des Zertifikats oder des asymmetrischen Schlüssels. Wird auf NULL festgelegt, wenn kein Name angegeben ist.|  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **db_backupoperator** -Daten Bank Rolle mit **ALTER ANY CREDENTIAL** -Berechtigungen. Dem Benutzer sollte die **View any Definition** -Berechtigung nicht verweigert werden.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel gibt die [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]-Standardkonfigurationseinstellungen für die Instanz zurück, auf der es ausgeführt wird:  
  
```  
Use msdb;  
GO  
SELECT * FROM managed_backup.fn_backup_instance_config ();  
  
```  
  
  
