---
description: Verwerfen von Änderungen an Abfragen (Visual Database Tools)
title: Verwerfen von Änderungen an Abfragen
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- reverting queries
- queries [SQL Server], discarding changes
- discarding query changes
ms.assetid: 7bb17ece-1222-4622-b476-5789d7641c64
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 6df4797bab17deec42e0d5657633f0d48304d80a
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88480022"
---
# <a name="discard-changes-made-to-queries-visual-database-tools"></a>Verwerfen von Änderungen an Abfragen (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Sie können an einer Abfragedefinition vorgenommene Änderungen vor dem Speichern verwerfen. Nach dem Speichern kann ein früherer Zustand nicht wiederhergestellt werden.  
  
> [!NOTE]  
> Um Änderungen an Werten im Ergebnisbereich rückgängig zu machen, drücken Sie die ESC-TASTE, bevor Sie zu einem anderen Datensatz wechseln. Wenn Sie zu einem anderen Datensatz wechseln und eine Benachrichtigung erhalten, dass die Änderungen nicht an die Datenbank übergeben werden, haben Sie nochmals die Gelegenheit, durch Drücken der ESC-TASTE den früheren Wert wiederherzustellen.  
  
### <a name="to-discard-changes-made-to-a-query-definition"></a>So verwerfen Sie die an einer Abfragedefinition vorgenommenen Änderungen  
  
1.  Klicken Sie, während die Abfrage im Abfrage- und Sicht-Designer geöffnet ist, im Menü **Datei** auf **Schließen**.  
  
2.  Klicken Sie im Dialogfeld **Microsoft SQL Server Management Studio** auf **Nein**.  
  
    Die Abfragedefinition wird in den Zustand beim letzten Speichern zurückgeführt.  
  
## <a name="see-also"></a>Weitere Informationen  
[Speichern von Abfragen](../../ssms/visual-db-tools/save-queries-visual-database-tools.md)  
[Artikel zum Entwerfen von Abfragen und Sichten](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
[Ausführen grundlegender Vorgänge mit Abfragen](../../ssms/visual-db-tools/perform-basic-operations-with-queries-visual-database-tools.md)  
[Verwenden von Daten im Ergebnisbereich](../../ssms/visual-db-tools/work-with-data-in-the-results-pane-visual-database-tools.md)  
  
