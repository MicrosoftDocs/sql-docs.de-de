---
description: sp_posttracertoken (Transact-SQL)
title: sp_posttracertoken (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- posttracerttoken
- posttracerttoken_TSQL
- sp_posttracertoken
- sp_posttracertoken_TSQL
helpviewer_keywords:
- sp_posttracertoken
ms.assetid: 24da5cd2-1c45-475e-93db-5bdf660f1c2c
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 74e1bcab6a1db0f8c92b82475689f24b53d72316
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88489140"
---
# <a name="sp_posttracertoken-transact-sql"></a>sp_posttracertoken (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Mit dieser Prozedur wird ein Überwachungstoken in das Transaktionsprotokoll am Verleger platziert, und der Prozess der Nachverfolgung von Statistiken über Latenzzeiten wird gestartet. Informationen werden erfasst, wenn das Überwachungstoken in das Transaktionsprotokoll geschrieben wird, wenn es vom Protokolllese-Agent aufgenommen wird und vom Verteilungs-Agent angewendet wird. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt. Weitere Informationen finden Sie unter [Messen der Wartezeit und Überprüfen der Verbindungen bei Transaktionsreplikationen](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_posttracertoken [ @publication = ] 'publication'   
    [ , [ @tracer_token_id = ] tracer_token_id OUTPUT  
    [ , [ @publisher = ] 'publisher'   
```  
  
## <a name="arguments"></a>Argumente  
`[ @publication = ] 'publication'` Der Name der Veröffentlichung, für die die Latenzzeit gemessen wird. *Publication* ist vom **Datentyp vom Datentyp sysname**und hat keinen Standardwert.  
  
`[ @tracer_token_id = ] _tracer_token_id OUTPUT` Die ID des eingefügten Überwachungs Tokens. *tracer_token_id* ist vom Datentyp **int** und hat den Standardwert NULL, und es handelt sich um einen Output-Parameter. Dieser Wert kann verwendet werden, um [sp_helptracertokenhistory &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-helptracertokenhistory-transact-sql.md) oder [sp_deletetracertokenhistory &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-deletetracertokenhistory-transact-sql.md) auszuführen, ohne zuerst [sp_helptracertokens &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md)auszuführen.  
  
`[ @publisher = ] 'publisher'` Gibt einen nicht-- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verleger an. *Publisher* ist vom **Datentyp vom Datentyp sysname**. der Standardwert ist NULL und sollte für einen Verleger nicht angegeben werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_posttracertoken** wird bei der Transaktions Replikation verwendet.  
  
## <a name="example"></a>Beispiel  
 [!code-sql[HowTo#sp_tracertokens](../../relational-databases/replication/codesnippet/tsql/sp-posttracertoken-trans_1.sql)]  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_posttracertoken**ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Messen der Latenzzeit und Überprüfen der Verbindungen bei Transaktionsreplikationen](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)  
  
  
