---
title: Konfigurieren der Ausführung von SQL Server-Komponententests
description: In diesem Artikel erfahren Sie, wie Sie Komponententests in SQL Server konfigurieren. Sie erfahren außerdem, wie Sie Verbindungszeichenfolgen angeben und ein Datenbankschema bereitstellen.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: e0179429-13ce-4d23-ae27-e6419de0a575
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 569262aece2d8b1212704c0b2ba443c6c63f2b07
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018410"
---
# <a name="how-to-configure-sql-server-unit-test-execution"></a>Gewusst wie: Konfigurieren der Ausführung von SQL Server-Komponententests

Indem Sie das Testprojekt konfigurieren, können Sie mehrere Einstellungen zur Steuerung der Ausführung von SQL Server-Komponententests angeben. Diese Konfigurationseinstellungen werden in der Datei app.config des Testprojekts gespeichert. Wenn Sie diese Datei direkt bearbeiten, werden die neuen Werte im Dialogfeld Testkonfiguration angezeigt.  
  
Eine Projektmappe kann mehrere Testprojekte enthalten. Jedes Testprojekt enthält eine Datei app.config (also einen Satz von Konfigurationseinstellungen). Folglich kann eine Projektmappe verschiedene Sätze von Komponententests (einen Satz für jedes Testprojekt) enthalten, deren Ausführung unterschiedlich konfiguriert ist.  
  
Diese Einstellungen steuern, auf welche Weise der Test mit der zu testenden Datenbank verbunden wird und wie ein Schema aus einem Datenbankprojekt für die Datenbank bereitgestellt wird:  
  
-   **Datenbankverbindungen**: Mit dieser Einstellung geben Sie die Verbindungszeichenfolgen an, durch die eine Verbindung mit der Datenbank hergestellt wird, die Sie testen möchten. Weitere Informationen finden Sie unter [Angeben von Verbindungszeichenfolgen](#SpecifyConnectionStrings).  
  
-   **Schemabereitstellung**: Ein Datenbankprojekt ist eine Offlinedarstellung der Datenbank. Im Datenbankprojekt wird die Struktur der Datenbankobjekte dargestellt, ohne dass es Daten enthält. Nachdem Sie in einem Datenbankprojekt Änderungen am Schema vorgenommen haben, können Sie sie in einer realen Datenbank testen. In der Schemabereitstellungsphase werden die zu testenden Datenbankobjekte aus dem Datenbankprojekt in die Datenbank kopiert, für die die Tests ausgeführt werden. Weitere Informationen zur Schemabereitstellung finden Sie unter [Bereitstellen eines Datenbankschemas](#DeployingDBSchema).  
  
    > [!NOTE]  
    > Tests werden nicht im Projektmappenordner, sondern in einem separaten Ordner auf der lokalen Festplatte ausgeführt. Obwohl verschiedene Aspekte der Testbereitstellung konfiguriert werden können, müssen diese für Komponententests in der Regel nicht konfiguriert werden. Weitere Informationen zur Testbereitstellung finden Sie unter [Ausführen von Tests](/previous-versions/visualstudio/visual-studio-2010/dd286680(v=vs.100)).  
  
## <a name="specify-connection-strings"></a><a name="SpecifyConnectionStrings"></a>Angeben von Verbindungszeichenfolgen  
  
#### <a name="to-specify-database-connection-strings"></a>So geben Sie Datenbankverbindungszeichenfolgen an  
  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Komponententestprojekt, und klicken Sie auf **SQL Server-Testkonfiguration**.  
  
    Das Dialogfeld **SQL Server-Testkonfiguration – "<projectname>"** wird angezeigt.  
  
2.  Unter **Datenbankverbindungen** können Sie wie folgt verfahren:  
  
    -   Klicken Sie auf die Datenbankverbindung, für die Komponententests ausgeführt werden sollen.  
  
    -   Aktivieren Sie das Kontrollkästchen **Zum Überprüfen von Komponententests sekundäre Datenverbindung verwenden**, und klicken Sie in der Liste auf eine Datenbankverbindung, wenn die Testausführung unter Verwendung einer anderen Datenbankverbindung überprüft werden soll.  
  
    -   Klicken Sie auf **Neue Verbindung**, um einer der Listen eine Verbindung hinzuzufügen. Sie können auch auf **Verbindung bearbeiten** klicken, um die Einstellungen einer vorhandenen Verbindung zu ändern.  
  
    In diesem Schritt wird die `ExecutionContext`-Verbindungszeichenfolge erstellt, mit der das Testskript im Komponententest ausgeführt wird. Wenn Sie auch eine sekundäre Verbindung angeben, wird zusätzlich die `PrivilegedContext`-Verbindungszeichenfolge erstellt. Mithilfe dieser Verbindung werden Interaktionen mit der Datenbank außerhalb des Testskripts im Komponententest getestet. Weitere Informationen finden Sie unter [Übersicht über Verbindungszeichenfolgen und Berechtigungen](../ssdt/overview-of-connection-strings-and-permissions.md).  
  
3.  Klicken Sie auf **OK**, um das Dialogfeld **SQL Server-Testkonfiguration – "<projectname>"** zu schließen.  
  
4.  Erstellen Sie das Testprojekt erneut, um die Konfigurationsänderungen anzuwenden.  
  
## <a name="deploy-a-database-schema"></a><a name="DeployingDBSchema"></a>Bereitstellen eines Datenbankschemas  
  
#### <a name="to-deploy-to-a-database-the-schema-of-a-database-project"></a>So stellen Sie das Schema eines Datenbankprojekts für eine Datenbank bereit  
  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Datenbankprojekt, und klicken Sie dann auf **Erstellen**.  
  
    Bei der Erstellung des Datenbankprojekts wird ein Transact\-SQL-Skript generiert. Durch die Ausführung dieses Skripts für eine Datenbank wird die Struktur des Datenbankprojekts in dieser Datenbank neu erstellt.  
  
2.  Wählen Sie das Testprojekt aus, das Sie konfigurieren möchten.  
  
3.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Komponententestprojekt, und klicken Sie auf **SQL Server-Testkonfiguration**.  
  
    Das Dialogfeld **SQL Server-Testkonfiguration – "<projectname>"** wird angezeigt.  
  
4.  Sie können unter **Bereitstellung** wie folgt verfahren:  
  
    -   Aktivieren Sie das Kontrollkästchen **Datenbankprojekte vor dem Ausführen von Tests automatisch bereitstellen**, um sicherzustellen, dass für alle am Datenbankprojekt vorgenommenen Schemaänderungen ein Commit ausgeführt wird, bevor die Tests ausgeführt werden.  
  
    -   Klicken Sie unter **Datenbankprojekt** auf das bereitzustellende Datenbankprojekt, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten, um ein anderes Projekt zu suchen. Datenbankprojektdateien haben die Erweiterung DBPROJ.  
  
    -   Klicken Sie unter **Bereitstellungskonfiguration** auf die Projektkonfiguration, für die die Bereitstellung erfolgen soll. Die Optionen lauten **Debug**, **Standard** oder **Release**. Wenn Sie jedoch eine Konfiguration für Komponententests erstellen, wird diese Konfiguration zusätzlich als Option angezeigt.  
  
5.  Klicken Sie auf **OK**, um das Dialogfeld **SQL Server-Testkonfiguration – "<projectname>"** zu schließen.  
  
    Zu Beginn des Testlaufs wird das in Schritt 1 generierte Transact\-SQL-Skript ausgeführt. Durch diese Aktion wird das Schema auf der Zieldatenbank bereitgestellt.  
  
6.  Erstellen Sie das Komponententestprojekt erneut, um die Konfigurationsänderungen anzuwenden.  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen und Definieren von SQL Server-Komponententests](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Überprüfen des Datenbankcodes mithilfe von SQL Server-Komponententests](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
