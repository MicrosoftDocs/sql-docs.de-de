---
title: Einfügen von Transact-SQL-Ausschnitten
description: Hier erfahren Sie, wie Sie einen Transact-SQL-Codeausschnitt auswählen, einfügen und ändern, der beim Schreiben neuer Transact-SQL-Anweisungen im Abfrage-Editor der Datenbank-Engine als Startpunkt dienen kann.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- IntelliSense [SQL Server], snippets
- Transact-SQL snippets, code
- snippets [Transact-SQL], how to insert
ms.assetid: d66c96f4-2e84-4d79-9bfd-3635fdd98425
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a3eca2cf0492a3676a715c94a3b195e6c509678d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271141"
---
# <a name="insert-transact-sql-snippets"></a>Einfügen von Transact-SQL-Ausschnitten
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Ein [!INCLUDE[tsql](../../includes/tsql-md.md)] -Codeausschnitt ist eine Vorlage, die Sie als Ausgangspunkt beim Schreiben von neuen [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen im [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor verwenden können.  
  
## <a name="inserting-snippets"></a>Einfügen von Ausschnitten  
 Sie können das Menü **Ausschnitt einfügen** verwenden, um eine kategorisierte Liste von Ausschnitten zu öffnen, die Sie auswählen können.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] -Ausschnitte enthalten Ersetzungspunkte. Dabei handelt es sich um Text, der die für diesen Punkt relevante Syntax vorschlägt. Beispielsweise enthält der CREATE TABLE-Ausschnitt Ersetzungspunkte für Elemente wie den Tabellennamen, die Spaltennamen und die Spaltendatentypen. Wenn Sie den Ausschnitt eingefügt haben, müssen Sie den Ersetzungstext so ändern, dass eine gültige [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung entsteht. Weitere Informationen finden Sie unter [Abschließen von Transact-SQL-Ausschnitten](./complete-transact-sql-snippets.md).  
  
#### <a name="inserting-a-snippet-by-using-the-insert-snippet-menu"></a>Einfügen eines Ausschnitts mit dem Menü "Ausschnitt einfügen"  
  
1.  Setzen Sie im [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor-Fenster den Cursor an die Position, an der Sie den [!INCLUDE[tsql](../../includes/tsql-md.md)] -Ausschnitt einfügen möchten.  
  
2.  Rufen Sie die QuickInfo für die Ausschnittauswahl mit einer von drei Methoden auf:  
  
    -   Drücken Sie STRG+K, STRG+X.  
  
    -   Zeigen Sie im Menü **Bearbeiten** auf **IntelliSense**, und klicken Sie dann auf **Ausschnitt einfügen**.  
  
    -   Klicken Sie mit der rechten Maustaste, und wählen Sie dann im Kontextmenü den Befehl **Ausschnitt einfügen** aus.  
  
3.  Doppelklicken Sie auf den Ausschnitt, oder wählen Sie in der Ausschnittauswahl den Ausschnitt aus, und drücken Sie dann TAB oder die EINGABETASTE.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Einfügen von Transact-SQL-Umschließungsausschnitten](./insert-surround-with-transact-sql-snippets.md)  
  
