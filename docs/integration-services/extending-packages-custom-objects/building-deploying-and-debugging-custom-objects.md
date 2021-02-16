---
description: Erstellen, Bereitstellen und Debuggen von benutzerdefinierten Objekten
title: Erstellen, Bereitstellen und Debuggen von benutzerdefinierten Objekten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom objects [Integration Services]
ms.assetid: b03685bc-5398-4c3f-901a-1219c1098fbe
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bb634702b48de70d505851c53ce3f1f6562547a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348948"
---
# <a name="building-deploying-and-debugging-custom-objects"></a>Erstellen, Bereitstellen und Debuggen von benutzerdefinierten Objekten

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Nachdem Sie den Code für ein benutzerdefiniertes Objekt für [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] geschrieben haben, müssen Sie die Assembly erstellen, bereitstellen und in [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer integrieren, um sie für die Nutzung in Paketen verfügbar zu machen, sie zu testen und zu debuggen.  
  
##  <a name="steps-in-building-deploying-and-debugging-a-custom-object-for-integration-services"></a><a name="top"></a> Schritte zum Erstellen, Bereitstellen und Debuggen eines benutzerdefinierten Objekts für Integration Services  
 Sie haben bereits die benutzerdefinierte Funktionalität für das Objekt geschrieben. Jetzt müssen Sie es testen und Benutzern zur Verfügung stellen. Die Schritte sind für alle Typen benutzerdefinierter Objekte, die Sie für [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] erstellen können, ähnlich.  
  
 Im Folgenden werden die einzelnen Schritte zum Erstellen, Bereitstellen und Testen erläutert.  
  
1.  [Signieren](#signing) Sie die zu generierende Assembly mit einem starken Namen.  
  
2.  [Erstellen](#building) Sie die Assembly.  
  
3.  [Stellen Sie die Assembly bereit](#deploying), indem Sie sie verschieben oder in den entsprechenden [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Ordner kopieren.  
  
4.  [Installieren](#installing) Sie die Assembly im globalen Assemblycache (Global Assembly Cache, GAC).  
  
     Das Objekt wird der Toolbox automatisch hinzugefügt.  
  
5.  [Beheben](#troubleshooting) Sie ggf. Probleme bei der Bereitstellung.  
  
6.  [Testen](#testing) und debuggen Sie den Code.  
  
 Sie können den SSIS-Designer in den SQL Server Data Tools (SSDT) jetzt verwenden, um Pakete zu erstellen, zu verwalten und auszuführen, die auf verschiedene [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Versionen ausgerichtet sind. Weitere Informationen zum Umfang dieser Verbesserung Ihrer benutzerdefinierten Erweiterungen finden Sie unter [Getting your SSIS custom extensions to be supported by the multi-version support of SSDT 2015 for SQL Server 2016 (Unterstützung benutzerdefinierter SSIS-Erweiterungen dank der Unterstützung mehrerer Versionen von SSDT 2015 für SQL Server 2016)](https://blogs.msdn.microsoft.com/ssis/2016/04/19/getting-your-ssis-custom-extensions-to-be-supported-by-the-multi-version-support-of-ssdt-2015-for-sql-server-2016/).  
  
##  <a name="signing-the-assembly"></a><a name="signing"></a> Signieren der Assembly  
 Wenn eine Assembly freigegeben werden soll, muss sie im GAC installiert sein. Nachdem die Assembly dem globalen Assemblycache hinzugefügt wurden, kann die Assembly von Anwendungen wie [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] genutzt werden. Für den globalen Assemblycache muss die Assembly mit einem starken Namen signiert werden, um sicherzustellen, dass eine Assembly global eindeutig ist. Eine Assembly mit starkem Namen verfügt über einen vollqualifizierten Namen, der den Namen, öffentlichen Schlüssel und die Versionsnummer der Assembly umfasst. Anhand dieser Informationen wird die Assembly von der Laufzeit gefunden und von anderen Assemblys mit demselben Namen unterschieden.  
  
 Um eine Assembly mit einem starken Namen zu signieren, müssen Sie zuerst über ein Schlüsselpaar verfügen oder eines erstellen, das aus einem öffentlichen und einem privaten Schlüssel besteht. Dieses kryptografische Schlüsselpaar wird während der Erstellung zum Generieren einer Assembly mit starkem Namen verwendet.  
  
 Weitere Informationen zu starken Namen und zu den Schritten zum Signieren einer Assembly finden Sie in den folgenden Themen in der [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK-Dokumentation:  
  
-   Assemblys mit starkem Namen  
  
-   Erstellen eines Schlüsselpaars  
  
-   Signieren einer Assembly mit einem starken Namen  
  
 Sie können die Assembly mühelos in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] beim Erstellen mit einem starken Namen signieren. Wählen Sie im Dialogfeld **Projekteigenschaften** die Registerkarte **Signierung** aus. Aktivieren Sie die Option **Assembly signieren**, und geben Sie dann den Pfad der Schlüsseldatei (.snk) an.  
  
##  <a name="building-the-assembly"></a><a name="building"></a> Erstellen der Assembly  
 Nach der Signierung des Projekts müssen Sie das Projekt oder die Projektmappe erstellen bzw. neu erstellen, indem Sie die im **Build**-Menü von [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] verfügbaren Befehle verwenden. Die Lösung enthält möglicherweise ein separates Projekt für eine benutzerdefinierte Benutzeroberfläche, die ebenfalls mit einem starken Namen signiert sein muss und zur gleichen Zeit erstellt werden kann.  
  
 Der einfachste Weg, um die nächsten zwei Schritte (Bereitstellen der Assembly und Installieren im globalen Assemblycache) auszuführen, besteht darin, ein Skript für diese Schritte als Postbuildereignis in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] zu erstellen. Buildereignisse stehen unter „Projekteigenschaften“ auf der Seite **Kompilieren** für ein [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]-Projekt und auf der Seite **Buildereignisse** für ein C#-Projekt zur Verfügung. Der vollständige Pfad ist für Eingabeaufforderungs-Hilfsprogramme wie **gacutil.exe** erforderlich. Pfade mit Leerzeichen und Makros wie $(TargetPath), mit denen Pfade mit Leerzeichen erweitert werden, müssen von Anführungszeichen umschlossen werden.  
  
 Nachfolgend finden Sie ein Beispiel einer Postbuildereignis-Befehlszeile für einen benutzerdefinierten Protokollanbieter:  
  
```cmd
"C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0A\bin\NETFX 4.0 Tools\gacutil.exe" -u $(TargetName)  
"C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0A\bin\NETFX 4.0 Tools\gacutil.exe" -i $(TargetFileName)  
copy $(TargetFileName) "C:\Program Files\Microsoft SQL Server\130\DTS\LogProviders "  
```  
  
##  <a name="deploying-the-assembly"></a><a name="deploying"></a> Bereitstellen der Assembly  
 Der [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer sucht die benutzerdefinierten Objekte, die in Paketen verwendet werden können, indem die Dateien in mehreren Ordnern aufgezählt werden, die bei der Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] erstellt werden. Wenn die Standardeinstellungen für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Installation verwendet werden, befinden sich die Ordner im Verzeichnis **C:\Programme\Microsoft SQL Server\130\DTS**. Wenn Sie jedoch für Ihr benutzerdefiniertes Objekt ein Setupprogramm erstellen, sollten Sie den Wert des **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS\Setup\DtsPath**-Registrierungsschlüssels überprüfen, um den Speicherort des Ordners zu überprüfen.  
  
> [!NOTE]  
>  Informationen zur Bereitstellung von benutzerdefinierten Komponenten, die mit der Unterstützung mehrerer Versionen in SQL Server Data Tools zusammenarbeiten, finden Sie unter [Getting your SSIS custom extensions to be supported by the multi-version support of SSDT 2015 for SQL Server 2016 (Unterstützung benutzerdefinierter SSIS-Erweiterungen dank der Unterstützung mehrerer Versionen von SSDT 2015 für SQL Server 2016)](https://blogs.msdn.microsoft.com/ssis/2016/04/19/getting-your-ssis-custom-extensions-to-be-supported-by-the-multi-version-support-of-ssdt-2015-for-sql-server-2016/).  
  
 Sie können die Assembly auf zwei Arten im Ordner platzieren:  
  
-   Verschieben oder kopieren Sie die kompilierte Assembly nach der Erstellung in den entsprechenden Ordner. (Sie können auch einfach den Kopierbefehl in ein Postbuildereignis einschließen.)  
  
-   Erstellen Sie die Assembly direkt im entsprechenden Ordner.  
  
 Die folgenden Bereitstellungsordner unter **C:\Programme\Microsoft SQL Server\130\DTS** werden für die unterschiedlichen Typen benutzerdefinierter Objekte verwendet:  
  
|Benutzerdefiniertes Objekt|Bereitstellungsordner|  
|-------------------|-----------------------|  
|Aufgabe|Aufgaben|  
|Ziel-Editor für Dimensionsverarbeitung|Verbindungen|  
|Protokollanbieter|LogProviders|  
|Datenflusskomponente|PipelineComponents|  
  
> [!NOTE]  
>  Assemblys werden in diese Ordner kopiert, um die Aufzählung der verfügbaren Tasks, Verbindungs-Manager usw. zu unterstützen. Sie müssen daher keine Assemblys, die nur die benutzerdefinierte Benutzeroberfläche für benutzerdefinierte Objekte enthalten, in diesen Ordnern bereitstellen.  
  
##  <a name="installing-the-assembly-in-the-global-assembly-cache"></a><a name="installing"></a> Installieren der Assembly im globalen Assemblycache  
 Verwenden Sie das Befehlszeilentool **gacutil.exe**, oder ziehen Sie die Assemblys in das Verzeichnis `%system%\assembly`, um die Taskassembly im globalen Assemblycache (GAC) zu installieren. Sie können auch ganz einfach den Aufruf von **gacutil.exe** in ein Postbuildereignis einschließen.  
  
 Der folgende Befehl installiert eine Komponente namens *MyTask.dll* in den GAC mithilfe von **gacutil.exe**.  
  
 `gacutil /iF MyTask.dll`  
  
 Sie müssen [!INCLUDE[ssIS](../../includes/ssis-md.md)] Designer nach der Installation einer neuer Version des benutzerdefinierten Objekts schließen und erneut öffnen. Wenn Sie frühere Versionen des benutzerdefinierten Objekts im globalen Assemblycache installiert haben, müssen Sie sie vor der Installation der neuen Version entfernen. Führen Sie **gacutil.exe** aus, und geben Sie den Assemblynamen mit der `/u`-Option an, um eine Assembly zu deinstallieren.  
  
 Weitere Informationen zum globalen Assemblycache finden Sie unter Globales Assemblycache-Tool (Gactutil.exe) in den [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]-Tools.  
  
##  <a name="troubleshooting-the-deployment"></a><a name="troubleshooting"></a> Behandeln von Problemen bei der Bereitstellung  
 Wenn das benutzerdefinierte Objekt in der **Toolbox** oder der Liste verfügbarer Objekte angezeigt wird, Sie es jedoch keinem Paket hinzufügen können, versuchen Sie Folgendes:  
  
1.  Suchen Sie im globalen Assemblycache nach mehreren Versionen der Komponente. Wenn im globalen Assemblycache mehrere Versionen der Komponente vorliegen, kann der Designer u. U. die Komponente nicht laden. Löschen Sie alle Instanzen der Assembly aus dem globalen Assemblycache, und fügen Sie die Assembly wieder hinzu.  
  
2.  Stellen Sie sicher, dass nur eine einzelne Instanz der Assembly im Bereitstellungsordner existiert.  
  
3.  Aktualisieren Sie die Toolbox.  
  
4.  Fügen Sie [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] an **devenv.exe** an, und legen Sie einen Breakpoint fest, um den Initialisierungscode zu durchlaufen und sicherzustellen, dass keine Ausnahmen auftreten.  
  
##  <a name="testing-and-debugging-your-code"></a><a name="testing"></a> Testen und Debuggen des Codes  
 Am einfachsten können Sie die Laufzeitmethoden eines benutzerdefinierten Objekts debuggen, indem Sie nach dem Erstellen des benutzerdefinierten Objekts **dtexec.exe** von [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] aus starten und ein Paket, das diese Komponente verwendet, ausführen.  
  
 Wenn Sie die Entwurfszeitmethoden der Komponente, z.B. die **Validate**-Methode, debuggen möchten, öffnen Sie ein Paket, das die Komponente in einer zweiten Instanz von [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] verwendet, und hängen Sie es an den **devenv.exe**-Prozess an.  
  
 Wenn Sie auch die Laufzeitmethoden der Komponente debuggen möchten, wenn ein Paket offen ist und in [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer ausgeführt wird, müssen Sie die Ausführung des Pakets anhalten, damit Sie auch für den **DtsDebugHost.exe**-Prozess einen Anfügevorgang durchführen können.  
  
#### <a name="to-debug-an-objects-run-time-methods-by-attaching-to-dtexecexe"></a>So debuggen Sie die Laufzeitmethoden eines Objekts durch das Anfügen an dtexec.exe  
  
1.  Signieren und erstellen Sie Ihr Projekt in der Debug-Konfiguration, stellen Sie es bereit, und installieren Sie es im globalen Assemblycache wie in diesem Thema beschrieben.  
  
2.  Wählen Sie unter **Projekteigenschaften** auf der Registerkarte **Debuggen** die Option **Externes Programm starten** als **Startaktion** aus, und gehen Sie zu **dtexec.exe**, standardmäßig unter „C:\Programme\Microsoft SQL Server\130\DTS\Binn“ installiert.  
  
3.  Geben Sie im Textfeld **Befehlszeilenoptionen** unter **Startoptionen** die Befehlszeilenargumente ein, die zum Ausführen eines Pakets erforderlich sind, das die Komponente verwendet. Oftmals besteht das Befehlszeilenargument aus dem /F[ILE]-Schalter gefolgt vom Pfad und dem Namen der .dtsx-Datei. Weitere Informationen finden Sie [hier](../../integration-services/packages/dtexec-utility.md).  
  
4.  Legen Sie sofern erforderlich im Quellcode Breakpoints in den Laufzeitmethoden der Komponente fest.  
  
5.  Führen Sie Ihr Projekt aus.  
  
#### <a name="to-debug-a-custom-objects-design-time-methods-by-attaching-to-sql-server-data-tools"></a>So debuggen Sie die Entwurfszeitmethoden eines benutzerdefinierten Objekts durch das Anfügen zu SQL Server-Datentools  
  
1.  Signieren und erstellen Sie Ihr Projekt in der Debug-Konfiguration, stellen Sie es bereit, und installieren Sie es im globalen Assemblycache wie in diesem Thema beschrieben.  
  
2.  Legen Sie sofern erforderlich im Quellcode Breakpoints in den Entwurfszeitmethoden des benutzerdefinierten Objekts fest.  
  
3.  Öffnen Sie eine zweite Instanz von [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], und laden Sie ein [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Projekt, das ein Paket enthält, das das benutzerdefinierte Objekt verwendet.  
  
4.  Führen Sie von der ersten Instanz von [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] einen Anfügevorgang für die zweite Instanz von **devenv.exe** durch, in der das Paket geladen wird, indem Sie aus dem Menü **Debuggen** der ersten Instanz die Option **An den Prozess anhängen** auswählen.  
  
5.  Führen Sie das Paket aus der zweiten Instanz von [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] aus.  
  
#### <a name="to-debug-a-custom-objects-run-time-methods-by-attaching-to-sql-server-data-tools"></a>So debuggen Sie die Laufzeitmethoden eines benutzerdefinierten Objekts durch das Anfügen zu SQL Server-Datentools  
  
1.  Wenn Sie die in der vorherigen Prozedur aufgeführten Schritte ausgeführt haben, halten Sie die Ausführung des Pakets an, sodass Sie einen Anfügevorgang für **DtsDebugHost.exe** durchführen können. Sie können diese Pause erzwingen, indem Sie dem **OnPreExecute**-Ereignis einen Breakpoint hinzufügen, oder indem Sie dem Projekt einen Skripttask hinzufügen und ein Skript eingeben, das ein modales Meldungsfeld anzeigt.  
  
2.  Führen Sie das Paket aus. Wechseln Sie nach dem Anhalten des Vorgangs zur Instanz von [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], in der das Codeprojekt geöffnet ist, und wählen Sie aus dem Menü **Debuggen** die Option **An den Prozess anhängen** aus. Stellen Sie sicher, dass Sie an die Instanz von **DtsDebugHost.exe** anfügen, die in der Spalte **Typ** als **Verwaltet, x86** aufgeführt wird, und nicht an die nur als **x86** aufgeführte Instanz.  
  
3.  Kehren Sie zum angehaltenen Paket zurück, und setzen Sie den Vorgang über den Breakpoint hinaus fort, oder klicken Sie auf **OK**, um das Meldungsfeld, das vom Skripttask aufgerufen wird, zu verwenden, und setzen Sie die Paketausführung und den Debugvorgang fort.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Entwickeln benutzerdefinierter Objekte für Integration Services](../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md)   
 [Beibehalten von benutzerdefinierten Objekten](../../integration-services/extending-packages-custom-objects/persisting-custom-objects.md)   
 [Tools zur Problembehandlung für die Paketentwicklung](../../integration-services/troubleshooting/troubleshooting-tools-for-package-development.md)  
  
  
