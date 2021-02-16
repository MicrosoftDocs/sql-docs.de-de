---
description: XML-Task
title: XML-Task | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.xmltask.f1
- sql13.dts.designer.xmltask.general.f1
helpviewer_keywords:
- XML [Integration Services]
- XML task [Integration Services]
ms.assetid: 9f761846-390e-46d5-9db7-858943d40849
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f3b2b309d07e14347aa2d89038c897b3db804c9b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354401"
---
# <a name="xml-task"></a>XML-Task

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Der XML-Task wird für XML-Daten verwendet. Mit diesem Task kann ein Paket XML-Dokumente abrufen, mithilfe von XSLT-Stylesheets (Extensible Stylesheet Language Transformations) und XPath-Ausdrücken Vorgänge auf die Dokumente anwenden, mehrere Dokumente zusammenführen oder die aktualisierten Dokumente mit Dateien und Variablen überprüfen, vergleichen und speichern.  
  
 Mit diesem Task kann ein [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paket XML-Dokumente zur Laufzeit dynamisch ändern. Der XML-Task kann für folgende Zwecke verwendet werden:  
  
-   Neuformatieren eines XML-Dokuments. Beispielsweise kann der Task auf einen Bericht in einer XML-Datei zugreifen und ein XSLT-Stylesheet dynamisch anwenden, um die Dokumentpräsentation anzupassen.  
  
-   Auswählen von Abschnitten eines XML-Dokuments. Beispielsweise kann der Task auf einen Bericht in einer XML-Datei zugreifen und einen XPath-Ausdruck dynamisch anwenden, um einen Abschnitt des Dokuments auszuwählen. Mit dem Vorgang können auch Werte im Dokument abgerufen und verarbeitet werden.  
  
-   Zusammenführen von Dokumenten aus vielen Quellen. Beispielsweise können mit dem Task Berichte aus mehreren Quellen heruntergeladen und dynamisch zu einem umfassenden XML-Dokument zusammengeführt werden.  
  
-   Validieren eines XML-Dokuments und Erhalt einer ausführlichen Fehlerausgabe (optional). Weitere Informationen finden Sie unter [Validate XML with the XML Task](../../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
 Sie können XML-Daten in einen Datenfluss einschließen, indem Sie mithilfe einer XML-Quelle Werte aus einem XML-Dokument extrahieren. Weitere Informationen finden Sie unter [XML Source](../../integration-services/data-flow/xml-source.md).  
  
## <a name="xml-operations"></a>XML-Vorgänge  
 Der XML-Task ruft als erste Aktion ein bestimmtes XML-Dokument ab. Diese Aktion ist in den XML-Task integriert und wird automatisch ausgeführt. Das abgerufene XML-Dokument wird als Quelle von Daten für den Vorgang verwendet, den der XML-Task ausführt.  
  
 Für die XML-Vorgänge Diff, Merge und Patch sind zwei Operanden erforderlich. Der erste Operand gibt das XML-Quelldokument an. Der zweite Operand gibt ebenfalls ein XML-Dokument an, dessen Inhalt von den Anforderungen des Vorgangs abhängen. Beispielsweise werden mit Diff zwei Dokumente verglichen. Deshalb gibt der zweite Operand ein anderes, ähnliches XML-Dokument an, mit dem das XML-Quelldokument verglichen wird.  
  
 Der XML-Task kann eine Variable oder einen Dateiverbindungs-Manager als Quelle verwenden oder die XML-Daten in eine Taskeigenschaft einschließen.  
  
 Falls es sich bei der Quelle um eine Variable handelt, enthält die angegebene Variable den Pfad des XML-Dokuments.  
  
 Falls es sich bei der Quelle um einen Dateiverbindungs-Manager handelt, stellt der angegebene Dateiverbindungs-Manager die Quellinformationen bereit. Der Dateiverbindungs-Manager wird separat vom XML-Task konfiguriert, und im XML-Task wird dann darauf verwiesen. Die Verbindungszeichenfolge aller Dateiverbindungs-Manager gibt den Pfad der XML-Datei an. Weitere Informationen finden Sie unter [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md).  
  
 Der XML-Task kann so konfiguriert werden, dass das Ergebnis des Vorgangs in einer Variablen oder in einer Datei gespeichert wird. Falls Sie das Ergebnis in einer Datei speichern, verwendet der XML-Task einen Dateiverbindungs-Manager für den Zugriff auf die Datei. Sie können die Ergebnisse des Diffgram-Objekts, das vom Diff-Vorgang generiert wird, in Dateien und Variablen speichern.  
  
## <a name="predefined-xml-operations"></a>Vordefinierte XML-Vorgänge  
 Der XML-Task schließt vordefinierte Vorgänge zum Verwenden von XML-Dokumenten ein. In der folgenden Tabelle werden diese Vorgänge beschrieben.  
  
|Vorgang|BESCHREIBUNG|  
|---------------|-----------------|  
|Diff|Vergleicht zwei XML-Dokumente miteinander. Diff verwendet das XML-Quelldokument als Basisdokument und vergleicht es mit einem zweiten XML-Dokument, stellt die Unterschiede fest und schreibt diese in ein XML-Diffgram-Dokument. Dieser Vorgang schließt Eigenschaften zum Anpassen des Vergleichs ein.|  
|Merge|Führt zwei XML-Dokumente zusammen. Der Merge-Vorgang verwendet das XML-Quelldokument als Basisdokument und fügt diesem den Inhalt eines zweiten Dokuments hinzu. Bei diesem Vorgang kann ein Mergespeicherort innerhalb des Basisdokuments angegeben werden.|  
|Patch|Wendet die Ausgabe des Diff-Vorgangs, ein so genanntes Diffgram-Dokument, auf ein XML-Dokument an, um ein neues übergeordnetes Dokument zu erstellen, das Inhalt aus dem Diffgram-Dokument einschließt.|  
|Überprüfen|Überprüft das XML-Dokument mithilfe eines DTD-(Document Type Definition-) bzw. XSD-(XML Schema Definition-)Schemas.|  
|XPath|Führt XPath-Abfragen und -Auswertungen aus.|  
|XSLT|Führt XSL-Transformationen in XML-Dokumenten aus.|  
  
### <a name="diff-operation"></a>Diff-Vorgang  
 Für den Diff-Vorgang kann die Verwendung eines anderen Vergleichsalgorithmus konfiguriert werden, abhängig davon, ob der Vergleich schnell oder präzise sein muss. Dieser Vorgang kann auch so konfiguriert werden, dass automatisch ein schneller oder präziser Vergleich basierend auf der Größe der verglichenen Dokumente ausgewählt wird.  
  
 Der Diff-Vorgang enthält Optionen, mit denen der XML-Vergleich angepasst wird. In der folgenden Tabelle werden diese Optionen beschrieben.  
  
|Option|BESCHREIBUNG|  
|------------|-----------------|  
|**IgnoreComments**|Dieser Wert gibt an, ob Kommentarknoten verglichen werden.|  
|**IgnoreNamespaces**|Dieser Wert gibt an, ob der Namespace-URI (Uniform Resource Identifier) eines Elements und dessen Attributnamen verglichen werden. Falls diese Option auf **true** festgelegt ist, werden zwei Elemente mit dem gleichen lokalen Namen, aber einem unterschiedlichen Namespace, als identisch betrachtet.|  
|**IgnorePrefixes**|Dieser Wert gibt an, ob Präfixe von Element- und Attributnamen verglichen werden. Falls diese Option auf **true,** festgelegt ist, werden zwei Elemente mit dem gleichen lokalen Namen, aber einem unterschiedlichen Namespace-URI und Präfix, als identisch betrachtet.|  
|**IgnoreXMLDeclaration**|Dieser Wert gibt an, ob die XML-Deklarationen verglichen werden.|  
|**IgnoreOrderOfChildElements**|Dieser Wert gibt an, ob die Reihenfolge von untergeordneten Elementen verglichen wird. Falls diese Option auf **true** festgelegt ist, werden untergeordnete Elemente, die sich nur bezüglich der Position in einer Liste gleichgeordneter Elemente unterscheiden, als identisch betrachtet.|  
|**IgnoreWhiteSpaces**|Dieser Wert gibt an, dass Leerzeichen verglichen werden.|  
|**IgnoreProcessingInstructions**|Dieser Wert gibt an, ob Verarbeitungsanweisungen verglichen werden.|  
|**IgnoreDTD**|Dieser Wert gibt an, ob die DTD ignoriert wird.|  
  
### <a name="merge-operation"></a>Merge-Vorgang  
 Wenn Sie eine XPath-Anweisung zur Identifizierung des Mergespeicherorts im Quelldokument verwenden, wird erwartet, dass diese Anweisung einen einzelnen Knoten zurückgibt. Wenn die Anweisung mehrere Knoten zurückgibt, wird nur der erste Knoten verwendet. Der Inhalt des zweiten Dokuments wird unter dem ersten Knoten zusammengeführt, den die XPath-Abfrage zurückgibt.  
  
### <a name="xpath-operation"></a>XPath-Vorgang  
 Für den XPath-Vorgang kann die Verwendung unterschiedlicher Typen von XPath-Funktionalität konfiguriert werden.  
  
-   Wählen Sie die Option **Evaluation** aus, um XPath-Funktionen wie z.B. „sum()“ zu implementieren.  
  
-   Wählen Sie die Option **Node list** aus, um die ausgewählten Knoten als XML-Fragment zurückzugeben.  
  
-   Wählen Sie die Option **Values** aus, um den inneren Textwert von allen ausgewählten Knoten verkettet als Zeichenfolge zurückzugeben.  
  
### <a name="validation-operation"></a>Validation-Vorgang  
 Für den Validation-Vorgang kann die Verwendung einer Dokumenttypdefinition (DTD) oder einer XML-Schemadefinition (XSD) konfiguriert werden.  
  
 Aktivieren Sie **ValidationDetails** , um eine ausführliche Fehlerausgabe zu erhalten. Weitere Informationen finden Sie unter [Validate XML with the XML Task](../../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
## <a name="xml-document-encoding"></a>XML-Dokumentcodierung  
 Der XML-Task unterstützt nur das Zusammenführen von Unicode-Dokumenten. Dies bedeutet, der Task kann den Merge-Vorgang nur auf Dokumente mit einer Unicode-Codierung anwenden. Wenn Sie andere Codierungen verwenden, tritt beim XML-Task ein Fehler auf.  
  
> [!NOTE]  
>  Die Vorgänge Diff und Patch enthalten eine Option, um die XML-Deklaration in den XML-Daten des zweiten Operanden zu ignorieren. Auf diese Weise können für diese Vorgänge Dokumente mit anderen Codierungen verwendet werden.  
  
 Überprüfen Sie die XML-Deklaration, um festzustellen, ob das XML-Dokument verwendet werden kann. In der Deklaration muss explizit UTF-8 angegeben sein. Dies steht für die 8-Bit-Unicode-Codierung.  
  
 Der folgende Code veranschaulicht die 8-Bit-Unicode-Codierung.  
  
 `<?xml version="1.0" encoding="UTF-8"?>`  
  
## <a name="custom-logging-messages-available-on-the-xml-task"></a>Verfügbare benutzerdefinierte Meldungen für die Protokollierung für den XML-Task  
 In der folgenden Tabelle werden die benutzerdefinierten Protokolleinträge für den XML-Task beschrieben. Weitere Informationen finden Sie unter [Integration Services-Protokollierung &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Protokolleintrag|BESCHREIBUNG|  
|---------------|-----------------|  
|**XMLOperation**|Stellt Informationen über den vom Task durchgeführten Vorgang bereit.|  
  
## <a name="configuration-of-the-xml-task"></a>Konfiguration des XML-Tasks  
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
 Klicken Sie auf eines der folgenden Themen, um weitere Informationen zu den Eigenschaften zu erhalten, die Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen können:  
  
-   [Validieren von XML-Dokumenten mit dem XML-Task](../../integration-services/control-flow/validate-xml-with-the-xml-task.md)  
  
-   [Seite Ausdrücke](../../integration-services/expressions/expressions-page.md)  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum Festlegen von Eigenschaften im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer zu erhalten:  
  
-   [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-the-xml-task"></a>Programmgesteuerte Konfiguration des XML-Tasks  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum programmgesteuerten Festlegen dieser Eigenschaften anzuzeigen:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.XMLTask.XMLTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="xml-task-editor-general-page"></a>Editor für den XML-Task (Seite Allgemein)
  Auf der Seite **Allgemein** des Dialogfelds **Editor für den XML-Task** können Sie den Vorgangstyp angeben und den Vorgang konfigurieren.  
  
 Informationen, um sich mit diesem Thema vertraut zu machen, finden Sie unter [Validate XML with the XML Task](../../integration-services/control-flow/validate-xml-with-the-xml-task.md) (Validieren von XML-Dokumenten mit dem XML-Task). Weitere Informationen zum Arbeiten mit XML-Dokumenten und Daten finden Sie unter "[XML im .NET Framework](/previous-versions/aa720019(v=vs.71))" in der MSDN Library.  
  
### <a name="static-options"></a>Statische Optionen  
 **OperationType**  
 Wählen Sie den Vorgangstyp aus der Liste aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Überprüfen**|Überprüft das XML-Dokument mithilfe eines DTD-(Document Type Definition-) bzw. XSD-(XML Schema Definition-)Schemas. Nach Auswahl dieser Option werden die dynamischen Optionen im Bereich **Validate** angezeigt.|  
|**XSLT**|Führt XSL-Transformationen in XML-Dokumenten aus. Nach Auswahl dieser Option werden die dynamischen Optionen im Bereich **XSLT** angezeigt.|  
|**XPATH**|Führt XPath-Abfragen und -Auswertungen aus. Nach Auswahl dieser Option werden die dynamischen Optionen im Bereich **XPATH** angezeigt.|  
|**Merge** (Zusammenführen)|Führt zwei XML-Dokumente zusammen. Nach Auswahl dieser Option werden die dynamischen Optionen im Bereich **Merge** angezeigt.|  
|**Diff**|Vergleicht zwei XML-Dokumente miteinander. Nach Auswahl dieser Option werden die dynamischen Optionen im Bereich **Diff** angezeigt.|  
|**Patch**|Erstellt auf der Grundlage der Ausgabe des Vergleichsvorgangs ein neues Dokument. Nach Auswahl dieser Option werden die dynamischen Optionen im Bereich **Patch** angezeigt.|  
  
 **SourceType**  
 Hiermit wählen Sie den Quelltyp des XML-Dokuments aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Direkteingabe**|Legt ein XML-Dokument als Quelle fest.|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **Quelle**  
 Wenn **Quelle** auf **Direkteingabe** festgelegt ist, geben Sie den XML-Code an, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(…)** , und stellen Sie dann mithilfe des Dialogfelds **Dokumentquellen-Editor** den XML-Code bereit.  
  
 Wenn die **Quelle** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn die **Quelle** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf **\<New variable...>** , um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md).  
  
### <a name="operationtype-dynamic-options"></a>OperationType (dynamische Optionen)  
  
#### <a name="operationtype--validate"></a>OperationType = Validate  
 Geben Sie Optionen für den Validate-Vorgang an.  
  
 **SaveOperationResult**  
 Geben Sie an, ob die Ausgabe des Validate-Vorgangs vom XML-Task gespeichert werden soll.  
  
 **OverwriteDestination**  
 Geben Sie an, ob die Zieldatei oder -variable überschrieben werden soll.  
  
 **Ziel**  
 Wählen Sie einen vorhandenen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 **DestinationType**  
 Wählen Sie den Zieltyp des XML-Dokuments aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **ValidationType**  
 Wählen Sie den Überprüfungstyp aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**DTD**|Verwenden Sie eine Dokumenttypdefinition (DTD).|  
|**XSD**|Verwenden Sie eine XML-Schemadefinition (XSD). Nach Auswahl dieser Option werden die dynamischen Optionen im Bereich **ValidationType** angezeigt.|  
  
 **FailOnValidationFail**  
 Geben Sie an, ob der Vorgang fehlschlägt, wenn bei der Dokumentüberprüfung ein Fehler auftritt.  
  
 **ValidationDetails**  
 Bietet eine umfassende Fehlerausgabe, wenn der Wert dieser Eigenschaft auf „true“ festgelegt ist. Weitere Informationen finden Sie unter [Validate XML with the XML Task](../../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
### <a name="validationtype-dynamic-options"></a>ValidationType (dynamische Optionen)  
  
#### <a name="validationtype--xsd"></a>ValidationType = XSD  
 **SecondOperandType**  
 Wählen Sie den Quelltyp des zweiten XML-Dokuments aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Direkteingabe**|Legt ein XML-Dokument als Quelle fest.|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **SecondOperand**  
 Wenn **SecondOperandType** auf **Direkteingabe** festgelegt ist, geben Sie den XML-Code an, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(…)** , und stellen Sie dann über das Dialogfeld **Quellen-Editor** den XML-Code bereit.  
  
 Wenn **SecondOperandType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **XPathStringSourceType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md).  
  
#### <a name="operationtype--xslt"></a>OperationType = XSLT  
 Geben Sie Optionen für den XSLT-Vorgang an.  
  
 **SaveOperationResult**  
 Geben Sie an, ob die Ausgabe des XSLT-Vorgangs vom XML-Task gespeichert werden soll.  
  
 **OverwriteDestination**  
 Geben Sie an, ob die Zieldatei oder -variable überschrieben werden soll.  
  
 **Ziel**  
 Wenn **DestinationType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **DestinationType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Wählen Sie den Zieltyp des XML-Dokuments aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **SecondOperandType**  
 Wählen Sie den Quelltyp des zweiten XML-Dokuments aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Direkteingabe**|Legt ein XML-Dokument als Quelle fest.|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **SecondOperand**  
 Wenn **SecondOperandType** auf **Direkteingabe** festgelegt ist, geben Sie den XML-Code an, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(…)** , und stellen Sie dann über das Dialogfeld **Quellen-Editor** den XML-Code bereit.  
  
 Wenn **SecondOperandType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **XPathStringSourceType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md).  
  
#### <a name="operationtype--xpath"></a>OperationType = XPATH  
 Geben Sie Optionen für den XPath-Vorgang an.  
  
 **SaveOperationResult**  
 Geben Sie an, ob die Ausgabe des XPath-Vorgangs vom XML-Task gespeichert werden soll.  
  
 **OverwriteDestination**  
 Geben Sie an, ob die Zieldatei oder -variable überschrieben werden soll.  
  
 **Ziel**  
 Wenn **DestinationType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **DestinationType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Wählen Sie den Zieltyp des XML-Dokuments aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **SecondOperandType**  
 Wählen Sie den Quelltyp des zweiten XML-Dokuments aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Direkteingabe**|Legt ein XML-Dokument als Quelle fest.|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **SecondOperand**  
 Wenn **SecondOperandType** auf **Direkteingabe** festgelegt ist, geben Sie den XML-Code an, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(…)** , und stellen Sie dann über das Dialogfeld **Quellen-Editor** den XML-Code bereit.  
  
 Wenn **SecondOperandType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **XPathStringSourceType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md).  
  
 **PutResultInOneNode**  
 Geben Sie an, ob das Ergebnis in einen einzelnen Knoten geschrieben werden soll.  
  
 **XPathOperation**  
 Wählen Sie den XPath-Ergebnistyp aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Auswertung**|Gibt die Ergebnisse einer XPath-Funktion zurück.|  
|**Node list**|Gibt die ausgewählten Knoten als XML-Fragment zurück.|  
|**Werte**|Gibt den inneren Textwert aller ausgewählten Knoten als verkettete Zeichenfolge zurück.|  
  
#### <a name="operationtype--merge"></a>OperationType = Merge  
 Geben Sie Optionen für den Merge-Vorgang an.  
  
 **XPathStringSourceType**  
 Hiermit wählen Sie den Quelltyp des XML-Dokuments aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Direkteingabe**|Legt ein XML-Dokument als Quelle fest.|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **XPathStringSource**  
 Wenn **XPathStringSourceType** auf **Direkteingabe** festgelegt ist, geben Sie den XML-Code an, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(…)** , und stellen Sie dann mithilfe des Dialogfelds **Dokumentquellen-Editor** den XML-Code bereit.  
  
 Wenn **XPathStringSourceType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **XPathStringSourceType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
 Wenn Sie eine XPath-Anweisung zur Identifizierung des Mergespeicherorts im Quelldokument verwenden, wird erwartet, dass diese Anweisung einen einzelnen Knoten zurückgibt. Wenn die Anweisung mehrere Knoten zurückgibt, wird nur der erste Knoten verwendet. Der Inhalt des zweiten Dokuments wird unter dem ersten Knoten zusammengeführt, den die XPath-Abfrage zurückgibt.  
  
 **SaveOperationResult**  
 Geben Sie an, ob die Ausgabe des Merge-Vorgangs vom XML-Task gespeichert werden soll.  
  
 **OverwriteDestination**  
 Geben Sie an, ob die Zieldatei oder -variable überschrieben werden soll.  
  
 **Ziel**  
 Wenn **DestinationType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **DestinationType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Wählen Sie den Zieltyp des XML-Dokuments aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **SecondOperandType**  
 Wählen Sie den Zieltyp des zweiten XML-Dokuments aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Direkteingabe**|Legt ein XML-Dokument als Quelle fest.|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **SecondOperand**  
 Wenn **SecondOperandType** auf **Direkteingabe** festgelegt ist, geben Sie den XML-Code an, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(…)** , und stellen Sie dann über das Dialogfeld **Dokumentquellen-Editor** den XML-Code bereit.  
  
 Wenn **SecondOperandType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **SecondOperandType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
#### <a name="operationtype--diff"></a>OperationType = Diff  
 Geben Sie Optionen für den Vergleichsvorgang an.  
  
 **DiffAlgorithm**  
 Wählen Sie den Vergleichsalgorithmus aus, der beim Vergleich von Dokumenten verwendet werden soll. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Automatisch**|Wenn Sie diese Option auswählen, wird vom XML-Task bestimmt, ob der schnelle oder der genaue Algorithmus verwendet wird.|  
|**Fast**|Wenn diese Option ausgewählt ist, wird ein schneller, aber weniger genauer Vergleichsalgorithmus verwendet.|  
|**Precise**|Wenn diese Option ausgewählt ist, wird genauer Vergleichsalgorithmus verwendet.|  
  
 **Diff Options**  
 Legen Sie die Vergleichsoptionen fest, die auf den Vergleichsvorgang angewendet werden sollen. Die Optionen sind in der folgenden Tabelle aufgeführt.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**IgnoreXMLDeclaration**|Geben Sie an, ob XML-Deklaration verglichen werden soll.|  
|**IgnoreDTD**|Geben Sie an, ob die Dokumenttypdefinition (DTD) ignoriert werden soll.|  
|**IgnoreWhiteSpaces**|Geben Sie an, ob Unterschiede hinsichtlich der Menge an Leerzeichen beim Vergleichen von Dokumenten ignoriert werden sollen.|  
|**IgnoreNamespaces**|Geben Sie an, ob der Namespace-URI (Uniform Resource Identifier) eines Elements und seine Attributnamen verglichen werden sollen.<br /><br /> Hinweis: Wenn diese Option auf **True** festgelegt ist, werden zwei Elemente, die denselben lokalen Namen, aber unterschiedliche Namespaces haben, als identisch betrachtet.|  
|**IgnoreProcessingInstructions**|Geben Sie an, ob Verarbeitungsanweisungen verglichen werden sollen.|  
|**IgnoreOrderOfChildElements**|Geben Sie an, ob die Reihenfolge untergeordneter Elemente verglichen werden soll.<br /><br /> Hinweis: Wenn diese Option auf **True** festgelegt ist, werden untergeordnete Elemente, die sich innerhalb einer Liste gleichgeordneter Elemente lediglich durch ihre Position unterscheiden, als identisch betrachtet.|  
|**IgnoreComments**|Geben Sie an, ob Kommentarknoten verglichen werden sollen.|  
|**IgnorePrefixes**|Geben Sie an, ob die Präfixe der Element- und Attributnamen verglichen werden sollen.<br /><br /> Hinweis: Wenn diese Option auf **True** festgelegt ist, werden zwei Elemente, die denselben lokalen Namen, aber unterschiedliche Namespace-URIs und -Präfixe haben, als identisch betrachtet.|  
  
 **FailOnDifference**  
 Geben Sie an, ob der Task fehlschlagen soll, wenn beim Vergleichsvorgang ein Fehler auftritt.  
  
 **SaveDiffGram**  
 Geben Sie an, ob das Vergleichsergebnis in einem DiffGram-Dokument gespeichert werden soll.  
  
 **SaveOperationResult**  
 Geben Sie an, ob die Ausgabe des Vergleichsvorgangs vom XML-Task gespeichert werden soll.  
  
 **OverwriteDestination**  
 Geben Sie an, ob die Zieldatei oder -variable überschrieben werden soll.  
  
 **Ziel**  
 Wenn **DestinationType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **DestinationType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Wählen Sie den Zieltyp des XML-Dokuments aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **SecondOperandType**  
 Wählen Sie den Zieltyp des XML-Dokuments aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Direkteingabe**|Legt ein XML-Dokument als Quelle fest.|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **SecondOperand**  
 Wenn **SecondOperandType** auf **Direkteingabe** festgelegt ist, geben Sie den XML-Code an, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(…)** , und stellen Sie dann über das Dialogfeld **Dokumentquellen-Editor** den XML-Code bereit.  
  
 Wenn **SecondOperandType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **SecondOperandType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
#### <a name="operationtype--patch"></a>OperationType = Patch  
 Geben Sie Optionen für den Patch-Vorgang an.  
  
 **SaveOperationResult**  
 Geben Sie an, ob die Ausgabe des Patch-Vorgangs vom XML-Task gespeichert werden soll.  
  
 **OverwriteDestination**  
 Geben Sie an, ob die Zieldatei oder -variable überschrieben werden soll.  
  
 **Ziel**  
 Wenn **DestinationType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **DestinationType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Wählen Sie den Zieltyp des XML-Dokuments aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **SecondOperandType**  
 Wählen Sie den Zieltyp des XML-Dokuments aus. Diese Eigenschaft besitzt die in der folgenden Tabelle aufgeführten Optionen.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Direkteingabe**|Legt ein XML-Dokument als Quelle fest.|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **SecondOperand**  
 Wenn **SecondOperandType** auf **Direkteingabe** festgelegt ist, geben Sie den XML-Code an, oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten **(…)** , und stellen Sie dann über das Dialogfeld **Dokumentquellen-Editor** den XML-Code bereit.  
  
 Wenn **SecondOperandType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **SecondOperandType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
## <a name="related-content"></a>Verwandte Inhalte  

-   CodePlex-Beispiel, [Process XML Data Package Sample](https://msftisprodsamples.codeplex.com/wikipage?title=SS2008!Process%20XML%20Data%20Package%20Sample&version=10&ProjectName=msftisprodsamples), auf www.codeplex.com  
  
