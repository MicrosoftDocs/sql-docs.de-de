---
title: Hinzufügen von Testbedingungen zu SQL Server-Komponententests
description: Hier erfahren Sie, wie Sie Testbedingungen zu einer SQL Server-Komponente hinzufügen. Dabei wird gezeigt, wie Sie hierfür den SQL Server-Komponententest-Designer verwenden.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 85ba2e56-a0b2-489c-aea2-fb135cce0cfc
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 9920430615d8391ef069078272ed8abbfcef6a00
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063595"
---
# <a name="how-to-add-test-conditions-to-sql-server-unit-tests"></a>Gewusst wie: Hinzufügen von Testbedingungen zu SQL Server-Komponententests

Sie können den **SQL Server-Komponententest-Designer** verwenden, um einem SQL Server-Komponententest Testbedingungen hinzuzufügen. Wenn Sie die Testklasse speichern, werden die Testbedingungen automatisch in Ihrem Testprojekt als Visual C\#- oder Visual Basic-Code in der Quellcodedatei gespeichert, in der die Testklasse enthalten ist. Nachdem Sie eine Testbedingung gespeichert haben, können Sie diese im **SQL Server-Komponententest-Designer** oder in der zugehörigen Quellcodedatei bearbeiten.  
  
### <a name="to-add-test-conditions-to-a-sql-server-unit-test"></a>So fügen Sie Testbedingungen zu einem SQL Server-Komponententest hinzu  
  
1.  Öffnen Sie einen SQL Server-Komponententest im **SQL Server-Komponententest-Designer**.  
  
    Der Name des geöffneten Tests wird auf der Navigationsleiste oben im SQL Server-Komponententest-Designer angezeigt. Über die Navigationsleiste können Sie die unterschiedlichen Testmethoden auswählen, die in der Testklasse enthalten sind.  
  
2.  Klicken Sie auf der Navigationsleiste auf die Testmethode, der Sie Testbedingungen hinzufügen möchten, oder klicken Sie auf **Allgemeine Skripts**.  
  
    > [!NOTE]  
    > CommonScripts sind keinem bestimmten Komponententest zugeordnet. Stattdessen sind sie den Komponententests in der Testklasse vor- oder nachgestellt. Weitere Informationen finden Sie unter [Skripts in SQL Server-Komponententests](../ssdt/scripts-in-sql-server-unit-tests.md).  
  
3.  Klicken Sie auf der Navigationsleiste auf das Transact\-SQL-Skript, dem Sie Testbedingungen hinzufügen möchten. Testbedingungen können dem Vortest-, Test- oder Nachtestskript hinzugefügt werden.  
  
    Das Transact\-SQL-Skript für diesen Test wird im Transact\-SQL-Editor, und die zugehörigen Testbedingungen werden im Bereich **Testbedingungen** angezeigt.  
  
4.  Klicken Sie in der Auswahlliste **Testbedingungen** erst auf eine Testbedingung und dann auf **Testbedingung hinzufügen** (+).  
  
    Die Testbedingung wird der Komponententestmethode hinzugefügt.  
  
    > [!NOTE]  
    > Sie können Testbedingungen innerhalb einer Testmethode neu anordnen, indem Sie auf eine Testbedingung und dann im Bereich **Testbedingungen** auf den Aufwärts- oder Abwärtspfeil klicken.  
  
5.  Wählen Sie die gerade hinzugefügte Testbedingung aus, und zeigen Sie das Fenster **Eigenschaften** an.  
  
    Konfigurieren Sie die Testbedingung im Fenster Eigenschaften. Sie können die **ExecutionTime**-Eigenschaft einer Ausführungszeit-Testbedingung ändern. Wenn Sie diese Eigenschaft festlegen, tritt beim Test ein Fehler auf, falls das Transact\-SQL-Skript nicht innerhalb der angegebenen Zeit ausgeführt wird.  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen und Definieren von SQL Server-Komponententests](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Vorgehensweise: Erstellen eines leeren SQL Server-Komponententests](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)  
[Vorgehensweise: Erstellen von SQL Server-Komponententests für Funktionen, Trigger und gespeicherte Prozeduren](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md)  
[Verwenden von Testbedingungen in SQL Server-Komponententests](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)  
[Skripts in SQL Server-Komponententests](../ssdt/scripts-in-sql-server-unit-tests.md)  
[Interpretieren der Ergebnisse von SQL Server-Komponententests](../ssdt/interpreting-sql-server-unit-test-results.md)  
[Vorgehensweise: Ausführen von SQL Server-Komponententests](../ssdt/how-to-run-sql-server-unit-tests.md)  
  
