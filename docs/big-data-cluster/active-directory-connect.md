---
title: Herstellen von Verbindungen im Active Directory-Modus
titleSuffix: SQL Server Big Data Cluster
description: Erfahren Sie, wie Sie eine Verbindung mit einem SQL Server-Big Data-Cluster in einer Active Directory-Domäne herstellen.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 809a91c59973bee1602f335ed37fbef5931bbfeb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039910"
---
# <a name="connect-big-data-clusters-2019-active-directory-mode"></a>[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] verbinden: Active Directory-Modus

In diesem Artikel wird beschrieben, wie Sie eine Verbindung mit Endpunkten von SQL Server Big Data-Clustern herstellen, die im Active Directory-Modus bereitgestellt wurden. Für die Aufgaben in diesem Artikel ist es erforderlich, dass Sie über einen im Active Directory-Modus bereitgestellten SQL Server Big Data-Cluster verfügen. Wenn Sie nicht über einen Cluster verfügen, lesen Sie die weiteren Informationen unter [Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] im Active Directory Modus](active-directory-deploy.md).

## <a name="overview"></a>Übersicht

Melden Sie sich über die AD-Authentifizierung bei der SQL Server-Masterinstanz an.

Um AD-Verbindungen mit der SQL Server-Instanz zu überprüfen, stellen Sie mit `sqlcmd` eine Verbindung mit der SQL-Masterinstanz her. Für die angegebenen Gruppen werden bei der Bereitstellung automatisch Anmeldungen erstellt (`clusterUsers` und `clusterAdmins`).

Wenn Sie Linux verwenden, führen Sie zunächst `kinit` als der AD-Benutzer und dann `sqlcmd` aus. Bei Verwendung von Windows melden Sie sich einfach als der gewünschte Benutzer von einem **in die Domäne eingebundenen Clientcomputer** aus an.

## <a name="connect-to-master-instance-from-linuxmac"></a>Verbindungsherstellung mit der Masterinstanz unter Linux/Mac

```bash
kinit <username>@<domain name>
sqlcmd -S <DNS name for master instance>,31433 -E
```

## <a name="connect-to-master-instance-from-windows"></a>Verbindungsherstellung mit der Masterinstanz unter Windows

```cmd
sqlcmd -S <DNS name for master instance>,31433 -E
```

## <a name="log-in-to-sql-server-master-instance-using-azure-data-studio-or-ssms"></a>Anmeldung bei der SQL Server-Masterinstanz über Azure Data Studio oder SSMS

Von einem in die Domäne eingebundenen Client aus können Sie SSMS oder Azure Data Studio öffnen und eine Verbindung mit der Masterinstanz erstellen. Dies entspricht der Verbindungsherstellung mit einer beliebigen SQL Server-Instanz über die AD-Authentifizierung.

Aus SSMS:

![Dialogfeld „Verbindung mit SQL Server herstellen“ in SSMS](./media/deploy-active-directory/image23.png)

Aus Azure Data Studio:

![Dialogfeld „Connect to SQL Server in Azure Data Studio“ (Herstellen einer Verbindung mit SQL Server in Azure Data Studio)](./media/deploy-active-directory/image24.png)}

## <a name="log-in-to-controller-with-ad-authentication"></a>Anmeldung beim Controller mithilfe der AD-Authentifizierung

### <a name="connect-to-controller-with-ad-authentication-from-linuxmac"></a>Verbindungsherstellung mit dem Controller mithilfe der AD-Authentifizierung unter Linux/Mac

Es gibt zwei Optionen zum Herstellen einer Verbindung mit dem Controllerendpunkt mit [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] und der AD-Authentifizierung. Sie können den Parameter *--endpoint/-e* verwenden:

```bash
kinit <username>@<domain name>
azdata login -e https://<controller DNS name>:30080 --auth ad
```

Alternativ können Sie mithilfe des Parameters *--namespace/-n* eine Verbindung herstellen, bei dem es sich um den Big Data-Clusternamen handelt:

```bash
kinit <username>@<domain name>
azdata login -n <clusterName> --auth ad
```

### <a name="connect-to-controller-with-ad-authentication-from-windows"></a>Verbindungsherstellung mit dem Controller mithilfe der AD-Authentifizierung unter Windows

```cmd
azdata login -e https://<controller DNS name>:30080 --auth ad
```

## <a name="use-ad-authentication-to-knox-gateway-webhdfs"></a>Verwenden der AD-Authentifizierung für das Knox-Gateway (webHDFS)

Sie können auch HDFS-Befehle unter Verwendung von curl über den Knox-Gatewayendpunkt ausführen. Dies erfordert eine AD-Authentifizierung bei Knox. Der nachstehende curl-Befehl löst einen webHDFS-REST-Aufruf über das Knox-Gateway auf, um ein Verzeichnis namens `products` zu erstellen.

```bash
curl -k -v --negotiate -u : https://<Gateway DNS name>:30443/gateway/default/webhdfs/v1/products?op=MKDIRS -X PUT
```

## <a name="next-steps"></a>Nächste Schritte

[Behandeln von Problemen mit der Integration von Active Directory in SQL Server-Big Data-Clustern](troubleshoot-active-directory.md)

[Konzept: Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] im Active Directory-Modus](active-directory-deployment-background.md)
