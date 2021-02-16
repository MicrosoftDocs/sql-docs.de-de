---
description: Erstellen einer Quelle mit der Skriptkomponente
title: Erstellen einer Quelle mit der Skriptkomponente | Microsoft-Dokumentation
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
- Script component [Integration Services], source components
- output columns [Integration Services]
- sources [Integration Services], components
ms.assetid: 547c4179-ea82-4265-8c6f-04a2aa77a3c0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1aa4ed0d717e8b2d6dfe7f2b6b7d331936e00100
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344876"
---
# <a name="creating-a-source-with-the-script-component"></a>Erstellen einer Quelle mit der Skriptkomponente

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Quellkomponenten dienen im Datenfluss eines [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Pakets dazu, Daten aus einer Datenquelle zu laden, um sie an Downstreamtransformationen und -ziele zu übergeben. Gewöhnlich stellen Sie über einen vorhandenen Verbindungs-Manager eine Verbindung mit der Datenquelle her.  
  
 Eine Übersicht der Skriptkomponenten finden Sie unter [Extending the Data Flow with the Script Component (Erweitern des Datenflusses mit der Skriptkomponente)](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md).  
  
 Die Skriptkomponente und der Infrastrukturcode, den sie generieren, erleichtern Ihnen die Entwicklung benutzerdefinierter Datenflusskomponenten deutlich. Um die Funktionsweise der Skriptkomponente zu verstehen, kann es jedoch hilfreich sein, sich mit den Schritten, die bei der Entwicklung einer benutzerdefinierten Datenflusskomponente durchlaufen werden, vertraut zu machen. Weitere Informationen finden Sie im Abschnitt [Developing a Custom Data Flow Component (Entwickeln einer benutzerdefinierten Datenflusskomponente)](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md). Beachten Sie dabei insbesondere das Thema [Developing a Custom Source Component (Entwickeln einer benutzerdefinierten Quellkomponente)](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md).  
  
## <a name="getting-started-with-a-source-component"></a>Erste Schritte mit einer Quellkomponente  
 Wenn Sie im Bereich Datenfluss des [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designers eine Skriptkomponente hinzufügen, wird das Dialogfeld **Skriptkomponententyp auswählen** geöffnet, und Sie werden zur Auswahl eines Quell-, Ziel- oder Transformationsskripts aufgefordert. Klicken Sie in diesem Dialogfeld auf **Quelle**.  
  
## <a name="configuring-a-source-component-in-metadata-design-mode"></a>Konfigurieren einer Quellkomponente im Metadatenentwurfsmodus  
 Nachdem Sie die Erstellung einer Quellkomponente ausgewählt haben, konfigurieren Sie die Komponente mit dem **Transformations-Editor für Skripterstellung**. Weitere Informationen finden Sie unter [Configuring the Script Component in the Script Component Editor (Konfigurieren der Skriptkomponente im Skriptkomponenten-Editor)](../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md).  
  
 Eine Datenflussquellkomponente verfügt über keine Eingaben und unterstützt eine oder mehrere Ausgaben. Die Konfiguration der Ausgaben für die Komponente ist einer der Schritte, den Sie mithilfe des **Transformations-Editors für Skripterstellung** im Metadatenentwurfsmodus abschließen müssen, bevor Sie das benutzerdefinierte Skript schreiben.  
  
 Sie können zudem die Skriptsprache über die **ScriptLanguage**-Eigenschaft auf der Seite **Skript** im **Transformations-Editor für Skripterstellung** festlegen.  
  
> [!NOTE]  
>  Verwenden Sie im Dialogfeld **Optionen** auf der Seite **Allgemein** die Option **Skriptsprache**, um die Standardskriptsprache für Skriptkomponenten und Skripttasks festzulegen. Weitere Informationen finden Sie unter [General Page](~/integration-services/control-flow/script-task-editor-general-page.md).  
  
### <a name="adding-connection-managers"></a>Hinzufügen von Verbindungs-Managern  
 Eine Quellkomponente verwendet normalerweise einen vorhandenen Verbindungs-Manager zum Herstellen einer Verbindung mit der Datenquelle, aus der Daten in den Datenfluss geladen werden. Klicken Sie im **Transformations-Editor für Skripterstellung** auf der Seite **Verbindungs-Manager** auf **Hinzufügen**, um den passenden Verbindungs-Manager hinzuzufügen.  
  
 Ein Verbindungs-Manager ist jedoch nur eine praktische Einheit, die Daten kapselt und speichert, die sie benötigt, um eine Verbindung mit einer bestimmten Art von Datenquelle herzustellen. Sie müssen, um Daten zu laden und zu speichern sowie möglicherweise auch um eine Verbindung mit der Datenquelle herzustellen und diese zu beenden, eigenen benutzerdefinierten Code schreiben.  
  
 Allgemeine Informationen zum Verwenden von Verbindungs-Managern mit der Skriptkomponente finden Sie unter [Connecting to Data Sources in the Script Component (Herstellen einer Verbindung mit Datenquellen in der Skriptkomponente)](../../integration-services/extending-packages-scripting/data-flow-script-component/connecting-to-data-sources-in-the-script-component.md).  
  
 Weitere Informationen über die Seite **Verbindungs-Manager** im **Transformations-Editor für Skripterstellung** finden Sie unter [Script Transformation Editor (Connection Managers Page) (Transformations-Editor für Skripterstellung (Seite „Verbindungs-Manager“))](../data-flow/transformations/script-component.md).  
  
### <a name="configuring-outputs-and-output-columns"></a>Konfigurieren von Ausgaben und Ausgabespalten  
 Eine Quellkomponente verfügt über keine Eingaben und unterstützt eine oder mehrere Ausgaben. Auf der Seite **Eingaben und Ausgaben** im **Transformations-Editor für Skripterstellung** wurde standardmäßig eine Ausgabe, jedoch keine Ausgabespalten erstellt. Auf dieser Seite des Editors haben Sie die Möglichkeit, die folgenden Elemente zu konfigurieren.  
  
-   Sie müssen Ausgabespalten für jede Ausgabe manuell hinzufügen und konfigurieren. Wählen Sie den Ausgabespaltenordner für die jeweilige Ausgabe aus, und verwalten Sie anschließend die Ausgabespalten für alle Ausgaben der Quellkomponente mithilfe der Schaltflächen **Spalte hinzufügen** und **Spalte entfernen**. Die Namen, die Sie den Ausgabespalten hier zuweisen, verwenden Sie später mithilfe der typisierten Accessoreigenschaften, die im automatisch generierten Code erstellt wurden, für Verweise im Skript.  
  
-   Sie haben die Möglichkeit, eine oder mehrere zusätzliche Ausgaben zu erstellen, beispielsweise eine simulierte Fehlerausgabe für Zeilen, die unerwartete Werte aufweisen. Verwenden Sie die Schaltflächen **Ausgabe hinzufügen** und **Ausgabe entfernen**, um die Ausgaben der Quellkomponente zu verwalten. Alle Eingabezeilen werden an alle verfügbaren Ausgaben weitergeleitet, außer Sie legen einen identischen Wert ungleich Null für die **ExclusionGroup**-Eigenschaft der Ausgaben fest, bei denen Sie jede Zeile nur an eine der Ausgaben weiterleiten möchten, die denselben **ExclusionGroup**-Wert aufweisen. Der spezifische ganzzahlige Wert, den Sie auswählen, um **ExclusionGroup** zu kennzeichnen, ist nicht von Bedeutung.  
  
    > [!NOTE]  
    >  Sie können auch einen Wert ungleich 0 (null) für die **ExclusionGroup**-Eigenschaft für eine einzelne Ausgabe verwenden, wenn Sie nicht alle Zeilen ausgeben möchten. In diesem Fall müssen Sie jedoch für alle Zeilen, die an die Ausgabe gesendet werden sollen, explizit die Methode **DirectRowTo\<outputbuffer>** aufrufen.  
  
-   Sie können allen Ausgaben einen Anzeigenamen zuweisen. Sie verwenden die Namen der Ausgaben in Ihrem Skript später für Verweise mithilfe der typisierten Accessoreigenschaften, die im automatisch generierten Code erstellt wurden.  
  
-   Üblicherweise besitzen mehrere Ausgaben in derselben **ExclusionGroup** die gleichen Ausgabespalten. Falls Sie eine simulierte Fehlerausgabe erstellen, sollten Sie jedoch mehrere Spalten hinzufügen, um Fehlerinformationen zu speichern. Informationen dazu, wie die Datenfluss-Engine Fehlerzeilen verarbeitet, finden Sie unter [Using Error Outputs in a Data Flow Component (Verwenden von Fehlerausgaben in einer Datenflusskomponente)](../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md). In der Skriptkomponente müssen Sie hingegen eigenen Code schreiben, um geeignete Fehlerinformationen für die zusätzlichen Spalten zu erhalten. Weitere Informationen finden Sie unter [Simulating an Error Output for the Script Component (Simulieren einer Fehlerausgabe für die Skriptkomponente)](../../integration-services/extending-packages-scripting-data-flow-script-component-examples/simulating-an-error-output-for-the-script-component.md).  
  
 Weitere Informationen über die Seite **Eingaben und Ausgaben** im **Transformations-Editor für Skripterstellung** finden Sie unter [Script Transformation Editor (Inputs and Outputs Page) (Transformations-Editor für Skripterstellung (Seite „Eingaben und Ausgaben“))](../data-flow/transformations/script-component.md).  
  
### <a name="adding-variables"></a>Hinzufügen von Variablen  
 Wenn Sie die Werte von vorhandenen Variablen in Ihrem Skript verwenden möchten, können Sie diese in den Eigenschaftsfeldern **ReadOnlyVariables** und **ReadWriteVariables** auf der Seite **Skript** im **Transformations-Editor für Skripterstellung** hinzufügen.  
  
 Wenn Sie mehrere Variablen in die Eigenschaftenfelder eingeben, trennen Sie die Variablennamen durch Kommas. Sie können auch mehrere Variablen auswählen, indem Sie auf die Schaltfläche mit den Auslassungspunkten ( **…** ) neben den Eigenschaftenfeldern für **ReadOnlyVariables** und **ReadWriteVariables** klicken und dann die Variablen im Dialogfeld **Variablen auswählen** festlegen.  
  
 Allgemeine Informationen über das Verwenden von Variablen mit der Skriptkomponente finden Sie unter [Using Variables in the Script Component (Verwenden von Variablen in der Skriptkomponente)](../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md).  
  
 Weitere Informationen über die Seite **Skript** im **Transformations-Editor für Skripterstellung** finden Sie unter [Script Transformation Editor (Script Page) (Transformations-Editor für Skripterstellung (Seite „Skript“))](../data-flow/transformations/script-component.md).  
  
## <a name="scripting-a-source-component-in-code-design-mode"></a>Schreiben einer Quellkomponente im Codeentwurfsmodus  
 Nachdem Sie die Metadaten für Ihre Komponente konfiguriert haben, öffnen Sie in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) die IDE, um das benutzerdefinierte Skript zu schreiben. Klicken Sie im **Transformations-Editor für Skripterstellung** auf der Seite **Skript** auf **Skript bearbeiten**, um VSTA zu öffnen. Abhängig von der Skriptsprache, die Sie für die **ScriptLanguage**-Eigenschaft ausgewählt haben, können Sie das Skript entweder in [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic oder [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# schreiben.  
  
 Wichtige Informationen, die alle Arten von Komponenten betreffen, die mithilfe der Skriptkomponente erstellt wurden, finden Sie unter [Coding and Debugging the Script Component (Codieren und Debuggen der Skriptkomponente)](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md).  
  
### <a name="understanding-the-auto-generated-code"></a>Grundlegendes zum automatisch generierten Code  
 Wenn Sie nach der Erstellung und Konfiguration einer Quellkomponente die VSTA-IDE öffnen, wird die bearbeitbare **ScriptMain**-Klasse im Code-Editor angezeigt. Sie schreiben den benutzerdefinierten Code in der **ScriptMain**-Klasse.  
  
 Die **ScriptMain**-Klasse enthält einen Stub für die **CreateNewOutputRows**-Methode. **CreateNewOutputRows** ist die wichtigste Methode in einer Quellkomponente.  
  
 Wenn Sie das Fenster **Projektexplorer** in VSTA öffnen, können Sie sehen, dass die Skriptkomponente auch schreibgeschützte **BufferWrapper**- und **ComponentWrapper**-Projektelemente generiert hat. Die **ScriptMain**-Klasse erbt von der **UserComponent**-Klasse im **ComponentWrapper**-Projektelement.  
  
 Zur Laufzeit ruft die Datenfluss-Engine die **PrimeOutput**-Methode in der **UserComponent**-Klasse auf, die die <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.PrimeOutput%2A>-Methode der übergeordneten <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent>-Klasse überschreibt. Die **PrimeOutput**-Methode ruft anschließend die folgenden Methoden auf:  
  
1.  Die **CreateNewOutputRows**-Methode, die Sie in **ScriptMain** überschreiben können, um den Ausgabepuffern, die zunächst leer sind, Zeilen der Datenquelle hinzuzufügen.  
  
2.  Die **FinishOutputs**-Methode, die standardmäßig leer ist. Überschreiben Sie diese Methode in **ScriptMain**, um Verarbeitungsschritte auszuführen, die zum Abschluss der Ausgabe erforderlich sind.  
  
3.  Die private **MarkOutputsAsFinished**-Methode, die die <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer.SetEndOfRowset%2A>-Methode der übergeordneten <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer>-Klasse aufruft, um die Datenfluss-Engine über die Fertigstellung der Ausgabe in Kenntnis zu setzen. Sie müssen **SetEndOfRowset** nicht explizit im Code aufrufen.  
  
### <a name="writing-your-custom-code"></a>Schreiben von benutzerdefiniertem Code  
 Sie können Skripts in den folgenden Methoden schreiben, die in der **ScriptMain**-Klasse zur Verfügung stehen, um die Erstellung einer benutzerdefinierten Quellkomponente abzuschließen.  
  
1.  Überschreiben Sie die **AcquireConnections**-Methode, um eine Verbindung mit der externen Datenquelle herzustellen. Extrahieren Sie das Verbindungsobjekt bzw. die erforderlichen Verbindungsinformationen vom Verbindungs-Manager.  
  
2.  Überschreiben Sie die **PreExecute**-Methode, um Daten zu laden, falls Sie alle Quelldaten gleichzeitig laden können. Sie können beispielsweise einen **SqlCommand** über eine [!INCLUDE[vstecado](../../includes/vstecado-md.md)]-Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank ausführen und alle Quelldaten gleichzeitig in einen **SqlDataReader** laden. Falls Sie die Quelldaten zeilenweise laden müssen (z.B. beim Lesen einer Textdatei), können Sie die Daten beim Durchlaufen der Zeilen in **CreateNewOutputRows** laden.  
  
3.  Mithilfe der überschriebenen **CreateNewOutputRows**-Methode können Sie leere Ausgabepuffer um neue Zeilen ergänzen und die Werte der einzelnen Spalten den neuen Ausgabezeilen hinzufügen. Verwenden Sie die **AddRow**-Methode der einzelnen Ausgabepuffer, um leere neue Zeilen hinzuzufügen, und legen Sie anschließend die Werte der einzelnen Spalten fest. Üblicherweise kopieren Sie die Werte aus den Spalten, die aus der externen Quelle geladen werden.  
  
4.  Überschreiben Sie die **PostExecute**-Methode, um die Verarbeitung der Daten abzuschließen. Sie können beispielsweise den **SqlDataReader** schließen, mit dem Sie die Daten geladen haben.  
  
5.  Überschreiben Sie bei Bedarf die **ReleaseConnections**-Methode, um die Verbindung mit der externen Datenquelle zu trennen.  
  
## <a name="examples"></a>Beispiele  
 In den folgenden Beispielen wird der benutzerdefinierte Code veranschaulicht, der in der **ScriptMain**-Klasse zur Erstellung einer Quellkomponente erforderlich ist.  
  
> [!NOTE]  
>  In diesen Beispielen werden die erste und die vierte Spalte der Tabelle **Person.Address** in der Beispieldatenbank **AdventureWorks** verwendet, und die Spalten **intAddressID** und **nvarchar(30)City** werden durch den Datenfluss weitergeleitet. Die gleichen Daten werden in den Quellen-, Transformations- und Zielbeispielen in diesem Abschnitt verwendet. Zusätzliche Voraussetzungen und Annahmen werden für jedes Beispiel dokumentiert.  
  
### <a name="adonet-source-example"></a>ADO.NET-Quellenbeispiel  
 Dieses Beispiel zeigt eine Quellkomponente, die einen vorhandenen [!INCLUDE[vstecado](../../includes/vstecado-md.md)]-Verbindungs-Manager zum Laden von Daten aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Tabelle in den Datenfluss verwendet.  
  
 Wenn Sie den Beispielcode ausführen möchten, müssen Sie das Paket und die Komponente folgendermaßen konfigurieren:  
  
1.  Erstellen Sie einen [!INCLUDE[vstecado](../../includes/vstecado-md.md)]-Verbindungs-Manager, der mithilfe des **SqlClient**-Anbieters eine Verbindung mit der **AdventureWorks**-Datenbank herstellt.  
  
2.  Fügen Sie der Datenfluss-Designeroberfläche eine neue Skriptkomponente hinzu, und konfigurieren Sie sie als Quelle.  
  
3.  Öffnen Sie den **Transformations-Editor für Skripterstellung**. Geben Sie auf der Seite **Eingaben und Ausgaben** der Standardausgabe einen aussagekräftigeren Namen ein, z.B. **MyAddressOutput**. Fügen Sie die zwei Ausgabespalten **AddressID** und **City** hinzu, und konfigurieren Sie diese.  
  
    > [!NOTE]  
    >  Achten Sie darauf, den Datentyp der **City**-Ausgabespalte in DT_WSTR zu ändern.  
  
4.  Fügen Sie auf der Seite **Verbindungs-Manager** den [!INCLUDE[vstecado](../../includes/vstecado-md.md)]-Verbindungs-Manager hinzu, oder erstellen Sie diesen, und geben Sie ihm einen Namen wie **MyADONETConnection**.  
  
5.  Klicken Sie auf der Seite **Skript** auf **Skript bearbeiten**, und geben Sie das folgende Skript ein. Schließen Sie anschließend die Skriptentwicklungsumgebung und den **Transformations-Editor für Skripterstellung**.  
  
6.  Erstellen und konfigurieren Sie eine Zielkomponente, die die Spalten **AddressID** und **City** erwartet, z.B. ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ziel oder die Beispielzielkomponente, die unter [Creating a Destination with the Script Component (Erstellen eines Ziels mit der Skriptkomponente)](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md) veranschaulicht wird. Stellen Sie anschließend eine Verbindung der Quellkomponente mit dem Ziel her. (Sie können eine Quelle ohne Transformationen direkt mit einem Ziel verbinden.) Sie können eine Zieltabelle erstellen, indem Sie den folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Befehl in der **AdventureWorks**-Datenbank ausführen:  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
7.  Führen Sie das Beispiel aus.  
  
    ```vb  
    Imports System.Data.SqlClient  
    ...  
    Public Class ScriptMain  
        Inherits UserComponent  
  
        Dim connMgr As IDTSConnectionManager100  
        Dim sqlConn As SqlConnection  
        Dim sqlReader As SqlDataReader  
  
        Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
            connMgr = Me.Connections.MyADONETConnection  
            sqlConn = CType(connMgr.AcquireConnection(Nothing), SqlConnection)  
  
        End Sub  
  
        Public Overrides Sub PreExecute()  
  
            Dim cmd As New SqlCommand("SELECT AddressID, City, StateProvinceID FROM Person.Address", sqlConn)  
            sqlReader = cmd.ExecuteReader  
  
        End Sub  
  
        Public Overrides Sub CreateNewOutputRows()  
  
            Do While sqlReader.Read  
                With MyAddressOutputBuffer  
                    .AddRow()  
                    .AddressID = sqlReader.GetInt32(0)  
                    .City = sqlReader.GetString(1)  
                End With  
            Loop  
  
        End Sub  
  
        Public Overrides Sub PostExecute()  
  
            sqlReader.Close()  
  
        End Sub  
  
        Public Overrides Sub ReleaseConnections()  
  
            connMgr.ReleaseConnection(sqlConn)  
  
        End Sub  
  
    End Class  
    ```  
  
    ```csharp  
    using System.Data.SqlClient;  
    public class ScriptMain:  
        UserComponent  
  
    {  
        IDTSConnectionManager100 connMgr;  
        SqlConnection sqlConn;  
        SqlDataReader sqlReader;  
  
        public override void AcquireConnections(object Transaction)  
        {  
            connMgr = this.Connections.MyADONETConnection;  
            sqlConn = (SqlConnection)connMgr.AcquireConnection(null);  
  
        }  
  
        public override void PreExecute()  
        {  
  
            SqlCommand cmd = new SqlCommand("SELECT AddressID, City, StateProvinceID FROM Person.Address", sqlConn);  
            sqlReader = cmd.ExecuteReader();  
  
        }  
  
        public override void CreateNewOutputRows()  
        {  
  
            while (sqlReader.Read())  
            {  
                {  
                    MyAddressOutputBuffer.AddRow();  
                    MyAddressOutputBuffer.AddressID = sqlReader.GetInt32(0);  
                    MyAddressOutputBuffer.City = sqlReader.GetString(1);  
                }  
            }  
  
        }  
  
        public override void PostExecute()  
        {  
  
            sqlReader.Close();  
  
        }  
  
        public override void ReleaseConnections()  
        {  
  
            connMgr.ReleaseConnection(sqlConn);  
  
        }  
  
    }  
    ```  
  
### <a name="flat-file-source-example"></a>Flatfilequellenbeispiel  
 Diese Beispiel zeigt eine Quellkomponente, die einen vorhandenen Verbindungs-Manager für Flatfiles zum Laden von Daten aus einer Flatfile in den Datenfluss einsetzt. Die Flatfilequelldaten werden durch einen Export aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellt.  
  
 Wenn Sie den Beispielcode ausführen möchten, müssen Sie das Paket und die Komponente folgendermaßen konfigurieren:  
  
1.  Exportieren Sie mithilfe des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Import/Export-Assistenten die Tabelle **Person.Address** aus der **AdventureWorks**-Beispieldatenbank in eine durch Trennzeichen getrennte Flatfile. In diesem Beispiel wird der Dateiname ExportedAddresses.txt verwendet.  
  
2.  Erstellen Sie einen Verbindungs-Manager für Flatfiles, der eine Verbindung mit der exportierten Datendatei herstellt.  
  
3.  Fügen Sie der Datenfluss-Designeroberfläche eine neue Skriptkomponente hinzu, und konfigurieren Sie sie als Quelle.  
  
4.  Öffnen Sie den **Transformations-Editor für Skripterstellung**. Geben Sie auf der Seite **Eingaben und Ausgaben** der Standardausgabe einen aussagekräftigeren Namen ein, z.B. **MyAddressOutput**. Fügen Sie die beiden Ausgabespalten **AddressID** und **City** hinzu, und konfigurieren Sie diese.  
  
5.  Fügen Sie auf der Seite **Verbindungs-Manager** den Verbindungs-Manager für Flatfiles hinzu, oder erstellen Sie diesen, und geben Sie ihm einen aussagekräftigen Namen, z.B. **MyFlatFileSrcConnectionManager**.  
  
6.  Klicken Sie auf der Seite **Skript** auf **Skript bearbeiten**, und geben Sie das folgende Skript ein. Schließen Sie anschließend die Skriptentwicklungsumgebung und den **Transformations-Editor für Skripterstellung**.  
  
7.  Erstellen und konfigurieren Sie eine Zielkomponente, z.B. ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ziel oder die Beispielzielkomponente, die unter [Creating a Destination with the Script Component (Erstellen eines Ziels mit der Skriptkomponente)](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md) veranschaulicht wird. Stellen Sie anschließend eine Verbindung der Quellkomponente mit dem Ziel her. (Sie können eine Quelle ohne Transformationen direkt mit einem Ziel verbinden.) Sie können eine Zieltabelle erstellen, indem Sie den folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Befehl in der **AdventureWorks**-Datenbank ausführen:  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
8.  Führen Sie das Beispiel aus.  
  
    ```vb  
    Imports System.IO  
    ...  
    Public Class ScriptMain  
        Inherits UserComponent  
  
        Private textReader As StreamReader  
        Private exportedAddressFile As String  
  
        Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
            Dim connMgr As IDTSConnectionManager100 = _  
                Me.Connections.MyFlatFileSrcConnectionManager  
            exportedAddressFile = _  
                CType(connMgr.AcquireConnection(Nothing), String)  
  
        End Sub  
  
        Public Overrides Sub PreExecute()  
            MyBase.PreExecute()  
            textReader = New StreamReader(exportedAddressFile)  
        End Sub  
  
        Public Overrides Sub CreateNewOutputRows()  
  
            Dim nextLine As String  
            Dim columns As String()  
  
            Dim delimiters As Char()  
            delimiters = ",".ToCharArray  
  
            nextLine = textReader.ReadLine  
            Do While nextLine IsNot Nothing  
                columns = nextLine.Split(delimiters)  
                With MyAddressOutputBuffer  
                    .AddRow()  
                    .AddressID = columns(0)  
                    .City = columns(3)  
                End With  
                nextLine = textReader.ReadLine  
            Loop  
  
        End Sub  
  
        Public Overrides Sub PostExecute()  
            MyBase.PostExecute()  
            textReader.Close()  
  
        End Sub  
  
    End Class  
    ```  
  
    ```csharp  
    using System.IO;  
    public class ScriptMain:  
        UserComponent  
  
    {  
        private StreamReader textReader;  
        private string exportedAddressFile;  
  
        public override void AcquireConnections(object Transaction)  
        {  
  
            IDTSConnectionManager100 connMgr = this.Connections.MyFlatFileSrcConnectionManager;  
            exportedAddressFile = (string)connMgr.AcquireConnection(null);  
  
        }  
  
        public override void PreExecute()  
        {  
            base.PreExecute();  
            textReader = new StreamReader(exportedAddressFile);  
        }  
  
        public override void CreateNewOutputRows()  
        {  
  
            string nextLine;  
            string[] columns;  
  
            char[] delimiters;  
            delimiters = ",".ToCharArray();  
  
            nextLine = textReader.ReadLine();  
            while (nextLine != null)  
            {  
                columns = nextLine.Split(delimiters);  
                {  
                    MyAddressOutputBuffer.AddRow();  
                    MyAddressOutputBuffer.AddressID = columns[0];  
                    MyAddressOutputBuffer.City = columns[3];  
                }  
                nextLine = textReader.ReadLine();  
            }  
  
        }  
  
        public override void PostExecute()  
        {  
  
            base.PostExecute();  
            textReader.Close();  
  
        }  
  
    }  
    ```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Creating a Destination with the Script Component (Erstellen eines Ziels mit der Skriptkomponente)](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md)   
 [Entwickeln einer benutzerdefinierten Quellkomponente](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md)  
  
