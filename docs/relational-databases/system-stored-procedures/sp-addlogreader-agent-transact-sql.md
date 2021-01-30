---
description: sp_addlogreader_agent (Transact-SQL)
title: sp_addlogreader_agent (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addlogreader_agent
- sp_addlogreader_agent_TSQL
helpviewer_keywords:
- sp_addlogreader_agent
ms.assetid: d83096b9-96ee-4789-bde0-940d4765b9ed
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: a511c4f47f84a8f083dbab86bf1d851762142d90
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202371"
---
# <a name="sp_addlogreader_agent-transact-sql"></a>sp_addlogreader_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Fügt einen Protokolllese-Agent für eine bestimmte Datenbank hinzu. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.  
  
> [!IMPORTANT]  
>  Beim Konfigurieren eines Verlegers mit einem Remoteverteiler werden die Werte, die für alle Parameter, einschließlich *job_login* und *job_password*, bereitgestellt werden, als Nur-Text an den Verteiler gesendet. Sie sollten die Verbindung zwischen dem Verleger und dem zugehörigen Remoteverteiler verschlüsseln, bevor Sie diese gespeicherte Prozedur ausführen. Weitere Informationen finden Sie unter [Aktivieren von verschlüsselten Verbindungen zur Datenbank-Engine &#40;SQL Server-Konfigurations-Manager&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_addlogreader_agent [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
    [ , [ @job_name = ] 'job_name' ]  
    [ , [ @publisher_security_mode = ] publisher_security_mode ]  
    [ , [ @publisher_login = ] 'publisher_login' ]  
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @job_login = ] 'job_login'` Der Anmelde Name für das [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Konto, unter dem der Agent ausgeführt wird. *job_login* ist vom Datentyp **nvarchar (257)** und hat den Standardwert NULL. Das Windows-Konto wird stets für Agent-Verbindungen mit dem Verteiler verwendet.  
  
> [!NOTE]
>  Bei nicht-- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verlegern muss es sich hierbei um denselben Anmelde Namen handeln, der in [sp_adddistpublisher &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)angegeben ist.  
  
`[ @job_password = ] 'job_password'` Das Kennwort für das Windows-Konto, unter dem der Agent ausgeführt wird. *job_password* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL.  
  
> [!IMPORTANT]  
>  Speichern Sie keine Authentifizierungsinformationen in Skriptdateien. Für die optimale Sicherheit sollten Anmeldenamen und Kennwörter zur Laufzeit bereitgestellt werden.  
  
`[ @job_name = ] 'job_name'` Der Name eines vorhandenen Agentauftrags. *job_name* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Dieser Parameter wird nur dann angegeben, wenn der Agent mit einem vorhandenen Auftrag anstatt mit einem neu erstellten Auftrag (Standard) gestartet wird.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Der Sicherheitsmodus, der vom Agent beim Herstellen einer Verbindung mit dem Verleger verwendet wird. *publisher_security_mode* ist vom Datentyp **smallint**. der Standardwert ist **1**. **0** gibt die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentifizierung an, und **1** gibt die Windows-Authentifizierung an. Für nicht--Verleger muss ein Wert von **0** angegeben werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @publisher_login = ] 'publisher_login'` Der Anmelde Name, der beim Herstellen einer Verbindung mit dem Verleger verwendet wird. *publisher_login* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. *publisher_login* muss angegeben werden, wenn *publisher_security_mode* den Wert **0** hat. Wenn *publisher_login* NULL und *publisher_security_mode* 1 ist, wird das in *job_login* angegebene Windows-Konto verwendet, wenn **eine** Verbindung mit dem Verleger hergestellt wird.  
  
`[ @publisher_password = ] 'publisher_password'` Das Kennwort, das beim Herstellen einer Verbindung mit dem Verleger verwendet wird. *publisher_password* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL.  
  
> [!IMPORTANT]  
>  Speichern Sie keine Authentifizierungsinformationen in Skriptdateien. Für die optimale Sicherheit sollten Anmeldenamen und Kennwörter zur Laufzeit bereitgestellt werden.  
  
`[ @publisher = ] 'publisher'` Der Name des nicht-- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verlegers. *Publisher* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL.  
  
> [!NOTE]  
>  Sie sollten diesen Parameter nicht für einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verleger festlegen.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_addlogreader_agent** wird bei der Transaktions Replikation verwendet.  
  
 Sie müssen **sp_addlogreader_agent** ausführen, um einen Protokoll Lese-Agent hinzuzufügen, wenn Sie eine Datenbank, die für die Replikation aktiviert wurde, auf diese Version von aktualisiert haben, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bevor eine Veröffentlichung erstellt wurde, die die Datenbank verwendet.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_addlogreader_agent** ausführen.  
  
## <a name="example"></a>Beispiel  
 [!code-sql[HowTo#sp_AddTranPub](../../relational-databases/replication/codesnippet/tsql/sp-addlogreader-agent-tr_1.sql)]  
  
## <a name="see-also"></a>Weitere Informationen  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [sp_addpublication &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changelogreader_agent &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-changelogreader-agent-transact-sql.md)   
 [Gespeicherte Automatisierungsprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
