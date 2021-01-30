---
description: sp_syspolicy_subscribe_to_policy_category (Transact-SQL)
title: sp_syspolicy_subscribe_to_policy_category (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_subscribe_to_policy_category
- sp_syspolicy_subscribe_to_policy_category_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_subscribe_to_policy_category
ms.assetid: de88cc49-bcc8-4dc6-8e59-ad85cfbfb2fb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 6304985143a25c55b3639c1b137e0463a44c6af2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161309"
---
# <a name="sp_syspolicy_subscribe_to_policy_category-transact-sql"></a>sp_syspolicy_subscribe_to_policy_category (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fügt ein Richtlinienkategorieabonnement für die angegebene Datenbank hinzu.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_syspolicy_subscribe_to_policy_category [ @policy_category = ] 'policy_category'  
```  
  
## <a name="arguments"></a>Argumente  
`[ @policy_category = ] 'policy_category'` Der Name der Richtlinien Kategorie, die von der Datenbank abonniert werden soll. *policy_category* ist vom **Datentyp vom Datentyp sysname** und ist erforderlich.  
  
 Wenn Sie Werte für *policy_category* abrufen möchten, Fragen Sie die msdb.dbo.syspolicy_policy_categories-Systemsicht ab.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 Sie müssen sp_syspolicy_subscribe_to_policy_category im Kontext der Datenbank ausführen, in der Sie ein Richtlinienkategorieabonnement hinzufügen möchten.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Datenbankrolle "db_owner".  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird für die angegebene Datenbank ein Abonnement der Richtlinienkategorie "Finance" hinzugefügt.  
  
```  
USE <database_name>;  
  
EXEC sys.sp_syspolicy_subscribe_to_policy_category @policy_category = N'Finance';  
  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gespeicherte Prozeduren der Richtlinien basierten Verwaltung &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_unsubscribe_from_policy_category &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-unsubscribe-from-policy-category-transact-sql.md)  
  
  
