---
title: Ausführen von SQL Server-Komponententests
description: Hier erfahren Sie, wie Sie SQL Server-Komponententests ausführen. In den Schritten wird erläutert, wie Sie Tests über verschiedene Fenster und Tools in unterschiedlichen Versionen von Visual Studio ausführen.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 34fe2d1e-d47b-4808-af56-8cc0fdae6518
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 05ad77151fbe0f9e7c0a9c7667b1dea6826f397f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018120"
---
# <a name="how-to-run-sql-server-unit-tests"></a>Gewusst wie: Ausführen von SQL Server-Komponententests

Sie können einen SQL Server-Komponententest auf verschiedene Weisen ausführen, indem Sie z.B. das Eingabeaufforderungsfenster oder verschiedene andere Fenster verwenden.  
  
> [!NOTE]  
> Komponententests können nicht remote ausgeführt werden.  
  
Welche Möglichkeiten verfügbar sind, hängt von der installierten Software ab, wie unter [Ausführen von SQL Server-Komponententests](../ssdt/running-sql-server-unit-tests.md) beschrieben wird.  
  
### <a name="to-run-sql-server-unit-tests-using-test-view-visual-studio-2010"></a>So führen Sie SQL Server-Komponententests mithilfe der Testansicht (Visual Studio 2010) aus  
  
1.  Zeigen Sie im Menü **Test** auf **Fenster**, und klicken Sie dann auf **Testansicht**.  
  
    Das Fenster **Testansicht** wird geöffnet.  
  
2.  Klicken Sie im Fenster **Testansicht** auf die Tests, die Sie ausführen möchten. Durch Verwendung der STRG- oder UMSCHALTTASTE können Sie zusammenhängende oder unzusammenhängende Testblöcke angeben.  
  
3.  Führen Sie einen der folgenden Schritte aus:  
  
    -   Klicken Sie mit der rechten Maustaste auf die Fläche des Fensters **Testansicht**, und klicken Sie dann auf **Auswahl ausführen**.  
  
    -   Klicken Sie auf der Symbolleiste **Testansicht** auf **Auswahl ausführen**.  
  
### <a name="to-run-sql-server-unit-tests-using-test-explorer-visual-studio-2012"></a>So führen Sie SQL Server-Komponententests mithilfe des Test-Explorers (Visual Studio 2012) aus  
  
1.  Zeigen Sie im Menü **Test** auf **Fenster**, und klicken Sie dann auf **Test-Explorer**.  
  
    Das Fenster **Test-Explorer** wird geöffnet.  
  
2.  Klicken Sie im Fenster **Test-Explorer** auf die Tests, die Sie ausführen möchten. Durch Verwendung der STRG- oder UMSCHALTTASTE können Sie zusammenhängende oder unzusammenhängende Testblöcke angeben.  
  
3.  Klicken Sie mit der rechten Maustaste auf einen der markierten Tests, und klicken Sie auf **Ausgewählte Tests ausführen**.  
  
### <a name="to-run-sql-server-unit-tests-from-the-sql-server-unit-test-designer-visual-studio-2010"></a>So führen Sie SQL Server-Komponententests über den SQL Server-Komponententest-Designers (Visual Studio 2010) aus  
  
-   Auf der Symbolleiste **Testtools** befinden sich Schaltflächen zum Starten eines Projekts mit oder ohne dem Debugger.  
  
Durch diesen Schritt werden alle Tests im aktuellen Testlauf ausgeführt. Sobald Sie einen Testlauf starten, wird das Fenster **Testergebnisse** mit dem Status des Testlaufs angezeigt. In diesem Fenster sind sowohl die aktiven als auch die abgeschlossenen Tests enthalten. Weitere Informationen finden Sie unter [Interpretieren der Ergebnisse von SQL Server-Komponententests](../ssdt/interpreting-sql-server-unit-test-results.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[Ausführen von SQL Server-Komponententests](../ssdt/running-sql-server-unit-tests.md)  
[Vorgehensweise: Ausführen von automatisierten Tests in Microsoft Visual Studio 2010](/previous-versions/visualstudio/visual-studio-2010/ms182470(v=vs.100))  
[Ausführen von automatisierten Tests in der Befehlszeile (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182486(v=vs.100))  
[Testen der Anwendung (Visual Studio 2012)](/azure/devops/test/overview)  
