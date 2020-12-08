---
title: Anwenden eines festen Abfrageplans auf eine Planhinweisliste | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen festen Abfrageplan auf eine Planhinweisliste des Typs OBJECT oder SQL in SQL Server anwenden können. In diesem Beispiel wird eine Planhinweisliste für eine einfache Ad-hoc-SQL-Anweisung erstellt.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: bbf401f9-af7c-48e7-8a43-bf25e8af2fd7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 550933e8c8152bd021c871d4e466f1b9fafb3907
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505438"
---
# <a name="apply-a-fixed-query-plan-to-a-plan-guide"></a>Anwenden eines festen Abfrageplans auf eine Planhinweisliste
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Sie können einen festen Abfrageplan auf eine Planhinweisliste des Typs OBJECT oder SQL anwenden. Planhinweislisten, die einen festen Abfrageplan anwenden, sind hilfreich, wenn Sie wissen, dass ein vorhandener Ausführungsplan für eine bestimmte Abfrage bessere Ergebnisse erzielt als der vom Abfrageoptimierer ausgewählte Ausführungsplan.  
  
 Im folgenden Beispiel wird eine Planhinweisliste für eine einfache Ad-hoc-SQL-Anweisung erstellt. Der gewünschte Abfrageplan für diese Anweisung wird in der Planhinweisliste durch die direkte Angabe des XML-Showplans für die Abfrage im `@hints` -Parameter bereitgestellt. Im Beispiel wird zunächst die SQL-Anweisung ausgeführt, um einen Plan im Plancache zu erzeugen. Dabei wird davon ausgegangen, dass der erzeugte Plan dem gewünschten Plan entspricht und keine weitere Optimierung der Abfrage erforderlich ist. Der XML-Showplan wird durch eine Abfrage der dynamischen Verwaltungssichten `sys.dm_exec_query_stats`, `sys.dm_exec_sql_text`und `sys.dm_exec_text_query_plan` sys.dm_exec_query_stats abgerufen und der Variablen `@xml_showplan` zugewiesen. Die `@xml_showplan` -Variable wird dann im `sp_create_plan_guide` -Parameter an die `@hints` -Anweisung übergeben. Alternativ können Sie die gespeicherte Prozedur [sp_create_plan_guide_from_handle](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md) verwenden, um eine Planhinweisliste aus einem Abfrageplan im Plancache zu erstellen.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;  
GO  
DECLARE @xml_showplan nvarchar(max);  
SET @xml_showplan = (SELECT query_plan  
    FROM sys.dm_exec_query_stats AS qs   
    CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st  
    CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, DEFAULT, DEFAULT) AS qp  
    WHERE st.text LIKE N'SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;%');  
  
EXEC sp_create_plan_guide   
    @name = N'Guide1_from_XML_showplan',   
    @stmt = N'SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = @xml_showplan;  
GO  
```  
  
  
