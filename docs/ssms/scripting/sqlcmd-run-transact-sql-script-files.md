---
title: Ausführen von Transact-SQL-Skriptdateien mithilfe von sqlcmd
description: Informieren Sie sich, wie Sie sqlcmd zum Ausführen einer Transact-SQL-Skriptdatei verwenden. Es können Transact-SQL-Anweisungen, sqlcmd-Befehle und Skriptvariablen enthalten sein.
ms.custom: seo-lt-2019
ms.date: 07/15/2016
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- transact sql scripts
ms.assetid: 90067eb8-ca3e-44e8-bb1a-bf7d1a359423
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: edfa425e7cceba9264ae640239039105f6b366cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346667"
---
# <a name="sqlcmd---run-transact-sql-script-files"></a>Ausführen von Transact-SQL-Skriptdateien mithilfe von „sqlcmd“
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 Verwenden Sie zum Ausführen einer Transact-SQL-Skriptdatei den Befehl **sqlcmd** . Eine Transact-SQL-Skriptdatei ist eine Textdatei, die eine Kombination aus Transact-SQL-Anweisungen, **sqlcmd** -Befehlen und Skriptvariablen enthalten kann.  

## <a name="create-a-script-file"></a>Erstellen einer Skriptdatei  
 Führen Sie die folgenden Schritte aus, um eine einfache Transact-SQL-Skriptdatei mithilfe des Editors zu erstellen:  
  
1.  Klicken Sie auf **Start**, zeigen Sie auf **Alle Programme**, zeigen Sie auf **Zubehör**, und klicken Sie dann auf **Editor**.  
  
2.  Kopieren Sie den folgenden Transact-SQL-Code, und fügen Sie ihn im Editor ein:  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT p.FirstName + ' ' + p.LastName AS 'Employee Name',  
    a.AddressLine1, a.AddressLine2 , a.City, a.PostalCode   
    FROM Person.Person AS p   
       INNER JOIN HumanResources.Employee AS e   
            ON p.BusinessEntityID = e.BusinessEntityID  
        INNER JOIN Person.BusinessEntityAddress bea   
            ON bea.BusinessEntityID = e.BusinessEntityID  
        INNER JOIN Person.Address AS a   
            ON a.AddressID = bea.AddressID;  
    GO  
    ```  
  
3.  Speichern Sie die Datei auf dem Laufwerk C unter dem Namen **myScript.sql** .  
  
## <a name="run-the-script-file"></a>Ausführen der Skriptdatei  
  
1.  Öffnen Sie ein Eingabeaufforderungsfenster.  
  
2.  Geben Sie im Eingabeaufforderungsfenster Folgendes ein: **sqlcmd -S myServer\instanceName -i C:\myScript.sql**  
  
3.  Drücken Sie die EINGABETASTE.  
  
 Eine Liste mit Namen und Adressen von Mitarbeitern aus [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] wird in das Eingabeaufforderungsfenster geschrieben.  

## <a name="save-the-output-to-a-text-file"></a>Speichern der Ausgabe in einer Textdatei
  
1.  Öffnen Sie ein Eingabeaufforderungsfenster.  
  
2.  Geben Sie im Eingabeaufforderungsfenster Folgendes ein: **sqlcmd -S myServer\instanceName -i C:\myScript.sql -o C:\EmpAdds.txt**  
  
3.  Drücken Sie die EINGABETASTE.  
  
 In diesem Fall erfolgt keine Ausgabe im Eingabeaufforderungsfenster. Stattdessen erfolgt die Ausgabe in die Datei EmpAdds.txt. Sie können diese Ausgabe prüfen, indem Sie die Datei EmpAdds.txt öffnen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Starten des Hilfsprogramms „sqlcmd“](./sqlcmd-start-the-utility.md)   
 [SQLCMD-Hilfsprogramm](../../tools/sqlcmd-utility.md)  
  
