---
description: sp_dropdistpublisher (Transact-SQL)
title: sp_dropdistpublisher (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropdistpublisher
- sp_dropdistpublisher_TSQL
helpviewer_keywords:
- sp_dropdistpublisher
ms.assetid: c0bdd3de-3be0-455c-898a-98d4660e7ce3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 09db4d47afee6795b403542c8442cc9d74724f4b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200696"
---
# <a name="sp_dropdistpublisher-transact-sql"></a>sp_dropdistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Löscht einen Verteilungsverleger. Diese gespeicherte Prozedur wird auf dem Verteiler für jede Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_dropdistpublisher [ @publisher = ] 'publisher'  
    [ , [ @no_checks = ] no_checks ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publisher = ] 'publisher'` Der Verleger, der gelöscht werden soll. *Publisher* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert.  
  
`[ @no_checks = ] no_checks` Gibt an, ob **sp_dropdistpublisher** überprüft, ob der Verleger den Server als Verteiler deinstalliert hat. *no_checks* ist vom Typ **Bit**. der Standardwert ist **0**.  
  
 Bei einem Wert von **0** überprüft die Replikation, ob der Remote Verleger den lokalen Server als Verteiler deinstalliert hat. Wenn es sich beim Verleger um einen lokalen Verleger handelt, überprüft die Replikation, ob sich auf dem lokalen Server keine Veröffentlichungs- oder Verteilungsobjekte mehr befinden.  
  
 Bei **1** werden alle dem Verteilungs Verleger zugeordneten Replikations Objekte gelöscht, selbst wenn ein Remote Verleger nicht erreicht werden kann. Danach muss der Remote Verleger die Replikation mithilfe [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md) mit **\@ ignore_distributor**  =  **1** deinstallieren.  
  
`[ @ignore_distributor = ] ignore_distributor` Gibt an, ob Verteilungs Objekte auf dem Verteiler verbleiben, wenn der Verleger entfernt wird. *ignore_distributor* ist vom **Bit** und kann einen der folgenden Werte aufweisen:  
  
 **1** = Verteilungs Objekte, die dem *Verleger* angehören, bleiben auf dem Verteiler.  
  
 **0** = Verteilungs Objekte für den *Verleger* werden auf dem Verteiler bereinigt.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_dropdistpublisher** wird bei allen Replikations Typen verwendet.  
  
 Wenn beim Löschen eines Oracle-Verlegers der Verleger nicht gelöscht werden kann **sp_dropdistpublisher** wird ein Fehler zurückgegeben, und die Verteiler Objekte für den Verleger werden entfernt.  
  
## <a name="example"></a>Beispiel  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/sp-dropdistpublisher-tra_1.sql)]  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** können **sp_dropdistpublisher** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Deaktivieren der Veröffentlichung und Verteilung](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [sp_adddistpublisher &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_changedistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_helpdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [Gespeicherte Automatisierungsprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
