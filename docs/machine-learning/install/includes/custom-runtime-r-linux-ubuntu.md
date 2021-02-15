---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 83b39be5be128ba4fbda17765a7b67deead2c80c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072790"
---
## <a name="install-language-extensions"></a>Installieren von Spracherweiterungen

> [!NOTE]
> Wenn Sie [Machine Learning Services](../../sql-server-machine-learning-services.md) unter SQL Server 2019 installiert haben, ist das Paket **mssql-server-extensibility** für Spracherweiterungen bereits installiert, und Sie können diesen Schritt überspringen.

Führen Sie die folgenden Befehle aus, um [SQL Server-Spracherweiterungen](../../../language-extensions/language-extensions-overview.md) unter Ubuntu Linux zu installieren, die für die benutzerdefinierte R-Runtime verwendet werden.

1. Führen Sie diesen Befehl wenn möglich aus, um die Pakete auf dem System vor der Installation zu aktualisieren.

    ```bash
    # Install as root or sudo
    sudo apt-get update
    ```

1. Installieren Sie **mssql-server-extensibility** mit diesem Befehl.

    ```bash
    # Install as root or sudo
    sudo apt-get install mssql-server-extensibility
    ```

## <a name="install-r"></a>Installieren von R

1. Wenn Sie [Machine Learning Services](../../sql-server-machine-learning-services.md) installiert haben, ist R bereits in `/opt/microsoft/ropen/3.5.2/lib64/R` installiert. Wenn Sie diesen Pfad weiterhin als R_HOME verwenden möchten, können Sie diesen Schritt überspringen.

    Wenn Sie eine andere Laufzeit von R verwenden möchten, müssen Sie zunächst `microsoft-r-open-mro` entfernen, bevor Sie mit der Installation einer neuen Version fortfahren können.

    ```bash
    sudo apt remove microsoft-r-open-mro-3.5.2
    ```

1. Installieren Sie [R (3.3 oder höher)](https://www.r-project.org/) für Ubuntu. Standardmäßig wird R in **/usr/lib/R** installiert. Dieser Pfad ist Ihr **R_HOME**. Wenn Sie R an einem anderen Speicherort installieren, notieren Sie sich diesen Pfad als **R_HOME**.

    Nachfolgende finden Sie Beispielanweisungen für Ubuntu. Ändern Sie die unten angegebene Repository-URL für Ihre Version von R.

    ```bash
    export DEBIAN_FRONTEND=noninteractive
    sudo apt-get update
    sudo apt-get --no-install-recommends -y install curl zip unzip apt-transport-https libstdc++6
    
    # Add R CRAN repository. This repository works for R 4.0.x.
    #
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
    sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu xenial-cran40/'
    sudo apt-get update
    
    # Install R runtime.
    #
    sudo apt-get -y install r-base-core
    ```
