---
description: sp_grantdbaccess (Transact-SQL)
title: sp_grantdbaccess (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_grantdbaccess
- sp_grantdbaccess_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_grantdbaccess
ms.assetid: 3eb09513-03f1-42f8-9917-3a1f3a579bec
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: 5a08a424a09d8f0178137b5a83bb64b44f5b0b04
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208984"
---
# <a name="sp_grantdbaccess-transact-sql"></a>sp_grantdbaccess (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fügt der aktuellen Datenbank einen Datenbankbenutzer hinzu.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen [Create User](../../t-sql/statements/create-user-transact-sql.md) .  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
sp_grantdbaccess [ @loginame = ] 'login'  
    [ , [ @name_in_db = ] 'name_in_db' [ OUTPUT ] ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @loginame = ] 'login_ '` Der Name der Windows-Gruppe, Windows-Anmeldung oder-Anmeldung, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dem neuen Datenbankbenutzer zugeordnet werden soll. Namen von Windows-Gruppen und Windows-Anmeldungen müssen mit einem Windows-Domänen Namen in der Form *Domänen* \\ *Anmeldung* qualifiziert werden, z. b. **London\JoeB**. Der Anmeldename darf noch keinem Benutzer in der Datenbank zugewiesen sein. *login* ist vom Datentyp **sysname** und hat keinen Standardwert.  
  
``[ @name_in_db = ] 'name_in_db' [ OUTPUT]`` Der Name des neuen Daten Bank Benutzers. *name_in_db* ist eine OUTPUT-Variable vom Datentyp **sysname**. Der Standardwert ist NULL. Wenn dieses Argument nicht angegeben ist, wird *login* verwendet. Wenn die Angabe als Ausgabevariable mit dem Wert NULL angegeben ist, wird **\@ name_in_db** auf *Login* festgelegt. *name_in_db* darf in der aktuellen Datenbank noch nicht vorhanden sein.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_grantdbaccess** ruft CREATE USER auf, wodurch zusätzliche Optionen unterstützt werden. Weitere Informationen zum Erstellen von Datenbankbenutzern finden Sie unter [Create User &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md). Wenn Sie einen Datenbankbenutzer aus einer Datenbank entfernen möchten, verwenden Sie hierzu [DROP USER](../../t-sql/statements/drop-user-transact-sql.md).  
  
 **sp_grantdbaccess** kann nicht innerhalb einer benutzerdefinierten Transaktion ausgeführt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Setzt die Mitgliedschaft in der festen Datenbankrolle **db_owner** oder in der festen Datenbankrolle **db_accessadmin** voraus.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der aktuellen Datenbank mithilfe von `CREATE USER` ein Datenbankbenutzer für den Windows-Anmeldenamen `Edmonds\LolanSo` hinzugefügt. Der neue Benutzer erhält den Namen `Lolan`. Dies ist die bevorzugte Methode zum Erstellen eines Datenbankbenutzers.  
  
```sql
CREATE USER Lolan FOR LOGIN [Edmonds\LolanSo];  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Security Stored Procedures &#40;Transact-SQL&#41; (Gespeicherte Sicherheitsprozeduren (Transact-SQL))](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
