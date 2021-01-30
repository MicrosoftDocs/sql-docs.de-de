---
description: sp_resyncmergesubscription (Transact-SQL)
title: sp_resyncmergesubscription (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_resyncmergesubscription_TSQL
- sp_resyncmergesubscription
helpviewer_keywords:
- sp_resyncmergesubscription
ms.assetid: e04d464a-60ab-4b39-a710-c066025708e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 530c93cbb99db63c7ced6e454ea78b4f6a0f1ebf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183079"
---
# <a name="sp_resyncmergesubscription-transact-sql"></a>sp_resyncmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Synchronisiert ein Mergeabonnement in einen bekannten Validierungsstatus neu, der von Ihnen angegeben wird. Dies ermöglicht es Ihnen, die Konvergenz zu erzwingen oder die Abonnementdatenbank mit der Version eines bestimmten Zeitpunkts zu synchronisieren. Dabei kann es sich z. B. um die letzte erfolgreiche Überprüfung oder um ein angegebenes Datum handeln. Die Momentaufnahme wird bei der erneuten Synchronisierung eines Abonnements mithilfe dieser Methode nicht erneut angewendet. Diese gespeicherte Prozedur wird nicht für Momentaufnahme- oder Transaktionsreplikationsabonnements verwendet. Diese gespeicherte Prozedur wird auf dem Verleger, in der Veröffentlichungs Datenbank oder auf dem Abonnenten für die Abonnement Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_resyncmergesubscription [ [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
        , [ @publication = ] 'publication'   
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @resync_type = ] resync_type ]  
    [ , [ @resync_date_str = ] resync_date_string ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publisher = ] 'publisher'` Der Name des Verlegers. *Publisher* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Der Wert NULL ist zulässig, wenn die gespeicherte Prozedur auf dem Verleger ausgeführt wird. Wenn die gespeicherte Prozedur auf dem Abonnenten ausgeführt wird, muss ein Verleger angegeben werden.  
  
`[ @publisher_db = ] 'publisher_db'` Der Name der Veröffentlichungs Datenbank. *publisher_db* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Der Wert NULL ist zulässig, wenn die gespeicherte Prozedur auf dem Verleger in der Veröffentlichungsdatenbank ausgeführt wird. Wenn die gespeicherte Prozedur auf dem Abonnenten ausgeführt wird, muss ein Verleger angegeben werden.  
  
`[ @publication = ] 'publication'` Der Name der Veröffentlichung. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @subscriber = ] 'subscriber'` Der Name des Abonnenten. *Subscriber* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Der Wert NULL ist zulässig, wenn die gespeicherte Prozedur auf dem Abonnenten ausgeführt wird. Wenn die gespeicherte Prozedur auf dem Verleger ausgeführt wird, muss ein Abonnent angegeben werden.  
  
`[ @subscriber_db = ] 'subscriber_db'` Der Name der Abonnement Datenbank. *subscription_db* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Der Wert NULL ist zulässig, wenn die gespeicherte Prozedur auf dem Abonnenten in der Abonnementdatenbank ausgeführt wird. Wenn die gespeicherte Prozedur auf dem Verleger ausgeführt wird, muss ein Abonnent angegeben werden.  
  
`[ @resync_type = ] resync_type` Definiert, wann die erneute Synchronisierung beginnen soll. *resync_type* ist vom Datentyp **int** und kann einen der folgenden Werte aufweisen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**0**|Die Synchronisierung beginnt nach der Anfangsmomentaufnahme. Dies ist die ressourcenintensivste Option, da alle Änderungen seit der Anfangsmomentaufnahme auf den Abonnenten erneut angewendet werden.|  
|**1**|Die Synchronisierung beginnt bei der letzten erfolgreichen Überprüfung. Alle neuen oder unvollständigen Generierungen, die seit der letzten erfolgreichen Überprüfung durchgeführt wurden, werden erneut auf den Abonnenten angewendet.|  
|**2**|Die Synchronisierung beginnt ab dem in *resync_date_str* angegebenen Datum. Alle neuen oder unvollständigen Generierungen, die seit diesem Datum durchgeführt wurden, werden erneut auf den Abonnenten angewendet.|  
  
`[ @resync_date_str = ] resync_date_string` Definiert das Datum, an dem die erneute Synchronisierung beginnen soll. *resync_date_string* ist vom Datentyp **nvarchar (30)** und hat den Standardwert NULL. Dieser Parameter wird verwendet, wenn das *resync_type* den Wert **2** hat. Das angegebene Datum wird in den entsprechenden **DateTime** -Wert konvertiert.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_resyncmergesubscription** wird bei der Mergereplikation verwendet.  
  
 Der Wert **0** für den *resync_type* -Parameter, der alle Änderungen seit der Anfangs Momentaufnahme erneut anwendet, kann ressourcenintensiv sein, aber möglicherweise sehr viel kleiner als eine vollständige erneute Initialisierung. Wenn z. B. die Anfangsmomentaufnahme vor einem Monat erstellt wurde, dann führt dieser Wert dazu, dass die Daten des letzten Monats erneut angewendet werden. Wenn die Anfangsmomentaufnahme 1 Gigabyte (GB) an Daten umfasst, der Umfang der Änderungen des letzten Monats jedoch nur 2 Megabyte (MB), dann ist es effizienter, die Daten und nicht die gesamte 1-GB-Momentaufnahme erneut anzuwenden.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_resyncmergesubscription** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
