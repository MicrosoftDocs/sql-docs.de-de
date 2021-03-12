---
description: SET CONTEXT_INFO (Transact-SQL)
title: SET CONTEXT_INFO (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_CONTEXT_INFO_TSQL
- SET CONTEXT_INFO
dev_langs:
- TSQL
helpviewer_keywords:
- context information [SQL Server]
- CONTEXT_INFO option
- SET CONTEXT_INFO statement
ms.assetid: a0b7b9f3-dbda-4350-a274-bd9ecd5c0a74
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 271cf281b2e3dc5f015dd02bc1778fab24be0794
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464159"
---
# <a name="set-context_info-transact-sql"></a>SET CONTEXT_INFO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Ordnet bis zu 128 Byte an binären Informationen der aktuellen Sitzung oder Verbindung zu.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
SET CONTEXT_INFO { binary_str | @binary_var }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *binary_str*  
 Eine Konstante vom Typ **binary** oder eine implizit in **binary** konvertierbare Konstante, die der aktuellen Sitzung oder Verbindung zugeordnet werden soll.  
  
 **@** *binary_var*  
 Eine Variable vom Typ **varbinary** oder **binary** mit einem Kontextwert, der der aktuellen Sitzung oder Verbindung zugeordnet werden soll.  
  
## <a name="remarks"></a>Bemerkungen  
 Die bevorzugte Vorgehensweise zum Abrufen von Kontextinformationen für die aktuelle Sitzung stellt die Verwendung der CONTEXT_INFO-Funktion dar. Informationen zum Sitzungskontext werden zudem in den **context_info**-Spalten der folgenden Systemsichten gespeichert:  
  
-   **sys.dm_exec_requests**  
  
-   **sys.dm_exec_sessions**  
  
-   **sys.sysprocesses**  
  
 SET CONTEXT_INFO kann nicht in einer benutzerdefinierten Funktion angegeben werden. Sie können mit SET CONTEXT_INFO keinen NULL-Wert angeben, da in den Sichten, die diese Werte enthalten, keine NULL-Werte zugelassen sind.  
  
 SET CONTEXT_INFO akzeptiert keine Ausdrücke, bei denen es sich nicht um Konstanten oder Variablennamen handelt. Wenn Sie die Kontextinformationen für das Ergebnis eines Funktionsaufrufes festlegen möchten, müssen Sie das Ergebnis des Funktionsaufrufes zuerst in einer Variablen vom Datentyp **binary** oder **varbinary** platzieren.  
  
 Wenn Sie SET CONTEXT_INFO in einer gespeicherten Prozedur oder einem Trigger ausführen, ist im Gegensatz zu anderen SET-Anweisungen der neue Wert für die Kontextinformationen persistent, nachdem die gespeicherte Prozedur oder der Trigger beendet wurden.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-setting-context-information-by-using-a-constant"></a>A. Festlegen von Kontextinformationen durch Verwenden einer Konstante  
 Im folgenden Beispiel wird die Verwendung von `SET CONTEXT_INFO` durch das Festlegen des Wertes und Anzeigen der Ergebnisse erläutert. Beachten Sie, dass zum Abfragen von `sys.dm_exec_sessions` die SELECT- und VIEW SERVER STATE-Berechtigungen erforderlich sind. Bei Verwendung der CONTEXT_INFO-Funktion gilt dies nicht.  
  
```sql
SET CONTEXT_INFO 0x01010101;  
GO  
SELECT context_info   
FROM sys.dm_exec_sessions  
WHERE session_id = @@SPID;  
GO  
```  
  
### <a name="b-setting-context-information-by-using-a-function"></a>B. Festlegen von Kontextinformationen durch Verwenden einer Funktion  
 Im folgenden Beispiel wird die Verwendung der Ausgabe einer Funktion zum Festlegen des Kontextwertes gezeigt, wobei der Wert aus der Funktion zunächst in einer Variablen vom Typ **binary** platziert werden muss.  
  
```sql
DECLARE @BinVar varbinary(128);  
SET @BinVar = CAST(REPLICATE( 0x20, 128 ) AS varbinary(128) );  
SET CONTEXT_INFO @BinVar;  
  
SELECT CONTEXT_INFO() AS MyContextInfo;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Sicherheit auf Zeilenebene](../../relational-databases/security/row-level-security.md) [SET-Anweisungen &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [CONTEXT_INFO  &#40;Transact-SQL&#41;](../../t-sql/functions/context-info-transact-sql.md)  
 [SESSION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md)  
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
 [sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)  
  
