---
description: WMI-Datenleser (Task)
title: WMI-Datenleser (Task) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.wmidatareadertask.f1
- sql13.dts.designer.wmidatareadertask.general.f1
- sql13.dts.designer.wmidatareadertask.wmiquery.f1
helpviewer_keywords:
- WQL [Integration Services]
- WMI Data Reader task [Integration Services]
ms.assetid: dae57067-0275-4ac3-8f34-1b9d169f1112
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d246e590cd54aca1b3a6ac911ec12d2835c8d91f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339236"
---
# <a name="wmi-data-reader-task"></a>WMI-Datenleser (Task)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Der Task WMI-Datenleser führt Abfragen mithilfe von WQL (WMI Query Language) aus, womit Informationen von WMI zu einem Computersystem zurückgegeben werden. Der Task WMI-Datenleser kann für folgende Zwecke verwendet werden:  
  
-   Abfragen der Windows-Ereignisprotokolle auf einem lokalen Computer oder einem Remotecomputer und Schreiben der Informationen in eine Datei oder Variable.  
  
-   Abrufen von Informationen zum Vorhandensein, zum Status oder zu Eigenschaften von Hardwarekomponenten und Ermitteln mithilfe dieser Informationen, ob andere Tasks in der Ablaufsteuerung ausgeführt werden sollten.  
  
-   Abrufen einer Liste der Anwendungen und Ermitteln der installierten Version jeder Anwendung.  
  
 Es gibt folgende Möglichkeiten, um den Task WMI-Datenleser zu konfigurieren:  
  
-   Geben Sie den zu verwendenden WMI-Verbindungs-Manager an.  
  
-   Geben Sie die Quelle der WQL-Abfrage an. Die Abfrage kann in einer Taskeigenschaft gespeichert sein, die Abfrage kann aber auch außerhalb des Tasks in einer Variablen oder einer Datei gespeichert sein.  
  
-   Definieren Sie das Format der WQL-Abfrageergebnisse. Der Task unterstützt ein Tabellen-, Eigenschaftsname/Wert-Paar- oder Eigenschaftswertformat.  
  
-   Geben Sie das Ziel der Abfrage an. Das Ziel kann eine Variable oder eine Datei sein.  
  
-   Geben Sie an, ob das Abfrageziel überschrieben, beibehalten oder angefügt wird.  
  
 Falls es sich bei der Quelle oder dem Ziel um eine Datei handelt, verwendet der Task WMI-Datenleser einen Dateiverbindungs-Manager zum Herstellen einer Verbindung mit der Datei. Weitere Informationen finden Sie unter [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 Der Task WMI-Datenleser verwendet einen WMI-Verbindungs-Manager zum Herstellen einer Verbindung mit dem Server, von dem er WMI-Informationen liest. Weitere Informationen finden Sie unter [WMI Connection Manager](../../integration-services/connection-manager/wmi-connection-manager.md).  
  
## <a name="wql-query"></a>WQL-Abfrage  
 WQL ist ein Dialekt von SQL mit Erweiterungen zur Unterstützung der WMI-Ereignisbenachrichtigung und sonstigen WMI-spezifischen Funktionen. Weitere Informationen zu WQL finden Sie in der WMI-Dokumentation in der [MSDN Library](../../sql-server/index.yml).  
  
> [!NOTE]  
>  Die WMI-Klassen variieren in den verschiedenen Windows-Versionen.  
  
 Die folgende WQL-Abfrage gibt Einträge aus dem Anwendungsereignisprotokoll zurück.  
  
```  
SELECT * FROM Win32_NTLogEvent WHERE LogFile = 'Application' AND (SourceName='SQLISService' OR SourceName='SQLISPackage') AND TimeGenerated > '20050117'  
```  
  
 Die folgende WQL-Abfrage gibt Informationen zum logischen Datenträger zurück.  
  
```  
SELECT FreeSpace, DeviceId, Size, SystemName, Description FROM Win32_LlogicalDisk  
```  
  
 Die folgende WQL-Abfrage gibt eine Liste der QFE-Updates (Quick Fix Engineering) für das Betriebssystem zurück.  
  
```  
Select * FROM Win32_QuickFixEngineering  
```  
  
## <a name="custom-logging-messages-available-on-the-wmi-data-reader-task"></a>Verfügbare benutzerdefinierte Meldungen für die Protokollierung für den Task 'WMI-Datenleser'  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task WMI-Datenleser aufgelistet. Weitere Informationen finden Sie unter [Integration Services-Protokollierung &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**WMIDataReaderGettingWMIData**|Zeigt an, dass das Lesen der WMI-Daten begonnen wurde.|  
|**WMIDataReaderOperation**|Berichtet die vom Task ausgeführte WQL-Abfrage.|  
  
## <a name="configuration-of-the-wmi-data-reader-task"></a>Konfiguration des Tasks "WMI-Datenleser"  
 Eigenschaften können Sie programmgesteuert oder mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen.  
  
 Klicken Sie auf das folgende Thema, um Informationen zu den Eigenschaften zu erhalten, die Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen können:  
  
-   [Seite Ausdrücke](../../integration-services/expressions/expressions-page.md)  
  
 Klicken Sie auf das folgende Thema, um Informationen zum programmgesteuerten Festlegen dieser Eigenschaften anzuzeigen:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.WmiDataReaderTask.WmiDataReaderTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum Festlegen dieser Eigenschaften im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer zu erhalten:  
  
-   [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="wmi-data-reader-task-editor-general-page"></a>Editor für den Task 'WMI-Datenleser' (Seite Allgemein)
  Auf der Seite **Allgemein** des Dialogfelds **Editor für den Task 'WMI-Datenleser'** können Sie einen Namen und eine Beschreibung für den Task 'WMI-Datenleser' angeben.  
  
  Weitere Informationen zur WMI Query Language (WQL) finden Sie im Thema zur Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI) unter [Querying with WQL](/windows/win32/wmisdk/querying-with-wql)(Abfragen mit WQL) in der MSDN Library.  
  
### <a name="options"></a>Tastatur  
 **Name**  
 Geben Sie einen eindeutigen Namen für den Task 'WMI-Datenleser' an. Dieser Name wird im Tasksymbol als Bezeichnung verwendet.  
  
> [!NOTE]  
>  Tasknamen müssen innerhalb eines Pakets eindeutig sein.  
  
 **Beschreibung**  
 Geben Sie eine Beschreibung für den Task 'WMI-Datenleser' ein.  
  
## <a name="wmi-data-reader-task-editor-wmi-options-page"></a>Editor für den Task 'WMI-Datenleser' (Seite WMI-Optionen)
  Auf der Seite **WMI-Optionen** des Dialogfelds **Editor für den Task „WMI-Datenleser“** können Sie die Quelle der WQL-Abfrage (Windows Management Instrumentation Query Language) und das Ziel des Abfrageergebnisses angeben.  
  
 Weitere Informationen zur WMI Query Language (WQL) finden Sie im Thema zur Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI) unter [Querying with WQL](/windows/win32/wmisdk/querying-with-wql)(Abfragen mit WQL) in der MSDN Library.  
  
### <a name="static-options"></a>Statische Optionen  
 **WMIConnectionName**  
 Wählen Sie einen WMI-Verbindungs-Manager aus der Liste aus, oder klicken Sie auf \<**New WMI Connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [WMI-Verbindungs-Manager](../../integration-services/connection-manager/wmi-connection-manager.md), [WMI-Verbindungs-Manager-Editor](../connection-manager/wmi-connection-manager.md)  
  
 **WQLQuerySourceType**  
 Wählen Sie den Quelltyp der WQL-Abfrage aus, die von dem Task ausgeführt wird. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Direkteingabe**|Legen Sie die Quelle für eine WQL-Abfrage fest. Bei Auswahl dieses Werts wird die dynamische Option **WQLQuerySourceType** angezeigt.|  
|**Dateiverbindung**|Wählen Sie eine Datei aus, in der die WQL-Abfrage enthalten ist. Bei Auswahl dieses Werts wird die dynamische Option **WQLQuerySourceType** angezeigt.|  
|**Variable**|Legen Sie die Quelle für eine Variable fest, die die WQL-Abfrage definiert. Bei Auswahl dieses Werts wird die dynamische Option **WQLQuerySourceType** angezeigt.|  
  
 **OutputType**  
 Geben Sie an, ob es sich bei der Ausgabe um eine Datentabelle, einen Eigenschaftswert oder einen Eigenschaftsnamen und -wert handeln soll.  
  
 **OverwriteDestination**  
 Gibt an, ob die ursprünglichen Daten in der Zieldatei oder -variablen beibehalten, überschrieben oder an diese angefügt werden sollen.  
  
 **DestinationType**  
 Wählen Sie den Zieltyp der WQL-Abfrage aus, die von dem Task ausgeführt wird. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Dateiverbindung**|Wählen Sie eine Datei aus, um die Ergebnisse der WQL-Abfrage darin zu speichern. Bei Auswahl dieses Werts wird die dynamische Option **DestinationType** angezeigt.|  
|**Variable**|Legen Sie die Variable fest, um die Ergebnisse der WQL-Abfrage darin zu speichern. Bei Auswahl dieses Werts wird die dynamische Option **DestinationType** angezeigt.|  
  
### <a name="wqlquerysourcetype-dynamic-options"></a>Dynamische Optionen von WQLQuerySourceType  
  
#### <a name="wqlquerysourcetype--direct-input"></a>WQLQuerySourceType = Direct input  
 **WQLQuerySource**  
 Stellen Sie eine Abfrage bereit, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten (...), und geben Sie eine Abfrage mithilfe des Dialogfelds **WQL-Abfrage** ein.  
  
#### <a name="wqlquerysourcetype--file-connection"></a>WQLQuerySourceType = File connection  
 **WQLQuerySource**  
 Wählen Sie einen Dateiverbindungs-Manager aus der Liste aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
#### <a name="wqlquerysourcetype--variable"></a>WQLQuerySourceType = Variable  
 **WQLQuerySource**  
 Wählen Sie eine Variable aus der Liste aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
### <a name="destinationtype-dynamic-options"></a>Dynamische Optionen von DestinationType  
  
#### <a name="destinationtype--file-connection"></a>DestinationType = File connection  
 **Ziel**  
 Wählen Sie einen Dateiverbindungs-Manager aus der Liste aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
#### <a name="destinationtype--variable"></a>DestinationType = Variable  
 **Ziel**  
 Wählen Sie eine Variable aus der Liste aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Tasks](../../integration-services/control-flow/integration-services-tasks.md)   
 [Ablaufsteuerung](../../integration-services/control-flow/control-flow.md)  
  
