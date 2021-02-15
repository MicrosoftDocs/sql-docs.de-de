---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 0c0efdc599574748112c1d5909404603de24da2c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072952"
---
## <a name="install-language-extensions"></a>Installieren von Spracherweiterungen

> [!NOTE]
> Wenn Sie [Machine Learning Services](../../sql-server-machine-learning-services.md) unter SQL Server 2019 installiert haben, ist das Paket **mssql-server-extensibility** für Spracherweiterungen bereits installiert, und Sie können diesen Schritt überspringen.

Führen Sie die folgenden Befehle aus, um [SQL Server-Spracherweiterungen](../../../language-extensions/language-extensions-overview.md) unter Ubuntu Linux zu installieren, die für die benutzerdefinierte Python-Runtime verwendet werden.

1. Führen Sie diesen Befehl wenn möglich aus, um die Pakete auf dem System vor der Installation zu aktualisieren.

    ```bash
    # Install as root or sudo
    sudo apt-get update
    ```

1. Ubuntu verfügt möglicherweise nicht über die Option „https apt transport“. Führen Sie diesen Befehl aus, um sie zu installieren.

    ```bash
    # Install as root or sudo
    apt-get install apt-transport-https
    ```

1. Installieren Sie **mssql-server-extensibility** mit diesem Befehl.

    ```bash
    # Install as root or sudo
    sudo apt-get install mssql-server-extensibility
    ```

## <a name="install-python-37-and-pandas"></a>Installieren von Python 3.7 and Pandas

Die für die benutzerdefinierte Python-Runtime verwendete Python-Spracherweiterung unterstützt derzeit nur [Python 3.7](https://www.python.org/). Wenn Sie eine andere Version von Python verwenden möchten, folgen Sie den Anweisungen im [GitHub-Repository für die Python-Spracherweiterung](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python), um die Erweiterung zu ändern und neu zu erstellen.

1. Führen Sie die folgenden Befehle aus, um Python 3.7 zu installieren.

    ```bash
    # Install python3.7 and the corresponding library:
    sudo add-apt-repository ppa:deadsnakes/ppa
    sudo apt-get update
    sudo apt-get install python3.7 python3-pip libpython3.7
    ```

1. Führen Sie den folgenden Befehl aus, um das Pandas-Paket zu installieren

    ```bash
    # Install pandas to /usr/lib:
    sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
    ```
