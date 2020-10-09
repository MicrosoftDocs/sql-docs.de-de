---
description: SQLRowCount
title: SQLRowCount | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLRowCount function
ms.assetid: 967ed3d4-3d31-4485-ac92-027076ebc829
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 81d32f5f577d9c594556c477620e89435cf66226
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91869137"
---
# <a name="sqlrowcount"></a>SQLRowCount
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Sind Arrays für Parameterwerte für Anweisungsausführungen gebunden, gibt **SQLRowCount** den Wert SQL_ERROR zurück, wenn eine Zeile mit Parameterwerten eine Fehlerbedingung bei der Anweisungsausführung generiert. Kein Wert wird durch das *RowCountPtr* -Argument der Funktion zurückgegeben.  
  
 Die Anwendung nutzt das SQL_ATTR_PARAMS_PROCESSED_PTR-Anweisungsattribut, um die Anzahl der vor Auftreten des Fehlers verarbeiteten Parameter zu erfassen.  
  
 Darüber hinaus kann die Anwendung ein Array von Statuswerten verwenden, die mithilfe des SQL_ATTR_PARAM_STATUS_PTR-Anweisungsattributs gebunden sind, um die Arrayoffsets der ungültigen Parameterzeilen zu erfassen. Die Anwendung kann das Statusarray durchsuchen, um die tatsächliche Anzahl verarbeiteter Zeilen zu bestimmen.  
  
 Wenn eine [!INCLUDE[tsql](../../includes/tsql-md.md)] INSERT-, Update-, DELETE-oder MERGE-Anweisung mit einer OUTPUT-Klausel ausgeführt wird, gibt SQLRowCount nicht die Anzahl der betroffenen Zeilen zurück, bis alle Zeilen im Resultset, das von der OUTPUT-Klausel generiert wurde, genutzt wurden. Um diese Zeilen zu sverarbeiten, rufen Sie SQLFetch oder SQLFetchScroll auf. SQLResultCols gibt-1 zurück, bis alle Ergebniszeilen verarbeitet wurden. Nachdem SQLFetch oder SQLFetchScroll SQL_NO_DATA zurückgegeben hat, muss die Anwendung SQLRowCount aufrufen, um die Anzahl der betroffenen Zeilen zu bestimmen, bevor SQLMoreResults aufgerufen wird, um zum nächsten Ergebnis zu gelangen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLRowCount-Funktion](../../odbc/reference/syntax/sqlrowcount-function.md)   
 [ODBC API Implementation Details](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
