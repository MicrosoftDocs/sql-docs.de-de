---
description: Senden einer HTML-E-Mail mit dem Skripttask
title: Senden einer HTML-E-Mail mit dem Skripttask | Microsoft-Dokumentation
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
- Send Mail task [Integration Services]
- Script task [Integration Services], examples
- Script task [Integration Services], HTML mail message
ms.assetid: dd2b1eef-b04f-4946-87ab-7bc56bb525ce
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 964c9b74e530d1662608a9ed8043a4e4a89fee11
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344832"
---
# <a name="sending-an-html-mail-message-with-the-script-task"></a>Senden einer HTML-E-Mail mit dem Skripttask

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Der Task „Mail senden“ in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] unterstützt nur E-Mails im Nur-Text-Format. Sie können jedoch problemlos HTML-E-Mails mithilfe des Skripttasks und den E-Mail-Funktionen des .NET Framework senden.  
  
> [!NOTE]  
>  Wenn Sie einen Task erstellen möchten, den Sie einfacher in mehreren Paketen wiederverwenden können, empfiehlt es sich, den Code in diesem Skripttaskbeispiel als Ausgangspunkt für einen benutzerdefinierten Task zu verwenden. Weitere Informationen finden Sie unter [Entwickeln eines benutzerdefinierten Tasks](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md).  
  
## <a name="description"></a>BESCHREIBUNG  
 Im folgenden Beispiel wird der **System.Net.Mail**-Namespace verwendet, um eine HTML-E-Mail zu konfigurieren und zu senden. Das Skript ruft den Empfänger, den Absender, den Betreff und den Text der E-Mail aus Paketvariablen ab, verwendet diese zur Erstellung einer neuen **MailMessage** und legt deren **IsBodyHtml**-Eigenschaft auf **True** fest. Es ruft dann den SMTP-Servernamen aus einer weiteren Paketvariablen ab, initialisiert eine Instanz von **System.Net.Mail.SmtpClient** und ruft die **Send**-Methode zum Senden der HTML-Nachricht auf. Im Beispiel wird die Funktionalität zum Senden der Nachricht in eine Unterroutine gekapselt, die in anderen Skripts wiederverwendet werden kann.  
  
#### <a name="to-configure-this-script-task-example-without-an-smtp-connection-manager"></a>So konfigurieren Sie dieses Skripttaskbeispiel ohne einen SMTP-Verbindungs-Manager  
  
1.  Erstellen Sie Zeichenfolgenvariablen mit den Namen `HtmlEmailTo`, `HtmlEmailFrom` und `HtmlEmailSubject`, und weisen Sie diesen entsprechende Werte für eine gültige Testnachricht zu.  
  
2.  Erstellen Sie eine Zeichenfolgenvariable mit dem Namen `HtmlEmailBody`, und weisen Sie dieser eine Zeichenfolge eines HTML-Markups zu. Beispiel:  
  
    ```  
    <html><body><h1>Testing</h1><p>This is a <b>test</b> message.</p></body></html>  
    ```  
  
3.  Erstellen Sie eine Zeichenfolgenvariable mit dem Namen `HtmlEmailServer`, und weisen Sie den Namen eines verfügbaren SMTP-Servers zu, der anonyme ausgehende Nachrichten akzeptiert.  
  
4.  Weisen Sie alle fünf Variablen der **ReadOnlyVariables**-Eigenschaft eines neuen Skripttasks zu.  
  
5.  Importieren Sie die Namespaces **System.Net** und **System.Net.Mail** in Ihren Code.  
  
 In dem Beispielcode in diesem Thema wird der Name des SMTP-Servers aus einer Paketvariablen abgerufen. Sie könnten jedoch auch einen SMTP-Verbindungs-Manager nutzen, um die Verbindungsinformationen zu kapseln, und den Servernamen aus dem Verbindungs-Manager in Ihrem Code extrahieren. Die <xref:Microsoft.SqlServer.Dts.ManagedConnections.SMTPConn.AcquireConnection%2A>-Methode des SMTP-Verbindungs-Managers gibt eine Zeichenfolge im folgenden Format zurück:  
  
 `SmtpServer=smtphost;UseWindowsAuthentication=False;EnableSsl=False;`  
  
 Sie können die **String.Split**-Methode verwenden, um diese Argumentliste bei jedem Semikolon (;) oder Gleichheitszeichen (=) in ein Array einzelner Zeichenfolgen zu trennen, und das zweite Argument (Subskript 1) dann als Servernamen aus dem Array extrahieren.  
  
#### <a name="to-configure-this-script-task-example-with-an-smtp-connection-manager"></a>So konfigurieren Sie dieses Skripttaskbeispiel mit einem SMTP-Verbindungs-Manager  
  
1.  Ändern Sie den zuvor konfigurierten Skripttask, indem Sie die `HtmlEmailServer`-Variable aus der Liste von **ReadOnlyVariables**-Variablen entfernen.  
  
2.  Ersetzen Sie die folgende Codezeile, die den Servernamen abruft:  
  
    ```  
    Dim smtpServer As String = _  
      Dts.Variables("HtmlEmailServer").Value.ToString  
    ```  
  
     durch die folgenden Zeilen:  
  
    ```  
    Dim smtpConnectionString As String = _  
      DirectCast(Dts.Connections("SMTP Connection Manager").AcquireConnection(Dts.Transaction), String)  
    Dim smtpServer As String = _  
      smtpConnectionString.Split(New Char() {"="c, ";"c})(1)  
    ```  
  
## <a name="code"></a>Code  
  
```vb  
Public Sub Main()  
  
  Dim htmlMessageFrom As String = _  
    Dts.Variables("HtmlEmailFrom").Value.ToString  
  Dim htmlMessageTo As String = _  
    Dts.Variables("HtmlEmailTo").Value.ToString  
  Dim htmlMessageSubject As String = _  
    Dts.Variables("HtmlEmailSubject").Value.ToString  
  Dim htmlMessageBody As String = _  
    Dts.Variables("HtmlEmailBody").Value.ToString  
  Dim smtpServer As String = _  
    Dts.Variables("HtmlEmailServer").Value.ToString  
  
  SendMailMessage( _  
      htmlMessageFrom, htmlMessageTo, _  
      htmlMessageSubject, htmlMessageBody, _  
      True, smtpServer)  
  
  Dts.TaskResult = ScriptResults.Success  
  
End Sub  
  
Private Sub SendMailMessage( _  
    ByVal From As String, ByVal SendTo As String,  _  
    ByVal Subject As String, ByVal Body As String, _  
    ByVal IsBodyHtml As Boolean, ByVal Server As String)  
  
  Dim htmlMessage As MailMessage  
  Dim mySmtpClient As SmtpClient  
  
  htmlMessage = New MailMessage( _  
    From, SendTo, Subject, Body)  
  htmlMessage.IsBodyHtml = IsBodyHtml  
  
  mySmtpClient = New SmtpClient(Server)  
  mySmtpClient.Credentials = CredentialCache.DefaultNetworkCredentials  
  mySmtpClient.Send(htmlMessage)  
  
End Sub  
```  
  
```csharp  
public void Main()  
        {  
  
            string htmlMessageFrom = Dts.Variables["HtmlEmailFrom"].Value.ToString();  
            string htmlMessageTo = Dts.Variables["HtmlEmailTo"].Value.ToString();  
            string htmlMessageSubject = Dts.Variables["HtmlEmailSubject"].Value.ToString();  
            string htmlMessageBody = Dts.Variables["HtmlEmailBody"].Value.ToString();  
            string smtpServer = Dts.Variables["HtmlEmailServer"].Value.ToString();  
  
            SendMailMessage(htmlMessageFrom, htmlMessageTo, htmlMessageSubject, htmlMessageBody, true, smtpServer);  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
  
        }  
  
        private void SendMailMessage(string From, string SendTo, string Subject, string Body, bool IsBodyHtml, string Server)  
        {  
  
            MailMessage htmlMessage;  
            SmtpClient mySmtpClient;  
  
            htmlMessage = new MailMessage(From, SendTo, Subject, Body);  
            htmlMessage.IsBodyHtml = IsBodyHtml;  
  
            mySmtpClient = new SmtpClient(Server);  
            mySmtpClient.Credentials = CredentialCache.DefaultNetworkCredentials;  
            mySmtpClient.Send(htmlMessage);  
  
        }  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Mail senden (Task)](../../integration-services/control-flow/send-mail-task.md)  
  
  
