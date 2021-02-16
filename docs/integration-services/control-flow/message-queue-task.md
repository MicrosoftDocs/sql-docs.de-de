---
description: Message Queue Task
title: Nachrichtenwarteschlange (Task) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.messagequeuetask.f1
- sql13.dts.designer.msgqueuetask.general.f1
- sql13.dts.designer.msgqueuetask.send.f1
- sql13.dts.designer.msgqueuetask.receive.f1
- sql13.dts.designer.selectvariables.f1
helpviewer_keywords:
- Message Queue task [Integration Services]
- receiving messages
- messages [Integration Services]
- sending messages
ms.assetid: ae1d8fad-6649-4e93-b589-14a32d07da33
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e39187e32afbcf72bf1eb950f18d5f21cef38211
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348299"
---
# <a name="message-queue-task"></a>Message Queue Task

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Mit dem Task „Nachrichtenwarteschlange“ können Sie Message Queuing (auch als MSMQ bezeichnet) verwenden, um Nachrichten zwischen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Paketen zu senden und zu empfangen oder um Nachrichten an eine Anwendungswarteschlange zu senden, die von einer benutzerdefinierten Anwendung verarbeitet wird. Bei diesen Nachrichten kann es sich um einfachen Text, Dateien oder Variablen und deren Werte handeln.  
  
 Mit dem Task Nachrichtenwarteschlange können Sie Vorgänge im gesamten Unternehmen koordinieren. Nachrichten können in eine Warteschlange eingereiht und später übermittelt werden, falls das Ziel nicht verfügbar oder ausgelastet ist. Beispielsweise können mit diesem Task Nachrichten für den Offlinelaptopcomputer von Vertriebsmitarbeitern, die ihre Nachrichten beim Herstellen einer Verbindung mit dem Netzwerk erhalten, einer Warteschlange hinzugefügt werden. Der Task Nachrichtenwarteschlange kann für folgende Zwecke verwendet werden:  
  
-   Verzögern der Taskausführung bis zum Einchecken anderer Pakete. Beispielsweise sendet an jedem Einzelhandelsstandort ein Task Nachrichtenwarteschlange nach der nächtlichen Wartung eine Nachricht an Ihren Firmencomputer. Ein Paket, das auf dem Firmencomputer ausgeführt wird, enthält Tasks Nachrichtenwarteschlange, die auf die Nachricht von einem bestimmten Einzelhandelsstandort warten. Beim Eintreffen einer Nachricht werden von einem Task Daten von diesem Standort hochgeladen. Nach dem Einchecken aller Standorte berechnet das Paket die Gesamtbeträge.  
  
-   Senden von Datendateien an den Computer, der diese verarbeitet. Beispielsweise kann der Kassenstand eines Restaurants in einer Datendateinachricht an das Buchhaltungssystem des Unternehmens gesendet werden, um Daten zum Trinkgeld jedes Kellners zu extrahieren.  
  
-   Verteilen von Dateien im gesamten Unternehmen. Beispielsweise kann ein Paket einen Task Nachrichtenwarteschlange verwenden, um eine Paketdatei an einen anderen Computer zu senden. Ein Paket, das auf dem Zielcomputer ausgeführt wird, kann mit einem Task Nachrichtenwarteschlange das Paket lokal abrufen und speichern.  
  
 Beim Senden oder Empfangen von Nachrichten verwendet der Task Nachrichtenwarteschlange einen von vier Nachrichtentypen: Datendatei, Zeichenfolge, Zeichenfolgennachricht an Variable oder Variable. Der Nachrichtentyp Zeichenfolgennachricht an Variable kann nur zum Empfangen von Nachrichten verwendet werden.  
  
 Dieser Task verwendet einen MSMQ-Verbindungs-Manager, um eine Verbindung mit einer Nachrichtenwarteschlange herzustellen. Weitere Informationen finden Sie unter [MSMQ-Verbindungs-Manager](../../integration-services/connection-manager/msmq-connection-manager.md). Weitere Informationen zu Message Queuing finden Sie in der [MSDN Library](../../sql-server/index.yml).  
  
 Für den Task Nachrichtenwarteschlange muss der [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Dienst installiert sein. Manche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Komponenten, die Sie für die Installation auf der Seite **Zu installierende Komponenten** oder der Seite **Funktionsauswahl** des Installations-Assistenten für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auswählen können, installieren eine Teilmenge der [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Komponenten. Diese Komponenten sind für bestimmte Tasks hilfreich, aber die Funktionalität von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ist begrenzt. Beispielsweise installiert die [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] -Option [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Komponenten, die zum Entwerfen eines Pakets erforderlich sind, aber der [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Dienst wird nicht installiert. Deshalb kann der Task Nachrichtenwarteschlange nicht ausgeführt werden. Um eine vollständige Installation von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]sicherzustellen, müssen Sie [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] auf der Seite **Zu installierende Komponenten** auswählen. Weitere Informationen zum Installieren und Ausführen des Tasks „Nachrichtenwarteschlange“ finden Sie unter [Installieren von Integration Services](../../integration-services/install-windows/install-integration-services.md).  
  
> [!NOTE]  
>  Der Task Nachrichtenwarteschlange entspricht nicht dem Federal Information Processing Standard (FIPS) 140-2, wenn das Betriebssystem des Computers im FIPS-Modus konfiguriert ist und der Task die Verschlüsselung verwendet. Wenn der Task Nachrichtenwarteschlange keine Verschlüsselung verwendet, kann der Task erfolgreich ausgeführt werden.  
  
## <a name="message-types"></a>Nachrichtentypen  
 Es gibt folgende Möglichkeiten, um die Nachrichtentypen zu konfigurieren, die der Task Nachrichtenwarteschlange bereitstellt:  
  
-   **Data file** gibt an, dass eine Datei die Nachricht enthält. Wenn Sie Nachrichten empfangen, können Sie den Task so konfigurieren, dass die Datei gespeichert wird und eine vorhandene Datei überschreiben wird, und das Paket angeben, von dem der Task Nachrichten empfangen kann.  
  
-   **String** definiert die Nachricht als Zeichenfolge. Wenn Sie Nachrichten empfangen, können Sie den Task so konfigurieren, dass die empfangene Zeichenfolge mit einer benutzerdefinierten Zeichenfolge verglichen und abhängig vom Vergleich die entsprechende Maßnahme ergriffen wird. Ein Zeichenfolgenvergleich kann genau sein, die Groß-/Kleinschreibung beachten oder die Groß-/Kleinschreibung ignorieren sowie eine Teilzeichenfolge verwenden.  
  
-   **String message to variable** gibt die Quellnachricht als Zeichenfolge an, die an eine Zielvariable gesendet wird. Sie können den Task so konfigurieren, dass die empfangene Zeichenfolge mit einer benutzerdefinierten Zeichenfolge mithilfe eines genauen Vergleichs, eines Vergleichs mit Beachtung der Groß-/Kleinschreibung oder eines Teilzeichenfolge-Vergleichs verglichen wird. Dieser Nachrichtentyp ist nur verfügbar, wenn der Task Nachrichten empfängt.  
  
-   **Variable** gibt an, dass die Nachricht mindestens eine Variable enthält. Sie können den Task so konfigurieren, dass die in der Nachricht enthaltenen Namen der Variablen angegeben werden. Wenn Sie Nachrichten empfangen, können Sie den Task so konfigurieren, dass sowohl das Paket, von dem Nachrichten empfangen werden können, als auch die Variable, die das Ziel der Nachricht ist, angegeben werden.  
  
## <a name="sending-messages"></a>Senden von Nachrichten  
 Beim Konfigurieren des Tasks Nachrichtenwarteschlange zum Senden von Nachrichten können Sie einen der zurzeit von der Message Queuing-Technologie unterstützten Verschlüsselungsalgorithmen, RC2 und RC4, zum Verschlüsseln der Nachricht verwenden. Diese Verschlüsselungsalgorithmen werden inzwischen im Vergleich zu neueren Algorithmen, die von der Message Queuing-Technologie noch nicht unterstützt werden, beide als kryptografisch schwach betrachtet. Daher sollten Sie Ihren Kryptografiebedarf sorgfältig überdenken, wenn Sie Nachrichten mithilfe des Tasks Nachrichtenwarteschlange senden.  
  
## <a name="receiving-messages"></a>Empfangen von Nachrichten  
 Beim Empfangen von Nachrichten kann der Task Nachrichtenwarteschlange wie folgt konfiguriert werden.  
  
-   Umgehen der Nachricht oder Entfernen der Nachricht aus der Warteschlange.  
  
-   Angeben eines Timeouts.  
  
-   Fehler bei einem Timeout.  
  
-   Überschreiben einer vorhandenen Datei, falls die Nachricht in einer **Data file** gespeichert ist.  
  
-   Speichern der Nachrichtendatei unter einem anderen Dateinamen, falls die Nachricht den Typ **Data file message** verwendet.  
  
## <a name="custom-logging-messages-available-on-the-message-queue-task"></a>Verfügbare benutzerdefinierte Meldungen für die Protokollierung für den Task 'Nachrichtenwarteschlange'  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den Task Nachrichtenwarteschlange aufgelistet. Weitere Informationen finden Sie unter [Integration Services-Protokollierung &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**MSMQAfterOpen**|Zeigt an, dass das Öffnen der Warteschlange beendet wurde.|  
|**MSMQBeforeOpen**|Zeigt an, dass das Öffnen der Warteschlange begonnen wurde.|  
|**MSMQBeginReceive**|Zeigt an, dass das Empfangen einer Meldung begonnen wurde.|  
|**MSMQBeginSend**|Zeigt an, dass das Senden einer Meldung begonnen wurde.|  
|**MSMQEndReceive**|Zeigt an, dass das Empfangen einer Meldung beendet wurde.|  
|**MSMQEndSend**|Zeigt an, dass das Senden einer Meldung beendet wurde.|  
|**MSMQTaskInfo**|Enthält beschreibende Informationen zum Task.|  
|**MSMQTaskTimeOut**|Zeigt an, dass beim Task ein Timeout eingetreten ist.|  
  
## <a name="configuration-of-the-message-queue-task"></a>Konfiguration des Tasks "Nachrichtenwarteschlange"  
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen. Klicken Sie auf das folgende Thema, um Informationen zu den Eigenschaften zu erhalten, die Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen können:  
  
-   [Seite Ausdrücke](../../integration-services/expressions/expressions-page.md)  
  
 Weitere Informationen zum programmgesteuerten Festlegen dieser Eigenschaften finden Sie in der Dokumentation zur **Microsoft.SqlServer.Dts.Tasks.MessageQueueTask.MessageQueueTask** -Klasse im Entwicklerhandbuch.  
  
## <a name="related-tasks"></a>Related Tasks  
 Weitere Informationen zum Anzeigen dieser Eigenschaften im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer finden Sie unter [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
## <a name="message-queue-task-editor-general-page"></a>Editor für den Task 'Nachrichtenwarteschlange' (Seite Allgemein)
  Auf der Seite **Allgemein** des Dialogfelds **Editor für den Task „Nachrichtenwarteschlange“** können Sie den Task „Nachrichtenwarteschlange“ benennen und beschreiben, das Nachrichtenformat angeben und kennzeichnen, ob vom Task Nachrichten gesendet oder empfangen werden.  
  
### <a name="options"></a>Tastatur  
 **Name**  
 Geben Sie einen eindeutigen Namen für den Task Nachrichtenwarteschlange an. Dieser Name wird im Tasksymbol als Bezeichnung verwendet.  
  
> [!NOTE]  
>  Tasknamen müssen innerhalb eines Pakets eindeutig sein.  
  
 **Beschreibung**  
 Geben Sie eine Beschreibung des Tasks Nachrichtenwarteschlange ein.  
  
 **Use2000Format**  
 Geben Sie an, ob das 2000-Format von Message Queuing (auch bekannt als MSMQ) verwendet werden soll. Der Standardwert ist **False**.  
  
 **MSMQConnection**  
 Wählen Sie einen vorhandenen MSMQ-Verbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [MSMQ-Verbindungs-Manager](../../integration-services/connection-manager/msmq-connection-manager.md), [MSMQ-Verbindungs-Manager-Editor](../connection-manager/msmq-connection-manager.md)  
  
 **Meldung**  
 Geben Sie an, ob Nachrichten vom Task Nachrichtenwarteschlange gesendet oder empfangen werden. Wenn Sie **Nachricht senden** auswählen, wird im linken Bereich des Dialogfelds die Seite Senden angezeigt; wenn Sie **Nachricht empfangen** auswählen, wird die Seite Empfangen aufgelistet. Standardmäßig ist dieser Wert auf **Nachricht senden** festgelegt.  
  
## <a name="message-queue-task-editor-send-page"></a>Task 'Nachrichtenwarteschlange' (Seite Senden)
  Im Dialogfeld **Editor für den Task „Nachrichtenwarteschlange“** können Sie auf der Seite **Senden** den Task „Nachrichtenwarteschlange“ so konfigurieren, dass Nachrichten von einem [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Paket gesendet werden.  
  
### <a name="options"></a>Tastatur  
 **UseEncryption**  
 Geben Sie an, ob die Nachricht verschlüsselt werden soll. Der Standardwert ist **False**.  
  
 **EncryptionAlgorithm**  
 Wenn Sie die Verschlüsselung verwenden möchten, müssen Sie den Namen des verwendeten Verschlüsselungsalgorithmus angeben. Für den Task "Nachrichtenwarteschlange" können die Algorithmen RC2 und RC4 verwendet werden. Die Standardeinstellung lautet **RC2**.  
  
> [!NOTE]  
>  Der RC4-Algorithmus wird nur aus Gründen der Abwärtskompatibilität unterstützt. Neues Material kann nur mit RC4 oder RC4_128 verschlüsselt werden, wenn die Datenbank den Kompatibilitätsgrad 90 oder 100 besitzt. (Nicht empfohlen.) Verwenden Sie stattdessen einen neueren Algorithmus, z. B. einen der AES-Algorithmen. In der aktuellen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]kann mit RC4 oder RC4_128 verschlüsseltes Material in jedem Kompatibilitätsgrad entschlüsselt werden.  
  
> [!IMPORTANT]  
>  Diese Verschlüsselungsalgorithmen werden von der Message Queuing-Technologie (auch bekannt als MSMQ) unterstützt. Diese Verschlüsselungsalgorithmen werden inzwischen im Vergleich zu neueren Algorithmen, die von Message Queuing noch nicht unterstützt werden, beide als kryptografisch schwach betrachtet. Daher sollten Sie Ihren Kryptografiebedarf sorgfältig überdenken, wenn Sie Nachrichten mithilfe des Tasks Nachrichtenwarteschlange senden.  
  
 **MessageType**  
 Wählen Sie den Nachrichtentyp aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Data file message**|Die Nachricht wird in einer Datei gespeichert. Bei Auswahl dieses Wertes wird die dynamische Option **DataFileMessage** angezeigt.|  
|**Variable message**|Die Nachricht wird in einer Variable gespeichert. Bei Auswahl dieses Wertes wird die dynamische Option **VariableMessage** angezeigt.|  
|**String message**|Die Nachricht wird im Task 'Nachrichtenwarteschlange' gespeichert. Bei Auswahl dieses Wertes wird die dynamische Option **StringMessage** angezeigt.|  
  
### <a name="messagetype-dynamic-options"></a>MessageType (dynamische Optionen)  
  
#### <a name="messagetype--data-file-message"></a>MessageType = Data file message  
 **DataFileMessage**  
 Geben Sie den Pfad der Datendatei an, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(…)** , um nach der Datei zu suchen.  
  
#### <a name="messagetype--variable-message"></a>MessageType = Variable message  
 **VariableMessage**  
 Geben Sie die Variablennamen ein, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(…)** , und wählen Sie dann die Variablen aus. Die Variablen werden durch Kommas getrennt.  
  
 **Verwandte Themen:** Variablen auswählen  
  
#### <a name="messagetype--string-message"></a>MessageType = Zeichenfolgennachricht  
 **StringMessage**  
 Geben Sie die Zeichenfolgennachricht ein, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(…)** , und geben Sie dann im Dialogfeld **Zeichenfolgennachricht eingeben** die Nachricht ein.  
  
## <a name="message-queue-task-editor-receive-page"></a>Editor für den Task 'Nachrichtenwarteschlange' (Seite Empfangen)
  Auf der Seite **Empfangen** des Dialogfelds **Editor für den Task „Nachrichtenwarteschlange“** können Sie den Task „Nachrichtenwarteschlange“ konfigurieren, um MSMQ-Nachrichten ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing) zu empfangen.  
  
### <a name="options"></a>Tastatur  
 **RemoveFromMessageQueue**  
 Geben Sie an, ob die Nachricht aus der Warteschlange entfernt werden soll, nachdem sie empfangen wurde. Standardmäßig ist dieser Wert auf **False** festgelegt.  
  
 **ErrorIfMessageTimeOut**  
 Geben Sie an, ob der Task fehlschlägt und eine Fehlermeldung angezeigt wird, wenn die Zeit für die Nachricht überschritten wird. Der Standardwert ist **False**.  
  
 **TimeoutAfter**  
 Wenn eine Fehlermeldung beim Fehlschlagen des Tasks angezeigt werden soll, geben Sie hier die Wartezeit vor dem Anzeigen der Timeoutmeldung in Sekunden an.  
  
 **MessageType**  
 Wählen Sie den Nachrichtentyp aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Data file message**|Die Nachricht wird in einer Datei gespeichert. Bei Auswahl dieses Wertes wird die dynamische Option **DataFileMessage** angezeigt.|  
|**Variable message**|Die Nachricht wird in einer Variable gespeichert. Bei Auswahl dieses Wertes wird die dynamische Option **VariableMessage** angezeigt.|  
|**String message**|Die Nachricht wird im Task 'Nachrichtenwarteschlange' gespeichert. Bei Auswahl dieses Wertes wird die dynamische Option **StringMessage** angezeigt.|  
|**String message to variable**|Die Nachricht wurde in einer Variablen gespeichert.<br /><br /> Bei Auswahl dieses Wertes wird die dynamische Option **StringMessage** angezeigt.|  
  
### <a name="messagetype-dynamic-options"></a>MessageType (dynamische Optionen)  
  
#### <a name="messagetype--data-file-message"></a>MessageType = Data file message  
 **SaveFileAs**  
 Geben Sie den Pfad der zu verwendenden Datei an, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(...)** , um nach der Datei zu suchen.  
  
 **Overwrite**  
 Geben Sie an, dass die Daten in einer vorhandenen Datei überschrieben werden sollen, wenn der Inhalt einer Datendateinachricht gespeichert wird. Der Standardwert ist **False**.  
  
 **Filter**  
 Geben Sie an, ob auf die Nachricht ein Filter angewendet werden soll. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Kein Filter**|Der Task filtert keine Nachrichten. Wenn Sie diesen Wert auswählen, wird die dynamische Option **IdentifierReadOnly** angezeigt.|  
|**Vom Paket**|Der Task empfängt nur Nachrichten von dem angegebenen Paket. Wenn Sie diesen Wert auswählen, wird die dynamische Option **Identifier** angezeigt.|  
  
#### <a name="filter-dynamic-options"></a>Filter (dynamische Optionen)  
  
##### <a name="filter--no-filter"></a>Filter = No filter  
 **IdentifierReadOnly**  
 Diese Option ist schreibgeschützt. Sie kann leer sein oder die GUID eines Pakets enthalten, wenn die Eigenschaft Filter vorher festgelegt wurde.  
  
##### <a name="filter--from-package"></a>Filter = From package  
 **Bezeichner**  
 Wenn Sie einen Filter anwenden möchten, geben Sie den eindeutigen Bezeichner des Pakets ein, aus dem die Nachrichten empfangen werden können, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(...)** , und geben Sie das Paket an.  
  
 **Verwandte Themen:** [Paket auswählen](../../integration-services/control-flow/select-a-package.md)  
  
#### <a name="messagetype--variable-message"></a>MessageType = Variable message  
 **Filter**  
 Geben Sie an, ob auf Nachrichten ein Filter angewendet werden soll. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Kein Filter**|Der Task filtert keine Nachrichten. Wenn Sie diesen Wert auswählen, wird die dynamische Option **IdentifierReadOnly** angezeigt.|  
|**Vom Paket**|Der Task empfängt nur Nachrichten von dem angegebenen Paket. Wenn Sie diesen Wert auswählen, wird die dynamische Option **Identifier** angezeigt.|  
  
 **Variable**  
 Geben Sie den Variablennamen ein, oder klicken Sie auf \<**New variable...**>, und konfigurieren Sie dann eine neue Variable.  
  
 **Verwandte Themen:** [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
#### <a name="filter-dynamic-options"></a>Filter (dynamische Optionen)  
  
##### <a name="filter--no-filter"></a>Filter = No filter  
 **IdentifierReadOnly**  
 Diese Option ist leer.  
  
##### <a name="filter--from-package"></a>Filter = From package  
 **Bezeichner**  
 Wenn Sie einen Filter anwenden möchten, geben Sie den eindeutigen Bezeichner des Pakets ein, aus dem die Nachrichten empfangen werden können, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(...)** , und geben Sie das Paket an.  
  
 **Verwandte Themen:** [Paket auswählen](../../integration-services/control-flow/select-a-package.md)  
  
#### <a name="messagetype--string-message"></a>MessageType = Zeichenfolgennachricht  
 **Vergleichen**  
 Geben Sie an, ob auf Nachrichten ein Filter angewendet werden soll. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**None**|Die Nachrichten werden nicht verglichen.|  
|**Genaue Übereinstimmung**|Die Nachrichten müssen genau mit der Zeichenfolge in der Option **CompareString** übereinstimmen.|  
|**Groß-/Kleinschreibung ignorieren**|Die Nachricht muss mit der Zeichenfolge in der Option **CompareString** übereinstimmen, aber beim Vergleichen wird nicht zwischen Groß- und Kleinschreibung unterschieden.|  
|**Mit Inhalt**|Die Nachrichten müssen die Zeichenfolge in der Option **CompareString** enthalten.|  
  
 **CompareString**  
 Geben Sie hier die Zeichenfolge an, mit der die Nachricht verglichen wird, wenn die Option **Vergleichen** nicht auf **Keine** festgelegt ist.  
  
#### <a name="messagetype--string-message-to-variable"></a>MessageType = String message to variable  
 **Vergleichen**  
 Geben Sie an, ob auf Nachrichten ein Filter angewendet werden soll. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**None**|Die Nachrichten werden nicht verglichen.|  
|**Genaue Übereinstimmung**|Die Nachricht muss genau mit der Zeichenfolge in der Option **CompareString** übereinstimmen.|  
|**Groß-/Kleinschreibung ignorieren**|Die Nachricht muss mit der Zeichenfolge in der Option **CompareString** übereinstimmen, aber beim Vergleichen wird nicht zwischen Groß- und Kleinschreibung unterschieden.|  
|**Mit Inhalt**|Die Nachricht muss die Zeichenfolge in der Option **CompareString** enthalten.|  
  
 **CompareString**  
 Geben Sie hier die Zeichenfolge an, mit der die Nachricht verglichen wird, wenn die Option **Vergleichen** nicht auf **Keine** festgelegt ist.  
  
 **Variable**  
 Geben Sie den Namen der Variablen ein, die die empfangene Nachricht enthalten soll, oder klicken Sie auf \<**New variable...**>, und konfigurieren Sie dann eine neue Variable.  
  
 **Verwandte Themen:** [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
## <a name="select-variables"></a>Variablen auswählen
  Mithilfe des Dialogfelds **Variablen auswählen** geben Sie die Variablen an, die beim Vorgang des Sendens einer Nachricht im Task Nachrichtenwarteschlange verwendet werden. Die Liste **Verfügbare Variablen** enthält Systemvariablen und benutzerdefinierte Variablen, die sich auf den Task „Nachrichtenwarteschlange“ oder dessen übergeordneten Container beziehen. Der Task verwendet die Variablen der Liste **Ausgewählte Variablen** .  
  
### <a name="options"></a>Tastatur  
 **Verfügbare Variablen**  
 Wählen Sie eine oder mehrere Variablen aus.  
  
 **Ausgewählte Variablen**  
 Wählen Sie eine oder mehrere Variablen aus.  
  
 **Pfeile nach rechts**  
 Verschiebt die ausgewählten Variablen in die Liste **Ausgewählte Variablen** .  
  
 **Pfeile nach links**  
 Verschiebt die ausgewählten Variablen wieder in die Liste **Verfügbare Variablen** .  
  
 **Neue Variable**  
 Erstellt eine neue Variable.  
  
 **Verwandte Themen:** [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Tasks](../../integration-services/control-flow/integration-services-tasks.md)   
 [Ablaufsteuerung](../../integration-services/control-flow/control-flow.md)  
  
