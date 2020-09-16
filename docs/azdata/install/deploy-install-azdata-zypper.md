---
title: Installieren von azdata mit zypper
titleSuffix: SQL Server big data clusters
description: Erfahren Sie, wie Sie das Tool azdata zum Installieren und Verwalten von Big Data-Clustern mit zypper installieren.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 01/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2445fecc554ff9f7816bbf75483ab49bbee542c1
ms.sourcegitcommit: 883435b4c7366f06ac03579752093737b098feab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89733892"
---
# <a name="install-azdata-with-zypper"></a>`azdata` mit zypper installieren

Für Linux-Distributionen mit `zypper` gibt es ein Paket für die `azdata-cli`. Das CLI-Paket wurde mit den folgenden Linux-Versionen getestet, die `zyper` verwenden:

- openSUSE 42.2 (Leap) und höher
- SLES 12 SP 2 und höher

[!INCLUDE [azdata-package-installation-remove-pip-install](../../includes/azdata-package-installation-remove-pip-install.md)]

## <a name="install-with-zypper"></a>Installieren mit zypper
>[!IMPORTANT]
>Das RPM-Paket von `azdata-cli` ist vom Python 3-Paket abhängig. Auf Ihrem System handelt es sich dabei möglicherweise um eine ältere Python-Version als die erforderliche *Version 3.6.x*. Sollte dies ein Problem für Sie darstellen, ersetzen Sie das Python 3-Paket, oder führen Sie die Anweisungen zur manuellen Installation mit [`pip`](../install/deploy-install-azdata-pip.md) aus.

1. Installieren Sie die für die Installation von `azdata-cli` erforderlichen Abhängigkeiten.

   ```bash
   sudo zypper install -y curl
   ```

1. Importieren Sie den Microsoft-Repositoryschlüssel.

   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```

1. Erstellen Sie lokale Repositoryinformationen.

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo
   ```

1. Installieren

   ```bash
   sudo zypper install --from packages-microsoft-com-mssql-server-2019 -y azdata-cli
   ```

## <a name="verify-install"></a>Überprüfen der Installation

   ```bash
   azdata
   azdata --version
   ```

## <a name="update"></a>Aktualisieren

Aktualisieren Sie die `azdata-cli` mit dem Befehl `zypper update`.

   ```bash
   sudo zypper refresh
   sudo zypper update azdata-cli
   ```

## <a name="uninstall"></a>Deinstallieren

Entfernen Sie das Paket aus Ihrem System.

```bash
   sudo zypper removerepo azdata-cli
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Big Data-Clustern finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../../includes/ssbigdataclusters-ver15.md)]?](../../big-data-cluster/big-data-cluster-overview.md).
