---
description: sys.openkeys (Transact-SQL)
title: sys. openkeys (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- openkeys_TSQL
- sys.openkeys_TSQL
- openkeys
- sys.openkeys
dev_langs:
- TSQL
helpviewer_keywords:
- sys.openkeys catalog view
ms.assetid: 719a1259-2398-4fcb-ba05-aeabba7aec21
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7da45ef149fdd32cb16602bbd2aa3151e53ca4b2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97404986"
---
# <a name="sysopenkeys-transact-sql"></a>sys.openkeys (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Diese Katalogsicht gibt Informationen zu den Verschlüsselungsschlüsseln zurück, die in der aktuellen Sitzung geöffnet sind.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Die ID der Datenbank, die den Schlüssel enthält.|  
|**database_name**|**sysname**|Der Name der Datenbank, die den Schlüssel enthält.|  
|**key_id**|**int**|ID des Schlüssels. Die ID ist innerhalb der Datenbank eindeutig.|  
|**key_name**|**sysname**|Name des Schlüssels. Ist innerhalb der Datenbank eindeutig.|  
|**key_guid**|**varbinary**|Die GUID des Schlüssels. Ist innerhalb der Datenbank eindeutig.|  
|**opened_date**|**datetime**|Datum und Uhrzeit, zu der der Schlüssel geöffnet wurde.|  
|**status**|**int**|1, wenn der Schlüssel in den Metadaten gültig ist. 0, wenn der Schlüssel nicht in den Metadaten vorhanden ist.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-symmetric-key-transact-sql.md)  
  
  
