---
description: sys.server_sql_modules (Transact-SQL)
title: sys. server_sql_modules (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.server_sql_modules
- sys.server_sql_modules_TSQL
- server_sql_modules_TSQL
- server_sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_sql_modules catalog view
ms.assetid: 9ef9a8b9-c470-4a61-b0c4-ee24ad871d63
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7f74b41384c2aa42c26623fa69ef35c74f49eab3
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89539579"
---
# <a name="sysserver_sql_modules-transact-sql"></a>sys.server_sql_modules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält den SQL-Modulsatz für Trigger vom Typ TR auf Serverebene. Sie können diese Beziehung mit sys.server_triggers verknüpfen. Das Tupel (object_id) ist der Schlüssel der Beziehung.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Ein FOREIGN KEY-Verweis zurück auf den Trigger auf Serverebene, wo dieses Modul definiert ist.|  
|**Definition**|**nvarchar(max)**|Der SQL-Text, der dieses Modul definiert.<br /><br /> NULL = Verschlüsselt.|  
|**uses_ansi_nulls**|**bit**|Beim Erstellen des Moduls war die Option SET ANSI NULLS auf ON festgelegt.|  
|**uses_quoted_identifier**|**bit**|Beim Erstellen des Moduls war die Option SET QUOTED IDENTIFIER auf ON festgelegt.|  
|**execute_as_principal_id**|**int**|ID des Serverprinzipals, der mit EXECUTE AS verwendet wird.<br /><br /> NULL als Standard oder bei EXECUTE AS CALLER<br /><br /> ID des angegebenen Prinzipals, wenn EXECUTE AS Self EXECUTE AS Principal-2 = EXECUTE AS Owner.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
