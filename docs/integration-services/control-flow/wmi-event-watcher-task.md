---
description: WMI-Ereignisüberwachung (Task)
title: WMI-Ereignisüberwachung (Task) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.wmieventwatchertask.f1
- sql13.dts.designer.wmieventwatcher.general.f1
- sql13.dts.designer.wmieventwatcher.wmiquery.f1
helpviewer_keywords:
- WQL [Integration Services]
- WMI Event Watcher task [Integration Services]
ms.assetid: b5bb52e9-a77e-41e1-93f9-d4c3bc6b2c9a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f121c76658d16d59994ac379d11e59c9ec03d710
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348335"
---
# <a name="wmi-event-watcher-task"></a>WMI-Ereignisüberwachung (Task)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Der Task "WMI-Ereignisüberwachung" überwacht ein WMI-Ereignis (Windows Management Instrumentation) mithilfe einer WQL-Ereignisabfrage (Windows Management Instrumentation Query Language), um relevante Ereignisse anzugeben. Der Task WMI-Ereignisüberwachung kann für folgende Zwecke verwendet werden:  
  
-   Warten auf die Benachrichtigung, dass Dateien einem Ordner hinzugefügt wurden, und dann Initiieren der Dateiverarbeitung.  
  
-   Ausführen eines Pakets, mit dem Dateien gelöscht werden, wenn der verfügbare Arbeitsspeicher auf einem Server unter einen angegebenen Prozentsatz sinkt.  
  
-   Überwachen der Installation einer Anwendung und dann Ausführen eines Pakets, das die Anwendung verwendet.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] enthält einen Task, der WMI-Informationen liest.  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zu diesem Task zu erhalten:  
  
-   [WMI-Datenleser (Task)](../../integration-services/control-flow/wmi-data-reader-task.md)  
  
## <a name="wql-queries"></a>WQL-Abfragen  
 WQL ist ein Dialekt von SQL mit Erweiterungen zur Unterstützung der WMI-Ereignisbenachrichtigung und sonstigen WMI-spezifischen Funktionen. Weitere Informationen zu WQL finden Sie in der WMI-Dokumentation in der [MSDN Library](/documentation/).  
  
> [!NOTE]  
>  Die WMI-Klassen variieren in den verschiedenen Windows-Versionen.  
  
 Die folgende Abfrage überwacht die Benachrichtigung, dass die CPU-Nutzung über 40 % beträgt.  
  
```  
SELECT * from __InstanceModificationEvent WITHIN 2 WHERE TargetInstance ISA 'Win32_Processor' and TargetInstance.LoadPercentage > 40  
```  
  
 Die folgende Abfrage überwacht die Benachrichtigung, dass einem Ordner eine Datei hinzugefügt wurde.  
  
```  
SELECT * FROM __InstanceCreationEvent WITHIN 10 WHERE TargetInstance ISA "CIM_DirectoryContainsFile" and TargetInstance.GroupComponent= "Win32_Directory.Name=\"c:\\\\WMIFileWatcher\""   
```  
  
## <a name="custom-logging-messages-available-on-the-wmi-event-watcher-task"></a>Verfügbare benutzerdefinierte Meldungen für die Protokollierung für den Task "WMI-Ereignisüberwachung"  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task WMI-Ereignisüberwachung aufgelistet. Weitere Informationen finden Sie unter [Integration Services-Protokollierung &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**WMIEventWatcherEventOccurred**|Zeigt an, dass ein vom Task überwachtes Ereignis aufgetreten ist.|  
|**WMIEventWatcherTimedout**|Zeigt an, dass beim Task ein Timeout eingetreten ist.|  
|**WMIEventWatcherWatchingForWMIEvents**|Zeigt an, dass die Ausführung der WQL-Abfrage begonnen wurde. Der Eintrag schließt die Abfrage ein.|  
  
## <a name="configuration-of-the-wmi-event-watcher-task"></a>Konfiguration des Tasks "WMI-Ereignisüberwachung"  
 Es gibt folgende Möglichkeiten, um den Task WMI-Datenleser zu konfigurieren:  
  
-   Geben Sie den zu verwendenden WMI-Verbindungs-Manager an.  
  
-   Geben Sie die Quelle der WQL-Abfrage an. Die Quelle kann extern zum Task oder eine Variable bzw. Datei sein, die Abfrage kann aber auch in einer Taskeigenschaft gespeichert sein.  
  
-   Geben Sie die Aktion an, die der Task ausführt, wenn das WMI-Ereignis auftritt. Sie können die Ereignisbenachrichtigung und den Status nach dem Ereignis protokollieren oder benutzerdefinierte [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Ereignisse auslösen, die Informationen zum WMI-Ereignis, zur Benachrichtigung und zum Status nach dem Ereignis bereitstellen.  
  
-   Definieren Sie, wie der Task auf das Ereignis antwortet. Der Task kann je nach Ereignis erfolgreich ausgeführt werden oder einen Fehler melden, der Task kann aber auch lediglich das Ereignis erneut überwachen.  
  
-   Geben Sie die Aktion an, die der Task bei einem Timeout der WMI-Abfrage ausführt. Sie können das Timeout und den Status nach dem Timeout protokollieren oder aber ein benutzerdefiniertes [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Ereignis mit dem Hinweis, dass beim WMI-Ereignis ein Timeout aufgetreten ist, auslösen und das Timeout und den Timeoutstatus protokollieren.  
  
-   Definieren Sie, wie der Task auf das Timeout antwortet. Der Task kann erfolgreich ausgeführt werden oder einen Fehler melden, der Task kann aber auch lediglich das Ereignis erneut überwachen.  
  
-   Geben Sie an, wie oft der Task das Ereignis überwacht.  
  
-   Geben Sie das Timeout an.  
  
 Falls die Quelle eine Datei ist, verwendet der Task WMI-Ereignisüberwachung einen Dateiverbindungs-Manager zum Herstellen einer Verbindung mit der Datei. Weitere Informationen finden Sie unter [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 Der Task "WMI-Ereignisüberwachung" verwendet einen WMI-Verbindungs-Manager zum Herstellen einer Verbindung mit dem Server, von dem er WMI-Informationen liest. Weitere Informationen finden Sie unter [WMI Connection Manager](../../integration-services/connection-manager/wmi-connection-manager.md).  
  
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zu den Eigenschaften zu erhalten, die Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen können:  
  
-   [Seite Ausdrücke](../../integration-services/expressions/expressions-page.md)  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum Festlegen dieser Eigenschaften im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer zu erhalten:  
  
-   [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-the-wmi-event-watcher-task"></a>Programmgesteuerte Konfiguration des Tasks "WMI-Ereignisüberwachung"  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum programmgesteuerten Festlegen dieser Eigenschaften anzuzeigen:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.WmiEventWatcherTask.WmiEventWatcherTask>  
  
## <a name="wmi-event-watcher-task-editor-general-page"></a>Editor für den Task 'WMI-Ereignisüberwachung' (Seite Allgemein)
  Auf der Seite **Allgemein** des Dialogfelds **Editor für den Task 'WMI-Ereignisüberwachung'** können Sie einen Namen und eine Beschreibung für den Task 'WMI-Ereignisüberwachung' angeben.  
  
 Weitere Informationen zur WMI Query Language (WQL) finden Sie im Thema zur Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI) unter [Querying with WQL](/windows/win32/wmisdk/querying-with-wql)(Abfragen mit WQL) in der MSDN Library.  
  
### <a name="options"></a>Tastatur  
 **Name**  
 Geben Sie einen eindeutigen Namen für den Task 'WMI-Ereignisüberwachung' an. Dieser Name wird im Tasksymbol als Bezeichnung verwendet.  
  
> [!NOTE]  
>  Tasknamen müssen innerhalb eines Pakets eindeutig sein.  
  
 **Beschreibung**  
 Geben Sie eine Beschreibung für den Task 'WMI-Ereignisüberwachung' ein.  
  
## <a name="wmi-event-watcher-task-editor-wmi-options-page"></a>Editor für den Task 'WMI-Ereignisüberwachung' (Seite WMI-Optionen)
  Auf der Seite **WMI-Optionen** des Dialogfelds **Editor für den Task „WMI-Ereignisüberwachung“** können Sie die Quelle der WQL-Abfrage (Windows Management Instrumentation Query Language) und das Verhalten angeben, mit dem der Task „WMI-Ereignisüberwachung“ auf WMI-Ereignisse (Microsoft Windows Instrumentation) antwortet.  
  
 Weitere Informationen zur WMI Query Language (WQL) finden Sie im Thema zur Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI) unter [Querying with WQL](/windows/win32/wmisdk/querying-with-wql)(Abfragen mit WQL) in der MSDN Library.  
  
### <a name="static-options"></a>Statische Optionen  
 **WMIConnectionName**  
 Wählen Sie einen WMI-Verbindungs-Manager aus der Liste aus, oder klicken Sie auf \<**New WMI Connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [WMI-Verbindungs-Manager](../../integration-services/connection-manager/wmi-connection-manager.md), [WMI-Verbindungs-Manager-Editor](../connection-manager/wmi-connection-manager.md)  
  
 **WQLQuerySourceType**  
 Wählen Sie den Quelltyp der WQL-Abfrage aus, die von dem Task ausgeführt wird. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Direkteingabe**|Legen Sie die Quelle für eine WQL-Abfrage fest. Bei Auswahl dieses Wertes wird die dynamische Option **WQLQuerySource** angezeigt.|  
|**Dateiverbindung**|Wählen Sie eine Datei aus, in der die WQL-Abfrage enthalten ist. Bei Auswahl dieses Wertes wird die dynamische Option **WQLQuerySource** angezeigt.|  
|**Variable**|Legen Sie die Quelle für eine Variable fest, die die WQL-Abfrage definiert. Bei Auswahl dieses Wertes wird die dynamische Option **WQLQuerySource** angezeigt.|  
  
 **ActionAtEvent**  
 Geben Sie an, ob das WMI-Ereignis das Ereignis protokolliert und eine [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Aktion initiiert oder nur das Ereignis protokolliert.  
  
 **AfterEvent**  
 Gibt an, ob der Task nach dem Empfangen des WMI-Ereignisses erfolgreich ausgeführt wird oder fehlschlägt, oder ob die Überwachung des Auftretens dieses Ereignisses durch den Task fortgesetzt wird.  
  
 **ActionAtTimeout**  
 Gibt an, ob ein WMI-Abfragetimeout durch den Task protokolliert und als Antwort ein [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Ereignis ausgelöst wird, oder ob nur der Timeout protokolliert wird.  
  
 **AfterTimeout**  
 Gibt an, ob der Task als Antwort auf einen Timeout erfolgreich ausgeführt wird oder fehlschlägt, oder ob der Task die Überwachung fortsetzt, bis ein weiterer Timeout auftritt.  
  
 **NumberOfEvents**  
 Gibt die Anzahl der zu überwachenden Ereignisse an.  
  
 **Timeout**  
 Gibt die Zeit in Sekunden an, in der auf das Auftreten des Ereignisses gewartet wird. Der Wert 0 bedeutet, dass kein Timeout aktiviert ist.  
  
### <a name="wqlquerysource-dynamic-options"></a>WQLQuerySource (dynamische Optionen)  
  
#### <a name="wqlquerysource--direct-input"></a>WQLQuerySource = Direct input  
 **WQLQuerySource**  
 Stellen Sie eine Abfrage bereit, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten (...), und geben Sie eine Abfrage mithilfe des Dialogfelds **WQL-Abfrage** ein.  
  
#### <a name="wqlquerysource--file-connection"></a>WQLQuerySource = File connection  
 **WQLQuerySource**  
 Wählen Sie einen Dateiverbindungs-Manager aus der Liste aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
#### <a name="wqlquerysource--variable"></a>WQLQuerySource = Variable  
 **WQLQuerySource**  
 Wählen Sie eine Variable aus der Liste aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md)