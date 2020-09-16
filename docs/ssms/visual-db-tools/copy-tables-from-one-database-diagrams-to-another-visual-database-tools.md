---
description: Kopieren von Tabellen von einem Datenbankdiagramm in ein anderes Datenbankdiagramm (Visual Database Tools)
title: Kopieren von Tabellen von einem Datenbankdiagramm in ein anderes
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- copying tables
- duplicating tables
ms.assetid: 155a4f09-9321-4887-a7d4-aa2ce6b51277
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: b75d0d2c3429f5649e1034ea9eb4f922ca995652
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88468474"
---
# <a name="copy-tables-from-one-database-diagrams-to-another-visual-database-tools"></a>Kopieren von Tabellen von einem Datenbankdiagramm in ein anderes Datenbankdiagramm (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Sie können eine Tabelle innerhalb derselben Datenbank von einem Datenbankdiagramm in ein anderes kopieren.  
  
Durch das Kopieren einer Tabelle von einem Datenbankdiagramm in ein anderes Diagramm wird letzterem ein Verweis auf die Tabelle hinzugefügt. Die Tabelle wird jedoch nicht in die Datenbank dupliziert. Wenn Sie beispielsweise die Tabelle `authors` von einem Datenbankdiagramm in ein anderes kopieren, verweisen beide Diagramme auf dieselbe Tabelle `authors` in der Datenbank.  
  
### <a name="to-copy-a-table-from-another-database-diagram"></a>So kopieren Sie eine Tabelle von einem anderen Datenbankdiagramm  
  
1.  Vergewissern Sie sich, dass Sie mit der Datenbank verbunden sind, deren Tabelle Sie kopieren möchten.  
  
2.  Öffnen Sie im Ausgangsdiagramm sowohl das Ausgangs- als auch das Zieldiagramm, und markieren Sie die Tabelle, die in das Zieldiagramm kopiert werden soll.  
  
3.  Klicken Sie auf der Symbolleiste auf die Schaltfläche **Kopieren** . Dadurch wird die ausgewählte Tabellendefinition in die Zwischenablage kopiert.  
  
4.  Wechseln Sie zum Zieldiagramm. Dieses Diagramm muss sich in derselben Datenbank befinden wie das Ausgangsdiagramm.  
  
5.  Klicken Sie auf der Symbolleiste auf die Schaltfläche **Einfügen** . Der Inhalt der Zwischenablage wird nun an der neuen Stelle angezeigt und bleibt hervorgehoben, bis Sie auf eine andere Stelle klicken. Wenn Beziehungen zwischen den ausgewählten Tabellen und anderen Tabellen im Zieldiagramm bestehen, werden automatisch Beziehungslinien gezogen.  
  
Wenn Sie die Tabelle in einem der beiden Diagramme bearbeiten, wirken sich die Änderungen auf beide Diagramme aus. Ähnliches gilt für das Speichern der Tabelle. Wenn Sie die Tabelle in einem der beiden Diagramme speichern, gilt sie in beiden Diagrammen nicht mehr als "verändert".  
  
## <a name="see-also"></a>Weitere Informationen  
[Verwenden von Datenbankdiagrammen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-database-diagrams-visual-database-tools.md)  
[Hinzufügen von Tabellen zu Diagrammen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/add-tables-to-diagrams-visual-database-tools.md)  
  
