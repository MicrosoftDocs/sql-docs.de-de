---
description: Erstellen eines benutzerdefinierten Foreach-Enumerators
title: Erstellen eines benutzerdefinierten Foreach-Enumerators | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom foreach enumerators [Integration Services], creating
ms.assetid: 050e8455-2ed0-4b6d-b3ea-4e80e6c28487
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ab6fe83aca3ddcbf5aad1826016bdc2f017537d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350286"
---
# <a name="creating-a-custom-foreach-enumerator"></a>Erstellen eines benutzerdefinierten Foreach-Enumerators

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Die Schritte zum Erstellen eines benutzerdefinierten Foreach-Enumerators ähneln denen jedes anderen benutzerdefinierten Objekts für [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]:  
  
-   Erstellen Sie eine neue Klasse, die von der Basisklasse erbt. Für einen Foreach-Enumerator ist <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator> die Basisklasse.  
  
-   Weisen Sie das Attribut zu, das den Typ des Objekts für die Klasse identifiziert. Für einen Foreach-Enumerator ist <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute> das Attribut.  
  
-   Überschreiben Sie die Implementierung der Methoden und Eigenschaften der Basisklasse. Für einen Foreach-Enumerator ist die <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A>-Methode die wichtigste.  
  
-   Entwickeln Sie optional eine individuelle Benutzeroberfläche. Für einen Foreach-Enumerator ist dazu eine Klasse erforderlich, die die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSForEachEnumeratorUI>-Schnittstelle implementiert.  
  
 Ein benutzerdefinierter Enumerator wird vom <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop>-Container gehostet. Zur Laufzeit ruft der <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop>-Container die <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A>-Methode des benutzerdefinierten Enumerators auf. Der benutzerdefinierte Enumerator gibt ein Objekt zurück, mit dem die **IEnumerable**-Schnittstelle implementiert wird, wie beispielsweise eine **ArrayList**. <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop> führt anschließend eine Iteration über jedes Element in der Auflistung durch, gibt den Wert des aktuellen Elements durch eine benutzerdefinierte Variable an und führt die Ablaufsteuerung im Container aus.  
  
## <a name="getting-started-with-a-custom-foreach-enumerator"></a>Erste Schritte mit einem benutzerdefinierten ForEach-Enumerator  
  
### <a name="creating-projects-and-classes"></a>Erstellen von Projekten und Klassen  
 Da alle verwalteten Foreach-Enumeratoren von der <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator>-Basisklasse abgeleitet sind, besteht der erste Schritt beim Erstellen eines benutzerdefinierten Foreach-Enumerators darin, in Ihrer bevorzugten verwalteten Programmiersprache ein Klassenbibliotheksprojekt anzulegen und eine Klasse zu generieren, die von der Basisklasse erbt. In dieser abgeleiteten Klasse überschreiben Sie die Methoden und Eigenschaften der Basisklasse, um die benutzerdefinierten Funktionen zu implementieren.  
  
 Erstellen Sie in der gleichen Lösung ein zweites Klassenbibliotheksprojekt für die individuelle Benutzeroberfläche. Für eine einfache Bereitstellung sollten Sie eine eigene Assembly für die Benutzeroberfläche verwenden, da Sie so den Foreach-Enumerator oder seine Benutzeroberfläche unabhängig aktualisieren und erneut bereitstellen können.  
  
 Konfigurieren Sie beide Projekte für das Signieren der Assemblys, die bei der Erstellung erzeugt werden, mit einer Schlüsseldatei mit starkem Namen.  
  
### <a name="applying-the-dtsforeachenumerator-attribute"></a>Anwenden des DtsForEachEnumerator-Attributs  
 Wenden Sie das <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute>-Attribut der Klasse zu, die Sie erstellt haben, um sie als Foreach-Enumerator zu kennzeichnen. Dieses Attribut stellt Entwurfszeitinformationen bereit, z. B. Name und Beschreibung des Foreach-Enumerators. Die **Name**-Eigenschaft wird in der Dropdown-Liste der verfügbaren Enumeratoren auf der Registerkarte **Auflistung** des Dialogfelds **Foreach-Schleifen-Editor** angezeigt.  
  
 Verwenden Sie die <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute.UITypeName%2A>-Eigenschaft, um den Foreach-Enumerator mit der individuellen Benutzeroberfläche zu verknüpfen. Um das für diese Eigenschaft erforderliche öffentliche Schlüsseltoken zu erhalten, können Sie **sn.exe -t** verwenden. Damit zeigen Sie das öffentliche Schlüsseltoken aus der Schlüsselpaardatei (.snk) an, die Sie für das Signieren der Benutzeroberflächenassembly verwenden möchten.  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
Namespace Microsoft.Samples.SqlServer.Dts  
    <DtsForEachEnumerator(DisplayName = "MyEnumerator", Description="A sample custom enumerator", UITypeName="FullyQualifiedTypeName,AssemblyName,Version=1.00.000.00,Culture=Neutral,PublicKeyToken=<publickeytoken>")> _   
    Public Class MyEnumerator  
     Inherits ForEachEnumerator  
        'Insert code here.  
    End Class  
End Namespace  
```  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsForEachEnumerator( DisplayName = "MyEnumerator", Description="A sample custom enumerator", UITypeName="FullyQualifiedTypeName,AssemblyName,Version=1.00.000.00,Culture=Neutral,PublicKeyToken=<publickeytoken>")]  
    public class MyEnumerator : ForEachEnumerator  
    {  
        //Insert code here.  
    }  
}  
```  
  
## <a name="building-deploying-and-debugging-a-custom-enumerator"></a>Erstellen, Bereitstellen und Debuggen eines benutzerdefinierten Enumerators  
 Die Schritte zum Erstellen, Bereitstellen und Debuggen eines benutzerdefinierten Foreach-Enumerators in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] ähneln denen für andere Arten benutzerdefinierter Objekte stark. Weitere Informationen finden Sie unter [Building, Deploying, and Debugging Custom Objects](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md) (Erstellen, Bereitstellen und Debuggen von benutzerdefinierten Objekten).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Codieren eines benutzerdefinierten Foreach-Enumerators](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/coding-a-custom-foreach-enumerator.md)   
 [Entwickeln einer Benutzeroberfläche für einen benutzerdefinierten ForEach-Enumerator](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-user-interface-for-a-custom-foreach-enumerator.md)  
  
  
