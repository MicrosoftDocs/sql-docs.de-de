---
title: Einfügen von Transact-SQL-Umschließungsausschnitten
description: Informieren Sie sich, wie Sie einen Umschließungsausschnitt einfügen, der als Startpunkt für das Platzieren von Anweisungen in Codeblöcken fungiert.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- snippets [Transact-SQL], surround with
- IntelliSense [SQL Server], surround with snippets
- Transact-SQL snippets, surround with
ms.assetid: 5b5a8c6c-968e-4361-a7f5-9e2ac186d927
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a62f995d8f75eef26c6e27afdfac06f365251796
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354637"
---
# <a name="insert-surround-with-transact-sql-snippets"></a>Einfügen von Transact-SQL-Umschließungsausschnitten
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Als Umschließungsausschnitt wird eine Vorlage bezeichnet, die Sie als Ausgangspunkt beim Einschließen von [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen in einem BEGIN-, IF- oder WHILE-Block verwenden können.  
  
## <a name="inserting-surround-with-snippets"></a>Einfügen von Umschließungsausschnitten  
 Umschließungsausschnitte können mit einer von drei Methoden gestartet werden: mit einer Tastenkombination, über das Menü **Bearbeiten** und über das Kontextmenü.  
  
 Wenn Sie den Ausschnitt eingefügt haben, müssen Sie den Ersetzungstext so ändern, dass eine gültige [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung entsteht. Weitere Informationen finden Sie unter [Abschließen von Transact-SQL-Ausschnitten](./complete-transact-sql-snippets.md).  
  
#### <a name="to-insert-a-surround-with-snippet"></a>So fügen Sie einen Umschließungsausschnitt ein  
  
1.  Wählen Sie im [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor-Fenster die im Block einzuschließenden Anweisungen aus.  
  
2.  Die Liste der Umschließungsausschnitte können Sie mit einer der folgenden drei Methoden anzeigen:  
  
    -   Drücken Sie STRG+K, STRG+S.  
  
    -   Zeigen Sie im Menü **Bearbeiten** auf **IntelliSense**, und wählen Sie dann den Befehl des **Umschließen mit** aus.  
  
    -   Klicken Sie mit der rechten Maustaste auf den markierten Text, und wählen Sie dann im Kontextmenü den Befehl **Umschließen mit** aus.  
  
3.  Wählen Sie in der Liste den Namen des Ausschnitts (BEGIN, IF oder WHILE) aus, indem Sie die Maus verwenden oder den Namen des Ausschnitts eingeben und TAB oder die EINGABETASTE drücken.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Einfügen von Transact-SQL-Ausschnitten](./insert-transact-sql-snippets.md)  
  
