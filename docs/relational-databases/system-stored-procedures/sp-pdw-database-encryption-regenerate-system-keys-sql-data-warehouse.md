---
title: sp_pdw_database_encryption_regenerate_system_keys (Azure-Synapse-Analyse)
description: Verwenden Sie **sp_pdw_database_encryption_regenerate_system_keys** , um das Zertifikat und den Verschlüsselungsschlüssel für interne Datenbanken zu drehen, die verschlüsselt werden, wenn TDE auf dem Gerät aktiviert ist.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: bb13e323-a984-4462-8b6d-6019c38ddd9d
author: ronortloff
ms.author: rortloff
ms.reviewer: ''
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 2c6474944be4ef9911f30ed49947e1a0e959a456
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186397"
---
# <a name="sp_pdw_database_encryption_regenerate_system_keys-azure-synapse-analytics"></a>sp_pdw_database_encryption_regenerate_system_keys (Azure-Synapse-Analyse)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

Verwenden Sie **sp_pdw_database_encryption_regenerate_system_keys** , um das Zertifikat und den Verschlüsselungsschlüssel für interne Datenbanken zu drehen, die verschlüsselt werden, wenn TDE auf dem Gerät aktiviert ist. einschließlich `tempdb`, zu verwenden. Dies ist nur erfolgreich, wenn TDE aktiviert ist.  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_database_encryption_regenerate_system_keys  ;  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 Die Prozedur weist keine Parameter auf.  
  
 Diese Prozedur sollte verwendet werden, wenn der Datenverkehr in der Appliance gering ist.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Daten Bank Rolle **sysadmin** oder die **Control Server** -Berechtigung.  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel werden die Verschlüsselungsschlüssel für die Datenbank neu generiert.  
  
```sql  
EXEC sys.sp_pdw_database_encryption_regenerate_system_keys;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_pdw_database_encryption &#40;Azure-Synapse-Analyse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)   
 [sp_pdw_log_user_data_masking &#40;Azure-Synapse-Analyse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)  
  
