---
description: sp_dropmergealternatepublisher (Transact-SQL)
title: sp_dropmergealternatepublisher (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropmergealternatepublisher
- sp_dropmergealternatepublisher_TSQL
helpviewer_keywords:
- sp_dropmergealternatepublisher
ms.assetid: a7dee4e2-2a60-41da-9d1d-6f991d7e2c5e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5d787648f197c54623ba305503ddd878a3a7061d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208280"
---
# <a name="sp_dropmergealternatepublisher-transact-sql"></a>sp_dropmergealternatepublisher (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Löscht einen alternativen Verleger aus einer Mergeveröffentlichung. Diese gespeicherte Prozedur wird auf dem Abonnenten für die Abonnement Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_dropmergealaternatepublisher [ @publisher = ] 'publisher'    , [ @publisher_db = ] 'publisher_db'    , [ @publication = ] 'publication'    , [ @alternate_publisher = ] 'alternate_publisher'    , [ @alternate_publisher_db = ] 'alternate_publisher_db'    , [ @alternate_publication = ] 'alternate_publication'  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publisher = ] 'publisher'` Der Name des aktuellen Verlegers. *Publisher* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @publisher_db = ] 'publisher_db'` Der Name der aktuellen Veröffentlichungs Datenbank. *publisher_db* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @publication = ] 'publication'` Der Name der aktuellen Veröffentlichung. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @alternate_publisher = ] 'alternate_publisher'` Der Name des alternativen Verlegers, der als alternativer Synchronisierungs Partner gelöscht werden soll. *alternate_publisher* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @alternate_publisher_db = ] 'alternate_publisher_db'` Der Name der Veröffentlichungs Datenbank, die als Veröffentlichungs Datenbank für den alternativen Synchronisierungs Partner gelöscht werden soll. *alternate_publisher_db* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @alternate_publication = ] 'alternate_publication'` Der Name der Veröffentlichung, die als alternative Synchronisierungs Partner Veröffentlichung gelöscht werden soll. *alternate_publication* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_dropmergealternatepublisher** wird bei der Mergereplikation verwendet.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_dropmergelternatepublisher** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_addmergealternatepublisher &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addmergealternatepublisher-transact-sql.md)  
  
  
