---
title: Verwalten von Tabellen und Beziehungen mithilfe des Tabellen-Designers
description: In diesem Artikel erhalten Sie einen Überblick über den Tabellen-Designer. Sie erfahren, wie Sie dieses Tool verwenden, um Datenbanktabellenstrukturen zu erstellen und zu bearbeiten und Beziehungen zwischen Datenbankobjekten anzuzeigen.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.design.table.columnspecification.index.dialog
- sql.data.tools.design.table.columnsgrid.view
- sql.data.tools.design.table.scriptpanel
ms.assetid: 322a2903-d7a6-4f52-9048-1bd413b4c799
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: a71009351c187bdda50c00b4f3b00bfee0fd2acf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018000"
---
# <a name="how-to-use-the-table-designer-to-manage-tables-and-relationships"></a>Gewusst wie: Verwalten von Tabellen und Beziehungen mithilfe des Tabellen-Designers

Der Tabellen-Designer bietet gemeinsam mit dem Transact\-SQL-Editor eine visuelle Oberfläche zum Erstellen und Bearbeiten von Tabellenstrukturen, einschließlich tabellenspezifischer Programmierobjekte, für SQL Server-Datenbanken.  Er wird gestartet, wenn Sie eine neue Tabelle für eine verbundene Datenbank oder ein Projekt erstellen oder wenn Sie im SQL Server-Objekt-Explorer oder Projektmappen-Explorer auf eine Tabelle doppelklicken, um sie zu bearbeiten.  
  
Der Designer besteht aus dem Spaltenraster, dem Skriptbereich und dem Kontextbereich. Im Spaltenraster werden alle Spalten in der Tabelle aufgelistet. In diesem Raster können Sie Spalten hinzufügen, bearbeiten und löschen.  Im Kontextbereich erhalten Sie eine logische Ansicht der Tabellendefinition (Schlüssel, Indizes, Einschränkungen, Trigger usw.). Hier können Sie ein Objekt auswählen, um dessen Beziehungen zu einzelnen Spalten hervorzuheben. In diesem Bereich können Sie der Tabelle außerdem neue Objekte hinzufügen und die Eigenschaften eines ausgewählten Objekts im Eigenschaftenraster bearbeiten. Im Skriptbereich wird die Definition der Tabellenstruktur angezeigt, und das Skript des ausgewählten Objekts wird im Kontextbereich oder Spaltenraster hervorgehoben. Sie können das Skript bearbeiten, während daneben das Spaltenraster und der Kontextbereich angezeigt werden. Alle Änderungen in einem der drei Bereiche werden sofort in den anderen beiden Bereichen übernommen.  
  
> [!WARNING]  
> Bei den folgenden Vorgehensweisen werden Entitäten verwendet, die in den Vorgehensweisen in den Abschnitten [Entwicklung verbundener Datenbanken](../ssdt/connected-database-development.md) und [Projektorientierte Entwicklung von Offlinedatenbanken](../ssdt/project-oriented-offline-database-development.md) erstellt wurden.  
  
### <a name="to-create-a-new-table"></a>So erstellen Sie eine neue Tabelle  
  
1.  Öffnen Sie das Projekt "TradeDev", an dem Sie in vorherigen Prozeduren gearbeitet haben.  
  
2.  Erweitern Sie im **Projektmappen-Explorer** den Ordner **dbo**, klicken Sie mit der rechten Maustaste auf den Ordner **Tabellen**, klicken Sie auf **Hinzufügen** und dann auf **Tabelle**.  
  
3.  Nennen Sie die neue Tabelle **Shipper**, und klicken Sie auf **Hinzufügen**.  
  
4.  Der Tabellen-Designer wird geöffnet. Fügen Sie im Spaltenraster der Tabelle eine neue Spalte mit dem Namen **ShipperName** und dem Datentyp **int** hinzu.  
  
5.  Beachten Sie, dass Sie im Fenster **Eigenschaften** auch die Eigenschaften der Spalten bearbeiten können. Klicken Sie auf die Spalte **ShipperName**, und ändern Sie im Fenster **Eigenschaften** den **DataType** der Spalte in **nvarchar** und **length** in **128**. Beachten Sie, dass der Skriptbereich und das Spaltenraster des Designers automatisch mit der Änderung aktualisiert werden, wenn Sie den Fokus aus dem Feld verschieben.  
  
### <a name="to-create-a-new-foreign-key-constraint"></a>So erstellen Sie eine neue Fremdschlüsseleinschränkung  
  
1.  Klicken Sie im Kontextbereich des Designers mit der rechten Maustaste auf den Knoten **Fremdschlüssel**, und wählen Sie **Neuen Fremdschlüssel hinzufügen** aus.  
  
2.  Beachten Sie, dass die Knotenanzahl automatisch um 1 inkrementiert wird. Drücken Sie die EINGABETASTE, um den Standardnamen der Einschränkung zu übernehmen.  
  
3.  Ersetzen Sie im Skriptbereich die Standarddefinition der Einschränkung durch die folgende Definition.  
  
    ```  
    CONSTRAINT [FK_Shipper_Products] FOREIGN KEY ([Id]) REFERENCES [dbo].[Products]([Id])  
    ```  
  
    Beachten Sie, dass das Erstellen und Bearbeiten von Datenbankentitäten für ein Offlineprojekt auf die gleiche Weise wie für eine verbundene Datenbank erfolgt.  
  
## <a name="see-also"></a>Weitere Informationen  
[Vorgehensweise: Erstellen von Datenbankobjekten mit dem Tabellen-Designer](../ssdt/how-to-create-database-objects-using-table-designer.md)  
  
