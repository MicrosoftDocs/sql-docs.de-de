---
description: CONTEXT_INFO (Transact-SQL)
title: CONTEXT_INFO (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CONTEXT_INFO_TSQL
- CONTEXT_INFO
dev_langs:
- TSQL
helpviewer_keywords:
- CONTEXT_INFO function
- Multiple Active Result Sets
- context information [SQL Server]
- MARS [SQL Server]
- session context information [SQL Server]
ms.assetid: 571320f5-7228-4b0e-9d01-ab732d2d1eab
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ad1dea00f71039a3dfb37386bfd7ebe63b53a6d6
ms.sourcegitcommit: c74bb5944994e34b102615b592fdaabe54713047
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90990254"
---
# <a name="context_info--transact-sql"></a>CONTEXT_INFO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Diese Funktion gibt den **context_info**-Wert zurück, der entweder für die aktuelle Sitzung bzw. den aktuellen Batch festgelegt oder mithilfe der [SET CONTEXT_INFO](../../t-sql/statements/set-context-info-transact-sql.md)-Anweisung abgeleitet wurde.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CONTEXT_INFO()  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-value"></a>Rückgabewert
Der Wert von **context_info**.
  
Wenn **context_info** nicht festgelegt wurde:
-   Wird in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] NULL zurückgegeben.  
-   Wird in [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ein eindeutiger sitzungsspezifischer GUID zurückgegeben.  
  
## <a name="remarks"></a>Hinweise  
Das MARS-Feature (Multiple Active Result Sets) ermöglicht Anwendungen die Ausführung mehrerer Batches oder Anforderungen zur gleichen Zeit über dieselbe Verbindung. Führt einer der Batches in einer MARS-Sitzung SET CONTEXT_INFO aus, wird der neue Kontextwert von der `CONTEXT_INFO`-Funktion zurückgegeben, wenn die `CONTEXT_INFO`-Funktion im gleichen Batch wie die SET-Anweisung ausgeführt wird. Wenn die `CONTEXT_INFO`-Funktion in einem oder mehreren der anderen Verbindungsbatches ausgeführt wird, gibt die `CONTEXT_FUNCTION` den neuen Wert nur dann zurück, wenn diese Batches nach dem Abschluss des Batches ausgeführt werden, für den die SET-Anweisung ausgeführt wurde.
  
## <a name="permissions"></a>Berechtigungen  
Benötigt keine besonderen Berechtigungen. Die Kontextinformationen sind in den folgenden Systemsichten gespeichert, das direkte Abfragen dieser Sichten erfordert aber die Berechtigungen SELECT und VIEW SERVER STATE:
- **sys.dm_exec_requests**
- **sys.dm_exec_sessions**
- **sys.sysprocesses**
  
## <a name="examples"></a>Beispiele  
In diesem einfachen Beispiel wird der **context_info**-Wert auf `0x1256698456` festgelegt und der Wert dann mithilfe der `CONTEXT_INFO`-Funktion abgerufen.
  
```sql
SET CONTEXT_INFO 0x1256698456;  
GO  
SELECT CONTEXT_INFO();  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen
[SET CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/statements/set-context-info-transact-sql.md)
  
  
