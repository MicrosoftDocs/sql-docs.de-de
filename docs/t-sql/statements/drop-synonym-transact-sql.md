---
description: DROP SYNONYM (Transact-SQL)
title: DROP SYNONYM (Transact-SQL)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP SYNONYM
- DROP_SYNONYM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting synonyms
- synonyms [SQL Server], removing
- removing synonyms
- DROP SYNONYM statement
- dropping synonyms
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a156c4c5bda031b079df53fa3f3796d271b62a1b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236204"
---
# <a name="drop-synonym-transact-sql"></a>DROP SYNONYM (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Entfernt ein Synonym aus einem angegebenen Schema.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DROP SYNONYM [ IF EXISTS ] [ schema. ] synonym_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *IF EXISTS*  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] bis zur [aktuellen Version](/troubleshoot/sql/general/determine-version-edition-update-level)).
  
 Löscht das Synonym nur, wenn dieses bereits vorhanden ist.  
  
 *schema*  
 Gibt das Schema an, in dem das Synonym vorhanden ist. Wird kein Schema angegeben, verwendet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das Standardschema des aktuellen Benutzers.  
  
 *synonym_name*  
 Der Name des Synonyms, das gelöscht werden soll.  
  
## <a name="remarks"></a>Bemerkungen  
 Verweise auf Synonyme sind nicht an ein Schema gebunden. Deshalb können Sie ein Synonym jederzeit löschen. Verweise auf gelöschte Synonyme werden erst zur Laufzeit gefunden.  
  
 In dynamischem SQL können Synonyme erstellt und gelöscht werden. Außerdem kann auf sie verwiesen werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Zum Löschen eines Synonyms muss ein Benutzer mindestens eine der folgenden Bedingungen erfüllen. Folgende Voraussetzungen müssen erfüllt sein:  
  
-   Der Benutzer muss der aktuelle Besitzer eines Synonyms sein.  
  
-   Der Benutzer muss ein Berechtigter für CONTROL für ein Synonym sein.  
  
-   Der Benutzer muss ein Berechtigter sein, der über die ALTER SCHEMA-Berechtigung für das enthaltene Schema verfügt.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird zuerst das Synonym `MyProduct` erstellt und dann gelöscht.  
  
```sql  
USE tempdb;  
GO  
-- Create a synonym for the Product table in AdventureWorks2012.  
CREATE SYNONYM MyProduct  
FOR AdventureWorks2012.Production.Product;  
GO  
-- Drop synonym MyProduct.  
USE tempdb;  
GO  
DROP SYNONYM MyProduct;  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [CREATE SYNONYM &#40;Transact-SQL&#41;](../../t-sql/statements/create-synonym-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
