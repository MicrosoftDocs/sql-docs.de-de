---
title: Datenformate für Massenimport und -export
description: SQL Server akzeptiert Daten im Zeichenformat oder im nativen Binärformat. Verwenden Sie zwischen SQL Server und anderen Apps das Zeichenformat und zwischen Instanzen von SQL Server das native Format.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- data formats [SQL Server], choosing
- bulk importing [SQL Server], data formats
ms.assetid: 73fe6741-9437-4b26-b030-28b863e74399
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 3a043b45749674d09cd47bfc62eea7d7f70647ee
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474051"
---
# <a name="data-formats-for-bulk-import-or-bulk-export-sql-server"></a>Datenformate für Massenimport und -export (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann Daten in Zeichendatenformat oder systemeigenem binärem Datenformat akzeptieren. Verwenden Sie das Zeichenformat, wenn Sie Daten zwischen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und einer anderen Anwendung (z. B. [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel) bzw. einem anderen Datenbankserver (wie Oracle oder [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) verschieben. Das systemeigene Format kann nur verwendet werden, wenn Sie Daten zwischen verschiedenen Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verschieben.  
  
 **In diesem Thema:**  
  
-   [Datenformate für Massenimport oder -export](#ComponentsAndConcepts)  
  
-   [Verwandte Aufgaben](#RelatedTasks)  
  
##  <a name="data-formats-for-bulk-import-or-export"></a><a name="ComponentsAndConcepts"></a> Datenformate für Massenimport oder -export  
 In der folgenden Tabelle ist angegeben, welches Datenformat im Allgemeinen für die Verwendung geeignet ist. Die Wahl hängt von der Darstellung der Daten und von der Quelle bzw. dem Ziel des Vorgangs ab.  
  
|Vorgang|Systemeigenes Format|Systemeigenes Unicode-Format|Zeichen|Unicode-Zeichen|  
|---------------|------------|--------------------|---------------|-----------------------|  
|Massenübertragung von Daten zwischen mehreren Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe einer Datendatei, die keine Sonderzeichen oder Zeichen aus Doppelbyte-Zeichensätzen (Double-Byte Character Set, DBCS) enthält. Sofern keine Formatdatei verwendet wird, müssen diese Tabellen identisch definiert sein.|Ja*|-|-|-|  
|Für **sql_variant** -Spalten eignet sich das systemeigene Datenformat am besten, da es im Gegensatz zu Zeichen- und Unicode-Formaten die Metadaten für die einzelnen **sql_variant** -Werte beibehält.|Ja|-|-|-|  
|Massenübertragung von Daten zwischen mehreren Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe einer Datendatei, die Sonderzeichen oder DBCS-Zeichen enthält.|-|Ja|-|-|  
|Massenimport von Daten aus einer Textdatei, die von einem anderen Programm generiert wurde.|-|-|Ja|-|  
|Massenexport von Daten in eine Textdatei, die in einem anderen Programm verwendet werden soll.|-|-|Ja|-|  
|Massenübertragung von Daten zwischen mehreren Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe einer Datendatei, die Unicode-Daten, aber keine Sonderzeichen oder DBCS-Zeichen enthält.|-|-|-|Ja|  
  
 \* Die schnellste Methode für den Massenexport von Daten aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bei Verwendung von **bcp**.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Verwenden des nativen Formats zum Importieren oder Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Verwenden des Zeichenformats zum Importieren und Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Verwenden des nativen Unicode-Formats zum Importieren oder Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Verwenden des Unicode-Zeichenformats zum Importieren und Exportieren von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Importieren von Daten aus früheren SQL Server-Versionen im nativen Format oder im Zeichenformat](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Angeben von Datenformaten für die Kompatibilität bei Verwendung von bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)  
  
  
