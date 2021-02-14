---
title: Behandeln von Problemen bei SQL Server-Datenbankkomponententests
description: Sehen Sie sich Tipps für die Behandlung von Problemen an, die möglicherweise bei SQL Server-Komponententests auftreten, z. B. Timeoutfehler oder eine Bereitstellung von Datenbanken auf unerwarteten Zielen.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: cf4c9cd1-7e73-4c3b-922a-68b9247e7b33
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0bd87bb75255ca50ceaed8e46c356555cdedf799
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066865"
---
# <a name="troubleshooting-sql-server-database-unit-testing-issues"></a>Behandeln von Problemen bei SQL Server-Datenbankkomponententests

Möglicherweise stoßen Sie auf die in diesem Thema beschriebenen Probleme, wenn Sie SQL Server-Komponententests für eine Datenbank ausführen:  
  
-   [Komponententests und ignorierte Änderungen an „App.Config“ beim Ausführen von Komponententests](#UnitTestingAndAppConfigChanges)  
  
-   [Datenbankbereitstellung auf unerwartetem Ziel beim Ausführen von Komponententests](#DatabaseDeploymentInUnitTests)  
  
-   [Timeouts beim Ausführen von Datenbankkomponententests](#TimeoutsDuringUnitTests)  
  
## <a name="unit-testing-and-appconfig-changes-ignored-when-you-run-unit-tests"></a><a name="UnitTestingAndAppConfigChanges"></a>Komponententests und ignorierte Änderungen an „App.Config“ beim Ausführen von Komponententests  
Wenn Sie die Datei "App.Config" im Testprojekt ändern, müssen Sie das Testprojekt neu erstellen, bevor die Änderungen wirksam werden. Diese Änderungen schließen auch diejenigen ein, die Sie an „App.Config“ im Dialogfeld **SQL Server-Testkonfiguration** vorgenommen haben. Wenn Sie das Testprojekt nicht neu erstellen, werden die Änderungen beim Ausführen der Komponententests nicht übernommen.  
  
## <a name="database-deployment-to-unexpected-target-when-you-run-unit-tests"></a><a name="DatabaseDeploymentInUnitTests"></a>Datenbankbereitstellung auf unerwartetem Ziel beim Ausführen von Komponententests  
Wenn Sie eine Datenbank von einem Datenbankprojekt bereitstellen und Komponententests ausführen, wird die Datenbank anhand der Informationen der Verbindungszeichenfolge bereitgestellt, die in der Konfiguration der Komponententests angegeben sind. Die in den Debugeigenschaften des Datenbankprojekts angegebenen Verbindungsinformationen werden für diese Aufgabe nicht verwendet. So können Sie die SQL Server-Komponententests für andere Instanzen derselben Datenbank ausführen.  
  
## <a name="timeouts-when-you-run-database-unit-tests"></a><a name="TimeoutsDuringUnitTests"></a>Timeouts beim Ausführen von Datenbankkomponententests  
Wenn Ihre Datenbankkomponententests aufgrund eines Timeouts fehlschlagen, können Sie den Zeitraum des Timeouts verlängern, indem Sie die Datei "App.Config" in Ihrem Testprojekt aktualisieren. Der in der Verbindungszeichenfolge angegebene Timeout für die Verbindung gibt die Wartezeit an, wenn der Komponententest eine Verbindung zum Server herstellt. Das Befehlstimeout, das direkt in der Datei „App.Config“ definiert werden muss, gibt die Wartezeit an, wenn der Komponententest das Transact\-SQL-Skript ausführt. Wenn Sie bei der Ausführung lang andauernder Komponententests auf Probleme stoßen, versuchen Sie, den Wert des Befehlstimeouts im entsprechenden Kontextelement zu erhöhen. Um beispielsweise ein Befehlstimeout von 120 Sekunden für das Element **PrivilegedContext** anzugeben, aktualisieren Sie die Datei „app.config“ wie folgt:  
  
```  
<SqlUnitTesting_VS2010>  
    <DatabaseDeployment DatabaseProjectFileName="..\..\..\..\..\..\Visual Studio 2010\Projects\Database10\Database10\AdventureWorks.sqlproj"  
        Configuration="Debug" />  
    <DataGeneration ClearDatabase="true" />  
    <ExecutionContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(LocalDB)\Projects;Initial Catalog=AdventureWorks_Test;Integrated Security=True;Pooling=False"  
        CommandTimeout="30" />  
    <PrivilegedContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(LocalDB)\Projects;Initial Catalog=AdventureWorks_Test;Integrated Security=True;Pooling=False"  
        CommandTimeout="120" />  
</SqlUnitTesting_VS2010>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[Vorgehensweise: Erstellen von SQL Server-Komponententests für Funktionen, Trigger und gespeicherte Prozeduren](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md)  
[Vorgehensweise: Konfigurieren der Ausführung von SQL Server-Komponententests](../ssdt/how-to-configure-sql-server-unit-test-execution.md)  
  
