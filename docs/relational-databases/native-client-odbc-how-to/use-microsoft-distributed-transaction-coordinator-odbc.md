---
description: Verwenden von Microsoft Distributed Transaction Coordinator (ODBC)
title: Distributed Transaction Coordinator (ODBC)
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- MS DTC, using
ms.assetid: 12a275e1-8c7e-436d-8a4e-b7bee853b35c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d8be0e1e3ffc0692eae8b2d1dfa6464ad2e7edae
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91868834"
---
# <a name="use-microsoft-distributed-transaction-coordinator-odbc"></a>Verwenden von Microsoft Distributed Transaction Coordinator (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-update-two-or-more-sql-servers-by-using-ms-dtc"></a>So aktualisieren Sie zwei oder mehr SQL Server mit MS DTC  
  
1.  Stellen Sie mit der MS DTC OLE-Funktion DtcGetTransactionManager eine Verbindung mit MS DTC her. Informationen zu MS DTC finden Sie unter Microsoft Distributed Transaction Coordinator.  
  
2.  Nennen Sie SQL driverconnect einmal für jede SQL Server Verbindung, die Sie einrichten möchten.  
  
3.  Rufen Sie die MS DTC OLE-Funktion ITransactionDispenser::BeginTransaction auf, um eine MS DTC-Transaktion zu starten und ein Transaction-Objekt zu erhalten, das diese Transaktion repräsentiert.  
  
4.  Rufen Sie [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) mindestens einmal für jede ODBC-Verbindung auf, die Sie in der MS DTC-Transaktion auflisten möchten. Der zweite [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md)-Parameter muss SQL_ATTR_ENLIST_IN_DTC lauten, und der dritte Parameter muss das Transaktionsobjekt (aus Schritt 3) sein.  
  
5.  Rufen Sie [SQLExecDirect](../../odbc/reference/syntax/sqlexecdirect-function.md) einmal für jeden SQL Server auf, den Sie aktualisieren möchten.  
  
6.  Rufen Sie MS DTC OLE-Funktion ITransaction::Commit auf, um ein Commit für die MS DTC-Transaktion auszuführen. Das Transaction-Objekt ist nicht mehr gültig.  

 Um eine Reihe von MS DTC-Transaktionen auszuführen, wiederholen Sie die Schritte 3 bis 6.  
  
 Um den Verweis auf das Transaction-Objekt freizugeben, rufen Sie die MS DTC OLE-Funktion ITransaction::Return auf.  
  
 Wenn Sie eine ODBC-Verbindung mit einer MS DTC-Transaktion und dieselbe Verbindung dann mit einer lokalen SQL Server-Transaktion verwenden möchten, rufen Sie [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) mit SQL_DTC_DONE auf.  
  
> [!NOTE]  
>  Sie können [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) und [SQLExecDirect](../../odbc/reference/syntax/sqlexecdirect-function.md) auch nacheinander für jeden SQL Server aufrufen, statt sie gemäß dem Vorschlag in den Schritten 4 und 5 aufzurufen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ausführen von Transaktionen &#40;ODBC-&#41;](../native-client/odbc/performing-transactions-in-odbc.md)  
  
