---
title: Übersicht über die Konfiguration von SQL Server Big Data-Clustern nach der Bereitstellung
titleSuffix: SQL Server big data clusters
description: Übersicht über die Konfiguration von Big Data-Clustern nach der Bereitstellung
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 478ecc9888bbd3c8f51ee96c6c796856472f93d5
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343915"
---
# <a name="how-to-configure-bdc-settings-post-deployment"></a>Konfigurieren von BDC-Einstellungen nach der Bereitstellung

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

> [!NOTE]
> Die Konfiguration von Einstellungen nach der Bereitstellung ist nur in BDC-Bereitstellungen ab CU9 verfügbar. Die Skalierungs-, Speicher- und Endpunktkonfiguration gehören nicht zur Konfiguration von Einstellungen. Optionen und Anleitungen zum Konfigurieren von BDCs vor CU9 finden Sie [hier](configure-bdc-pre-configuration.md).

Einstellungen für Big Data-Cluster auf Cluster-, Dienst- und Ressourcenebene können nach der Bereitstellung über die azdata-CLI konfiguriert werden. Mit dieser Funktion können BDC-Administratoren Konfigurationen an die jeweiligen Workloadanforderungen anpassen. In diesem Artikel werden anhand von Beispielen die Schritte zum Konfigurieren eines BDCs entsprechend den Anforderungen an die Spark-Workloads beschrieben. Die Funktion für die Konfiguration nach der Bereitstellung folgt einem „set, diff, apply“-Ablauf.

## <a name="step-by-step-configure-bdc-to-meet-your-spark-workload-requirements"></a>Schritt für Schritt: Konfigurieren eines BDCs entsprechend den Anforderungen an die Spark-Workloads

### <a name="view-the-current-configurations-of-the-big-data-cluster-spark-service"></a>Anzeigen der aktuellen Konfigurationen des Spark-Diensts im Big Data-Cluster
Im folgenden Beispiel wird gezeigt, wie die vom Benutzer konfigurierten Einstellungen des Spark-Diensts angezeigt werden. Sie können alle möglichen konfigurierbaren Einstellungen, alle vom System verwalteten und alle konfigurierbaren Einstellungen sowie ausstehende Einstellungen über optionale Parameter anzeigen. Weitere Informationen finden Sie unter [`azdata bdc spark` statement](../azdata/reference/reference-azdata-bdc-spark-statement.md).

```bash
azdata bdc spark settings show
```
#### <a name="sample-output"></a>Beispielausgabe
Spark-Dienst 

|Einstellung|Ausgeführter Wert|
| --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1` |
|`spark-defaults-conf.spark.driver.memory`|`1664m` |

### <a name="change-the-default-number-of-cores-and-memory-for-the-spark-driver-across-all-resources-with-spark-ie-for-the-spark-service"></a>Ändern der Standardanzahl der Kerne und Arbeitsspeicher für den Spark-Treiber in allen Ressourcen mit Spark (d. h. für den Spark-Dienst)
Aktualisieren Sie für den Spark-Dienst die Standardanzahl der Kerne auf 2 und den Standardarbeitsspeicher auf 7424 MB.

```bash
azdata bdc spark settings set spark-defaults-conf.spark.driver.cores=2, spark-defaults-conf.spark.driver.memory=7424m
```

### <a name="change-the-default-number-of-cores-and-memory-for-the-spark-executors-in-the-storage-pool"></a>Ändern der Standardanzahl der Kerne und Arbeitsspeicher für die Spark-Executors im Speicherpool
Aktualisieren Sie für den Speicherpool die Standardanzahl der Executorkerne auf 4.

```bash
azdata bdc spark settings set spark-defaults-conf.spark.executor.cores=4 --resource=storage-0
```

### <a name="view-the-pending-settings-changes-staged-in-the-bdc"></a>Anzeigen der im BDC bereitgestellten Änderungen an ausstehenden Einstellungen
Zeigen Sie die Änderungen an ausstehenden Einstellungen nur für den Spark-Dienst und für den gesamten Big Data-Cluster an.

#### <a name="pending-spark-service-settings"></a>Ausstehende Einstellungen für den Spark-Dienst
```bash
azdata bdc spark settings show --filter-option=pending --include-details
```

### <a name="spark-service"></a>Spark-Dienst

|Einstellung|Ausgeführter Wert|Konfigurierter Wert|Konfigurierbar|Konfiguriert |Zeitpunkt des letzten Updates|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1`| `2` | `true` | `true` |
|`spark-defaults-conf.spark.driver.memory`|`1664m`| `7424m` | `true` | `true` |

#### <a name="all-pending-settings"></a>Alle ausstehenden Einstellungen
```bash
azdata bdc settings show --filter-option=pending --include-details --recursive
```

Einstellungen auf Dienstebene für Spark: ausstehend

|Einstellung|Ausgeführter Wert|Konfigurierter Wert|Konfigurierbar|Konfiguriert|Zeitpunkt des letzten Updates|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1`| `2` | `true` | `true` |
|`spark-defaults-conf.spark.driver.memory`|`1664m`| `7424m` | `true` | `true` |

Speicher-0 Einstellungen auf Ressourcenebene für Spark: ausstehend

|Einstellung|Ausgeführter Wert|Konfigurierter Wert|Konfigurierbar|Konfiguriert|Zeitpunkt des letzten Updates|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.executor.cores`|`1`| `4` | `true` | `true` |

### <a name="apply-the-pending-settings-to-the-bdc"></a>Damit werden die ausstehenden Einstellungen auf den BDC angewendet.

```bash
azdata bdc settings apply
```

### <a name="monitor-the-status-of-the-bdc-configuration-update"></a>Überwachen des Status des BDC-Konfigurationsupdates

```bash
azdata bdc status show -all
```

## <a name="optional-steps"></a>Optionale Schritte

### <a name="revert-pending-configuration-settings"></a>Wiederherstellen der ausstehenden Konfigurationseinstellungen

Wenn die ausstehenden Konfigurationseinstellungen nicht mehr geändert werden sollen, können Sie die Bereitstellung dieser Einstellungen aufheben. Dadurch werden die ausstehenden Einstellungen auf allen Ebenen wiederhergestellt.

```bash
azdata bdc settings revert
```

### <a name="abort-the-configuration-upgrade"></a>Abbrechen des Konfigurationsupgrades

Wenn beim Konfigurationsupgrade bei einer der Komponenten ein Fehler auftritt, können Sie den Upgradevorgang abbrechen und die vorherigen BDC-Konfigurationen wiederherstellen. Einstellungen, die während des Upgrades für Änderungen bereitgestellt wurden, werden nochmals als ausstehende Einstellungen aufgeführt.

```bash
azdata bdc settings cancel-apply
```

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines Big Data-Clusters in SQL Server](configure-bdc-overview.md)