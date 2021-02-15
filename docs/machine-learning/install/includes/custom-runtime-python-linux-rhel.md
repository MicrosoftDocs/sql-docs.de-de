---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: c6d15087150e914e62b5d19951715198ced47512
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072928"
---
## <a name="install-language-extensions"></a>Installieren von Spracherweiterungen

> [!NOTE]
> Wenn Sie [Machine Learning Services](../../sql-server-machine-learning-services.md) unter SQL Server 2019 installiert haben, ist das Paket **mssql-server-extensibility** für Spracherweiterungen bereits installiert, und Sie können diesen Schritt überspringen.

Führen Sie die folgenden Befehle aus, um [SQL Server-Spracherweiterungen](../../../language-extensions/language-extensions-overview.md) unter Red Hat Enterprise Linux (RHEL) zu installieren, die für die benutzerdefinierte Python-Runtime verwendet werden.

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

## <a name="install-python-37-and-pandas"></a>Installieren von Python 3.7 and Pandas

Die für die benutzerdefinierte Python-Runtime verwendete Python-Spracherweiterung unterstützt derzeit nur [Python 3.7](https://www.python.org/). Wenn Sie eine andere Version von Python verwenden möchten, folgen Sie den Anweisungen im [GitHub-Repository für die Python-Spracherweiterung](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python), um die Erweiterung zu ändern und neu zu erstellen.

1. Führen Sie die folgenden Befehle aus, um Python 3.7 zu installieren.

    ```bash
    # Install python3.7 and the corresponding library:
    yum install gcc openssl-devel bzip2-devel libffi-devel zlib-devel
    
    cd /usr/src
    wget https://www.python.org/ftp/python/3.7.9/Python-3.7.9.tgz
    tar xzf Python-3.7.9.tgz
    
    cd Python-3.7.9
    ./configure --enable-optimizations --prefix=/usr
    make altinstall
    ```

1. Führen Sie den folgenden Befehl aus, um das Pandas-Paket zu installieren

    ```bash
    # Install pandas to /usr/lib:
    sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
    ```
