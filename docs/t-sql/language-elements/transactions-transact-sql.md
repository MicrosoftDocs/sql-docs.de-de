---
description: Transaktionen (Transact-SQL)
title: Transaktionen (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Transactions
- Transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transactions [SQL Server]
- transactions [SQL Server], about transactions
- UOW [SQL Server]
- unit of work [SQL Server]
ms.assetid: 1485c375-921a-42af-a871-bb333cc08d3e
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3588c21ff18120c390c6ecb4484d02bc7f83dbb9
ms.sourcegitcommit: 8f062015c2a033f5a0d805ee4adabbe15e7c8f94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91227492"
---
# <a name="transactions-transact-sql"></a>Transaktionen (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Eine Transaktion ist eine einzelne Arbeitseinheit. Ist eine Transaktion erfolgreich, wird für alle Datenänderungen, die während der Transaktion vorgenommen wurden, ein Commit ausgeführt, und sie werden dauerhaft in der Datenbank gespeichert. Treten während einer Transaktion Fehler auf, die den Abbruch oder ein Rollback der Transaktion erfordern, werden alle Datenänderungen rückgängig gemacht.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird in den folgenden Transaktionsmodi ausgeführt:  
  
 Autocommittransaktionen  
 Jede einzelne Anweisung ist eine Transaktion.  
  
 Explizite Transaktionen  
 Jede Transaktion wird explizit mit der BEGIN TRANSACTION-Anweisung gestartet und explizit mit einer COMMIT- oder ROLLBACK-Anweisung beendet.  
  
 Implizite Transaktionen  
 Eine neue Transaktion wird implizit gestartet, sobald die vorhergehende Transaktion abgeschlossen ist. Jede Transaktion wird jedoch explizit mit einer COMMIT- oder ROLLBACK-Anweisung beendet.  
  
 Transaktionen mit Batchbereich  
 Trifft nur auf MARS (Multiple Active Result Sets) zu; eine explizite oder implizite [!INCLUDE[tsql](../../includes/tsql-md.md)]-Transaktion, die unter einer MARS-Sitzung gestartet wird, wird zu einer Transaktion im Batchbereich. Für eine Transaktionen mit Batchbereich, für die nach Abschluss des Batches kein Commit oder Rollback ausgeführt wird, wird das Rollback automatisch durch [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vorgenommen.  

> [!NOTE] 
> Besondere Aspekte im Zusammenhang mit Data Warehouse-Produkten finden Sie unter [Transaktionen (Azure Synapse Analytics)](transactions-sql-data-warehouse.md).   

## <a name="in-this-section"></a>In diesem Abschnitt  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt die folgenden Transaktionsanweisungen bereit:  
  
:::row:::
    :::column:::
        [BEGIN DISTRIBUTED TRANSACTION](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)
    :::column-end:::
    :::column:::
        [ROLLBACK TRANSACTION](../../t-sql/language-elements/rollback-transaction-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [BEGIN TRANSACTION](../../t-sql/language-elements/begin-transaction-transact-sql.md)
    :::column-end:::
    :::column:::
        [ROLLBACK WORK](../../t-sql/language-elements/rollback-work-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [COMMIT TRANSACTION](../../t-sql/language-elements/commit-transaction-transact-sql.md)
    :::column-end:::
    :::column:::
        [SAVE TRANSACTION](../../t-sql/language-elements/save-transaction-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [COMMIT WORK](../../t-sql/language-elements/commit-work-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
  
## <a name="see-also"></a>Siehe auch  
 [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../t-sql/statements/set-implicit-transactions-transact-sql.md)   
 [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)  
  
  
