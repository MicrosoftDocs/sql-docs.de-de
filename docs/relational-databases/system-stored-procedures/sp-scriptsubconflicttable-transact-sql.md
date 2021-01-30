---
description: sp_scriptsubconflicttable (Transact-SQL)
title: sp_scriptsubconflicttable (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_scriptsubconflicttable
- sp_scriptsubconflicttable_TSQL
helpviewer_keywords:
- sp_scriptsubconflicttable
ms.assetid: 13867145-3dad-47a4-8d50-a65175418479
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4ed5d504e669e1bdf5fb6677880e87271739c1cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189953"
---
# <a name="sp_scriptsubconflicttable-transact-sql"></a>sp_scriptsubconflicttable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Generiert ein Skript zum Erstellen einer Konflikttabelle auf dem Abonnenten für einen gegebenen Artikel in einem Abonnement mit Warteschlange. Das generierte Skript wird beim Abonnenten mit der Abonnementdatenbank ausgeführt. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_scriptsubconflicttable [@publication =] 'publication'    , [@article =] 'article'  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publication = ] 'publication'` Der Name der Veröffentlichung, die den Artikel enthält. Der Name muss in der Datenbank eindeutig sein. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @article = ] 'article'` Der Name des Abonnement Artikels. der *Artikel* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**cmdtext**|**nvarchar(4000)**|Gibt das [!INCLUDE[tsql](../../includes/tsql-md.md)]-Skript für die Erstellung der Konflikttabelle beim Abonnementen für den Artikel in dem Abonnement mit Warteschlange zurück. Dieses Skript wird beim Abonnementen in der Abonnementendatenbank ausgeführt.|  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_scriptsubconflicttable** wird für Abonnenten verwendet, die über Abonnements verfügen, bei denen die Anfangs Momentaufnahme manuell angewendet wird. Bei der Konflikttabelle handelt es sich um eine optionale Tabelle beim Abonnementen.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_scriptsubconflicttable** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erkennen und Auflösen von Konflikten beim Aktualisieren von verzögertem Update](../../relational-databases/replication/transactional/updatable-subscriptions-queued-updating-conflict-resolution.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
