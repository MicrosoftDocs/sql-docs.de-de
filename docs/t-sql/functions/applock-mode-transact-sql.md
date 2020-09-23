---
description: APPLOCK_MODE (Transact-SQL)
title: APPLOCK_MODE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- APPLOCK_MODE_TSQL
- APPLOCK_MODE
dev_langs:
- TSQL
helpviewer_keywords:
- locking [SQL Server], applications
- applications [SQL Server], locks
- sessions [SQL Server], application locks
- APPLOCK_MODE function
ms.assetid: e43d4917-77f1-45cc-b231-68ba7fee3385
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 40e578837b86b75538e678711293e4adc6b1ac14
ms.sourcegitcommit: cc23d8646041336d119b74bf239a6ac305ff3d31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2020
ms.locfileid: "91111126"
---
# <a name="applock_mode-transact-sql"></a>APPLOCK_MODE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Diese Funktion gibt den Sperrmodus zurück, der vom Besitzer der Sperre für eine bestimmte Anwendungsressource aufrechterhalten wird. APPLOCK_MODE ist eine Anwendungssperrfunktion, die für die aktuelle Datenbank gilt. Die Datenbank umfasst den Bereich der Anwendungssperren.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
APPLOCK_MODE( 'database_principal' , 'resource_name' , 'lock_owner' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
'*database_principal*'  
Der Benutzer, die Rolle oder die Anwendungsrolle, dem bzw. der Berechtigungen für Objekte in einer Datenbank erteilt werden können. Um eine Funktion erfolgreich aufzurufen, muss der Aufrufer der Funktion Mitglied einer der folgenden festen Datenbankrollen sein: *database_principal*, „dbo“ oder „db_owner“.
  
'*resource_name*'  
Der Name einer Sperrressource, der von der Clientanwendung angegeben wird. Die Anwendung muss sicherstellen, dass der Ressourcenname eindeutig ist. Der angegebene Name wird intern als Hashwert codiert, der intern im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sperren-Manager gespeichert werden kann. *resource_name* ist vom Datentyp **nvarchar(255)** und besitzt keinen Standardwert. *resource_name* unterliegt dem Binärvergleich. Daher muss die Groß-/Kleinschreibung unabhängig von den Sortierungseinstellungen der aktuellen Datenbank berücksichtigt werden.
  
'*lock_owner*'  
Der Besitzer der Sperre. Dabei handelt es sich um den Wert von *lock_owner* beim Anfordern der Sperre. *lock_owner* ist vom Datentyp **nvarchar(32)**. Der Wert kann **Transaktion** (Standard) oder **Sitzung** entsprechen.
  
## <a name="return-types"></a>Rückgabetypen
**nvarchar(32)**
  
## <a name="return-value"></a>Rückgabewert
Gibt den Sperrmodus zurück, der vom Besitzer der Sperre für eine bestimmte Anwendungsressource aufrechterhalten wird. Für den Sperrmodus sind die folgenden Werte möglich:
  
||||  
|-|-|-|  
|**NoLock**|**Aktualisieren**|**\*SharedIntentExclusive**|  
|**IntentShared**|**IntentExclusive**|**\*UpdateIntentExclusive**|  
|**Freigegeben**|**Exklusiv**||  
  
*Dieser Sperrmodus ist eine Kombination aus anderen Sperrmodi und kann nicht mithilfe von „sp_getapplock“ explizit aktiviert werden.
  
## <a name="function-properties"></a>Funktionseigenschaften
**Nicht deterministisch**
  
**Nonindexable**
  
**Nonparallelizable**
  
## <a name="examples"></a>Beispiele  
Zwei Benutzer (Benutzer A und Benutzer B) führen in getrennten Sitzungen folgende Sequenz von [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen aus.
  
Benutzer A führt Folgendes aus:
  
```sql
USE AdventureWorks2012;  
GO  
BEGIN TRAN;  
DECLARE @result INT;  
EXEC @result=sp_getapplock  
    @DbPrincipal='public',  
    @Resource='Form1',  
    @LockMode='Shared',  
    @LockOwner='Transaction';  
SELECT APPLOCK_MODE('public', 'Form1', 'Transaction');  
GO  
```  
  
Benutzer B führt dann Folgendes aus:
  
```sql
Use AdventureWorks2012;  
GO  
BEGIN TRAN;  
SELECT APPLOCK_MODE('public', 'Form1', 'Transaction');  
--Result set: NoLock  
  
SELECT APPLOCK_TEST('public', 'Form1', 'Shared', 'Transaction');  
--Result set: 1 (Lock is grantable.)  
  
SELECT APPLOCK_TEST('public', 'Form1', 'Exclusive', 'Transaction');  
--Result set: 0 (Lock is not grantable.)  
GO  
```  
  
Benutzer A führt dann Folgendes aus:
  
```sql
EXEC sp_releaseapplock @Resource='Form1', @DbPrincipal='public';  
GO  
```  
  
Benutzer B führt dann Folgendes aus:
  
```sql
SELECT APPLOCK_TEST('public', 'Form1', 'Exclusive', 'Transaction');  
--Result set: '1' (The lock is grantable.)  
GO  
```  
  
Benutzer A und Benutzer B führen dann Folgendes aus:
  
```sql
COMMIT TRAN;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen
[APPLOCK_TEST &#40;Transact-SQL&#41;](../../t-sql/functions/applock-test-transact-sql.md)  
[sp_getapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getapplock-transact-sql.md)  
[sp_releaseapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-releaseapplock-transact-sql.md)
  
  
