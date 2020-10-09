---
description: "'SQLSpecialColumns'"
title: SQLSpecialColumns | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLSpecialColumns function
ms.assetid: dffe02ed-8f79-4c9a-af34-98130bbe5462
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 66413b416482526b45c30e256751342487b005fe
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91868433"
---
# <a name="sqlspecialcolumns"></a>'SQLSpecialColumns'
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Beim Anfordern von Zeilen bezeichgern (*IdentifierType* SQL_BEST_ROWID) gibt **SQLSpecialColumns** ein leeres Resultset (keine Daten Zeilen) für einen angeforderten Bereich außer SQL_SCOPE_CURROW zurück. Das generierte Resultset gibt an, dass die Spalten nur innerhalb dieses Bereichs gültig sind.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt keine Pseudospalten für Bezeichner. Das **SQLSpecialColumns** -Resultset identifiziert alle Spalten als SQL_PC_NOT_PSEUDO.  
  
 **SQLSpecialColumns** kann in einem statischen Cursor ausgeführt werden. Der Versuch, **SQLSpecialColumns** für eine aktualisierbare (keysetgesteuerte oder dynamische) auszuführen, gibt SQL_SUCCESS_WITH_INFO zurück, die angibt, dass der Cursortyp geändert wurde.  
  
## <a name="sqlspecialcolumns-support-for-enhanced-date-and-time-features"></a>SQLSpecialColumns-Unterstützung für erweiterte Funktionen zu Datum und Uhrzeit  
 Informationen zu den Werten, die für die Spalten DATA_TYPE, TYPE_NAME, COLUMN_SIZE, BUFFER_LENGTH und DECIMAL_DIGTS für Datums-/Uhrzeittypen zurückgegeben werden, finden Sie unter [catalog Metadata](../../relational-databases/native-client-odbc-date-time/metadata-catalog.md).  
  
 Weitere allgemeine Informationen finden Sie unter [Verbesserungen bei Datum und Uhrzeit &#40;ODBC-&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlspecialcolumns-support-for-large-clr-udts"></a>SQLSpecialColumns -Unterstützung für große CLR-UDTs  
 **SQLSpecialColumns** unterstützt große benutzerdefinierte CLR-Typen (UDTs). Weitere Informationen finden Sie unter [große CLR-User-Defined Typen &#40;ODBC-&#41;](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLSpecialColumns-Funktion](../../odbc/reference/syntax/sqlspecialcolumns-function.md)   
 [ODBC API Implementation Details](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
