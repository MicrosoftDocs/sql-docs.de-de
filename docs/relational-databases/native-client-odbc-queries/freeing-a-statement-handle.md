---
description: Freigeben eines Anweisungshandles
title: Freigeben eines Anweisungs Handles | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- reusing statement handles
- freeing statement handles
- statements [ODBC], statement handles
- SQLFreeStmt function
- ODBC applications, statements
- statement handles [ODBC]
ms.assetid: 96fdff84-0ca7-460a-a240-94ee826ea41c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5cf58becc532153617ebcbd3cafc6c89d21b4d00
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88499171"
---
# <a name="freeing-a-statement-handle"></a>Freigeben eines Anweisungshandles
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Es ist effizienter, Anweisungshandles wieder zu verwenden, als sie zu löschen und neu zuzuordnen. Vor dem Ausführen einer neuen SQL-Anweisung für ein Anweisungshandle sollten Anwendungen überprüfen, ob die aktuellen Anweisungseinstellungen korrekt sind. Dazu zählen beispielsweise Anweisungsattribute, Parameterbindungen und Resultsetbindungen. Im Allgemeinen müssen die Bindung von Parametern und Resultsets für die alte SQL-Anweisung aufgehoben werden, indem [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) mit den Optionen SQL_RESET_PARAMS und SQL_UNBIND aufgerufen und dann für die neue SQL-Anweisung erneut gebunden wird.  
  
 Wenn die Anwendung die Verwendung der-Anweisung abgeschlossen hat, wird [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) aufgerufen, um die-Anweisung freizugeben. Beachten Sie, dass **SQLDisconnect** automatisch alle-Anweisungen für eine Verbindung freigibt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ausführen von Abfragen &#40;ODBC-&#41;](../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
  
  
