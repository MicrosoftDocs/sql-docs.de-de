---
description: Verwenden von Variablen im Skripttask
title: Verwenden von Variablen im Skripttask | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Foreach Loop containers
- Script task [Integration Services], variables
- scripts [Integration Services], variables
- Variables property
- Variable object
- SSIS Script task, variables
- variables [Integration Services], Script task
ms.assetid: 593b5961-4bfa-4ce1-9531-a251c34e89d3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d6e84b1f0293680d593a2427b9d95e72c36fedff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347820"
---
# <a name="using-variables-in-the-script-task"></a>Verwenden von Variablen im Skripttask

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Variablen ermöglichen es dem Skripttask, Daten mit anderen Objekten im Paket auszutauschen. Weitere Informationen finden Sie unter [Integration Services-Variablen &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-variables.md).  
  
 Mithilfe der <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A>-Eigenschaft des **Dts**-Objekts erhält der Skripttask Lese- und Schreibzugriff auf <xref:Microsoft.SqlServer.Dts.Runtime.Variable>-Objekte im Paket.  
  
> [!NOTE]  
>  Die <xref:Microsoft.SqlServer.Dts.Runtime.Variable.Value%2A>-Eigenschaft der <xref:Microsoft.SqlServer.Dts.Runtime.Variable>-Klasse ist vom Typ **Object**. Da für den Skripttask **Option Strict** aktiviert ist, müssen Sie die <xref:Microsoft.SqlServer.Dts.Runtime.Variable.Value%2A>-Eigenschaft vor ihrer Verwendung in den richtigen Typ umwandeln.  
  
 Um bestehende Variablen für das benutzerdefinierte Skript verfügbar zu machen, fügen Sie diese den Listen <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask.ReadOnlyVariables%2A> und <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask.ReadWriteVariables%2A> im **Skripttask-Editor** hinzu. Beachten Sie, dass bei Variablennamen nach Groß-/Kleinschreibung unterschieden wird. Innerhalb des Skripts greifen Sie auf Variablen beider Typen über die <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A>-Eigenschaft des **Dts**-Objekts zu. Mithilfe der **Value**-Eigenschaft erhalten Sie Lese- und Schreibzugriff auf einzelne Variablen. Der Skripttask verwaltet Sperren transparent, während das Skript die Werte von Variablen liest und ändert.  
  
 Mithilfe der <xref:Microsoft.SqlServer.Dts.Runtime.Variables.Contains%2A>-Methode der <xref:Microsoft.SqlServer.Dts.Runtime.Variables>-Auflistung, die von der <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A>-Eigenschaft zurückgegeben wird, können Sie das Vorhandensein von Variablen vor ihrer Verwendung im Code überprüfen.  
  
 Für die Arbeit mit Variablen im Skripttask können Sie auch die <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.VariableDispenser%2A>-Eigenschaft (Dts.VariableDispenser) verwenden. Bei der Verwendung von <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.VariableDispenser%2A> müssen Sie die Sperrsemantik und die Umwandlung von Datentypen für variable Werte in Ihrem Code berücksichtigen. Wenn Sie mit Variablen arbeiten möchten, die zur Entwurfszeit nicht zur Verfügung stehen, sondern programmgesteuert zur Laufzeit erstellt werden, müssen Sie möglicherweise auf die <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.VariableDispenser%2A>-Eigenschaft anstelle der <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A>-Eigenschaft zurückgreifen.  
  
## <a name="using-the-script-task-within-a-foreach-loop-container"></a>Verwenden des Skripttasks in einem Foreach-Schleifencontainer  
 Wenn ein Skripttask wiederholt innerhalb eines Foreach-Schleifencontainers ausgeführt wird, muss das Skript normalerweise auf den Inhalt des aktuellen Elements im Enumerator zugreifen. Bei der Verwendung eines Foreach-Dateienumerators muss das Skript beispielsweise über den aktuellen Dateinamen verfügen und bei der Verwendung eines Foreach-ADO-Enumerators über den Inhalt der Spalten in der aktuellen Datenzeile.  
  
 Variablen ermöglichen diese Kommunikation zwischen dem Foreach-Schleifencontainer und dem Skripttask. Weisen Sie jedem Datenelement, das von einem Enumerationselement zurückgegeben wird, im **Foreach-Schleifen-Editor** auf der Seite **Variablenzuordnungen** Variablen zu. Ein Foreach-Dateienumerator gibt z. B. nur am Index 0 einen Dateinamen zurück und erfordert daher nur eine Variablenzuordnung. Bei einem Enumerator, der in jeder Zeile mehrere Datenspalten zurückgibt, müssen Sie hingegen für jede Spalte, die Sie im Skripttask verwenden möchten, eine andere Variable zuordnen.  
  
 Nachdem Sie die Enumerationselemente Variablen zugeordnet haben, müssen Sie die zugeordneten Variablen im **Skripttask-Editor** auf der Seite **Skript** der **ReadOnlyVariables**-Eigenschaft hinzufügen, um Sie für das Skript zur Verfügung zu stellen. Ein Beispiel eines Skripttasks innerhalb eines Foreach-Schleifencontainers zur Verarbeitung der Bilddateien in einem Ordner finden Sie unter [Arbeiten mit Bildern mithilfe des Skripttasks](../../../integration-services/extending-packages-scripting-task-examples/working-with-images-with-the-script-task.md).  
  
## <a name="variables-example"></a>Variablenbeispiel  
 Das folgende Beispiel veranschaulicht den Zugriff auf und die Verwendung von Variablen in einem Skripttask zur Ermittlung des Pfads eines Paket-Workflows. In diesem Beispiel wird vorausgesetzt, dass die ganzzahligen Variablen `CustomerCount` sowie `MaxRecordCount` erstellt und der **ReadOnlyVariables**-Auflistung im **Skripttask-Editor** hinzugefügt wurden. Die `CustomerCount`-Variable enthält die Anzahl an Kundendatensätzen, die importiert werden sollen. Falls der Wert höher als der Wert von `MaxRecordCount` ist, gibt der Skripttask eine Fehlermeldung aus. Falls der Fehler auftritt, weil der `MaxRecordCount`-Schwellenwert überschritten wurde, kann der Fehlerpfad des Workflows alle erforderlichen Cleanup-Schritte implementieren.  
  
 Um das Beispiel erfolgreich zu kompilieren, müssen Sie einen Verweis auf die Microsoft.SqlServer.ScriptTask-Assembly hinzufügen.  
  
```vb  
Public Sub Main()  
  
    Dim customerCount As Integer  
    Dim maxRecordCount As Integer  
  
    If Dts.Variables.Contains("CustomerCount") = True AndAlso _  
        Dts.Variables.Contains("MaxRecordCount") = True Then  
  
        customerCount = _  
            CType(Dts.Variables("CustomerCount").Value, Integer)  
        maxRecordCount = _  
            CType(Dts.Variables("MaxRecordCount").Value, Integer)  
  
    End If  
  
    If customerCount > maxRecordCount Then  
            Dts.TaskResult = ScriptResults.Failure  
    Else  
            Dts.TaskResult = ScriptResults.Success  
    End If  
  
End Sub  
```  
  
```csharp  
using System;  
using System.Data;  
using Microsoft.SqlServer.Dts.Runtime;  
  
public class ScriptMain  
{  
  
    public void Main()  
    {  
        int customerCount;  
        int maxRecordCount;  
  
        if (Dts.Variables.Contains("CustomerCount")==true&&Dts.Variables.Contains("MaxRecordCount")==true)  
  
        {  
            customerCount = (int) Dts.Variables["CustomerCount"].Value;  
            maxRecordCount = (int) Dts.Variables["MaxRecordCount"].Value;  
  
        }  
  
        if (customerCount>maxRecordCount)  
        {  
            Dts.TaskResult = (int)ScriptResults.Failure;  
        }  
        else  
        {  
            Dts.TaskResult = (int)ScriptResults.Success;  
        }  
  
    }  
  
}  
  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Variablen &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-variables.md)   
 [Verwenden von Variablen in Paketen](../../integration-services-ssis-variables.md)  
  
