---
title: Ausführen von SQL Server-Komponententests aus Team Foundation Build
description: Hier erfahren Sie, wie Sie SQL Server-Komponententests über den Team Foundation-Build ausführen. Sie erfahren, wie Sie eine Builddefinition erstellen und Komponententests in einem automatisierten Testlauf ausführen.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 24f5b85d-d6f9-415f-b09f-933b78dc0b67
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2b87720ba7a4cbdb531cd19fe23be7d48d9a07ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018130"
---
# <a name="how-to-run-sql-server-unit-tests-from-team-foundation-build"></a>Gewusst wie: Ausführen von SQL Server-Komponententests aus Team Foundation Build

Sie können Team Foundation Build verwenden, um SQL Server-Komponententests im Rahmen eines Buildüberprüfungstests (Build Verification Test, BVT) auszuführen. Komponententests können so konfiguriert werden, dass sie die Datenbank bereitstellen, Testdaten generieren und ausgewählte Tests ausführen. Wenn Sie mit Team Foundation Build nicht vertraut sind, sollten Sie die folgenden Informationen lesen, bevor Sie die Schritte in diesem Thema ausführen:  
  
-   [Erstellen und Definieren von SQL Server-Komponententests](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
  
-   [Vorgehensweise: Konfigurieren und Ausführen von geplanten Tests nach dem Erstellen der Anwendung](/previous-versions/visualstudio/visual-studio-2010/ms182465(v=vs.100))  
  
-   [Erstellen oder Bearbeiten einer Builddefinition](/previous-versions/visualstudio/visual-studio-2010/ms181716(v=vs.100))  
  
Bevor Sie diese Schritte ausführen, müssen Sie erst die Arbeitsumgebung anhand der folgenden Aufgaben konfigurieren:  
  
-   Installieren Sie Team Foundation Build sowie die Team Foundation-Versionskontrolle. Team Foundation Build und Team Foundation-Versionskontrolle müssen u. U. auf unterschiedlichen Computern installiert werden.  
  
-   Installieren Sie die Microsoft SQL Server Data Tools-Buildhilfsprogramme auf demselben Computer wie Team Foundation Build. Zum Installieren der SQL Server Data Tools-Buildhilfsprogramme führen Sie zunächst einen Administratorinstallationspfad aus. Weitere Informationen zu Administratorinstallationspfaden finden Sie unter [Installieren von SQL Server Data Tools](./download-sql-server-data-tools-ssdt.md). Installieren Sie anschließend SSDTBuildUtilties.msi von dem für den Administratorinstallationspunkt verwendeten Pfad (/location) auf dem Buildserver.  
  
-   Stellen Sie eine Verbindung mit einer Instanz von Visual Studio Team Foundation Server her.  
  
Nachdem Sie die Arbeitsumgebung konfiguriert haben, müssen Sie die folgenden Schritte ausführen:  
  
1.  Erstellen Sie ein Datenbankprojekt.  
  
2.  Importieren oder erstellen Sie das Schema und die Objekte für das Datenbankprojekt.  
  
3.  Konfigurieren Sie die Eigenschaften des Datenbankprojekts für den Build- und Bereitstellungsprozess.  
  
4.  Erstellen Sie mindestens einen Komponententest.  
  
5.  Fügen Sie der Versionskontrolle die Projektmappe hinzu, in der das Datenbankprojekt und das Komponententestprojekt enthalten sind, und checken Sie alle Dateien ein.  
  
Die Schritte in diesem Thema verdeutlichen, wie Sie eine Builddefinition erstellen, um Komponententests im Rahmen eines automatisierten Testlaufs auszuführen:  
  
1.  [Konfigurieren von Testeinstellungen zum Ausführen von Datenbankkomponententests unter einem x64-Build-Agent](#ConfigureX64)  
  
2.  [Zuweisen von Tests zu einer Testkategorie (optional)](#CreateATestList)  
  
3.  [Ändern des Testprojekts](#ModifyTestProject)  
  
4.  [Einchecken der Projektmappe](#CheckInTheTestList)  
  
5.  [Erstellen einer Builddefinition](#CreateBuildDef)  
  
6.  [Ausführen der neuen Builddefinition](#RunBuild)  
  
**Ausführen von SQL Server-Komponententests auf einem Buildcomputer**  
  
Wenn Sie Komponententests auf einem Buildcomputer ausführen, sind die Komponententests möglicherweise nicht in der Lage, die Datenbankprojektdateien (.sqlproj) zu finden. Dieses Problem tritt auf, weil die Datei app.config über relative Pfade auf diese Dateien verweist. Darüber hinaus kann bei den Komponententests ein Fehler auftreteten, wenn die Instanz von SQL Server, die zum Ausführen der Komponententests verwendet werden soll, nicht gefunden wird. Dieses Problem kann auftreten, wenn die in der Datei app.config gespeicherten Verbindungszeichenfolgen auf dem Buildcomputer ungültig sind.  
  
Zur Lösung dieser Probleme müssen Sie einen Überschreibungsabschnitt in der Datei app.config angeben, durch den app.config mit einer Konfigurationsdatei überschrieben wird, die spezifisch für die Team Foundation Build-Umgebung ist. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Ändern des Testprojekts](#ModifyTestProject).  
  
## <a name="configure-test-settings-to-run-sql-server-unit-tests-on-an-x64-build-agent"></a><a name="ConfigureX64"></a>Konfigurieren von Testeinstellungen zum Ausführen von SQL Server-Komponententests unter einem x64-Build-Agent  
Bevor Sie Komponententests unter einem x64-Build-Agent ausführen können, müssen Sie die Testeinstellungen konfigurieren, um die Hostprozessplattform zu wechseln.  
  
#### <a name="to-specify-the-host-process-platform"></a>So geben Sie die Hostprozessplattform an  
  
1.  Öffnen Sie die Projektmappe mit dem Testprojekt, für das Sie Einstellungen konfigurieren möchten.  
  
2.  Doppelklicken Sie im **Projektmappen-Explorer** im Ordner **Projektmappenelemente** auf die Datei **Local.testsettings**.  
  
    Das Dialogfeld **Testeinstellungen** wird angezeigt.  
  
3.  Klicken Sie in der Liste auf **Hosts**.  
  
4.  Klicken Sie im Detailbereich unter **Hostprozessplattform** auf **MSIL**, um die Tests zur Ausführung unter einem x64-Build-Agent zu konfigurieren.  
  
5.  Klicken Sie auf **Anwenden**.  
  
## <a name="assign-tests-to-a-test-category-optional"></a><a name="CreateATestList"></a>Zuweisen von Tests zu einer Testkategorie (optional)  
Wenn Sie eine Builddefinition zum Ausführen von Komponententests erstellen, geben Sie normalerweise mindestens eine Testkategorie an. Alle Tests in den angegebenen Kategorien werden zusammen mit dem Build ausgeführt.  
  
#### <a name="to-assign-tests-to-a-test-category"></a>So weisen Sie einer Testkategorie Tests zu  
  
1.  Öffnen Sie das Fenster **Testansicht**.  
  
2.  Wählen Sie einen Test aus.  
  
3.  Klicken Sie im Eigenschaftenbereich auf **Testkategorien**, und klicken Sie auf die Auslassungspunkte (...) in der äußersten rechten Spalte.  
  
4.  Geben Sie im Fenster **Testkategorie** im Feld **Neue Kategorie hinzufügen** einen Namen für die neue Testkategorie an.  
  
5.  Klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **OK**.  
  
    Die neue Testkategorie wird dem Test zugewiesen und den anderen Tests über ihre Eigenschaften zur Verfügung gestellt.  
  
## <a name="modify-the-test-project"></a><a name="ModifyTestProject"></a>Ändern des Testprojekts  
Team Foundation Build erstellt standardmäßig eine Konfigurationsdatei aus der Datei app.config des Projekts, wenn das Komponententestprojekt erstellt wird. Der Pfad zum Datenbankprojekt wird als relativer Pfad in der Datei „app.config“ gespeichert. Die relativen Pfade, die in Visual Studio funktionieren, sind nicht kompatibel, weil Team Foundation Build die erstellten Dateien – je nachdem, wo die Komponententests ausgeführt werden – an verschiedenen Orten ablegt. Zusätzlich enthält die Datei app.config die Verbindungszeichenfolgen, in denen die zu testende Datenbank angegeben ist. Darüber hinaus benötigen Sie eine separate Datei app.config für Team Foundation Build, wenn die Komponententests eine Verbindung mit einer anderen Datenbank als der Datenbank herstellen müssen, die bei der Erstellung des Testprojekts verwendet wurde. Durch die in den folgenden Schritten vorgenommenen Änderungen können Sie das Testprojekt und den Buildserver so einrichten, dass Team Foundation Build eine andere Konfiguration verwendet.  
  
> [!IMPORTANT]  
> Sie müssen diese Schritte für jedes Testprojekt (.vbproj oder .vsproj) ausführen.  
  
#### <a name="to-specify-an-appconfig-file-for-team-foundation-build"></a>So geben Sie eine Datei "app.config" für Team Foundation Build an  
  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Datei „app.config“, und klicken Sie auf **Kopieren**.  
  
2.  Klicken Sie mit der rechten Maustaste auf das Testprojekt, und klicken Sie auf **Einfügen**.  
  
3.  Klicken Sie mit der rechten Maustaste auf die Datei **Kopie von app.config**, und klicken Sie auf „Umbenennen“.  
  
4.  Geben Sie _BuildComputer_ **.sqlunitttest.config** ein, und drücken Sie die EINGABETASTE. Dabei entspricht *BuildComputer* dem Namen des Computers, auf dem der Build-Agent ausgeführt wird.  
  
5.  Doppelklicken Sie auf „*BuildComputer*.sqlunitttest.config“.  
  
    Die Konfigurationsdatei wird im Editor geöffnet.  
  
6.  Ändern Sie den relativen Pfad zu der Datei mit der Erweiterung .sqlproj, indem Sie eine Ordnerebene für den Ordner Quellen und einen Unterordner mit dem Namen der Projektmappe hinzufügen. Beispiel: Die Konfigurationsdatei enthält anfänglich den folgenden Eintrag:  
  
    ```  
    <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database3\Database3.sqlproj"      Configuration="Debug" />  
    ```  
  
    Aktualisieren Sie die Datei wie folgt:  
  
    ```  
    <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database3\Database3.sqlproj"      Configuration="Debug" />  
    ```  
  
    Wenn Sie fertig sind, sollte die Datei „*BuildComputer*.sqlunitttest.config“ wie im folgenden Beispiel für Visual Studio 2010 aussehen:  
  
    ```  
    <SqlUnitTesting_VS2010>  
        <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database4\Database4.sqlproj"  
            Configuration="Debug" />  
        <DataGeneration ClearDatabase="true" />  
        <ExecutionContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
            CommandTimeout="30" />  
        <PrivilegedContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
            CommandTimeout="30" />  
    </SqlUnitTesting_VS2010>  
    ```  
  
    Oder, wenn Sie Visual Studio 2012 verwenden:  
  
    ```  
    <SqlUnitTesting_VS2012>  
            <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database4\Database4.sqlproj"  
                Configuration="Debug" />  
            <DataGeneration ClearDatabase="true" />  
            <ExecutionContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
                CommandTimeout="30" />  
            <PrivilegedContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
                CommandTimeout="30" />  
        </SqlUnitTesting_VS2012>  
    ```  
  
7.  Aktualisieren Sie das ConnectionString-Attribut für ExecutionContext und PrivilegedContext, um Verbindungen mit der Zieldatenbank anzugeben, die zur Bereitstellung verwendet werden soll.  
  
8.  Klicken Sie im Menü **Datei** auf **Alle speichern**.  
  
9. Doppelklicken Sie im Projektmappen-Explorer auf app.config.  
  
10. Fügen Sie im Editor für jeden \<SqlUnitTesting_*VSVersion*>-Knoten `AllowConfigurationOverride="true"` hinzu. Beispiel:  
  
    ```  
    -- Update SqlUnitTesting_VS2010 node to:  
    <SqlUnitTesting_VS2010 AllowConfigurationOverride="true">   
  
    -- Update SqlUnitTesting_VS2012 node to:  
    <SqlUnitTesting_VS2012 AllowConfigurationOverride="true">  
    ```  
  
    Durch diese Änderung ermöglichen Sie Team Foundation Build die Verwendung der von Ihnen erstellten Ersatzkonfigurationsdatei.  
  
11. Klicken Sie im Menü **Datei** auf **Alle speichern**.  
  
    Als Nächstes müssen Sie Local.testsettings aktualisieren, um die angepasste Konfigurationsdatei einzuschließen.  
  
#### <a name="to-customize-localtestsettings-to-deploy-the-customized-configuration-file"></a>So passen Sie Local.testsettings für die Bereitstellung der angepassten Konfigurationsdatei an  
  
1.  Doppelklicken Sie im Projektmappen-Explorer auf Local.testsettings.  
  
    Das Dialogfeld **Testeinstellungen** wird angezeigt.  
  
2.  Klicken Sie in der Liste der Kategorien auf **Bereitstellung**.  
  
3.  Aktivieren Sie das Kontrollkästchen **Bereitstellung aktivieren**.  
  
4.  Klicken Sie auf **Datei hinzufügen**.  
  
5.  Geben Sie im Dialogfeld **Bereitstellungsdateien hinzufügen** die erstellte Datei „*BuildComputer*.sqlunitttest.config“ an.  
  
6.  Klicken Sie auf **Anwenden**.  
  
7.  Klicken Sie auf **Schließen**.  
  
8.  Klicken Sie im Menü **Datei** auf **Alle speichern**.  
  
    Als Nächstes checken Sie die Projektmappe in die Versionskontrolle ein.  
  
## <a name="check-in-the-solution"></a><a name="CheckInTheTestList"></a>Einchecken der Projektmappe  
In dieser Prozedur checken Sie alle Dateien der Projektmappe ein. Diese Dateien enthalten die Testmetadatendatei der Projektmappe, in der die Testkategoriezuweisungen und -tests enthalten sind. Sobald Sie Testinhalt hinzufügen, löschen, neu anordnen oder ändern, wird die Testmetadatendatei automatisch anhand dieser Änderungen aktualisiert.  
  
> [!NOTE]  
> In diesem Verfahren werden die Schritte beschrieben, die bei Verwendung der Team Foundation-Versionskontrolle gelten. Wenn Sie eine andere Versionskontrollsoftware verwenden, müssen Sie die für diese Software geltenden Schritte ausführen.  
  
#### <a name="to-check-in-the-solution"></a>So checken Sie die Projektmappe ein  
  
1.  Stellen Sie eine Verbindung mit einem Computer her, auf dem Team Foundation Server ausgeführt wird.  
  
    Weitere Informationen finden Sie unter [Verwenden des Quellcodeverwaltungs-Explorers](/previous-versions/visualstudio/visual-studio-2010/ms181370(v=vs.100)).  
  
2.  Wenn die Projektmappe nicht bereits in der Quellcodeverwaltung enthalten ist, fügen Sie sie der Quellcodeverwaltung hinzu.  
  
    Weitere Informationen finden Sie unter [Hinzufügen eines Projekts oder einer Projektmappe zur Versionskontrolle](/previous-versions/visualstudio/visual-studio-2010/ms181374(v=vs.100)).  
  
3.  Klicken Sie auf **Ansicht** und dann auf **Anstehende Eincheckvorgänge**.  
  
4.  Checken Sie alle Dateien der Projektmappe ein.  
  
    Weitere Informationen finden Sie unter [Einchecken ausstehender Änderungen](/previous-versions/visualstudio/visual-studio-2010/ms181411(v=vs.100)).  
  
    > [!NOTE]  
    > Möglicherweise unterliegen die Schritte einem spezifischen Teamprozess, der regelt, wie automatisierte Tests erstellt und verwaltet werden. Beispielsweise kann der Prozess erforderlich machen, dass Sie den Build lokal überprüfen, bevor Sie diesen Code zusammen mit den Tests einchecken, die für den Code ausgeführt werden sollen.  
  
    Im **Projektmappen-Explorer** wird neben jeder Datei ein Vorhängeschloss als Symbol angezeigt, um anzugeben, dass sie eingecheckt ist. Weitere Informationen finden Sie unter [Anzeigen der Eigenschaften von Dateien und Ordnern, die der Versionskontrolle unterliegen](/previous-versions/visualstudio/visual-studio-2010/ms245468(v=vs.100)).  
  
    Die Tests stehen Team Foundation Build zur Verfügung. Sie können jetzt eine Builddefinition erstellen, in der die auszuführenden Tests enthalten sind.  
  
## <a name="create-a-build-definition"></a><a name="CreateBuildDef"></a>Erstellen einer Builddefinition  
  
#### <a name="to-create-a-build-definition"></a>So erstellen Sie eine Builddefinition  
  
1.  Klicken Sie im Team Explorer auf das Teamprojekt, klicken Sie mit der rechten Maustaste auf den Knoten **Builds**, und klicken Sie dann auf **Neue Builddefinition**.  
  
    Das Fenster **Neue Builddefinition** wird angezeigt.  
  
2.  Geben Sie unter **Builddefinitionsname** den Namen ein, den Sie für die Builddefinition verwenden möchten.  
  
3.  Klicken Sie auf der Navigationsleiste auf **Buildstandardwerte**.  
  
4.  Geben Sie unter **Buildausgabe in den folgenden Ablageordner kopieren (UNC-Pfad, z.B. „\\\Server\Freigabe“)** einen Ordner für die Buildausgabe an.  
  
    Sie können einen freigegebenen Ordner auf dem lokalen Computer bzw. einen beliebigen Netzwerkpfad angeben, für den der Buildprozess über eine Zugriffsberechtigung verfügt.  
  
5.  Klicken Sie auf der Navigationsleiste auf **Verarbeiten**.  
  
6.  Klicken Sie in der Gruppe **Erforderlich** unter **Zu erstellende Elemente** auf die Schaltfläche zum Durchsuchen (...).  
  
7.  Klicken Sie im Dialogfeld **Buildprojektlisten-Editor** auf **Hinzufügen**.  
  
8.  Geben Sie die Projektmappendatei (.sln) an, die Sie der Versionskontrolle weiter oben in dieser exemplarischen Vorgehensweise hinzugefügt haben, und klicken Sie auf **OK**.  
  
    Die Projektmappe wird in der Liste **Zu erstellende Projektmappen- oder Projektdateien** angezeigt.  
  
9. Klicken Sie auf **OK**.  
  
10. Geben Sie in der Gruppe **Standard** unter **Automatisierte Tests** die Tests an, die Sie ausführen möchten. Standardmäßig werden die Tests ausgeführt, die in der Projektmappe in Dateien mit dem Namen „\*test\*.dll“ enthalten sind.  
  
11. Klicken Sie im Menü **Datei** auf *Projektname* **speichern**.  
  
    Die Builddefinition ist jetzt erstellt. Als Nächstes ändern Sie das Testprojekt.  
  
## <a name="run-the-new-build-definition"></a><a name="RunBuild"></a>Ausführen der neuen Builddefinition  
  
#### <a name="to-run-the-new-build-type"></a>So führen Sie den neuen Buildtyp aus  
  
1.  Erweitern Sie im Team-Explorer erst den Teamprojektknoten und dann den Knoten Builds. Klicken Sie mit der rechten Maustaste auf die Builddefinition, die Sie ausführen möchten, und klicken Sie dann auf Neuen Build in Warteschlange.  
  
    Das Dialogfeld **Build {** _TeamProjectName_ **} zur Warteschlange hinzufügen** wird mit einer Liste aller vorhandenen Buildtypen angezeigt.  
  
2.  Klicken Sie unter **Builddefinition** ggf. auf die neue Builddefinition.  
  
3.  Überprüfen Sie, ob die Werte in den Feldern **Builddefinition**, **Build-Agent** und **Ablageordner für diesen Build** richtig sind, und klicken Sie auf **Warteschlange**.  
  
    Die Registerkarte **In Warteschlange gestellt** wird im **Build-Explorer** angezeigt. Weitere Informationen finden Sie unter [Verwalten und Anzeigen abgeschlossener Builds (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms181730(v=vs.100)) oder [Verwalten von Builds im Build-Explorer (Visual Studio 2012)](/previous-versions/ms181732(v=vs.140)).  
  
## <a name="see-also"></a>Weitere Informationen  
[Ausführen von SQL Server-Komponententests](../ssdt/running-sql-server-unit-tests.md)  
[Erstellen oder Bearbeiten einer Builddefinition](/previous-versions/visualstudio/visual-studio-2010/ms181716(v=vs.100))  
[Einreihen eines Builds in die Warteschlange](/previous-versions/visualstudio/visual-studio-2010/ms181722(v=vs.100))  
[Überwachen des Status eines Builds während der Ausführung](/previous-versions/visualstudio/visual-studio-2010/ms181724(v=vs.100))  
