---
description: Programmgesteuertes Hinzufügen des Datenflusstasks
title: Programmgesteuertes Hinzufügen des Datenflusstasks | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- adding Data Flow task
- SSIS data flow
- data flow task [Integration Services], adding
- MainPipe object
ms.assetid: 0ca03712-a82e-4aa7-949b-f869a8936ddf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c5090cbe224a9ac3f94bff828189d3517c1dd376
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130704"
---
# <a name="adding-the-data-flow-task-programmatically"></a>Programmgesteuertes Hinzufügen des Datenflusstasks

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] umfasst einen Task namens Datenflusstask, der durch den <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper>-Namespace im Objektmodell dargestellt wird. Der Datenflusstask ist ein spezialisierter Hochleistungstask, der dem Transformieren und Verschieben von Daten bei der Paketausführung dient. Genau wie andere Tasks ist der Datenflusstask vom <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>-Objekt umschlossen, und aus Sicht der Runtime-Engine ist dieser Task nur einer der Tasks im Paket. Der Datenfluss enthält jedoch zusätzliche Objekte, die so genannten Datenflusskomponenten. Diese Komponenten bewirken, dass Daten von einer Quelle an ein Ziel verschoben werden, was manchmal durch eine Transformation erfolgt. Die Komponenten definieren sowohl die Richtung des Verschiebens als auch die Art der Datentransformation. Zum Konfigurieren des Datenflusstasks müssen dem Task Komponenten hinzugefügt und anschließend verbunden werden, damit der Datenfluss eingerichtet und die beabsichtigte Transformation erzielt wird.  
  
 In einem Datenflusstask gibt es drei Arten von Komponenten: **Datenflussquellen**, **Datenflusstransformationen** und **Datenflussziele**, die in dieser Reihenfolge in der [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer-Toolbox angezeigt werden. Diese Typen werden auch einfacher als Quellen, Transformationen oder Ziele bezeichnet. Wie die Namen nahe legen, fließen Daten von einer Quelle zu einer Transformation und dann zu einem Ziel. Dies ist eine vereinfachte Beschreibung des Datenflusses zur Veranschaulichung des Konzepts. Der Datenflusstask ist jedoch flexibel und leistungsfähig genug, um mehrere Quellen zu verarbeiten und zahlreiche Transformationen miteinander zu verbinden, die Ausgabe an mehrere Ziele senden.  
  
 Der Datenflusstask wird einem Paket auf die gleiche Weise wie andere Tasks hinzugefügt. Nachdem der Task hinzugefügt wurde, wird er durch Hinzufügen von Komponenten zum Datenflusstask sowie Konfigurieren und Verbinden von Komponenten im Task konfiguriert.  
  
## <a name="sample"></a>Beispiel  
 Im folgenden Codebeispiel wird gezeigt, wie einem Paket ein Datenflusstask hinzugefügt wird. Für dieses Beispiel ist ein Verweis auf die Microsoft.SqlServer.PipelineHost-, Microsoft.SqlServer.DTSPipelineWrap- und Microsoft.SqlServer.ManagedDTS-Assemblys erforderlich.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
      Executable e = p.Executables.Add("STOCK:PipelineTask");  
      TaskHost thMainPipe = e as TaskHost;  
      MainPipe dataFlowTask = thMainPipe.InnerObject as MainPipe;   
    }  
  }  
}  
```  
  
```vb  
Imports System.IO  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    Dim e As Executable = p.Executables.Add("STOCK:PipelineTask")  
    Dim thMainPipe As TaskHost = CType(e, TaskHost)  
    Dim dataFlowTask As MainPipe = CType(thMainPipe.InnerObject, MainPipe)  
  
  End Sub  
  
End Module  
```  
  
## <a name="external-resources"></a>Externe Ressourcen  
 Blogbeitrag, [EzAPI – Updated for SQL Server 2012 (EzAPI: für SQL Server 2012 aktualisiert)](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ezapi-updated-for-sql-server-2012/ba-p/388042), auf blogs.msdn.com.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Programmgesteuertes Auffinden von Datenflusskomponenten](../../integration-services/building-packages-programmatically/discovering-data-flow-components-programmatically.md)  
  
  
