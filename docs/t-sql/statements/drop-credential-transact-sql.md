---
description: DROP CREDENTIAL (Transact-SQL)
title: DROP CREDENTIAL (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/19/2015
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP CREDENTIAL
- DROP_CREDENTIAL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- removing credentials
- DROP CREDENTIAL statement
- credentials [SQL Server], DROP CREDENTIAL statement
- authentication [SQL Server], credentials
- deleting credentials
- dropping credentials
ms.assetid: df22c826-317d-45a6-b078-186acb65f71e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a8a95dc2eb072659c9b8648e2d78f214a632b521
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189183"
---
# <a name="drop-credential-transact-sql"></a>DROP CREDENTIAL (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Entfernt Anmeldeinformationen für den Server.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DROP CREDENTIAL credential_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *credential_name*  
 Der Name der Anmeldeinformationen, die für den Server entfernt werden sollen.  
  
## <a name="remarks"></a>Bemerkungen  
 Sie können das Geheimnis löschen, das Anmeldeinformationen zugeordnet ist, ohne diese Anmeldeinformationen zu löschen, indem Sie [ALTER CREDENTIAL](../../t-sql/statements/alter-credential-transact-sql.md) verwenden.  
  
 Informationen zu Anmeldeinformationen werden in der **sys.credentials**-Katalogsicht angezeigt.  
  
> [!WARNING]  
>  Proxys werden Anmeldeinformationen zugeordnet. Werden Anmeldeinformationen gelöscht, die von einem Proxy verwendet werden, kann der zugehörige Proxy nicht mehr verwendet werden. Wenn Sie von einem Proxy verwendete Anmeldeinformationen entfernen, löschen Sie den Proxy (indem Sie [sp_delete_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-proxy-transact-sql.md) verwenden und den zugehörigen Proxy anhand von [sp_add_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md) wiederherstellen).  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER ANY CREDENTIAL-Berechtigung. Zum Löschen von Systemanmeldeinformationen ist die CONTROL SERVER-Berechtigung erforderlich.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden die Anmeldeinformationen `Saddles` entfernt.  
  
```sql  
DROP CREDENTIAL Saddles;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Anmeldeinformationen &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [ALTER CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-credential-transact-sql.md)   
 [DROP DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-scoped-credential-transact-sql.md)   
 [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)  
  
  
