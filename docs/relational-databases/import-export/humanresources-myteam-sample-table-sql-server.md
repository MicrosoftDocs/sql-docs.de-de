---
title: HumanResources.myTeam-Beispieltabelle (SQL Server) | Microsoft-Dokumentation
description: Wenn Sie Codebeispiele für den Massenimport und -export von Daten in SQL Server ausführen möchten, erstellen Sie im Schema „HumanResources“ eine Testtabelle mit dem Namen „myTeam“.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- myTeam sample table [SQL Server]
- bulk importing [SQL Server], examples
- bulk exporting [SQL Server], examples
ms.assetid: 27da45a0-c1f4-4bf4-ab24-6196e80d3834
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: da6202e2a32fd0e553e014097cabddce686e5b0a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351361"
---
# <a name="humanresourcesmyteam-sample-table-sql-server"></a>HumanResources.myTeam-Beispieltabelle (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Viele der Codebeispiele in [Importieren und Exportieren von Massendaten](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md) erfordern eine besondere Testtabelle namens **myTeam**. Bevor Sie die Beispiele ausführen können, müssen Sie die **myTeam** -Tabelle im **HumanResources** -Schema der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Datenbank erstellen.  
  
> [!NOTE]  
>  [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] ist eine der Beispieldatenbanken in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
 Die Tabelle **myTeam** enthält die folgenden Spalten.  
  
|Column|Datentyp|NULL-Zulässigkeit|BESCHREIBUNG|  
|------------|---------------|-----------------|-----------------|  
|**EmployeeID**|**smallint**|Nicht NULL|Primärschlüssel für die Zeilen. Mitarbeiter-ID eines Mitglieds meines Teams.|  
|**Name**|**nvarchar(50)**|Nicht NULL|Name eines Mitglieds meines Teams.|  
|**Titel**|**nvarchar(50)**|Nullable|Titel des Mitarbeiters in meinem Team.|  
|**Hintergrund**|**nvarchar(50)**|Nicht NULL|Datum und Uhrzeit des letzten Updates der Zeile. (Standardwert)|  
  
**So erstellen Sie HumanResources.myTeam**  
  
-   Verwenden Sie die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen:  
  
    ```sql
    --Create HumanResources.MyTeam:   
    USE AdventureWorks;  
    GO  
    CREATE TABLE HumanResources.myTeam   
    (EmployeeID smallint NOT NULL,  
    Name nvarchar(50) NOT NULL,  
    Title nvarchar(50) NULL,  
    Background nvarchar(50) NOT NULL DEFAULT ''  
    );  
    GO  
    ```  
  
**So füllen Sie HumanResources.myTeam auf**  
  
-   Führen Sie die folgenden `INSERT` -Anweisungen zum Auffüllen der Tabelle mit zwei Zeilen aus:  
  
    ```sql
    USE AdventureWorks;  
    GO  
    INSERT INTO HumanResources.myTeam(EmployeeID,Name,Title,Background)  
       VALUES(77,'Mia Doppleganger','Administrative Assistant','Microsoft Office');  
    GO  
    INSERT INTO HumanResources.myTeam(EmployeeID,Name,Title,Background)  
       VALUES(49,'Hirum Mollicat','I.T. Specialist','Report Writing and Data Mining');  
    GO  
    ```  
  
    > [!NOTE]  
    >  Bei diesen Anweisungen wird die vierte Spalte ( `Background`) ausgelassen. Diese besitzt einen Standardwert. Das Auslassen dieser Spalte bewirkt, dass diese Spalte bei der `INSERT` -Anweisung leer bleibt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Massenimport und -export von Daten &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)  
  
  
