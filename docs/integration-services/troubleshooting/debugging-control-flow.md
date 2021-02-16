---
description: Debuggen der Ablaufsteuerung
title: Debuggen der Ablaufsteuerung | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.setbreakpoints.f1
helpviewer_keywords:
- progress reporting [Integration Services]
- breakpoints [Integration Services]
- debugging [Integration Services], control flow
- control flow [Integration Services], debugging
- color-coded progress reporting [Integration Services]
- Set Breakpoints dialog box
ms.assetid: 54a458cc-9f4f-4b48-8cf2-db2e0fa7756c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 53fc539f46751cf6073aee0d5cd3aefae1971283
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338398"
---
# <a name="debugging-control-flow"></a>Debuggen der Ablaufsteuerung

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] und [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] enthalten Features und Tools, die Sie für die Behandlung von Problemen mit der Ablaufsteuerung in einem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Paket verwenden können.  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] unterstützt Breakpoints für Container und Tasks.  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer stellt zur Laufzeit Fortschrittsberichte bereit.  
  
-   [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] stellt Debugfenster bereit.  
  
## <a name="breakpoints"></a>Breakpoints  
 [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer stellt das Dialogfeld **Breakpoints festlegen** bereit, in dem Sie Breakpoints festlegen können. Hierzu aktivieren Sie Unterbrechungsbedingungen und geben an, wie häufig ein Breakpoint auftreten kann, bevor die Ausführung des Pakets angehalten wird. Breakpoints können auf Paketebene oder auf der Ebene der einzelnen Komponenten aktiviert werden. Falls Unterbrechungsbedingungen auf der Task- oder Containerebene aktiviert sind, wird das Breakpointsymbol neben dem Task oder Container auf der Entwurfsoberfläche der Registerkarte **Ablaufsteuerung** angezeigt. Falls die Unterbrechungsbedingungen im Paket aktiviert sind, wird das Breakpointsymbol in der Bezeichnung der Registerkarte **Ablaufsteuerung** angezeigt.  
  
 Wird ein Breakpoint erreicht, ändert sich das Breakpointsymbol, damit Sie die Quelle des Breakpoints identifizieren können. Sie können beim Ausführen des Pakets Breakpoints hinzufügen, löschen und ändern.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] stellt zehn Unterbrechungsbedingungen bereit, die Sie für alle Tasks und Container aktivieren können. Im Dialogfeld **Breakpoints festlegen** können Sie Breakpoints für die folgenden Bedingungen aktivieren:  
  
|Unterbrechungsbedingung|BESCHREIBUNG|  
|---------------------|-----------------|  
|Wenn der Task oder Container das **OnPreExecute** -Ereignis empfängt.|Wird aufgerufen, unmittelbar bevor ein Task ausgeführt wird. Dieses Ereignis wird durch einen Task oder Container ausgelöst, unmittelbar bevor er ausgeführt wird.|  
|Wenn der Task oder Container das **OnPostExecute** -Ereignis empfängt.|Wird aufgerufen, unmittelbar nachdem die Ausführungslogik des Tasks beendet wurde. Dieses Ereignis wird durch einen Task oder Container ausgelöst, unmittelbar nachdem er ausgeführt wurde.|  
|Wenn der Task oder Container das **OnError** -Ereignis empfängt.|Wird durch einen Task oder Container bei einem Fehler aufgerufen.|  
|Wenn der Task oder Container das **OnWarning** -Ereignis empfängt.|Wird aufgerufen, wenn der Task einen Status aufweist, der keinen Fehler, aber eine Warnung rechtfertigt.|  
|Wenn der Task oder Container das **OnInformation** -Ereignis empfängt.|Wird aufgerufen, wenn der Task Informationen bereitstellen muss.|  
|Wenn der Task oder Container das **OnTaskFailed** -Ereignis empfängt.|Wird durch den Taskhost bei einem Fehler aufgerufen.|  
|Wenn der Task oder Container das **OnProgress** -Ereignis empfängt.|Wird aufgerufen, um den Status der Taskausführung zu aktualisieren.|  
|Wenn der Task oder Container das **OnQueryCancel** -Ereignis empfängt.|Wird zu einem beliebigen Zeitpunkt der Taskverarbeitung aufgerufen, wenn Sie die Ausführung abbrechen.|  
|Wenn der Task oder Container das **OnVariableValueChanged** -Ereignis empfängt.|Wird durch die [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Laufzeit aufgerufen, wenn sich der Wert einer Variablen ändert. Das RaiseChangeEvent der Variablen muss auf **TRUE** festgelegt werden, damit dieses Ereignis ausgelöst wird.<br /><br /> **&#42;&#42; Warnung &#42;&#42;** Die diesem Breakpoint zugeordnete Variable muss im **Containerbereich** definiert werden. Wenn die Variable im Paketbereich definiert wird, wird der Breakpoint nicht erreicht.|  
|Wenn der Task oder Container das **OnCustomEvent** -Ereignis empfängt.|Wird durch Tasks aufgerufen, um benutzerdefinierte Taskereignisse auszulösen.|  
  
 Neben den Unterbrechungsbedingungen, die für alle Tasks und Container verfügbar sind, enthalten manche Tasks und Container spezielle Unterbrechungsbedingungen zum Festlegen von Breakpoints. Beispielsweise können Sie eine Unterbrechungsbedingung für den For-Schleifencontainer aktivieren, um einen Breakpoint festzulegen, der die Ausführung zu Beginn jeder Iteration der Schleife anhält.  
  
 Wenn Sie einen Breakpoint flexibler und leistungsfähiger gestalten möchten, können Sie dessen Verhalten durch Angeben der folgenden Optionen ändern:  
  
-   Die Trefferanzahl oder die Angabe, oder nach dem wievielten Auftreten einer Unterbrechungsbedingung die Ausführung angehalten wird.  
  
-   Den Typ der Trefferanzahl oder die Regel, die angibt, wann die Unterbrechungsbedingung den Breakpoint auslöst.  
  
 Die Typen der Trefferanzahl, mit Ausnahme des Typs Immer, werden durch die Trefferanzahl weiter definiert. Beispielsweise wird beim Typ "Trefferanzahl ist gleich" und der Trefferanzahl 5 die Ausführung angehalten, wenn die Unterbrechungsbedingung das sechste Mal auftritt.  
  
 In der folgenden Tabelle sind die Typen der Trefferanzahl aufgeführt.  
  
|Typ der Trefferanzahl|BESCHREIBUNG|  
|--------------------|-----------------|  
|Always|Die Ausführung wird immer angehalten, wenn der Breakpoint erreicht wird.|  
|Trefferanzahl ist gleich|Die Ausführung wird angehalten, wenn die Anzahl des Auftretens des Breakpoints der Trefferanzahl entspricht.|  
|Trefferanzahl ist größer als oder gleich|Die Ausführung wird angehalten, wenn die Anzahl des Auftretens des Breakpoints mindestens so groß wie die Trefferanzahl ist.|  
|Trefferanzahl mehrfach|Die Ausführung wird angehalten, wenn ein Mehrfaches der Trefferanzahl erreicht wird. Wenn Sie z. B. diese Option auf 5 festlegen, wird die Ausführung bei jedem fünften Mal angehalten.|  
  
#### <a name="to-set-breakpoints"></a>So legen Sie Breakpoints fest  
  
-   [Debuggen eines Pakets durch Festlegen von Breakpoints auf einem Task oder Container](#debug)  
  
## <a name="progress-reporting"></a>Fortschrittsberichte  
 [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer enthält zwei Arten von Fortschrittsberichten: Farbcodierung auf der Entwurfsoberfläche der Registerkarte **Ablaufsteuerung** und Statusmeldungen auf der Registerkarte **Status** .  
  
 Wenn Sie ein Paket ausführen, wird der Ausführungsstatus im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer gekennzeichnet, indem jeder Task oder Container in einer Farbe angezeigt wird, die Ausschluss über den Ausführungsstatus gibt. Anhand der Farbe wissen Sie, ob das Element auf die Ausführung wartet, zurzeit ausgeführt wird, erfolgreich ausgeführt wurde oder nicht erfolgreich beendet wurde. Nachdem Sie die Paketausführung beendet haben, verschwindet die Farbcodierung.  
  
 In der folgenden Tabelle werden die Farben beschrieben, die zum Kennzeichnen des Ausführungsstatus verwendet werden.  
  
|Color|Ausführungsstatus|  
|-----------|----------------------|  
|Grau|Wartet auf die Ausführung|  
|Gelb|Wird ausgeführt|  
|Grün|Wurde erfolgreich ausgeführt|  
|Hervorgehoben|Wurde mit Fehlern ausgeführt|  
  
 Auf der Registerkarte **Status** werden Tasks und Container in der Ausführungsreihenfolge aufgeführt. Diese Registerkarte enthält außerdem die Start- und Beendigungszeiten, Warnungen und Fehlermeldungen. Wenn die Ausführung des Pakets beendet wurde, sind die Statusinformationen weiterhin auf der Registerkarte **Ausführungsergebnisse** verfügbar.  
  
> [!NOTE]  
>  Zum Aktivieren bzw. Deaktivieren der Anzeige von Meldungen auf der Registerkarte **Status** schalten Sie die Option **Debug-Statusbericht** im Menü **SSIS** um.  
  
 Im folgenden Diagramm wird die Registerkarte **Status** angezeigt.  
  
 ![Fortschrittsregisterkarte des SSIS-Designers](../../integration-services/troubleshooting/media/mw-dtsflow04.gif "Fortschrittsregisterkarte des SSIS-Designers")  
  
## <a name="debug-windows"></a>Debugfenster  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] enthält viele Fenster, in denen Sie Breakpoints verwenden und Pakete, die Breakpoints enthalten, debuggen können. Um weitere Informationen zu den einzelnen Fenstern zu erhalten, öffnen Sie das entsprechende Fenster, und drücken Sie F1, um die zugehörige Hilfe anzuzeigen.  
  
 Zeigen Sie zum Öffnen dieser Fenster in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]im Menü **Debuggen** auf **Fenster**, und klicken Sie dann auf **Breakpoints**, **Ausgabe** oder **Direkt**.  
  
 In der folgenden Tabelle sind diese Fenster beschrieben.  
  
|Fenster|BESCHREIBUNG|  
|------------|-----------------|  
|Breakpoints|Listet die Breakpoints in einem Paket auf und stellt Optionen zum Aktivieren und Löschen von Breakpoints bereit.|  
|Output|Zeigt Statusmeldungen für Funktionen in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]an.|  
|Unmittelbar|Wird zum Debuggen und Auswerten von Ausdrücken und zum Drucken von Variablenwerten verwendet.|  

## <a name="debug-a-package-by-setting-breakpoints-on-a-task-or-a-container"></a><a name="debug"></a> Debuggen eines Pakets durch Festlegen von Breakpoints auf einem Task oder Container
  In diesem Verfahren wird beschrieben, wie in einem Paket, Task, For-Schleifencontainer, Foreach-Schleifencontainer oder einem Sequenzcontainer Breakpoints festgelegt werden.  
  
### <a name="to-set-breakpoints-in-a-package-a-task-or-a-container"></a>So legen Sie Breakpoints in einem Paket, einem Task oder einem Container fest  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Projekt mit dem gewünschten Paket.  
  
2.  Doppelklicken Sie auf das Paket, in dem Sie Breakpoints festlegen möchten.  
  
3.  Gehen Sie im SSIS-Designer wie folgt vor:  
  
    -   Um Breakpoints im Paketobjekt festzulegen, klicken Sie auf die Registerkarte **Ablaufsteuerung,** setzen den Cursor an eine beliebige Stelle im Hintergrund der Entwurfsoberfläche, klicken mit der rechten Maustaste und wählen dann **Breakpoints bearbeiten** aus.  
  
    -   Um Breakpoints in einer Paketablaufsteuerung festzulegen, klicken Sie auf die Registerkarte **Ablaufsteuerung** , klicken mit der rechten Maustaste auf einen Task, einen For-Schleifencontainer, einen Foreach-Schleifencontainer oder einen Sequenzcontainer und dann auf **Breakpoints bearbeiten**.  
  
    -   Um Breakpoints in einem Ereignishandler festzulegen, klicken Sie auf die Registerkarte **Ereignishandler** , klicken mit der rechten Maustaste auf einen Task, einen For-Schleifencontainer, einen Foreach-Schleifencontainer oder einen Sequenzcontainer und dann auf **Breakpoints bearbeiten**.  
  
4.  Wählen Sie im Dialogfeld **Breakpoints festlegen\<container name>** die zu aktivierenden Breakpoints aus.  
  
5.  Ändern Sie wahlweise den Typ der Trefferanzahl und die Trefferanzahl für jeden Breakpoint.  
  
6.  Klicken Sie im Menü **Datei** auf **Ausgewählte Elemente speichern** , um das aktualisierte Paket zu speichern.  

## <a name="set-breakpoints"></a>Breakpoints festlegen
  Verwenden Sie das Dialogfeld **Breakpoints festlegen** , um die Ereignisse anzugeben, für die Breakpoints aktiviert werden sollen, sowie um das Verhalten der Breakpoints zu steuern.  
  
### <a name="options"></a>Tastatur  
 **Aktiviert**  
 Wählen Sie diese Option aus, um einen Breakpoint für ein Ergebnis zu aktivieren.  
  
 **Break Condition**  
 Zeigen Sie eine Liste von verfügbaren Ereignissen an, für die Breakpoints festgelegt werden können.  
  
 **Hit Count Type**  
 Geben Sie an, wann der Breakpoint wirksam werden soll.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Always**|Die Ausführung wird immer angehalten, wenn der Breakpoint erreicht wird.|  
|**Trefferanzahl ist gleich**|Die Ausführung wird angehalten, wenn die Anzahl des Auftretens des Breakpoints der Trefferanzahl entspricht.|  
|**Trefferanzahl ist größer als oder gleich**|Die Ausführung wird angehalten, wenn die Anzahl des Auftretens des Breakpoints mindestens so groß wie die Trefferanzahl ist.|  
|**Trefferanzahl mehrfach**|Die Ausführung wird angehalten, wenn ein Mehrfaches der Trefferanzahl erreicht wird. Wenn Sie z. B. diese Option auf 5 festlegen, wird die Ausführung bei jedem fünften Mal angehalten.|  
  
 **Trefferanzahl**  
 Geben Sie die Anzahl der Treffer an, nach denen ein Breakpoint ausgelöst werden soll. Diese Option ist nicht verfügbar, wenn der Breakpoint immer wirksam ist.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Tools zur Problembehandlung für die Paketentwicklung](../../integration-services/troubleshooting/troubleshooting-tools-for-package-development.md)  
 [Debuggen eines Skripts durch Festlegen von Breakpoints in einem Skripttask und einer Skriptkomponente](../../integration-services/extending-packages-scripting/debug-a-script-by-setting-breakpoints-in-a-script-task-and-script-component.md)   
