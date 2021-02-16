---
description: Entwickeln einer Benutzeroberfläche für eine Datenflusskomponente
title: Entwickeln einer Benutzeroberfläche für eine Datenflusskomponente | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- data flow components [Integration Services], custom editors
- user interfaces [Integration Services]
- custom data flow components [Integration Services], custom editors
- custom component editors [Integration Services]
- IDtsComponentUI interface
- UITypeName property
- custom user interface [Integration Services], custom data flow component
- editors [Integration Services]
ms.assetid: 10b829a1-609b-42e3-9070-cfe5a2bb698c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cfe2ebf4e63237f9e5039188c5f9fec883fd797e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343699"
---
# <a name="developing-a-user-interface-for-a-data-flow-component"></a>Entwickeln einer Benutzeroberfläche für eine Datenflusskomponente

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Komponentenentwickler können für eine Komponente eine benutzerdefinierte Benutzeroberfläche bereitstellen, die in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] angezeigt wird, wenn die Komponente bearbeitet wird. Bei der Implementierung einer benutzerdefinierten Benutzeroberfläche werden Ihnen Benachrichtigungen bereitgestellt, wenn die Komponente zur Datenflusstask hinzugefügt oder aus diesem gelöscht wird und wenn für die Komponente Hilfe angefordert wird.  
  
 Wenn Sie keine individuelle Benutzeroberfläche für Ihre Komponente bereitstellen, können die Benutzer die Komponente und ihre benutzerdefinieren Eigenschaften auch mithilfe des erweiterten Editors konfigurieren. Sie können sicherstellen, dass der erweiterte Editor es Benutzern ermöglicht, mithilfe der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.TypeConverter%2A>- und <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.UITypeEditor%2A>-Eigenschaften von <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100> benutzerdefinierte Eigenschaftswerte ggf. entsprechend zu bearbeiten. Weitere Informationen finden Sie in [Entwurfszeitmethoden einer Datenflusskomponente](../../../integration-services/extending-packages-custom-objects/data-flow/design-time-methods-of-a-data-flow-component.md) unter „Erstellen von benutzerdefinierten Eigenschaften“.  
  
## <a name="setting-the-uitypename-property"></a>Festlegen der UITypeName-Eigenschaft  
 Für die Bereitstellung einer benutzerdefinierten Benutzeroberfläche muss der Entwickler die <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A>-Eigenschaft von <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute> auf den Namen einer Klasse festlegen, die die <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>-Schnittstelle implementiert. Bei Festlegung dieser Eigenschaft durch die Komponente wird die benutzerdefinierte Oberfläche von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] geladen und aufgerufen, wenn die Komponente im [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer bearbeitet wird.  
  
 Die <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A>-Eigenschaft ist eine durch Trennzeichen getrennte Zeichenfolge, die den vollqualifizierten Namen des Typs angibt. Die folgende Liste zeigt die Elemente, die den Typ identifizieren, in Reihenfolge, an:  
  
-   Typname  
  
-   Assemblyname  
  
-   Dateiversion  
  
-   Kultur  
  
-   Öffentliches Schlüsseltoken  
  
 Im folgenden Codebeispiel wird eine Klasse veranschaulicht, die von der <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>-Basisklasse abgeleitet wird und die die <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A>-Eigenschaft angibt.  
  
```csharp  
[DtsPipelineComponent(  
DisplayName="SampleComponent",  
UITypeName="MyNamespace.MyComponentUIClassName,MyAssemblyName,Version=1.0.0.0,Culture=neutral,PublicKeyToken=abcd...",  
ComponentType = ComponentType.Transform)]  
public class SampleComponent : PipelineComponent  
{  
//TODO: Implement the component here.  
}  
```  
  
```vb  
<DtsPipelineComponent(DisplayName="SampleComponent", _  
UITypeName="MyNamespace.MyComponentUIClassName,MyAssemblyName,Version=1.0.0.0,Culture=neutral,PublicKeyToken=abcd...", ComponentType=ComponentType.Transform)> _   
Public Class SampleComponent   
 Inherits PipelineComponent   
End Class  
```  
  
## <a name="implementing-the-idtscomponentui-interface"></a>Implementieren der IDtsComponentUI-Schnittstelle  
 Die <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>-Schnittstelle enthält Methoden, die der [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer aufruft, wenn eine Komponente hinzugefügt, gelöscht oder bearbeitet wird. Komponentenentwickler können zur Interaktion mit den Benutzern der Komponente Code in der Implementierung dieser Methoden bereitstellen.  
  
 Diese Klasse wird in der Regel in einer Assembly implementiert, die von der Komponente selbst getrennt ist. Die Verwendung einer separaten Assembly ist zwar nicht erforderlich, doch dadurch kann der Entwickler die Komponente und die Benutzeroberfläche unabhängig voneinander erstellen und bereitstellen, und der Speicherbedarf der Komponente wird gering gehalten.  
  
 Durch die Implementierung einer benutzerdefinierten Benutzeroberfläche hat der Komponentenentwickler mehr Steuerungsmöglichkeiten für die Komponente, wenn diese im [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer bearbeitet wird. So kann die Komponente Code zu der <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.New%2A>-Methode hinzufügen, die aufgerufen wird, wenn eine Komponte erstmals zum Datenflusstask hinzugefügt wird, und einen Assistenten anzeigen, der den Benutzer durch die anfängliche Konfiguration der Komponente begleitet.  
  
 Nachdem Sie eine Klasse erstellt haben, die die <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>-Schnittstelle implementiert, müssen Sie Code hinzufügen, mit dem auf Interaktionen der Benutzer mit der Komponente reagiert wird. Die <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A>-Methode stellt die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>-Schnittstelle der Komponente bereit und wird vor den <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.New%2A>- und <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A>-Methoden aufgerufen. Dieser Verweis sollte in einer privaten Membervariablen gespeichert und zur anschließenden Änderung der Metadaten der Komponente verwendet werden.  
  
## <a name="modifying-a-component-and-persisting-changes"></a>Ändern einer Komponente und Beibehalten von Änderungen  
 Die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>-Schnittstelle wird der <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A>-Methode als Parameter bereitgestellt. Dieser Verweis sollte in einer Membervariablen vom Benutzeroberflächencode zwischengespeichert werden und dann verwendet werden, um die Komponente als Reaktion auf Interaktionen der Benutzer mit der Benutzeroberfläche zu ändern.  
  
 Obwohl Sie die Komponente direkt über die <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>-Schnittstelle ändern können, ist es vorteilhafter, eine Instanz von <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapper> mithilfe der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.Instantiate%2A>-Methode zu erstellen. Wenn Sie die Komponente direkt mithilfe der Schnittstelle bearbeiten, umgehen Sie die Schutzmaßnahmen zur Überprüfung der Komponente. Die Verwendung der Entwurfszeitinstanz der Komponente über <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapper> hat den Vorteil, sicherzustellen, dass die Komponente, die an ihr vorgenommenen Änderungen steuern kann.  
  
 Der Rückgabewert der <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A>-Methode bestimmt, ob die an einer Komponente vorgenommenen Änderungen beibehalten oder verworfen werden. Wenn diese Methode **false** zurückgibt, dann werden alle Änderungen verworfen; bei **true** werden die Änderungen an der Komponente beibehalten und durch eine Kennzeichnung angegeben, dass das Paket gespeichert werden muss.  
  
### <a name="using-the-services-of-the-ssis-designer"></a>Verwenden der Dienste des SSIS-Designers  
 Der **IServiceProvider**-Parameter der <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A>-Methode bietet Zugriff auf die folgenden Dienste des [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designers:  
  
|Dienst|BESCHREIBUNG|  
|-------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Design.IDtsClipboardService>|Wird verwendet, um zu bestimmen, ob die Komponente als Teil eines Kopier-/Einfüge- oder Ausschneide-/Einfügevorgangs generiert wurde.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionService>|Wird verwendet, um auf vorhandene Verbindungen zuzugreifen oder neue Verbindungen im Paket zu erstellen.|  
|<xref:Microsoft.SqlServer.Dts.Design.IErrorCollectionService>|Wird verwendet, um Ereignisse von Datenflusskomponenten aufzuzeichnen, wenn Sie alle Fehler und Warnungen, die von der Komponente ausgelöst wurden, erfassen, und nicht nur den letzten Fehler oder die letzte Warnung, empfangen müssen.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsVariableService>|Wird verwendet, um auf vorhandene Variablen zuzugreifen oder neue Variablen im Paket zu erstellen.|  
|<xref:Microsoft.SqlServer.Dts.Design.IDtsPipelineEnvironmentService>|Wird von Datenflusskomponenten für den Zugriff auf den übergeordneten Datenflusstask und andere Komponenten im Datenfluss verwendet. Diese Funktion könnte z. B. verwendet werden, um eine Komponente wie den Assistent für langsam veränderliche Dimensionen zu entwickeln, der bei Bedarf zusätzliche Datenflusskomponenten erstellt und verbindet.|  
  
 Diese Dienste bieten Komponentenentwicklern die Möglichkeit, Objekte in dem Paket, in das die Komponete geladen ist, zu erstellen bzw. auf diese zuzugreifen.  
  
## <a name="sample"></a>Beispiel  
 Im folgenden Codebeispiel wird die Integration einer benutzerdefinierten Benutzeroberflächenklasse gezeigt, die die <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>-Schnittstelle implementiert, sowie ein Windows Form, das als Editor für eine Komponente dient.  
  
### <a name="custom-user-interface-class"></a>Benutzerdefinierte Benutzeroberflächenklasse  
 Das folgende Codebeispiel zeigt die Klasse, die die <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>-Schnittstelle implementiert. Die <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A>-Methode erstellt den Komponenten-Editor und zeigt dann das Formular an. Der Rückgabewert des Formulars bestimmt, ob die an der Komponente vorgenommenen Änderungen beibehalten werden.  
  
```csharp  
using System;  
using System.Windows.Forms;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Pipeline.Design;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    public class SampleComponentUI : IDtsComponentUI  
    {  
        IDTSComponentMetaData100 md;  
        IServiceProvider sp;  
  
        public void Help(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public void New(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public void Delete(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public bool Edit(System.Windows.Forms.IWin32Window parentWindow, Variables vars, Connections cons)  
        {  
            // Create and display the form for the user interface.  
            SampleComponentUIForm componentEditor = new SampleComponentUIForm(cons, vars, md);  
  
            DialogResult result  = componentEditor.ShowDialog(parentWindow);  
  
            if (result == DialogResult.OK)  
                return true;  
  
            return false;  
        }  
        public void Initialize(IDTSComponentMetaData100 dtsComponentMetadata, IServiceProvider serviceProvider)  
        {  
            // Store the component metadata.  
            this.md = dtsComponentMetadata;  
        }  
    }  
}  
```  
  
```vb  
Imports System   
Imports System.Windows.Forms   
Imports Microsoft.SqlServer.Dts.Runtime   
Imports Microsoft.SqlServer.Dts.Pipeline.Design   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
  
Namespace Microsoft.Samples.SqlServer.Dts   
  
 Public Class SampleComponentUI   
 Implements IDtsComponentUI   
   Private md As IDTSComponentMetaData100   
   Private sp As IServiceProvider   
  
   Public Sub Help(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Sub New(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Sub Delete(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Function Edit(ByVal parentWindow As System.Windows.Forms.IWin32Window, ByVal vars As Variables, ByVal cons As Connections) As Boolean   
     ' Create and display the form for the user interface.  
     Dim componentEditor As SampleComponentUIForm = New SampleComponentUIForm(cons, vars, md)   
     Dim result As DialogResult = componentEditor.ShowDialog(parentWindow)   
     If result = DialogResult.OK Then   
       Return True   
     End If   
     Return False   
   End Function   
  
   Public Sub Initialize(ByVal dtsComponentMetadata As IDTSComponentMetaData100, ByVal serviceProvider As IServiceProvider)   
     Me.md = dtsComponentMetadata   
   End Sub   
 End Class   
  
End Namespace  
```  
  
### <a name="custom-editor"></a>Benutzerdefinierter Editor  
 Das folgende Codebeispiel zeigt die Implementierung des Windows Form, das während des Aufrufs der <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A>-Methode angezeigt wird.  
  
```csharp  
using System;  
using System.Drawing;  
using System.Collections;  
using System.ComponentModel;  
using System.Windows.Forms;  
using System.Data;  
  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    public partial class SampleComponentUIForm : System.Windows.Forms.Form  
    {  
        private Connections connections;  
        private Variables variables;  
        private IDTSComponentMetaData100 metaData;  
        private CManagedComponentWrapper designTimeInstance;  
        private System.ComponentModel.IContainer components = null;  
  
        public SampleComponentUIForm( Connections cons, Variables vars, IDTSComponentMetaData100 md)  
        {  
            variables = vars;  
            connections = cons;  
            metaData = md;  
        }  
  
        private void btnOk_Click(object sender, System.EventArgs e)  
        {  
            if (designTimeInstance == null)  
                designTimeInstance = metaData.Instantiate();  
  
            designTimeInstance.SetComponentProperty( "CustomProperty", txtCustomPropertyValue.Text);  
  
            this.Close();  
        }  
  
        private void btnCancel_Click(object sender, System.EventArgs e)  
        {  
            this.Close();  
        }  
    }  
}  
```  
  
```vb  
Imports System   
Imports System.Drawing   
Imports System.Collections   
Imports System.ComponentModel   
Imports System.Windows.Forms   
Imports System.Data   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
Imports Microsoft.SqlServer.Dts.Runtime   
  
Namespace Microsoft.Samples.SqlServer.Dts   
  
 Public Partial Class SampleComponentUIForm   
  Inherits System.Windows.Forms.Form   
   Private connections As Connections   
   Private variables As Variables   
   Private metaData As IDTSComponentMetaData100   
   Private designTimeInstance As CManagedComponentWrapper   
   Private components As System.ComponentModel.IContainer = Nothing   
  
   Public Sub New(ByVal cons As Connections, ByVal vars As Variables, ByVal md As IDTSComponentMetaData100)   
     variables = vars   
     connections = cons   
     metaData = md   
   End Sub   
  
   Private Sub btnOk_Click(ByVal sender As Object, ByVal e As System.EventArgs)   
     If designTimeInstance Is Nothing Then   
       designTimeInstance = metaData.Instantiate   
     End If   
     designTimeInstance.SetComponentProperty("CustomProperty", txtCustomPropertyValue.Text)   
     Me.Close   
   End Sub   
  
   Private Sub btnCancel_Click(ByVal sender As Object, ByVal e As System.EventArgs)   
     Me.Close   
   End Sub   
 End Class   
  
End Namespace  
```
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen einer benutzerdefinierten Datenflusskomponente](../../../integration-services/extending-packages-custom-objects/data-flow/creating-a-custom-data-flow-component.md)  
  
  
