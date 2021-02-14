---
title: Skripts in SQL Server-Komponententests
description: In diesem Artikel erhalten Sie Informationen zu Komponententestskripts in SQL Server. Dabei erfahren Sie mehr über Vortest-, Test- und Nachtestskripts sowie die CommonScripts TestInitialize und TestCleanup.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 80c5cf62-a9c9-4e9d-8c6f-8eed50a595a7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: eb725a491eb87a0116413a786c514a0ede98361f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066945"
---
# <a name="scripts-in-sql-server-unit-tests"></a>Skripts in SQL Server-Komponententests

Jeder SQL Server-Komponententest enthält eine einzelne Aktion für Vortest, Test und Nachtest. Jede dieser Aktionen enthält wiederum folgende Elemente:  
  
-   Ein Transact\-SQL-Skript zur Ausführung auf einer Datenbank.  
  
-   0 (null) oder mehr Testbedingungen zur Auswertung der von der Skriptausführung zurückgegebenen Ergebnisse.  
  
Das Transact\-SQL-Testskript in der Testaktion ist die einzige Komponente, die in jedem SQL Server-Komponententest enthalten sein muss. Zusätzlich zum Testskript selbst möchten Sie wahrscheinlich Testbedingungen angeben, um zu überprüfen, ob das Testskript den erwarteten Wert bzw. die erwartete Gruppe von Werten zurückgibt. Durch die Testaktion wird ein bestimmtes Objekt in der Datenbank ausgeführt oder geändert und diese Änderung im Anschluss ausgewertet.  
  
Für jede Testaktion können Sie eine Vortestaktion und eine Nachtestaktion einschließen. Ähnlich wie die Testaktion enthält jede Vortestaktion und jede Nachtestaktion ein Transact\-SQL-Skript und 0 (null) oder mehr Testbedingungen. Anhand einer Vortestaktion können Sie sicherstellen, dass sich die Datenbank in einem Zustand befindet, der die Ausführung der Testaktion ermöglicht und die Rückgabe aussagekräftiger Ergebnisse sicherstellt. Beispielsweise können Sie eine Vortestaktion verwenden, um zu überprüfen, ob eine Tabelle Daten enthält, bevor das Testskript einen Testvorgang für diese Daten ausführt. Nachdem die Datenbank von der Vortestaktion vorbereitet wurde und die Testaktion aussagekräftige Ergebnisse zurückgegeben hat, kann die Nachtestaktion verwendet werden, um die Datenbank wieder in den Zustand zurückzuversetzen, der vor Ausführung der Vortestaktion gültig war. In einigen Fällen müssen Sie die Nachtestaktion verwenden, um die Ergebnisse der Testaktion zu überprüfen. Dies liegt daran, dass die Nachtestaktion über umfassendere Datenbankberechtigungen als die Testaktion verfügen kann. Weitere Informationen finden Sie unter [Übersicht über Verbindungszeichenfolgen und Berechtigungen](../ssdt/overview-of-connection-strings-and-permissions.md).  
  
Zusätzlich zu diesen drei Aktionen gibt es zwei Testskripts (CommonScripts), die vor und nach jedem SQL Server-Komponententest ausgeführt werden. Folglich können während eines einzelnen SQL Server-Komponententests bis zu fünf Transact\-SQL-Skripts ausgeführt werden. Nur das in der Testaktion enthaltene Transact\-SQL-Skript ist erforderlich. Die Ausführung von CommonScripts sowie von Skripts für die Vortest- und Nachtestaktion ist optional.  
  
Die folgende Tabelle enthält eine vollständige Liste der Skripts für die jeweiligen SQL Server-Komponententests.  
  
|**Aktion**|**Skripttyp**|**Beschreibung**|  
|--------------|-------------------|-------------------|  
|TestInitialize|CommonScript (Initialisierung)|(Optional) Dieses Skript ist allen Vortest- und Nachtestaktionen im Komponententest vorangestellt. Das TestInitialize-Skript wird vor allen Komponententests innerhalb einer bestimmten Testklasse ausgeführt. Dieses Skript wird im privilegierten Kontext ausgeführt.|  
|Vortest|Testskript|(Optional) Dieses Skript ist Teil des Komponententests. Das Vortestskript wird innerhalb eines Komponententests vor der Testaktion ausgeführt. Dieses Skript wird im privilegierten Kontext ausgeführt.|  
|Test|Testskript|(Erforderlich) Dieses Skript ist Teil des Komponententests. Durch dieses Skript kann z. B. eine gespeicherte Prozedur ausgeführt werden, durch die Tabellenwerte abgerufen, eingefügt oder aktualisiert werden. Dieses Skript wird im Ausführungskontext ausgeführt.|  
|Nachtest|Testskript|(Optional) Dieses Skript ist Teil des Komponententests. Das Nachtestskript wird nach einem einzelnen Komponententest ausgeführt. Dieses Skript wird im privilegierten Kontext ausgeführt.|  
|TestCleanup|CommonScript (Bereinigung)|(Optional) Dieses Skript wird im Anschluss an den Komponententest ausgeführt. Das TestCleanup-Skript wird nach allen Komponententests einer bestimmten Testklasse ausgeführt. Dieses Skript wird im privilegierten Kontext ausgeführt.|  
  
Weitere Informationen zu den verschiedenen Sicherheitskontexten, in denen diese Skripts ausgeführt werden, finden Sie unter [Übersicht über Verbindungszeichenfolgen und Berechtigungen](../ssdt/overview-of-connection-strings-and-permissions.md) und im Abschnitt zu den Berechtigungen für SQL Server-Komponententests unter [Erforderliche Berechtigungen für SQL Server Data Tools](../ssdt/required-permissions-for-sql-server-data-tools.md).  
  
## <a name="order-in-which-scripts-are-run"></a>Ausführungsreihenfolge für Skripts  
Die Reihenfolge, in der die einzelnen Skripts ausgeführt werden, ist von besonderer Bedeutung. Obwohl diese Reihenfolge nicht geändert werden kann, können Sie entscheiden, welche Skripts ausgeführt werden sollen. Die folgende Abbildung enthält die Auswahl von Skripts, die Sie in einem aus zwei SQL Server-Komponententests bestehenden Testlauf ausführen können. Darüber hinaus wird die Ausführungsreihenfolge verdeutlicht:  
  
![Zwei Datenbankkomponententests](../ssdt/media/twodatabaseunittests.png "Zwei Datenbankkomponententests")  
  
> [!NOTE]  
> Wenn die Bereitstellung eines SQL Server-Datenbankprojekts konfiguriert wurde, wurde dieser Schritt zu Beginn des Testlaufs mithilfe der Verbindungszeichenfolge für den privilegierten Kontext durchgeführt. Weitere Informationen finden Sie unter [Vorgehensweise: Konfigurieren der Ausführung von SQL Server-Komponententests](../ssdt/how-to-configure-sql-server-unit-test-execution.md).  
  
## <a name="initialization-and-cleanup-scripts"></a>Initialisierungs- und Bereinigungsskripts  
Im SQL Server-Komponententest-Designer werden die TestInitialize- und TestCleanup-Skripts als CommonScripts bezeichnet. Im vorangehenden Beispiel wird davon ausgegangen, dass die beiden Komponententests derselben Testklasse angehören. Folglich verwenden sie dieselben TestInitialize- und TestCleanup-Skripts. Dies trifft auf alle Komponententests innerhalb einer einzelnen Testklasse zu. Wenn der Testlauf jedoch Komponententests aus verschiedenen Testklassen umfasst, werden die CommonScripts der zugehörige Testklasse vor und nach der Ausführung des Komponententests ausgeführt.  
  
Wenn Sie Komponententests ausschließlich mithilfe des SQL Server-Komponententest-Designers schreiben, sind Sie möglicherweise nicht mit dem Konzept einer Testklasse vertraut. Sobald Sie einen Komponententest erstellen, indem Sie das Menü **Test** öffnen und auf **Neuer Test** klicken, generiert SQL Server Data Tools eine Testklasse. Testklassen werden im **Projektmappen-Explorer** mit dem angegebenen Testnamen gefolgt von der Erweiterung .cs oder .vb angezeigt. Innerhalb jeder Testklasse werden einzelne Komponententests als Testmethoden gespeichert. Unabhängig von der Anzahl der Testmethoden (d. h. Komponententests) kann jede Testklasse über 0 (null) bzw. ein TestInitialize- und TestCleanup-Skript verfügen.  
  
Mit dem TestInitialize-Skript können Sie die Testdatenbank vorbereiten, und mit dem TestCleanup-Skript können Sie die Testdatenbank in einen bekannten Zustand zurückversetzen. Beispielsweise können Sie mit TestInitialize eine gespeicherte Hilfsmethode erstellen, die Sie zu einem späteren Zeitpunkt im Testskript ausführen, um eine andere gespeicherte Prozedur zu testen.  
  
## <a name="pre-test-and-post-test-scripts"></a>Vortest- und Nachtestskripts  
Die Skripts, die den Vortest- und Nachtestaktionen zugeordnet sind, unterscheiden sich wahrscheinlich von Komponententest zu Komponententest. Sie können diese Skripts verwenden, um inkrementelle Änderungen an der Datenbank vorzunehmen und diese Änderungen dann zu bereinigen.  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen und Definieren von SQL Server-Komponententests](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Verwenden von Testbedingungen in SQL Server-Komponententests](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)  
  
