---
description: Verwenden von OPENJSON mit einem expliziten Schema (SQL Server)
title: Verwenden von OPENJSON mit einem expliziten Schema
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- OPENJSON, with explicit schema
ms.assetid: 9c1c3bfb-e1ad-4659-b94f-722b0848d5a2
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e3bc02f2e3274a0d1653f808b58336648522a700
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478081"
---
# <a name="use-openjson-with-an-explicit-schema-sql-server"></a>Verwenden von OPENJSON mit einem expliziten Schema (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

  Verwenden Sie **OPENJSON** mit einem expliziten Schema, um eine Tabelle zurückzugeben, die wie von Ihnen in der WITH-Klausel angegeben formatiert ist.  
  
 Hier einige Beispiele, in denen **OPENJSON** mit einem expliziten Schema verwendet wird. Weitere Informationen und Beispiele finden Sie unter [OPENJSON &#40;Transact-SQL&#41;](../../t-sql/functions/openjson-transact-sql.md).  
  
## <a name="example---use-the-with-clause-to-format-the-output"></a>Beispiel: Verwenden Sie die WITH-Klausel, um die Ausgabe zu formatieren.  
 Die folgende Abfrage gibt die in der folgenden Tabelle angezeigten Ergebnisse zurück. Beachten Sie, wie die JSON-Klausel dafür sorgt, dass Werte als JSON-Objekte und nicht als Skalarwerten in col5 und Array_element zurückgegeben werden.  
  
```sql  
DECLARE @json NVARCHAR(MAX) =
N'{"someObject":   
    {"someArray":  
      [  
          {"k1": 11, "k2": null, "k3": "text"},  
          {"k1": 21, "k2": "text2", "k4": { "data": "text4" }},  
          {"k1": 31, "k2": 32},  
          {"k1": 41, "k2": null, "k4": { "data": false }}     
       ]  
    }  
 }'  
   
SELECT * FROM  
 OPENJSON(@json, N'lax $.someObject.someArray')  
WITH ( k1 int,   
        k2 varchar(100),  
        col3 varchar(6) N'$.k3',  
        col4 varchar(10) N'lax $.k4.data',  
        col5 nvarchar(MAX) N'lax $.k4' AS JSON, 
        array_element nvarchar(MAX) N'$' AS JSON  
 )  
```  
  
 **Ergebnisse**  
  
|k1|k2|col3|col4|col5|array_element|  
|--------|--------|----------|----------|----------|--------------------|  
|11|*NULL*|"text"|*NULL*|*NULL*|{"k1": 11, "k2": null, "k3": "text"}|  
|21|"text2"|*NULL*|"text4"|{ "data": "text4" }|{"k1": true, "k2": "text2", "k4": { "data": "text4" } }|  
|31|"32"|*NULL*|*NULL*|*NULL*|{"k1": 31, "k2": 32 }|  
|41|*NULL*|*NULL*|false|{ "data": false }|{"k1": 41, "k2": null,       "k4": { "data": false }    }|  
  
## <a name="example---load-json-into-a-ssnoversion-table"></a>Beispiel: Laden von JSON in eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Tabelle.  
 Im folgenden Beispiel wird ein komplettes JSON-Objekt in eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Tabelle geladen.  
  
```sql  
DECLARE @json NVARCHAR(MAX) = '{  
  "id" : 2,  
  "firstName": "John",  
  "lastName": "Smith",  
  "isAlive": true,  
  "age": 25,  
  "dateOfBirth": "2015-03-25T12:00:00",  
  "spouse": null  
  }';  
   
  INSERT INTO Person  
  SELECT *   
  FROM OPENJSON(@json)  
  WITH (id int,  
        firstName nvarchar(50), lastName nvarchar(50),   
        isAlive bit, age int,  
        dateOfBirth datetime2, spouse nvarchar(50))  
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Weitere Informationen zu JSON in SQL Server und Azure SQL-Datenbank  
  
### <a name="microsoft-videos"></a>Microsoft-Videos

Eine visuelle Einführung in die JSON-Unterstützung, die in SQL Server und Azure SQL-Datenbank integriert ist, finden Sie in den folgenden Videos:

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a>Weitere Informationen  
 [OPENJSON &#40;Transact-SQL&#41;](../../t-sql/functions/openjson-transact-sql.md)  
  
  
