---
description: Schutz von Sonderzeichen und Steuerzeichen durch FOR JSON (SQL Server)
title: Schutz von Sonderzeichen und Steuerzeichen durch FOR JSON
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON, special characters
ms.assetid: 4ba90025-5a09-4f0a-836a-54c886324530
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 68e6fd0763d65fcb457b569405d650782f2265e8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478101"
---
# <a name="how-for-json-escapes-special-characters-and-control-characters-sql-server"></a>Schutz von Sonderzeichen und Steuerzeichen durch FOR JSON (SQL Server)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

  Dieses Thema beschreibt, wie die **FOR JSON**-Klausel einer **SELECT**-Anweisung von SQL Server Sonderzeichen schützt und Steuerzeichen in der JSON-Ausgabe darstellt.  

> [!IMPORTANT]
> Diese Seite beschreibt die integrierte Unterstützung für JSON in Microsoft SQL Server. Allgemeine Informationen zum Maskieren und Codierung in JSON finden Sie in Abschnitt 2.5 der JSON-Referenz unter [https://www.ietf.org/rfc/rfc4627.txt](https://www.ietf.org/rfc/rfc4627.txt).

## <a name="escaping-of-special-characters"></a>Schutz von Sonderzeichen  
Wenn die Quelldatei Sonderzeichen enthält, umgeht die **FOR JSON**-Klausel diese in der JSON-Ausgabe mit `\`, wie in der folgenden Tabelle dargestellt. Dieser Schutz tritt in den Namen von Eigenschaften und in ihren Werte auf.  
  
|**Sonderzeichen**|**Ausgabe mit Escapezeichen**|  
|---------------------------|--------------------------|  
|Anführungszeichen (")|\\"|  
|Umgekehrter Schrägstrich (\\)|\\\\|  
|Schrägstrich (/)|\\/|  
|Rückschritt|\b|  
|Seitenvorschub|\f|  
|Zeilenwechsel|\n|  
|Wagenrücklauf|\r|  
|Horizontaler Tabulator|\t|  
  
## <a name="control-characters"></a>Steuerzeichen  
Wenn die Quelldatei Steuerzeichen enthält, kodiert die **FOR JSON**-Klausel diese in der JSON-Ausgabe im `\u<code>`-Format, wie in der folgenden Tabelle dargestellt.  
  
|**Steuerzeichen**|**Codierte Ausgabe**|  
|---------------------------|--------------------------|  
|CHAR(0)|\u0000|  
|CHAR(1)|\u0001|  
|...|...|  
|CHAR(31)|\u001f|  
  
## <a name="example"></a>Beispiel  
 Hier ist ein Beispiel für die **FOR JSON**-Ausgabe von Quelldaten, die Sonderzeichen und Steuerelement enthält.  
  
 Abfrage:  
  
```sql  
SELECT  
  'VALUE\    /  
  "' as [KEY\/"],  
  CHAR(0) as '0',  
  CHAR(1) as '1',  
  CHAR(31) as '31'  
FOR JSON PATH  
```  
  
 Ergebnis:  
  
```json  
{
    "KEY\\\t\/\"": "VALUE\\\t\/\r\n\"",
    "0": "\u0000",
    "1": "\u0001",
    "31": "\u001f"
}
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Weitere Informationen zu JSON in SQL Server und Azure SQL-Datenbank  
  
### <a name="microsoft-videos"></a>Microsoft-Videos

Eine visuelle Einführung in die JSON-Unterstützung, die in SQL Server und Azure SQL-Datenbank integriert ist, finden Sie in den folgenden Videos:

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a>Weitere Informationen  
 [Format Query Results as JSON with FOR JSON &#40;SQL Server&#41; (Formatieren von Abfrageergebnissen als JSON mit FOR JSON [SQL Server])](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md)  
[FOR-Klausel](../../t-sql/queries/select-for-clause-transact-sql.md)
