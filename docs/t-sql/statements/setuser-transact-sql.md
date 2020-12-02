---
description: SETUSER (Transact-SQL)
title: SETUSER (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SETUSER_TSQL
- SETUSER
dev_langs:
- TSQL
helpviewer_keywords:
- delegation [SQL Server]
- impersonation [SQL Server]
- SETUSER statement
- user impersonation [SQL Server]
ms.assetid: 7acfac5c-9ad6-4226-b874-7add36c4ea43
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e542bb0ef16017744c7f5f61d7358ec66149e1e8
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "88478667"
---
# <a name="setuser-transact-sql"></a>SETUSER (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ermöglicht einem Mitglied der festen Serverrolle **sysadmin** oder dem Besitzer einer Datenbank die Identität eines anderen Benutzers anzunehmen.  
  
> [!IMPORTANT]  
>  SETUSER ist nur aus Gründen der Abwärtskompatibilität enthalten. SETUSER wird in einer künftigen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] möglicherweise nicht mehr unterstützt. Es empfiehlt sich, stattdessen [EXECUTE AS](../../t-sql/statements/execute-as-transact-sql.md) zu verwenden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
SETUSER [ 'username' [ WITH NORESET ] ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 **'** *username* **'**  
 Dies ist der Name eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]- oder Windows-Benutzers in der aktuellen Datenbank, dessen Identität angenommen werden soll. Wird *username* nicht angegeben, so wird die ursprüngliche Identität des Systemadministrators oder des Datenbankbesitzers, der eine fremde Identität angenommen hat, wiederhergestellt.  
  
 WITH NORESET  
 Gibt an, dass bei nachfolgenden SETUSER-Anweisungen (ohne Angabe von *username*) die ursprüngliche Identität des Systemadministrators bzw. Datenbankbesitzers nicht wiederhergestellt werden soll.  
  
## <a name="remarks"></a>Bemerkungen  
 SETUSER kann von einem Mitglied der festen Serverrolle **sysadmin** oder dem Besitzer einer Datenbank verwendet werden, um die Identität eines anderen Benutzers zum Testen der Berechtigungen dieses Benutzers anzunehmen. Die Mitgliedschaft in der festen Datenbankrolle db_owner reicht nicht aus.  
  
 Verwenden Sie SETUSER nur für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Benutzer. SETUSER wird nicht für Windows-Benutzer unterstützt. Wenn Sie mit SETUSER die Identität eines anderen Benutzers angenommen haben, gehören alle Objekte, die Sie unter seiner Identität erstellen, diesem Benutzer. Wenn der Datenbankbesitzer beispielsweise die Identität der Benutzerin **Margaret** annimmt und eine Tabelle namens **orders** erstellt, gehört diese **orders**-Tabelle der Benutzerin **Margaret** und nicht dem Systemadministrator.  
  
 SETUSER bleibt wirksam, bis eine andere SETUSER-Anweisung ausgeführt oder die aktuelle Datenbank mit der USE-Anweisung gewechselt wird.  
  
> [!NOTE]  
>  Falls SETUSER WITH NORESET verwendet wird, muss der Datenbankbesitzer bzw. der Systemadministrator sich ab- und wieder anmelden, um seine ursprünglichen Berechtigungen wiederherzustellen.  
  
## <a name="permissions"></a>Berechtigungen  
 Setzt die Mitgliedschaft in der festen Serverrolle **sysadmin** oder den Besitz der Datenbank voraus. Die Mitgliedschaft in der festen Datenbankrolle **db_owner** reicht nicht aus.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel zeigt, wie der Datenbankbesitzer die Identität eines anderen Benutzers annehmen kann. Die Benutzerin `mary` hat eine Tabelle namens `computer_types` erstellt. Mithilfe von SETUSER nimmt der Datenbankbesitzer die Identität von `mary` an, um dem Benutzer `joe` den Zugriff auf die `computer_types`-Tabelle zu erteilen. Anschließend nimmt der Benutzer wieder seine eigene Identität an.  
  
```sql
SETUSER 'mary';  
GO  
GRANT SELECT ON computer_types TO joe;  
GO  
--To revert to the original user  
SETUSER;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md)  
  
  
