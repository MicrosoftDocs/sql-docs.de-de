---
description: Zusätzliche Tabellenwertparameter-Metadaten
title: Zusätzliche Table-Valued Parameter Metadaten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), catalog functions to retrieve metadata
- table-valued parameters (ODBC), metadata
ms.assetid: 6c193188-5185-4373-9a0d-76cfc150c828
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0944671091961bf03f0715b661a612526d249c67
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97436558"
---
# <a name="additional-table-valued-parameter-metadata"></a>Zusätzliche Tabellenwertparameter-Metadaten
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Zum Abrufen von Metadaten für einen Tabellenwert Parameter ruft eine Anwendung sqlprocedurecolrens auf. Für einen Tabellenwert Parameter gibt sqlprocedurecolrens eine einzelne Zeile zurück. Zwei zusätzliche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -spezifische Spalten, SS_TYPE_CATALOG_NAME und SS_TYPE_SCHEMA_NAME, wurden hinzugefügt, um Schema-und Katalog Informationen für Tabellentypen, die Tabellenwert Parametern zugeordnet sind, bereitzustellen. In Übereinstimmung mit der ODBC-Spezifikation werden SS_TYPE_CATALOG_NAME und SS_TYPE_SCHEMA_NAME allen treiberspezifischen Spalten vorangestellt, die in früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hinzugefügt wurden, sowie allen Spalten nachgestellt, die von ODBC selbst benötigt werden.  
  
 In der folgenden Tabelle sind die Spalten aufgeführt, die für Tabellenwertparameter signifikant sind.  
  
|Spaltenname|Datentyp|Wert/Kommentare|  
|-----------------|---------------|---------------------|  
|DATA_TYPE|Smallint nicht NULL|SQL_SS_TABLE|  
|TYPE_NAME|WVarchar(128) nicht NULL|Der Typname des Tabellenwertparameters.|  
|COLUMN_SIZE|Integer|NULL|  
|BUFFER_LENGTH|Integer|0|  
|DECIMAL_DIGITS|Smallint|NULL|  
|NUM_PREC_RADIX|Smallint|NULL|  
|NULLABLE|Smallint nicht NULL|SQL_NULLABLE|  
|ANMERKUNGEN|Varchar|NULL|  
|COLUMN_DEF|WVarchar(4000)|NULL|  
|SQL_DATA_TYPE|Smallint nicht NULL|SQL_SS_TABLE|  
|SQL_DATETIME_SUB|Smallint|NULL|  
|CHAR_OCTET_LENGTH|Integer|NULL|  
|ORDINAL_POSITION|Integer nicht NULL|Die Ordnungsposition des Parameters.|  
|IS_NULLABLE|Varchar|"YES"|  
|SS_TYPE_CATALOG_NAME|WVarchar(128) nicht NULL|Der Katalog, der die Typdefinition für den Tabellentyp des Tabellenwertparameters enthält.|  
|SS_TYPE_SCHEMA_NAME|WVarchar(128) nicht NULL|Das Schema, das die Typdefinition für den Tabellentyp des Tabellenwertparameters enthält.|  
  
 Die WVarchar-Spalten werden in der ODBC-Spezifikation als Varchar definiert, werden aber tatsächlich in allen neueren Versionen des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-ODBC-Treibers als WVarchar zurückgegeben. Diese Änderung wurde vorgenommen, als der ODBC 3.5-Spezifikation die Unicode-Unterstützung hinzugefügt wurde. Sie wurde jedoch nicht explizit ausgeschrieben.  
  
 Zum Abrufen zusätzlicher Metadaten für Tabellenwert Parameter verwendet eine Anwendung die Katalog Funktionen SQLColumns und SQLPrimaryKeys. Vor dem Aufruf dieser Funktionen für Tabellenwertparameter muss die Anwendung das Anweisungsattribut SQL_SOPT_SS_NAME_SCOPE auf SQL_SS_NAME_SCOPE_TABLE_TYPE festlegen. Dieser Wert gibt an, dass die Anwendung Metadaten für einen Tabellentyp anstatt einer tatsächlichen Tabelle erfordert. Die Anwendung übergibt dann den TYPE_NAME des Tabellenwert Parameters als *TableName* -Parameter. SS_TYPE_CATALOG_NAME und SS_TYPE_SCHEMA_NAME werden mit *den Parametern* *CatalogName* bzw. Schema Name verwendet, um den Katalog und das Schema für den Tabellenwert Parameter zu identifizieren. Wenn eine Anwendung das Abrufen von Metadaten für Tabellenwertparameter abgeschlossen hat, muss sie SQL_SOPT_SS_NAME_SCOPE wieder auf den Standardwert SQL_SS_NAME_SCOPE_TABLE festlegen.  
  
 Wenn SQL_SOPT_SS_NAME_SCOPE auf SQL_SS_NAME_SCOPE_TABLE festgelegt ist, schlagen Abfragen von Verbindungsservern fehl. Aufrufe von SQLColumns oder SQLPrimaryKeys mit einem Katalog, der eine Serverkomponente enthält, schlagen fehl.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Tabellenwert Parameter &#40;ODBC-&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
  
