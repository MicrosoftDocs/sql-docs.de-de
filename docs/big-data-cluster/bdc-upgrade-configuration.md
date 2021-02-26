---
title: Upgrade auf einen Big Data-Cluster mit aktivierter Konfigurationsverwaltung
titleSuffix: SQL Server big data clusters
description: Upgrade auf einen Big Data-Cluster mit aktivierter Konfigurationsverwaltung
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 0a9f6addcf73e0c50d67f75f19fedd51b2f3dbc9
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100344015"
---
# <a name="upgrading-to-a-configuration-management-enabled-cluster-cu8-or-lower-to-cu9"></a>Upgrade auf einen Cluster mit aktivierter Konfigurationsverwaltung (bis CU8 oder ab CU9)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
Ab CU9 verfügen Big Data-Cluster über ein Feature zur Konfigurationsverwaltung, mit deren Hilfe Administratoren verschiedene Bereiche des Big Data-Clusters nach der Bereitstellung ändern oder optimieren können. Zudem erhalten sie tiefere Einblicke in die Konfigurationen, die in den BDCs ausgeführt werden. Vor CU9 konnte die Konfiguration von Big Data-Clustern in der Regel nur während der Bereitstellung geändert werden, wobei jedoch einige SQL-Konfigurationen über eine benutzerdefinierte `mssql-custom.conf`-Datei konfiguriert werden konnten. Dieses Problem wurde behoben, sodass diese Einstellungen nun mit dem Feature zur Konfigurationsverwaltung konfiguriert werden können.

## <a name="migrating-sql-configurations-in-mssql-customconf-to-the-configuration-management-system"></a>Migrieren von SQL-Konfigurationen in der „mssql-custom.conf“-Datei zum Konfigurationsverwaltungssystem
Wenn Sie für die SQL Server-Masterinstanzen eine benutzerdefinierte `mssql-custom.conf`-Datei erstellt haben, führen Sie die folgenden Schritte einmal durch, um die Einstellungen über das Konfigurationssystem und nicht über die Datei zu verwalten. Wenn Sie diese Schritte nicht durchführen, können diese SQL-Konfigurationen nicht über die Funktion zur Konfigurationsverwaltung verwaltet werden und alle Änderungen, die an diesen Änderungen über die Funktion zur Konfigurationsverwaltung vorgenommen wurden, werden durch die `mssql-custom.conf`-Einstellungen überschrieben.

Schritte:
1. Upgraden Sie den Big Data-Cluster auf CU9.
> [!NOTE]
> Über die Datei `mssql-custom.conf` definierte Einstellungen werden nicht geändert oder entfernt. Sie werden lediglich vom Konfigurationsframework nicht berücksichtigt und verwaltet.

2. Legen Sie alle zuvor in der Datei `mssql-custom.conf` definierten Einstellungen mithilfe der neuen Konfigurationsfunktion fest, und wenden Sie sie an. Eine Schritt-für-Schritt-Anleitung zum Ändern von Einstellungen finden Sie unter [Übersicht über die Konfiguration von SQL Server Big Data-Clustern nach der Bereitstellung](configure-bdc-postdeployment.md). Eine umfassende Liste mit verfügbaren Einstellungen für die einzelnen Ebenen finden Sie unter [Eigenschaften für die Konfiguration von SQL Server Big Data-Clustern](reference-config-bdc-overview.md). Beachten Sie, dass für einige Einstellungen wie customerFeedback die Ebene geändert wurde, die Einstellungen jedoch weiterhin verfügbar sind.
3. Benennen Sie die Datei `mssql-custom.conf` im Container `mssql-server` in den einzelnen Masterpods in `deprecated-mssql-custom.conf` um. Wenn nur ein Master vorhanden ist, führen Sie dies für `master-0` durch. Wenn ein Downgrade oder Rollback auf ein Cluster erforderlich ist, bei dem eine Konfiguration nicht möglich ist (bis CU8 ), kann diese Datei zum Anwenden dieser benutzerdefinierten SQL-Konfigurationen wiederverwendet werden.

## <a name="downgrading-from-a-configuration-management-enabled-cluster-to-a-non-configuration-management-enabled-cluster-cu9-to-cu8-or-lower"></a>Downgrade von einem Cluster mit aktivierter Konfigurationsverwaltung auf einen Cluster ohne aktivierte Konfigurationsverwaltung (ab CU9 auf eine Version bis CU8)
Beim Downgrade von einem Cluster mit aktivierter Konfigurationsverwaltung (ab CU9) auf einen Cluster ohne aktivierte Konfigurationsverwaltung (bis CU8) geht die Funktion zum Optimieren des Big Data-Clusters nach der Bereitstellung verloren. Zudem muss die optionale `mssql-custom.conf`-Datei zum Festlegen von SQL-Konfigurationen verwendet werden. Wenn Sie die Datei beim Upgrade auf CU9 in `deprecated-mssql-custom.conf` umbenannt haben, benennen Sie sie wieder in `mssql-custom.conf` um. Wenn Sie die Datei gelöscht oder gar nicht erstellt haben und nun diese SQL-Konfigurationen definieren müssen, erstellen Sie die Datei. Eine Anleitung dazu finden Sie unter [Konfigurationseigenschaften der SQL Server-Masterinstanz (vor CU9)](reference-config-master-instance.md). Alle Einstellungen, die über die Konfigurationsverwaltung definiert und geändert werden, werden auf die vorherigen Konfigurationen oder Standardwerte des Systems zurückgesetzt. 

Nach dem Downgrade des Clusters werden die Einstellungen auf ihre Standardwerte oder auf die in der Bereitstellungsdatei „bcd.json“ angegebenen Werte zurückgesetzt. Nach dem Downgrade müssen keine weiteren Schritte durchgeführt werden.