---
description: MSSQLSERVER_41368
title: MSSQLSERVER_41368 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 41368 (Database Engine error)
ms.assetid: abc71559-4c4d-4cce-a08f-3299dd167842
author: MashaMSFT
ms.author: mathoma
robots: noindex,nofollow
ms.openlocfilehash: 97fec0c4cb8f24fe7e11b0399fa60289d68b7b07
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179494"
---
# <a name="mssqlserver_41368"></a>MSSQLSERVER_41368
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Ereignis-ID|41368|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|SQL_IMPLICIT_AND_EXPLICIT_TX_NOT_SUPPORTED|  
|Meldungstext|Der Zugriff auf speicheroptimierte Tabellen mit der READ COMMITTED-Isolationsstufe wird nur für Autocommittransaktionen unterstützt. Er wird nicht für explizite oder implizite Transaktionen unterstützt. Geben Sie eine unterstützte Isolationsstufe für die speicheroptimierte Tabelle mithilfe eines Tabellentipps wie WITH (SNAPSHOT) an.|  
  
## <a name="explanation"></a>Erklärung  
Der Zugriff auf speicheroptimierte Tabellen mit der READ COMMITTED-Isolationsstufe wird nur für Autocommittransaktionen unterstützt. Weitere Informationen finden Sie unter [Transaktionen mit In-Memory-Tabellen und Prozeduren](~/relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md).  
  
Wenn Sie unter Verwendung einer expliziten Transaktion, die mit BEGIN TRANSACTION gestartet wurde, oder einer impliziten Transaktion auf eine speicheroptimierte Tabelle zugreifen, während IMPLICIT_TRANSACTIONS auf ON festgelegt ist, wird die READ COMMITTED-Isolationsstufe nicht unterstützt.  
  
## <a name="user-action"></a>Benutzeraktion  
Verwenden Sie für den Tabellenzugriff die SNAPSHOT-Isolationsstufe, wenn Sie von einer expliziten oder impliziten READ COMMITTED-Transaktion auf eine speicheroptimierte Tabelle zugreifen. Dazu können Sie den Tabellenhinweis WITH (SNAPSHOT) verwenden (weitere Informationen finden Sie unter [Transaktionen mit In-Memory-Tabellen und Prozeduren](~/relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)) oder die Datenbankoption MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT auf ON festlegen (weitere Informationen finden Sie unter [ALTER DATABASE SET-Optionen &#40;Transact-SQL&#41;](~/t-sql/statements/alter-database-transact-sql.md)).  
  
## <a name="see-also"></a>Weitere Informationen  
[In-Memory-OLTP &#40;Arbeitsspeicheroptimierung&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
