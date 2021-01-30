---
description: sp_droplinkedsrvlogin (Transact-SQL)
title: sp_droplinkedsrvlogin (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_droplinkedsrvlogin_TSQL
- sp_droplinkedsrvlogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_droplinkedsrvlogin
ms.assetid: 75a4a040-72d5-4d29-8304-de0aa481ad4b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c6e67dc1b33fb5c5655992b11367ab60639c8654
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209320"
---
# <a name="sp_droplinkedsrvlogin-transact-sql"></a>sp_droplinkedsrvlogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Entfernt eine vorhandene Zuordnung zwischen einem Anmeldenamen auf dem lokalen Server, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird, und einem Anmeldenamen auf dem Verbindungsserver.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_droplinkedsrvlogin [ @rmtsrvname= ] 'rmtsrvname' ,   
   [ @locallogin= ] 'locallogin'  
```  
  
## <a name="arguments"></a>Argumente  
`[ @rmtsrvname = ] 'rmtsrvname'` Der Name eines Verbindungs Servers, für den die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Anmelde Namen Zuordnung gilt. *rmzrvname* ist vom Datentyp **vom Datentyp sysname** und hat keinen Standardwert. *rmzrvname* muss bereits vorhanden sein.  
  
`[ @locallogin = ] 'locallogin'` Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Anmelde Name auf dem lokalen Server, der eine Zuordnung zum Verbindungs Server *rmzrvname* aufweist. *loczuweisung* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert. Eine Zuordnung für " *loczuweisung* " zu " *rmtrvname* " muss bereits vorhanden sein. Wenn der Wert NULL ist, wird die von **sp_addlinkedserver** erstellte Standard Zuordnung, die alle Anmeldungen auf dem lokalen Server den Anmeldungen auf dem Verbindungs Server zuordnet, gelöscht.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn die vorhandene Zuordnung für eine-Anmeldung gelöscht wird, verwendet der lokale Server die von **sp_addlinkedserver** erstellte Standard Zuordnung, wenn eine Verbindung mit dem Verbindungs Server für diesen Anmelde Namen hergestellt wird. Verwenden Sie **sp_addlinkedsrvlogin**, um die Standard Zuordnung zu ändern.  
  
 Wenn die Standard Zuordnung ebenfalls gelöscht wird, können nur Anmeldungen, denen explizit eine Anmelde Namen Zuordnung für den Verbindungs Server zugewiesen wurde, mithilfe von **sp_addlinkedsrvlogin** auf den Verbindungs Server zugreifen.  
  
 **sp_droplinkedsrvlogin** kann nicht innerhalb einer benutzerdefinierten Transaktion ausgeführt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER ANY LOGIN-Berechtigung auf dem Server.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-removing-the-login-mapping-for-an-existing-user"></a>A. Entfernen der Anmeldenamenzuordnung für einen vorhandenen Benutzer  
 Im folgenden Beispiel wird die Zuordnung für den Anmeldenamen `Mary` vom lokalen Server zum Verbindungsserver `Accounts` entfernt. Daher verwendet der Anmeldename `Mary` die standardmäßige Anmeldenamenzuordnung.  
  
```  
EXEC sp_droplinkedsrvlogin 'Accounts', 'Mary';  
```  
  
### <a name="b-removing-the-default-login-mapping"></a>B. Entfernen der Standardanmeldenamenzuordnung  
 Im folgenden Beispiel wird die standardmäßige Anmeldenamenzuordnung entfernt, die durch das Ausführen von `sp_addlinkedserver` auf dem Verbindungsserver `Accounts` erstellt wurde.  
  
```  
EXEC sp_droplinkedsrvlogin 'Accounts', NULL;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
