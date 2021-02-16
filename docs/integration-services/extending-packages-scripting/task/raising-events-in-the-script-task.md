---
description: Auslösen von Ereignissen im Skripttask
title: Auslösen von Ereignissen im Skripttask | Microsoft-Dokumentation
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
- events [Integration Services], scripts
- warnings [Integration Services]
- SSIS events, scripts
- errors [Integration Services], events
- SSIS Script task, events
- Events property
- Script task [Integration Services], events
ms.assetid: 21ea07d1-e267-4fb1-a6cc-82c95a39beae
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2dddd04120c80ddd2f80151b2dba22c7079f8790
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351546"
---
# <a name="raising-events-in-the-script-task"></a>Auslösen von Ereignissen im Skripttask

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Ereignisse bieten eine Möglichkeit, Fehler, Warnungen und andere Informationen, wie z. B. den Fortschritt oder Status eines Tasks, an das entsprechende Paket zu melden. Das Paket stellt Ereignishandler zum Verwalten von Ereignisbenachrichtigungen bereit. Der Skripttask kann Ereignisse durch Aufrufen der Methoden in der <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A>-Eigenschaft des **Dts**-Objekts auslösen. Weitere Informationen zum Umgang von [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]-Paketen mit Ereignissen finden Sie unter [Integration Services-Ereignishandler &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-event-handlers.md).  
  
 Ereignisse können in jedem Protokollanbieter protokolliert werden, der im Paket aktiviert wird. Protokollanbieter speichern Informationen über Ereignisse in einem Datenspeicher. Der Skripttask kann ebenfalls die <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A>-Methode verwenden, um Informationen in einem Protokollanbieter zu protokollieren, ohne ein Ereignis auszulösen. Weitere Informationen zur Verwendung der <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A>-Methode finden Sie unter [Logging in the Script Task](../../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md) (Protokollieren im Skripttask).  
  
 Um ein Ereignis auszulösen, ruft der Skripttask eine der Methoden auf, die von der <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A>-Eigenschaft verfügbar gemacht werden. In der folgenden Tabelle werden die Methoden aufgelistet, die von der <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A>-Eigenschaft verfügbar gemacht werden.  
  
|Ereignis|BESCHREIBUNG|  
|-----------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireCustomEvent%2A>|Löst ein benutzerdefiniertes Ereignis im Paket aus.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireError%2A>|Informiert das Paket über eine Fehlerbedingung.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireInformation%2A>|Stellt Informationen für den Benutzer bereit.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireProgress%2A>|Informiert das Paket über den Fortschritt des Tasks.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireQueryCancel%2A>|Gibt einen Wert zurück, der angibt, ob das Paket erfordert, dass der Task vorzeitig geschlossen wird.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireWarning%2A>|Informiert das Paket darüber, dass der Task einen Status aufweist, der eine Benutzerbenachrichtigung erfordert, bei dem es sich jedoch nicht um eine Fehlerbedingung handelt.|  
  
## <a name="events-example"></a>Beispiel für ein Ereignis  
 Im folgenden Beispiel wird veranschaulicht, wie Ereignisse innerhalb des Skripttasks ausgelöst werden können. Im Beispiel wird eine systemeigene Windows-API-Funktion verwendet, um zu bestimmen, ob eine Internetverbindung verfügbar ist. Wenn keine Verbindung verfügbar ist, wird ein Fehler ausgelöst. Wenn eine potenziell flüchtige Modemverbindung in Gebrauch ist, wird im Beispiel eine Warnung ausgelöst. Andernfalls wird die Informationsmeldung zurückgegeben, dass eine Internetverbindung erkannt wurde.  
  
```vb  
Private Declare Function InternetGetConnectedState Lib "wininet" _  
    (ByRef dwFlags As Long, ByVal dwReserved As Long) As Long  
  
Private Enum ConnectedStates  
    LAN = &H2  
    Modem = &H1  
    Proxy = &H4  
    Offline = &H20  
    Configured = &H40  
    RasInstalled = &H10  
End Enum  
  
Public Sub Main()  
  
    Dim dwFlags As Long  
    Dim connectedState As Long  
    Dim fireAgain as Boolean  
  
    connectedState = InternetGetConnectedState(dwFlags, 0)  
  
    If connectedState <> 0 Then  
        If (dwFlags And ConnectedStates.Modem) = ConnectedStates.Modem Then  
            Dts.Events.FireWarning(0, "Script Task Example", _  
                "Volatile Internet connection detected.", String.Empty, 0)  
        Else  
            Dts.Events.FireInformation(0, "Script Task Example", _  
                "Internet connection detected.", String.Empty, 0, fireAgain)  
        End If  
    Else  
        ' If not connected to the Internet, raise an error.  
        Dts.Events.FireError(0, "Script Task Example", _  
            "Internet connection not available.", String.Empty, 0)  
    End If  
  
    Dts.TaskResult = ScriptResults.Success  
  
End Sub  
```  
  
```csharp  
using System;  
using System.Data;  
using Microsoft.SqlServer.Dts.Runtime;  
using System.Windows.Forms;  
using System.Runtime.InteropServices;  
  
public class ScriptMain  
{  
  
[DllImport("wininet")]  
        private extern static long InternetGetConnectedState(ref long dwFlags, long dwReserved);  
  
        private enum ConnectedStates  
        {  
            LAN = 0x2,  
            Modem = 0x1,  
            Proxy = 0x4,  
            Offline = 0x20,  
            Configured = 0x40,  
            RasInstalled = 0x10  
        };  
  
        public void Main()  
        {  
            //  
            long dwFlags = 0;  
            long connectedState;  
            bool fireAgain = true;  
            int state;  
  
            connectedState = InternetGetConnectedState(ref dwFlags, 0);  
            state = (int)ConnectedStates.Modem;  
            if (connectedState != 0)  
            {  
                if ((dwFlags & state) == state)  
                {  
                    Dts.Events.FireWarning(0, "Script Task Example", "Volatile Internet connection detected.", String.Empty, 0);  
                }  
                else  
                {  
                    Dts.Events.FireInformation(0, "Script Task Example", "Internet connection detected.", String.Empty, 0, ref fireAgain);  
                }  
            }  
            else  
            {  
                // If not connected to the Internet, raise an error.  
                Dts.Events.FireError(0, "Script Task Example", "Internet connection not available.", String.Empty, 0);  
            }  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
  
        }  
  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Ereignishandler &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-event-handlers.md)   
 [Hinzufügen eines Ereignishandlers zu einem Paket](../../integration-services-ssis-event-handlers.md)  
  
