---
description: sp_helpfile (Transact-SQL)
title: sp_helpfile (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpfile
- sp_helpfile_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpfile
ms.assetid: 1546e0ae-5a99-4e01-9eb9-d147fa65884c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 87bf389e2cd28fdf031308c116c32105d2f3c215
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204425"
---
# <a name="sp_helpfile-transact-sql"></a>sp_helpfile (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt die physischen Namen und Attribute der Dateien zurück, die der aktuellen Datenbank zugeordnet sind. Bestimmen Sie mithilfe dieser gespeicherten Prozedur die Namen von Dateien, die an den Server angefügt oder von diesem getrennt werden sollen.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_helpfile [ [ @filename= ] 'name' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @filename = ] 'name'` Der logische Name einer beliebigen Datei in der aktuellen Datenbank. *name* ist vom Datentyp **sysname** und hat den Standardwert NULL. Wenn *Name* nicht angegeben wird, werden die Attribute aller Dateien in der aktuellen Datenbank zurückgegeben.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Logischer Dateiname der Datei.|  
|**FileID**|**smallint**|Numerischer Bezeichner der Datei. Wird nicht zurückgegeben, wenn *Name* angegeben ist *.*|  
|**filename**|**NCHAR (260)**|Physischer Dateiname.|  
|**Datei Gruppe**|**sysname**|Dateigruppe, zu der die Datei gehört.<br /><br /> NULL = Die Datei ist eine Protokolldatei. Sie gehört nie zu einer Dateigruppe.|  
|**size**|**nvarchar (15)**|Die Dateigröße in KB.|  
|**MaxSize**|**nvarchar (15)**|Maximale Größe, auf die die Datei vergrößert werden kann. Mit UNLIMITED in diesem Feld kann die Datei so lange vergrößert werden, bis der Datenträger voll ist.|  
|**wachsen**|**nvarchar (15)**|Vergrößerungsinkrement der Datei. Zeigt die Menge an Speicherplatz an, die jedes Mal der Datei hinzugefügt wird, sobald neuer Speicherplatz erforderlich wird.<br /><br /> 0 = Die Datei weist eine feste Größe auf und wird nicht vergrößert.|  
|**ungs**|**varchar (9)**|Bei der Datendatei lautet der Wert **' nur Daten** ', und für die Protokolldatei lautet der Wert **' nur protokollieren '**.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **public** -Rolle.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel gibt Informationen zu den Dateien in [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] zurück.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_helpfile;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbank-Engine gespeicherter Prozeduren &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_helpfilegroup &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Datenbankdateien und Dateigruppen](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  
