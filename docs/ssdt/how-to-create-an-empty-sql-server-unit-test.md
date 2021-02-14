---
title: Erstellen eines leeren SQL Server-Komponententests
description: In diesem Artikel erfahren Sie, wie Sie Komponententests in SQL Server erstellen. Dabei lernen Sie, wie Sie die auch von anderen Tests verwendeten Skripts „TestInitialize“ und „TestCleanup“ sowie verschiedene Skripts verwenden.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.unittesting.createtest
ms.assetid: b6f3cd5a-3389-42d6-a93f-97b3ddf31b95
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 510d669d780a7f7b6f71f5a84eab87e9b70bf4b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018320"
---
# <a name="how-to-create-an-empty-sql-server-unit-test"></a>Gewusst wie: Erstellen eines leeren SQL Server-Komponententests

Schließen Sie Komponententests in das Datenbankprojekt ein, um sicherzustellen, dass vorhandene Funktionen nicht durch Änderungen an Datenbankobjekten beeinträchtigt werden. Bei den folgenden Vorgehensweisen wird das Erstellen von SQL Server-Komponententests für beliebige Datenbankobjekte erläutert. SQL Server Data Tools bietet ein gewisses Maß an ergänzender Unterstützung für Datenbankfunktionen, Trigger und gespeicherte Prozeduren. Weitere Informationen finden Sie unter [Vorgehensweise: Erstellen von SQL Server-Komponententests für Funktionen, Trigger und gespeicherte Prozeduren](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md).  
  
Wenn Sie einen SQL Server-Komponententest anhand der ersten Prozedur erstellen, wird automatisch ein Testprojekt erstellt, sofern noch kein Testprojekt vorhanden ist. Wenn bereits Testprojekte vorhanden sind, können Sie einem dieser Projekte entweder den neuen Test hinzufügen oder ein neues Testprojekt erstellen. Weitere Informationen zu Testprojekten finden Sie unter [Vorgehensweise: Erstellen eines Testprojekts für SQL Server-Datenbank-Unittests](../ssdt/how-to-create-a-test-project-for-sql-server-database-unit-testing.md).  
  
Es gibt zwei Methoden zum Erstellen eines SQL Server-Komponententests:  
  
-   Erstellen eines neuen SQL Server-Komponententests in einer neuen Testklasse.  
  
    Alle SQL Server-Komponententests in einer bestimmten Testklasse verwenden das gleiche TestInitialize-Skript und TestCleanup-Skript. Erstellen Sie eine neue Testklasse, wenn der Komponententest andere TestInitialize- und TestCleanup-Skripts verwenden soll als die übrigen Komponententests. Weitere Informationen finden Sie unter [Skripts in SQL Server-Komponententests](../ssdt/scripts-in-sql-server-unit-tests.md).  
  
-   Erstellen Sie einen neuen SQL Server-Komponententest in einer vorhandenen Testklasse.  
  
    Entscheiden Sie sich für diese Option, wenn der Komponententest die gleichen TestInitialize- und TestCleanup-Skripts wie andere Komponententests innerhalb der Klasse verwendet.  
  
### <a name="to-create-a-sql-server-unit-test-inside-a-new-test-class"></a>So erstellen Sie einen SQL Server-Komponententest in einer neuen Testklasse  
  
1.  Klicken Sie im Menü **Test** auf **Neuer Test**.  
  
    Das Dialogfeld **Neuen Test hinzufügen** wird angezeigt.  
  
2.  Klicken Sie in der Liste **Vorlagen** auf **SQL Server-Komponententest**.  
  
3.  Geben Sie unter **Testname** einen Namen für den Test ein.  
  
4.  Wählen Sie unter **Zu Testprojekt hinzufügen** ein vorhandenes Testprojekt aus, dem dieser Test hinzugefügt wird. Wenn kein Testprojekt vorhanden ist bzw. Sie ein neues Testprojekt erstellen möchten, wählen Sie **Ein neues <language>-Testprojekt erstellen** aus.  
  
5.  Klicken Sie auf **OK**.  
  
    Wenn das Testprojekt neu ist, wird das Dialogfeld **Neues Testprojekt** angezeigt. Geben Sie dem Projekt einen Namen, und klicken Sie auf **OK**.  
  
    Wenn das Testprojekt neu ist bzw. noch nicht konfiguriert wurde, wird das Dialogfeld **SQL Server-Testkonfiguration von <ProjectName>** angezeigt. In diesem Dialogfeld können Sie die folgenden Informationen für das Testprojekt konfigurieren:  
  
    -   Die zur Testausführung verwendete Datenbankverbindung.  
  
    -   Die Datenbankverbindung, die zur Überprüfung von Testergebnissen, Datenbankbereitstellung und Datengenerierung verwendet wird.  
  
    -   Die automatische Bereitstellung des Datenbankprojekts und zugehöriger Schemaänderungen an einer bestimmten Projektkonfiguration, bevor Komponententests ausgeführt werden.  
  
    Weitere Informationen finden Sie unter [Vorgehensweise: Konfigurieren der Ausführung von SQL Server-Komponententests](../ssdt/how-to-configure-sql-server-unit-test-execution.md).  
  
6.  Geben Sie die Projektkonfigurationsinformationen an, und klicken Sie auf **OK**.  
  
    \- oder –  
  
    Klicken Sie auf **Abbrechen**, um den Komponententest ohne Konfiguration des Testprojekts zu erstellen.  
  
    Der leere Test wird im **SQL Server-Komponententest-Designer** angezeigt. Je nach Programmiersprache, die Sie zum Erstellen des Testprojekts angegeben haben, wird dem Testprojekt eine Visual Basic- oder Visual C\#-Quellcodedatei hinzugefügt. Diese Datei enthält die SQL Server-Komponententestklasse, die SQL Server Data Tools für den gerade erstellten Komponententest generiert. Diese Testklasse kann einen oder mehrere Komponententests enthalten, die Sie über den SQL Server-Komponententest-Designer bzw. mittels Code als neue Testmethoden in die Testklasse aufnehmen.  
  
    Darüber hinaus können Sie zusätzliche Tests hinzufügen.  
  
    -   Klicken Sie dazu mit der rechten Maustaste im **Projektmappen-Explorer** auf ein Testprojekt, wählen Sie **Hinzufügen**, **Neuer Test** und dann **SQL Server-Komponententest** aus.  
  
    -   Wählen Sie im SQL Server-Objekt-Explorer „Komponententests erstellen“ aus.  
  
    Wenn Sie diese Datei im **Projektmappen-Explorer** auswählen, wird sie standardmäßig im SQL Server-Komponententest-Designer angezeigt. Um den Code anzuzeigen oder anzupassen, damit in den Komponententests weitere Funktionalität zur Verfügung stehen, markieren Sie die Datei, klicken mit der rechten Maustaste darauf und wählen **Code anzeigen** aus.  
  
### <a name="to-create-a-sql-server-unit-test-inside-an-existing-test-class"></a>So erstellen Sie einen neuen SQL Server-Komponententest in einer vorhandenen Testklasse  
  
1.  Öffnen Sie eine vorhandene SQL Server-Komponententestklasse im **SQL Server-Komponententest-Designer**. Sie greifen auf den **SQL Server-Komponententest-Designer** zu, indem Sie auf die Quellcodedatei eines Komponententests im **Projektmappen-Explorer** doppelklicken.  
  
2.  Klicken Sie auf der Navigationsleiste auf das Pluszeichen ( **+** ), um das Dialogfeld **Namen für Komponententest angeben** anzuzeigen.  
  
3.  Geben Sie einen Namen ein, und klicken Sie auf **OK**.  
  
    Der neue SQL Server-Komponententest ist in der Dropdownliste in der Navigationsleiste verfügbar. Darüber hinaus wird er der Testklasse als neue Testmethode hinzugefügt. Um die Testmethode im Code anzuzeigen, markieren Sie die Klassendatei, klicken mit der rechten Maustaste darauf und wählen **Code anzeigen** aus. Der Name der aktuellen Testklassendatei wird auf der Registerkarte oben im **SQL Server-Komponententest-Designer** angezeigt.  
  
Nachdem Sie das Testprojekt konfiguriert und den Komponententest erstellt haben, sind folgende Schritte auszuführen:  
  
-   Hinzufügen eines Transact\-SQL-Testskripts  
  
-   Definieren Sie eine Vortest- und Nachtestaktion.  
  
-   Fügen Sie Testbedingungen oder eine andere Bestätigungsanweisung hinzu, um die Skriptergebnisse zu überprüfen.  
  
> [!NOTE]  
> Die Testbedingung „Nicht eindeutig“ ist die Standardbedingung, die jedem Test hinzugefügt wird. Diese Testbedingung wird eingeschlossen, um anzuzeigen, dass die Testüberprüfung nicht implementiert wurde. Löschen Sie diese Testbedingung aus dem Test, nachdem Sie weitere Testbedingungen hinzugefügt haben. Weitere Informationen finden Sie unter [Vorgehensweise: Hinzufügen von Testbedingungen zu Datenbankkomponententests](/previous-versions/visualstudio/visual-studio-2010/aa833242(v=vs.100)).  
  
## <a name="see-also"></a>Weitere Informationen  
[Vorgehensweise: Ausführen von SQL Server-Komponententests](../ssdt/how-to-run-sql-server-unit-tests.md)  
[Erstellen und Definieren von SQL Server-Komponententests](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Erstellen von Komponententests](/previous-versions/visualstudio/visual-studio-2008/ms182523(v=vs.90))  
