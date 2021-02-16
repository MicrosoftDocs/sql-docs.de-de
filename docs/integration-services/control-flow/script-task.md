---
description: Skripttask
title: Skripttask | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.scripttask.f1
- sql13.dts.designer.scripttask.general.f1
- sql13.dts.designer.scripttask.script.f1
helpviewer_keywords:
- scripts [Integration Services], tasks
- Script task [Integration Services], about Script task
- Script task [Integration Services]
ms.assetid: f6cce7df-4bd6-4b75-9f89-6c37b4bb5558
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 10be8411e2de29c514c3d9aed50c550a0e86ba91
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348274"
---
# <a name="script-task"></a>Skripttask

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Der Skripttask stellt Code zum Ausführen von Funktionen bereit, die in den integrierten Tasks und Transformationen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] nicht verfügbar sind. Der Skripttask kann auch Funktionen in einem einzigen Skript zusammenfassen, statt mehrere Tasks und Transformationen zu verwenden. Sie verwenden den Skripttask für einmalige Aktionen in einem Paket (einmalig pro aufgezähltem Objekt), anstatt diese einmalig pro Datenzeile auszuführen.  
  
 Der Skripttask kann für folgende Zwecke verwendet werden:  
  
-   Zugreifen auf Daten mithilfe anderer Technologien, die nicht von integrierten Verbindungstypen unterstützt werden. Beispielsweise kann ein Skript mithilfe von Active Directory Service Interfaces (ADSI) auf Benutzernamen von Active Directory zugreifen und diese extrahieren.  
  
-   Erstellen eines paketspezifischen Leistungsindikators. Beispielsweise kann ein Skript einen Leistungsindikator erstellen, der aktualisiert wird, während ein komplexer Task oder ein Task mit einem geringen Leistungsverhalten ausgeführt wird.  
  
-   Identifizieren, ob angegebene Dateien leer sind bzw. wie viele Zeilen sie enthalten, und deren Auswirkungen auf die Ablaufsteuerung in einem Paket auf Basis dieser Informationen. Enthält eine Datei beispielsweise 0 Zeilen, wird der Wert einer Variablen auf 0 festgelegt, und eine Rangfolgeneinschränkung, die den Wert auswertet, verhindert das Kopieren der Datei mit einem Dateisystemtask.  
  
 Wenn Sie das Skript verwenden müssen, um die gleichen Aktionen für jede Datenzeile einer Gruppe auszuführen, verwenden Sie anstelle des Skripttasks die Skriptkomponente. Eine Skriptkomponente wird beispielsweise verwendet, wenn Sie auswerten möchten, ob eine Portogebühr und das Auslassen von Datenzeilen mit extrem hohen oder niedrigen Beträgen angemessen sind. Weitere Informationen finden Sie unter [Script Component](../../integration-services/data-flow/transformations/script-component.md).  
  
 Falls ein Skript von mehreren Paketen verwendet wird, sollten Sie einen benutzerdefinierten Task erstellen, statt den Skripttask zu verwenden. Weitere Informationen finden Sie unter [Entwickeln eines benutzerdefinierten Tasks](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md).  
  
 Nachdem Sie entschieden haben, dass die Skriptkomponente für Ihr Paket geeignet ist, müssen Sie das Skript entwickeln, das vom Task verwendet wird, und den Task konfigurieren.  
  
## <a name="writing-and-running-the-script-that-the-task-uses"></a>Erstellen und Ausführen des Skripts, das vom Task verwendet wird  
 Der Skripttask verwendet [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]-Tools für Anwendungen (VSTA) als Umgebung zum Erstellen der Skripts und der Engine, mit der die Skripts ausgeführt werden.  
  
 VSTA enthält alle Standardfunktionen der [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] -Umgebung, z.B. den [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] -Editor mit Farbcodierung, IntelliSense und den **Objekt-Explorer**. VSTA verwendet zudem den Debugger, der auch von anderen [!INCLUDE[msCoName](../../includes/msconame-md.md)] -Entwicklungstools verwendet wird. Breakpoints im Skript sind vollständig mit Breakpoints in Tasks und Containern von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] kompatibel. VSTA unterstützt die Programmiersprachen [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic und [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C#.  
  
 Zum Ausführen eines Skripts muss VSTA auf dem Computer installiert sein, auf dem das Paket ausgeführt wird. Wenn das Paket ausgeführt wird, lädt der Task die Skript-Engine und führt das Skript aus. Sie können auf externe .NET-Assemblys zugreifen, indem Sie Verweise auf die Assemblys im Projekt hinzufügen. Zurzeit werden .NET Core-Referenzen und .NET-Standardassemblyreferenzen nicht unterstützt.  
  
> [!NOTE]  
>  Anders als in früheren Versionen, in denen Sie angeben konnten, ob die Skripts vorkompiliert sind, werden in [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] und nachfolgenden Versionen alle Skripts vorkompiliert. Wenn ein Skript vorkompiliert ist, wird die Sprach-Engine zur Laufzeit nicht geladen, und das Paket wird schneller ausgeführt. Kompilierte binäre Dateien belegen jedoch erheblich mehr Speicherplatz.  
  
## <a name="configuring-the-script-task"></a>Konfigurieren des Skripttasks  
 Es gibt folgende Möglichkeiten, um den Skripttask zu konfigurieren:  
  
-   Stellen Sie das benutzerdefinierte Skript bereit, das von dem Task ausgeführt wird.  
  
-   Geben Sie die Methode im VSTA-Projekt an, die die [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Laufzeit als Einstiegspunkt in den Code des Skripttasks aufruft.  
  
-   Geben Sie die Skriptsprache an.  
  
-   Geben Sie optional Listen mit schreibgeschützten und Lese-/Schreibvariablen für die Verwendung im Skript an.  
  
 Diese Eigenschaften können Sie mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
### <a name="configuring-the-script-task-in-the-designer"></a>Konfigurieren des Skripttasks im Designer  
 In der folgenden Tabelle wird das **ScriptTaskLogEntry** -Ereignis beschrieben, das für den Skripttask protokolliert werden kann. Das **ScriptTaskLogEntry** -Ereignis wird für die Protokollierung auf der Registerkarte **Details** des Dialogfelds **SSIS-Protokolle konfigurieren** ausgewählt. Weitere Informationen finden Sie unter [Integration Services-Protokollierung &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**ScriptTaskLogEntry**|Gibt die Ergebnisse des Implementierens der Protokollierung innerhalb des Skripts an. Der Task schreibt für jeden Aufruf der **Log** -Methode des **Dts** -Objekts einen Protokolleintrag. Der Task schreibt diese Einträge, wenn der Code ausgeführt wird. Weitere Informationen finden Sie unter [Logging in the Script Task](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md).|  
  
 Klicken Sie auf die folgenden Themen, um weitere Informationen zu den Eigenschaften zu erhalten, die Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen können:  
  
-   [Skripttask-Editor &#40;Seite „Allgemein“&#41;]()  
  
-   [Skripttask-Editor &#40;Seite Skript&#41;]()  
  
-   [Seite Ausdrücke](../../integration-services/expressions/expressions-page.md)  
  
 Weitere Informationen zum Festlegen dieser Eigenschaften im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer finden Sie im folgenden Thema:  
  
-   [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
### <a name="configuring-the-script-task-programmatically"></a>Programmgesteuertes Konfigurieren des Skripttasks  
 Weitere Informationen zum programmgesteuerten Festlegen dieser Eigenschaften finden Sie im folgenden Thema:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask>  
  
## <a name="script-task-editor-general-page"></a>Skripttask-Editor (Seite Allgemein)
  Auf der Seite **Allgemein** des Dialogfelds **Skripttask-Editor** können Sie einen Namen und eine Beschreibung für den Skripttask angeben.  
  
 Weitere Informationen zum Skripttask finden Sie unter [Script Task](../../integration-services/control-flow/script-task.md) und [Konfigurieren des Skripttasks im Skripttask-Editor](../../integration-services/extending-packages-scripting/task/configuring-the-script-task-in-the-script-task-editor.md). Informationen zum Programmieren des Skripttasks finden Sie unter [Extending the Package with the Script Task](../../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md).  
  
### <a name="options"></a>Tastatur  
 **Name**  
 Geben Sie einen eindeutigen Namen für den Skripttask an. Dieser Name wird im Tasksymbol als Bezeichnung verwendet.  
  
> [!NOTE]  
>  Tasknamen müssen innerhalb eines Pakets eindeutig sein.  
  
 **Beschreibung**  
 Geben Sie eine Beschreibung des Skripttasks ein.  
  
## <a name="script-task-editor-script-page"></a>Skripttask-Editor (Seite Skript)
  Mithilfe der Seite **Skript** des Dialogfelds **Skripttask-Editor** können Sie Skripteigenschaften festlegen und Variablen angeben, auf die dieses Skript zugreifen kann.  
  
> [!NOTE]  
>  In [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] und höheren Versionen werden alle Skripts vorkompiliert. In früheren Versionen wurde eine **PrecompileScriptIntoBinaryCode** -Eigenschaft festgelegt, um anzugeben, dass das Skript vorkompiliert wurde.  
  
 Weitere Informationen zum Skripttask finden Sie unter [Script Task](../../integration-services/control-flow/script-task.md) und [Konfigurieren des Skripttasks im Skripttask-Editor](../../integration-services/extending-packages-scripting/task/configuring-the-script-task-in-the-script-task-editor.md). Informationen zum Programmieren des Skripttasks finden Sie unter [Extending the Package with the Script Task](../../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md).  
  
### <a name="options"></a>Optionen  
 **ScriptLanguage**  
 Wählen Sie die Skriptsprache für den Task aus, entweder [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic oder [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C#.  
  
 Nachdem Sie ein Skript für den Task erstellt haben, können Sie den Wert der **ScriptLanguage** -Eigenschaft nicht mehr ändern.  
  
 Um die Standardskriptsprache für den Skripttask festzulegen, verwenden Sie im Dialogfeld **Optionen** auf der Seite **Allgemein** die Option **Skriptsprache** . Weitere Informationen finden Sie unter [General Page]().  
  
 **EntryPoint**  
 Geben Sie die Methode an, die die [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Laufzeit als Einstiegspunkt in den Code des Skripttasks aufruft. Die angegebene Methode muss in der ScriptMain-Klasse des Projekts der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]-Tools für Anwendungen (VSTA) angegeben werden. ScriptMain ist die Standardklasse, die von den Skriptvorlagen generiert wird.  
  
 Wenn Sie den Namen der Methode im VSTA-Projekt geändert haben, müssen Sie den Wert der **EntryPoint** -Eigenschaft ändern.  
  
 **ReadOnlyVariables**  
 Geben Sie eine durch Trennzeichen getrennte Liste von schreibgeschützten Variablen ein, die für das Skript verfügbar sind, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten (**...**), und wählen Sie die Variablen im Dialogfeld **Variablen auswählen** aus.  
  
> [!NOTE]  
>  Bei Variablennamen wird nach Groß-/Kleinschreibung unterschieden.  
  
 **ReadWriteVariables**  
 Geben Sie eine durch Trennzeichen getrennte Liste von Lese-/Schreibvariablen ein, die für das Skript verfügbar sind, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten (**...**), und wählen Sie die Variablen im Dialogfeld **Variablen auswählen** aus.  
  
> [!NOTE]  
>  Bei Variablennamen wird nach Groß-/Kleinschreibung unterschieden.  
  
 **Skript bearbeiten**  
 Öffnet die VSTA IDE, in der Sie das Skript erstellen oder ändern können.  
  
## <a name="related-content"></a>Verwandte Inhalte  
  
-   Technischer Artikel [Vorgehensweise: Senden von E-Mails mit Zustellungsbenachrichtigung in C#](https://go.microsoft.com/fwlink/?LinkId=237625)(Gewusst wie: Senden von E-Mails mit Zustellungsbenachrichtigung in C#) auf shareourideas.com  
  
