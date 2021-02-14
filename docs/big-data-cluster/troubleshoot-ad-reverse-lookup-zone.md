---
title: 'Bereitstellung im AD-Modus beendet: Fehlender Eintrag für Reverse-Lookupzone für DC'
titleSuffix: SQL Server Big Data Cluster
description: Die Bereitstellung des BDC im AD-Modus wurde aufgrund eines fehlenden Eintrags für die Reverse-Lookupzone für den Domänencontroller im DNS-Server des Domänencontrollers angehalten.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 04/21/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 02276cee62edd604df5e018bc22cf86cddaaec03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043950"
---
# <a name="ad-mode-deployment-stopped---missing-reverse-lookup-zone-entry-for-dc"></a>Bereitstellung im AD-Modus beendet: Fehlender Eintrag für Reverse-Lookupzone für DC

Die Bereitstellung im Active Directory-Modus (AD) ist angehalten. Überprüfen Sie die Symptome, um festzustellen, ob die Ursache im fehlenden Eintrag für die Reverse-Lookupzone im DNS-Server des Domänencontrollers liegt. 

## <a name="symptom"></a>Symptom

Sie haben die Bereitstellung des BDC im AD-Modus gestartet, die Bereitstellung ist jedoch steckengeblieben und macht keine Fortschritte.

Das folgende Beispiel zeigt die Ergebnisse der Bereitstellung in einer Bash-Shell.

```
The privacy statement can be viewed at:
https://go.microsoft.com/fwlink/?LinkId=853010
 
The license terms for SQL Server Big Data Cluster can be viewed at:
Enterprise: https://go.microsoft.com/fwlink/?linkid=2104292
Standard: https://go.microsoft.com/fwlink/?linkid=2104294
Developer: https://go.microsoft.com/fwlink/?linkid=2104079
 
Cluster deployment documentation can be viewed at:
https://aka.ms/bdc-deploy
 
NOTE: Cluster creation can take a significant amount of time depending on
configuration, network speed, and the number of nodes in the cluster.
 
Starting cluster deployment.
Cluster controller endpoint is available at bdc-control.contoso.com:30080, 193.168.5.14:30080.
Waiting for control plane to be ready after 5 minutes.
Waiting for control plane to be ready after 10 minutes.
Waiting for control plane to be ready after 15 minutes.
Waiting for control plane to be ready after 20 minutes.
Waiting for control plane to be ready after 25 minutes.
```

Überprüfen Sie die aktuell bereitgestellten Pods.

```bash
kubectl get pods -n mssql-cluster
```

Die Ergebnisse unten weisen darauf hin, dass nur Pods bereitgestellt wurden, die zum Controller gehören. Die Pods für Compute, Daten oder Speicher werden nicht erstellt.

```
NAME              READY   STATUS    RESTARTS   AGE
control-rts5t     3/3     Running   0          18m
controldb-0       2/2     Running   0          18m
controlwd-csgst   1/1     Running   0          16m
dns-7kfnz         2/2     Running   0          16m
logsdb-0          1/1     Running   0          16m
logsui-2pc29      1/1     Running   0          16m
metricsdb-0       1/1     Running   0          16m
metricsdc-4rtm4   1/1     Running   0          16m
metricsdc-6lr2t   1/1     Running   0          16m
metricsdc-ftx9m   1/1     Running   0          16m
metricsdc-h59jb   1/1     Running   0          16m
metricsui-lvdpt   1/1     Running   0          16m
mgmtproxy-mkmxp   2/2     Running   0          16m
```

Überprüfen Sie die Containerprotokolle von „security-support“. Suchen Sie nach LDAP-Fehlern. 

## <a name="check-security-support-container"></a>Überprüfen des Containers „security-support“ 

Überprüfen Sie die Protokolle des Containers „security-support“.

Mit dem folgenden Befehl werden die security-support-Protokolle in einem Cluster unter den Namespace `mssql-cluster` erfasst.

```bash
azdata bdc debug copy-logs -n mssql-cluster -c security-support
```

Extrahieren Sie die Protokolle, und suchen Sie nach `\mssql-cluster\control-<identifier>\controller\control-rts5t-controller-stdout.log`.

> [!TIP]
> Protokolle können auf verschiedene Weise gesammelt werden. Anstatt die Protokolle mit [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] zu kopieren, können Sie ein Notebook in Azure Data Studio verwenden.
> Stellen Sie in Azure Data Studio eine Verbindung mit dem Kubernetes-Cluster her, und führen Sie ein entsprechendes Notebook zur Problembehandlung aus. Hier einige Beispiele für Notebooks:
>
> - TSG027 – Clusterbereitstellung beobachten
> - TSG061 – Alle Containerprotokollfragmente für Pods im Namespace des Big-Data-Clusters abrufen
> - TSG001 – `azdata copy-logs` ausführen
>

## <a name="inspect-the-logs"></a>Protokolle untersuchen

Den Speicherort des Protokolls ermitteln. Im folgenden Beispiel wird auf das Protokoll einer Controllerbereitstellung verwiesen. 

`<folderOfDebugCopyLog>\debuglogs-mssql-cluster-YYYYMMDD-HHMMSS\<namespace>\control-<identifier>\controller\control-<identifier>-controller-stdout.log`"

```
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'cntrl-controller'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=cntrl-controller,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'ldap-user'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=ldap-user,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'nginx-mgmtproxy'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=nginx-mgmtproxy,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
```

## <a name="cause"></a>Ursache

Der Eintrag für die Reverse-Lookupzone des Domänencontrollers im DNS-Eintrag des Domänencontrollers fehlt. 

## <a name="resolution"></a>Lösung

Führen Sie das folgende PowerShell-Skript aus, um zu überprüfen, ob Sie der Reverse-DNS-Eintrag (PTR-Eintrag) konfiguriert ist.

```powershell
#Domain Controller FQDN 'DCserver01.contoso.local'
$Domain_controller_FQDN = 'DCserver01.contoso.local'

#Performing Domain Controller DNS record, reverse PTR Checks...
$DcControllerDnsPtr_Result = New-Object System.Collections.ArrayList
try {
    $Domain_controller_DNS_Record = Resolve-DnsName $Domain_controller_FQDN -Type A -Server $Domain_DNS_IP_address -ErrorAction Stop
    foreach ($ip in $Domain_controller_DNS_Record.IPAddress) {
        #resolving hostname by IP address to make sure we have reverse PTR record 
        if ((Resolve-DnsName $ip).NameHost -eq $Domain_controller_FQDN) {
            [void]$DcControllerDnsPtr_Result.add("OK - $Domain_controller_FQDN has an A record with an IP $ip, Reverse PTR record is in place") 
        }
        else {
            [void]$DcControllerDnsPtr_Result.add("Missing - $Domain_controller_FQDN has an A record with an IP $ip, But no reverse PTR record was found for the host")
        }
    }
}
catch {
    [void]$DcControllerDnsPtr_Result.add("Error - " + $_.exception.message)
}

#show the results 
$DcControllerDnsPtr_Result
```

## <a name="next-steps"></a>Nächste Schritte

[Überprüfen der Reverse-DNS-Einträge (PTR-Eintrag) für den Domänencontroller](active-directory-deploy.md#verify-reverse-dns-entry-for-domain-controller)
