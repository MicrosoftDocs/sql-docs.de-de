---
description: sp_help_peerconflictdetection (Transact-SQL)
title: sp_help_peerconflictdetection (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_help_peerconflictdetection
- sp_help_peerconflictdetection_TSQL
helpviewer_keywords:
- sp_help_peerconflictdetection
ms.assetid: 59e04107-5eaa-44a1-beb6-ac4f2dbbcb28
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 960d2a58e01fcb2352fe68d60564534d43d4faf3
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89527697"
---
# <a name="sp_help_peerconflictdetection-transact-sql"></a>sp_help_peerconflictdetection (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt Informationen über die Konflikterkennungseinstellungen für eine Veröffentlichung zurück, die Teil einer Peer-zu-Peer-Transaktionsreplikationstopologie ist.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_help_peerconflictdetection [ @publication = ] 'publication'  
    [ ,[ @timeout = ] timeout ]  
```  
  
## <a name="arguments"></a>Argumente  
 [ @publication =] '*Veröffentlichung*'  
 Der Name der Veröffentlichung, für die Informationen zurückgegeben werden sollen. *Publication* ist vom **Datentyp vom Datentyp sysname**und hat keinen Standardwert.  
  
 [ @timeout =] *Timeout*  
 Gibt den Zeitraum in Sekunden an, nachdem für die Prozedur ein Timeout eintritt, während auf die Antwort von jedem Knoten in der Topologie gewartet wird. Wenn die Topologie einen schreibgeschützten Abonnenten enthält, ist die Angabe eines Timeoutwerts nicht gültig. Schreibgeschützte Abonnenten reagieren nie auf einen Aufruf von dieser Prozedur. *Timeout* ist vom Datentyp **int**. der Standardwert ist 60.  
  
## <a name="result-sets"></a>Resultsets  
 sp_help_peerconflictdetection gibt drei Resultsets zurück. Diese Ergebnisse sind in den folgenden Themen dokumentiert:  
  
-   [MSpeer_conflictdetectionconfigrequest](../../relational-databases/system-tables/mspeer-conflictdetectionconfigrequest-transact-sql.md)  
  
-   [MSpeer_conflictdetectionconfigresponse](../../relational-databases/system-tables/mspeer-conflictdetectionconfigresponse-transact-sql.md)  
  
-   [Mspeer_originatorid_history](../../relational-databases/system-tables/mspeer-originatorid-history-transact-sql.md)  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 sp_help_peerconflictdetection wird in der Peer-zu-Peer-Transaktionsreplikation verwendet.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Serverrolle sysadmin oder in der festen Datenbankrolle db_owner.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Konflikterkennung bei der Peer-zu-Peer-Replikation](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)   
 [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)   
 [Gespeicherte Automatisierungsprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
