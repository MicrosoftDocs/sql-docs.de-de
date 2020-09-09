---
title: sp_dropdistributiondb (Transact-SQL) | Microsoft-Dokumentation
description: Löscht eine Verteilungs Datenbank und die von ihr verwendeten Dateien, wenn Sie nicht von einer anderen Datenbank verwendet werden. Diese gespeicherte Prozedur wird auf dem Verteiler für jede Datenbank ausgeführt.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_dropdistributiondb_TSQL
- sp_dropdistributiondb
helpviewer_keywords:
- sp_dropdistributiondb
ms.assetid: b6dd1846-2259-4d29-93af-a70a5d25a0c5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d5c1cb17767cf61b49345b93f7c5ee5ebfbc9d27
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89543477"
---
# <a name="sp_dropdistributiondb-transact-sql"></a>sp_dropdistributiondb (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Löscht eine Verteilungsdatenbank. Löscht die von der Datenbank verwendeten physischen Dateien, falls sie nicht von einer anderen Datenbank verwendet werden. Diese gespeicherte Prozedur wird auf dem Verteiler für jede Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_dropdistributiondb [ @database= ] 'database'  
```  
  
## <a name="arguments"></a>Argumente  
`[ @database = ] 'database'` Die Datenbank, die gelöscht werden soll. *Database* ist vom **Datentyp vom Datentyp sysname**und hat keinen Standardwert.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 **sp_dropdistributiondb** wird bei allen Replikations Typen verwendet.  
  
 Diese gespeicherte Prozedur muss ausgeführt werden, bevor der Verteiler gelöscht wird, indem **sp_dropdistributor**ausgeführt wird.  
  
 **sp_dropdistributiondb** entfernt auch einen Warteschlangenlese-Agent Auftrag für die Verteilungs Datenbank, sofern vorhanden.  
  
 Zum Deaktivieren der Verteilung muss die Verteilungsdatenbank online geschaltet sein. Wenn eine Datenbankmomentaufnahme für die Verteilungsdatenbank vorhanden ist, muss diese vor dem Deaktivieren der Verteilung gelöscht werden. Eine Datenbankmomentaufnahme ist eine schreibgeschützte Offlinekopie einer Datenbank und steht nicht in Verbindung mit einer Replikationsmomentaufnahme. Weitere Informationen finden Sie unter [Datenbankmomentaufnahmen &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md).  
  
## <a name="example"></a>Beispiel  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/sp-dropdistributiondb-tr_1.sql)]  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** können **sp_dropdistributiondb**ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Deaktivieren der Veröffentlichung und Verteilung](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [sp_adddistributiondb &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md)   
 [sp_changedistributiondb &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-changedistributiondb-transact-sql.md)   
 [sp_helpdistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md)   
 [Gespeicherte Automatisierungsprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
