---
description: sp_resetsnapshotdeliveryprogress (Transact-SQL)
title: sp_resetsnapshotdeliveryprogress (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_resetsnapshotdeliveryprogress
- sp_resetsnapshotdeliveryprogress_TSQL
helpviewer_keywords:
- sp_resetsnapshotdeliveryprogress
ms.assetid: 5df7d86b-d343-4d9b-88b1-74429ed092e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b4bcee8dfc47a489fc605bcd3bd787a1ed393329
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89543080"
---
# <a name="sp_resetsnapshotdeliveryprogress-transact-sql"></a>sp_resetsnapshotdeliveryprogress (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Setzt den Momentaufnahme-Übermittlungsprozess für ein Pullabonnement zurück, damit die Übermittlung der Momentaufnahme neu gestartet werden kann. Diese gespeicherte Prozedur wird auf dem Abonnenten für die Abonnementdatenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_resetsnapshotdeliveryprogress [ [ @verbose_level = ] verbose_level ]  
    [ , [ @drop_table = ] 'drop_table' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @verbose_level = ] verbose_level` Gibt die Menge der zurückgegebenen Informationen an. *verbose_level*ist vom Datentyp **int**und hat den Standardwert **1**. Der Wert **1** bedeutet, dass ein Fehler zurückgegeben wird, wenn die erforderlichen Sperren für die **MSsnapshotdeliveryprogress** -Tabelle nicht abgerufen werden können, und **0** bedeutet, dass kein Fehler zurückgegeben wird.  
  
`[ @drop_table = ] 'drop_table'` Gibt an, ob die Tabelle mit Informationen zum Status der Momentaufnahme gelöscht oder abgeschnitten werden soll. *DROP_TABLE* ist vom Datentyp **nvarchar (5)** und hat den Standardwert **false**. Bei false wird die Tabelle abgeschnitten, und bei true wird die Tabelle gelöscht.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 **sp_resetsnapshotdeliveryprogress** entfernt alle Zeilen in der **MSsnapshotdeliveryprogress** -Tabelle. Auf diese Weise werden alle Metadaten entfernt, die in der Abonnementdatenbank durch vorherige Momentaufnahme-Übermittlungsprozesse zurückgeblieben sind.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_resetsnapshotdeliveryprogress**ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gespeicherte Automatisierungsprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
