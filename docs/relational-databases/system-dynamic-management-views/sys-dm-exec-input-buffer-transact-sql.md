---
description: sys. dm_exec_input_buffer (Transact-SQL)
title: sys. dm_exec_input_buffer (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_input_buffer
- sys.dm_exec_input_buffer _tsql
- dm_exec_input_buffer
- dm_exec_input_buffer_tsql
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_input_buffer dynamic management function
ms.assetid: fb34a560-bde9-4ad9-aa96-0d4baa4fc104
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4fecdc698dc7015ab47e5a8c97b3990c7e5bf1f4
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89536954"
---
# <a name="sysdm_exec_input_buffer-transact-sql"></a>sys. dm_exec_input_buffer (Transact-SQL)

[!INCLUDE[tsql-appliesto-2014sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md)]

Gibt Informationen zu Anweisungen zurück, die an eine Instanz von übermittelt werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

## <a name="syntax"></a>Syntax

```
sys.dm_exec_input_buffer ( session_id , request_id )
```

## <a name="arguments"></a>Argumente

*session_id* Die Sitzungs-ID, die den Batch ausführt, der gesucht werden soll. *session_id* ist vom Datentyp **smallint**. *session_id* können aus den folgenden dynamischen Verwaltungs Objekten abgerufen werden:

- [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)
- [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)
- [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)

*request_id* Der request_id aus [sys. dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md). *request_id* ist vom Datentyp **int**.

## <a name="table-returned"></a>Zurückgegebene Tabelle

|Spaltenname|Datentyp|BESCHREIBUNG|
|-----------------|---------------|-----------------|
|**event_type**|**nvarchar(256)**|Der Ereignistyp im Eingabepuffer für die angegebene SPID.|
|**parameters**|**smallint**|Alle Parameter, die für die Anweisung bereitgestellt werden.|
|**event_info**|**nvarchar(max)**|Der Text der Anweisung im Eingabepuffer für die angegebene SPID.|

## <a name="permissions"></a>Berechtigungen

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Wenn der Benutzer über die View Server State-Berechtigung verfügt, werden dem Benutzer alle ausgeführten Sitzungen in der Instanz von angezeigt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . andernfalls wird dem Benutzer nur die aktuelle Sitzung angezeigt.

> [!IMPORTANT]
> Das Ausführen dieser DMV außerhalb SQL Server Management Studio gegen SQL Server ohne View Server State-Berechtigungen (z. b. in einem-Triggern, einer gespeicherten Prozedur oder Funktion) löst einen Berechtigungs Fehler für die Master-Datenbank aus.

[!INCLUDE[ssSDS](../../includes/sssds-md.md)]Wenn der Benutzer der Datenbankbesitzer ist, werden dem Benutzer alle ausgeführten Sitzungen auf dem angezeigt, [!INCLUDE[ssSDS](../../includes/sssds-md.md)] andernfalls wird dem Benutzer nur die aktuelle Sitzung angezeigt.

> [!IMPORTANT]
> Das Ausführen dieser DMV außerhalb der SQL Server Management Studio für Azure SQL-Datenbank ohne Besitzer Berechtigungen (z. b. in einem-Triggern, einer gespeicherten Prozedur oder Funktion) löst einen Berechtigungs Fehler für die Master-Datenbank aus.

## <a name="remarks"></a>Hinweise

Diese dynamische Verwaltungsfunktion kann in Verbindung mit sys. dm_exec_sessions oder sys. dm_exec_requests verwendet werden, indem **CROSS APPLY**verwendet wird.

## <a name="examples"></a>Beispiele

### <a name="a-simple-example"></a>A. Einfaches Beispiel

Im folgenden Beispiel wird veranschaulicht, wie eine Sitzungs-ID (SPID) und eine Anforderungs-ID an die-Funktion übergeben werden.

```sql
SELECT * FROM sys.dm_exec_input_buffer (52, 0);
GO
```

### <a name="b-using-cross-apply-to-additional-information"></a>B. Verwenden von Cross Apply für weitere Informationen

Im folgenden Beispiel wird der Eingabepuffer für Sitzungen mit einer Sitzungs-ID größer als 50 aufgelistet.

```sql
SELECT es.session_id, ib.event_info
FROM sys.dm_exec_sessions AS es
CROSS APPLY sys.dm_exec_input_buffer(es.session_id, NULL) AS ib
WHERE es.session_id > 50;
GO
```

## <a name="see-also"></a>Weitere Informationen

- [Execution Related Dynamic Management Views and Functions &#40;Transact-SQL&#41; (Dynamische Verwaltungssichten und Funktionen im Zusammenhang mit der Ausführung (Transact-SQL))](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)
- [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)
- [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)
- [DBCC INPUTBUFFER &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md)
