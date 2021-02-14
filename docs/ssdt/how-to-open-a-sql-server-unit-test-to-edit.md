---
title: Öffnen eines SQL Server-Komponententests zur Bearbeitung
description: In diesem Artikel wird erläutert, wie Sie einen SQL Server-Komponententest bearbeiten, sodass Sie Funktionen hinzufügen oder Bedingungen anpassen können. Außerdem erfahren Sie verschiedene Möglichkeiten, die Quellcodedatei des Tests zu öffnen.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: c6af1b12-54cd-42f9-b2ef-7164f8078323
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0cd06acbd8817c62e728ee054024f570682450fc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018170"
---
# <a name="how-to-open-a-sql-server-unit-test-to-edit"></a>Gewusst wie: Öffnen eines SQL Server-Komponententests zur Bearbeitung

Nachdem Sie einen SQL Server-Komponententest erstellt haben, verwenden Sie den **SQL Server-Komponententest-Designer**, um Transact\-SQL-Anweisungen und -Testbedingungen hinzuzufügen. Durch die vom Designer erstellten Tests wird Visual C#- oder Visual Basic-Code generiert. Dieser Code wird zusammen mit dem Test ausgeführt.  
  
Wenn der Test zu Ihrer Zufriedenheit verläuft, können Sie ihn in dieser Form ausführen. Wenn Sie dem Komponententest weitere Funktionen hinzufügen möchten, können Sie den zugehörigen Code bearbeiten. Der Code ist im Testprojekt in der Datei mit der Endung .cs oder .vb enthalten. Weitere Informationen finden Sie unter [SQL Server-Komponententestdateien](../ssdt/sql-server-unit-test-files.md). Sie können die Tests auch anpassen, indem Sie neue Testbedingungen erstellen. Weitere Informationen finden Sie unter [Vorgehensweise: Erstellen von Testbedingungen für den Datenbankkomponententest-Designer](/previous-versions/visualstudio/visual-studio-2010/aa833409(v=vs.100)).  
  
> [!NOTE]  
> Wenn Sie eine Testmethode durch Bearbeiten der CS- oder VB-Datei löschen, wird die Testmethode weiterhin im **SQL Server-Komponententest-Designer** angezeigt. Diese Situation tritt ein, weil die InitializeComponent-Methode der Testklasse immer noch die Membervariablen für diesen Test enthält. Obwohl der Test im Designer angezeigt wird, können Sie ihn nicht ausführen, weil der Code nicht mehr vorhanden ist. Um die Testmethode für diesen Test erneut zu generieren, bearbeiten Sie Transact\-SQL im Editor und speichern dann die CS- oder VB-Testdatei bzw. erstellen das Testprojekt neu.  
  
### <a name="to-open-the-source-code-file-of-a-sql-server-unit-test-from-solution-explorer"></a>So öffnen Sie die Quellcodedatei eines SQL Server-Komponententests vom Projektmappen-Explorer aus  
  
-   Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Quellcodedatei, in der der SQL Server-Komponententest enthalten ist, und klicken Sie dann auf **Code anzeigen**.  
  
    Die Testmethode des Komponententests wird beim Öffnen der Datei im Hauptbearbeitungsfenster von Visual Studio angezeigt.  
  
### <a name="to-open-the-source-code-file-of-a-sql-server-unit-test-from-the-test-view-window-visual-studio-2010"></a>So öffnen Sie die Quellcodedatei eines SQL Server-Komponententests vom Fenster „Testansicht“ aus (Visual Studio 2010)  
  
1.  Führen Sie einen Komponententest aus. Weitere Informationen finden Sie im Abschnitt „Ausführen von SQL Server-Komponententests“ im Artikel [Exemplarische Vorgehensweise: Erstellen und Ausführen eines SQL Server-Komponententests](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
2.  Klicken Sie im Fenster „Testansicht“ mit der rechten Maustaste auf den Test, und klicken Sie auf **Test öffnen**.  
  
    Die Testmethode des Komponententests wird beim Öffnen der Datei im Hauptbearbeitungsfenster von Visual Studio angezeigt.  
  
### <a name="to-open-the-source-code-file-of-a-sql-server-unit-test-from-the-test-view-window-visual-studio-2012"></a>So öffnen Sie die Quellcodedatei eines SQL Server-Komponententests vom Fenster „Testansicht“ aus (Visual Studio 2012)  
  
1.  Führen Sie einen Komponententest aus. Weitere Informationen finden Sie im Abschnitt „Ausführen von SQL Server-Komponententests“ im Artikel [Exemplarische Vorgehensweise: Erstellen und Ausführen eines SQL Server-Komponententests](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
2.  Klicken Sie im Fenster "Test-Explorer" auf den Namen der Quellcodedatei des Komponententests.  
  
    Die Testmethode des Komponententests wird beim Öffnen der Datei im Hauptbearbeitungsfenster von Visual Studio angezeigt.  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen und Definieren von SQL Server-Komponententests](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Überprüfen des Datenbankcodes mithilfe von SQL Server-Komponententests](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
