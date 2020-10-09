---
description: Verbesserungen bei Datum und Zeit (ODBC)
title: Datums-und Uhrzeit Verbesserungen (ODBC) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- date/time [ODBC]
- ODBC, date/time improvements
ms.assetid: e31d5ca5-2103-498f-954c-1ee93e217186
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 596dd2755e41eb476ee5588909cae5e13cf1e1ca
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91868949"
---
# <a name="date-and-time-improvements-odbc"></a>Verbesserungen bei Datum und Zeit (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] wurden neue Datums- und Uhrzeitdatentypen eingeführt. In diesem Abschnitt wird beschrieben, wie diese neuen Typen als Erweiterungen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client verfügbar gemacht werden. Eine Übersicht über [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Native Client-Unterstützung für die neuen Datums-und Uhrzeit Datentypen finden Sie unter [Verbesserungen bei Datum und Uhrzeit](../../relational-databases/native-client/features/date-and-time-improvements.md). Ein Beispiel für die ODBC-Datums-/Uhrzeitunterstützung finden Sie unter [Verwenden von Datums-und Uhrzeit Typen](../../relational-databases/native-client-odbc-how-to/use-date-and-time-types.md).  
  
 Allgemeine Informationen über Datums- und Uhrzeitdatentypen finden Sie unter [datetime &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime-transact-sql.md).  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 [Datentypunterstützung für ODBC-Verbesserungen bei Datum und Uhrzeit](../../relational-databases/native-client-odbc-date-time/data-type-support-for-odbc-date-and-time-improvements.md)  
 Liefert Informationen über ODBC-Typen, die Datums- und Uhrzeitdatentypen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützen.  
  
 [Metadaten &#40;ODBC-&#41;]()  
 Beschreibt Informationen, die in den Feldern Implementierungs Parameter Deskriptor (IPD) und Implementierungs Zeilen Deskriptor (IRD) zurückgegeben werden, sowie von **SQLColumns** und **sqlprocedurecolrens**zurückgegebene Spalten Metadaten. Beschreibt auch die von **SQLGetTypeInfo**zurückgegebenen Metadaten des Datentyps.  
  
 [DateTime-Datentyp Konvertierungen &#40;ODBC-&#41;](../../relational-databases/native-client-odbc-date-time/datetime-data-type-conversions-odbc.md)  
 Beschreibt die Konvertierung zwischen datetime- und datetimeoffset-Werten.  
  
 [sql_variant-Unterstützung für Datum- und Uhrzeit-Typen](../../relational-databases/native-client-odbc-date-time/sql-variant-support-for-date-and-time-types.md)  
 Beschreibt die Unterstützung der SQL_VARIANT-Funktion für die verbesserte Datums- und Uhrzeitfunktionalität.  
  
 [Massen Kopier Änderungen für verbesserte Datums-und Uhrzeittypen &#40;OLE DB und ODBC-&#41;](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md)  
 Beschreibt Datums-/Uhrzeitverbesserungen zur Unterstützung von Massenkopiervorgängen.  
  
 [Verbessertes Verhalten des Datums-und Uhrzeittyps mit früheren Versionen von SQL Server &#40;ODBC&#41;](../../relational-databases/native-client-odbc-date-time/enhanced-date-and-time-type-behavior-with-previous-sql-server-versions-odbc.md)  
 Beschreibt das erwartete Verhalten, wenn eine Client Anwendung, die verbesserte Datums-und Uhrzeit Funktionen verwendet, mit einer älteren Version von kommuniziert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und wenn ein Client, der mit einer älteren Version von Native Client kompiliert wurde, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Befehle an einen Server sendet, der erweiterte Datums-und Uhrzeit Funktionen unterstützt.  
  
 [ODBC API-Unterstützung für verbesserte Funktionen für Datum und Uhrzeit](../../relational-databases/native-client-odbc-date-time/odbc-api-support-for-enhanced-date-and-time-features.md)  
 Listet die ODBC-Funktionen auf, die die verbesserte Datums- und Uhrzeitfunktionalität unterstützen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  
