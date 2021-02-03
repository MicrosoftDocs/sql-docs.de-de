---
description: '&#x40;&#x40;Version: Transact SQL-Konfigurationsfunktionen'
title: '@@VERSION (Transact-SQL) | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 03/20/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@VERSION'
- '@@VERSION_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@VERSION function'
- current SQL Server installation information
- versions [SQL Server], @@VERSION
- processors [SQL Server], types
ms.assetid: 385ba80e-7c28-41a5-9cdb-5648f3785983
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 39bf4fb339f27cd62929e5bc216e2bc27ad03def
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181806"
---
# <a name="x40x40version---transact-sql-configuration-functions"></a>&#x40;&#x40;Version: Transact SQL-Konfigurationsfunktionen
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt System- und Buildinformationen für die aktuelle Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
@@VERSION  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 **nvarchar**  
  
## <a name="remarks"></a>Bemerkungen  
 Die @@VERSION-Ergebnisse werden als eine nvarchar-Zeichenfolge dargestellt. Sie können die [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)-Funktion verwenden, um die einzelnen Eigenschaftenwerte abzurufen.  
  
 Für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden die folgenden Informationen zurückgegeben.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Version  
  
-   Architektur des Prozessors  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Erstellungsdatum  
  
-   Urheberrechtserklärung  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Edition  
  
-   Betriebssystemversion  
  
 Für [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] werden die folgenden Informationen zurückgegeben.  
  
-   Edition – Microsoft SQL Azure  
  
-   Produktebene – RTM  
  
-   Produktversion  
  
-   Erstellungsdatum  
  
-   Urheberrechtserklärung  

> [!NOTE]  
> Derzeit ist ein Problem bekannt, durch das @@VERSION die falsche Produktversion für Azure SQL-Datenbank meldet. Die von Azure SQL-Datenbank ausgeführte Version der SQL Server-Datenbank-Engine ist im Gegensatz zur lokalen Version von SQL Server immer aktuell und enthält die neuesten Sicherheitspatches. Die Patchstufe entspricht also der lokalen Version von SQL Server oder ist aktueller als diese. Außerdem sind die neuesten SQL Server-Features in Azure SQL-Datenbank verfügbar.
>
> Um die Edition der Engine programmgesteuert zu ermitteln, können Sie die Anweisung SELECT SERVERPROPERTY('EngineEdition') verwenden. Diese Abfrage gibt „5“ für Azure SQL-Datenbank und „8“ für Azure SQL Managed Instance zurück.
>
> Sobald dieses Problem behoben ist, wird die Dokumentation aktualisiert.

  
## <a name="examples"></a>Beispiele  
  
### <a name="a-return-the-current-version-of-ssnoversion"></a>A: Rückgabe der aktuellen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
 Im folgenden Beispiel werden die Versionsinformationen für die aktuelle Installation zurückgegeben.  
  
```sql
SELECT @@VERSION AS 'SQL Server Version';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-return-the-current-version-of-ssdw"></a>B. Rückgabe der aktuellen Version von [!INCLUDE[ssDW](../../includes/ssdw-md.md)].  
  
```sql
SELECT @@VERSION AS 'SQL Server PDW Version';  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)  
  
  

