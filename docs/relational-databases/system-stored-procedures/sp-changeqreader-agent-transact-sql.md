---
description: sp_changeqreader_agent (Transact-SQL)
title: sp_changeqreader_agent (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_changeqreader_agent_TSQL
- sp_changeqreader_agent
helpviewer_keywords:
- sp_changeqreader_agent
ms.assetid: d3fe79c5-31ef-4565-bf38-b476b5fb16f7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 08e7e0571cab57d50da670495af95709482899a3
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89543681"
---
# <a name="sp_changeqreader_agent-transact-sql"></a>sp_changeqreader_agent (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ändert die Sicherheitseigenschaften eines Warteschlangenlese-Agents. Diese gespeicherte Prozedur wird auf dem Verteiler für die Verteilungsdatenbank oder auf dem Verleger für die Veröffentlichungsdatenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_changeqreader_agent [ [ @job_login = ] 'job_login' ]  
    [ , [ @job_password = ] 'job_password' ]  
    [ , [ @frompublisher = ] frompublisher   
```  
  
## <a name="arguments"></a>Argumente  
`[ @job_login = ] 'job_login'` Der Anmelde Name für das [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Konto, unter dem der Agent ausgeführt wird. *job_login* ist vom Datentyp **nvarchar (257)** und hat den Standardwert NULL.  
  
`[ @job_password = ] 'job_password'` Das Kennwort für das Windows-Konto, unter dem der Agent ausgeführt wird. *job_password* ist vom **Datentyp vom Datentyp sysname**und hat den Standardwert NULL.  
  
`[ @frompublisher = ] frompublisher` Gibt an, ob die Prozedur auf dem Verleger ausgeführt wird. *frompublisher* ist vom Typ Bit und hat den Standardwert **0**. Der Wert **1** bedeutet, dass die Prozedur auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt wird.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 **sp_changeqreader_agent** wird bei der Transaktions Replikation verwendet.  
  
 **sp_changeqreader_agent** wird verwendet, um das Windows-Konto zu ändern, unter dem ein Warteschlangen Lese-Agent ausgeführt wird. Sie können das Kennwort für einen vorhandenen Windows-Anmeldenamen ändern oder einen neuen Windows-Anmeldenamen und ein neues Kennwort angeben.  
  
 Nach dem Ändern des Anmeldenamens oder Kennworts eines Agents müssen Sie den Agent beenden und neu starten, damit die Änderungen in Kraft treten.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** können **sp_changeqreader_agent**ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Anzeigen und Ändern von Replikationssicherheitseinstellungen](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [sp_addqreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md)  
  
  
