---
title: Debuggen von gespeicherten Prozeduren
description: In diesem Artikel erfahren Sie, wie Sie den Transact-SQL-Debugger verwenden, um eine gespeicherte Prozedur interaktiv zu debuggen. Außerdem erfahren Sie, wie Sie den SQL-Aufrufstapel, lokale Variablen und Parameter anzeigen.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- SQL.DATA.TOOLS.EXECUTESTOREDPROCEDURE.DIALOG
ms.assetid: e3c8707f-0f6b-4265-8a5a-81f079330b52
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6c34db253855127451f375045e51418850bc824a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018240"
---
# <a name="how-to-debug-stored-procedures"></a>Gewusst wie: Debuggen von gespeicherten Prozeduren

Mit dem Transact\-SQL-Debugger können Sie gespeicherte Prozeduren interaktiv debuggen, indem Sie die SQL-Aufrufliste, lokale Variablen und Parameter für die gespeicherte SQL-Prozedur anzeigen. Wie beim Debuggen in anderen Programmiersprachen können Sie beim Debuggen des Transact\-SQL-Skripts lokale Variablen und Parameter anzeigen und ändern, globale Variablen anzeigen sowie Breakpoints steuern und verwalten.  
  
In diesem Beispiel wird veranschaulicht, wie eine gespeicherte Transact\-SQL-Prozedur durch schrittweises Ausführen erstellt und gedebuggt wird.  
  
> [!WARNING]  
> Bei den folgenden Vorgehensweisen werden Entitäten verwendet, die in vorherigen Vorgehensweisen in den Abschnitten [Entwicklung verbundener Datenbanken](../ssdt/connected-database-development.md) und [Projektorientierte Entwicklung von Offlinedatenbanken](../ssdt/project-oriented-offline-database-development.md) erstellt wurden.  
  
### <a name="to-debug-stored-procedures"></a>So debuggen Sie gespeicherte Prozeduren  
  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **TradeDev**, wählen Sie **Hinzufügen** aus, und klicken Sie anschließend auf **Gespeicherte Prozedur**. Weisen Sie dieser neuen gespeicherten Prozedur den Namen **AddProduct** zu, und klicken Sie auf **Hinzufügen**.  
  
2.  Fügen Sie in der gespeicherten Prozedur den folgenden Code ein:  
  
    ```  
    CREATE PROCEDURE [dbo].[AddProduct]  
    @id int,  
    @name nvarchar(128)  
    AS  
    INSERT INTO [dbo].[Product] (Id, Name) VALUES (@id, @name)  
    ```  
  
3.  Drücken Sie F5, um das Projekt zu erstellen und bereitzustellen.  
  
4.  Klicken Sie im SQL Server-Objekt-Explorer unter dem Knoten **Lokal** mit der rechten Maustaste auf die Datenbank **TradeDev**, und wählen Sie **Neue Abfrage** aus.  
  
5.  Fügen Sie im Abfragefenster den folgenden Code ein:  
  
    ```  
    EXEC [dbo].[AddProduct] 50, N'Contoso';  
    GO  
    ```  
  
6.  Klicken Sie auf den linken Rand des Fensters, um der `EXEC`-Anweisung einen Breakpoint hinzuzufügen.  
  
7.  Klicken Sie auf der Transact\-SQL-Editor-Symbolleiste auf der grünen Pfeilschaltfläche auf den Dropdownpfeil, und wählen Sie **Mit Debugger ausführen** aus, um die Abfrage mit aktiviertem Debugging auszuführen.  
  
8.  Alternativ können Sie das Debuggen über den SQL Server-Objekt-Explorer starten. Klicken Sie mit der rechten Maustaste auf die gespeicherte Prozedur **AddProduct** (unter **Lokal** -> **TradeDev**-Datenbank > **Programmierbarkeit** -> **Gespeicherte Prozeduren**). Wählen Sie **Prozedur debuggen...** aus. Wenn das Objekt Parameter erfordert, wird das Dialogfeld **Prozedur debuggen** mit einer Tabelle angezeigt, die eine Zeile für jeden Parameter enthält. Jede Zeile der Tabelle enthält eine Spalte für den Namen des Parameters und eine Spalte für den Wert dieses Parameters. Geben Sie für jeden Parameter Werte ein, und klicken Sie auf "OK".  
  
9. Vergewissern Sie sich, dass das Fenster **Lokal** geöffnet ist. Wenn dies nicht der Fall ist, klicken Sie im Menü **Debuggen** auf **Fenster** und anschließend auf **Lokal**.  
  
10. Drücken Sie F11, um einen Einzelschritt in die Abfrage auszuführen. Beachten Sie, dass die Parameter der gespeicherten Prozedur und deren jeweilige Werte im Fenster **Lokal** angezeigt werden. Sie können auch mit der Maus auf den `@name`-Parameter in der `INSERT`-Klausel zeigen. Diesem wird der Wert **Contoso** zugewiesen.  
  
11. Klicken Sie im Textfeld auf **Contoso**. Geben Sie **Fabrikam** ein, und drücken Sie die EINGABETASTE, um den Wert der `name`-Variable beim Debuggen zu ändern. Sie können den zugehörigen Wert auch im Fenster **Lokal** ändern. Der Wert des Parameters wird nun rot angezeigt, womit angegeben wird, dass er geändert wurde.  
  
12. Drücken Sie F10, um den verbleibenden Code zu überspringen.  
  
13. Aktualisieren Sie im SQL Server-Objekt-Explorer den Datenbankknoten **TradeDev**, um den neuen Inhalt in der Datenansicht der Tabelle **Product** anzuzeigen.  
  
14. Suchen Sie im SQL Server-Objekt-Explorer unter dem Knoten **Lokal** die Tabelle **Product** in der Datenbank **TradeDev**.  
  
15. Klicken Sie mit der rechten Maustaste auf die Tabelle **Product**, und wählen Sie **Daten anzeigen** aus. Die neue Zeile wurde der Tabelle hinzugefügt.  
  
