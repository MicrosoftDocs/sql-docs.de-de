---
description: sp_delete_proxy (Transact-SQL)
title: sp_delete_proxy (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_delete_proxy
- sp_delete_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_proxy
- DROP PROXY statement
ms.assetid: 44a1db13-b7f2-4dab-a1b5-b8dafb41737c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ccc9d7639bbe7f929a28b249e6767d0d703a0068
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89529044"
---
# <a name="sp_delete_proxy-transact-sql"></a>sp_delete_proxy (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Entfernt den angegebenen Proxy.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_delete_proxy [ @proxy_id = ] id , [ @proxy_name = ] 'proxy_name'  
```  
  
## <a name="arguments"></a>Argumente  
`[ @proxy_id = ] id` Die Proxy-ID des zu entfernenden Proxys. Der *proxy_id* ist vom Datentyp **int**und hat den Standardwert NULL.  
  
`[ @proxy_name = ] 'proxy_name'` Der Name des zu entfernenden Proxys. Der *proxy_name* ist vom **Datentyp vom Datentyp sysname**und hat den Standardwert NULL.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="remarks"></a>Bemerkungen  
 Es muss entweder ** \@ proxy_name** oder ** \@ proxy_id** angegeben werden. Wenn beide Argumente angegeben werden, müssen sie sich beide auf denselben Proxy beziehen. Andernfalls erzeugt die gespeicherte Prozedur einen Fehler.  
  
 Wenn ein Auftragsschritt auf den angegebenen Proxy verweist, kann der Proxy nicht gelöscht werden und die gespeicherte Prozedur erzeugt einen Fehler.  
  
## <a name="permissions"></a>Berechtigungen  
 Standardmäßig können nur Mitglieder der festen Server Rolle **sysadmin** **sp_delete_proxy**ausführen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der Proxy `Catalog application proxy` gelöscht.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_proxy  
    @proxy_name = N'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_add_proxy &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)  
  
  
