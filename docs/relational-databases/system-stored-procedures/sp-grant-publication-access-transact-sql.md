---
description: sp_grant_publication_access (Transact-SQL)
title: sp_grant_publication_access (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_grant_publication_access_TSQL
- sp_grant_publication_access
helpviewer_keywords:
- sp_grant_publication_access
ms.assetid: 17993952-def6-4a16-b1c1-323ec42967f8
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: 1bea8096d3d0d57b6044edc26bb5f64572487157
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208995"
---
# <a name="sp_grant_publication_access-transact-sql"></a>sp_grant_publication_access (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fügt der Zugriffsliste der Veröffentlichung einen Anmeldenamen hinzu. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
sp_grant_publication_access [ @publication = ] 'publication', [ @login = ] 'login'   
    [ , [ @reserved = ] 'reserved' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publication = ] 'publication'` Der Name der Veröffentlichung, auf die zugegriffen werden soll. **'**_Publication_*_'_* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @login = ] 'login'` Die Anmelde-ID. **"**_Login_*_"_* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_grant_publication_access** wird bei der Momentaufnahme-, Transaktions-und Mergereplikation verwendet.  
  
 Diese gespeicherte Prozedur kann wiederholt aufgerufen werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_grant_publication_access** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_help_publication_access &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-help-publication-access-transact-sql.md)   
 [sp_revoke_publication_access &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-revoke-publication-access-transact-sql.md)   
 [Sichern des Verlegers](../../relational-databases/replication/security/secure-the-publisher.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
