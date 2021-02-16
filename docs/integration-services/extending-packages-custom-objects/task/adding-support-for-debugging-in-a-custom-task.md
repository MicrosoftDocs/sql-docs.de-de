---
description: Bereitstellen von Unterstützung für das Debuggen in einem benutzerdefinierten Task
title: Bereitstellen von Unterstützung für das Debuggen in einem benutzerdefinierten Task | Microsoft-Dokumentation
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
- BreakpointManager class
- breakpoints [Integration Services]
- custom tasks [Integration Services], debugging
- IDTSSuspend interface
- IDTSBreakpointSite interface
- SSIS custom tasks, debugging
- debugging [Integration Services], custom tasks
ms.assetid: 7f06e49b-0b60-4e81-97da-d32dc248264a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 28ba35dc2aecd6c25da114493b8c6e424b77dccf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346529"
---
# <a name="adding-support-for-debugging-in-a-custom-task"></a>Bereitstellen von Unterstützung für das Debuggen in einem benutzerdefinierten Task

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Die [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]-Runtime-Engine ermöglicht das Anhalten der Ausführung von Paketen, Tasks und anderen Arten von Containern mithilfe von Breakpoints. Mit Breakpoints können Sie Überprüfungen durchführen und Fehler beheben, die verhindern, dass die Anwendung oder die Tasks korrekt ausgeführt werden. Die Breakpointarchitektur ermöglicht es dem Client, den Laufzeitwert von Objekten im Paket an definierten Ausführungpunkten auszuwerten, während die Verarbeitung des Tasks angehalten ist.  
  
 Entwickler benutzerdefinierter Tasks können mit dieser Architektur über die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite>-Schnittstelle und ihre übergeordnete Schnittstelle, <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend>, benutzerdefinierte Breakpointziele erstellen. Die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite>-Schnittstelle definiert die Interaktion zwischen der Runtime-Engine und dem Task zur Erstellung und Verwaltung benutzerdefinierter Breakpointorte oder -ziele. Die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend>-Schnittstelle bietet Methoden und Eigenschaften, die von der Runtime-Engine aufgerufen werden, um den Task anzuhalten oder seine Ausführung fortzusetzen.  
  
 Breakpointorte oder -ziele sind Punkte in der Taskausführung, an denen die Verarbeitung angehalten werden kann. Benutzer können im Dialogfeld **Breakpoints festlegen** unter den verfügbaren Breakpointzielen auswählen. Zusätzlich zu den standardmäßigen Breakpointoptionen bietet beispielsweise der Foreach-Schleifencontainer die Option Am Anfang jeder Iteration der Schleife unterbrechen.  
  
 Wenn ein Task während der Ausführung ein Breakpointziel erreicht, wertet er dieses aus, um festzustellen, ob ein Breakpoint aktiviert ist. Damit wird angezeigt, dass der Benutzer die Ausführung an diesem Breakpoint beenden möchte. Wenn der Breakpoint aktiviert ist, löst der Task das <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A>-Ereignis bei der Runtime-Engine aus. Die Runtime-Engine reagiert auf das Ereignis durch Aufruf der **Suspend**-Methode für alle Tasks, die derzeit im Paket ausgeführt werden. Die Ausführung des Tasks wird fortgesetzt, wenn die Laufzeit die **ResumeExecution**-Methode des angehaltenen Tasks aufruft.  
  
 Tasks, die keine Breakpoints verwenden, sollten dennoch die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite>- und <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend>-Schnittstellen implementieren. Dadurch wird sichergestellt, dass der Task ordnungsgemäß angehalten wird, wenn andere Objekte im Paket <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A>-Ereignisse auslösen.  
  
## <a name="idtsbreakpointsite-interface-and-breakpointmanager"></a>IDTSBreakpointSite-Schnittstelle und BreakpointManager  
 Tasks erstellen Breakpointziele durch Aufruf der <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager.CreateBreakpointTarget%2A>-Methode des <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager>, indem sie eine ganzzahlige ID und eine Zeichenfolgenbeschreibung als Parameter bereitstellen. Wenn ein Task den Punkt im Code erreicht, der ein Breakpointziel enthält, überprüft er mithilfe der <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager.IsBreakpointTargetEnabled%2A>-Methode, ob der Breakpoint aktiviert ist. Falls der Wert **true** lautet, benachrichtigt der Task die Runtime-Engine, indem er das <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A>-Ereignis auslöst.  
  
 Die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite>-Schnittstelle definiert eine Methode, <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite.AcceptBreakpointManager%2A>, die von der Runtime-Engine während der Taskerstellung aufgerufen wird. Diese Methode stellt als Parameter das <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager>-Objekt bereit, das anschließend vom Task zur Erstellung und Verwaltung der Breakpoints verwendet wird. Tasks sollten den <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager> zur Verwendung während der **Validate**- und **Execute**-Methoden lokal speichern.  
  
 Im folgenden Codebeispiel wird das Erstellen eines Breakpointziels mithilfe des <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager> veranschaulicht. Im Beispiel wird die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A>-Methode aufgerufen, um das Ereignis auszulösen.  
  
```csharp  
public void AcceptBreakpointManager( BreakpointManager breakPointManager )  
{  
   //   Store the breakpoint manager locally.  
   this.bpm  = breakPointManager;  
}  
public override DTSExecResult Execute( Connections connections,  
  Variables variables, IDTSComponentEvents events,  
  IDTSLogging log, DtsTransaction txn)  
{  
   //   Create a breakpoint.  
   this.bpm.CreateBreakPointTarget( 1 , "A sample breakpoint target." );  
...  
   if( this.bpm.IsBreakpointTargetEnabled( 1 ) == true )  
      events.OnBreakpointHit( this.bpm.GetBreakpointTarget( 1 ) );  
}  
```  
  
```vb  
Public Sub AcceptBreakpointManager(ByVal breakPointManager As BreakpointManager)  
  
   ' Store the breakpoint manager locally.  
   Me.bpm  = breakPointManager  
  
End Sub  
  
Public Overrides Function Execute(ByVal connections As Connections, _  
  ByVal variables As Variables, ByVal events As IDTSComponentEvents, _  
  ByVal log As IDTSLogging, ByVal txn As DtsTransaction) As DTSExecResult  
  
   ' Create a breakpoint.  
   Me.bpm.CreateBreakPointTarget(1 , "A sample breakpoint target.")  
  
   If Me.bpm.IsBreakpointTargetEnabled(1) = True Then  
      events.OnBreakpointHit(Me.bpm.GetBreakpointTarget(1))  
   End If  
  
End Function  
```  
  
## <a name="idtssuspend-interface"></a>IDTSSuspend-Schnittstelle  
 Die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend>-Schnittstelle definiert die Methoden, die von der Runtime-Engine aufgerufen werden, um den Task anzuhalten oder seine Ausführung fortzusetzen. Die <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend>-Schnittstelle wird von der <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite>-Schnittstelle implementiert, und ihre **Suspend**- und **ResumeExecution**-Methoden werden meist durch den benutzerdefinierten Task überschrieben. Wenn die Runtime-Engine ein **OnBreakpointHit**-Ereignis von einem Task empfängt, ruft es die **Suspend**-Methode für alle derzeit ausgeführten Tasks auf und veranlasst sie, die Ausführung anzuhalten. Wenn der Client die Ausführung fortsetzt, ruft die Runtime-Engine die **ResumeExecution**-Methode der angehaltenen Tasks auf.  
  
 Das Anhalten und Fortsetzen der Taskausführung schließt das Anhalten und Fortsetzen des Ausführungsthreads des Tasks ein. In verwaltetem Code erreichen Sie dies mit der **ManualResetEvent**-Klasse im **System.Threading**-Namespace von .NET Framework.  
  
 Im folgenden Codebeispiel wird die Unterbrechung und Wiederaufnahme der Taskausführung veranschaulicht. Beachten Sie dabei, dass sich die **Execute**-Methode gegenüber dem letzten Codebeispiel geändert hat und der Ausführungsthread angehalten wird, wenn der Breakpoint ausgelöst wird.  
  
```csharp  
private ManualResetEvent m_suspended = new ManualResetEvent( true );  
private ManualResetEvent m_canExecute = new ManualResetEvent( true );  
private int   m_suspendRequired = 0;  
private int   m_debugMode = 0;  
  
public override DTSExecResult Execute( Connections connections, Variables variables, IDTSComponentEvents events, IDTSLogging log, DtsTransaction txn)  
{  
   // While a task is not executing, it is suspended.    
   // Now that we are executing,  
   // change to not suspended.  
   ChangeEvent(m_suspended, false);  
  
   // Check for a suspend before doing any work,   
   // in case the suspend and execute calls  
   // were initiated at virtually the same time.  
   CheckAndSuspend();  
   CheckAndFireBreakpoint( componentEvents, 1);  
}  
private void CheckAndSuspend()  
{  
   // Loop until we can execute.    
   // The loop is required rather than a simple If  
   // because there is a time between the return from WaitOne and the  
   // reset that we might receive another Suspend call.    
   // Suspend() will see that we are suspended  
   // and return.  So we need to rewait.  
   while (!m_canExecute.WaitOne(0, false))  
   {  
      ChangeEvent(m_suspended, true);  
      m_canExecute.WaitOne();  
      ChangeEvent(m_suspended, false);  
   }  
}  
private void CheckAndFireBreakpoint(IDTSComponentEvents events, int breakpointID)  
{  
   // If the breakpoint is enabled, fire it.  
   if (m_debugMode != 0 &&    this.bpm.IsBreakpointTargetEnabled(breakpointID))  
   {  
      //   Enter a suspend mode before firing the breakpoint.    
      //   Firing the breakpoint will cause the runtime   
      //   to call Suspend on this task.    
      //   Because we are blocked on the breakpoint,   
      //   we are suspended.  
      ChangeEvent(m_suspended, true);  
      events.OnBreakpointHit(this.bpm.GetBreakpointTarget(breakpointID));  
      ChangeEvent(m_suspended, false);  
   }  
   // Check for a suspension for two reasons:   
   //   1. If we are at a point where we could fire a breakpoint,   
   //      we are at a valid suspend point.  Even if we didn't hit a  
   //      breakpoint, the runtime may have called suspend,   
   //      so check for it.       
   //   2. Between the return from OnBreakpointHit   
   //      and the reset of the event, it is possible to have  
   //      received a suspend call from which we returned because   
   //      we were already suspended.  We need to be sure it is okay  
   //      to continue executing now.  
   CheckAndSuspend();  
}  
static void ChangeEvent(ManualResetEvent e, bool shouldSet)  
{  
   bool succeeded;  
   if (shouldSet)  
      succeeded = e.Set();  
   else  
      succeeded = e.Reset();  
  
   if (!succeeded)  
      throw new Exception("Synchronization object failed.");  
  
}  
public bool SuspendRequired  
{  
   get   {return m_suspendRequired != 0;}  
   set  
   {  
      // This lock is also taken by Suspend().    
      // Because it is possible for the package to be  
      // suspended and resumed in quick succession,   
      // this property might be set before  
      // the actual Suspend() call.    
      // Without the lock, the Suspend() might reset the canExecute  
      // event after we set it to abort the suspension.  
      lock (this)  
      {  
         Interlocked.Exchange(ref m_suspendRequired, value ? 1 : 0);  
         if (!value)  
            ResumeExecution();  
      }  
   }  
}  
public void ResumeExecution()  
{  
   ChangeEvent( m_canExecute,true );  
}  
public void Suspend()  
{  
   // This lock is also taken by the set SuspendRequired method.    
   // It prevents this call from overriding an   
   // aborted suspension.  See comments in set SuspendRequired.  
   lock (this)  
   {  
      // If a Suspend is required, do it.  
      if (m_suspendRequired != 0)  
         ChangeEvent(m_canExecute, false);  
   }  
   // We can't return from Suspend until the task is "suspended".  
   // This can happen one of two ways:   
   // the m_suspended event occurs, indicating that the execute thread  
   // has suspended, or the canExecute flag is set,   
   // indicating that a suspend is no longer required.  
   WaitHandle [] suspendOperationComplete = {m_suspended, m_canExecute};  
   WaitHandle.WaitAny(suspendOperationComplete);  
}  
```  
  
```vb  
Private m_suspended As ManualResetEvent = New ManualResetEvent(True)  
Private m_canExecute As ManualResetEvent = New ManualResetEvent(True)  
Private m_suspendRequired As Integer = 0  
Private m_debugMode As Integer = 0  
  
Public Overrides Function Execute(ByVal connections As Connections, _  
ByVal variables As Variables, ByVal events As IDTSComponentEvents, _  
ByVal log As IDTSLogging, ByVal txn As DtsTransaction) As DTSExecResult  
  
   ' While a task is not executing it is suspended.    
   ' Now that we are executing,  
   ' change to not suspended.  
   ChangeEvent(m_suspended, False)  
  
   ' Check for a suspend before doing any work,   
   ' in case the suspend and execute calls  
   ' were initiated at virtually the same time.  
   CheckAndSuspend()  
   CheckAndFireBreakpoint(componentEvents, 1)  
  
End Function  
  
Private Sub CheckAndSuspend()  
  
   ' Loop until we can execute.    
   ' The loop is required rather than a simple if  
   ' because there is a time between the return from WaitOne and the  
   ' reset that we might receive another Suspend call.    
   ' Suspend() will see that we are suspended  
   ' and return.  So we need to rewait.  
   Do While Not m_canExecute.WaitOne(0, False)  
              ChangeEvent(m_suspended, True)  
              m_canExecute.WaitOne()  
              ChangeEvent(m_suspended, False)  
   Loop  
  
End Sub  
  
Private Sub CheckAndFireBreakpoint(ByVal events As IDTSComponentEvents, _  
ByVal breakpointID As Integer)  
  
   ' If the breakpoint is enabled, fire it.  
   If m_debugMode <> 0 AndAlso Me.bpm.IsBreakpointTargetEnabled(breakpointID) Then  
              '   Enter a suspend mode before firing the breakpoint.    
              '   Firing the breakpoint will cause the runtime   
              '   to call Suspend on this task.    
              '   Because we are blocked on the breakpoint,   
              '   we are suspended.  
              ChangeEvent(m_suspended, True)  
              events.OnBreakpointHit(Me.bpm.GetBreakpointTarget(breakpointID))  
              ChangeEvent(m_suspended, False)  
   End If  
  
   ' Check for a suspension for two reasons:   
   '   1. If we are at a point where we could fire a breakpoint,   
   '         we are at a valid suspend point.  Even if we didn't hit a  
   '         breakpoint, the runtime may have called suspend,   
   '         so check for it.     
   '   2. Between the return from OnBreakpointHit   
   '         and the reset of the event, it is possible to have  
   '         received a suspend call from which we returned because   
   '         we were already suspended.  We need to be sure it is okay  
   '         to continue executing now.  
   CheckAndSuspend()  
  
End Sub  
  
Shared Sub ChangeEvent(ByVal e As ManualResetEvent, ByVal shouldSet As Boolean)  
  
   Dim succeeded As Boolean  
   If shouldSet Then  
              succeeded = e.Set()  
   Else  
              succeeded = e.Reset()  
   End If  
  
   If (Not succeeded) Then  
              Throw New Exception("Synchronization object failed.")  
   End If  
  
End Sub  
  
Public Property SuspendRequired() As Boolean  
   Get  
               Return m_suspendRequired <> 0  
  End Get  
  Set  
    ' This lock is also taken by Suspend().    
     '   Because it is possible for the package to be  
     '   suspended and resumed in quick succession,   
     '   this property might be set before  
     '   the actual Suspend() call.    
     '   Without the lock, the Suspend() might reset the canExecute  
     '   event after we set it to abort the suspension.  
              SyncLock Me  
                         Interlocked.Exchange(m_suspendRequired,IIf(Value, 1, 0))  
                         If (Not Value) Then  
                                    ResumeExecution()  
                         End If  
              End SyncLock  
   End Set  
End Property  
  
Public Sub ResumeExecution()  
   ChangeEvent(m_canExecute,True)  
End Sub  
  
Public Sub Suspend()  
  
   ' This lock is also taken by the set SuspendRequired method.    
   ' It prevents this call from overriding an   
   ' aborted suspension.  See comments in set SuspendRequired.  
   SyncLock Me  
   ' If a Suspend is required, do it.  
              If m_suspendRequired <> 0 Then  
                         ChangeEvent(m_canExecute, False)  
              End If  
   End SyncLock  
   ' We can't return from Suspend until the task is "suspended".  
   ' This can happen one of two ways:   
   ' the m_suspended event occurs, indicating that the execute thread  
   ' has suspended, or the canExecute flag is set,   
   ' indicating that a suspend is no longer required.  
   Dim suspendOperationComplete As WaitHandle() = {m_suspended, m_canExecute}  
   WaitHandle.WaitAny(suspendOperationComplete)  
  
End Sub  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Debuggen der Ablaufsteuerung](../../../integration-services/troubleshooting/debugging-control-flow.md)  
  
  
