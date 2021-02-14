---
title: Überprüfen des Datenbankcodes mithilfe von SQL Server-Komponententests
description: In diesem Artikel erfahren Sie mehr zu SQL Server-Komponententests. Sie erfahren außerdem, welche Visual Studio-Versionen die Tests unterstützen, und Sie können sich Ressourcen zur Verwendung der Tests ansehen, um Datenbankcode zu überprüfen.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 003713e2-de6b-4277-a0a8-7d1f2f4ffb39
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 4c05b9e3a3330b395ee3cd8c2cb3da5aa7d50508
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066698"
---
# <a name="verifying-database-code-by-using-sql-server-unit-tests"></a>Überprüfen des Datenbankcodes mithilfe von SQL Server-Komponententests

Sie können die SQL Server-Komponententests verwenden, um einen Baselinezustand für die Datenbank festzulegen und anschließend alle nachfolgenden Änderungen zu überprüfen, die an Datenbankobjekten vorgenommen werden.  
  
Um einen Baselinezustand für eine Datenbank herzustellen, erstellen Sie ein Testprojekt und schreiben Transact\-SQL-Codesätze, die für Datenbankobjekte ausgeführt werden. Mithilfe dieser Tests können Sie in einer isolierten Entwicklungsumgebung überprüfen, ob Ihre Objekte wie erwartet funktionieren. SQL Server-Komponententests funktionieren gut in Kombination mit der Offlinedatenbankentwicklung, bei denen SQL Server-Datenbankprojekte verwendet werden (weitere Informationen finden Sie unter [Projektorientierte Offlinedatenbankentwicklung](../ssdt/project-oriented-offline-database-development.md)). Sobald Sie über einen Baselinesatz von SQL Server-Komponententests verfügen, können Sie mithilfe der Tests überprüfen, ob die Datenbank ordnungsgemäß funktioniert, bevor Sie Änderungen in die Versionskontrolle einchecken.  
  
Sie können Tests erstellen, durch die Änderungen an beliebigen Datenbankobjekten überprüft werden. Darüber hinaus können Sie automatisch Transact\-SQL-Codestubs zum Testen von Datenbankfunktionen, Triggern und gespeicherten Prozeduren generieren.  
  
> [!NOTE]  
> SQL Server-Komponententests können erstellt und ausgeführt werden, ohne dass ein Datenbankprojekt geöffnet ist. Wenn Sie Testskripts zum Testen spezifischer Datenbankobjekte aus dem Projekt jedoch automatisch generieren möchten, müssen Sie das Datenbankprojekt öffnen, in dem die Testobjekte enthalten sind.  
  
Während das Datenbankschema von den Mitgliedern des Teams geändert wird, können Sie anhand dieser Tests überprüfen, ob durch die Änderungen bestehende Funktionen außer Kraft gesetzt wurden. Sie erstellen SQL Server-Komponententests als Ergänzung zu den Softwarekomponententests, die von den Softwareentwicklern erstellt werden. Um das Gesamtverhalten der Anwendung zu überprüfen, müssen beide Testsätze abgeschlossen werden.  
  
Anhand der Komponententests können Sie überprüfen, ob Prozeduren erfolgreich sind, wenn ein erfolgreicher Verlauf erwartet wird, bzw. ob sie fehlschlagen, wenn ein Fehler erwartet wird. Tests, in denen das Auftreten bestimmter Fehler erwartet wird, werden als Negativnachweis bezeichnet.  
  
## <a name="visual-studio-editions-support-for-sql-server-unit-tests"></a>Unterstützung von SQL Server-Komponententests in den Visual Studio-Editionen  
Mit der Funktion für SQL Server-Komponententests, die im Dezember 2012-Update für SQL Server Data Tools hinzugefügt wurde, können Sie in Visual Studio 2010 Professional sowie in Visual Studio 2012 Professional und höheren Versionen SQL Server-Komponententests erstellen, ändern und ausführen.  
  
Um sicherzustellen, dass Sie das aktuelle Update für SQL Server Data Tools installieren, lesen Sie [Dialogfeld „Nach Updates suchen“](../ssdt/check-for-updates-dialog-box.md).  
  
Die in Visual Studio 2010 und Visual Studio 2012 integrierte SQL Server Data Tools-Shell unterstützt keine SQL Server-Komponententests.  
  
## <a name="common-tasks"></a>Allgemeine Aufgaben  
Die folgende Tabelle enthält Beschreibungen allgemeiner Aufgaben, die dieses Szenario unterstützen. Außerdem finden Sie dort Links zu weiteren Informationen, wie Sie diese Aufgaben erfolgreich ausführen können.  
  
|Allgemeine Aufgaben|Hilfreiche Themen|  
|----------------|----------------------|  
|**Praktische Übungen:** Sie können eine allgemeine exemplarische Vorgehensweise durchlaufen, um sich mit der Erstellung und Ausführung eines einfachen SQL Server-Komponententests vertraut zu machen. Diese exemplarische Vorgehensweise enthält ein Beispiel für einen negativen SQL Server-Komponententest.|[Exemplarische Vorgehensweise: Erstellen und Ausführen eines SQL Server-Komponententests](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md)|  
|**Erstellen von SQL Server-Komponententest:** Sie müssen SQL Server-Komponententests in deren eigenem Projekt erstellen. Sie konfigurieren die Einstellungen für das Projekt und legen mindestens eine Testbedingung pro Test fest.|[Erstellen und Definieren von SQL Server-Komponententests](../ssdt/creating-and-defining-sql-server-unit-tests.md)<br /><br />[Verwenden von Testbedingungen in SQL Server-Komponententests](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)|  
|**SQL Server-Komponententests ausführen**: Nachdem Sie mindestens einen Komponententest definiert haben, können Sie den Test bzw. die Tests ausführen, Probleme debuggen und die Testergebnisse untersuchen.|[Ausführen von SQL Server-Komponententests](../ssdt/running-sql-server-unit-tests.md)|  
|**Verwalten von Testgruppen (Visual Studio 2010):** Sie können Tests in Gruppen organisieren, wenn diese normalerweise gleichzeitig ausgeführt werden sollen. Testlisten werden weiterhin unterstützt, für neue Testgruppen sollten Sie jedoch die Verwendung von Testkategorien erwägen. Beispielsweise können Sie eine Testkategorie erstellen, in der Tests für Trigger oder Tests für alle Objekte in einem bestimmten *Schema* zusammengefasst sind.|[Definieren von Testkategorien zum Gruppieren von Tests](/previous-versions/visualstudio/visual-studio-2010/dd286595(v=vs.100))<br /><br />[Definieren von Testlisten zum Gruppieren von Tests](/previous-versions/visualstudio/visual-studio-2010/dd286584(v=vs.100))|  
|**Testprojekte und Tests in die Versionskontrolle einchecken**: Nachdem Sie Tests ausgeführt und sichergestellt haben, dass sie ordnungsgemäß funktionieren, sollten Sie das Testprojekt und alle zugehörigen Dateien in die Versionskontrolle einchecken, damit die Tests von allen Teammitgliedern ausgeführt werden können. Indem Sie das Testprojekt zusammen mit dem SQL Server-Datenbankprojekt in die Versionskontrolle einchecken, können Sie auf einfache Weise kompatible Versionen sowohl von der Datenbank als auch von den Datenbanktests wiederherstellen.|[Hinzufügen von Dateien zur Versionskontrolle](/previous-versions/visualstudio/visual-studio-2010/ms181374(v=vs.100))<br /><br />[Verwenden der Fenster „Einchecken“ und „Ausstehende Änderungen“](/previous-versions/visualstudio/visual-studio-2010/ms245462(v=vs.100))|  
|**Benutzerdefinierte Testbedingungen definieren**: Sie können benutzerdefinierte Testbedingungen erstellen, wenn Sie ein bestimmtes Verhalten testen müssen, das vom Standardsatz der Testbedingungen nicht abgedeckt wird. Diese Bedingungen müssen an alle Teammitglieder verteilt werden, die die Tests mit den neuen Bedingungen ausführen möchten.|[Szenario: Definieren benutzerdefinierter Testbedingungen für SQL Server-Komponententests](/previous-versions/visualstudio/visual-studio-2010/dd193282(v=vs.100))|  
|**Bestehende Komponententests aktualisieren**: Wenn Sie über Datenbankkomponententests verfügen, die in einer Vorgängerversion von Visual Studio erstellt wurden, müssen Sie ein Upgrade für diese durchführen, damit sie in dieser Version erfolgreich erstellt und ausgeführt werden.<br /><br />**HINWEIS**: Wenn Sie eine Projektmappe öffnen, in der sowohl ein Datenbankprojekt als auch ein Datenbankkomponententestprojekt aus einer Vorgängerversion von Visual Studio enthalten ist, werden Sie aufgefordert, ein Upgrade für das Datenbankprojekt durchzuführen. Sie werden nicht aufgefordert, Datenbankkomponententest-Projekte zu aktualisieren, da sie manuell aktualisiert werden müssen.|[Durchführen eines Upgrades für ein älteres Testprojekt mit Datenbankkomponententests](../ssdt/upgrade-an-older-test-project-containing-database-unit-tests.md)|  
|**Erweiterbarkeit:** Sie können SQL Server Data Tools erweitern, indem Sie Funktionserweiterungen erstellen.|[Benutzerdefinierte Testbedingungen für SQL Server-Komponententests](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)|  
|**Behandeln von Problemen:** Erhalten Sie weitere Informationen zur Behandlung allgemeiner Probleme mit SQL Server-Komponententests.|[Behandeln von Problemen bei SQL Server-Datenbankkomponententests](../ssdt/troubleshooting-sql-server-database-unit-testing-issues.md)|  
  
## <a name="related-scenarios"></a>Verwandte Szenarien  
[Projektorientierte Offlinedatenbankentwicklung](../ssdt/project-oriented-offline-database-development.md)  
Datenbankkomponententests sind besonders wirkungsvoll, wenn sie mit der Offlineprojektentwicklung unter Verwendung von SQL Server-Datenbankprojekten kombiniert werden.  
  
## <a name="see-also"></a>Weitere Informationen  
[SQL Server Data Tools](../ssdt/sql-server-data-tools.md)  
