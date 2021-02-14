---
title: Debuggen von Datenbankobjekten
description: Hier erfahren Sie, wie Sie gespeicherte Prozeduren, Funktionen und Trigger debuggen. Es wird gezeigt, wie Sie das Debuggen aktivieren, Breakpoints festlegen und einen SQL Server-Komponententest im Debugmodus ausführen.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: f5d4584f-e85f-4558-b056-83681c365978
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 9bc07e55af111adca6c8a995e2f40994ed2d9e5b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018250"
---
# <a name="how-to--debug-database-objects"></a>Gewusst wie:  Debuggen von Datenbankobjekten

Ein SQL Server-Komponententest umfasst folgende Komponenten:  
  
-   Den in Visual C\# oder Visual Basic geschriebenen Komponententestcode. Dieser Code, der vom SQL Server-Komponententest-Designer generiert wird, übermittelt das Transact\-SQL-Skript, das den Textkörper des Testes darstellt.  
  
-   Mindestens eine in Visual C\# oder Visual Basic geschriebene Testbedingung. Um Testbedingungen zu debuggen, befolgen Sie die Schritte zum Debuggen eines Komponententests wie unter [Gewusst wie: Debuggen während der Ausführung eines Tests (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182484(v=vs.100)) oder [Gewusst wie: Debuggen während der Ausführung eines Tests (Visual Studio 2012)](/previous-versions/ms182484(v=vs.140)) beschrieben.  
  
-   Mindestens ein Transact\-SQL-Skript das für Objekte in der Datenbank ausgeführt wird, die Sie testen. Diese Transact\-SQL-Skripts können nicht gedebuggt werden.  
  
In den Verfahren in diesem Thema wird das Debuggen bestimmter Datenbankobjekte beschrieben, z. B. von gespeicherten Prozeduren, Funktionen und Triggern in der getesteten Datenbank. Befolgen Sie zum Debuggen eines Datenbankobjekts die nachstehenden Verfahren in der angegebenen Reihenfolge:  
  
1.  Aktivieren Sie das SQL Server-Debugging für das Testprojekt.  
  
2.  Aktivieren Sie das Anwendungsdebugging für die SQL Server-Instanz, auf der die getestete Datenbank gehostet wird.  
  
3.  Legen Sie Breakpoints im Transact\-SQL-Skript der Datenbankobjekte fest, die Sie debuggen.  
  
4.  Debuggen Sie den Komponententest. In diesem Verfahren führen Sie den Test im Debugmodus aus.  
  
### <a name="to-enable-sql-debugging-on-your-test-project"></a>So aktivieren Sie das SQL-Debugging für das Testprojekt  
  
1.  Öffnen Sie den **Projektmappen-Explorer**.  
  
2.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Testprojekt, und klicken Sie auf **Eigenschaften**.  
  
    Eine Eigenschaftenseite mit demselben Namen wie das Testprojekt wird geöffnet.  
  
3.  Klicken Sie auf der Eigenschaftenseite auf **Debuggen**.  
  
4.  Klicken Sie unter **Debugger aktivieren** auf **SQL Server-Debugging aktivieren**.  
  
5.  Speichern Sie die Änderungen.  
  
### <a name="to-set-an-increased-execution-context-timeout-to-enable-debugging-for-your-test-project"></a>So legen Sie einen höheren Timeout für den Ausführungskontext fest, um das Debuggen des Testprojekts zu ermöglichen  
  
1.  Zeigen Sie im Menü **Datei** auf **Öffnen**, und klicken Sie auf **Datei**.  
  
2.  Wechseln Sie zu dem Ordner, in dem das Testprojekt enthalten ist, und doppelklicken Sie auf die Datei app.config.  
  
    Die Datei app.config wird im Editor geöffnet.  
  
3.  Ändern Sie den Knoten ExecutionContext, indem Sie ein Befehlstimeout hinzufügen, wie im folgenden Beispiel veranschaulicht:  
  
    ```  
    <ExecutionContext CommandTimeout ="300" Provider="System.Data.SqlClient" ConnectionString="Data Source=TargetServerName\TargetInstanceName;Initial Catalog=TargetDatabaseName;Integrated Security=True;Pooling=False" />  
    ```  
  
4.  Speichern Sie die Änderungen.  
  
5.  Erstellen Sie das Komponententestprojekt erneut.  
  
> [!IMPORTANT]  
> Wenn Sie das Projekt nicht neu erstellen, werden die an app.config vorgenommenen Änderungen beim Ausführen der Komponententests nicht angewendet, sodass das Debuggen fehlschlägt.  
  
### <a name="to-add-breakpoints-to-your-transact-sql-script"></a>So fügen Sie dem Transact\-SQL-Skript Breakpoints hinzu  
  
1.  Öffnen Sie im Menü **Sicht** den **SQL Server-Objekt-Explorer**.  
  
2.  Erweitern Sie unter **Datenverbindungen** den Knoten der zu testenden Datenbank.  
  
3.  Wenn neben dem Datenbanksymbol ein kleines rotes "x" angezeigt wird, ist die Verbindung mit der Datenbank geschlossen. Klicken Sie in diesem Fall mit der rechten Maustaste auf die Datenbank, und klicken Sie dann auf **Aktualisieren**. Sie müssen u. U. Anmeldeinformationen angeben, um die Verbindung mit der Datenbank zu öffnen.  
  
4.  Erweitern Sie den Knoten **Sichten**, **Gespeicherte Prozeduren** oder **Funktionen**, um das zu debuggende Objekt zu suchen.  
  
5.  Doppelklicken Sie auf das Objekt, das Sie debuggen möchten.  
  
6.  Klicken Sie auf die graue Randleiste, um einen Breakpoint festzulegen.  
  
### <a name="to-debug-your-sql-server-unit-test"></a>So debuggen Sie Ihren SQL Server-Komponententest  
  
1.  Öffnen Sie in Visual Studio 2010 das Fenster **Testansicht** („Test“ > „Windows“). Öffnen Sie in Visual Studio 2012 das Fenster **Test-Explorer**.  
  
2.  Klicken Sie mit der rechten Maustaste auf den Test, durch dessen Transact\-SQL-Skript das Datenbankobjekt ausgeführt wird, in dem Sie Breakpoints festgelegt haben, und wählen Sie **Debugauswahl** aus.  
  
    Der Test wird im Debugmodus ausgeführt, bis im Datenbankobjekt ein Breakpoint auftritt.  
  
3.  (Optional) Um ein weiteres Debugfenster zu öffnen, öffnen Sie das Menü **Debuggen**, zeigen auf **Fenster** und klicken auf **Breakpoints**, **Ausgabe** oder **Direkt**.  
  
## <a name="see-also"></a>Weitere Informationen  
[Ausführen von SQL Server-Komponententests](../ssdt/running-sql-server-unit-tests.md)  
[Debuggen von Transact-SQL (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/zefbf0t6(v=vs.100))  
