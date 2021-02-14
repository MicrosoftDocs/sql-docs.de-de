---
title: mssql-cli
description: „mssql-cli“ ist ein interaktives Befehlszeilenabfrage-Tool für SQL Server, das unter Windows, macOS oder Linux ausgeführt werden kann.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein, maghan
ms.custom: tools|mssql-cli
ms.date: 02/22/2018
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 1514c776944b66757a23baf06bb5169f66171f2d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033520"
---
# <a name="mssql-cli-command-line-query-tool-for-sql-server-preview"></a>Befehlszeilenabfrage-Tool „mssql-cli“ für SQL Server (Vorschau)

[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

„mssql-cli“ ist ein interaktives Befehlszeilentool zum Abfragen von SQL Server, das Sie unter Windows, macOS oder Linux ausführen können.

## <a name="install-mssql-cli"></a>Installieren des Tools „mssql-cli“

Ausführlichere Installationsanweisungen finden Sie im [Installationsleitfaden](https://github.com/dbcli/mssql-cli/tree/master/doc/installation). Die gängigsten Installationsszenarios sind im Folgenden zusammengefasst.

### <a name="windows-and-macos-installation"></a>Installation unter Windows und macOS

mssql-cli wird unter Windows und macOS mit `pip`installiert:

```$ pip install mssql-cli```

Ausführlichere Anweisungen finden Sie im [Installationsleitfaden](https://github.com/dbcli/mssql-cli/tree/master/doc/installation).

### <a name="linux-installation"></a>Installation unter Linux

Nach dem Registrieren des Microsoft-Repositorys kann mssql-cli bei einigen Linux-Distributionen über Paket-Manager installiert und aktualisiert werden.

Das folgende Beispiel bezieht sich auf Ubuntu 18.04 (Bionic). Weitere Informationen und Beispiele für andere Distributionen finden Sie im [Installationsleitfaden](https://github.com/dbcli/mssql-cli/tree/master/doc/installation).

```
# Import the public repository GPG keys
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -

# Register the Microsoft Ubuntu repository
sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod

# Update the list of products
sudo apt-get update

# Install mssql-cli
sudo apt-get install mssql-cli

# Install missing dependencies
sudo apt-get install -f
```

## <a name="mssql-cli-documentation"></a>Dokumentation zu „mssql-cli“

Die Dokumentation zum Tool „mssql-cli“ finden Sie im [GitHub-Repository zu mssql-cli](https://github.com/dbcli/mssql-cli).

- [Hauptseite/Info](https://github.com/dbcli/mssql-cli)
- [Installationsleitfaden](https://github.com/dbcli/mssql-cli/tree/master/doc/installation)
- [Benutzerhandbuch](https://github.com/dbcli/mssql-cli/blob/master/doc/usage_guide.md)

Weitere Dokumentationen finden Sie im [doc-Ordner](https://github.com/dbcli/mssql-cli/tree/master/doc).
