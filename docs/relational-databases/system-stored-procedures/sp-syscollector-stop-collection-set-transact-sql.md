---
description: sp_syscollector_stop_collection_set (Transact-SQL)
title: sp_syscollector_stop_collection_set (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_stop_collection_set_TSQL
- sp_syscollector_stop_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_stop_collection_set
- data collector [SQL Server], stored procedures
ms.assetid: 4668cfb7-462f-40d0-948c-8f740a792a4d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 335e4966896ef6e08cdec78612caa2a53b3641b7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159487"
---
# <a name="sp_syscollector_stop_collection_set-transact-sql"></a>sp_syscollector_stop_collection_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Beendet einen Sammlungs Satz.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_syscollector_stop_collection_set   
    [ [ @collection_set_id = ] collection_set_id ]  
    , [ [ @name = ] 'name' ]  
    , [ [ @stop_collection_job = ] stop_collection_job ]  
```  
  
## <a name="arguments"></a>Argumente  
 [ @collection_set_id =] *collection_set_id*  
 Der eindeutige lokale Bezeichner für den Sammlungssatz. *collection_set_id* ist vom Datentyp **int** und hat den Standardwert NULL. *collection_set_id* muss einen Wert haben, wenn der *Name* NULL ist.  
  
 [ @name =] '*Name*'  
 Der Name des Sammlungs Satzes. *Name ist vom Datentyp* **vom Datentyp sysname** und hat den Standardwert NULL. der *Name* muss einen Wert haben, wenn *collection_set_id* NULL ist.  
  
 [ @stop_collection_job =] *stop_collection_job*  
 Gibt an, dass der Sammlungsauftrag für den Sammlungssatz beendet wird, falls er ausgeführt wird. *stop_collection_job* ist vom Typ **Bit** und hat den Standardwert 1.  
  
 *stop_collection_job* gilt nur für Sammlungs Sätze, für die der Sammlungs Modus auf "zwischengespeichert" festgelegt Weitere Informationen finden Sie unter [sp_syscollector_create_collection_set &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md).  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 sp_syscollector_create_collection_set muss im Kontext der msdb-Systemdatenbank ausgeführt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Damit diese Prozedur ausgeführt werden kann, ist die Mitgliedschaft in der festen Datenbankrolle dc_operator (mit EXECUTE-Berechtigung) erforderlich.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird ein Sammlungssatz mithilfe seines Bezeichners beendet.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_stop_collection_set @collection_set_id = 1;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datensammlung](../../relational-databases/data-collection/data-collection.md)   
 [Gespeicherte Prozeduren für den Datensammler &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)  
  
  
