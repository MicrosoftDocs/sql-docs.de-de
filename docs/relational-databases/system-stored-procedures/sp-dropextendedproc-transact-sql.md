---
description: sp_dropextendedproc (Transact-SQL)
title: sp_dropextendedproc (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropextendedproc
- sp_dropextendedproc_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropextendedproc
ms.assetid: dd93af2c-1b7d-4e39-af23-2d21d270a381
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bc877761f2e86ff63883c9f84ead94955044acbf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209333"
---
# <a name="sp_dropextendedproc-transact-sql"></a>sp_dropextendedproc (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Löscht eine erweiterte gespeicherte Prozedur.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen die [CLR-Integration](../../relational-databases/clr-integration/common-language-runtime-integration-overview.md) .  
  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
sp_dropextendedproc [ @functname = ] 'procedure'   
```  
  
## <a name="arguments"></a>Argumente  
`[ @functname = ] 'procedure'` Der Name der zu löschenden erweiterten gespeicherten Prozedur. die *Prozedur* ist vom Datentyp **nvarchar (517)** und hat keinen Standardwert.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="remarks"></a>Bemerkungen  
 Beim Ausführen von **sp_dropextendedproc** wird der benutzerdefinierte Name der erweiterten gespeicherten Prozedur aus der [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) -Katalog Sicht gelöscht und der Eintrag aus der [sys.extended_procedures](../../relational-databases/system-catalog-views/sys-extended-procedures-transact-sql.md) -Katalog Sicht entfernt. Diese gespeicherte Prozedur kann nur in der **Master** -Datenbank ausgeführt werden.  
  
**sp_dropextendedproc** löscht erweiterte gespeicherte System Prozeduren nicht. Stattdessen sollte der Systemadministrator die EXECUTE-Berechtigung für die erweiterte gespeicherte Prozedur der **Public** -Rolle verweigern.  
  
 **sp_dropextendedproc** kann nicht innerhalb einer Transaktion ausgeführt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** können **sp_dropextendedproc** ausführen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die erweiterte gespeicherte Prozedur `xp_hello` gelöscht.  
  
> [!NOTE]  
>  Diese erweiterte gespeicherte Prozedur muss bereits vorhanden sein; andernfalls gibt das Beispiel eine Fehlermeldung zurück.  
  
```  
USE master;  
GO  
EXEC sp_dropextendedproc 'xp_hello';  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_addextendedproc &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addextendedproc-transact-sql.md)   
 [sp_helpextendedproc &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-helpextendedproc-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
