---
description: sys.dm_tran_current_transactions (Transact-SQL)
title: sys.dm_tran_current_transaction (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_current_transaction
- sys.dm_tran_current_transaction_TSQL
- dm_tran_current_transaction_TSQL
- dm_tran_current_transaction
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_current_transaction dynamic management view
ms.assetid: 75d5697d-b390-4963-99b8-fa0b4244a40c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 40e4b9e502756d12a88bbf02692519f8fb68dbf2
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100347957"
---
# <a name="sysdm_tran_current_transaction-transact-sql"></a>sys.dm_tran_current_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine einzelne Zeile zurück, in der die Statusinformationen der Transaktion in der aktuellen Sitzung angezeigt werden.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_tran_current_transaction**.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.dm_tran_current_transaction  
```  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**transaction_id**|**bigint**|Transaktions-ID der aktuellen Momentaufnahme.|  
|**transaction_sequence_num**|**bigint**|Sequenznummer der Transaktion, die die Datensatzversion generiert.|  
|**transaction_is_snapshot**|**bit**|Der Momentaufnahmeisolationsstatus. Dieser Wert beträgt 1, wenn die Transaktion unter der Momentaufnahmeisolation gestartet wird. Anderenfalls ist der Wert 0.|  
|**first_snapshot_sequence_num**|**bigint**|Niedrigste Transaktionssequenznummer der Transaktionen, die beim Erstellen einer Momentaufnahme aktiviert waren. Bei der Ausführung einer Momentaufnahmetransaktion wird eine Momentaufnahme aller zu diesem Zeitpunkt aktiven Transaktionen erstellt. Für NonSnapshot-Transaktionen wird in dieser Spalte 0 angezeigt.|  
|**last_transaction_sequence_num**|**bigint**|Globale Sequenznummer. Dieser Wert stellt die letzte Transaktionssequenznummer dar, die vom System generiert wurde.|  
|**first_useful_sequence_num**|**bigint**|Globale Sequenznummer. Dieser Wert stellt die älteste Transaktionssequenznummer der Transaktion dar, die über Zeilenversionen verfügt, die im Versionsspeicher beibehalten werden müssen. Zeilenversionen, die von früheren Transaktionen erstellt wurden, können entfernt werden.|  
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird ein Testszenario verwendet, in dem vier gleichzeitige Transaktionen, die jeweils durch eine Transaktionssequenznummer (XSN) identifiziert werden, in einer Datenbank ausgeführt werden, für die die Optionen ALLOW_SNAPSHOT_ISOLATION und READ_COMMITTED_SNAPSHOT auf ON festgelegt sind. Die folgenden Transaktionen werden ausgeführt:  
  
-   XSN-57 ist ein Updatevorgang auf der serialisierbaren Isolationsstufe.  
  
-   XSN-58 entspricht XSN-57.  
  
-   XSN-59 ist ein Select-Vorgang auf der Momentaufnahmeisolationsstufe.  
  
-   XSN-60 entspricht XSN-59.  
  
 Die folgende Abfrage wird im Bereich jeder Transaktion ausgeführt.  
  
```  
SELECT   
    transaction_id  
   ,transaction_sequence_num  
   ,transaction_is_snapshot  
   ,first_snapshot_sequence_num  
   ,last_transaction_sequence_num  
   ,first_useful_sequence_num  
  FROM sys.dm_tran_current_transaction;  
```  
  
 Im Folgenden wird das Ergebnis für XSN-59 aufgeführt.  
  
```  
transaction_id       transaction_sequence_num transaction_is_snapshot  
-------------------- ------------------------ -----------------------  
9387                 59                       1                         
  
first_snapshot_sequence_num last_transaction_sequence_num  
--------------------------- -----------------------------  
57                               61                        
  
first_useful_sequence_num  
-------------------------  
57  
```  
  
 Die Ausgabe zeigt an, dass es sich bei XSN-59 um eine Momentaufnahmetransaktion handelt, die XSN-57 als die erste Transaktion verwendet, die beim Start von XSN-59 aktiviert war. Dies bedeutet, dass XSN-59 Daten liest, für die von Transaktionen, deren Transaktionssequenznummer niedriger ist als XSN-57, ein Commit ausgeführt wurde.  
  
 Im Folgenden wird das Ergebnis für XSN-57 aufgeführt.  
  
```  
transaction_id       transaction_sequence_num transaction_is_snapshot  
-------------------- ------------------------ -----------------------  
9295                 57                       0  
  
first_snapshot_sequence_num last_transaction_sequence_num  
--------------------------- -----------------------------  
NULL                        61  
  
first_useful_sequence_num  
-------------------------  
57  
```  
  
 Da XSN-57 keine Momentaufnahmetransaktion ist, weist `first_snapshot_sequence_num` den Wert `NULL` auf.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungssichten und Funktionen in Verbindung mit Transaktionen &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


