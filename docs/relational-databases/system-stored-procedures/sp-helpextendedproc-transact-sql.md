---
description: sp_helpextendedproc (Transact-SQL)
title: sp_helpextendedproc (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_helpextendedproc
- sp_helpextendedproc_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpextendedproc
ms.assetid: 7e1f017e-c898-4225-b375-6a73ef9aac7b
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 68bc88bdaddc873c0f272ffef37d6465cddb45af
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88493227"
---
# <a name="sp_helpextendedproc-transact-sql"></a>sp_helpextendedproc (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Zeigt die zurzeit definierten erweiterten gespeicherten Prozeduren und den Namen der DLL (Dynamic Link Library) an, zu der die Prozedur (Funktion) gehört.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen die [CLR-Integration](../../relational-databases/clr-integration/common-language-runtime-integration-overview.md) .  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_helpextendedproc [ [@funcname = ] 'procedure' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @funcname = ] 'procedure'` Der Name der erweiterten gespeicherten Prozedur, für die Informationen gemeldet werden. *procedure* ist vom Datentyp **sysname**und hat den Standardwert NULL.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Name der erweiterten gespeicherten Prozedur|  
|**dll**|**nvarchar(255)**|Name der DLL|  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn *procedure* angegeben wird, zeigt **sp_helpextendedproc** Informationen zur angegebenen erweiterten gespeicherten Prozedur an. Andernfalls gibt **sp_helpextendedproc** die Namen aller erweiterten gespeicherten Prozeduren und die Namen der DLLs zurück, zu denen die einzelnen erweiterten gespeicherten Prozeduren gehören.  
  
## <a name="permissions"></a>Berechtigungen  
 Die Berechtigung zum Ausführen von **sp_helpextendedproc** wird **public**erteilt.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-reporting-help-on-all-extended-stored-procedures"></a>A. Anzeigen der Hilfe zu allen erweiterten gespeicherten Prozeduren  
 Im folgenden Beispiel werden Informationen zu allen erweiterten gespeicherten Prozeduren angezeigt.  
  
```  
USE master;  
GO  
EXEC sp_helpextendedproc;  
GO  
```  
  
### <a name="b-reporting-help-on-a-single-extended-stored-procedure"></a>B. Anzeigen der Hilfe zu einer einzelnen erweiterten gespeicherten Prozedur  
 Im folgenden Beispiel werden Informationen zur erweiterten gespeicherten Prozedur `xp_cmdshell` angezeigt.  
  
```  
USE master;  
GO  
EXEC sp_helpextendedproc xp_cmdshell;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_addextendedproc &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addextendedproc-transact-sql.md)   
 [sp_dropextendedproc &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-dropextendedproc-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
