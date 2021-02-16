---
description: Protokollieren im Skripttask
title: Protokollieren im Skripttask | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- SQL Server Integration Services packages, logs
- SSIS packages, logs
- logs [Integration Services], scripts
- Integration Services packages, logs
- Log method
- SSIS Script task, logs
- Script task [Integration Services], logs
- packages [Integration Services], logs
ms.assetid: 2e11fc15-df18-4309-bd2d-fc58aa4b9b7a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2ea76691d3fb5cf6f3fcdd4ba5043d9af8b0484b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351551"
---
# <a name="logging-in-the-script-task"></a>Protokollieren im Skripttask

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Durch Verwendung der Protokollierung in [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]-Paketen können Sie detaillierte Informationen über Status, Ergebnisse und Probleme der Ausführung aufzeichnen, indem Sie vordefinierte Ereignisse bzw. benutzerdefinierte Meldungen für die spätere Analyse erfassen. Der Skripttask kann die <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A>-Methode des **Dts**-Objekts verwenden, um benutzerdefinierte Daten zu protokollieren. Wenn die Protokollierung aktiviert ist und im Dialogfeld **SSIS-Protokolle konfigurieren** auf der Registerkarte **Details** das **ScriptTaskLogEntry**-Ereignis für die Protokollierung ausgewählt ist, dann speichert ein einzelner Aufruf der <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A>-Methode die Ereignisinformationen in allen Protokollanbietern, die für den Task konfiguriert wurden.  
  
> [!NOTE]  
>  Obwohl Sie Protokollierungen direkt vom Skripttask aus ausführen können, ist ggf. eine Implementierung von Ereignissen einer Protokollierung vorzuziehen. Bei der Verwendung von Ereignissen können Sie nicht nur die Protokollierung von Ereignismeldungen aktivieren, sondern auch mit standardmäßigen oder benutzerdefinierten Ereignishandlern auf das Ereignis reagieren.  
  
 Weitere Informationen zur Protokollierung finden Sie unter [Integration Services-Protokollierung &#40;SSIS&#41;](../../../integration-services/performance/integration-services-ssis-logging.md).  
  
## <a name="logging-example"></a>Beispiel für die Protokollierung  
 Im folgenden Beispiel wird die Protokollierung vom Skripttask aus durch Protokollierung eines Werts, der die Anzahl der verarbeiteten Zeilen darstellt, gezeigt.  
  
```vb  
Public Sub Main()  
  
    Dim rowsProcessed As Integer = 100  
    Dim emptyBytes(0) As Byte  
  
    Try  
        Dts.Log("Rows processed: " & rowsProcessed.ToString, _  
            0, _  
            emptyBytes)  
        Dts.TaskResult = ScriptResults.Success  
    Catch ex As Exception  
        'An error occurred.  
        Dts.Events.FireError(0, "Script Task Example", _  
            ex.Message & ControlChars.CrLf & ex.StackTrace, _  
            String.Empty, 0)  
        Dts.TaskResult = ScriptResults.Failure  
    End Try  
  
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
            //  
            int rowsProcessed = 100;  
            byte[] emptyBytes = new byte[0];  
  
            try  
            {  
                Dts.Log("Rows processed: " + rowsProcessed.ToString(), 0, emptyBytes);  
                Dts.TaskResult = (int)ScriptResults.Success;  
            }  
            catch (Exception ex)  
            {  
                //An error occurred.  
                Dts.Events.FireError(0, "Script Task Example", ex.Message + "\r" + ex.StackTrace, String.Empty, 0);  
                Dts.TaskResult = (int)ScriptResults.Failure;  
            }  
  
        }  
```  
  
 }  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Protokollierung &#40;SSIS&#41;](../../../integration-services/performance/integration-services-ssis-logging.md)  
  
  
