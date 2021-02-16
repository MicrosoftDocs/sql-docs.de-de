---
description: DROP SCHEMA (Transact-SQL)
title: DROP SCHEMA (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP SCHEMA
- DROP_SCHEMA_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting schemas
- schemas [SQL Server], removing
- DROP SCHEMA statement
- dropping schemas
- removing schemas
ms.assetid: 874aa29e-c8ad-41e4-a672-900fdc58f1f6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d7b137d4601200cbbcb444408e17c62d4d9b1b0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340949"
---
# <a name="drop-schema-transact-sql"></a>DROP SCHEMA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Entfernt ein Schema aus der Datenbank.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database  
  
DROP SCHEMA  [ IF EXISTS ] schema_name  
```  
  

```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP SCHEMA schema_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *IF EXISTS*  
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] bis zur [aktuellen Version](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 Löscht das Schema nur, wenn dieses bereits vorhanden ist.  
  
 *schema_name*  
 Der Name des Schemas in der Datenbank.  
  
## <a name="remarks"></a>Bemerkungen  
 Das zu löschende Schema darf keine Objekte enthalten. Falls das Schema Objekte enthält, erzeugt die DROP-Anweisung einen Fehler.  
  
 Informationen zu Schemas werden in der [sys.schemas](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md)-Katalogsicht angezeigt.  
  
 **Achtung** [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die CONTROL-Berechtigung für das Schema oder die ALTER ANY SCHEMA-Berechtigung für die Datenbank.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel beginnt mit einer einzelnen `CREATE SCHEMA`-Anweisung. Von der Anweisung werden das `Sprockets`-Schema, das sich im Besitz von `Krishna` befindet, und die `Sprockets.NineProngs`-Tabelle erstellt. Anschließend wird die `SELECT`-Berechtigung für `Anibal` erteilt und die `SELECT`-Berechtigung für `Hung-Fu` verweigert.  
  
```sql  
CREATE SCHEMA Sprockets AUTHORIZATION Krishna   
    CREATE TABLE NineProngs (source INT, cost INT, partnumber INT)  
    GRANT SELECT TO Anibal   
    DENY SELECT TO [Hung-Fu];  
GO  
```  
  
 Mit den folgenden Anweisungen wird das Schema gelöscht. Beachten Sie, dass zunächst die Tabelle gelöscht werden muss, die sich im Schema befindet.  
  
```sql  
DROP TABLE Sprockets.NineProngs;  
DROP SCHEMA Sprockets;  
GO  
```  
  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/create-schema-transact-sql.md)   
 [ALTER SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/alter-schema-transact-sql.md)   
 [DROP SCHEMA (Transact-SQL)](../../t-sql/statements/drop-schema-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)