---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: a10c54f61151835692f2bc05de440062fe2f3cfb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072791"
---
## <a name="install-language-extensions"></a>Installieren von Spracherweiterungen

> [!NOTE]
> Wenn Sie [Machine Learning Services](../../sql-server-machine-learning-services.md) unter SQL Server 2019 installiert haben, ist das Paket **mssql-server-extensibility** für Spracherweiterungen bereits installiert, und Sie können diesen Schritt überspringen.

Führen Sie die folgenden Befehle aus, um [SQL Server-Spracherweiterungen](../../../language-extensions/language-extensions-overview.md) unter SUSE Linux Enterprise Server (SLES) zu installieren, die für die benutzerdefinierte R-Runtime verwendet werden.

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

## <a name="install-r"></a>Installieren von R

1. Wenn Sie [Machine Learning Services](../../sql-server-machine-learning-services.md) installiert haben, ist R bereits in `/opt/microsoft/ropen/3.5.2/lib64/R` installiert. Wenn Sie diesen Pfad weiterhin als R_HOME verwenden möchten, können Sie diesen Schritt überspringen.

    Wenn Sie eine andere Laufzeit von R verwenden möchten, müssen Sie zunächst `microsoft-r-open-mro` entfernen, bevor Sie mit der Installation einer neuen Version fortfahren können.

    ```bash
    sudo zypper remove microsoft-r-open-mro-3.5.2
    ```

1. Installieren Sie [R (3.3 oder höher)](https://www.r-project.org/) für SUSE Linux Enterprise Server (SLES). Standardmäßig wird R in **/usr/lib/R** installiert. Dieser Pfad ist Ihr **R_HOME**. Wenn Sie R an einem anderen Speicherort installieren, notieren Sie sich diesen Pfad als **R_HOME**.
