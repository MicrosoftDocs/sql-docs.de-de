---
description: Senden mit dem Skripttask an eine private Remotemeldungswarteschlange
title: Senden mit dem Skripttask an eine private Remotemeldungswarteschlange | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Script task [Integration Services], remote private message queues
- Message Queue task [Integration Services]
- Script task [Integration Services], examples
ms.assetid: 636314fd-d099-45cd-8bb4-f730d0a06739
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5bc25b37bb4dfac2aa03795972edb56eb406aebd
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "88430342"
---
# <a name="sending-to-a-remote-private-message-queue-with-the-script-task"></a>Senden mit dem Skripttask an eine private Remotemeldungswarteschlange

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Message Queuing (auch als MSMQ bezeichnet) bietet Entwicklern eine einfache Möglichkeit, durch das Senden und Empfangen von Meldungen schnell und zuverlässig mit Anwendungshilfsprogrammen zu kommunizieren. Meldungswarteschlangen können sich auf einem lokalen oder einem Remotecomputer befinden und öffentlich oder privat sein. In [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] unterstützen der MSMQ-Verbindungs-Manager und der Task Nachrichtenwarteschlange das Senden an eine private Warteschlange auf einem Remotecomputer nicht. Mit dem Skripttask können Meldungen jedoch ganz einfach an eine private Remotewarteschlange gesendet werden.  
  
> [!NOTE]  
>  Wenn Sie einen Task erstellen möchten, den Sie einfacher in mehreren Paketen wiederverwenden können, empfiehlt es sich, den Code in diesem Skripttaskbeispiel als Ausgangspunkt für einen benutzerdefinierten Task zu verwenden. Weitere Informationen finden Sie unter [Entwickeln eines benutzerdefinierten Tasks](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md).  
  
## <a name="description"></a>BESCHREIBUNG  
 Im folgenden Beispiel werden ein vorhandener MSMQ-Verbindungs-Manager sowie Objekte und Methoden des System.Messaging-Namespace verwendet, um in einer Paketvariablen enthaltenen Text an eine private Remotemeldungswarteschlange zu senden. Der Aufruf der M:Microsoft.SqlServer.Dts.ManagedConnections.MSMQConn.AcquireConnection(System.Object)-Methode des MSMQ-Verbindungs-Managers gibt ein **MessageQueue**-Objekt zurück, dessen **Send**-Methode diese Aufgabe erfüllt.  
  
#### <a name="to-configure-this-script-task-example"></a>So konfigurieren Sie dieses Skripttaskbeispiel  
  
1.  Erstellen Sie einen MSMQ-Verbindungs-Manager mit dem Standardnamen. Geben Sie den Pfad einer gültigen privaten Remotewarteschlange im folgenden Format an:  
  
    ```  
    FORMATNAME:DIRECT=OS:<computername>\private$\<queuename>  
    ```  
  
2.  Erstellen Sie eine [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Variable namens **MessageText** des Typs **String**, um den Meldungstext an das Skript zu übergeben. Geben Sie eine Standardmeldung als Wert der Variablen ein.  
  
3.  Fügen Sie der Entwurfsoberfläche einen Skripttask hinzu, und bearbeiten Sie ihn. Um die Variable im Skript verfügbar zu machen, fügen Sie die `MessageText`-Variable im **Scripttask-Editor** auf der Registerkarte **Skript** der **ReadOnlyVariables**-Eigenschaft hinzu.  
  
4.  Klicken Sie auf **Skript bearbeiten**, um den [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]-Tools für Anwendungen-Skript-Editor (VSTA) zu öffnen.  
  
5.  Fügen Sie dem **System.Messaging**-Namespace einen Verweis im Skriptprojekt hinzu.  
  
6.  Ersetzen Sie den Inhalt des Skriptfensters durch den Code im folgenden Abschnitt.  
  
## <a name="code"></a>Code  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports System.Messaging  
  
Public Class ScriptMain  
  
    Public Sub Main()  
  
        Dim remotePrivateQueue As MessageQueue  
        Dim messageText As String  
  
        remotePrivateQueue = _  
            DirectCast(Dts.Connections("Message Queue Connection Manager").AcquireConnection(Dts.Transaction), _  
            MessageQueue)  
        messageText = DirectCast(Dts.Variables("MessageText").Value, String)  
        remotePrivateQueue.Send(messageText)  
  
        Dts.TaskResult = ScriptResults.Success  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using System.Messaging;  
  
public class ScriptMain  
{  
  
    public void Main()  
        {  
  
            MessageQueue remotePrivateQueue = new MessageQueue();  
            string messageText;  
  
            remotePrivateQueue = (MessageQueue)(Dts.Connections["Message Queue Connection Manager"].AcquireConnection(Dts.Transaction) as MessageQueue);  
            messageText = (string)(Dts.Variables["MessageText"].Value);  
            remotePrivateQueue.Send(messageText);  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
  
        }  
  
}  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Nachrichtenwarteschlange (Task)](../../integration-services/control-flow/message-queue-task.md)  
  
  
