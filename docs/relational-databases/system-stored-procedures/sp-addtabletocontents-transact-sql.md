---
description: sp_addtabletocontents (Transact-SQL)
title: sp_addtabletocontents (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addtabletocontents_TSQL
- sp_addtabletocontents
helpviewer_keywords:
- sp_addtabletocontents
ms.assetid: 2ea27001-74f4-463e-bf1b-b6b5a86b9219
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 469d9255813e1a072539e6aa8ca7035378ce2b92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198410"
---
# <a name="sp_addtabletocontents-transact-sql"></a>sp_addtabletocontents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fügt in die Mergenachverfolgungstabellen Verweise auf alle Zeilen in einer Quelltabelle ein, die derzeit nicht in den Nachverfolgungstabellen eingeschlossen sind. Verwenden Sie diese Option, wenn Sie eine große Datenmenge mithilfe von **bcp** Massen geladen haben, wodurch keine mergeüberwachungsertrigger ausgelöst werden. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_addtabletocontents [ @table_name = ] 'table_name'  
    [ , [ @owner_name = ] 'owner_name' ]  
    [ , [ @filter_clause = ] 'filter_clause' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @table_name = ] 'table_name'` Der Name der Tabelle. *table_name* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @owner_name = ] 'owner_name'` Der Name des Besitzers der Tabelle. *owner_name* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL.  
  
`[ @filter_clause = ] 'filter_clause'` Gibt eine Filter Klausel an, die steuert, welche Zeilen der neu geladenen Daten den Mergenachverfolgungstabellen hinzugefügt werden sollen. *filter_clause* ist vom Datentyp **nvarchar (4000)** und hat den Standardwert NULL. Wenn *filter_clause* **null** ist, werden alle Massen geladenen Zeilen hinzugefügt.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_addtabletocontents** wird nur bei der Mergereplikation verwendet.  
  
 Auf die Zeilen in der *table_name* wird von der **ROWGUIDCOL** verwiesen, und die Verweise werden den mergeverfolgungs-Tabellen hinzugefügt. **sp_addtabletocontents** sollte nach dem Massen Kopieren von Daten in eine Tabelle verwendet werden, die mithilfe der Mergereplikation veröffentlicht wird. Die gespeicherte Prozedur veranlasst die Nachverfolgung der kopierten Zeilen und gewährleistet, dass die neuen Zeilen bei der nächsten Synchronisation berücksichtigt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_addtabletocontents** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
