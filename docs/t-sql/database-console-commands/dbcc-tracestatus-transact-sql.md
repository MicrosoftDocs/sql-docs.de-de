---
description: DBCC TRACESTATUS (Transact-SQL)
title: DBCC TRACESTATUS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/17/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC_TRACESTATUS_TSQL
- DBCC TRACESTATUS
- TRACESTATUS_TSQL
- TRACESTATUS
dev_langs:
- TSQL
helpviewer_keywords:
- global trace flags [SQL Server]
- status information [SQL Server], trace flags
- trace flags [SQL Server], status information
- DBCC TRACESTATUS statement
- session trace flags [SQL Server]
- displaying trace flag status
ms.assetid: 9be51199-78b4-4b87-ae6e-557246b7e29a
author: pmasl
ms.author: umajay
ms.openlocfilehash: 046c8fa60fc4bc4930089d8c7e9a87a3480bff23
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126275"
---
# <a name="dbcc-tracestatus-transact-sql"></a>DBCC TRACESTATUS (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

Zeigt den Status der Ablaufverfolgungsflags an.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DBCC TRACESTATUS ( [ [ trace# [ ,...n ] ] [ , ] [ -1 ] ] )   
[ WITH NO_INFOMSGS ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*trace#*  
Die Nummer des Ablaufverfolgungsflags, für das der Status angezeigt wird. Wenn *trace#* und -1 nicht angegeben werden, werden alle für die Sitzung aktivierten Ablaufverfolgungsflags angezeigt.
  
*n*  
Ein Platzhalter, der anzeigt, dass mehrere Ablaufverfolgungsflags angegeben werden können.
  
-1  
Zeigt den Status von global aktivierten Ablaufverfolgungsflags an. Wenn -1 ohne *trace#* angegeben wird, werden alle aktivierten Ablaufverfolgungsflags des Typs global angezeigt.
  
WITH NO_INFOMSGS  
Unterdrückt alle Informationsmeldungen mit einem Schweregrad von 0 bis 10.
  
## <a name="result-sets"></a>Resultsets  
In der folgenden Tabelle finden Sie eine Beschreibung der Informationen des Resultsets:
  
|Spaltenname|BESCHREIBUNG|  
|---|---|
|**TraceFlag**|Name des Ablaufverfolgungsflags|  
|**Status**|Zeigt an, ob das Ablaufverfolgungsflag entweder global oder für die Sitzung auf ON oder OFF festgelegt wurde.<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|**Global**|Zeigt an, ob das Ablaufverfolgungsflag global festgelegt ist.<br /><br /> 1 = True<br /><br /> 0 = False|  
|**Sitzungskonsistenz**|Zeigt an, ob das Ablaufverfolgungsflag für die Sitzung festgelegt ist.<br /><br /> 1 = True<br /><br /> 0 = False|  
  
DBCC TRACESTATUS gibt eine Spalte für die Nummer des Ablaufverfolgungsflags und eine Spalte für den Status zurück. Zeigt an, ob das Ablaufverfolgungsflag auf ON (1) oder OFF (0) festgelegt ist. Die Spaltenüberschrift für die Nummer des Ablaufverfolgungsflags lautet entweder **Global Trace Flag** oder **Session Trace Flag**, je nachdem, ob Sie den Status eines Ablaufverfolgungsflags vom Typ „Global“ oder „Session“ überprüfen.
  
## <a name="remarks"></a>Hinweise  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gibt es zwei Typen von Ablaufverfolgungsflags: Sitzung und global. Ablaufverfolgungsflags des Typs Session werden für eine Verbindung aktiviert und sind nur für diese Verbindung sichtbar. Globale Ablaufverfolgungsflags werden auf Serverebene festgelegt und sind für jede Verbindung auf dem Server sichtbar.
  
## <a name="permissions"></a>Berechtigungen  
Erfordert die Mitgliedschaft in der **public** -Rolle.
  
## <a name="examples"></a>Beispiele  
Im folgenden Beispiel wird der Status aller Ablaufverfolgungsflags gezeigt, die zurzeit global aktiviert sind.
  
```sql  
DBCC TRACESTATUS(-1);  
GO  
```  
  
Im folgenden Beispiel wird der Status von Ablaufverfolgungsflags `2528` und `3205` gezeigt.
  
```sql  
DBCC TRACESTATUS (2528, 3205);  
GO  
```  
  
Im folgenden Beispiel wird gezeigt, ob Ablaufverfolgungsflag `3205` global aktiviert ist.
  
```sql  
DBCC TRACESTATUS (3205, -1);  
GO  
```  
  
Im folgenden Beispiel werden alle Ablaufverfolgungsflags gezeigt, die für die aktuelle Sitzung aktiviert sind.
  
```sql  
DBCC TRACESTATUS();  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC TRACEOFF &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceoff-transact-sql.md)  
[DBCC TRACEON &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md)  
[Ablaufverfolgungsflags &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)
  
  
