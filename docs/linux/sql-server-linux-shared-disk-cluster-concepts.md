---
title: Failoverclusterinstanzen – SQL Server für Linux
description: Zu den Konzepten für SQL Server-Failoverclusterinstanzen unter Linux zählen die Clusteringebene, die Anzahl der Instanzen, die IP-Adresse und der Name sowie der freigegebene Speicherplatz.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 1839442f20e2a48ed612e42e3c654d9e3255a477
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346400"
---
# <a name="failover-cluster-instances---sql-server-on-linux"></a>Failoverclusterinstanzen – SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel werden die Konzepte im Zusammenhang mit SQL Server-Failoverclusterinstanzen (FCI) unter Linux erläutert. 

Informationen zum Erstellen einer SQL Server-FCI unter Linux finden Sie unter [Configure SQL Server FCI on Linux (Konfigurieren von SQL Server-FCI unter Linux)](sql-server-linux-shared-disk-cluster-configure.md).

## <a name="the-clustering-layer"></a>Die Clusteringebene

* Die Clusteringebene basiert auf einem [Hochverfügbarkeits-Add-On](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) für Red Hat Enterprise Linux (RHEL). 

    > [!NOTE] 
    > Für den Zugriff auf das Red Hat-Hochverfügbarkeits-Add-On und die Dokumentation ist ein Abonnement erforderlich. 

* Die Clusteringebene in SLES basiert auf SUSE Linux Enterprise [High Availability Extension (HAE)](https://www.suse.com/products/highavailability).

    Weitere Informationen zu Clusterkonfiguration, Ressourcen-Agent-Optionen, Verwaltung, Best Practices und Empfehlungen finden Sie unter [SUSE Linux Enterprise High Availability Extension 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html).

Sowohl das RHEL-Hochverfügbarkeits-Add-On als auch SUSE HAE basieren auf [Pacemaker](https://clusterlabs.org/).

Wie in der folgenden Abbildung zu sehen, wird der Speicher zwei Servern präsentiert. Clusteringkomponenten – Corosync und Pacemaker – koordinieren die Kommunikation und die Ressourcenverwaltung. Ein Server verfügt über die aktive Verbindung mit den Speicherressourcen und SQL Server. Wenn Pacemaker einen Fehler erkennt, übernehmen die Clusteringkomponenten das Verschieben der Ressourcen auf den anderen Knoten.  

![Freigegebener SQL-Datenträgercluster mit Red Hat Enterprise Linux 7](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 


> [!NOTE]
> An diesem Punkt ist die Integration von SQL Server in Pacemaker unter Linux nicht so stark gekoppelt wie in WSFC unter Windows. Aus SQL kann der Cluster nicht erkannt werden. Die gesamte Orchestrierung erfolgt außerhalb durch Pacemaker. Zudem wird der Dienst als eigenständige Instanz von Pacemaker gesteuert. Ferner gilt der virtuelle Netzwerkname für WSFC. Dazu gibt es in Pacemaker keine Entsprechung. Es wird erwartet, dass „@@servername“ und „sys.servers“ den Knotennamen zurückgeben, während „dmvs sys.dm_os_cluster_nodes“ und „sys.dm_os_cluster_properties“ keine Datensätze erstellen. Wenn Sie eine Verbindungszeichenfolge verwenden möchten, die auf einen Zeichenfolgenservernamen zeigt und nicht die IP-Adresse verwendet, müssen Sie die IP-Adresse, die (wie in den folgenden Abschnitten beschrieben) zum Erstellen der virtuellen IP-Ressource verwendet wird, mit dem ausgewählten Servernamen beim DNS-Server registrieren.

## <a name="number-of-instances-and-nodes"></a>Anzahl der Instanzen und Knoten

Ein wichtiger Unterschied bei SQL Server für Linux besteht darin, dass es pro Linux-Server nur eine Installation von SQL Server geben kann. Diese Installation wird als Instanz bezeichnet. Das bedeutet, dass eine Linux-basierte FCI nur über eine einzige Instanz verfügt, während von Windows Server bis zu 25 FCIS pro Windows Server-Failovercluster (WSFC) unterstützt werden. Diese eine Instanz ist zudem eine Standardinstanz. Es gibt kein Konzept für eine benannten Instanz unter Linux. 

Wenn Corosync verwendet wird, kann ein Pacemaker-Cluster nur bis zu 16 Knoten enthalten, sodass eine einzelne FCI bis zu 16 Server umfassen kann. Eine mit der Standard Edition von SQL Server implementierte FCI unterstützt bis zu zwei Knoten eines Clusters, auch wenn der Pacemaker-Cluster über die maximal möglichen 16 Knoten verfügt.

Die SQL Server-Instanz in einer SQL Server-FCI ist nur auf einem der beiden Knoten aktiv.

## <a name="ip-address-and-name"></a>IP-Adresse und Name
In einem Linux Pacemaker-Cluster benötigt jede SQL Server-FCI eine eigene eindeutige IP-Adresse und einen eigenen Namen. Wenn die FCI-Konfiguration mehrere Subnetze umfasst, ist eine IP-Adresse pro Subnetz erforderlich. Der eindeutige Name und die IP-Adressen werden für den Zugriff auf die FCI verwendet, damit Anwendungen und Endbenutzer nicht wissen müssen, welcher Server dem Pacemaker-Cluster zugrunde liegt.

Der Name der FCI in DNS muss mit dem Namen der FCI-Ressource übereinstimmen, die im Pacemaker-Cluster erstellt wird.
Sowohl der Name als auch die IP-Adresse müssen in DNS registriert werden.

## <a name="shared-storage"></a>Freigegebener Speicher
Alle FCIs benötigen unabhängig davon, ob sie unter Linux oder Windows Server ausgeführt werden, einen freigegebenen Speicher. Dieser Speicher wird allen Servern präsentiert, die die FCI hosten können, aber nur ein einziger Server kann den Speicher jederzeit für die FCI verwenden. Für den freigegebenen Speicher unter Linux sind folgende Optionen verfügbar:

- iSCSI
- Network File System (NFS)
- SMB (Server Message Block): Unter Windows Server gibt es etwas andere Optionen. Eine Option, die derzeit für Linux-basierte FCIS nicht unterstützt wird, ist die Möglichkeit, für tempdb einen für den Knoten lokalen Datenträger zu verwenden, bei dem es sich um den temporären Arbeitsbereich von SQL Server handelt.

In einer Konfiguration, die mehrere Standorte umfasst, muss das, was in einem Rechenzentrum gespeichert wird, mit dem anderen synchronisiert werden. Bei einem Failovers kann die FCI online geschaltet werden, und der Speicher wird als identisch betrachtet. Hierfür ist eine externe Methode für die Speicherreplikation erforderlich, und zwar unabhängig davon, ob dies über die zugrunde liegende Speicherhardware oder ein softwarebasiertes Dienstprogramm erfolgt. 

>[!NOTE]
>Bei SQL Server müssen Linux-basierte Bereitstellungen, in denen Datenträger verwendet werden, die einem Server direkt präsentiert werden, mit XFS oder EXT4 formatiert werden. Andere Dateisysteme werden derzeit nicht unterstützt. Sämtliche Änderungen werden hier berücksichtigt.

Der Prozess für die Präsentation von freigegebenem Speicher ist für die verschiedenen unterstützten Methoden identisch:

- Konfigurieren des freigegebenen Speichers
- Einbinden des Speichers als Ordner auf den Servern, die als Knoten des Pacemaker-Clusters für die FCI dienen sollen
- Ggf. Verschieben der SQL Server-Systemdatenbanken in den freigegebenen Speicher
- Testen, ob SQL Server über alle mit dem freigegebenen Speicher verbundenen Server funktioniert

Ein wesentlicher Unterschied bei SQL Server für Linux besteht darin, dass die Systemdatenbanken während der Konfiguration der standardmäßigen Benutzerdaten und des Speicherorts für die Protokolldatei immer unter `/var/opt/mssql/data` vorhanden sein müssen. Unter Windows Server besteht die Möglichkeit, die Systemdatenbanken sowie tempdb zu verschieben. Dieser Umstand spielt bei der Konfiguration des freigegebenen Speichers für eine FCI eine Rolle.

Die Standardpfade für systemfremde Datenbanken können mithilfe des Hilfsprogramms `mssql-conf` geändert werden. Informationen zum Ändern der Standardeinstellungen finden Sie unter [Ändern des Speicherorts für das Datenbankdateien- oder Protokollverzeichnis](sql-server-linux-configure-mssql-conf.md#datadir). Sofern Sie über die richtigen Sicherheitseinstellungen verfügen, können Sie SQL Server-Daten und -Transaktionen auch an anderen Speicherorten speichern, selbst dann, wenn es sich nicht um einen Standardspeicherort handelt. Der Speicherort muss dann angegeben werden.

In den folgenden Themen wird erläutert, wie unterstützte Speichertypen für eine Linux-basierte SQL Server-FCI konfiguriert werden:

- [Configure failover cluster instance - iSCSI - SQL Server on Linux (Konfigurieren einer Failoverclusterinstanz (iSCSI): SQL Server für Linux)](sql-server-linux-shared-disk-cluster-configure-iscsi.md)
- [Configure failover cluster instance - NFS - SQL Server on Linux (Konfigurieren einer Failoverclusterinstanz (NFS): SQL Server für Linux)](sql-server-linux-shared-disk-cluster-configure-nfs.md)
- [Configure failover cluster instance - SMB - SQL Server on Linux (Konfigurieren einer Failoverclusterinstanz (SMB): SQL Server für Linux)](sql-server-linux-shared-disk-cluster-configure-smb.md)
