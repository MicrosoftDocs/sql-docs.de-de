---
title: Konfigurationseigenschaften der SQL Server-Masterinstanz
titleSuffix: SQL Server big data clusters
description: Referenzartikel zu den Konfigurationseigenschaften der SQL Server-Masterinstanz.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2d986013374e7f69111288d2d0f50b09130a2d68
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343517"
---
# <a name="sql-server-master-instance-configuration-properties----pre-cu9-release"></a>Konfigurationseigenschaften der SQL Server-Masterinstanz (vor CU9)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
> [!NOTE]
> Die folgenden Informationen gelten nur für Cluster vor CU9, bei denen eine Konfiguration nicht möglich ist, und bei denen für die Konfiguration der SQL Server-Masterinstanz die Datei „mssql-conf“ erforderlich ist. Bei Clustern ab CU9 wird die Funktion zur Konfigurationsverwaltung genutzt, sodass keine „mssql-conf“-Datei mehr erforderlich ist. Informationen zu verfügbaren Konfigurationen für die SQL Server-Masterinstanz und andere BDC-Komponenten finden Sie [hier](reference-config-bdc-overview.md).

## <a name="properties"></a>Eigenschaften

Sie können die folgenden SQL Server-Optionen für die Masterinstanz zum Zeitpunkt der Bereitstellung konfigurieren.

|Eigenschaft|Optionen|
| --- | --- |
|`[sqlagent]`|`enabled = { true | false }` |
|`[telemetry]`|`customerfeedback = { true | false }` |
|`[telemetry]`|`userRequestedLocalAuditDirectory = </path/file>`|
|`[licensing]`| `pid = { Enterprise | Developer }`|
|`[traceflag]`|` traceflag<#> = <####>`|

### <a name="examples"></a>Beispiele

Im folgenden Beispiel werden der SQL Server-Agent, die Telemetrie und das Ablaufverfolgungsflag 1204 aktiviert sowie eine PID für die Enterprise Edition festgelegt:

```
[sqlagent]
enabled=true

[telemetry]
customerfeedback=true
userRequestedLocalAuditDirectory = /tmp/audit

[licensing]
pid = Enterprise

[traceflag]
traceflag0 = 1204
```

Entsprechende Anweisungen finden Sie unter [Konfigurieren der Masterinstanz von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](configure-sql-server-master-instance.md).

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren der Masterinstanz von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](configure-sql-server-master-instance.md)
