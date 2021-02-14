---
title: Erstellen eines Testprojekts für SQL Server-Datenbankkomponententests
description: In diesem Artikel wird erläutert, wie Sie Testprojekte für SQL Server-Datenbankkomponententests erstellen. Informieren Sie sich über die verschiedenen Möglichkeiten zum Hinzufügen von Testprojekten zu Projektmappen, die Datenbankprojekte enthalten.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 4b3e7ba8-b565-4689-af1a-34cc255b7c60
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 8fe5cea5559f687b74fac93239b694a7736c6fcd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018330"
---
# <a name="how-to-create-a-test-project-for-sql-server-database-unit-testing"></a>Gewusst wie: Erstellen eines Testprojekts für SQL Server-Datenbankkomponententests

Bevor Sie mit dem Schreiben von Komponententests beginnen können, durch die Datenbankobjekte ausgewertet werden, müssen Sie erst ein Testprojekt erstellen. Das Projekt enthält SQL Server-Komponententests, kann jedoch auch andere Testtypen enthalten.  
  
Sie können alle SQL Server-Komponententests für ein bestimmtes Datenbankprojekt in einem einzelnen Testprojekt platzieren. Möglicherweise möchten Sie jedoch zusätzliche Testprojekte erstellen, je nachdem, wie die Antwort auf die folgenden Fragen ausfällt:  
  
|Frage|Entscheidung|  
|-|-|   
|Müssen verschiedene SQL Server-Komponententests zur Testausführung oder Testüberprüfung auf verschiedene Datenbankverbindungen zugreifen?|Falls ja, benötigen Sie mehr als ein Testprojekt. Es kann nur maximal eine Datenbankverbindung für die Testausführung angegeben werden. Sie können jedoch eine andere Datenbankverbindung für die Testüberprüfung angeben.|  
|Möchten Sie für verschiedene Komponententests unterschiedliche Datenbankprojekte bereitstellen?|Falls ja, benötigen Sie mehr als ein Testprojekt. Von einem Testprojekt kann nur ein einzelnes Datenbankprojekt bereitgestellt werden.|  
  
Weitere Informationen zu den einzelnen Fragen finden Sie unter [Vorgehensweise: Konfigurieren der Ausführung von SQL Server-Komponententests](../ssdt/how-to-configure-sql-server-unit-test-execution.md). Anstatt mehrere Testprojekte zu erstellen, können Sie auch eine eigene [DatabaseTestService](/previous-versions/visualstudio/visual-studio-2010/dd154755(v=vs.100))-Implementierung „Microsoft.Data.Schema.UnitTesting.DatabaseTestService“ bereitstellen.  
  
Es gibt drei Möglichkeiten, um einer Projektmappe, in der ein Datenbankprojekt enthalten ist, ein Testprojekt hinzuzufügen:  
  
-   Fügen Sie der Projektmappe ein Testprojekt hinzu. Das Testprojekt enthält einen Standardkomponententest, den Sie löschen können. Dieses Projekt enthält keine SQL Server-Komponententestklasse. Diese muss von Ihnen hinzugefügt werden.  
  
-   Fügen Sie einen neuen SQL Server-Komponententest über das Menü **Test** hinzu. Wenn Sie den Komponententest hinzufügen, erstellt SQL Server Data Tools, falls von Ihnen angefordert, auch ein Testprojekt. Dieses Projekt enthält eine Klasse für SQL Server-Komponententests. SQL Server-Testklassen für Komponententests enthalten mindestens einen Komponententest.  
  
-   Erstellen Sie einen Komponententest von einer gespeicherten Prozedur, einer Funktion oder einem Trigger aus einem geöffneten Projekt im SQL Server-Objekt-Explorer. Wenn Sie den Komponententest erstellen, erstellt SQL Server Data Tools, falls von Ihnen angefordert, auch ein Testprojekt. Dieses Projekt enthält eine Klasse für SQL Server-Komponententests. SQL Server-Testklassen enthalten mindestens einen Komponententest.  
  
Die einzelnen Methoden werden in den folgenden Prozeduren vorgestellt.  
  
### <a name="to-add-a-test-project-to-an-existing-solution"></a>So fügen Sie einer vorhandenen Projektmappe ein Testprojekt hinzu  
  
1.  Zeigen Sie im Menü **Datei** auf **Neu**, und klicken Sie auf **Projekt**.  
  
    Das Dialogfeld **Neues Projekt** wird angezeigt.  
  
2.  Erweitern Sie unter **Installierte Vorlagen** den Knoten **SQL Server**, und klicken Sie dann auf **SQL Server-Datenbankprojekt**.  
  
3.  Geben Sie unter **Name** einen Projektnamen ein.  
  
### <a name="to-create-a-test-project-with-a-sql-server-unit-test-class"></a>So erstellen Sie ein Testprojekt mit einer SQL Server-Komponententestklasse  
  
-   Führen Sie die Vorgehensweise durch, die unter [Gewusst wie: Erstellen eines leeren SQL Server-Komponententests](../ssdt/how-to-create-an-empty-sql-server-unit-test.md) oder [Gewusst wie: Erstellen von SQL Server-Komponententests für Funktionen, Trigger und gespeicherte Prozeduren](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md) beschrieben wird.  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen und Definieren von SQL Server-Komponententests](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
