---
description: Erstellen von Beziehungen zwischen Tabellen in einem Diagramm (Visual Database Tools)
title: Erstellen von Beziehungen zwischen Tabellen in einem Diagramm
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- diagrams [SQL Server], designing
ms.assetid: 28e9630c-dff4-46cc-bb0e-fe77998b6ac2
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 5dd6604f4fb82a502402d74cb15c0a0744f82e0e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350466"
---
# <a name="create-relationships-between-tables-on-a-diagram-visual-database-tools"></a>Erstellen von Beziehungen zwischen Tabellen in einem Diagramm (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Sie können im Datenbank-Designer Beziehungen zwischen den Spalten in verschiedenen Tabellen erstellen, indem Sie Spalten zwischen Tabellen ziehen.  
  
### <a name="to-create-a-relationship-graphically"></a>So erstellen Sie eine Beziehung grafisch  
  
1.  Klicken Sie im Datenbank-Designer auf die Zeilenauswahl, um eine oder mehrere Datenbankspalten auszuwählen, für die Sie eine Beziehung mit einer Spalte in einer anderen Tabelle erstellen möchten.  
  
2.  Ziehen Sie die Spalte(n) auf die zugehörige Tabelle.  
  
3.  Es werden zwei Dialogfelder angezeigt: **Fremdschlüsselbeziehung** und **Tabellen und Spalten**, wobei letzteres im Vordergrund angezeigt wird.  
  
4.  Unter **Beziehungsname** wird ein vom System bereitgestellter Name im Format FK_ *localtable*\_*foreigntable* angegeben. Sie können diesen Wert ändern.  
  
5.  Stellen Sie sicher, dass für **Primärschlüsseltabelle** die richtige Tabelle angegeben ist.  
  
6.  Im Raster sind alle lokalen Spalten und die entsprechenden Fremdspalten aufgeführt. Sie können Spalten hinzufügen oder entfernen bzw. die Zuordnungen ändern.  
  
7.  Wählen Sie **OK** aus.  
  
    Das Dialogfeld **Fremdschlüsselbeziehung** wird angezeigt. Unter **Ausgewählte Beziehung** wird die von Ihnen erstellte Beziehung angezeigt.  
  
8.  Sie können die Eigenschaften der Beziehung im Raster ändern.  
  
9. Klicken Sie auf **OK** , um die Beziehung zu erstellen.  
  
    Im Datenbank-Designer wird eine Beziehung zwischen von Ihnen ausgewählten Spalten gezeigt.  
  
## <a name="see-also"></a>Weitere Informationen  
[Tabellen und Spalten (Dialogfeld) &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/tables-and-columns-dialog-box-visual-database-tools.md)  
[Verwenden von Einschränkungen](../../relational-databases/tables/unique-constraints-and-check-constraints.md)  
[Verwenden von Tabellen in Datenbankdiagrammen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-tables-in-database-diagram-visual-database-tools.md)  
