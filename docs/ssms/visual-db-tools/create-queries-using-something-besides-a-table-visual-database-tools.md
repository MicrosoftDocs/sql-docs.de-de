---
description: Erstellen von Abfragen mit anderen Quellen als einer Tabelle (Visual Database Tools)
title: Erstellen von Abfragen mit anderen Quellen als einer Tabelle
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- user-defined functions [SQL Server], queries
- queries [SQL Server], creating
ms.assetid: 8e4a1f0a-8a42-4733-be8d-e21d6dbddb33
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 95a432ca8912b4d306538906d11bb2a3d5464e5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350454"
---
# <a name="create-queries-using-something-besides-a-table-visual-database-tools"></a>Erstellen von Abfragen mit anderen Quellen als einer Tabelle (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Wenn Sie eine Abrufabfrage erstellen, geben Sie die aufzunehmenden Spalten und Zeilen sowie den Ort an, von dem der Abfrageprozessor die Ausgangsdaten abrufen kann. Üblicherweise handelt es sich bei diesen Ausgangsdaten um eine Tabelle oder mehrere verknüpfte Tabellen. Die zugrunde liegenden Daten können jedoch auch aus anderen Quellen stammen. Dies können Sichten, Abfragen, Synonyme oder benutzerdefinierte Funktionen sein, die eine Tabelle zurückgeben.  
  
## <a name="using-a-view-in-place-of-a-table"></a>Verwenden einer Sicht anstelle einer Tabelle  
Sie können Zeilen aus einer Sicht auswählen. Nehmen Sie z. B. an, dass die Datenbank eine Sicht mit der Bezeichnung "ExpensiveBooks" enthält, in der jede Zeile einen Titel mit einem Preis über 19,99 beschreibt. Die Definition der Sicht kann folgendermaßen lauten:  
  
```  
SELECT *  
FROM titles  
WHERE price > 19.99  
```  
  
Sie können alle teuren Titel zum Thema Psychologie auswählen, indem Sie einfach alle Psychologiebücher aus der Sicht ExpensiveBooks abrufen. Hierfür kann folgende SQL-Anweisung formuliert werden:  
  
```  
SELECT *  
FROM ExpensiveBooks  
WHERE type = 'psychology'  
```  
  
In ähnlicher Weise kann eine Sicht in ein JOIN-Vorgang eingebunden werden. Sie können z. B. die Verkäufe an teuren Büchern ermitteln, indem Sie die Tabelle der Verkäufe mit der Sicht ExpensiveBooks verknüpfen. Hierfür kann folgende SQL-Anweisung formuliert werden:  
  
```  
SELECT *  
FROM sales   
         INNER JOIN   
         ExpensiveBooks   
         ON sales.title_id   
         =  ExpensiveBooks.title_id  
```  
  
Weitere Informationen zum Hinzufügen einer Sicht zu einer Abfrage finden Sie unter [Hinzufügen von Tabellen zu Abfragen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md).  
  
## <a name="using-a-query-in-place-of-a-table"></a>Verwenden einer Abfrage anstelle einer Tabelle  
Sie können Zeilen aus einer Abfrage auswählen. Nehmen Sie z.B. an, dass Sie bereits eine Abfrage erstellt haben, die die Titel und IDs für Bücher zurückgibt, für die ein Mitautor angegeben ist, also alle Bücher mit mehreren Autoren. Die SQL-Anweisung könnte folgendermaßen aussehen:  
  
```  
SELECT   
     titles.title_id, title, type  
FROM   
     titleauthor   
         INNER JOIN  
         titles   
         ON titleauthor.title_id   
         =  titles.title_id   
GROUP BY   
     titles.title_id, title, type  
HAVING COUNT(*) > 1  
```  
  
Sie können nun eine weitere Abfrage erstellen, die auf diesem Ergebnis aufbaut. Dies kann z. B. eine Abfrage sein, die alle Psychologiebücher ermittelt, die von mehreren Autoren geschrieben wurden. In dieser Abfrage können Sie die bereits vorhandene Abfrage als Quelle für die neuen Abfragedaten verwenden. Hierfür kann folgende SQL-Anweisung formuliert werden:  
  
```  
SELECT   
    title  
FROM   
    (  
    SELECT   
        titles.title_id,   
        title,   
        type  
    FROM   
        titleauthor   
            INNER JOIN  
            titles   
            ON titleauthor.title_id   
            =  titles.title_id   
    GROUP BY   
        titles.title_id,   
        title,   
        type  
    HAVING COUNT(*) > 1  
    )   
    co_authored_books  
WHERE     type = 'psychology'  
```  
  
Der hervorgehobene Text zeigt die vorhandene Abfrage, die die Ausgangsdaten für die neue Abfrage liefert. Beachten Sie, dass in der neuen Abfrage ein Alias (co_authored_books) für die vorhandene Abfrage verwendet wird. Weitere Informationen zu Aliasen finden Sie unter [Erstellen von Tabellenaliasen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-table-aliases-visual-database-tools.md) und [Erstellen von Spaltenaliasen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-column-aliases-visual-database-tools.md).  
  
In ähnlicher Weise kann eine Abfrage in ein JOIN-Vorgang eingebunden werden. Sie können z. B. die Verkäufe an teuren Büchern mit Mitautorenschaft ermitteln, indem Sie die Sicht ExpensiveBooks mit der Abfrage verknüpfen, die die Bücher mit mehreren Autoren zurückgibt. Hierfür kann folgende SQL-Anweisung formuliert werden:  
  
```  
SELECT   
    ExpensiveBooks.title  
FROM   
    ExpensiveBooks   
        INNER JOIN  
        (  
        SELECT   
            titles.title_id,   
            title,   
            type  
        FROM   
            titleauthor   
                INNER JOIN  
                titles   
                ON titleauthor.title_id   
                =  titles.title_id   
        GROUP BY   
            titles.title_id,   
            title,   
            type  
        HAVING COUNT(*) > 1  
        )  
```  
  
Weitere Informationen zum Hinzufügen einer Abfrage zu einer Abfrage finden Sie unter [Hinzufügen von Tabellen zu Abfragen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md).  
  
## <a name="using-a-user-defined-function-in-place-of-a-table"></a>Verwenden einer benutzerdefinierten Funktion anstelle einer Tabelle  
In SQL Server 2000 oder höher können Sie eine benutzerdefinierte Funktion erstellen, die eine Tabelle zurückgibt. Solche Funktionen eignen sich zum Ausführen komplexer oder prozeduraler Logiken.  
  
Angenommen die Tabelle der Mitarbeiter enthält eine zusätzliche Spalte employee.manager_emp_id und eine Fremdschlüsselbeziehung liegt zwischen manager_emp_id und employee.emp_id vor. Innerhalb jeder Zeile der Tabelle der Mitarbeiter gibt die Spalte manager_emp_id den Vorgesetzten des Mitarbeiters an. Genauer gesagt wird die emp_id des Vorgesetzten des Mitarbeiters angezeigt. Sie können eine benutzerdefinierte Funktion erstellen, die eine Tabelle mit jeweils einer Zeile für jeden Mitarbeiter zurückgibt, der innerhalb der Organisationshierarchie unter einem Manager mit der angegebenen Tätigkeitsstufe arbeitet. Die Funktion trägt die Bezeichnung „fn_GetWholeTeam“ und ist so formuliert, dass eine Eingabevariable übergeben werden kann: die emp_id des Managers, dessen Team Sie abrufen möchten.  
  
Sie können eine Abfrage erstellen, die die Funktion fn_GetWholeTeam als Datenquelle verwendet. Hierfür kann folgende SQL-Anweisung formuliert werden:  
  
```  
SELECT *   
FROM   
     fn_GetWholeTeam ('VPA30890F')  
```  
  
"VPA30890F" ist die emp_id des Managers, dessen Organisation abgerufen wird. Weitere Informationen zum Hinzufügen einer Abfrage zu einer benutzerdefinierten Funktion finden Sie unter [Hinzufügen von Tabellen zu Abfragen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md). Eine vollständige Beschreibung der benutzerdefinierten Funktionen finden Sie unter [Benutzerdefinierte Funktionen](../../relational-databases/user-defined-functions/user-defined-functions.md).  
