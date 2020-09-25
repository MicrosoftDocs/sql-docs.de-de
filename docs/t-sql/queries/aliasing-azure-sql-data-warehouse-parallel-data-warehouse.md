---
title: Aliase
description: Aliase für Azure Synapse Analytics und Parallel Data Warehouse
titleSuffix: Azure Synapse Analytics
ms.custom: seo-lt-2019
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
ms.assetid: 7b3a5c74-05cf-4385-8ee6-6176d003cb8a
author: shkale-msft
ms.author: shkale
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: be27468691c64cd23020dd9232e146e44f2a45fa
ms.sourcegitcommit: 8f062015c2a033f5a0d805ee4adabbe15e7c8f94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91227344"
---
# <a name="aliasing-azure-synapse-analytics-parallel-data-warehouse"></a>Aliase (Azure Synapse Analytics, Parallel Data Warehouse)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

Aliasing ermöglicht das temporäre Ersetzen eines Tabellen- oder Spaltennamens in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]- oder [!INCLUDE[ssPDW](../../includes/sspdw-md.md)][!INCLUDE[DWsql](../../includes/dwsql-md.md)]-Abfragen durch eine kurze und leicht zu merkenden Zeichenfolge. Tabellenaliase werden häufig in JOIN-Abfragen verwendet, da die JOIN-Syntax vollqualifizierte Objektnamen erfordert, wenn sie auf Spalten verweist.  

Aliase müssen einzelne Wörter sein, die die Objektbenennungsregeln erfüllen. Weitere Informationen finden Sie unter „Object Naming Rules“ (Objektbenennungsregeln) in [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)]. Aliase dürfen keine Leerzeichen enthalten und nicht in einfache oder doppelte Anführungszeichen gesetzt werden.  

## <a name="syntax"></a>Syntax

```tsql
object_source [ AS ] alias
```

## <a name="arguments"></a>Argumente

*object_source*  
Der Name der Quelltabelle oder -spalte.  

AS  
Eine optionale Aliaspräposition. Beim Aliasing von Bereichsvariablen ist das Schlüsselwort AS nicht zulässig.  

*alias* steht für den gewünschten temporären Verweisnamen für die Tabelle oder Spalte. Alle gültigen Objektnamen können verwendet werden. Weitere Informationen finden Sie unter „Object Naming Rules“ (Objektbenennungsregeln) in [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  

## <a name="examples-sssdw-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Das folgende Beispiel zeigt eine Abfrage mit mehreren Joins. In diesem Beispiel wird das Aliasing sowohl von Tabellen als auch von Spalten veranschaulicht.  

- Spaltenaliasing: In diesem Beispiel wird sowohl Spalten als auch Ausdrücken, die Spalten in der ausgewählten Liste haben, ein Alias zugeordnet. `SalesTerritoryRegion AS SalesTR` stellt einen einfachen Spaltenalias dar. `Sum(SalesAmountQuota) AS TotalSales` veranschaulicht  

- Tabellenaliasing: `dbo.DimSalesTerritory AS st` zeigt die Erstellung des `st`-Alias für die `dbo.DimSalesTerritory`-Tabelle.  

```tsql
-- Uses AdventureWorks

SELECT LastName, SUM(SalesAmountQuota) AS TotalSales, SalesTerritoryRegion AS SalesTR,  
    RANK() OVER (PARTITION BY SalesTerritoryRegion ORDER BY SUM(SalesAmountQuota) DESC ) AS RankResult  
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory AS st ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE SalesPersonFlag = 1 AND SalesTerritoryRegion != N'NA'  
GROUP BY LastName, SalesTerritoryRegion;  
```

Das AS-Schlüsselwort kann wie unten dargestellt ausgeschlossen werden, wird aber häufig aus Gründen der Lesbarkeit verwendet.  

```tsql
-- Uses AdventureWorks

SELECT LastName, SUM(SalesAmountQuota) TotalSales, SalesTerritoryRegion SalesTR,  
RANK() OVER (PARTITION BY SalesTerritoryRegion ORDER BY SUM(SalesAmountQuota) DESC ) RankResult  
FROM dbo.DimEmployee e  
INNER JOIN dbo.FactSalesQuota sq ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory st ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE SalesPersonFlag = 1 AND SalesTerritoryRegion != N'NA'  
GROUP BY LastName, SalesTerritoryRegion;  
```

## <a name="next-steps"></a>Nächste Schritte

- [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)
- [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
- [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)