---
description: Programmgesteuertes Arbeiten mit Variablen
title: Programmgesteuertes Arbeiten mit Variablen | Microsoft-Dokumentation
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
- System namespace
- scope [Integration Services]
- variables [Integration Services], programmatically
- configuration files [Integration Services]
- Variables collection
- User namespace
- custom variables [Integration Services]
- variables [Integration Services], customizing
ms.assetid: c4b76a3d-94ca-4a8e-bb45-cb8bd0ea3ec1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8258d74d7f81b17d1e3e7973011a036031bd78da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355011"
---
# <a name="working-with-variables-programmatically"></a>Programmgesteuertes Arbeiten mit Variablen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Variablen bieten die Möglichkeit, Werte dynamisch festzulegen und Prozesse in Paketen, Containern, Tasks und Ereignishandlern zu steuern. Variablen können auch von Rangfolgeneinschränkungen verwendet werden, um die Richtung des Datenflusses an andere Tasks zu steuern. Variablen haben vielerlei Verwendungszwecke:  
  
-   Aktualisieren der Eigenschaften eines Pakets zur Laufzeit.  
  
-   Auffüllen von Parameterwerten für Transact-SQL-Anweisungen zur Laufzeit.  
  
-   Kontrollieren des Flusses einer Foreach-Schleife. Weitere Informationen finden Sie unter [Hinzufügen einer Enumeration zu einer Ablaufsteuerung](../control-flow/foreach-loop-container.md).  
  
-   Steuern einer Rangfolgeneinschränkung durch ihre Verwendung in einem Ausdruck. Eine Rangfolgeneinschränkung kann Variablen in die Einschränkungsdefinition einschließen. Weitere Informationen finden Sie unter [Hinzufügen von Ausdrücken zu Rangfolgeneinschränkungen](../control-flow/precedence-constraints.md).  
  
-   Kontrollieren der bedingten Wiederholung eines For-Schleifencontainers. Weitere Informationen finden Sie unter [Hinzufügen einer Iteration zu einer Ablaufsteuerung](../control-flow/for-loop-container.md).  
  
-   Erstellen von Ausdrücken, die Variablenwerte einschließen.  
  
-   Sie können benutzerdefinierte Variablen für alle Containertypen erstellen: Pakete, **Foreach-Schleifencontainer**, **For-Schleifencontainer**, **Sequenzcontainer**, TasksHosts und Ereignishandler. Weitere Informationen finden Sie unter [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) und [Verwenden von Variablen in Paketen](../integration-services-ssis-variables.md).  
  
## <a name="scope"></a>`Scope`  
 Jeder Container weist seine eigene <xref:Microsoft.SqlServer.Dts.Runtime.Variables>-Auflistung auf. Wenn eine neue Variable erstellt wird, liegt diese innerhalb des Bereichs seines übergeordneten Containers. Da sich der Paketcontainer ganz oben in der Containerhierarchie befindet, funktionieren Variablen mit Paketbereich wie globale Variablen und sind für alle Container innerhalb des Pakets sichtbar. Auf die Auflistung von Variablen für den Container kann über die <xref:Microsoft.SqlServer.Dts.Runtime.Variables>-Auflistung auch von untergeordneten Elementen des Containers zugegriffen werden, indem entweder der Variablenname oder der Index der Variablen in der Auflistung verwendet wird.  
  
 Da die Sichtbarkeit einer Variablen von oben nach unten verläuft, sind Variablen, die auf der Paketebene deklariert werden, für alle Container in dem Paket sichtbar. Die <xref:Microsoft.SqlServer.Dts.Runtime.Variables>-Auflistung in einem Container umfasst daher alle Variablen, die neben den eigenen Variablen des Containers auch zu seinem übergeordneten Element gehören.  
  
 Die Variablen, die in einem Task enthalten sind, sind dagegen hinsichtlich Bereich und Sichtbarkeit eingeschränkt und nur für den Task sichtbar.  
  
 Wenn ein Paket andere Pakete ausführt, sind die in dem Bereich des aufrufenden Pakets definierten Variablen für das aufgerufene Paket verfügbar. Die einzige Ausnahme tritt auf, wenn eine gleichnamige Variable im aufgerufenen Paket vorhanden ist. Bei diesem Konflikt wird der Wert des aufrufenden Pakets von dem Variablenwert des aufgerufenen Pakets überschrieben. Die in dem Bereich des aufgerufenen Pakets definierten Variablen sind niemals für das aufrufende Paket verfügbar.  
  
 Im folgenden Beispielcode wird eine Variable `myCustomVar` programmgesteuert in dem Paketbereich erstellt. Diese führt dann eine Iteration durch alle Variablen durch, die für das Paket sichtbar sind. Dabei wird der Name, der Datentyp und der Wert ausgedruckt.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Application app = new Application();  
      // Load a sample package that contains a variable that sets the file name.  
      Package pkg = app.LoadPackage(  
        @"C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" +  
        @"\Package Samples\CaptureDataLineage Sample\CaptureDataLineage\CaptureDataLineage.dtsx",  
        null);  
      Variables pkgVars = pkg.Variables;  
      Variable myVar = pkg.Variables.Add("myCustomVar", false, "User", "3");  
      foreach (Variable pkgVar in pkgVars)  
      {  
        Console.WriteLine("Variable: {0}, {1}, {2}", pkgVar.Name,  
          pkgVar.DataType, pkgVar.Value.ToString());  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim app As Application = New Application()  
    ' Load a sample package that contains a variable that sets the file name.  
    Dim pkg As Package = app.LoadPackage( _  
      "C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" & _  
      "\Package Samples\CaptureDataLineage Sample\CaptureDataLineage\CaptureDataLineage.dtsx", _  
      Nothing)  
    Dim pkgVars As Variables = pkg.Variables  
    Dim myVar As Variable = pkg.Variables.Add("myCustomVar", False, "User", "3")  
    Dim pkgVar As Variable  
    For Each pkgVar In pkgVars  
      Console.WriteLine("Variable: {0}, {1}, {2}", pkgVar.Name, _  
        pkgVar.DataType, pkgVar.Value.ToString())  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Beispielausgabe:**  
  
 `Variable: CancelEvent, Int32, 0`  
  
 `Variable: CreationDate, DateTime, 4/18/2003 11:57:00 AM`  
  
 `Variable: CreatorComputerName, String,`  
  
 `Variable: CreatorName, String,`  
  
 `Variable: ExecutionInstanceGUID, String, {237AB5A4-7E59-4FC9-8D61-E8F20363DF25}`  
  
 `Variable: FileName, String, Junk`  
  
 `Variable: InteractiveMode, Boolean, False`  
  
 `Variable: LocaleID, Int32, 1033`  
  
 `Variable: MachineName, String, MYCOMPUTERNAME`  
  
 `Variable: myCustomVar, String, 3`  
  
 `Variable: OfflineMode, Boolean, False`  
  
 `Variable: PackageID, String, {F0D2E396-A6A5-42AE-9467-04CE946A810C}`  
  
 `Variable: PackageName, String, DTSPackage1`  
  
 `Variable: StartTime, DateTime, 1/28/2005 7:55:39 AM`  
  
 `Variable: UserName, String, <domain>\<userid>`  
  
 `Variable: VersionBuild, Int32, 198`  
  
 `Variable: VersionComments, String,`  
  
 `Variable: VersionGUID, String, {90E105B4-B4AF-4263-9CBD-C2050C2D6148}`  
  
 `Variable: VersionMajor, Int32, 1`  
  
 `Variable: VersionMinor, Int32, 0`  
  
 Beachten Sie, dass alle im **System**-Namespace bewerteten Variablen dem Paket zur Verfügung stehen. Weitere Informationen finden Sie unter [System Variables](../../integration-services/system-variables.md).  
  
## <a name="namespaces"></a>Namespaces  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ([!INCLUDE[ssIS](../../includes/ssis-md.md)]) stellt zwei Standardnamespaces für Variablen bereit: **User** und **System**. Standardmäßig werden dem **User**-Namespace alle vom Entwickler erstellten benutzerdefinierten Variablen hinzugefügt. Systemvariablen befinden sich im **System**-Namespace. Sie können neben dem **User**-Namespace zusätzliche Namespaces für benutzerdefinierte Variablen erstellen und den Namen des **User**-Namespaces ändern. Es ist jedoch nicht möglich, Variablen im **System**-Namespace hinzuzufügen oder zu ändern oder Systemvariablen einem anderen Namespace zuzuweisen.  
  
 Die verfügbaren Systemvariablen sind je nach Containertyp unterschiedlich. Eine Liste der für Pakete, Container, Tasks und Ereignishandler verfügbaren Systemvariablen finden Sie unter [System Variables](../../integration-services/system-variables.md) (Systemvariablen).  
  
## <a name="value"></a>Wert  
 Der Wert einer benutzerdefinierten Variablen kann ein Literal oder ein Ausdruck sein:  
  
-   Wenn die Variable einen Literalwert enthalten soll, legen Sie den Wert seiner <xref:Microsoft.SqlServer.Dts.Runtime.Variable.Value%2A>-Eigenschaft fest.  
  
-   Wenn die Variable einen Ausdruck enthalten soll, sodass Sie die Ergebnisse des Ausdrucks als Wert verwenden können, legen Sie die <xref:Microsoft.SqlServer.Dts.Runtime.Variable.EvaluateAsExpression%2A>-Eigenschaft der Variablen auf **true** fest, und geben Sie einen Ausdruck in der <xref:Microsoft.SqlServer.Dts.Runtime.Variable.Expression%2A>-Eigenschaft an. Zur Laufzeit wird der Ausdruck ausgewertet, und das Ergebnis des Ausdrucks wird als Wert der Variablen verwendet. Wenn z. B. die Ausdruckseigenschaft einer Variablen`"100 * 2""100 * 2"` ist, wird die Variable auf einen Wert von 200 ausgewertet.  
  
 Für eine Variable kann der Wert ihres <xref:Microsoft.SqlServer.Dts.Runtime.Variable.DataType%2A> nicht explizit festgelegt werden. Der Wert <xref:Microsoft.SqlServer.Dts.Runtime.Variable.DataType%2A> wird aus dem Startwert, der der Variablen zugewiesen ist, abgeleitet und kann nachträglich nicht geändert werden. Weitere Informationen zu Variablendatentypen finden Sie unter [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md) (Integration Services-Datentypen).  
  
 Im folgenden Codebeispiel wird eine neue Variable erstellt, <xref:Microsoft.SqlServer.Dts.Runtime.Variable.EvaluateAsExpression%2A> auf **true** festgelegt, der Ausdruck `"100 * 2"` der Ausdruckseigenschaft der Variablen zugewiesen und dann der Wert der Variablen ausgegeben.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package pkg = new Package();  
      Variable v100 = pkg.Variables.Add("myVar", false, "", 1);  
      v100.EvaluateAsExpression = true;  
      v100.Expression = "100 * 2";  
      Console.WriteLine("Expression for myVar: {0}",   
        v100.Properties["Expression"].GetValue(v100));  
      Console.WriteLine("Value of myVar: {0}", v100.Value.ToString());  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim pkg As Package = New Package  
    Dim v100 As Variable = pkg.Variables.Add("myVar", False, "", 1)  
    v100.EvaluateAsExpression = True  
    v100.Expression = "100 * 2"  
    Console.WriteLine("Expression for myVar: {0}", _  
      v100.Properties("Expression").GetValue(v100))  
    Console.WriteLine("Value of myVar: {0}", v100.Value.ToString)  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Beispielausgabe:**  
  
 `Expression for myVar: 100 * 2`  
  
 `Value of myVar: 200`  
  
 Der Ausdruck muss ein gültiger Ausdruck sein, der die [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Ausdruckssyntax verwendet. Neben den Operatoren und Funktionen, die der Ausdruck bereitstellt, sind Literale in Variablenausdrücke zulässig, Ausdrücke können jedoch nicht auf andere Variablen oder Spalten verweisen. Weitere Informationen finden Sie unter [Integration Services-Ausdrücke &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md)ausgewertet wird.  
  
## <a name="configuration-files"></a>Konfigurationsdateien  
 Wenn eine Konfigurationsdatei eine benutzerdefinierte Variable einschließt, kann die Variable zur Laufzeit aktualisiert werden. Dies bedeutet, dass der Wert der ursprünglich in dem Paket enthaltenen Variablen beim Ausführen des Pakets durch einen neuen Wert aus der Konfigurationsdatei ersetzt wird. Diese Ersetzungstechnik ist hilfreich, wenn ein Paket für mehrere Server bereitgestellt wird, die unterschiedliche Variablenwerte erfordern. In einer Variablen kann beispielsweise angegeben werden, wie oft ein **Foreach-Schleifencontainer** seinen Workflow wiederholt, oder es können die Empfänger aufgelistet werden, an die von einem Ereignishandler eine E-Mail gesendet wird, wenn ein Fehler ausgelöst wird. In einer Variablen kann auch die Anzahl von Fehlern geändert werden, die auftreten können, bevor für das Paket ein Fehler gemeldet wird. Diese Variablen werden dynamisch in Konfigurationsdateien für jede Umgebung bereitgestellt. Daher sind nur Lese-/Schreibvariablen in Konfigurationsdateien zulässig. Weitere Informationen finden Sie unter [Erstellen von Paketkonfigurationen](../packages/legacy-package-deployment-ssis.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md)   
 [Verwenden von Variablen in Paketen](../integration-services-ssis-variables.md)  
  
