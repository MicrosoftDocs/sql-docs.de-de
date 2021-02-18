---
title: Abschließen von Transact-SQL-Ausschnitten
description: Ein Transact-SQL-Codeausschnitt ist eine Codevorlage. Hier erfahren Sie, wie Sie deren Verwendung anpassen, indem Sie an den Ersetzungspunkten Inhalt einfügen und Syntaxelemente zu der Vorlage hinzufügen.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- IntelliSense [SQL Server], completing snippets
- snippets [Transact-SQL], completing
- Transact-SQL snippets, completing
ms.assetid: a8316a58-bb57-485e-845f-84c23360314c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a601f7fe6192a7a5fd1395cb95c2eabf6285dfc5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354718"
---
# <a name="complete-transact-sql-snippets"></a>Abschließen von Transact-SQL-Ausschnitten
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Sobald ein [!INCLUDE[tsql](../../includes/tsql-md.md)] -Codeausschnitt in ein Skript eingefügt wurde, bearbeiten Sie den Inhalt des Ausschnitts, um eine vollständige [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung zu erstellen.  
  
## <a name="completing-snippets"></a>Vervollständigen von Ausschnitten  
 Wenn Sie dem Skript einen [!INCLUDE[tsql](../../includes/tsql-md.md)] -Ausschnitt hinzufügen, enthält die eingefügte Ausschnittanweisung einen oder mehrere Ersetzungspunkte, die hervorgehoben werden. Wenn Sie mit der Maus auf einen Ersetzungspunkt zeigen, wird eine QuickInfo mit einer Beschreibung des Syntaxelements angezeigt, das Sie angeben können. Der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor erkennt den Ausschnitt als vom umgebenden Skript separaten Code, bis Sie die Quelldatei schließen. Die Ersetzungspunkte bleiben aktiv, bis Sie die Quelldatei schließen.  
  
 Sie können auch dem durch einen Ausschnitt eingefügten Vorlagencode zusätzliche Syntaxelemente hinzufügen. Beispielsweise werden mit der Ausschnittvorlage Tabelle erstellen zwei Spaltendefinitionen hinzugefügt. Sie müssen weitere Spaltendefinitionen hinzufügen, um eine Tabelle mit mehr als zwei Spalten zu erstellen.  
  
#### <a name="completing-the-snippet-statement"></a>Vervollständigen der Ausschnittanweisung  
  
1.  Wechseln Sie mithilfe der TAB-TASTE von einem Ersetzungspunkt zum nächsten Ersetzungspunkt. Mit UMSCHALT+TAB wechseln Sie zum vorherigen Ersetzungspunkt.  
  
2.  Drücken Sie STRG+LEERTASTE, um IntelliSense aufzurufen.  
  
3.  Wählen Sie aus der Liste ein Element aus, oder geben Sie eine gewünschte Ersetzung ein.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Einfügen von Transact-SQL-Ausschnitten](./insert-transact-sql-snippets.md)   
 [Einfügen von Transact-SQL-Umschließungsausschnitten](./insert-surround-with-transact-sql-snippets.md)  
  
