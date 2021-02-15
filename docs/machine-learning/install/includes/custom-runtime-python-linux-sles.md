---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: a4f854d13b794644b7491db52d204c3375cc8f0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072929"
---
## <a name="install-language-extensions"></a>Installieren von Spracherweiterungen

> [!NOTE]
> Wenn Sie [Machine Learning Services](../../sql-server-machine-learning-services.md) unter SQL Server 2019 installiert haben, ist das Paket **mssql-server-extensibility** für Spracherweiterungen bereits installiert, und Sie können diesen Schritt überspringen.

Führen Sie die folgenden Befehle aus, um [SQL Server-Spracherweiterungen](../../../language-extensions/language-extensions-overview.md) unter SUSE Linux Enterprise Server (SLES) zu installieren, die für die benutzerdefinierte Python-Runtime verwendet werden.

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

## <a name="install-python-37-and-pandas"></a>Installieren von Python 3.7 and Pandas

Die für die benutzerdefinierte Python-Runtime verwendete Python-Spracherweiterung unterstützt derzeit nur [Python 3.7](https://www.python.org/). Wenn Sie eine andere Version von Python verwenden möchten, folgen Sie den Anweisungen im [GitHub-Repository für die Python-Spracherweiterung](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python), um die Erweiterung zu ändern und neu zu erstellen.

1. Installieren Sie [Python 3.7](https://www.python.org/) auf dem Server.

1. Führen Sie den folgenden Befehl aus, um das Pandas-Paket zu installieren

    ```bash
    # Install pandas to /usr/lib:
    sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
    ```
