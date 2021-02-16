---
title: Problembehandlung für SSIS Scale Out | Microsoft-Dokumentation
description: Erfahren Sie mehr über das Behandeln von häufigen Problemen mit SSIS Scale Out-Konfigurationsfehlern und fehlenden Zugriffsberechtigungen.
ms.custom: performance
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 7d9c02b6912019304b37d3d536312c6a5707f769
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337414"
---
# <a name="troubleshoot-scale-out"></a>Problembehandlung in Scale Out

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



SSIS Scale Out umfasst die Kommunikation zwischen der SSISDB-Katalogdatenbank `SSISDB`, dem Scale Out-Masterdienst und dem Scale Out-Workerdienst. Gelegentlich wird diese Kommunikation u.a. aufgrund von Konfigurationsfehlern und fehlenden Zugriffsberechtigungen unterbrochen. Dieser Artikel soll Sie bei der Behandlung von Problemen mit der Scale Out-Konfiguration unterstützen.

Führen Sie die unten aufgeführten Schritte nacheinander aus, bis Ihr Problem gelöst ist, um den Symptomen, auf die Sie stoßen, auf den Grund zu gehen.

## <a name="scale-out-master-fails"></a>Der Scale Out-Master löst einen Fehler aus

### <a name="symptoms"></a>Symptome 
-   Der Scale Out-Master kann keine Verbindung mit SSISDB herstellen. 

-   Die Mastereigenschaften können im Scale Out-Manager nicht angezeigt werden.

-   Die Mastereigenschaften werden in der Ansicht `[catalog].[master_properties]` nicht aufgefüllt.

### <a name="solution"></a>Lösung
1.  Überprüfen Sie, ob Scale Out aktiviert ist.

    Führen Sie im Objekt-Explorer von SSMS einen Rechtsklick auf **SSISDB** aus, und überprüfen Sie, ob das **Scale Out-Feature aktiviert ist**.

    ![Scale Out ist aktiviert](media/isenabled.PNG)

    Wenn der Eigenschaftswert FALSE ist, aktivieren Sie Scale Out, indem Sie die gespeicherte Prozedur `[catalog].[enable_scaleout]` aufrufen.

2.  Überprüfen Sie, ob der in der Konfigurationsdatei des Scale Out-Masters angegebene SQL Server-Name richtig ist, und starten Sie den Scale Out-Masterdienst neu.

## <a name="scale-out-worker-fails"></a>Der Scale Out-Worker löst einen Fehler aus

### <a name="symptoms"></a>Symptome 
-   Der Scale Out-Worker kann keine Verbindung mit dem Scale Out-Master herstellen.

-   Der Scale Out-Worker wird nicht angezeigt, nachdem er dem Scale Out-Manager hinzugefügt wurde.

-   Der Scale Out-Worker wird in der Ansicht `[catalog].[worker_agents]` nicht angezeigt.

-   Der Scale Out-Workerdienst wird ausgeführt, aber der Scale Out-Worker ist offline.

### <a name="solution"></a>Lösung
Überprüfen Sie die Fehlermeldungen im Protokoll des Scale Out-Workers unter `\<drive\>:\Users\\*[account running worker service]*\AppData\Local\SSIS\Cluster\Agent`.

## <a name="no-endpoint-listening"></a>Keine Überwachung des Endpunkts

### <a name="symptoms"></a>Symptome

*"System.ServiceModel.EndpointNotFoundException: There was no endpoint listening at https://* [MachineName]:[Port] */ClusterManagement/ that could accept the message."* (System.ServiceModel.EndpointNotFoundException: Unter https://[Computername]:[Port]/ClusterManagement/ wurde keine Überwachung durch einen Endpunkt durchgeführt, der die Meldung annehmen konnte.)

### <a name="solution"></a>Lösung

1.  Überprüfen Sie, ob die in der Konfigurationsdatei des Scale Out-Masterdiensts angegebene Portnummer richtig ist, und starten Sie den Scale Out-Masterdienst neu. 

2.  Überprüfen Sie, ob der in der Konfigurationsdatei des Scale Out-Workerdiensts angegebene Masterendpunkt richtig ist, und starten Sie den Scale Out-Workerdienst neu.

3.  Überprüfen Sie, ob der Firewallport auf dem Scale Out-Masterknoten geöffnet ist.

4.  Lösen Sie alle anderen bestehenden Verbindungsprobleme zwischen dem Scale Out-Masterknoten und dem Scale Out-Workerknoten.

## <a name="could-not-establish-trust-relationship"></a>Es konnte keine Vertrauensstellung hergestellt werden

### <a name="symptoms"></a>Symptome
*""System.ServiceModel.Security.SecurityNegotiationException: Could not establish trust relationship for the SSL/TLS secure channel with authority '[Machine Name]:[Port]'."* (System.ServiceModel.Security.SecurityNegotiationException: Es konnte keine Vertrauensstellung für den sicheren SSL/TLS-Kanal mit der Autorität „[Computername]:[Port]“ eingerichtet werden.)

*"System.Net.WebException: Die zugrunde liegende Verbindung wurde geschlossen: Could not establish trust relationship for the SSL/TLS secure channel."* (System.Net.WebException: Die zugrunde liegende Verbindung wurde vom Server geschlossen: Es konnte keine Vertrauensstellung für den sicheren SSL/TLS-Kanal eingerichtet werden.)

*"System.Security.Authentication.AuthenticationException: System.Security.Authentication.AuthenticationException: The remote certificate is invalid according to the validation procedure."* (System.Security.Authentication.AuthenticationException: Das Remotezertifikat ist laut Validierungsverfahren ungültig.)

### <a name="solution"></a>Lösung
1.  Installieren Sie das Scale Out-Masterzertifikat im Stammzertifikatspeicher auf dem lokalen Computer auf dem Scale Out-Workerknoten, falls noch nicht geschehen, und starten Sie den Scale Out-Workerdienst neu.

2.  Überprüfen Sie, ob der Hostname im Masterendpunkt im allgemeinen Namen des Scale Out-Masterzertifikats enthalten ist. Falls dies nicht der Fall ist, setzen Sie den Masterendpunkt in der Konfigurationsdatei des Scale Out-Workers zurück, und starten Sie den Scale Out-Workerdienst neu. 

    > [!NOTE]
    > Wenn Sie den Hostnamen des Masterendpunkts aufgrund von DNS-Einstellungen nicht ändern können, müssen Sie das Scale Out-Masterzertifikat ändern. Weitere Informationen finden Sie unter [Verwalten von Zertifikaten für Scale Out](deal-with-certificates-in-ssis-scale-out.md).

3.  Überprüfen Sie, ob der Fingerabdruck des Masters, der in der Konfiguration des Scale Out-Workers angegeben wird, mit dem Fingerabdruck des Zertifikats des Scale Out-Masters übereinstimmt. 

## <a name="could-not-establish-secure-channel"></a>Es konnte kein sicherer Kanal hergestellt werden

### <a name="symptoms"></a>Symptome

*"System.ServiceModel.Security.SecurityNegotiationException: Could not establish secure channel for SSL/TLS with authority '[Machine Name]:[Port]'."* (System.ServiceModel.Security.SecurityNegotiationException: Es konnte kein sicherer SSL/TLS-Kanal mit der Autorität „[Computername]:[Port]“ hergestellt werden.)

*"System.Net.WebException: Anforderung abgebrochen: Could not create SSL/TLS secure channel."* (System.Net.WebException: Die Anforderung wurde abgebrochen: Es konnte kein sicherer SSL/TLS-Kanal erstellt werden).

### <a name="solution"></a>Lösung
Überprüfen Sie mithilfe des folgenden Befehls, ob das Konto, das den Scale Out-Workerdienst ausführt, Zugriff auf das Zertifikat des Scale Out-Workers hat:

```dos
winhttpcertcfg.exe -l -c LOCAL_MACHINE\MY -s {CN of the worker certificate}
```

Wenn das Konto keinen Zugriff hat, erteilen Sie Ihm diesen mithilfe des folgenden Befehls, und starten Sie den Scale Out-Workerdienst neu.

```dos
winhttpcertcfg.exe -g -c LOCAL_MACHINE\My -s {CN of the worker certificate} -a {the account running Scale Out Worker service}
```

## <a name="http-request-forbidden"></a>HTTP-Anforderung nicht zulässig

### <a name="symptoms"></a>Symptome

*"System.ServiceModel.Security.MessageSecurityException: The HTTP request was forbidden with client authentication scheme 'Anonymous'."* (System.ServiceModel.Security.MessageSecurityException: Die HTTP-Anforderung wurde mit dem Clientauthentifizierungsschema „Anonym“ verboten.)

*"System.Net.WebException: The remote server returned an error: (403) Forbidden."* (System.Net.WebException: Der Remoteserver hat einen Fehler zurückgegeben: (403) Verboten.)

### <a name="solution"></a>Lösung
1.  Installieren Sie das Scale Out-Workerzertifikat im Stammzertifikatspeicher auf dem lokalen Computer auf dem Scale Out-Masterknoten, falls noch nicht geschehen, und starten Sie den Scale Out-Workerdienst neu.

2.  Bereinigen Sie unnötige Zertifikate auf dem Scale Out-Masterknoten im Stammzertifikatspeicher des lokalen Computers.

3.  Konfigurieren Sie Schannel (sicherer Kanal), damit dieser keine Liste von vertrauenswürdigen Stammzertifizierungsstellen mehr während des TLS/SSL-Handshakevorgangs sendet, indem Sie den folgenden Registrierungseintrag unterhalb des Scale Out-Masterknotens hinzufügen.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL`

    Wertname: **SendTrustedIssuerList** 

    Werttyp: **REG_DWORD** 

    Wertdaten: **0 (FALSE)**

4.  Ist es nicht möglich, alle nicht selbstsignierten Zertifikate zu bereinigen, wie dies in Schritt 2 beschrieben ist, legen Sie den Wert des folgenden Registrierungsschlüssels auf „2“ fest.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL`

    Wertname: **ClientAuthTrustMode** 

    Werttyp: **REG_DWORD** 

    Wertdaten: **2**

    > [!NOTE]
    > Wenn der Stammzertifikatspeicher Zertifikate enthält, die nicht selbst signiert werden, tritt bei der Clientzertifikatauthentifizierung ein Fehler auf. Weitere Informationen finden Sie unter [Potenzielle Ablehnung von Clientzertifikatanforderungen mit den HTTP-Fehlern 403.7 oder 403.16 durch die IIS 8 (Internetinformationsdienste)](https://support.microsoft.com/help/2802568/internet-information-services-iis-8-may-reject-client-certificate-requ).

## <a name="http-request-error"></a>Fehler bei der HTTP-Anforderung

### <a name="symptoms"></a>Symptome

*"System.ServiceModel.CommunicationException: An error occurred while making the HTTP request to https://[Machine Name]:[Port]/ClusterManagement/. Dies könnte daran liegen, dass das Serverzertifikat nicht richtig mit „HTTP.SYS“ im HTTPS-Fall konfiguriert wurde. Eine andere mögliche Ursache kann eine fehlende Übereinstimmung bei der Sicherheitsbindung zwischen Client und Server sein.“*

### <a name="solution"></a>Lösung
1.  Überprüfen Sie mithilfe des folgenden Befehls, ob das Scale Out-Masterzertifikat korrekt an den Port im Masterendpunkt auf dem Masterknoten gebunden ist:

    ```dos
    netsh http show sslcert ipport=0.0.0.0:{Master port}
    ```

    Überprüfen Sie, ob der angezeigte Zertifikathash mit dem Zertifikatfingerabdruck des Scale Out-Masters übereinstimmt. Wenn die Bindung nicht korrekt durchgeführt wurde, setzen Sie sie mithilfe des folgenden Befehls zurück, und starten Sie den Scale Out-Workerdienst neu.

    ```dos
    netsh http delete sslcert ipport=0.0.0.0:{Master port}
    netsh http add sslcert ipport=0.0.0.0:{Master port} certhash={Master certificate thumbprint} certstorename=Root  appid={random guid}
    ```

## <a name="cannot-open-certificate-store"></a>Der Zertifikatspeicher kann nicht geöffnet werden

### <a name="symptoms"></a>Symptome
Die Validierung ist bei der Herstellung einer Verbindungen zwischen dem Scale Out-Worker und dem Scale Out-Master im Scale Out-Manager fehlgeschlagen, und es wird folgende Fehlermeldung angezeigt: *Der Zertifikatspeicher auf dem Computer konnte nicht geöffnet werden.*

### <a name="solution"></a>Lösung

1.  Führen Sie den Scale Out-Manager als Administrator aus. Wenn Sie den Scale Out-Manager mit SSMS öffnen, müssen Sie SSMS als Administrator ausführen.

2.  Starten Sie den Remoteregistrierungsdienst auf dem Computer, falls dieser nicht ausgeführt wird.

## <a name="execution-doesnt-start"></a>Die Ausführung startet nicht

### <a name="symptoms"></a>Symptome
Die Ausführung in Scale Out startet nicht.

### <a name="solution"></a>Lösung

Überprüfen Sie in der Ansicht `[catalog].[worker_agents]` den Status des Computers, den Sie ausgewählt haben. Es muss mindestens ein Worker online und aktiviert sein.

## <a name="no-log"></a>Kein Protokoll

### <a name="symptoms"></a>Symptome 
Die Pakete werden erfolgreich ausgeführt, allerdings werden keine Meldungen protokolliert.

### <a name="solution"></a>Lösung

Überprüfen Sie, ob die SQL Server-Authentifizierung für die SQL Server-Instanz zulässig ist, die SSISDB hostet.

> [!NOTE]  
> Wenn Sie das Konto für die Scale Out-Protokollierung geändert haben, prüfen Sie die Seite [Change the Account for Scale Out Logging (Ändern des Kontos für die Scale Out-Protokollierung)](change-logdb-account.md), und überprüfen Sie die für die Protokollierung verwendete Verbindungszeichenfolge.

## <a name="error-messages-arent-helpful"></a>Die Fehlermeldungen helfen nicht weiter

### <a name="symptoms"></a>Symptome
Die Fehlermeldungen in dem Paketausführungsbericht reichen für die Problembehandlung nicht aus.

### <a name="solution"></a>Lösung
Weitere Ausführungsprotokolle finden Sie im `TasksRootFolder`-Ordner, der in `WorkerSettings.config` konfiguriert ist. Standardmäßig handelt es sich um den Ordner `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Tasks`. Es handelt sich um *[Konto]* , das den Scale Out-Workerdienst mit dem Standardwert `SSISScaleOutWorker140` ausführt.

Führen Sie den folgenden T-SQL-Befehl aus, um das Protokoll für die Paketausführung mit der *[Ausführungs-ID]* zu suchen, sodass die *[Task-ID]* zurückgegeben wird. Suchen Sie anschließend unter `TasksRootFolder` nach dem Namen des Unterordners, der die *[Task-ID*] enthält.

```sql
SELECT [TaskId]
FROM [SSISDB].[internal].[tasks] tasks, [SSISDB].[internal].[executions] executions 
WHERE executions.execution_id = *Your Execution Id* AND tasks.JobId = executions.job_id
```

> [!WARNING]
> Verwenden Sie diese Abfrage ausschließlich zur Problembehebung. Die internen Ansichten, auf die in der Abfrage verwiesen wird, werden bald anders aussehen. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Artikeln, in denen das Einrichten und die Konfiguration von SSIS Scale Out behandelt werden:
-   [Erste Schritte mit Scale Out von SQL Server Integration Services (SSIS) auf einem einzelnen Computer](get-started-with-ssis-scale-out-onebox.md)
-   [Exemplarische Vorgehensweise: Einrichten von SQL Server Integration Services Scale Out (SSIS)](walkthrough-set-up-integration-services-scale-out.md)
