---
title: Verwalten von Zertifikaten für SQL Server Integration Services Scale Out | Microsoft-Dokumentation
description: In diesem Artikel wird die Verwaltung von Zertifikaten zum Schutz der Kommunikation zwischen SSIS Scale Out-Mastern und Scale Out-Workern beschrieben.
ms.date: 12/19/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.custom: performance
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 0577e6f0e0d1bd78b71b2945977c313ff6a36afc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347431"
---
# <a name="manage-certificates-for-sql-server-integration-services-scale-out"></a>Verwalten von Zertifikaten für SQL Server Integration Services Scale Out

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



SSIS Scale Out verwendet zwei Zertifikate, um die Kommunikation zwischen dem Scale Out-Master und den Scale Out-Workern sicherzustellen: eins für den Master und eins für die Worker. 

## <a name="scale-out-master-certificate"></a>Scale Out-Masterzertifikat

In der Regel wird das Scale Out-Masterzertifikat während der Installation des Scale Out-Masters konfiguriert.

Auf der Seite **Integration Services-Konfiguration für horizontales Hochskalieren – Masterknoten** des SQL Server-Installations-Assistenten können Sie entweder ein neues selbstsigniertes TLS/SSL-Zertifikat erstellen oder ein vorhandenes TLS/SSL-Zertifikat verwenden.

![Masterkonfiguration](media/master-config.PNG)

**Neues Zertifikat.** Wenn keine besonderen Anforderungen an Zertifikate bestehen, können Sie ein neues selbstsigniertes TLS/SSL-Zertifikat erstellen. Sie können die allgemeinen Namen in diesem Zertifikat näher bestimmen. Stellen Sie sicher, dass der Hostname des Masterendpunkts, der später vom Scale Out-Worker verwendet werden soll, in den allgemeinen Namen enthalten ist. Standardmäßig sind der Computername und die IP-Adresse des Masterknotens darin enthalten. 

**Vorhandenes Zertifikat.** Wenn Sie sich dafür entscheiden, ein vorhandenes Zertifikat zu verwenden, klicken Sie auf **Durchsuchen**, um aus dem **Stammzertifikatspeicher** des lokalen Computers ein TLS/SSL-Zertifikat auszuwählen.

### <a name="change-the-scale-out-master-certificate"></a>Ändern des Scale Out-Masterzertifikats

Sie sollten das Scale Out-Masterzertifikat ändern, weil es abgelaufen sein kann. Führen Sie die folgende Schritte aus, um das Scale Out-Masterzertifikat zu ändern:

#### <a name="1-create-a-tlsssl-certificate"></a>1. Erstellen Sie ein TLS/SSL-Zertifikat.
Erstellen und installieren Sie mithilfe des folgenden Befehls ein neues TLS/SSL-Zertifikat auf dem Masterknoten:

```dos
MakeCert.exe -n CN={master endpoint host} SSISScaleOutMaster.cer -r -ss Root -sr LocalMachine -a sha1
```
Beispiel:

```dos
MakeCert.exe -n CN=MasterMachine SSISScaleOutMaster.cer -r -ss Root -sr LocalMachine -a sha1
```

#### <a name="2-bind-the-certificate-to-the-master-port"></a>2. Binden Sie das Zertifikat an den Masterport
Überprüfen Sie mithilfe des folgenden Befehls die ursprüngliche Bindung:

```dos
netsh http show sslcert ipport=0.0.0.0:{Master port}
```

Beispiel:

```dos
netsh http show sslcert ipport=0.0.0.0:8391
```

Löschen Sie die ursprüngliche Bindung, und richten Sie die neue Bindung mithilfe des folgenden Befehls ein:

```dos
netsh http delete sslcert ipport=0.0.0.0:{Master port}
netsh http add sslcert ipport=0.0.0.0:{Master port} certhash={TLS/SSL Certificate Thumbprint} certstorename=Root appid={original appid}
```

Beispiel:

```dos
netsh http delete sslcert ipport=0.0.0.0:8391
netsh http add sslcert ipport=0.0.0.0:8391 certhash=01d207b300ca662f479beb884efe6ce328f77d53 certstorename=Root appid={a1f96506-93e0-4c91-9171-05a2f6739e13}
```

#### <a name="3-update-the-scale-out-master-service-configuration-file"></a>3. Aktualisieren Sie die Konfigurationsdatei des Scale Out-Masterdiensts
Aktualisieren Sie die Konfigurationsdatei des Scale Out-Masterdiensts (`\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config`) auf dem Masterknoten. Aktualisieren Sie **SSLCertThumbprint** auf den Fingerabdruck des neuen TLS/SSL-Zertifikats.

#### <a name="4-restart-the-scale-out-master-service"></a>4. Starten Sie den Scale Out-Masterdienst neu.

#### <a name="5-reconnect-scale-out-workers-to-scale-out-master"></a>5. Stellen Sie erneut eine Verbindung zwischen den Scale Out-Workern und dem Scale Out-Master her.
Löschen Sie für jeden Scale Out-Worker entweder den Worker, und fügen Sie Ihn erneut mit dem [Scale Out-Manager](integration-services-ssis-scale-out-manager.md) hinzu, oder führen Sie die folgenden Schritte aus:

a.  Installieren Sie das TLS/SSL-Clientzertifikat im Stammspeicher des lokalen Computers auf dem Workerknoten.

b.  Aktualisieren Sie die Konfigurationsdatei des Scale Out-Workerdiensts.

Aktualisieren Sie die Konfigurationsdatei des Scale Out-Workerdiensts (`\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\WorkerSettings.config`) auf dem Workerknoten. Aktualisieren Sie **MasterHttpsCertThumbprint** auf den Fingerabdruck des neuen TLS/SSL-Zertifikats.

c.  Starten Sie den Scale Out-Workerdienst neu.

## <a name="scale-out-worker-certificate"></a>Scale Out-Workerzertifikat

Das Scale Out-Workerzertifikat wird automatisch bei der Installation des Scale Out-Workers installiert. 

### <a name="change-the-scale-out-worker-certificate"></a>Ändern des Scale Out-Workerzertifikats

Wenn Sie das Scale Out-Workerzertifikat ändern möchten, führen Sie die folgenden Schritte aus:

#### <a name="1-create-a-certificate"></a>1. Erstellen eines Zertifikats
Erstellen und installieren Sie mithilfe des folgenden Befehls ein Zertifikat:

```dos
MakeCert.exe -n CN={worker machine name};CN={worker machine ip} SSISScaleOutWorker.cer -r -ss My -sr LocalMachine
```

Beispiel:

```dos
MakeCert.exe -n CN=WorkerMachine;CN=10.0.2.8 SSISScaleOutWorker.cer -r -ss My -sr LocalMachine
```

#### <a name="2-install-the-client-certificate-to-the-root-store-of-the-local-computer-on-the-worker-node"></a>2. Installieren Sie das Clientzertifikat im Stammspeicher des lokalen Computers auf dem Workerknoten.

#### <a name="3-grant-service-access-to-the-certificate"></a>3. Gewähren Sie dem Dienst Zugriff auf das Zertifikat.
Löschen Sie das alte Zertifikat, und gewähren Sie dem Scale Out-Workerdienst über den folgenden Befehl Zugriff auf das Zertifikat:

```dos
certmgr.exe /del /c /s /r localmachine My /n {CN of the old certificate}
winhttpcertcfg.exe -g -c LOCAL_MACHINE\My -s {CN of the new certificate} -a {the account running Scale Out Worker service}
```

Beispiel:

```dos
certmgr.exe /del /c /s /r localmachine My /n WorkerMachine
winhttpcertcfg.exe -g -c LOCAL_MACHINE\My -s WorkerMachine -a SSISScaleOutWorker140
```

#### <a name="4-update-the-scale-out-worker-service-configuration-file"></a>4. Aktualisieren Sie die Konfigurationsdatei des Scale Out-Workerdiensts
Aktualisieren Sie die Konfigurationsdatei des Scale Out-Workerdiensts (`\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\WorkerSettings.config`) auf dem Workerknoten. Aktualisieren Sie **WorkerHttpsCertThumbprint** auf den Fingerabdruck des neuen Zertifikats.

#### <a name="5-install-the-client-certificate-to-the-root-store-of-the-local-computer-on-the-master-node"></a>5. Installieren Sie das Clientzertifikat im Stammspeicher des lokalen Computers auf dem Masterknoten.

#### <a name="6-restart-the-scale-out-worker-service"></a>6. Starten Sie den Scale Out-Workerdienst neu

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Artikeln:
-   [Scale Out-Master von Integration Services (SSIS)](integration-services-ssis-scale-out-master.md)
-   [Scale Out-Worker von Integration Services (SSIS)](integration-services-ssis-scale-out-worker.md)
