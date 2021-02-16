---
title: Konfigurieren des persistenten Speichers (PMEM)
description: Erfahren Sie, wie Sie persistenten Speicher (Persistent Memory, PMEM) für SQL Server für Linux konfigurieren und Namespaces für PMEM-Geräte erstellen.
ms.custom: seo-lt-2019
author: briancarrig
ms.author: brcarrig
ms.date: 10/31/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
monikerRange: '>= sql-server-linux-ver15  || >= sql-server-ver15'
ms.openlocfilehash: c11807faa2180d3f86036392d42c7f6ee6b9f473
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348204"
---
# <a name="configure-persistent-memory-pmem-for-sql-server-on-linux"></a>Konfigurieren von persistentem Speicher (PMEM) für SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel wird beschrieben, wie der persistente Speicher (persistent memory, PMEM) für [!INCLUDE[sqlv15](../includes/sssql19-md.md)] für Linux konfiguriert wird.

## <a name="overview"></a>Übersicht

[!INCLUDE[sqlv15](../includes/sssql19-md.md)] verfügt über verschiedene In-Memory-Features, die persistenten Speicher verwenden. Hier fahren Sie, wie Sie den persistenten Speicher für SQL Server für Linux konfigurieren.

> [!NOTE]
> Der Begriff _Aufklärung_ wurde eingeführt, um das Konzept eines Dateisystems auszudrücken, das persistenten Speicher unterstützt. Der direkte Zugriff auf das Dateisystem von Anwendungen des Benutzerbereichs aus erfolgt mithilfe einer Speicherzuordnung (`mmap()`). Wenn eine Speicherzuordnung für eine Datei erstellt wird, kann die Anwendung Lade-/Speicheranweisungen ausgeben und den E/A-Stapel dabei vollständig umgehen. Dies gilt aus der Perspektive der Hosterweiterungsanwendung (dem Blackboxcode, über den SQLPAL mit Windows- oder Linux-Betriebssystemen interagieren kann) als „aufgeklärte“ Dateizugriffsmethode.

## <a name="create-namespaces-for-pmem-devices"></a>Erstellen von Namespaces für PMEM-Geräte

### <a name="configure-the-devices"></a>Konfigurieren der Geräte

Verwenden Sie in Linux das Hilfsprogramm `ndctl`.

- Installieren Sie `ndctl`, um das PMEM-Gerät zu konfigurieren. Sie finden das Hilfsprogramm [hier](https://docs.pmem.io/getting-started-guide/installing-ndctl).
- Erstellen Sie einen Namespace mit `ndctl`. Namespaces sind PMEM-NVDIMM-übergreifend verschachtelt und können unterschiedliche Typen von Benutzerbereichszugriffen auf Speicherbereiche auf dem Gerät bereitstellen. `fsdax` ist der standardmäßige und der empfohlene Modus für SQL Server.

```bash 
ndctl create-namespace -f -e namespace0.0 --mode=fsdax* --map=dev
```

Beachten Sie, dass der `fsdax`-Modus ausgewählt ist und dass seitenbezogene Metadaten im Systemspeicher gespeichert werden. Es wird empfohlen, `--map=dev`zu verwenden. Damit werden die Metadaten direkt im Namespace gespeichert. Das Speichern von Metadaten im Speicher mithilfe von `--map=mem` gilt zurzeit als experimentell.

Verwenden Sie `ndctl`, um den Namespace zu überprüfen. 
  
Die Beispielausgabe sieht wie folgt aus:

```bash
# ndctl list -N
{
  "dev":"namespace0.0",
  "mode":"fsdax",
  "map":"dev",
  "size":4294967296,
  "sector_size":512,
  "blockdev":"pmem0",
  "numa_node":0
}
```

### <a name="create-and-mount-pmem-device"></a>Erstellen Sie ein PMEM-Gerät, und binden Sie es ein (mounten Sie es).

Beispielsweise mit XFS

```bash
mkfs.xfs -f /dev/pmem0
mount -o dax,noatime /dev/pmem0 /mnt/dax
xfs_io -c "extsize 2m" /mnt/dax
```

Beispielsweise mit EXT4

```bash
mkfs.ext4 -b 4096 -E stride=512 -F /dev/pmem0
mount -o dax,noatime /dev/pmem0 /mnt/dax
```

## <a name="technical-considerations"></a>Technische Überlegungen

- Blockieren Sie, wie oben beschrieben, die Zuweisung von 2 MB für XFS- oder EXT4-Systeme.
- Abweichungen zwischen der Blockzuweisung und `mmap` führen zu einem stillen Fallback auf 4 KB
- Die Dateigrößen müssen ein Vielfaches von 2 MB sein (Modulo 2 MB).
- Deaktivieren Sie keine Transparent Huge Pages (THP) (standardmäßig in den meisten Distributionen aktiviert).

Wenn das Gerät mit `ndctl` konfiguriert, erstellt und eingebunden wurde, können Sie Datenbankdateien darin ablegen oder eine neue Datenbank erstellen.

Da PMEM-Geräte O_DIRECT-sicher (direkte E/A) sind, sollten Sie das Ablaufverfolgungsflag 3979 aktivieren, um den Mechanismus für das erzwungene Leeren zu deaktivieren. Weitere Informationen finden Sie unter [Verbesserung: Aktiveren des Mechanismus „Erzwungenes Leeren“ für SQL Server 2017 für Linux](https://support.microsoft.com/help/4131496/enable-forced-flush-mechanism-in-sql-server-2017-on-linux). Weitere Informationen zum erzwungenen Zugriff auf Einheiten (Forced Unit Access, FUA) finden Sie unter [Informationen zum erzwungenen Zugriff auf Einheiten ](/archive/blogs/bobsql/sql-server-on-linux-forced-unit-access-fua-internals).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu SQL Server für Linux finden Sie unter [SQL Server für Linux](sql-server-linux-overview.md).
Bewährte Methoden für die Leistung von SQL Server für Linux finden Sie unter [Bewährte Methoden für die Leistung und Konfigurationsrichtlinien für SQL Server für Linux](sql-server-linux-performance-best-practices.md).