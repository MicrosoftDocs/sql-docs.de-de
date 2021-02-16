---
description: Erstellen einer synchronen Transformation mit der Skriptkomponente
title: Erstellen einer synchronen Transformation mit der Skriptkomponente | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- synchronous outputs [Integration Services]
- transformation components [Integration Services]
- Script component [Integration Services], transformation components
ms.assetid: aa1bee1a-ab06-44d8-9944-4bff03d73016
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 08c7e20120ef5ff746b495535843bc93a46fea16
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344889"
---
# <a name="creating-a-synchronous-transformation-with-the-script-component"></a>Erstellen einer synchronen Transformation mit der Skriptkomponente

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Transformationskomponenten dienen im Datenfluss eines [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Pakets dazu, Daten auf dem Weg von der Quelle zum Ziel zu ändern und zu analysieren. Eine Transformation mit synchronen Ausgaben verarbeitet jede eingegebene Zeile, während sie die Komponente durchläuft. Eine Transformation mit asynchronen Ausgaben wartet, bis alle Eingabezeilen empfangen wurden, bevor die Verarbeitung abgeschlossen wird. In diesem Thema wird eine synchrone Transformation erläutert. Informationen zur asynchronen Transformation finden Sie unter [Creating an Asynchronous Transformation with the Script Component (Erstellen einer asynchronen Transformationen mit der Skriptkomponente)](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md). Weitere Informationen zu den Unterschieden zwischen synchronen und asynchronen Komponenten finden Sie unter [Grundlegendes zu synchronen und asynchronen Transformationen](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md).  
  
 Eine Übersicht der Skriptkomponenten finden Sie unter [Extending the Data Flow with the Script Component (Erweitern des Datenflusses mit der Skriptkomponente)](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md).  
  
 Die Skriptkomponente und der Infrastrukturcode, den sie generieren, erleichtern Ihnen die Entwicklung benutzerdefinierter Datenflusskomponenten deutlich. Um die Funktionsweise der Skriptkomponente zu verstehen, kann es jedoch hilfreich sein, sich mit den im Abschnitt [Developing a Custom Data Flow Component (Entwickeln einer benutzerdefinierten Datenflusskomponente)](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) beschriebenen Schritten vertraut zu machen, die Sie bei der Entwicklung einer benutzerdefinierten Datenflusskomponente durchführen müssen. Sehen Sie sich dabei insbesondere den Schritt [Developing a Custom Transformation Component with Synchronous Outputs (Entwickeln einer benutzerdefinierten Transformationskomponente mit synchronen Ausgaben)](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md) an.  
  
## <a name="getting-started-with-a-synchronous-transformation-component"></a>Erste Schritte mit einer synchronen Transformationskomponente  
 Wenn Sie im Bereich „Datenfluss“ des [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designers eine Skriptkomponente hinzufügen, wird das Dialogfeld **Skriptkomponententyp auswählen** geöffnet, und Sie werden zur Auswahl eines Quell-, Ziel- oder Transformationskomponententyps aufgefordert. Wählen Sie in diesem Dialogfeld die Option **Transformation** aus.  
  
## <a name="configuring-a-synchronous-transformation-component-in-metadata-design-mode"></a>Konfigurieren einer synchronen Transformationskomponente im Metadatenentwurfsmodus  
 Nachdem Sie die Option zur Erstellung einer Transformationskomponente ausgewählt haben, konfigurieren Sie die Komponente mit dem **Transformations-Editor für Skripterstellung**. Weitere Informationen finden Sie unter [Configuring the Script Component in the Script Component Editor (Konfigurieren der Skriptkomponente im Skriptkomponenten-Editor)](../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md).  
  
 Zum Bestimmen der Skriptsprache für die Skriptkomponente wird die **ScriptLanguage**-Eigenschaft auf der Seite **Skript** des **Transformations-Editors für Skripterstellung** festgelegt.  
  
> [!NOTE]  
>  Verwenden Sie im Dialogfeld **Optionen** auf der Seite **Allgemein** die Option **Skriptsprache**, um die Standardskriptsprache für die Skriptkomponente festzulegen. Weitere Informationen finden Sie unter [General Page (Allgemeines)](../general-page-of-integration-services-designers-options.md).  
  
 Eine Datenfluss-Transformationskomponente verfügt über eine Eingabe und unterstützt mindestens eine Ausgabe. Die Konfiguration der Eingabe und der Ausgaben für die Komponente ist einer der Schritte, die Sie mithilfe des **Transformations-Editors für Skripterstellung** im Metadatenentwurfsmodus ausführen müssen, bevor Sie das benutzerdefinierte Skript schreiben.  
  
### <a name="configuring-input-columns"></a>Konfigurieren von Eingabespalten  
 Eine Transformationskomponente verfügt über eine Eingabe.  
  
 Die Spaltenliste auf der Seite **Eingabespalten** im **Transformations-Editor für Skripterstellung** zeigt die von der Ausgabe der Upstreamkomponente im Datenfluss verfügbaren Spalten an. Markieren Sie die Spalten, die Sie transformieren oder durchlaufen möchten. Markieren Sie alle Spalten, die Sie an Ort und Stelle transformieren möchten, als Lesen/Schreiben.  
  
 Weitere Informationen über die Seite **Eingabespalten** im **Transformations-Editor für Skripterstellung** finden Sie unter [Script Transformation Editor (Input Columns Page) (Transformations-Editor für Skripterstellung (Seite „Eingabespalten“))](../data-flow/transformations/script-component.md).  
  
### <a name="configuring-inputs-outputs-and-output-columns"></a>Konfigurieren von Eingaben, Ausgaben und Ausgabespalten  
 Eine Transformationskomponente unterstützt eine oder mehrere Ausgaben.  
  
 Auf der Seite **Eingaben und Ausgaben** im **Transformations-Editor für Skripterstellung** können Sie erkennen, dass eine Ausgabe erstellt wurde. Spalten sind in der Ausgabe hingegen nicht vorhanden. Auf dieser Seite des Editors haben Sie die Möglichkeit, die folgenden Elemente zu konfigurieren.  
  
-   Erstellen Sie eine oder mehrere zusätzliche Ausgaben, beispielsweise eine simulierte Fehlerausgabe für Zeilen, die unerwartete Werte aufweisen. Verwenden Sie die Schaltflächen **Ausgabe hinzufügen** und **Ausgabe entfernen**, um die Ausgaben der synchronen Transformationskomponente zu verwalten. Alle Eingabezeilen werden an alle verfügbaren Ausgaben weitergeleitet, außer Sie geben an, dass jede Zeile nur an eine der Ausgaben weitergeleitet werden soll. Die Umleitung von Zahlen wird durch das Angeben eines ganzzahligen Werts ungleich 0 (null) für die **ExclusionGroup**-Eigenschaft der Ausgaben erreicht. Der für die **ExclusionGroup**-Eigenschaft eingegebene spezifische ganzzahlige Wert zum Identifizieren der Ausgaben ist nicht von Bedeutung, es muss jedoch darauf geachtet werden, dass für die angegebene Ausgabengruppe durchgängig dieselbe ganze Zahl verwendet wird.  
  
    > [!NOTE]  
    >  Sie können auch einen Wert ungleich 0 (null) für die **ExclusionGroup**-Eigenschaft für eine einzelne Ausgabe verwenden, wenn Sie nicht alle Zeilen ausgeben möchten. In diesem Fall müssen Sie jedoch für alle Zeilen, die an die Ausgabe gesendet werden sollen, explizit die Methode **DirectRowTo\<outputbuffer>** aufrufen.  
  
-   Zuweisen eines aussagekräftigeren Namens für Eingabe und Ausgaben In der Skriptkomponente werden diese Namen verwendet, um die typisierten Accessoreigenschaften zu erzeugen, mit denen Sie auf die Eingabe und die Ausgaben in Ihrem Skript verweisen.  
  
-   Verändern Sie die Spalten für synchrone Transformationen nicht. Normalerweise werden bei einer synchronen Transformation dem Datenfluss keine Spalten hinzugefügt. Die Daten werden gleich im Puffer verändert, und der Puffer wird an die nächste Komponente im Datenfluss übergeben. Bei einer solchen Vorgehensweise müssen Sie den Transformationsausgaben keine Ausgabespalten explizit hinzufügen und diese dann konfigurieren. Die Ausgaben werden ohne explizit definierte Spalten im Editor angezeigt.  
  
-   Fügen Sie simulierten Fehlerausgaben neue Spalten für Fehler auf Zeilenebene hinzu. Üblicherweise besitzen mehrere Ausgaben in derselben **ExclusionGroup** die gleichen Ausgabespalten. Falls Sie eine simulierte Fehlerausgabe erstellen, sollten Sie jedoch mehrere Spalten hinzufügen, um Fehlerinformationen zu speichern. Informationen dazu, wie die Datenfluss-Engine Fehlerzeilen verarbeitet, finden Sie unter [Using Error Outputs in a Data Flow Component (Verwenden von Fehlerausgaben in einer Datenflusskomponente)](../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md). Beachten Sie, dass Sie in der Skriptkomponente eigenen Code schreiben müssen, um geeignete Fehlerinformationen für die zusätzlichen Spalten zu erhalten. Weitere Informationen finden Sie unter [Simulating an Error Output for the Script Component (Simulieren einer Fehlerausgabe für die Skriptkomponente)](../../integration-services/extending-packages-scripting-data-flow-script-component-examples/simulating-an-error-output-for-the-script-component.md).  
  
 Weitere Informationen über die Seite **Eingaben und Ausgaben** im **Transformations-Editor für Skripterstellung** finden Sie unter [Script Transformation Editor (Inputs and Outputs Page) (Transformations-Editor für Skripterstellung (Seite „Eingaben und Ausgaben“))](../data-flow/transformations/script-component.md).  
  
### <a name="adding-variables"></a>Hinzufügen von Variablen  
 Wenn Sie die vorhandenen Variablen in Ihrem Skript verwenden möchten, können Sie diese in den Eigenschaftsfeldern **ReadOnlyVariables** und **ReadWriteVariables** auf der Seite **Skript** im **Transformations-Editor für Skripterstellung** hinzufügen.  
  
 Wenn Sie mehrere Variablen in die Eigenschaftsfelder hinzufügen, trennen Sie die Variablennamen durch Kommas. Sie können auch mehrere Variablen auswählen, indem Sie auf die Schaltfläche mit den Auslassungspunkten ( **...** ) neben den Eigenschaftsfeldern für **ReadOnlyVariables** und **ReadWriteVariables** klicken und dann die Variablen im Dialogfeld **Variablen auswählen** festlegen.  
  
 Allgemeine Informationen über das Verwenden von Variablen mit der Skriptkomponente finden Sie unter [Using Variables in the Script Component (Verwenden von Variablen in der Skriptkomponente)](../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md).  
  
 Weitere Informationen über die Seite **Skript** im **Transformations-Editor für Skripterstellung** finden Sie unter [Script Transformation Editor (Script Page) (Transformations-Editor für Skripterstellung (Seite „Skript“))](../data-flow/transformations/script-component.md).  
  
## <a name="scripting-a-synchronous-transformation-component-in-code-design-mode"></a>Skripterstellung für eine synchrone Transformationskomponente im Codeentwurfsmodus  
 Nachdem Sie die Metadaten für Ihre Komponente konfiguriert haben, können Sie das benutzerdefinierte Skript schreiben. Klicken Sie auf der Seite **Skript** im **Transformations-Editor für Skripterstellung** auf **Skript bearbeiten**, um die [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications-IDE (VSTA) zu öffnen und Ihr benutzerdefiniertes Skript hinzuzufügen. Welche Skriptsprache Sie verwenden, hängt davon ab, ob Sie auf der Seite **Skript**[!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic oder [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# als Skriptsprache für die **ScriptLanguage**-Eigenschaft festgelegt haben.  
  
 Wichtige Informationen, die alle Arten von Komponenten betreffen, die mithilfe der Skriptkomponente erstellt wurden, finden Sie unter [Coding and Debugging the Script Component (Codieren und Debuggen der Skriptkomponente)](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md).  
  
### <a name="understanding-the-auto-generated-code"></a>Grundlegendes zum automatisch generierten Code  
 Wenn Sie nach der Erstellung und Konfiguration einer Transformationskomponente die VSTA-IDE öffnen, wird die bearbeitbare **ScriptMain**-Klasse im Code-Editor mit einem Stub für die **ProcessInputRow**-Methode angezeigt. Sie schreiben Ihren benutzerdefinierten Code in der **ScriptMain**-Klasse. **ProcessInputRow** ist die wichtigste Methode in einer Transformationskomponente.  
  
 Wenn Sie das Fenster **Projektexplorer** in VSTA öffnen, können Sie sehen, dass die Skriptkomponente auch schreibgeschützte **BufferWrapper**- und **ComponentWrapper**-Projektelemente generiert hat. Die **ScriptMain**-Klasse erbt von der **UserComponent**-Klasse im **ComponentWrapper**-Projektelement.  
  
 Zur Laufzeit ruft die Datenfluss-Engine die **ProcessInput**-Methode in der **UserComponent**-Klasse auf, die die <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ProcessInput%2A>-Methode der übergeordneten <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent>-Klasse überschreibt. Die **ProcessInput**-Methode durchläuft die Zeilen im Eingabepuffer der Reihe nach und ruft für jede Zeile einmal die **ProcessInputRow**-Methode auf.  
  
### <a name="writing-your-custom-code"></a>Schreiben von benutzerdefiniertem Code  
 Eine Transformationskomponente mit synchronen Ausgaben ist die Datenflusskomponente, die am einfachsten geschrieben werden kann. Zum Beispiel besteht das später in diesem Thema dargestellte Beispiel mit einer einzigen Ausgabe aus dem folgenden benutzerdefinierten Code:  
  
```vb  
Row.City = UCase(Row.City)  
```  
  
```csharp  
Row.City = (Row.City).ToUpper();  
  
```  
  
 Sie können mit der überschriebenen **ProcessInputRow**-Methode die Daten in jeder Zeile des Eingabepuffers transformieren, um die Erstellung einer benutzerdefinierten synchronen Transformationskomponente abzuschließen. Die Datenfluss-Engine leitet diesen Puffer an die nächste Komponente im Datenfluss weiter, sobald er voll ist.  
  
 Je nach Anforderung können Sie auch Skript in den Methoden **PreExecute** und **PostExecute** schreiben, die in der **ScriptMain**-Klasse verfügbar sind, um vorbereitende oder abschließende Verarbeitungsvorgänge auszuführen.  
  
### <a name="working-with-multiple-outputs"></a>Arbeiten mit mehreren Ausgaben  
 Die Umleitung von Eingabezeilen an eine von zwei oder mehreren möglichen Ausgaben erfordert nicht viel mehr benutzerdefinierten Code als das Szenario mit einer einzigen Ausgabe, das bereits erläutert wurde. Zum Beispiel besteht das später in diesem Thema dargestellte Beispiel mit zwei Ausgaben aus dem folgenden benutzerdefinierten Code:  
  
```vb  
Row.City = UCase(Row.City)  
If Row.City = "REDMOND" Then  
    Row.DirectRowToMyRedmondAddresses()  
Else  
    Row.DirectRowToMyOtherAddresses()  
End If  
```  
  
```csharp  
Row.City = (Row.City).ToUpper();  
  
if (Row.City=="REDMOND")  
{  
    Row.DirectRowToMyRedmondAddresses();  
}  
else  
{  
    Row.DirectRowToMyOtherAddresses();  
}  
```  
  
 In diesem Beispiel werden von der Skriptkomponente auf Grundlage der von Ihnen konfigurierten Ausgabenamen die **DirectRowTo\<OutputBufferX>** -Methoden für Sie erstellt. Sie können ähnlichen Code verwenden, um Fehlerzeilen an eine simulierte Fehlerausgabe weiterzuleiten.  
  
## <a name="examples"></a>Beispiele  
 In diesem Beispiel wird der benutzerdefinierte Code veranschaulicht, der in der **ScriptMain**-Klasse zur Erstellung einer synchronen Transformationskomponente erforderlich ist.  
  
> [!NOTE]  
>  In diesen Beispielen werden die erste und die vierte Spalte der Tabelle **Person.Address** in der Beispieldatenbank **AdventureWorks** verwendet, und die Spalten **intAddressID** und **nvarchar(30)City** werden durch den Datenfluss weitergeleitet. Die gleichen Daten werden in den Quellen-, Transformations- und Zielbeispielen in diesem Abschnitt verwendet. Zusätzliche Voraussetzungen und Annahmen werden für jedes Beispiel dokumentiert.  
  
### <a name="single-output-synchronous-transformation-example"></a>Beispiel einer synchronen Transformation mit einer Ausgabe  
 Dieses Beispiel zeigt eine synchrone Transformationskomponente mit einer Ausgabe. Diese Transformation durchläuft die Spalte **AddressID** und konvertiert die Spalte **City** in Großbuchstaben.  
  
 Wenn Sie den Beispielcode ausführen möchten, müssen Sie das Paket und die Komponente folgendermaßen konfigurieren:  
  
1.  Fügen Sie der Datenfluss-Designeroberfläche eine neue Skriptkomponente hinzu, und konfigurieren Sie sie als Transformation.  
  
2.  Verbinden Sie die Ausgabe einer Quelle oder einer anderen Transformation mit der neuen Transformationskomponente im [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer. Diese Ausgabe sollte Daten aus der Tabelle **Person.Address** der Beispieldatenbank **AdventureWorks**, die mindestens die Spalten **AddressID** und **City** enthält, bereitstellen.  
  
3.  Öffnen Sie den **Transformations-Editor für Skripterstellung**. Wählen Sie auf der Seite **Eingabespalten** die Spalten **AddressID** und **City** aus. Markieren Sie die Spalte **City** als Lesen/Schreiben.  
  
4.  Geben Sie auf der Seite **Eingaben und Ausgaben** der Eingabe und der Ausgabe aussagekräftigere Namen, z.B. **MyAddressInput** und **MyAddressOutput**. Beachten Sie, dass die **SynchronousInputID** der Ausgabe der **ID** der Eingabe entspricht. Sie müssen deshalb keine Ausgabespalten hinzufügen und konfigurieren.  
  
5.  Klicken Sie auf der Seite **Skript** auf **Skript bearbeiten**, und geben Sie das folgende Skript ein. Schließen Sie anschließend die Skriptentwicklungsumgebung und den **Transformations-Editor für Skripterstellung**.  
  
6.  Erstellen und konfigurieren Sie eine Zielkomponente, die die Spalten **AddressID** und **City** aufnimmt, z.B. ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ziel oder die Beispielzielkomponente, die unter [Creating a Destination with the Script Component (Erstellen eines Ziels mit der Skriptkomponente)](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md) veranschaulicht wird. Verbinden Sie daraufhin die Ausgabe der Transformation mit der Zielkomponente. Sie können eine Zieltabelle erstellen, indem Sie den folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Befehl in der **AdventureWorks**-Datenbank ausführen:  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
7.  Führen Sie das Beispiel aus.  
  
```vb  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
        Row.City = UCase(Row.City)  
  
    End Sub  
  
End Class  
```  
  
```csharp  
public class ScriptMain:  
    UserComponent  
  
{  
    public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
  
        Row.City = (Row.City).ToUpper();  
  
    }  
  
}  
```  
  
### <a name="two-output-synchronous-transformation-example"></a>Beispiel einer synchronen Transformation mit zwei Ausgaben  
 Dieses Beispiel zeigt eine synchrone Transformationskomponente mit zwei Ausgaben. Diese Transformation durchläuft die Spalte **AddressID** und konvertiert die Spalte **City** in Großbuchstaben. Wenn der Name der Stadt Redmond ist, wird die Zeile an eine bestimmte Ausgabe weitergeleitet, während alle anderen Zeilen zu einer anderen Ausgabe weitergeleitet werden.  
  
 Wenn Sie den Beispielcode ausführen möchten, müssen Sie das Paket und die Komponente folgendermaßen konfigurieren:  
  
1.  Fügen Sie der Datenfluss-Designeroberfläche eine neue Skriptkomponente hinzu, und konfigurieren Sie sie als Transformation.  
  
2.  Verbinden Sie die Ausgabe einer Quelle oder einer anderen Transformation mit der neuen Transformationskomponente im [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer. Diese Ausgabe sollte Daten aus der Tabelle **Person.Address** der Beispieldatenbank **AdventureWorks** bereitstellen, die mindestens die Spalten **AddressID** und **City** enthält.  
  
3.  Öffnen Sie den **Transformations-Editor für Skripterstellung**. Wählen Sie auf der Seite **Eingabespalten** die Spalten **AddressID** und **City** aus. Markieren Sie die Spalte **City** als Lesen/Schreiben.  
  
4.  Erstellen Sie auf der Seite **Eingaben und Ausgaben** eine zweite Ausgabe. Stellen Sie nach dem Hinzufügen der neuen Ausgabe sicher, dass **SynchronousInputID** der Ausgabe als die **ID** der Eingabe festgelegt wird. Diese Eigenschaft ist für die erste Ausgabe bereits festgelegt, die standardmäßig erstellt wird. Legen Sie für jede Ausgabe die **ExclusionGroup**-Eigenschaft auf denselben Wert ungleich 0 (null) fest, um anzugeben, dass die Eingabezeilen auf zwei sich gegenseitig ausschließende Ausgaben verteilt werden. Sie müssen den Ausgaben keine Ausgabezeilen hinzufügen.  
  
5.  Geben Sie der Eingabe und den Ausgaben aussagekräftigere Namen, z.B. **MyAddressInput**, **MyRedmondAddresses** und **MyOtherAddresses**.  
  
6.  Klicken Sie auf der Seite **Skript** auf **Skript bearbeiten**, und geben Sie das folgende Skript ein. Schließen Sie anschließend die Skriptentwicklungsumgebung und den **Transformations-Editor für Skripterstellung**.  
  
7.  Erstellen und konfigurieren Sie zwei Zielkomponenten für die Spalten **AddressID** und **City**, z.B. ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ziel, ein Flatfileziel oder die unter [Creating a Destination with the Script Component (Erstellen eines Ziels mit der Skriptkomponente)](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md) erläuterte Beispielzielkomponente. Verbinden Sie daraufhin alle Ausgaben der Transformation mit einer der Zielkomponenten. Sie können Zieltabellen erstellen, indem Sie einen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Befehl ähnlich dem im Folgenden dargestellten Befehl (mit eindeutigen Tabellennamen) in der **AdventureWorks**-Datenbank ausführen:  
  
    ```sql
    CREATE TABLE [Person].[Address2](  
        [AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL  
    ```  
  
8.  Führen Sie das Beispiel aus.  
  
```vb  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
        Row.City = UCase(Row.City)  
  
        If Row.City = "REDMOND" Then  
            Row.DirectRowToMyRedmondAddresses()  
        Else  
            Row.DirectRowToMyOtherAddresses()  
        End If  
  
    End Sub  
  
End Class  
```  
  
```csharp  
public class ScriptMain:  
    UserComponent  
  
public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
  
        Row.City = (Row.City).ToUpper();  
  
        if (Row.City == "REDMOND")  
        {  
            Row.DirectRowToMyRedmondAddresses();  
        }  
        else  
        {  
            Row.DirectRowToMyOtherAddresses();  
        }  
  
    }  
}  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Grundlegendes zu synchronen und asynchronen Transformationen](~/integration-services/understanding-synchronous-and-asynchronous-transformations.md)  
 [Erstellen einer asynchronen Transformation mit der Skriptkomponente](~/integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)  
 [Entwickeln einer benutzerdefinierten Transformationskomponente mit synchronen Ausgaben](~/integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md)
