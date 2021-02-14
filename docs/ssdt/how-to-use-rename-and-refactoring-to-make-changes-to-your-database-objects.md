---
title: Vornehmen von Änderungen an Datenbankobjekten durch Umbenennen und Refactoring
description: Weitere Informationen über die Menüs „Umbenennen“ und „Umgestalten“ im Transact-SQL-Editor Hier erfahren Sie, wie Sie Typen umbenennen, Objektschemas wechseln, Platzhalter erweitern und Namen vollständig qualifizieren.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.dbrefactoring.previewdialog
- sql.data.tools.editor.howto.refactoring
- sql.data.tools.dbrefactoring.renamedialog
- sql.data.tools.dbrefactoring.moveschemadialog
- sql.data.tools.dbrefactoring.renameserverdatabasedialog
ms.assetid: f35520e6-8e6e-47b1-87a3-22c0cf2cabdb
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 8fd846301467dbbcbf08aa6bf4a02e52fb467c4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018020"
---
# <a name="how-to-use-rename-and-refactoring-to-make-changes-to-your-database-objects"></a>Gewusst wie: Vornehmen von Änderungen an Datenbankobjekten durch Umbenennen und Refactoring

Mit dem Kontextmenü „Umgestalten“ im Transact\-SQL-Editor können Sie ein Objekt umbenennen oder in ein anderes Schema verschieben. Außerdem können Sie eine Vorschau aller betroffenen Bereiche anzeigen, bevor Sie einen Commit für die Änderung ausführen. Mithilfe des Menüs „Umgestalten“ können Sie auch alle Verweise auf Datenbankobjekte vollständig qualifizieren, und Sie können Platzhalterzeichen in `SELECT`-Anweisungen im Datenbankprojekt erweitern.  
  
> [!NOTE]  
> Bei den folgenden Vorgehensweisen werden Entitäten verwendet, die in vorherigen Vorgehensweisen in den Abschnitten [Entwicklung verbundener Datenbanken](../ssdt/connected-database-development.md) und [Projektorientierte Entwicklung von Offlinedatenbanken](../ssdt/project-oriented-offline-database-development.md) erstellt wurden.  
  
### <a name="to-rename-a-type"></a>So benennen Sie einen Typ um  
  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Tabelle **Products** (Products.sql), und wählen Sie **Code anzeigen** aus, um das Skript im Transact\-SQL-Editor zu öffnen.  
  
2.  Klicken Sie im Skript mit der rechten Maustaste auf `[Products]`, und klicken Sie auf **Umgestalten** und **Umbenennen**.  
  
3.  Ändern Sie den Namen im Feld **Neuer Name** in **Product**. Lassen Sie die Option **Vorschau der Änderungen** aktiviert, und klicken Sie auf **OK**.  
  
4.  Der nächste Bildschirm enthält die Vorschau einer Liste von Skripts, die von diesem Umbenennungsvorgang betroffen sind. Darin sind insbesondere die Stellen hervorgehoben, die auf `Products` verweisen. Dies ähnelt der Aufgabe "Alle Verweise suchen" in der vorherigen Prozedur. Klicken Sie auf eine beliebige Stelle im oberen Bereich, und überprüfen Sie im unteren Bereich die tatsächliche Änderung in den Skripts (hervorgehoben in Grün).  
  
5.  Klicken Sie auf **Anwenden**.  
  
6.  Beachten Sie, dass bei Skriptdateien, die bereits im Tabellen-Designer oder im Transact\-SQL-Editor geöffnet sind, die Stellen, an denen Änderungen vorgenommen wurden, am linken Rand des Transact\-SQL-Editors mit einer grünen Leiste markiert sind.  
  
7.  Beachten Sie, dass **TradeDev.refactorlog** wurde im **Projektmappen-Explorer** hinzugefügt. Öffnen Sie die Datei, indem Sie darauf doppelklicken. Sie enthält eine XML-Darstellung sämtlicher Änderungen in dieser Sitzung.  
  
8.  Drücken Sie F5, um das Projekt zu erstellen und in der lokalen Datenbank bereitzustellen.  
  
9. Klicken Sie im **SQL Server-Objekt-Explorer** unter **Lokal** mit der rechten Maustaste auf die Datenbank **TradeDev**, und wählen Sie **Aktualisieren** aus.  
  
10. Erweitern Sie **Tabellen**. Die Tabelle **Products** wurde umbenannt.  
  
11. Klicken Sie mit der rechten Maustaste auf **Product**, und wählen Sie **Daten anzeigen** aus. Vergewissern Sie sich, dass vorhandene Daten ungeachtet des Umbenennungsvorgangs unverändert beibehalten wurden.  
  
### <a name="to-expand-wildcards"></a>So erweitern Sie Platzhalter  
  
1.  Erweitern Sie im **Projektmappen-Explorer** den Knoten **Funktionen**, und doppelklicken Sie auf **GetProductsBySupplier.sql**.  
  
2.  Platzieren Sie den Cursor auf das Sternchen in dieser Zeile, und klicken Sie mit der rechten Maustaste. Wählen Sie **Umgestalten** und **Platzhalter erweitern** aus.  
  
    ```  
    SELECT * from Product p  
    ```  
  
3.  Klicken Sie im Dialogfeld **Vorschau der Änderungen** auf `SELECT * from Product p` im oberen Bereich, um die Änderung zu markieren.  
  
4.  Im darunter liegenden Bereich **Vorschau der Änderungen** wurde nun `*` im Skript zu Folgendem erweitert.  
  
    ```  
    [Id], [Name], [ShelfLife], [SupplierId], [CustomerId]  
    ```  
  
5.  Klicken Sie auf die Schaltfläche **Übernehmen**.  Die Zeile, die die durch den Erweiterungsvorgang hervorgerufene Änderung enthält, ist wiederum mit einer grünen Leiste am linken Rand hervorgehoben.  
  
### <a name="to-fully-qualify-database-object-names"></a>So qualifizieren Sie Namen von Datenbankobjekten vollständig  
  
1.  Stellen Sie sicher, dass **GetProductsBySupplier.sql** noch im Transact\-SQL-Editor geöffnet ist.  
  
2.  Platzieren Sie den Cursor auf `Product` in dieser Zeile, und klicken Sie mit der rechten Maustaste. Wählen Sie **Umgestalten** und **Vollqualifizierte Namen** aus.  
  
    ```  
    SELECT [Id], [Name], [ShelfLife], [SupplierId], [CustomerId] from Product p  
    ```  
  
3.  Klicken Sie im Dialogfeld **Vorschau der Änderungen** auf die Schaltfläche **Anwenden**.  Beachten Sie, dass sämtliche Objektverweise so aktualisiert wurden, dass sie nun den Namen des Objektschemas und (falls das jeweilige Objekt über ein übergeordnetes Objekt verfügt) den Namen des übergeordneten Objekts enthalten.  
  
    ```  
    SELECT [p].[Id], [p].[Name], [p].[ShelfLife], [p].[SupplierId], [p].[CustomerId] from [dbo].[Product] p  
    ```  
  
### <a name="to-move-schema"></a>So verschieben Sie ein Schema  
  
1.  Klicken Sie mit der rechten Maustaste auf das zu verschiebende Objekt. Wählen Sie **Umgestalten** und **Schema verschieben** aus.  
  
2.  Klicken Sie in der Liste **Neues Schema** auf den Namen des Schemas, in das das Objekt verschoben werden soll. Klicken Sie auf OK.  
  
    Wenn Sie das Kontrollkästchen **Vorschau der Änderungen** aktiviert haben, wird das Dialogfeld **Vorschau der Änderungen** angezeigt. Andernfalls wird der Objektname aktualisiert und das Objekt in das neue Schema verschoben.  
  
