---
description: Entwickeln einer Benutzeroberfläche für einen benutzerdefinierten Task
title: Entwickeln einer Benutzeroberfläche für einen benutzerdefinierten Task | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom user interfaces [Integration Services]
- IDtsTaskUI interface
- DtsTaskAttribute attribute
- custom tasks [Integration Services], user interface
- custom user interface [Integration Services], custom tasks
- user interface [Integration Services]
- SSIS custom tasks, user interface
ms.assetid: 1e940cd1-c5f8-4527-b678-e89ba5dc398a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f0536913b55a3e798b7306f5bd2fa36d115e5f84
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337698"
---
# <a name="developing-a-user-interface-for-a-custom-task"></a>Entwickeln einer Benutzeroberfläche für einen benutzerdefinierten Task

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Das [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]-Objektmodell bietet Entwicklern benutzerdefinierter Tasks eine einfache Möglichkeit, eine individuelle Benutzeroberfläche für einen Task zu erstellen, der dann in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] integriert und angezeigt werden kann. Die Benutzeroberfläche kann nützliche Informationen für den Benutzer im [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer bereitstellen und den Benutzern Hinweise geben, wie sie die Eigenschaften und Einstellungen des benutzerdefinierten Tasks ordnungsgemäß konfigurieren können.  
  
 Bei der Entwicklung einer benutzerdefinierten Benutzeroberfläche für einen Task werden zwei wichtige Klassen verwendet. Diese Klassen werden in der folgenden Tabelle beschrieben.  
  
|Klasse|BESCHREIBUNG|  
|-----------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>|Ein Attribut, das einen verwalteten Task identifiziert und über seine Eigenschaften Informationen zur Entwurfszeit angibt, um zu kontrollieren, wie der [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer das Objekt anzeigt, bzw. wie er mit ihm interagiert.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI>|Eine vom Task verwendete Schnittstelle, um den Task seiner benutzerdefinierten Benutzeroberfläche zuzuordnen.|  
  
 Dieser Abschnitt beschreibt die Rolle der <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>-Attribute und der <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI>-Schnittstelle bei der Entwicklung einer Benutzeroberfläche für einen benutzerdefinierten Task und enthält Einzelheiten zur Erstellung, Integration, Bereitstellung und dem Debuggen des Tasks innerhalb des [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designers.  
  
 Der [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer stellt mehrere Einstiegspunkte zur Benutzeroberfläche für den Task bereit: Der Benutzer kann im Kontextmenü **Bearbeiten** auswählen, auf den Task doppelklicken oder unten auf dem Eigenschaftenblatt auf den Link **Editor anzeigen** klicken. Wenn der Benutzer auf einen dieser Einstiegspunkte zugreift, dann sucht und lädt der [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer die Assembly, die die Benutzeroberfläche für den Task enthält. Die Benutzeroberfläche für den Task ist für die Erstellung des Eigenschaftendialogfelds verantwortlich, das dem Benutzer in [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] angezeigt wird.  
  
 Ein Task und seine Benutzeroberfläche sind separate Entitäten. Sie sollten in separaten Assemblys implementiert werden, um den Aufwand bei der Lokalisierung, der Bereitstellung und den Wartungsarbeiten zu reduzieren. Mit Ausnahme der Informationen, die in den im Task codierten<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>-Attributwerten enthalten sind, werden von der DLL keine Informationen zur Benutzeroerfläche geladen oder aufgerufen bzw. sind solche Informationen in der Regel nicht in ihr enthalten. Dies ist die einzige Möglichkeit der Zuordnung eines Task zu seiner Benutzeroberfläche.  
  
## <a name="the-dtstask-attribute"></a>Das DtsTask-Attribut  
 Das <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>-Attribut ist im Code der Taskklasse enthalten, um den Task seiner Benutzeroberfläche zuzuordnen. Der [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer nutzt die Eigenschaften des Attributs, um zu ermitteln, wie der Task im Designer angezeigt werden soll.  Diese Eigenschaften schließen den anzuzeigenden Namen und ggf. das Symbol ein.  
  
 In der folgenden Tabelle werden die Eigenschaften des <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>-Attributs beschrieben.  
  
|Eigenschaft|BESCHREIBUNG|  
|--------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.DisplayName%2A>|Zeigt den Tasknamen in der Toolbox der Ablaufsteuerung an.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.Description%2A>|Die Taskbeschreibung (geerbt von <xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute>). Diese Eigenschaft wird in QuickInfos angezeigt.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A>|Das im [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer angezeigte Symbol.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.RequiredProductLevel%2A>|Sollte bei Verwendung auf einen der Werte in der <xref:Microsoft.SqlServer.Dts.Runtime.DTSProductLevel>-Enumeration festgelegt werden. Beispiel: `RequiredProductLevel = DTSProductLevel.None`.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.TaskContact%2A>|Enthält Kontaktinformationen, falls für den Task technischer Support benötigt wird.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.TaskType%2A>|Weist dem Task einen Typ zu.|  
|Attribute.TypeId|Ruft bei Implementierung in einer abgeleiteten Klasse einen eindeutigen Bezeichner für dieses Attribut ab. Weitere Informationen finden Sie in der .NET Framework-Klassenbibliothek unter der Eigenschaft **Attribute.TypeID**.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.UITypeName%2A>|Der Typname der Assembly, der vom [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer zum Laden der Assembly verwendet wird. Diese Eigenschaft wird verwendet, um die Benutzeroberflächenassembly für den Task zu suchen.|  
  
 Im folgenden Codebeispiel wird das <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> gezeigt, wie es aussehen würde, wenn es oberhalb der Klassendefinition codiert wäre.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
namespace Microsoft.SSIS.Samples  
{  
  [DtsTask  
  (  
   DisplayName = "MyTask",  
   IconResource = "MyTask.MyTaskIcon.ico",  
   UITypeName = "My Custom Task," +  
   "Version=1.0.0.0," +  
   "Culture = Neutral," +  
   "PublicKeyToken = 12345abc6789de01",  
   TaskType = "PackageMaintenance",  
   TaskContact = "MyTask; company name; any other information",  
   RequiredProductLevel = DTSProductLevel.None  
   )]  
  public class MyTask : Task  
  {  
    // Your code here.  
  }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsTask(DisplayName:="MyTask", _  
 IconResource:="MyTask.MyTaskIcon.ico", _  
 UITypeName:="My Custom Task," & _  
 "Version=1.0.0.0,Culture=Neutral," & _  
 "PublicKeyToken=12345abc6789de01", _  
 TaskType:="PackageMaintenance", _  
 TaskContact:="MyTask; company name; any other information", _  
 RequiredProductLevel:=DTSProductLevel.None)> _  
Public Class MyTask  
  Inherits Task  
  
  ' Your code here.  
  
End Class 'MyTask  
```  
  
 Der [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer sucht mithilfe der <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.UITypeName%2A>-Eigenschaft des Attributs, das den Namen, den Typnamen, die Version, die Kultur und das öffentliche Schlüsseltoken der Assembly enthält, um die Assembly im globalen Assemblycache (GAC) zu suchen und sie für die Verwendung durch den Designer zu laden.  
  
 Wenn die Assembly gefunden wurde, verwendet der [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer die anderen Eigenschaften im Attribut, um zusätzliche Informationen zum Task im [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer, wie z. B. Namen, Symbol und Beschreibung des Tasks anzuzeigen.  
  
 Die Eigenschaften <xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.DisplayName%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.Description%2A> und <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A> geben an, wie dem Benutzer der Task präsentiert wird. Die <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A>-Eigenschaft enthält die Ressourcen-ID des in der Benutzeroberflächenassembly eingebetteten Symbols. Der Designer lädt die Symbolressource über die ID aus der Assembly und zeigt sie neben dem Tasknamen in der Toolbox sowie auf der Designeroberfläche an, wenn der Task zum Paket hinzugefügt wird. Wenn ein Task keine Symbolressource bereitstellt, dann verwendet der Designer für den Task ein Standardsymbol.  
  
## <a name="the-idtstaskui-interface"></a>Die IDTSTaskUI-Schnittstelle  
 Die <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI>-Schnittstelle definiert die Auflistung von Methoden und Eigenschaften, die vom [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer aufgerufen werden, um die mit dem Task verbundene Benutzeroberfläche zu initialisieren und anzuzeigen. Wenn die Benutzeroberfläche für einen Task aufgerufen wird, dann ruft der Designer die <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.Initialize%2A>-Methode auf, die von der Task-Benutzeroberfläche implementiert wurde, als Sie diese schrieben, und stellt dann die <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>- und <xref:Microsoft.SqlServer.Dts.Runtime.Connections>-Auflistungen des Tasks und des Pakets jeweils als Parameter bereit. Diese Auflistungen werden lokal gespeichert und anschließend in der <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A>-Methode verwendet.  
  
 Der Designer ruft die <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A>-Methode auf, um das Fenster anzufordern, das im [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer angezeigt wird. Der Task erstellt eine Instanz des Fensters, das die Benutzeroberfläche für den Task enthält, und gibt die Benutzeroberfläche wieder zur Anzeige an den Designer zurück. In der Regel werden die <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>- und <xref:Microsoft.SqlServer.Dts.Runtime.Connections>-Objekte dem Fenster über einen überladenen Konstruktor bereitgestellt, sodass sie zum Konfigurieren des Tasks verwendet werden können.  
  
 Der [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer ruft die <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A>-Methode der Task-Benutzeroberfläche auf, um die Benutzeroberfläche für den Task anzuzeigen. Die Task-Benutzeroberfläche gibt das Windows Form von dieser Methode zurück, und der [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer zeigt dieses Formular als modales Dialogfeld an. Wenn das Formular geschlossen ist, untersucht der [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer den Wert der Eigenschaft **DialogResult** des Formulars, um zu ermitteln, ob der Task geändert wurde und ob diese Änderungen gespeichert werde sollten. Wenn der Wert der **DialogResult**-Eigenschaft **OK** lautet, dann ruft der [!INCLUDE[ssIS](../../../includes/ssis-md.md)]-Designer die Persistenzmethoden des Tasks auf, um die Änderungen zu speichern; andernfalls werden die Änderungen verworfen.  
  
 Das folgende Codebeispiel implementiert die <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI>-Schnittstelle, wobei es von der Existenz einer Windows Form-Klasse mit dem Namen SampleTaskForm ausgeht.  
  
```csharp  
using System;  
using System.Windows.Forms;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Runtime.Design;  
  
namespace Sample  
{  
   public class HelloWorldTaskUI : IDtsTaskUI  
   {  
      TaskHost   taskHost;  
      Connections connections;  
      public void Initialize(TaskHost taskHost, IServiceProvider serviceProvider)  
      {  
         this.taskHost = taskHost;  
         IDtsConnectionService cs = serviceProvider.GetService  
         ( typeof( IDtsConnectionService ) ) as   IDtsConnectionService;   
         this.connections = cs.GetConnections();  
      }  
      public ContainerControl GetView()  
      {  
        return new HelloWorldTaskForm(this.taskHost, this.connections);  
      }  
     public void Delete(IWin32Window parentWindow)  
     {  
     }  
     public void New(IWin32Window parentWindow)  
     {  
     }  
   }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Runtime.Design  
Imports System.Windows.Forms  
  
Public Class HelloWorldTaskUI  
  Implements IDtsTaskUI  
  
  Dim taskHost As TaskHost  
  Dim connections As Connections  
  
  Public Sub Initialize(ByVal taskHost As TaskHost, ByVal serviceProvider As IServiceProvider) _  
    Implements IDtsTaskUI.Initialize  
  
    Dim cs As IDtsConnectionService  
  
    Me.taskHost = taskHost  
    cs = DirectCast(serviceProvider.GetService(GetType(IDtsConnectionService)), IDtsConnectionService)  
    Me.connections = cs.GetConnections()  
  
  End Sub  
  
  Public Function GetView() As ContainerControl _  
    Implements IDtsTaskUI.GetView  
  
    Return New HelloWorldTaskForm(Me.taskHost, Me.connections)  
  
  End Function  
  
  Public Sub Delete(ByVal parentWindow As IWin32Window) _  
    Implements IDtsTaskUI.Delete  
  
  End Sub  
  
  Public Sub [New](ByVal parentWindow As IWin32Window) _  
    Implements IDtsTaskUI.[New]  
  
  End Sub  
  
End Class  
```  
 
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen eines benutzerdefinierten Tasks](../../../integration-services/extending-packages-custom-objects/task/creating-a-custom-task.md)   
 [Codieren eines benutzerdefinierten Tasks](../../../integration-services/extending-packages-custom-objects/task/coding-a-custom-task.md)   
 [Entwickeln einer Benutzeroberfläche für einen benutzerdefinierten Task](../../../integration-services/extending-packages-custom-objects/task/developing-a-user-interface-for-a-custom-task.md)  
  
  
