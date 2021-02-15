---
title: Installieren einer benutzerdefinierten Python-Laufzeit
description: Erfahren Sie, wie Sie eine benutzerdefinierte Python-Runtime für SQL Server mithilfe der Spracherweiterungen installieren. Die benutzerdefinierte Python-Runtime kann Machine Learning-Skripts ausführen.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
zone_pivot_groups: sqlml-platforms
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 23f1bac7674002178602c0d8fa844531ea4f1a8b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072940"
---
# <a name="install-a-python-custom-runtime-for-sql-server"></a>Installieren einer benutzerdefinierten Python-Laufzeit für SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Hier erfahren Sie, wie Sie eine benutzerdefinierte Python-Runtime zum Ausführen von externen Python-Skripts mit SQL Server auf folgenden Distributionen:

+ Windows
+ Ubuntu Linux
+ Red Hat Enterprise Linux (RHEL)
+ SUSE Linux Enterprise Server (SLES)

Die benutzerdefinierte Runtime kann Machine Learning-Skripts ausführen und verwendet die [SQL Server-Spracherweiterungen](../../language-extensions/language-extensions-overview.md).

Verwenden Sie Ihre eigene Version der Python-Runtime anstelle der Standardversion der Runtime mit SQL Server zu verwenden, die mit [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) installiert wird.

::: zone pivot="platform-windows"
[!INCLUDE [Python custom runtime - Windows](includes/custom-runtime-python-windows.md)]
::: zone-end

::: zone pivot="platform-linux-ubuntu"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - Ubuntu specific steps](includes/custom-runtime-python-linux-ubuntu.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-rhel"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - RHEL specific steps](includes/custom-runtime-python-linux-rhel.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-sles"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - SLES specific steps](includes/custom-runtime-python-linux-sles.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
::: zone-end

## <a name="enable-external-script"></a>Aktivieren des externen Skripts

Sie können ein externes Python-Skript mit der gespeicherten Prozedur [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) ausführen.

Zum Aktivieren externer Skripts verwenden Sie [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md), um die folgende Anweisung auszuführen.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## <a name="verify-installation"></a>Überprüfen der Installation

Verwenden Sie das folgende SQL-Skript, um die Installation und Funktionalität der benutzerdefinierten Python-Runtime zu überprüfen.

```sql
EXEC sp_execute_external_script
@language =N'myPython',
@script=N'
import sys
print(sys.path)
print(sys.version)
print(sys.executable)'
```

## <a name="next-steps"></a>Nächste Schritte

+ [Installieren einer benutzerdefinierten R-Laufzeit für SQL Server](custom-runtime-r.md)
+ [Erweiterbarkeitsframework in SQL Server](../concepts/extensibility-framework.md)
+ [Übersicht über Spracherweiterungen](../../language-extensions/language-extensions-overview.md)
