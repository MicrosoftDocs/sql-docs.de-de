---
description: Prozess ausführen (Task)
title: Prozess ausführen (Task) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executeprocesstask.f1
- sql13.dts.designer.executeprocesstask.general.f1
- sql13.dts.designer.executeprocesstask.process.f1
helpviewer_keywords:
- Execute Process task [Integration Services]
ms.assetid: aca5a0b5-34a9-45bc-a234-8e63ea51a1ee
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 98409c2a75cc76589a705579ff02f1be34c9c7b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352117"
---
# <a name="execute-process-task"></a>Prozess ausführen (Task)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Der Task „Prozess ausführen“ führt eine Anwendung oder Batchdatei als Teil eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Paketworkflows aus. Mit der Task 'Prozess ausführen' können alle Standardanwendungen wie z. B. [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] oder [!INCLUDE[ofprword](../../includes/ofprword-md.md)]geöffnet werden, normalerweise wird er jedoch zum Ausführen von Geschäftsanwendungen oder Batchdateien für eine Datenquelle verwendet. Beispielsweise können Sie mit dem Task 'Prozess ausführen' eine komprimierte Textdatei expandieren. Anschließend kann das Paket die Textdatei als Datenquelle für den Datenfluss im Paket verwenden. Sie können mit dem Task 'Prozess ausführen' auch eine benutzerdefinierte [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] -Anwendung ausführen, die einen täglichen Umsatzbericht erstellt. Anschließend können Sie den Bericht an einen "Mail senden"'-Task anfügen und an eine Verteilerliste weiterleiten.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] schließt weitere Tasks ein, die Workflowvorgänge ausführen, wie z. B. das Ausführen von Paketen. Weitere Informationen finden Sie unter [Execute Package Task](../../integration-services/control-flow/execute-package-task.md).  
  
## <a name="custom-log-entries-available-on-the-execute-process-task"></a>Verfügbare benutzerdefinierte Protokolleinträge für den Task 'Prozess ausführen'  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task Prozess ausführen aufgelistet. Weitere Informationen finden Sie unter [Integration Services-Protokollierung &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**ExecuteProcessExecutingProcess**|Enthält Informationen zu dem Prozess, für dessen Ausführung der Task konfiguriert ist.<br /><br /> Es werden zwei Protokolleinträge geschrieben. Der eine Protokolleintrag enthält Informationen über den Namen und Speicherort der vom Task ausgeführten ausführbaren Datei, im anderen Eintrag wird das Beenden der ausführbaren Datei erfasst.|  
|**ExecuteProcessVariableRouting**|Enthält Informationen darüber, welche Variablen an die Eingabe und an die Ausgaben der ausführbaren Datei geleitet werden. Es werden Protokolleinträge für stdin (für die Eingabe), für stdout (für die Ausgabe) und für stderr (für die Fehlerausgabe) geschrieben.|  
  
## <a name="configuration-of-the-execute-process-task"></a>Konfiguration des Tasks "Prozess ausführen"  
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum Festlegen dieser Eigenschaften im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer zu erhalten:  
  
-   [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
### <a name="property-settings"></a>Eigenschafteneinstellungen  
 Wenn der Task "Prozess ausführen" eine benutzerdefinierte Anwendung ausführt, stellt der Task mithilfe einer der beiden folgenden Methoden eine Eingabe für die Anwendung bereit:  
  
-   Eine Variable, die Sie in der Eigenschaftseinstellung **StandardInputVariable** angeben. Weitere Informationen zu Variablen finden Sie unter [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) und [Verwenden von Variablen in Paketen](../integration-services-ssis-variables.md).  
  
-   Ein Argument, das Sie in der Eigenschaftseinstellung **Argumente** angeben. (Wenn der Task beispielsweise ein Dokument in Word öffnet, kann mit dem Argument die DOC-Datei angegeben werden.)  
  
 Um mehrere Argumente an eine benutzerdefinierte Anwendung in einem einzigen 'Prozess ausführen'-Task zu übergeben, verwenden Sie Leerzeichen, um die Argumente voneinander zu trennen. Ein Argument darf kein Leerzeichen enthalten; sonst wird der Task nicht ausgeführt. Sie können einen Ausdruck verwenden, um einen Variablenwert als Argument zu übergeben. Im folgenden Beispiel übergibt der Ausdruck zwei Variablenwerte als Argumente und verwendet ein Leerzeichen, um die Argumente voneinander zu trennen:  
  
 `@variable1 + " " + @variable2`  
  
 Sie können einen Ausdruck verwenden, um verschiedene Eigenschaften des Tasks 'Prozess ausführen' festzulegen.  
  
 Wenn Sie zum Konfigurieren des Tasks Prozess ausführen für Eingaben die Eigenschaft **StandardInputVariable** verwenden, rufen Sie zum Lesen der Eingabe die **Console.ReadLine** -Methode der Anwendung auf. Weitere Informationen finden Sie im Artikel [Console.ReadLine-Methode](/dotnet/api/system.console.readline) in der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]-Klassenbibliothek.  
  
 Wenn Sie zum Konfigurieren des Tasks Prozess ausführen für Eingaben die Eigenschaft **Argumente** verwenden, führen Sie zum Abrufen der Argumente einen der folgenden Schritte aus:  
  
-   Wenn Sie Microsoft Visual Basic verwenden, um die Anwendung zu schreiben, legen Sie die **My.Application.CommandLineArgs** -Eigenschaft fest. Im folgenden Beispiel ruft die **My.Application.CommandLineArgs** -Eigenschaft zwei Argumente ab:  
  
    ```vb  
    Dim variable1 As String = My.Application.CommandLineArgs.Item(0)  
    Dim variable2 As String = My.Application.CommandLineArgs.Item(1)   
    ```  
  
     Weitere Informationen finden Sie in der [-Referenz unter](https://go.microsoft.com/fwlink/?LinkId=129200)My.Application.CommandLineArgs Property [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .  
  
-   Verwenden Sie die **Main**-Methode, wenn Sie die Anwendung mit Microsoft Visual C# schreiben.  
  
     Weitere Informationen finden Sie im C#-Programmierhandbuch unter [Befehlszeilenargumente (C#-Programmierhandbuch)](/dotnet/csharp/programming-guide/main-and-command-args/command-line-arguments).  
  
 Der Task 'Prozess ausführen' enthält außerdem die Eigenschaften **StandardOutputVariable** und **StandardErrorVariable** zum Angeben der Variablen, die jeweils die Standardausgabe und die Fehlerausgabe der Anwendung in Anspruch nehmen.  
  
 Darüber hinaus können Sie für den Task 'Prozess ausführen' ein Arbeitsverzeichnis, einen Timeoutzeitraum oder einen Wert zum Anzeigen der erfolgreichen Ausführung der ausführbaren Datei konfigurieren. Für den Task kann außerdem konfiguriert werden, dass ein Fehler gemeldet wird, falls der Rückgabecode der ausführbaren Datei nicht mit dem Wert übereinstimmt, der für die erfolgreiche Ausführung steht, oder falls die ausführbare Datei im angegebenen Speicherort nicht gefunden wird.  
  
## <a name="programmatic-configuration-of-the-execute-process-task"></a>Programmgesteuerte Konfiguration des Tasks "Prozess ausführen"  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum programmgesteuerten Festlegen dieser Eigenschaften anzuzeigen:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.ExecuteProcess.ExecuteProcess>  
  
## <a name="execute-process-task-editor-general-page"></a>Editor für den Task 'Prozess ausführen' (Seite Allgemein)
  Auf der Seite **Allgemein** des Dialogfelds **Editor für den Task „Prozess ausführen“** können Sie den Task „Prozess ausführen“ benennen und beschreiben.  
  
### <a name="options"></a>Tastatur  
 **Name**  
 Geben Sie einen eindeutigen Namen für den Task 'Prozess ausführen' an. Dieser Name wird im Tasksymbol als Bezeichnung verwendet.  
  
> [!NOTE]  
>  Tasknamen müssen innerhalb eines Pakets eindeutig sein.  
  
 **Beschreibung**  
 Geben Sie eine Beschreibung des Tasks 'Prozess ausführen' ein.  
  
## <a name="execute-process-task-editor-process-page"></a>Editor für den Task 'Prozess ausführen' (Seite Verarbeiten)
  Auf der Seite **Verarbeiten** des Dialogfelds **Editor für den Task 'Prozess ausführen'** können Sie die Optionen konfigurieren, die den Prozess ausführen. Zu den Optionen gehören der Name der ausführbaren Datei, der Speicherort dieser Datei, die Argumente der Eingabeaufforderung und die Variablen für die Ein- und Ausgabe.  
  
### <a name="options"></a>Tastatur  
 **RequireFullFileName**  
 Geben Sie an, ob der Task fehlschlagen soll, wenn die ausführbare Datei am angegebenen Speicherort nicht gefunden wird.  
  
 **Ausführbare Datei**  
 Geben Sie den Namen der ausführbaren Datei ein.  
  
 **Argumente**  
 Geben Sie Argumente für die Eingabeaufforderung an.  
  
 **WorkingDirectory**  
 Geben Sie den Pfad zu dem Ordner ein, in dem die ausführbare Datei enthalten ist, oder klicken Sie auf die Schaltfläche zum Durchsuchen **(...)** , und suchen Sie den Ordner.  
  
 **StandardInputVariable**  
 Wählen Sie eine Variable für die Bereitstellung der Eingabe zum Prozess aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
 **StandardOutputVariable**  
 Wählen Sie eine Variable zum Erfassen der Ausgabe des Prozess aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **StandardErrorVariable**  
 Wählen Sie eine Variable zum Erfassen der Fehlerausgabe des Prozessors aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **FailTaskIfReturnCodeIsNotSuccessValue**  
 Geben Sie an, ob beim Task ein Fehler auftritt, wenn der Prozessexitcode von dem unter **SuccessValue** angegebenen Wert abweicht.  
  
 **SuccessValue**  
 Geben Sie den Wert an, der von der ausführbaren Datei zurückgegeben wird, um einen Erfolg zu melden. Standardmäßig ist dieser Wert auf **0** festgelegt.  
  
 **TimeOut**  
 Geben Sie die Anzahl von Sekunden an, die der Prozess ausgeführt werden kann. Durch den Wert **0** wird angezeigt, dass kein Timeoutwert verwendet wird und der Prozess so lange ausgeführt wird, bis er entweder abgeschlossen ist oder ein Fehler auftritt.  
  
 **TerminateProcessAfterTimeOut**  
 Geben Sie an, ob das Ende des Prozesses nach Ablauf des durch die Option **TimeOut** angegebenen Timeoutzeitraumes erzwungen wird. Diese Option ist nur verfügbar, wenn der Wert unter **TimeOut** nicht **0** ist.  
  
 **WindowStyle**  
 Geben Sie die Fensteranordnung an, in der der Prozess ausgeführt wird.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Tasks](../../integration-services/control-flow/integration-services-tasks.md)   
 [Ablaufsteuerung](../../integration-services/control-flow/control-flow.md)  
  
