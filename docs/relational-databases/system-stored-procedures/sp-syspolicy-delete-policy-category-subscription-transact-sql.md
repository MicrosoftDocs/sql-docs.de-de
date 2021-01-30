---
description: sp_syspolicy_delete_policy_category_subscription (Transact-SQL)
title: sp_syspolicy_delete_policy_category_subscription (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_delete_policy_category_subscription_TSQL
- sp_syspolicy_delete_policy_category_subscription
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_delete_policy_category_subscription
ms.assetid: eeab0120-c869-4c95-a79d-6dc418d0b23a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ca183caac2ec8a6a42f8ded33330231b93e72747
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201237"
---
# <a name="sp_syspolicy_delete_policy_category_subscription-transact-sql"></a>sp_syspolicy_delete_policy_category_subscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Löscht ein Richtlinienkategorieabonnement für eine bestimmte Datenbank.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_syspolicy_delete_policy_category_subscription [ @policy_category_subscription_id = ] policy_category_subscription_id  
```  
  
## <a name="arguments"></a>Argumente  
`[ @policy_category_subscription_id = ] policy_category_subscription_id` Der Bezeichner für das Richtlinienkategorieabonnement. *policy_category_subscription_id* ist vom Datentyp **int**.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 Sie müssen sp_syspolicy_delete_policy_category_subscription im Kontext der Systemdatenbank msdb ausführen.  
  
 Sie können ein Richtlinienkategorieabonnement nicht löschen, wenn das Abonnement beauftragt wurde.  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur wird im Kontext des aktuellen Besitzers der gespeicherten Prozedur ausgeführt.  
  
 Zum Abrufen von Werten für *policy_category_subscription_id* können Sie die folgende Abfrage verwenden:  
  
```  
SELECT a.policy_category_subscription_id, a.target_object, b.name AS category_name  
FROM msdb.dbo.syspolicy_policy_category_subscriptions AS a  
INNER JOIN msdb.dbo.syspolicy_policy_categories AS b  
ON a.policy_category_id = b.policy_category_id;  
```  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird ein Richtlinienkategorieabonnement mit der ID 1 gelöscht.  
  
```  
EXEC msdb.dbo.sp_syspolicy_delete_policy_category_subscription @policy_category_subscription_id = 1;  
  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gespeicherte Prozeduren der Richtlinien basierten Verwaltung &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_update_policy_category_subscription &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-update-policy-category-subscription-transact-sql.md)  
  
  
