---
description: sp_databases (Transact-SQL)
title: sp_databases (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_databases_TSQL
- sp_databases
dev_langs:
- TSQL
helpviewer_keywords:
- sp_databases
ms.assetid: 2a83b92a-9ecc-43c4-8ff4-e91e3a940b5a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 90fbcbf710f1779ad011163641aa61b14b33c65b
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89549893"
---
# <a name="sp_databases-transact-sql"></a>sp_databases (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Listet Datenbanken auf, die in einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] enthalten sind bzw. auf die der Zugriff über ein Datenbank-Gateway möglich ist.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_databases  
```  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 Keine  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**DATABASE_NAME**|**sysname**|Der Name der Datenbank. Im [!INCLUDE[ssDE](../../includes/ssde-md.md)]stellt diese Spalte den Datenbanknamen dar, der in der Katalogsicht **sys.databases** gespeichert ist.|  
|**DATABASE_SIZE**|**int**|Die Größe der Datenbank in Kilobyte.|  
|**HINWEISE**|**varchar (254)**|Im [!INCLUDE[ssDE](../../includes/ssde-md.md)]gibt dieses Feld immer NULL zurück.|  
  
## <a name="remarks"></a>Hinweise  
 Die zurückgegebenen Datenbanknamen können als Parameter für die USE-Anweisung verwendet werden, um den aktuellen Datenbankkontext zu ändern.  
  
 Für**sp_databases** gibt es in Open Database Connectivity (ODBC) keine Entsprechung.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Berechtigung CREATE DATABASE oder ALTER ANY DATABASE oder VIEW ANY DEFINITION sowie die Zugriffsberechtigung für die Datenbank. Die VIEW ANY DEFINITION-Berechtigung darf nicht verweigert worden sein.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel zeigt die Ausführung von `sp_databases`.  
  
```sql  
USE master;  
GO  
EXEC sp_databases;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [HAS_DBACCESS &#40;Transact-SQL-&#41;](../../t-sql/functions/has-dbaccess-transact-sql.md)  
  
  
