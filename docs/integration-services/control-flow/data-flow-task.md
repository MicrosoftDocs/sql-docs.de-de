---
description: Datenflusstask
title: Datenflusstask | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataflowtask.f1
helpviewer_keywords:
- data flow task [Integration Services]
- performance [Integration Services]
- data flow [Integration Services], performance
- data flow [Integration Services], Data Flow task
- Integration Services, performance
ms.assetid: c27555c4-208c-43c8-b511-a4de2a8a3344
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 00270115679b886a0f4d657487a91e845c56da3f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352119"
---
# <a name="data-flow-task"></a>Datenflusstask

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Der Datenflusstask kapselt die Datenfluss-Engine, mit dem Daten zwischen Quellen und Zielen verschoben werden, und ermöglicht dem Benutzer das Transformieren, Bereinigen und Ändern von Daten beim Verschieben. Durch das Hinzufügen eines Datenflusstasks zu einer Paketablaufsteuerung kann das Paket Daten extrahieren, transformieren und laden.  
  
 Ein Datenfluss besteht aus mindestens einer Datenflusskomponente, normalerweise jedoch aus verbundenen Datenflusskomponenten. Dabei handelt es sich um Quellen zum Extrahieren von Daten, Transformationen zum Ändern, Routen oder Zusammenfassen von Daten sowie Ziele zum Laden von Daten.  
  
 Zur Laufzeit erstellt der Datenflusstask einen Ausführungsplan vom Datenfluss, und die Datenfluss-Engine führt den Plan aus. Sie können einen Datenflusstask ohne Datenfluss erstellen, aber der Task wird nur ausgeführt, wenn mindestens ein Datenfluss vorhanden ist.  
  
 Zum Masseneinfügen von Daten aus Textdateien in eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank können Sie den Masseneinfügungstask anstelle eines Datenflusstasks und eines Datenflusses verwenden. Mit dem Masseneinfügungstask ist es jedoch nicht möglich, Daten zu transformieren. Weitere Informationen finden Sie unter [Bulk Insert Task](../../integration-services/control-flow/bulk-insert-task.md).  
  
## <a name="multiple-flows"></a>Mehrere Flüsse  
 Ein Datenflusstask kann mehrere Datenflüsse einschließen. Falls ein Task mehrere Datasets kopiert und falls die Reihenfolge, in der die Daten kopiert werden, keine Rolle spielt, kann es praktischer sein, mehrere Datenflüsse in den Datenflusstask einzuschließen. Beispielsweise können Sie fünf Datenflüsse erstellen, von denen jeder Daten aus einer Flatfile in eine unterschiedliche Dimensionstabelle in einem Data Warehouse-Sternschema kopiert.  
  
 Die Datenfluss-Engine bestimmt jedoch die Ausführungsreihenfolge, wenn in einem einzigen Datenflusstask mehrere Datenflüsse vorhanden sind. Wenn deshalb die Reihenfolge eine Rolle spielt, sollte das Paket mehrere Datenflusstasks verwenden, wobei jeder Task einen Datenfluss enthält. Anschließend können Sie Rangfolgeneinschränkungen anwenden, um die Ausführungsreihenfolge der Tasks zu steuern.  
  
 Im folgenden Diagramm wird ein Datenflusstask mit mehreren Datenflüssen angezeigt.  
  
 ![Datenflüsse](../../integration-services/control-flow/media/mw-dts-09.gif "Datenflüsse")  
  
## <a name="log-entries"></a>Protokolleinträge  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] stellt für alle Tasks einen Satz Protokollereignisse zur Verfügung. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] für viele Tasks benutzerdefinierte Protokolleinträge bereit. Weitere Informationen finden Sie unter [Integration Services-Protokollierung &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md). Der Datenflusstask enthält die folgenden benutzerdefinierten Protokolleinträge:  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**BufferSizeTuning**|Zeigt an, dass der Datenflusstask die Größe des Puffers geändert hat. Der Protokolleintrag beschreibt die Gründe für die Größenänderung und listet die temporäre neue Puffergröße auf.|  
|**OnPipelinePostEndOfRowset**|Gibt an, dass eine Komponente das Signal für das Ende des Rowsets erhalten hat. Dieses Signal wird durch den letzten Aufruf der **ProcessInput** -Methode festgelegt. Für jede Komponente im Datenfluss, die eine Eingabe verarbeitet, wird ein Eintrag geschrieben. Der Eintrag schließt den Namen der Komponente ein.|  
|**OnPipelinePostPrimeOutput**|Zeigt an, dass die Komponente ihren letzten Aufruf der **PrimeOutput** -Methode abgeschlossen hat. Je nach Datenfluss werden möglicherweise mehrere Protokolleinträge geschrieben. Wenn es sich bei der Komponente um eine Quelle handelt, bedeutet dieser Protokolleintrag, dass die Komponente die Zeilenverarbeitung abgeschlossen hat.|  
|**OnPipelinePreEndOfRowset**|Zeigt an, dass eine Komponente das Signal für das Ende des Rowsets erhalten soll. Dieses Signal wird durch den letzten Aufruf der **ProcessInput** -Methode festgelegt. Für jede Komponente im Datenfluss, die eine Eingabe verarbeitet, wird ein Eintrag geschrieben. Der Eintrag schließt den Namen der Komponente ein.|  
|**OnPipelinePrePrimeOutput**|Zeigt an, dass die Komponente einen Aufruf aus der **PrimeOutput** -Methode erhalten soll. Je nach Datenfluss werden möglicherweise mehrere Protokolleinträge geschrieben.|  
|**OnPipelineRowsSent**|Berichtet die Anzahl von Zeilen, die einer Komponenteneingabe durch einen Aufruf der **ProcessInput** -Methode bereitgestellt wurden. Der Protokolleintrag enthält den Komponentennamen.|  
|**PipelineBufferLeak**|Stellt Informationen zu Komponenten bereit, die Puffer aufrechterhalten haben, nachdem der Puffer-Manager beendet wurde. Aufrechterhaltene Puffer blockieren die Freigabe von Pufferressourcen und können Speicherverluste verursachen. Der Protokolleintrag stellt den Namen der Komponente und die ID des Puffers bereit.|  
|**PipelineComponentTime**|Meldet den Zeitaufwand in Millisekunden, den die Komponente für jeden der Hauptverarbeitungsschritte „Validate“, „PreExecute“, „PostExecute“, „ProcessInput“ und „ProcessOutput“ benötigt.|  
|**PipelineExecutionPlan**|Berichtet den Ausführungsplan des Datenflusses. Der Ausführungsplan stellt Informationen dazu bereit, wie Puffer an Komponenten gesendet werden. Diese Informationen beschreiben in Kombination mit dem PipelineExecutionTrees-Protokolleintrag, was innerhalb des Datenflusstasks geschieht.|  
|**PipelineExecutionTrees**|Berichtet die Ausführungsstrukturen des Layouts im Datenfluss. Die Datenfluss-Engine-Planung verwendet die Strukturen zum Erstellen des Ausführungsplans des Datenflusses.|  
|**PipelineInitialization**|Bietet Initialisierungsinformationen zu dem Task. Zu diesen Informationen gehören die Verzeichnisse für die temporäre Speicherung von BLOB-Daten, die Standardpuffergröße und die Zeilenanzahl in einem Puffer. Je nach der Konfiguration des Datenflusstasks werden möglicherweise mehrere Protokolleinträge geschrieben.|  
  
 Diese Protokolleinträge stellen bei jeder Ausführung eines Pakets eine Fülle von Informationen zur Ausführung des Datenflusstasks bereit. Wenn Sie die Pakete wiederholt ausführen, können Sie Informationen erfassen, die im Laufe der Zeit wichtige Verlaufsinformationen zu der vom Task ausgeführten Verarbeitung, zu Problemen, die die Leistung beeinträchtigen können, und zu dem vom Task verarbeiteten Datenvolumen bereitstellen.  
  
 Weitere Informationen zur Verwendung dieser Protokolleinträge zum Überwachen und Verbessern der Leistung des Datenflusses finden Sie in einem der folgenden Themen:  
  
-   [Leistungsindikatoren](../../integration-services/performance/performance-counters.md)  
  
-   [Funktionen für die Datenflussleistung](../../integration-services/data-flow/data-flow-performance-features.md)  
  
### <a name="sample-messages-from-a-data-flow-task"></a>Beispielmeldungen aus einem Datenflusstask  
 In der folgenden Tabelle werden Beispielmeldungen für Protokolleinträge für ein sehr einfaches Paket aufgelistet. Das Paket verwendet eine OLE DB-Quelle zum Extrahieren von Daten aus einer Tabelle, eine Transformation zum Sortieren, um die Daten zu sortieren, und ein OLE DB-Ziel, um die Daten in eine andere Tabelle zu schreiben.  
  
|Protokolleintrag|Meldungen|  
|---------------|--------------|  
|**BufferSizeTuning**|`Rows in buffer type 0 would cause a buffer size greater than the configured maximum. There will be only 9637 rows in buffers of this type.`<br /><br /> `Rows in buffer type 2 would cause a buffer size greater than the configured maximum. There will be only 9497 rows in buffers of this type.`<br /><br /> `Rows in buffer type 3 would cause a buffer size greater than the configured maximum. There will be only 9497 rows in buffers of this type.`|  
|**OnPipelinePostEndOfRowset**|`A component will be given the end of rowset signal. : 1180 : Sort : 1181 : Sort Input`<br /><br /> `A component will be given the end of rowset signal. : 1291 : OLE DB Destination : 1304 : OLE DB Destination Input`|  
|**OnPipelinePostPrimeOutput**|`A component has returned from its PrimeOutput call. : 1180 : Sort`<br /><br /> `A component has returned from its PrimeOutput call. : 1 : OLE DB Source`|  
|**OnPipelinePreEndOfRowset**|`A component has finished processing all of its rows. : 1180 : Sort : 1181 : Sort Input`<br /><br /> `A component has finished processing all of its rows. : 1291 : OLE DB Destination : 1304 : OLE DB Destination Input`|  
|**OnPipelinePrePrimeOutput**|`PrimeOutput will be called on a component. : 1180 : Sort`<br /><br /> `PrimeOutput will be called on a component. : 1 : OLE DB Source`|  
|**OnPipelineRowsSent**|`Rows were provided to a data flow component as input. :  : 1185 : OLE DB Source Output : 1180 : Sort : 1181 : Sort Input : 76`<br /><br /> `Rows were provided to a data flow component as input. :  : 1308 : Sort Output : 1291 : OLE DB Destination : 1304 : OLE DB Destination Input : 76`|  
|**PipelineComponentTime**|`The component "Calculate LineItemTotalCost" (3522) spent 356 milliseconds in ProcessInput.`<br /><br /> `The component "Sum Quantity and LineItemTotalCost" (3619) spent 79 milliseconds in ProcessInput.`<br /><br /> `The component "Calculate Average Cost" (3662) spent 16 milliseconds in ProcessInput.`<br /><br /> `The component "Sort by ProductID" (3717) spent 125 milliseconds in ProcessInput.`<br /><br /> `The component "Load Data" (3773) spent 0 milliseconds in ProcessInput.`<br /><br /> `The component "Extract Data" (3869) spent 688 milliseconds in PrimeOutput filling buffers on output "OLE DB Source Output" (3879).`<br /><br /> `The component "Sum Quantity and LineItemTotalCost" (3619) spent 141 milliseconds in PrimeOutput filling buffers on output "Aggregate Output 1" (3621).`<br /><br /> `The component "Sort by ProductID" (3717) spent 16 milliseconds in PrimeOutput filling buffers on output "Sort Output" (3719).`|  
|**PipelineExecutionPlan**|`SourceThread0`<br /><br /> `Drives: 1`<br /><br /> `Influences: 1180 1291`<br /><br /> `Output Work List`<br /><br /> `CreatePrimeBuffer of type 1 for output ID 11.`<br /><br /> `SetBufferListener: "WorkThread0" for input ID 1181`<br /><br /> `CreatePrimeBuffer of type 3 for output ID 12.`<br /><br /> `CallPrimeOutput on component "OLE DB Source" (1)`<br /><br /> `End Output Work List`<br /><br /> `End SourceThread0`<br /><br /> `WorkThread0`<br /><br /> `Drives: 1180`<br /><br /> `Influences: 1180 1291`<br /><br /> `Input Work list, input ID 1181 (1 EORs Expected)`<br /><br /> `CallProcessInput on input ID 1181 on component "Sort" (1180) for view type 2`<br /><br /> `End Input Work list for input 1181`<br /><br /> `Output Work List`<br /><br /> `CreatePrimeBuffer of type 4 for output ID 1182.`<br /><br /> `SetBufferListener: "WorkThread1" for input ID 1304`<br /><br /> `CallPrimeOutput on component "Sort" (1180)`<br /><br /> `End Output Work List`<br /><br /> `End WorkThread0`<br /><br /> `WorkThread1`<br /><br /> `Drives: 1291`<br /><br /> `Influences: 1291`<br /><br /> `Input Work list, input ID 1304 (1 EORs Expected)`<br /><br /> `CallProcessInput on input ID 1304 on component "OLE DB Destination" (1291) for view type 5`<br /><br /> `End Input Work list for input 1304`<br /><br /> `Output Work List`<br /><br /> `End Output Work List`<br /><br /> `End WorkThread1`|  
|**PipelineExecutionTrees**|`begin execution tree 0`<br /><br /> `output "OLE DB Source Output" (11)`<br /><br /> `input "Sort Input" (1181)`<br /><br /> `end execution tree 0`<br /><br /> `begin execution tree 1`<br /><br /> `output "OLE DB Source Error Output" (12)`<br /><br /> `end execution tree 1`<br /><br /> `begin execution tree 2`<br /><br /> `output "Sort Output" (1182)`<br /><br /> `input "OLE DB Destination Input" (1304)`<br /><br /> `output "OLE DB Destination Error Output" (1305)`<br /><br /> `end execution tree 2`|  
|**PipelineInitialization**|`No temporary BLOB data storage locations were provided. The buffer manager will consider the directories in the TEMP and TMP environment variables.`<br /><br /> `The default buffer size is 10485760 bytes.`<br /><br /> `Buffers will have 10000 rows by default`<br /><br /> `The data flow will not remove unused components because its RunInOptimizedMode property is set to false.`|  
  
 Bei vielen Protokollereignissen werden mehrere Einträge in das Protokoll geschrieben, und die Meldungen für eine Reihe von Protokolleinträgen enthalten komplexe Daten. Damit es einfacher wird, den Inhalt komplexer Meldungen zu verstehen und zu kommunizieren, können Sie den Meldungstext analysieren. Je nach Speicherort des Protokolls können Sie Transact-SQL-Anweisungen oder Skriptkomponenten verwenden, um den komplexen Text in Spalten oder andere zweckmäßigere Formate zu zerlegen.  
  
 Die folgende Tabelle enthält z. B. die als Spalten analysierte Meldung "Die Zeilen wurden als Eingabe für eine Datenflusskomponente bereitgestellt. :  : 1185 : Ausgabe der OLE DB-Quelle : 1180 : Sort : 1181 : Sortiereingabe : 76". Die Meldung wurde vom **OnPipelineRowsSent** -Ereignis geschrieben, als Zeilen von der OLE DB-Quelle an die Transformation zum Sortieren gesendet wurden.  
  
|Column|BESCHREIBUNG|Wert|  
|------------|-----------------|-----------|  
|**PathID**|Der Wert der **ID** -Eigenschaft des Pfads zwischen der OLE DB-Quelle und der Transformation zum Sortieren.|1185|  
|**PathName**|Der Wert der **Name** -Eigenschaft des Pfads.|Ausgabe der OLE DB-Quelle|  
|**ComponentID**|Der Wert der **ID** -Eigenschaft der Transformation zum Sortieren.|1180|  
|**ComponentName**|Der Wert der **Name** -Eigenschaft der Transformation zum Sortieren.|Sortieren|  
|**InputID**|Der Wert der **ID** -Eigenschaft der Eingabe der Transformation zum Sortieren.|1181|  
|**InputName**|Der Wert der **Name** -Eigenschaft der Eingabe der Transformation zum Sortieren.|Sortiereingabe|  
|**RowsSent**|Die Anzahl von Zeilen, die an die Eingabe der Transformation zum Sortieren gesendet wurden.|76|  
  
## <a name="configuration-of-the-data-flow-task"></a>Konfiguration des Datenflusstasks  
 Eigenschaften können Sie im Fenster **Eigenschaften** oder programmgesteuert festlegen.  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum Festlegen dieser Eigenschaften im Fenster **Eigenschaften** zu erhalten:  
  
-   [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-the-data-flow-task"></a>Programmgesteuerte Konfiguration des Datenflusstasks  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum programmgesteuerten Hinzufügen von Datenflusstasks zu Paketen und zum Festlegen von Datenflusseigenschaften anzuzeigen:  
  
-   [Programmgesteuertes Hinzufügen des Datenflusstasks](../../integration-services/building-packages-programmatically/adding-the-data-flow-task-programmatically.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="related-content"></a>Verwandte Inhalte  
 Video [Balanced Data Distributor](/previous-versions/dn912438(v=msdn.10))auf technet.microsoft.com  
  
