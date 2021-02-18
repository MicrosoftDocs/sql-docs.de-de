---
title: Bewährte Methoden für die Leistung für SQL Server für Linux
description: Dieser Artikel enthält bewährte Methoden für die Leistung sowie Ausführungsrichtlinien für SQL Server für Linux.
author: tejasaks
ms.author: tejasaks
ms.reviewer: vanto
ms.date: 01/19/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 652db6f752f8bf46ad2b7d4779063c79359e3a33
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350942"
---
# <a name="performance-best-practices-and-configuration-guidelines-for-sql-server-on-linux"></a>Bewährte Methoden für die Leistung und Konfigurationsrichtlinien für SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Dieser Artikel enthält bewährte Methoden und Empfehlungen, um die Leistung für Datenbankanwendungen zu maximieren, die mit SQL Server für Linux verbunden sind. Diese Empfehlungen gelten speziell für die Ausführung auf der Linux-Plattform. Alle normalen SQL Server Empfehlungen wie der Indexentwurf gelten weiterhin.

Im Folgenden erhalten Sie Empfehlungen zum Konfigurieren von SQL Server und des Linux-Betriebssystems.

## <a name="linux-os-configuration"></a>Konfigurieren des Linux-Betriebssystems

Verwenden Sie die folgenden Konfigurationseinstellungen für das Linux-Betriebssystem, um bei einer SQL Server-Installation die beste Leistung zu erzielen.

### <a name="storage-configuration-recommendation"></a>Empfehlungen für die Speicherkonfiguration

#### <a name="use-storage-subsystem-with-appropriate-iops-throughput-and-redundancy"></a>Verwenden des Speichersubsystems mit angemessenen Werten für IOPS, Durchsatz und Redundanz

Das Speichersubsystem, in dem die Daten, Transaktionsprotokolle und weitere dazugehörige Dateien (z. B. Prüfpunktdateien für In-Memory-OLTP) gehostet sind, sollte in der Lage sein, sowohl durchschnittliche Workloads als auch Spitzenworkloads ordnungsgemäß zu verwalten. Normalerweise unterstützen Speicheranbieter in lokalen Umgebungen angemessene RAID-Konfigurationen für Hardware mit Striping für mehrere Datenträger, um für angemessene Werte für IOPS, Durchsatz und Redundanz zu sorgen. Dies kann für verschiedene Speicheranbieter und verschiedene Speicherangebote mit unterschiedlichen Architekturen jedoch variieren.

Für auf Azure-VMs bereitgestellte SQL Server für Linux-Instanzen sollten Sie die Verwendung von Software-RAID in Erwägung ziehen, um sicherzustellen, dass die entsprechenden Anforderungen an IOPS und Durchsatz erfüllt werden. Sehen Sie sich zu ähnlichen Speicheraspekten den folgenden Artikel an, wenn Sie SQL Server auf Azure-VMs konfigurieren: [Speicherkonfiguration für SQL Server-VMs](/azure/azure-sql/virtual-machines/windows/storage-configuration)

Unten sehen Sie ein Beispiel dafür, wie Software-RAID unter Linux auf Azure-VMs erstellt werden kann. Unten finden Sie ein Beispiel, Sie sollten jedoch eine angemessene Anzahl an Datenträgern für die erforderlichen Werte für Durchsatz und IOPS für Volumes basierend auf den Anforderungen an Daten, Transaktionsprotokolle und tempdb-EA verwenden. In diesem Beispiel wurden acht Datenträger an die Azure-VM angefügt: vier, um die Datendateien zu hosten, zwei für die Transaktionsprotokolle und zwei für die tempdb-Workload.

```bash
# To locate the devices (for example /dev/sdc) for RAID creation, use the lsblk command
# For Data volume, using 4 devices, in RAID 5 configuration with 8KB stripes
mdadm --create --verbose /dev/md0 --level=raid5 --chunk=8K --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf

# For Log volume, using 2 devices in RAID 10 configuration with 64KB stripes
mdadm --create --verbose /dev/md1 --level=raid10 --chunk=64K --raid-devices=2 /dev/sdg /dev/sdh

# For tempdb volume, using 2 devices in RAID 0 configuration with 64KB stripes
mdadm --create --verbose /dev/md2 --level=raid0 --chunk=64K --raid-devices=2 /dev/sdi /dev/sdj
```

#### <a name="disk-partitioning-and-configuration-recommendations"></a>Empfehlungen zur Partitionierung und Konfiguration von Datenträgern

Für SQL Server wird die Verwendung von RAID-Konfigurationen empfohlen. Die bereitgestellte Stripe-Dateisystemeinheit (sunit) und die Stripe-Breite sollte mit der RAID-Geometrie übereinstimmen. Hier wird ein Beispiel für ein Protokollvolume veranschaulicht, das auf einem XFS-Dateisystem basiert. 

```bash
# Creating a log volume, using 6 devices, in RAID 10 configuration with 64KB stripes
mdadm --create --verbose /dev/md3 --level=raid10 --chunk=64K --raid-devices=6 /dev/sda /dev/sdb /dev/sdc /dev/sdd /dev/sde /dev/sdf

mkfs.xfs /dev/sda1 -f -L log 
meta-data=/dev/sda1              isize=512    agcount=32, agsize=18287648 blks 
         =                       sectsz=4096  attr=2, projid32bit=1 
         =                       crc=1        finobt=1, sparse=1, rmapbt=0 
         =                       reflink=1 
data     =                       bsize=4096   blocks=585204384, imaxpct=5 
         =                       sunit=16     swidth=48 blks 
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1 
log      =internal log           bsize=4096   blocks=285744, version=2 
         =                       sectsz=4096  sunit=1 blks, lazy-count=1 
realtime =none                   extsz=4096   blocks=0, rtextents=0 
```

Das Protokollarray ist eine RAID-10-Instanz mit 6 Datenträgern und einem Bereichsstreifen mit einem Stripe mit einer Größe von 64 KB. Offensichtlich gilt also Folgendes:
   1. Die Angabe „sunit = 16 blks“ (16 × 4096 blk size = 64k) entspricht der Stripe-Größe. 
   2. „swidth = 48 blks“ (swidth/sunit = 3) entspricht der Anzahl der Datenträger im Array, mit Ausnahme der Paritätsdatenträger. 

#### <a name="file-system-configuration-recommendation"></a>Empfehlungen für die Dateisystemkonfiguration

SQL Server unterstützt sowohl EXT4- als auch XFS-Dateisysteme als Host für Datenbank, Transaktionsprotokolle und weitere Dateien wie Prüfpunktdateien für In-Memory-OLTP in SQL Server. Microsoft empfiehlt die Verwendung des XFS-Dateisystems für das Hosten der SQL Server-Daten und Transaktionsprotokolldateien.

```bash
# Formatting the volume with XFS filesystem
mkfs.xfs /dev/md0 -f -L datavolume
mkfs.xfs /dev/md1 -f -L logvolume
mkfs.xfs /dev/md2 -f -L tempdb
```

> [!NOTE]
> Es ist möglich, das XFS-Dateisystem so zu konfigurieren, dass Groß-/Kleinbuchstaben keine Rolle spielen, wenn das XFS-Volume erstellt und formatiert wird. Dabei handelt es sich um keine häufig genutzte Konfiguration im Linux-Ökosystem. Aus Kompatibilitätsgründen ist die Verwendung jedoch möglich.
>
> Beispiel: mkfs.xfs /dev/md0 -f -n version=ci -L datavolume
>
> Im Beispiel werden die Parameter `-n version=ci` verwendet, um das XFS-Dateisystem so zu konfigurieren, dass Groß-/Kleinbuchstaben keine Rolle spielen.

##### <a name="persistent-memory-filesystem-recommendation"></a>Empfehlungen für das PMEM-Dateisystem

Für die Konfiguration des Dateisystems für PMEM-Geräte sollte die Blockzuordnung für das zugrunde liegende Dateisystem 2 MB betragen. Weitere Informationen zu diesem Thema finden Sie unter [Technische Überlegungen](sql-server-linux-configure-pmem.md#technical-considerations).

#### <a name="disable-last-accessed-datetime-on-file-systems-for-sql-server-data-and-log-files"></a>Deaktivieren von Datum/Uhrzeit des letzten Zugriffs auf Dateisysteme für SQL Server-Daten- und -Protokolldateien

An das System angefügte Laufwerke müssen der Datei `/etc/fstab` hinzugefügt werden, um sicherzustellen, dass sie nach einem Neustart automatisch wieder eingebunden werden. Außerdem wird dringend empfohlen, den UUID (Universally Unique Identifier, universell eindeutiger Bezeichner) in `/etc/fstab` zu verwenden, um auf das Laufwerk und nicht auf den Gerätenamen (z. B. `/dev/sdc1`) zu verweisen.

Die Verwendung des Attributs **noatime** mit einem beliebigen Dateisystem, das zum Speichern von SQL Server-Daten- und -Protokolldateien verwendet wird, wird ausdrücklich empfohlen. Informationen zum Festlegen dieses Attributs finden Sie in der Linux-Dokumentation. Unten finden Sie ein Beispiel, wie die Option **noatime** für ein mit einer Azure-VM verknüpftes Volume aktiviert wird.

Unten sehen Sie den Eintrag für den Bereitstellungspunkt in ***/etc/fstab***

```bash
UUID="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" /data1 xfs,rw,attr2,noatime 0 0
```

Im Beispiel oben steht der UUID für das Gerät, das Sie mithilfe des ***blkid***-Befehls finden können.

#### <a name="sql-server-and-forced-unit-access-fua-io-subsystem-capability"></a>FUA-EA-Subsystemfunktion (Forced Unit Access) in SQL Server

In bestimmten Versionen unterstützter Linux-Distributionen steht Unterstützung für die FUA-EA-Subsystemfunktion zur Verfügung, um für Dauerhaftigkeit für Daten zu sorgen. SQL Server verwendet die FUA-Funktion, um für hocheffiziente und zuverlässige Eingabe/Ausgabe für SQL Server-Workloads zu sorgen. Weitere Informationen zur Unterstützung für FUA durch Linux und den Auswirkungen auf SQL Server finden Sie im folgenden Blog: [SQL Server unter Linux: Informationen zu FUA (Forced Unit Access)](https://bobsql.com/sql-server-on-linux-forced-unit-access-fua-internals/)

Ab SUSE Linux Enterprise Server 12 SP5 und Red Hat Enterprise Linux 8.0 wird die FUA-Funktion im E/A-Subsystem unterstützt. Wenn Sie SQL Server 2017 CU6 und höher oder SQL Server 2019 verwenden, sollten Sie die folgende Konfiguration für eine leistungsstarke und effiziente E/A-Implementierung mit FUA von SQL Server verwenden.

Verwenden Sie die unten aufgeführten empfohlenen Konfigurationen, wenn die folgenden Bedingungen erfüllt sind.

- Sie verwenden SQL Server 2017 CU6 oder höher oder SQL Server 2019.
- Sie verwenden eine Linux-Distribution und -Version, die die FUA-Funktion unterstützt (Red Hat Enterprise Linux 8.0 oder höher oder SUSE Linux Enterprise Server 12 SP5).
- Sie nutzen ein Speichersubsystem und/oder Hardware, die die FUA-Funktion unterstützt und entsprechend konfiguriert ist.

Empfohlene Konfiguration:

1. Aktivieren Sie den Ablaufverfolgungsflag 3979 als Startparameter.
2. Verwenden Sie **mssql-conf** zum Konfigurieren von `control.writethrough = 1` und `control.alternatewritethrough = 0`.

Für beinahe alle anderen Konfigurationen, die die vorherigen Bedingungen nicht erfüllen, lautet die empfohlene Konfiguration wie folgt:

1. Aktivieren Sie das Ablaufverfolgungsflag 3982 als Startparameter (dies ist der Standardwert für SQL Server im Linux-Ökosystem), und sorgen Sie dabei dafür, dass das Ablaufverfolgungsflag 3979 nicht als Startparameter aktiviert ist.
2. Verwenden Sie **mssql-conf** zum Konfigurieren von `control.writethrough = 1` und `control.alternatewritethrough = 1`.

### <a name="kernel-and-cpu-settings-for-high-performance"></a>Kerneleinstellungen und CPU-Einstellungen für hohe Leistung

Im folgenden Abschnitt werden die für das Linux-Betriebssystem empfohlenen Einstellungen erläutert, um bei einer SQL Server-Installation hohe Leistung und einen hohen Durchsatz zu erzielen. Der Prozess zum Konfigurieren dieser Einstellungen wird in der Linux-Dokumentation beschrieben. Wenn [***Tuned***](https://tuned-project.org) wie beschrieben verwendet wird, unterstützt Sie dies bei der Konfiguration vieler der unten beschriebenen CPUs und Kernelkonfigurationen.

#### <a name="using-tuned-to-configure-kernel-settings"></a>Verwenden von ***Tuned*** zum Konfigurieren von Kerneleinstellungen

Für Red Hat Enterprise Linux-Benutzer (RHEL) konfiguriert das [Tuned](https://tuned-project.org)-Profil für Durchsatz und Leistung einige Kerneleinstellungen und CPU-Einstellungen automatisch (mit Ausnahme von C-Status). Ab RHEL 8.0 bietet ein ***Tuned** _-Profil namens _ *mssql**, das in Zusammenarbeit mit Red Hat entwickelt wurde, genauer abgestimmte Linux-Leistungsoptimierungen für SQL Server-Workloads. Dieses Profil enthält das RHEL-Profil „throughput-performance“. Die Definitionen dieses Profils werden im Folgenden veranschaulicht, damit Sie es anderen Linux-Distributionen und RHEL-Releases ohne dieses Profil gegenüberstellen können.

Für SUSE Linux Enterprise Server 12 SP5, Ubuntu 18.04 und Red Hat Enterprise Linux 7.x kann das ***Tuned** _-Paket manuell installiert werden. Es kann verwendet werden, um das _ *mssql**-Profil wie unten beschrieben zu erstellen und zu konfigurieren.

##### <a name="proposed-linux-settings-using-a-tuned-mssql-profile"></a>Empfohlene Linux-Einstellungen mit einem mssql-Tuned-Profil

```bash
#
# A Tuned configuration for SQL Server on Linux
#

[main]
summary=Optimize for Microsoft SQL Server
include=throughput-performance

[cpu]
force_latency=5

[sysctl]
vm.swappiness = 1
vm.dirty_background_ratio = 3
vm.dirty_ratio = 80
vm.dirty_expire_centisecs = 500
vm.dirty_writeback_centisecs = 100
vm.transparent_hugepages=always
# For multi-instance SQL deployments, use
# vm.transparent_hugepages=madvise
vm.max_map_count=1600000
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
kernel.numa_balancing=0
kernel.sched_latency_ns = 60000000
kernel.sched_migration_cost_ns = 500000
kernel.sched_min_granularity_ns = 15000000
kernel.sched_wakeup_granularity_ns = 2000000
```

Zum Aktivieren dieses Tuned-Profils speichern Sie diese Definitionen in einer Datei namens **tuned.conf** im Ordner `/usr/lib/tuned/mssql`, und aktivieren Sie das Profil mithilfe der folgenden Befehle:

```bash
chmod +x /usr/lib/tuned/mssql/tuned.conf
tuned-adm profile mssql
```

Überprüfen Sie mit dem folgenden Befehl, ob das Profil aktiviert wurde:

```bash
tuned-adm active
```

oder

```bash
tuned-adm list
```

#### <a name="cpu-settings-recommendation"></a>Empfehlung für CPU-Einstellungen

In der folgenden Tabelle finden Sie Empfehlungen für die CPU-Einstellungen:

| Einstellung | value | Weitere Informationen |
|---|---|---|
| CPU frequency governor (Kontrolle der CPU-Häufigkeit) | Leistung | Dokumentation zum Befehl **cpupower** |
| ENERGY_PERF_BIAS | Leistung | Dokumentation zum Befehl **x86_energy_perf_policy** |
| min_perf_pct | 100 | Dokumentation zum Status „intel p“ |
| C-States (C-Status) | C1 only (Nur C1) | In der Linux- oder Systemdokumentation erhalten Sie Informationen dazu, wie Sie sicherstellen können, dass die Einstellung „C-States“ (C-Status) auf „C1 only“ (Nur C1) festgelegt ist. |

Wenn ***Tuned** _ wie zuvor beschrieben verwendet wird, werden die Einstellungen „CPU frequency governor“, „ENERGY_PERF_BIAS“ und „min_perf_pct“ automatisch entsprechend konfiguriert. Dies liegt daran, dass das Profil für Durchsatz und Leistung als Basis für das _ *mssql**-Profil verwendet wird. C-Statusparameter müssen manuell entsprechend der von Linux oder dem Systemvertriebspartner bereitgestellten Dokumentation konfiguriert werden.

#### <a name="disk-settings-recommendations"></a>Empfehlungen zu Datenträgereinstellungen

In der folgenden Tabelle finden Sie Empfehlungen für die Datenträgereinstellungen:

| Einstellung | value | Weitere Informationen |
|---|---|---|
| disk `readahead` | 4096 | Dokumentation zum Befehl `blockdev` |
| sysctl-Einstellungen | kernel.sched_min_granularity_ns = 10.000.000<br/>kernel.sched_wakeup_granularity_ns = 15.000.000<br/>vm.dirty_ratio = 40<br/>vm.dirty_background_ratio = 10<br/>vm.swappiness = 1 | Dokumentation zum Befehl **sysctl** |

**Beschreibung:**

- **vm.swappiness:** Dieser Parameter steuert die relative Gewichtung des Wechsels des Runtimeprozessarbeitsspeichers im Vergleich zum Dateisystemcache. Der Standardwert für diesen Parameter ist „60“, was angibt, dass das Auswechseln der Runtimeprozess-Arbeitsspeicherseiten im Verhältnis von 60:140 zum Entfernen der Dateisystemcache-Seiten steht. Das Festlegen des Werts „1“ stellt eine starke Präferenz zum Beibehalten des Runtimeprozessarbeitsspeichers im physischen Speicher auf Kosten des Dateisystemcaches dar. Da SQL Server den Pufferpool als Datenseitencache verwendet und es stark bevorzugt, für eine zuverlässige Wiederherstellung durch einen Hardware umgehenden Dateisystemcache zu schreiben, kann sich eine aggressive Swappiness-Konfiguration für leistungsstarke und dedizierte SQL Server-Instanzen als vorteilhaft erweisen.
Weitere Informationen finden Sie in der [Dokumentation zu /proc/sys/vm/ - #swappiness](https://www.kernel.org/doc/html/latest/admin-guide/sysctl/vm.html#swappiness).

- **vm.dirty_\*** : Schreibzugriffe auf SQL Server-Dateien werden nicht zwischengespeichert, um die Anforderungen an Datenintegrität zu erfüllen. Diese Parameter ermöglichen eine effiziente asynchrone Schreibleistung und senken die E/A-Speicherauswirkung von Cacheschreibvorgängen unter Linux, indem ermöglicht wird, dass der Umfang für das Zwischenspeichern eine ausreichende Größe aufweist und Leervorgänge gedrosselt werden.

- **kernel.sched_\*** : Diese Parameterwerte sind die aktuelle Empfehlung für das Anpassen des CFS-Algorithmus (Completely Fair Scheduling) im Linux-Kernel, um den Durchsatz des Netzwerks und E/A-Speicheraufrufe zu optimieren. Dabei wird vorzeitige Entfernung während Prozessen und die Wiederaufnahme von Threads berücksichtigt.

Die Verwendung des **_Tuned_ *_-Profils **mssql** konfiguriert die Einstellungen _* vm.swappiness**, **vm.dirty_\* *_ und _* kernel.sched_\*** . Die `readahead`-Datenträgerkonfiguration mithilfe des `blockdev`-Befehls erfolgt pro Gerät und muss manuell ausgeführt werden.

#### <a name="kernel-setting-auto-numa-balancing-for-multi-node-numa-systems"></a>Kernel-Einstellung für den automatischen NUMA-Ausgleich für NUMA-Systeme mit mehreren Knoten

Wenn Sie SQL Server auf einem **NUMA**-System mit mehreren Knoten installieren, ist die folgende **kernel.numa_balancing**-Kerneleinstellung standardmäßig aktiviert. Deaktivieren Sie den automatischen NUMA-Ausgleich für das **NUMA**-System mit mehreren Knoten, damit SQL Server auf diesem mit bestmöglicher Effizienz arbeiten können:

```bash
sysctl -w kernel.numa_balancing=0
```

Die Verwendung des **_Tuned_ *_-Profils **mssql** konfiguriert die Option _* kernel.numa_balancing**.

#### <a name="kernel-settings-for-virtual-address-space"></a>Kerneleinstellungen für virtuelle Adressräume

Die Standardeinstellung für **vm.max_map_count** (65536) ist für eine SQL Server-Installation möglicherweise nicht hoch genug. Ändern Sie aus diesem Grund für eine SQL Server-Bereitstellung den Wert **vm.max_map_count** mindestens in 262144, und lesen Sie den Abschnitt [Empfohlene Linux-Einstellungen für ein mssql-Tuned-Profil](#proposed-linux-settings-using-a-tuned-mssql-profile), um weitere Informationen zu Optimierungen für diese Kernelparameter zu erhalten. Der Maximalwert für „.max_map_count“ ist 2147483647.

```bash
sysctl -w vm.max_map_count=1600000
```

Die Verwendung des **_Tuned_ *_-Profils **mssql** konfiguriert die Option _* vm.max_map_count**.

#### <a name="leave-transparent-huge-pages-thp-enabled"></a>Lassen Sie die Option „Transparent Huge Pages“ aktiviert.

Bei den meisten Linux-Installationen sollte diese Option standardmäßig aktiviert sein. Es wird empfohlen, dies nicht zu ändern, damit die Leistung konstant gut bleibt. Bei hoher Arbeitsspeicherauslastung in SQL Server-Bereitstellungen mit mehreren Instanzen (z. B. bei SQL Server-Ausführung mit anderen anspruchsvollen Anwendungen auf dem Server) wird jedoch empfohlen, dass Sie die Leistung Ihrer Anwendungen nach Ausführung des folgenden Befehls testen:

```bash
echo madvise > /sys/kernel/mm/transparent_hugepage/enabled
```

Alternativ können Sie das **mssql**-**_Tuned_**-Profil mit der folgenden Zeile versehen:

```bash
vm.transparent_hugepages=madvise
```

Aktivieren Sie das Profil **mssql** außerdem nach der Änderung:

```bash
tuned-adm off
tuned-adm profile mssql
```

Die Verwendung des **_Tuned_ *_-Profils **mssql** konfiguriert die Option _* transparent_hugepage**.

#### <a name="network-setting-recommendations"></a>Empfehlungen für Netzwerkeinstellungen

Ebenso wie es Speicher- und CPU-Empfehlungen gibt, gibt es auch netzwerkspezifische Empfehlungen, die zur Referenz im Folgenden aufgeführt werden. Nicht alle im Folgenden erwähnten Einstellungen sind für verschiedene Netzwerkkarten (NICs) verfügbar. Wenden Sie sich für weitere Informationen über die einzelnen Optionen an die jeweiligen Netzwerkkartenanbieter. Testen und konfigurieren Sie diese in Entwicklungsumgebungen, bevor Sie sie in Produktionsumgebungen anwenden. Die im Folgenden erwähnten Optionen werden anhand von Beispielen erläutert. Die verwendeten Befehle sind für die Netzwerkkartenart und den -hersteller spezifisch. 

1. Konfigurieren der Netzwerkport-Puffergröße: Im folgenden Beispiel hat die Netzwerkkarte den Namen „eth0“. Dabei handelt es sich um eine Intel-basierte Netzwerkkarte. Für Intel-basierte Netzwerkkarten wird eine Puffergröße von 4 KB (4.096 Bytes) empfohlen. Überprüfen Sie die vorab festgelegten Höchstwerte, und konfigurieren Sie diese mithilfe der unten gezeigten Beispielbefehle:

 ```bash
         #To check the pre-set maximums please run the command, example NIC name used here is:"eth0"
         ethtool -g eth0
         #command to set both the rx(recieve) and tx (transmit) buffer size to 4 KB. 
         ethtool -G eth0 rx 4096 tx 4096
         #command to check the value is properly configured is:
         ethtool -g eth0
  ```

2. Aktivieren von Großrahmen: Stellen Sie sicher, dass alle Netzwerkswitchrouter und alle anderen wichtigen Bestandteile des Netzwerkpaketpfads zwischen den Clients und der SQL Server-Instanz Großrahmen unterstützen, bevor Sie Großrahmen aktivieren. Nur dann kann die Leistung durch die Aktivierung von Großrahmen verbessert werden. Stellen Sie nach Aktivierung von Großrahmen eine Verbindung mit SQL Server her, und ändern Sie die Netzwerkpaketgröße wie unten gezeigt mit `sp_configure` in „8.060“:

```bash
         #command to set jumbo frame to 9014 for a Intel NIC named eth0 is
         ifconfig eth0 mtu 9014
         #verify the setting using the command:
         ip addr | grep 9014
```

```sql
         sp_configure 'network packet size' , '8060'
         go
         reconfigure with override
         go
```

3. Standardmäßig wird empfohlen, den Port für die adaptive RX/TX-IRQ-Zusammenführung zu konfigurieren, was bedeutet, dass die Interruptbereitstellung angepasst wird, um bei niedriger Paketrate die Latenz und bei hoher Paketrate den Durchsatz zu verbessern. Beachten Sie, dass diese Einstellung möglicherweise nicht für alle verschiedenen Netzwerkinfrastrukturen verfügbar ist. Überprüfen Sie also, ob dies von der vorhandenen Netzwerkinfrastruktur unterstützt wird. Das folgende Beispiel handelt von der Netzwerkkarte namens „eth0“, bei der es sich um eine auf Intel basierende Netzwerkkarte handelt:

```bash
         #command to set the port for adaptive RX/TX IRQ coalescing
         echtool -C eth0 adaptive-rx on
         echtool -C eth0 adaptive-tx on
         #confirm the setting using the command:
         ethtool -c eth0
```

> [!NOTE]
> Für ein vorhersagbares Verhalten bei Hochleistungsumgebungen (z. B. Umgebungen für Benchmarking) können Sie die adaptive RX/TX-Zusammenführung deaktivieren und dann spezifisch die RX/TX-Interruptzusammenführung festlegen. Die folgenden Beispielbefehle deaktivieren die RX/TX-IRQ-Zusammenführung und legen dann die Werte spezifisch fest:

```bash
         #commands to disable adaptive RX/TX IRQ coalescing
         echtool -C eth0 adaptive-rx off
         echtool -C eth0 adaptive-tx off
         #confirm the setting using the command:
         ethtool -c eth0
         #Let us set the rx-usecs parameter which specify how many microseconds after at least 1 packet is received before generating an interrupt, and the [irq] parameters are the corresponding delays in updating the #status when the interrupt is disabled. For Intel bases NICs below are good values to start with:
         ethtool -C eth0 rx-usecs 100 tx-frames-irq 512
         #confirm the setting using the command:
         ethtool -c eth0
```

4. Außerdem werden die Aktivierung von RSS (Receive-Side Scaling, Empfangsseitige Skalierung) und die Standardkombination der RX- und TX-Seiten von RSS-Warteschlangen empfohlen. Es gab spezifische Szenarios bei der Zusammenarbeit mit dem Microsoft-Support, in denen die Deaktivierung von RSS auch zu Leistungsverbesserungen führte. Testen Sie diese Einstellung in Testumgebungen, bevor Sie sie in Produktionsumgebungen anwenden. Der unten gezeigte Beispielbefehl ist für Intel-Netzwerkkarten konzipiert.

```bash
         #command to get pre-set maximums
         ethtool -l eth0 
         #note the pre-set "Combined" maximum value. let's consider for this example, it is 8.
         #command to combine the queues with the value reported in the pre-set "Combined" maximum value:
         ethtool -L eth0 combined 8
         #you can verify the setting using the command below
         ethtool -l eth0
```

5. Arbeiten mit der IRQ-Affinität von NIC-Ports: Damit Sie die gewünschte Leistung durch Optimierung der IRQ-Affinität erreichen können, müssen Sie einige wichtige Parameter wie die Linux-Verarbeitung der Servertopologie, den NIC-Treiberstapel, die Standardeinstellungen und die irqbalance-Einstellung berücksichtigen. Für die Optimierung der Einstellungen der IRQ-Affinität von NIC-Ports ist Wissen über die Servertopologie, die Deaktivierung von „irqbalance“ und die Verwendung von NIC-herstellerspezifischen Einstellungen erforderlich. Im Folgenden finden Sie ein Beispiel für die spezifische Netzwerkinfrastruktur von Mellanox, anhand der die Konfiguration erläutert wird. Beachten Sie, dass die Befehle je nach Umgebung abweichen. Wenden Sie sich für weitere Informationen an den jeweiligen NIC-Hersteller:

```bash
         #disable irqbalance or get a snapshot of the IRQ settings and force the daemon to exit
         systemctl disable irqbalance.service
         #or
         irqbalance --oneshot

         #download the Mellanox mlnx_tuning_scripts tarball, https://www.mellanox.com/sites/default/files/downloads/tools/mlnx_tuning_scripts.tar.gz and extract it
         tar -xvf mlnx_tuning_scripts.tar.gz
         # be sure, common_irq_affinity.sh is executable. if not, 
         # chmod +x common_irq_affinity.sh       

         #display IRQ affinity for Mellanox NIC port; e.g eth0
         ./show_irq_affinity.sh eth0

         #optimize for best throughput performance
         ./mlnx_tune -p HIGH_THROUGHPUT

         #set hardware affinity to the NUMA node hosting physically the NIC and its port
         ./set_irq_affinity_bynode.sh `\cat /sys/class/net/eth0/device/numa_node` eth0

         #verify IRQ affinity
         ./show_irq_affinity.sh eth0

         #add IRQ coalescing optimizations
         ethtool -C eth0 adaptive-rx off
         ethtool -C eth0 adaptive-tx off
         ethtool -C eth0  rx-usecs 750 tx-frames-irq 2048

         #verify the settings
         ethtool -c eth0
```

6. Nachdem Sie die obigen Änderungen vorgenommen haben, überprüfen Sie mithilfe des folgenden Befehls die Geschwindigkeit der Netzwerkkarte, um sicherzustellen, dass die erwartete Leistung erzielt wird:

```bash
         ethtool eth0 | grep -i Speed
```

#### <a name="additional-advanced-kernelos-configuration"></a>Zusätzliche erweiterte Kernel-/Betriebssystemkonfiguration

1. Für eine optimale E/A-Speicherleistung wird die Verwendung einer Planung mit mehreren Warteschlagen für Blockgeräte empfohlen. So kann die Leistung der Blockebene gut mit schnellen SSD-Datenträgern und Systemen mit mehreren Kernen skaliert werden. In der Dokumentation können Sie herausfinden, ob diese Option für Ihre Linux-Distribution standardmäßig aktiviert ist. In den meisten anderen Fällen wird die Option durch Starten des Kernels mit **scsi_mod.use_blk_mq=y** aktiviert. Möglicherweise finden Sie in der Dokumentation zur verwendeten Linux-Distribution aber auch weitere Anweisungen dazu. Dies ist für den Linux-Upstreamkernel konsistent.

1. Da EA mit mehreren Pfaden oft für SQL Server-Bereitstellungen verwendet wird, sollte das Ziel mit mehreren Pfaden für die Gerätezuordnung (Device Mapper, DM) ebenfalls so konfiguriert werden, dass die `blk-mq`-Infrastruktur verwendet wird, indem die Kernelstartoption **dm_mod.use_blk_mq=y** aktiviert wird. Der Standardwert lautet `n` (Deaktiviert). Diese Einstellung senkt den Sperraufwand auf der DM-Ebene, wenn die zugrunde liegenden SCSI-Geräte `blk-mq` verwenden. In der Dokumentation zur verwendeten Linux-Distribution erhalten Sie weitere Anweisungen zur Konfiguration.

#### <a name="configure-swapfile"></a>Konfigurieren der Auslagerungsdatei

Stellen Sie sicher, dass Sie über eine ordnungsgemäß konfigurierte Auslagerungsdatei verfügen, damit keine Probleme mit dem Arbeitsspeicher auftreten. In der Linux-Dokumentation erhalten Sie Informationen über die Erstellung und ordnungsgemäße Größenanpassung von Auslagerungsdateien.

#### <a name="virtual-machines-and-dynamic-memory"></a>Virtuelle Computer und dynamischer Arbeitsspeicher

Wenn Sie SQL Server für Linux auf einer VM ausführen, stellen Sie sicher, dass Sie entsprechende Optionen auswählen, um dem für die VM reservierten Arbeitsspeicher gerecht zu werden. Verwenden Sie keine Features wie Hyper-V Dynamic Memory.

## <a name="sql-server-configuration"></a>SQL Server-Konfiguration

Es wird empfohlen, nach der Installation von SQL Server für Linux die folgenden Konfigurationsaufgaben auszuführen, um eine optimale Leistung für Ihre Anwendung zu erzielen.

### <a name="best-practices"></a>Bewährte Methoden

- **Verwenden von PROCESS AFFINITY für Knoten und/oder CPUs**

   Es wird empfohlen, `ALTER SERVER CONFIGURATION` zu verwenden, um `PROCESS AFFINITY` für alle **NUMANODE**-Elemente und/oder CPUs festzulegen, die Sie für SQL Server (in der Regel für alle Knoten und CPUs) unter Linux verwenden. Die Prozessoraffinität hilft dabei, das Verhalten von Linux und SQL effizient zu planen. Die Verwendung der Option **NUMANODE** ist die einfachste Methode. Beachten Sie, dass Sie **PROCESS AFFINITY** auch dann verwenden sollten, wenn auf Ihrem Computer nur ein einzelner NUMA-Knoten vorhanden ist. Weitere Informationen zum Festlegen von [PROCESS AFFINITY](../t-sql/statements/alter-server-configuration-transact-sql.md) finden Sie im Artikel **ALTER SERVER CONFIGURATION (Transact-SQL)** .

- **Konfigurieren mehrerer tempdb-Datendateien**

   Da die Installation von SQL Server für Linux keine Option zum Konfigurieren mehrerer tempdb-Dateien umfasst, empfiehlt es sich, erst nach der Installation die tempdb-Datendateien zu erstellen. Weitere Informationen finden Sie im Artikel [Empfehlungen zum Reduzieren von Konflikten bei der Zuweisung in tempdb-Datenbank für SQL Server](https://support.microsoft.com/help/2154845/recommendations-to-reduce-allocation-contention-in-sql-server-tempdb-d).

### <a name="advanced-configuration"></a>Erweiterte Konfiguration

Die folgenden Empfehlungen stellen optionale Konfigurationseinstellungen dar, die Sie nach der Installation von SQL Server für Linux durchführen können. Diese Optionen basieren auf den Anforderungen Ihrer Workload und der Konfiguration Ihres Linux-Betriebssystems.

- **Festlegen eines Arbeitsspeicherlimits mithilfe von mssql-conf**

   Damit immer genügend physischer Speicherplatz für Linux vorhanden ist, verwendet der SQL Server-Prozess standardmäßig nur 80 Prozent des physischen Speichers. Bei großen Systemen können 20 Prozent einen beachtlichen Unterschied darstellen. Bei einem System mit 1 TB RAM werden durch diese Standardeinstellung ca. 200 GB RAM freigelassen. In diesem Fall sollten Sie das Arbeitsspeicherlimit auf einen höheren Wert festlegen. Weitere Informationen finden Sie in der Dokumentation zum Tool **mssql-config** und der Einstellung [memory.memorylimitmb](sql-server-linux-configure-mssql-conf.md#memorylimit), die den für SQL Server sichtbaren Speicherplatz (in MB) steuert.

   Wenn Sie diese Einstellung ändern, achten Sie darauf, diesen Wert nicht zu hoch festzulegen. Wenn nicht genügend Arbeitsspeicher frei ist, können Probleme mit dem Linux-Betriebssystem und anderen Linux-Anwendungen auftreten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu SQL Server-Features, die die Leistung verbessern, finden Sie unter [Erste Schritte mit den Leistungsfeatures](sql-server-linux-performance-get-started.md).

Weitere Informationen zu SQL Server für Linux finden Sie in der [Übersicht für SQL Server für Linux](sql-server-linux-overview.md).