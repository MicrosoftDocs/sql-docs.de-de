---
description: SET OFFSETS (Transact-SQL)
title: SET OFFSETS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_OFFSETS_TSQL
- OFFSETS_TSQL
- SET OFFSETS
- OFFSETS
dev_langs:
- TSQL
helpviewer_keywords:
- position relative to start of statement [SQL Server]
- OFFSETS option
- offsets [SQL Server]
- SET OFFSETS statement
ms.assetid: c7bcc697-0930-4630-acae-d8ccbfa4414c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7226c450ed3c854847d609e2ddadf2f250ea5691
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189490"
---
# <a name="set-offsets-transact-sql"></a>SET OFFSETS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt den Offset (die relative Position zum Start einer Anweisung) der angegebenen Schlüsselwörter in [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen an DB-Library-Anwendungen zurück.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
 
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
SET OFFSETS keyword_list { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *keyword_list*  
 Eine durch Trennzeichen getrennte Liste von [!INCLUDE[tsql](../../includes/tsql-md.md)] -Konstrukten, einschließlich SELECT, FROM, ORDER, TABLE, PROCEDURE, STATEMENT, PARAM und EXECUTE.  
  
## <a name="remarks"></a>Bemerkungen  
 SET OFFSETS wird nur in DB-Library-Anwendungen verwendet.  
  
 Die Einstellung von SET OFFSETS wird zur Ausführungszeit und nicht zur Analysezeit festgelegt. Ein Festlegen zur Analysezeit bedeutet Folgendes: Befindet sich die SET-Anweisung im Batch oder in der gespeicherten Prozedur, wird die Einstellung unabhängig davon wirksam, ob die Codeausführung tatsächlich diesen Punkt erreicht, und die SET-Anweisung wird wirksam, bevor Anweisungen ausgeführt werden. Auch wenn sich die SET-Anweisung z. B. in einem IF...ELSE-Anweisungsblock befindet, der während der Ausführung niemals erreicht wird, ist die SET-Anweisung dennoch wirksam, da der IF...ELSE-Anweisungsblock analysiert wird.  
  
 Wird SET OFFSETS in einer gespeicherten Prozedur festgelegt, so wird der Wert von SET OFFSETS wiederhergestellt, nachdem die gespeicherte Prozedur die Steuerung zurückgegeben hat. Daher hat eine SET OFFSETS-Anweisung, die in einer dynamischem SQL-Anweisung angegeben wird, keine Auswirkung auf die Anweisungen, die der dynamischen SQL-Anweisung folgen.  
  
 SET PARSEONLY gibt Offsets zurück, wenn SET OFFSETS auf ON festgelegt ist und keine Fehler auftreten.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **public** -Rolle.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SET-Anweisungen (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET PARSEONLY &#40;Transact-SQL&#41;](../../t-sql/statements/set-parseonly-transact-sql.md)  
  
  
