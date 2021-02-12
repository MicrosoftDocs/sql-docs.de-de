---
description: ADO-Run-Time Fehler
title: ADO-Fehler | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO]
ms.assetid: 9bb84114-a1df-4122-a1b8-ad98dcd85cc3
author: rothja
ms.author: jroth
ms.openlocfilehash: bed9958455af424c4f18cb0f6051eb30e2efaa3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033390"
---
# <a name="ado-run-time-errors"></a>ADO-Run-Time Fehler
ADO-Fehler werden dem Programm als Laufzeitfehler gemeldet. Sie können den fehlerabfang Mechanismus ihrer Programmiersprache verwenden, um Sie abzufangen und zu behandeln. Verwenden Sie beispielsweise in Visual Basic die **On Error** -Anweisung. In Visual C++ hängt dies von der Methode ab, die Sie für den Zugriff auf die ADO-Bibliotheken verwenden. Verwenden Sie #Import einen **try-catch-** Block. Andernfalls müssen C++-Programmierer das Fehler Objekt explizit abrufen, indem Sie **GetErrorInfo** aufrufen. Die folgende Visual Basic unter Prozedur veranschaulicht das Abfangen eines ADO-Fehlers:

```
' BeginErrorHandlingVB01
Private Sub Form_Load()
' Turn on error handling
On Error GoTo FormLoadError

'Open the database and the recordset for processing.
'
Dim strCnn As String
strCnn = "Provider=sqloledb;" & _
    "Data Source=a-iresmi2000;" & _
    "Initial Catalog=Northwind;Integrated Security=SSPI"

' cnn is a Public Connection Object because
' it was defined WithEvents
Set cnn = New ADODB.Connection
cnn.Open strCnn

' The next line of code intentionally causes
' an error by trying to open a connection
' that has already been opened.
cnn.Open strCnn

' rst is a Public Recordset because it
' was defined WithEvents
Set rst = New ADODB.Recordset
rst.Open "Customers", cnn

Exit Sub

' Error handler
FormLoadError:
    Dim strErr As String
    Select Case Err
        Case adErrObjectOpen
            strErr = "Error #" & Err.Number & ": " & Err.Description & vbCrLf
            strErr = strErr & "Error reported by: " & Err.Source & vbCrLf
            strErr = strErr & "Help File: " & Err.HelpFile & vbCrLf
            strErr = strErr & "Topic ID: " & Err.HelpContext
            MsgBox strErr
            Debug.Print strErr
            Err.Clear
            Resume Next
        ' If some other error occurs that
        ' has nothing to do with ADO, show
        ' the number and description and exit.
        Case Else
            strErr = "Error #" & Err.Number & ": " & Err.Description & vbCrLf
            MsgBox strErr
            Debug.Print strErr
            Unload Me
    End Select
End Sub
' EndErrorHandlingVB01
```

 Diese **Form_Load** Ereignis Prozedur erstellt absichtlich einen Fehler, indem versucht wird, dasselbe **Verbindungs** Objekt zweimal zu öffnen. Wenn die **Open** -Methode zum zweiten Mal aufgerufen wird, wird der Fehlerhandler aktiviert. In diesem Fall ist der Fehler vom Typ **aderrobjectopen**, sodass der Fehlerhandler vor dem Fortsetzen der Programmausführung die folgende Meldung anzeigt:

```
Error #3705: Operation is not allowed when the object is open.
Error reported by: ADODB.Connection
Help File: E:\WINNT\HELP\ADO260.CHM Topic ID: 1003705
```

 Die Fehlermeldung enthält alle Informationen, die vom Visual Basic **Err** -Objekt bereitgestellt werden, mit Ausnahme des **LastDllError** -Werts, der hier nicht gilt. Die Fehlernummer gibt Aufschluss darüber, welcher Fehler aufgetreten ist. Die Beschreibung ist in Fällen nützlich, in denen Sie den Fehler nicht selbst behandeln möchten. Sie können Sie einfach an den Benutzer übergeben. Obwohl Sie normalerweise Nachrichten verwenden möchten, die für Ihre Anwendung angepasst werden, können Sie nicht jeden Fehler vorhersehen. die Beschreibung gibt Aufschluss darüber, was schief gelaufen ist. Im Beispielcode wurde der Fehler vom **Verbindungs** Objekt gemeldet. Hier wird der Objekttyp oder die programmgesteuerte ID angezeigt, kein Variablenname.

> [!NOTE]
>  Das Visual Basic **Err** -Objekt enthält nur Informationen zum letzten Fehler. Die ADO **Errors** -Auflistung des **Connection** -Objekts enthält ein **Error** -Objekt für jeden Fehler, der durch den letzten ADO-Vorgang ausgelöst wird. Verwenden Sie die **Errors** -Auflistung anstelle des **Err** -Objekts, um mehrere Fehler zu behandeln. Weitere Informationen zur **Fehler** Sammlung finden Sie unter [Anbieter Fehler](./provider-errors.md). Wenn jedoch kein gültiges **Verbindungs** Objekt vorhanden ist, ist das **Err** -Objekt die einzige Quelle für Informationen zu ADO-Fehlern.

 Welche Arten von Vorgängen verursachen wahrscheinlich ADO-Fehler? Häufige ADO-Fehler können das Öffnen eines Objekts sein, z. b. eine **Verbindung** oder ein **Recordset**, das Aktualisieren von Daten oder das Aufrufen einer Methode oder Eigenschaft, die nicht vom Anbieter unterstützt wird.

 OLE DB Fehler können auch als Laufzeitfehler in der **Fehler** Auflistung an die Anwendung übermittelt werden.

 Im folgenden Thema finden Sie weitere Informationen zu ADO-Fehlern.

-   [ADO-Fehlerreferenz](./ado-error-reference.md)