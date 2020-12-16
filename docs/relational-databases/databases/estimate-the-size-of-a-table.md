---
title: Schätzen der Größe einer Tabelle | Microsoft-Dokumentation
description: Verwenden Sie dieses Verfahren, um einzuschätzen, wie viel Speicherplatz zum Speichern von Daten in einer Tabelle in SQL Server erforderlich ist.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- pages [SQL Server], space
- space [SQL Server], tables
- row size [SQL Server]
- size [SQL Server], tables
- column size [SQL Server]
- predicting table size [SQL Server]
- table size [SQL Server]
- estimating table size
- clustered indexes, table size
- disk space [SQL Server], tables
- space allocation [SQL Server], table size
- designing databases [SQL Server], estimating size
- reserved free rows per page [SQL Server]
- calculating table size
ms.assetid: 15c17c92-616f-402e-894b-907a296efe5f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 06cc999eefd3515dc1cda8076d907843bb773931
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474091"
---
# <a name="estimate-the-size-of-a-table"></a>Schätzen der Größe einer Tabelle
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Mit den folgenden Schritten können Sie den Umfang des Speicherplatzes schätzen, der zum Speichern von Daten in einer Tabelle erforderlich ist:  
  
1.  Berechnen Sie den erforderlichen Speicherplatz für den Heap oder gruppierten Index mithilfe der Anweisungen in den Artikeln [Schätzen der Größe eines Heaps](../../relational-databases/databases/estimate-the-size-of-a-heap.md) oder [Schätzen der Größe eines gruppierten Indexes](../../relational-databases/databases/estimate-the-size-of-a-clustered-index.md).  
  
2.  Den für jeden nicht gruppierten Index benötigten Speicherplatz berechnen Sie, indem Sie die Anweisungen ausführen, die Sie unter [Schätzen der Größe eines nicht gruppierten Index](../../relational-databases/databases/estimate-the-size-of-a-nonclustered-index.md)finden.  
  
3.  Fügen Sie die in den Schritten 1 und 2 berechneten Werte hinzu.  

## <a name="see-also"></a>Weitere Informationen  
 [Schätzen der Größe einer Datenbank](../../relational-databases/databases/estimate-the-size-of-a-database.md)   
 [Schätzen der Größe eines Heaps](../../relational-databases/databases/estimate-the-size-of-a-heap.md)   
 [Schätzen der Größe eines gruppierten Indexes](../../relational-databases/databases/estimate-the-size-of-a-clustered-index.md)   
 [Schätzen der Größe eines nicht gruppierten Index](../../relational-databases/databases/estimate-the-size-of-a-nonclustered-index.md)  
  
  
