---
description: sp_audit_write (Transact-SQL)
title: sp_audit_write (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_audit_write
- sp_audit_write_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_audit_write
ms.assetid: 4c523848-1ce6-49ad-92b3-e0e90f24f1c2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0ac793b35f5e72b93a27369bf00327a969059b0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171762"
---
# <a name="sp_audit_write-transact-sql"></a>sp_audit_write (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fügt der **USER_DEFINED_AUDIT_GROUP** ein benutzerdefiniertes Überwachungsereignis hinzu. Wenn **USER_DEFINED_AUDIT_GROUP** nicht aktiviert ist, wird **sp_audit_write** ignoriert.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
sp_audit_write [ @user_defined_event_id = ] user_defined_event_id
    [ , [ @succeeded = ] succeeded ]
    [ , [ @user_defined_information = ] 'user_defined_information' ]
    [ ; ]
```  
  
## <a name="arguments"></a>Argumente  
 `[ @user_defined_event_id = ] user_defined_event_id`  
 Ein vom Benutzer definierter und in der **user_defined_event_id** -Spalte des Überwachungsprotokolls aufgezeichneter Parameter. *\@ user_defined_event_id* ist vom Typ **smallint**.  
  
 `[ @succeeded = ] succeeded`  
 Ein vom Benutzer übergebener Parameter, mit dem angegeben wird, ob das Ereignis erfolgreich war. Dies wird in der Spalte Erfolgreich des Überwachungsprotokolls angezeigt. `@succeeded` ist **Bit**.  
  
 `[ @user_defined_information = ] 'user_defined_information'`  
 Der vom Benutzer definierte und in der Spalte user_defined_event_id des Überwachungsprotokolls aufgezeichnete Text. `@user_defined_information` ist vom Datentyp **nvarchar (4000)**.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
 Fehler entstehen durch falsche Eingabeparameter oder Fehler beim Schreiben in das Zielüberwachungsprotokoll.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn **USER_DEFINED_AUDIT_GROUP** entweder einer Serverüberwachungs- oder Datenbanküberwachungsspezifikation hinzugefügt wird, wird das von **sp_audit_write** ausgelöste Ereignis in das Überwachungsprotokoll eingeschlossen.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **Public** -Daten Bank Rolle.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-creating-a-user-defined-audit-event-with-informational-text"></a>A. Erstellen eines benutzerdefinierten Überwachungsereignisses mit Informationstext  
 Im folgenden Beispiel wird ein Überwachungsereignis mit der ID 27, dem Erfolgswert 0 und optionalem Informationstext erstellt.  
  
```  
EXEC sp_audit_write @user_defined_event_id =  27 ,   
              @succeeded =  0   
            , @user_defined_information = N'Access to a monitored object.' ;  
```  
  
### <a name="b--creating-a-user-defined-audit-event-without-informational-text"></a>B.  Erstellen eines benutzerdefinierten Überwachungsereignisses ohne Informationstext  
 Im folgenden Beispiel wird ein Überwachungsereignis mit der ID 27 und dem Erfolgswert 0 erstellt. Optionaler Informationstext oder die Namen der optionalen Parameter werden nicht einbezogen.  
  
```  
EXEC sp_audit_write 27, 0;  
  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Security Stored Procedures &#40;Transact-SQL&#41; (Gespeicherte Sicherheitsprozeduren (Transact-SQL))](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sp_addrole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
