---
description: sp_syscollector_update_collection_item (Transact-SQL)
title: sp_syscollector_update_collection_item (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_syscollector_update_collection_item
- sp_syscollector_update_collection_item_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_update_collection_item
ms.assetid: 7a0d36c8-c6e9-431d-a5a4-6c1802bce846
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2dc7d6967f4bfa7aa1c22f4cfa5c55e06127455b
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89534859"
---
# <a name="sp_syscollector_update_collection_item-transact-sql"></a>sp_syscollector_update_collection_item (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Wird verwendet, um die Eigenschaften eines benutzerdefinierten Sammelelements zu ändern oder um ein benutzerdefiniertes Sammelelement umzubenennen.  
  
 
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_syscollector_update_collection_item   
      [ [ @collection_item_id = ] collection_item_id ]  
    , [ [ @name = ] 'name' ]  
    , [ [ @new_name = ] 'new_name' ]  
    , [ [ @frequency = ] frequency ]  
    , [ [ @parameters = ] 'parameters' ]  
```  
  
## <a name="arguments"></a>Argumente  
 [ @collection_item_id =] *collection_item_id*  
 Ist der eindeutige Bezeichner, der das Sammel Element identifiziert. *collection_item_id* ist vom Datentyp **int** und hat den Standardwert NULL. *collection_item_id* muss einen Wert haben, wenn der *Name* NULL ist.  
  
 [ @name =] '*Name*'  
 Der Name des Sammelelements. *Name ist vom Datentyp* **vom Datentyp sysname** und hat den Standardwert NULL. der *Name* muss einen Wert haben, wenn *collection_item_id* NULL ist.  
  
 [ @new_name =] '*new_name*'  
 Der neue Name für das Sammelelement. *new_name* ist vom **Datentyp vom Datentyp sysname**und kann, wenn verwendet, keine leere Zeichenfolge sein.  
  
 *new_name* muss eindeutig sein. Wenn Sie eine Liste der aktuellen Namen von Sammelelementen abrufen möchten, fragen Sie die syscollector_collection_items-Systemsicht ab.  
  
 [ @frequency =] *Häufigkeit*  
 Die Häufigkeit (in Sekunden), mit der Daten durch dieses Sammelelement aufgezeichnet werden. *Frequency* ist vom Datentyp **int**und hat den Standardwert 5. der Minimalwert, der angegeben werden kann.  
  
 [ @parameters =] '*Parameter*'  
 Die Eingabeparameter für das Sammelelement. *Parameter* ist vom Typ **XML** und hat den Standardwert NULL. Das *Parameter* Schema muss dem Parameter Schema des Sammler Typs entsprechen.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder 1 (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 Wenn der Sammlungssatz auf den Modus ohne Zwischenspeicherung festgelegt ist, werden Änderungen der Häufigkeit ignoriert, da dieser Modus bewirkt, dass sowohl die Datensammlung als auch der Datenupload dem Zeitplan entsprechend stattfinden, der für den Sammlungssatz angegeben wurde. Zum Anzeigen des Status des Sammlungssatzes führen Sie die folgende Abfrage aus. Ersetzen Sie `<collection_item_id>` durch die ID des zu aktualisierenden Sammelelements.  
  
```  
USE msdb;  
GO  
SELECT cs.collection_set_id, collection_set_uid, cs.name   
    ,'is running' = CASE WHEN is_running =  0 THEN 'No' ELSE 'Yes' END  
    ,'cache mode' = CASE WHEN collection_mode = 0 THEN 'Cached mode' ELSE 'Non-cached mode' END  
FROM syscollector_collection_sets AS cs  
JOIN syscollector_collection_items AS ci   
ON ci.collection_set_id = cs.collection_set_id  
WHERE collection_item_id = <collection_item_id>;  
```  
  
## <a name="permissions"></a>Berechtigungen  
 Damit dieser Vorgang ausgeführt werden kann, ist die Mitgliedschaft in der festen Datenbankrolle dc_admin oder dc_operator (mit EXECUTE-Berechtigung) erforderlich. Obwohl die dc_operator-Rolle diese gespeicherte Prozedur ausführen kann, können die Eigenschaften von den Mitgliedern dieser Rolle nur begrenzt geändert werden. Die folgenden Eigenschaften können nur von dc_admin geändert werden:  
  
-   @new_name  
  
-   @parameters  
  
## <a name="examples"></a>Beispiele  
 Die folgenden Beispiele basieren auf dem Sammel Element, das in dem in [sp_syscollector_create_collection_item &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-item-transact-sql.md)definierten Beispiel erstellt wurde.  
  
### <a name="a-changing-the-collection-frequency"></a>A. Ändern der Sammlungshäufigkeit  
 Im folgenden Beispiel wird die Sammlungshäufigkeit für das angegebene Sammelelement geändert.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_update_collection_item   
@name = N'My custom TSQL query collector item',  
@frequency = 3000;  
GO  
```  
  
### <a name="b-renaming-a-collection-item"></a>B. Umbenennen eines Sammelelements  
 Im folgenden Beispiel wird ein Sammelelement umbenannt.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_update_collection_item   
@name = N'My custom TSQL query collector item',  
@new_name = N'My modified TSQL item';  
GO  
```  
  
### <a name="c-changing-the-parameters-of-a-collection-item"></a>C. Ändern der Parameter eines Sammelelements  
 Im folgenden Beispiel werden die dem Sammelelement zugeordneten Parameter geändert. Die im `<Value>`-Attribut definierte Anweisung wird geändert, und das `UseSystemDatabases`-Attribut wird auf false festgelegt. Wenn Sie die aktuellen Parameter für dieses Element anzeigen möchten, fragen Sie die Parameter-Spalte in der syscollector_collection_items-Systemsicht ab. Möglicherweise müssen Sie den Wert für `@collection_item_id` ändern.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_update_collection_item   
@collection_item_id = 9,   
@parameters = '  
    \<ns:TSQLQueryCollector xmlns:ns="DataCollectorType">  
        <Query>  
            <Value>SELECT * FROM sys.dm_db_index_usage_stats</Value>  
            <OutputTable>MyOutputTable</OutputTable>  
        </Query>  
        <Databases>  
            <Database> UseSystemDatabases = "false"   
                       UseUserDatabases = "true"</Database>  
        </Databases>  
    \</ns:TSQLQueryCollector>';  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Datensammlung](../../relational-databases/data-collection/data-collection.md)   
 [sp_syscollector_create_collection_item &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-item-transact-sql.md)   
 [syscollector_collection_items &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-items-transact-sql.md)  
  
  
