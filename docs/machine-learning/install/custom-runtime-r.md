---
title: Installieren einer benutzerdefinierten R-Laufzeit
description: Hier erfahren Sie, wie Sie eine benutzerdefinierte R-Runtime für SQL Server mithilfe der Spracherweiterungen installieren. Die benutzerdefinierte Python-Runtime kann Machine Learning-Skripts ausführen.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
zone_pivot_groups: sqlml-platforms
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 17e4a885281cf428e8a5051b4060199b2824bd90
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072755"
---
# <a name="install-an-r-custom-runtime-for-sql-server"></a>Installieren einer benutzerdefinierten R-Laufzeit für SQL Server

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Hier erfahren Sie, wie Sie eine benutzerdefinierte R-Runtime zum Ausführen von externen R-Skripts mit SQL Server auf folgenden Distributionen installieren:

+ Windows
+ Ubuntu Linux
+ Red Hat Enterprise Linux (RHEL)
+ SUSE Linux Enterprise Server (SLES)

Die benutzerdefinierte Runtime kann Machine Learning-Skripts ausführen und verwendet die [SQL Server-Spracherweiterungen](../../language-extensions/language-extensions-overview.md).

Verwenden Sie Ihre eigene Version der R-Runtime anstelle der Standardversion der Runtime mit SQL Server, die mit [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) installiert wird.

::: zone pivot="platform-windows"
[!INCLUDE [R custom runtime - Windows](includes/custom-runtime-r-windows.md)]
::: zone-end

::: zone pivot="platform-linux-ubuntu"
[!INCLUDE [R custom runtime - Linux - Prerequisites](includes/custom-runtime-r-linux-prerequisites.md)]

[!INCLUDE [R custom runtime - Linux - Ubuntu specific steps](includes/custom-runtime-r-linux-ubuntu.md)]

[!INCLUDE [R custom runtime on Linux - Common steps](includes/custom-runtime-r-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-rhel"
[!INCLUDE [R custom runtime - Linux - Prerequisites](includes/custom-runtime-r-linux-prerequisites.md)]

[!INCLUDE [R custom runtime - Linux - RHEL specific steps](includes/custom-runtime-r-linux-rhel.md)]

[!INCLUDE [R custom runtime on Linux - Common steps](includes/custom-runtime-r-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-sles"
[!INCLUDE [R custom runtime - Linux - Prerequisites](includes/custom-runtime-r-linux-prerequisites.md)]

[!INCLUDE [R custom runtime - Linux - SLES specific steps](includes/custom-runtime-r-linux-sles.md)]

[!INCLUDE [R custom runtime on Linux - Common steps](includes/custom-runtime-r-linux-common.md)]
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
    @language =N'myR',
    @script=N'
print(R.home());
print(file.path(R.home("bin"), "R"));
print(R.version);
print("Hello RExtension!");'
```

## <a name="next-steps"></a>Nächste Schritte

+ [Installieren einer benutzerdefinierten Python-Laufzeit für SQL Server](custom-runtime-python.md)
+ [Erweiterbarkeitsframework in SQL Server](../concepts/extensibility-framework.md)
+ [Übersicht über Spracherweiterungen](../../language-extensions/language-extensions-overview.md)