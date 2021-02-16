---
description: Task Dateisystem
title: Task „Dateisystem“ | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.filesystemtask.f1
- sql13.dts.designer.filesystemtask.general.f1
helpviewer_keywords:
- File System task [Integration Services]
ms.assetid: 7dd79a6a-e066-4028-a385-1d40f31056f8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 51646edb5b2937d04d4915e2a7d4a66327343a6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345711"
---
# <a name="file-system-task"></a>Task Dateisystem

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Der Task Dateisystem führt Operationen für Dateien und Verzeichnisse im Dateisystem aus. Beispielsweise kann ein Paket mit dem Task Dateisystem Verzeichnisse und Dateien erstellen, verschieben oder löschen. Darüber hinaus können Sie mit dem Task Dateisystem Attribute für Dateien und Verzeichnisse festlegen. Beispielsweise können mit dem Task Dateisystem Dateien als ausgeblendet oder schreibgeschützt festgelegt werden.  
  
 Für alle Operationen mit dem Task Dateisystem wird eine Quelle verwendet, wobei es sich um eine Datei oder ein Verzeichnis handeln kann. Beispiele für Quellen sind die Datei, die von dem Task kopiert wird, oder das Verzeichnis, das von dem Task gelöscht wird. Die Quelle kann mithilfe eines Dateiverbindungs-Managers angegeben werden, der auf das Verzeichnis oder die Datei verweist, oder durch Eingabe des Namens einer Variablen, die den Quellpfad enthält. Weitere Informationen finden Sie unter [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md) und [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md).  
  
 Für die Vorgänge zum Kopieren und Verschieben von Dateien und Verzeichnissen und zum Umbenennen von Dateien wird ein Ziel und eine Quelle verwendet. Das Ziel wird mithilfe eines Dateiverbindungs-Managers oder einer Variablen angegeben. Vorgänge mit dem Task Dateisystem können so konfiguriert werden, dass Zieldateien und -verzeichnisse überschrieben werden dürfen. Der Vorgang, bei dem ein neues Verzeichnis erstellt wird, kann so konfiguriert werden, dass ein vorhandenes Verzeichnis verwendet wird. Dieses weist den angegebenen Namen auf, und es wird kein Fehler erzeugt, wenn das Verzeichnis bereits besteht.  
  
## <a name="predefined-file-system-operations"></a>Vordefinierte Dateisystemvorgänge  
 Der Task Dateisystem enthält vordefinierte Vorgänge. In der folgenden Tabelle werden diese Vorgänge beschrieben.  
  
|Vorgang|BESCHREIBUNG|  
|---------------|-----------------|  
|Verzeichnis kopieren|Kopiert einen Ordner zwischen Speicherorten.|  
|Datei kopieren|Kopiert eine Datei zwischen Speicherorten.|  
|Erstellen eines Verzeichnisses|Erstellt einen Ordner im angegebenen Speicherort.|  
|Verzeichnis löschen|Löscht einen Ordner im angegebenen Speicherort.|  
|Verzeichnisinhalt löschen|Löscht alle Dateien und Ordner in einem Ordner.|  
|Datei löschen|Löscht eine Datei im angegebenen Speicherort.|  
|Verzeichnis verschieben|Verschiebt einen Ordner zwischen Speicherorten.|  
|Datei verschieben|Verschiebt eine Datei zwischen Speicherorten.|  
|Datei umbenennen|Benennt eine Datei im angegebenen Speicherort um.|  
|Festlegen von Attributen|Legt Attribute für Dateien und Ordner fest. Zu den Attributen zählen Archive, Hidden, Normal, ReadOnly und System. Normalerweise sind keine Attribute angegeben, und die Kombination mit anderen Attributen ist nicht möglich. Alle anderen Attribute können in Kombination mit anderen Attributen verwendet werden.|  
  
 Der Task Dateisystem wird in einer einzelnen Datei oder in einem einzelnen Verzeichnis ausgeführt. Daher unterstützt dieser Task nicht die Verwendung von Platzhalterzeichen, um denselben Vorgang in mehreren Dateien auszuführen. Damit der Task Dateisystem einen Vorgang in mehreren Dateien oder Verzeichnissen wiederholt, platzieren Sie den Task Dateisystem in einem Foreach-Schleifencontainer, wie in den folgenden Schritten beschrieben.  
  
-   **Konfigurieren des Foreach-Schleifencontainers** Legen Sie auf der Seite **Auflistung** des Foreach-Schleifen-Editors den Enumerator auf **Foreach-Dateienumerator** fest, und geben Sie den Platzhalterausdruck als Enumeratorkonfiguration für **Dateien** an. Ordnen Sie auf der Seite **Variablenzuordnungen** des Foreach-Schleifen-Editors eine Variable zu, mit der die Dateinamen nacheinander an den Task Dateisystem übergeben werden.  
  
-   **Hinzufügen und Konfigurieren eines Tasks 'Dateisystem'** Fügen Sie dem Foreach-Schleifencontainer einen Task Dateisystem hinzu. Legen Sie auf der Seite **Allgemein** des Editors für den Task **Dateisystem** die Eigenschaft **SourceVariable** oder DestinationVariable auf die im Foreach-Schleifencontainer definierte Variable fest.  
  
## <a name="custom-log-entries-available-on-the-file-system-task"></a>Verfügbare benutzerdefinierte Protokolleinträge für den Task 'Dateisystem'  
 In der folgenden Tabelle wird der benutzerdefinierte Protokolleintrag für den Task "Dateisystem" beschrieben. Weitere Informationen finden Sie unter [Integration Services-Protokollierung &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**FileSystemOperation**|Berichtet den vom Task durchgeführten Vorgang. Der Protokolleintrag wird geschrieben, wenn der Dateisystemvorgang begonnen wird, und schließt Informationen über die Quelle und das Ziel ein.|  
  
## <a name="configuring-the-file-system-task"></a>Konfigurieren des Tasks Dateisystem  
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
 Klicken Sie auf die folgenden Themen, um weitere Informationen zu den Eigenschaften zu erhalten, die Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen können:  
  
-   [Editor für den Task „Dateisystem“ &#40;Seite „Allgemein“&#41;]()  
  
-   [Seite Ausdrücke](../../integration-services/expressions/expressions-page.md)  
  
 Weitere Informationen zum Festlegen dieser Eigenschaften im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer finden Sie im folgenden Thema:  
  
-   [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
 Weitere Informationen zum programmgesteuert Festlegen dieser Eigenschaften finden Sie im folgenden Thema:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] schließt einen Task zum Herunterladen und Hochladen von Datendateien und zum Verwalten von Verzeichnissen auf Servern ein. Weitere Informationen finden Sie unter [FTP Task](../../integration-services/control-flow/ftp-task.md).  
  
## <a name="file-system-task-editor-general-page"></a>Editor für den Task 'Dateisystem' (Seite Allgemein)
  Mithilfe der Seite **Allgemein** des Dialogfelds **Editor für den Task 'Dateisystem'** können Sie den Dateisystemvorgang konfigurieren, der durch den Task ausgeführt wird.  
  
 Sie müssen durch Festlegen der Eigenschaften SourceConnection und DestinationConnection einen Quell- und einen Zielverbindungs-Manager angeben. Sie können entweder die Namen von Dateiverbindungs-Managern bereitstellen, die auf die Dateien zeigen, die der Task als Quelle oder Ziel verwendet. Wenn die Pfade der Dateien in Variablen gespeichert sind, können Sie alternativ auch die Namen der Variablen bereitstellen. Wenn Sie Variablen zum Speichern der Dateipfade verwenden möchten, müssen Sie zuerst die Option IsSourcePathVariable für die Quellverbindung und die Option IsDestinationPathVariable für die Zielverbindung auf **True** festlegen. Sie können dann die vorhandenen System- oder benutzerdefinierten Variablen zur Verwendung auswählen oder neue Variablen erstellen. Im Dialogfeld **Variable hinzufügen** können Sie den Bereich der Variablen konfigurieren und angeben. Der Bereich muss der Task Dateisystem oder ein übergeordneter Container sein. Weitere Informationen finden Sie unter [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) und [Verwenden von Variablen in Paketen](../integration-services-ssis-variables.md).  
  
> [!NOTE]  
>  Um die für die **SourceConnection** -Eigenschaft und die **DestinationConnection** -Eigenschaft ausgewählten Variablen zu überschreiben, geben Sie einen Ausdruck für die **Source** -Eigenschaft und einen für die **Destination** -Eigenschaft ein. Die Ausdrücke werden im **Editor für den Task 'Dateisystem'** auf der Seite **Ausdrücke** eingegeben. Wenn Sie beispielsweise den Pfad der Dateien, die vom Task verwendet werden, als Ziel festlegen möchten, kann in einigen Fällen die Variable A und in anderen die Variable B besser geeignet sein.  
  
> [!NOTE]  
>  Der Task Dateisystem wird in einer einzelnen Datei oder in einem einzelnen Verzeichnis ausgeführt. Daher unterstützt dieser Task nicht die Verwendung von Platzhalterzeichen, um denselben Vorgang in mehreren Dateien oder Verzeichnissen auszuführen. Damit der Task Dateisystem einen Vorgang in mehreren Dateien oder Verzeichnissen wiederholt, platzieren Sie den Task Dateisystem in einem Foreach-Schleifencontainer. Weitere Informationen finden Sie unter [File System Task](../../integration-services/control-flow/file-system-task.md).  
  
 Sie können Ausdrücke verwenden, um verschiedene Variablen einzusetzen.  
  
### <a name="options"></a>Tastatur  
 **IsDestinationPathVariable**  
 Geben Sie an, ob der Zielpfad in einer Variablen gespeichert ist. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Wahr**|Der Zielpfad ist in einer Variablen gespeichert. Wenn Sie diesen Wert auswählen, wird die dynamische Option **DestinationVariable** angezeigt.|  
|**False**|Der Zielpfad wird in einem Dateiverbindungs-Manager angegeben. Bei Auswahl dieses Wertes wird die dynamische Option **DestinationConnection** angezeigt.|  
  
 **OverwriteDestination**  
 Geben Sie an, ob der Vorgang Dateien im Zielverzeichnis überschreiben kann.  
  
 **Name**  
 Geben Sie einen eindeutigen Namen für den Task Dateisystem an. Dieser Name wird im Tasksymbol als Bezeichnung verwendet.  
  
> [!NOTE]  
>  Tasknamen müssen innerhalb eines Pakets eindeutig sein.  
  
 **Beschreibung**  
 Geben Sie eine Beschreibung des Tasks Dateisystem ein.  
  
 **Vorgang**  
 Wählen Sie den auszuführenden Dateisystemvorgang aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Verzeichnis kopieren**|Kopieren Sie ein Verzeichnis. Bei Auswahl dieses Wertes werden die dynamischen Optionen für eine Quelle und ein Ziel angezeigt.|  
|**Datei kopieren**|Kopieren Sie eine Datei. Bei Auswahl dieses Wertes werden die dynamischen Optionen für eine Quelle und ein Ziel angezeigt.|  
|**Verzeichnis erstellen**|Erstellen Sie ein Verzeichnis. Bei Auswahl dieses Wertes werden die dynamischen Optionen für ein Quell- und ein Zielverzeichnis angezeigt.|  
|**Verzeichnis löschen**|Löschen Sie ein Verzeichnis. Bei Auswahl dieses Wertes werden die dynamischen Optionen für eine Quelle angezeigt.|  
|**Verzeichnisinhalt löschen**|Löschen Sie den Inhalt eines Verzeichnisses. Bei Auswahl dieses Wertes werden die dynamischen Optionen für eine Quelle angezeigt.|  
|**Datei löschen**|Löschen Sie eine Datei. Bei Auswahl dieses Wertes werden die dynamischen Optionen für eine Quelle angezeigt.|  
|**Verzeichnis verschieben**|Verschieben Sie ein Verzeichnis. Bei Auswahl dieses Wertes werden die dynamischen Optionen für eine Quelle und ein Ziel angezeigt.|  
|**Datei verschieben**|Verschieben Sie eine Datei. Bei Auswahl dieses Wertes werden die dynamischen Optionen für eine Quelle und ein Ziel angezeigt. Schließen Sie beim Verschieben einer Datei keinen Dateinamen in den Verzeichnispfad ein, den Sie als Ziel angeben.|  
|**Datei umbenennen**|Benennen Sie eine Datei um. Bei Auswahl dieses Wertes werden die dynamischen Optionen für eine Quelle und ein Ziel angezeigt. Schließen Sie beim Umbenennen einer Datei den neuen Dateinamen in den Verzeichnispfad ein, den Sie als Ziel angeben.|  
|**Attribute festlegen**|Legen Sie die Attribute einer Datei oder eines Verzeichnisses fest. Bei Auswahl dieses Wertes werden die dynamischen Optionen für eine Quelle und einen Vorgang angezeigt.|  
  
 **IsSourcePathVariable**  
 Geben Sie an, ob der Zielpfad in einer Variablen gespeichert ist. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert||  
|-----------|-|  
|**Wahr**|Der Zielpfad ist in einer Variablen gespeichert. Bei Auswahl dieses Wertes wird die dynamische Option **SourceVariable** angezeigt.|  
|**False**|Der Zielpfad wird in einem Dateiverbindungs-Manager angegeben. Wenn Sie diesen Wert auswählen, wird die dynamische Option **DestinationVariable** angezeigt.|  
  
### <a name="isdestinationpathvariable-dynamic-options"></a>IsDestinationPathVariable (dynamische Optionen)  
  
#### <a name="isdestinationpathvariable--true"></a>IsDestinationPathVariable = True  
 **DestinationVariable**  
 Wählen Sie den Variablennamen in der Liste aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
#### <a name="isdestinationpathvariable--false"></a>IsDestinationPathVariable = False  
 **DestinationConnection**  
 Wählen Sie einen Dateiverbindungs-Manager aus der Liste aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
### <a name="issourcepathvariable-dynamic-options"></a>IsSourcePathVariable (dynamische Optionen)  
  
#### <a name="issourcepathvariable--true"></a>IsSourcePathVariable = True  
 **SourceVariable**  
 Wählen Sie den Variablennamen in der Liste aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
#### <a name="issourcepathvariable--false"></a>IsSourcePathVariable = False  
 **SourceConnection**  
 Wählen Sie einen Dateiverbindungs-Manager aus der Liste aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md)  
  
### <a name="operation-dynamic-options"></a>Operation (dynamische Optionen)  
  
#### <a name="operation--set-attributes"></a>Operation = Set Attributes  
 **Hidden**  
 Geben Sie an, ob die Datei oder das Verzeichnis angezeigt wird.  
  
 **ReadOnly**  
 Geben Sie an, ob die Datei schreibgeschützt ist.  
  
 **Archivieren**  
 Geben Sie an, ob die Datei oder das Verzeichnis archiviert werden kann.  
  
 **System**  
 Geben Sie an, ob die Datei eine Betriebssystemdatei ist.  
  
#### <a name="operation--create-directory"></a>Operation = Create directory  
 **UseDirectoryIfExists**  
 Gibt an, ob der Vorgang **Verzeichnis erstellen** ein vorhandenes Verzeichnis mit dem angegebenen Namen verwendet, statt ein neues Verzeichnis zu erstellen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Tasks](../../integration-services/control-flow/integration-services-tasks.md)   
 [Ablaufsteuerung](../../integration-services/control-flow/control-flow.md)  
  
