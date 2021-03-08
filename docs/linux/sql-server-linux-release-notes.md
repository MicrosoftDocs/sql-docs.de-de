---
title: Versionshinweise für SQL Server 2017 für Linux
description: In diesem Artikel werden Versionshinweise und unterstützte Features für SQL Server 2017 für Linux aufgeführt. Es werden sowohl Versionshinweise zum neuesten Release als auch zu einigen früheren Releases aufgeführt.
author: VanMSFT
ms.author: vanto
ms.date: 02/24/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1314744f-fcaf-46db-800e-2918fa7e1b6c
ms.openlocfilehash: 1494c406ca4889e64e2016c36d48d5c7d54ee84a
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837933"
---
# <a name="release-notes-for-sql-server-2017-on-linux"></a>Versionshinweise für SQL Server 2017 für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Die folgenden Versionshinweise gelten für [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] für Linux. Dieser Artikel ist in Abschnitte für jedes einzelne Release unterteilt. Die Unterstützung und bekannten Probleme der allgemein verfügbaren Version sind ausführlich dokumentiert. Jedes kumulative Update (cumulative update, CU) und jede allgemeine Vertriebsversion (general distribution release, GDR) verfügt über einen Link zu einem Supportartikel, in dem die Änderungen des kumulativen Updates und Links zu herunterladbaren Linux-Paketen enthalten sind.

> [!TIP]
> Diese Versionshinweise gelten spezifisch für Releases von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Weitere Informationen über die neue Version von [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] finden Sie unter [Release notes for SQL Server 2019 preview on Linux (Versionshinweise zu SQL Server 2019 Preview für Linux)](sql-server-linux-release-notes-2019.md?view=sql-server-ver15&preserve-view=true).

## <a name="supported-platforms"></a>Unterstützte Plattformen

| Plattform | Dateisystem | Installationshandbuch |
|-----|-----|-----|
| Red Hat Enterprise Linux 7.3 bis 7.8 oder 8.0 bis 8.3 (Server) | XFS oder EXT4 | [Installationshandbuch](quickstart-install-connect-red-hat.md) | 
| SUSE Enterprise Linux Server v12 SP2 bis SP5 | XFS oder EXT4 | [Installationshandbuch](quickstart-install-connect-suse.md) |
| Ubuntu 16.04 LTS, 18.04 LTS | XFS oder EXT4 | [Installationshandbuch](quickstart-install-connect-ubuntu.md) | 
| Docker-Engine 1.8+ für Windows, Mac oder Linux | – | [Installationshandbuch](quickstart-install-connect-docker.md) | 

> [!TIP]
> Weitere Informationen finden Sie in den [Systemanforderungen](sql-server-linux-setup.md#system) für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für Linux. Die neuesten Richtlinien zur Unterstützung für [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] finden Sie unter [Technical support policy for Microsoft SQL Server (Richtlinien für die technische Unterstützung von Microsoft SQL Server)](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server).

## <a name="tools"></a>Tools

Die meisten Clienttools für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] können mühelos für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für Linux verwendet werden. Für einige Tools muss womöglich eine spezifische Versionsanforderung erfüllt werden, damit ihr volles Potential mit Linux genutzt werden kann. Eine vollständige Liste der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Tools finden Sie unter [SQL Tools and Utilities for SQL Server (SQL-Tools und Hilfsprogramme für SQL Server)](../tools/overview-sql-tools.md).

## <a name="release-history"></a>Releaseverlauf

In der folgenden Tabelle wird der Releaseverlauf von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] aufgelistet.

| Release               | Version       | Veröffentlichungsdatum |
|-----------------------|---------------|--------------|
| [CU23](#CU23)         | 14.0.3381.3  | 2020-02-24   |
| [CU22-GDR](#CU22)         | 14.0.3370.1  | 2021-01-12 |
| [CU22](#CU22)         | 14.0.3356.20  | 2020-09-10   |
| [CU21](#CU21)         | 14.0.3335.7   | 2020-07-01   |
| [CU20](#CU20)         | 14.0.3294.2   | 2020-04-10   |
| [CU19](#CU19)         | 14.0.3281.6   | 5\.2.2020   |
| [CU18](#CU18)         | 14.0.3257.3   | 2019-12-09   |
| [CU17](#CU17)         | 14.0.3238.1   | 2019-10-08   |
| [CU16](#CU16)         | 14.0.3223.3   | 2019-08-01   |
| [CU15](#CU15)         | 14.0.3162.1   | 23.5.2019   |
| [CU14](#CU14)         | 14.0.3076.1   | 25.3.2019   |
| [CU13](#CU13)         | 14.0.3048.4   | 18.12.2018   |
| [CU12](#CU12)         | 14.0.3045.24  | 24.10.2018   |
| [CU11](#CU11)         | 14.0.3038.14  | 20.9.2018   |
| [CU10](#CU10)         | 14.0.3037.1   | 27.8.2018   |
| [CU9-GDR2](#CU9-GDR2) | 14.0.3035.2   | 18.8.2018   |
| [GDR2](#GDR2)         | 14.0.2002.14  | 18.8.2018   |
| [CU9](#CU9)           | 14.0.3030.27  | 18.7.2018   |
| [CU8](#CU8)           | 14.0.3029.16  | 21.6.2018   |
| [CU7](#CU7)           | 14.0.3026.27  | 24.5.2018   |
| [CU6](#CU6)           | 14.0.3025.34  | 19.4.2018   |
| [CU5](#CU5)           | 14.0.3023.8   | 20.3.2018   |
| [CU4](#CU4)           | 14.0.3022.28  | 20.2.2018   |
| [CU3](#CU3)           | 14.0.3015.40  | 3\.1.2018   |
| [GDR1](#GDR1)         | 14.0.2000.63  | 3\.1.2018   |
| [CU2](#CU2)           | 14.0.3008.27  | 28.11.2017   |
| [CU1](#CU1)           | 14.0.3006.16  | 24.10.2017   |
| [GA](#GA)             | 14.0.1000.169 | 2\.10.2017   |

## <a name="how-to-install-updates"></a><a id="cuinstall"></a> Installieren der Updates

Wenn Sie das CU-Repository (**mssql-server-2017**) konfiguriert haben, erhalten Sie das neueste kumulative Update von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Paketen, wenn Sie neue Installationen durchführen. Das CU-Repository wird standardmäßig in allen Paketinstallationsartikeln zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für Linux verwendet. Wenn Sie das GDR-Repository (**mssql-server-2017-gdr**) konfiguriert haben, erhalten Sie nur die wichtigen Sicherheitsupdates, die seit der allgemeinen Verfügbarkeit veröffentlicht wurden. Wenn Sie CU- oder GDR-Updates für Docker-Container benötigen, verwenden Sie die offiziellen Images für [Microsoft SQL Server für Linux für die Docker-Engine](https://hub.docker.com/r/microsoft/mssql-server). Weitere Informationen zur Repositorykonfiguration finden Sie unter [Configure repositories for SQL Server on Linux (Konfigurieren von Repositorys für SQL Server für Linux)](sql-server-linux-change-repo.md).

Führen Sie den entsprechenden Updatebefehl für jedes Paket durch, wenn Sie vorhandene [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Pakete aktualisieren, um das neueste kumulative Update zu erhalten. Spezifische Updateanweisungen für die einzelnen Pakete finden Sie in den folgenden Installationshandbüchern:

- [Installieren des SQL Server-Pakets](sql-server-linux-setup.md#upgrade)
- [Installieren des Volltextsuchepakets](sql-server-linux-setup-full-text-search.md)
- [Install SQL Server Integration Services (Installieren von SQL Server Integration Services)](sql-server-linux-setup-ssis.md)
- [Aktivieren des SQL Server-Agents](sql-server-linux-setup-sql-agent.md)

## <a name="cu23-february-2021"></a><a id="CU23"></a> CU23 (Februar 2021)

Dies ist das CU23-Release (Cumulative Update 23) von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die Version von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] für dieses Release ist 14.0.3381.3. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter <https://support.microsoft.com/help/5000685>.

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

> [!NOTE]
> **Ubuntu 18.04** und **RHEL 8** werden jetzt für SQL Server 2017 ab CU20 unterstützt.
>
> Die Links zu den Offline-Installationspaketen für Ubuntu zeigen auf Ubuntu 18.04-Pakete, mit Ausnahme des SSIS-Pakets (dieses ist für Ubuntu 18.04 nicht verfügbar). Wenn Sie Pakete für Ubuntu 16.04 benötigen, können Sie diese von diesem Downloadpfad herunterladen: <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Die Links zu den Offline-Installationspaketen für Red Hat zeigen auf RHEL 8-Pakete, mit Ausnahme des SSIS-Pakets (dieses ist für RHEL 8 nicht verfügbar). Wenn Sie Pakete für RHEL 7 benötigen, können Sie diese von diesem Downloadpfad herunterladen: <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3381.3-2 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3381.3-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3381.3-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3381.3-2.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3381.3-2 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3381.3-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3381.3-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3381.3-2.x86_64.rpm) | 
| Debian-Paket für Ubuntu 18.04 | 14.0.3381.3-2 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3381.3-2_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3381.3-2_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3381.3-2_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu22-gdr-january-2021"></a><a id="CU22-GDR"></a> CU22-GDR (Januar 2021)

Dies ist das CU22-GDR-Release (GDR von Cumulative Update 22) von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die Version von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] für dieses Release ist 14.0.3370.1. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter <https://support.microsoft.com/help/4577467>.

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

> [!NOTE]
> **Ubuntu 18.04** und **RHEL 8** werden jetzt für SQL Server 2017 ab CU20 unterstützt.
>
> Die Links zu den Offline-Installationspaketen für Ubuntu zeigen auf Ubuntu 18.04-Pakete, mit Ausnahme des SSIS-Pakets (dieses ist für Ubuntu 18.04 nicht verfügbar). Wenn Sie Pakete für Ubuntu 16.04 benötigen, können Sie diese von diesem Downloadpfad herunterladen: <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Die Links zu den Offline-Installationspaketen für Red Hat zeigen auf RHEL 8-Pakete, mit Ausnahme des SSIS-Pakets (dieses ist für RHEL 8 nicht verfügbar). Wenn Sie Pakete für RHEL 7 benötigen, können Sie diese von diesem Downloadpfad herunterladen: <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3370.1-23-18 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3370.1-18.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3370.1-18.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3370.1-18.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) |
| SLES RPM-Paket | 14.0.3370.1-18 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3370.1-18.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3370.1-18.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3370.1-18.x86_64.rpm) | 
| Debian-Paket für Ubuntu 18.04 | 14.0.3370.1-18 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3370.1-18_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3370.1-18_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3370.1-18_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu22-september-2020"></a><a id="CU22"></a> CU22 (September 2020)

Dies ist das CU22-Release (Cumulative Update 22) von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die Version von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] für dieses Release ist 14.0.3356.20. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter <https://support.microsoft.com/help/4577467>.

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

> [!NOTE]
> **Ubuntu 18.04** und **RHEL 8** werden jetzt für SQL Server 2017 ab CU20 unterstützt.
>
> Die Links zu den Offline-Installationspaketen für Ubuntu zeigen auf Ubuntu 18.04-Pakete, mit Ausnahme des SSIS-Pakets (dieses ist für Ubuntu 18.04 nicht verfügbar). Wenn Sie Pakete für Ubuntu 16.04 benötigen, können Sie diese von diesem Downloadpfad herunterladen: <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Die Links zu den Offline-Installationspaketen für Red Hat zeigen auf RHEL 8-Pakete, mit Ausnahme des SSIS-Pakets (dieses ist für RHEL 8 nicht verfügbar). Wenn Sie Pakete für RHEL 7 benötigen, können Sie diese von diesem Downloadpfad herunterladen: <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3356.20–23 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3356.20-23.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3356.20-23.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3356.20-23.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3356.20–23 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3356.20-23.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3356.20-23.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3356.20-23.x86_64.rpm) | 
| Debian-Paket für Ubuntu 18.04 | 14.0.3356.20–23 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3356.20-23_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3356.20-23_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3356.20-23_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu21-july-2020"></a><a id="CU21"></a> CU21 (Juli 2020)

Dies ist das CU21-Release (Cumulative Update 21) von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die Version von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] für dieses Release ist 14.0.3335.7. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter <https://support.microsoft.com/help/4557397>.

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

> [!NOTE]
> **Ubuntu 18.04** und **RHEL 8** werden jetzt für SQL Server 2017 ab CU20 unterstützt.
>
> Die Links zu den Offline-Installationspaketen für Ubuntu zeigen auf Ubuntu 18.04-Pakete, mit Ausnahme des SSIS-Pakets (dieses ist für Ubuntu 18.04 nicht verfügbar). Wenn Sie Pakete für Ubuntu 16.04 benötigen, können Sie diese von diesem Downloadpfad herunterladen: <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Die Links zu den Offline-Installationspaketen für Red Hat zeigen auf RHEL 8-Pakete, mit Ausnahme des SSIS-Pakets (dieses ist für RHEL 8 nicht verfügbar). Wenn Sie Pakete für RHEL 7 benötigen, können Sie diese von diesem Downloadpfad herunterladen: <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3335.7-17 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3335.7-17.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3335.7-17.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3335.7-17.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3335.7-17 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3335.7-17.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3335.7-17.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3335.7-17.x86_64.rpm) | 
| Debian-Paket für Ubuntu 18.04 | 14.0.3335.7-17 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3335.7-17_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3335.7-17_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3335.7-17_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu20-april-2020"></a><a id="CU20"></a> CU20 (April 2020)

Dies ist das CU20-Release (Cumulative Update 20) von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die Version von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] für dieses Release ist 14.0.3294.2. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter <https://support.microsoft.com/help/4541283>.

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

> [!NOTE]
> **Ubuntu 18.04** und **RHEL 8** werden jetzt für SQL Server 2017 ab CU20 unterstützt.
>
> Die Links zu den Offline-Installationspaketen für Ubuntu zeigen auf Ubuntu 18.04-Pakete, mit Ausnahme des SSIS-Pakets (dieses ist für Ubuntu 18.04 nicht verfügbar). Wenn Sie Pakete für Ubuntu 16.04 benötigen, können Sie diese von diesem Downloadpfad herunterladen: <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Die Links zu den Offline-Installationspaketen für Red Hat zeigen auf RHEL 8-Pakete, mit Ausnahme des SSIS-Pakets (dieses ist für RHEL 8 nicht verfügbar). Wenn Sie Pakete für RHEL 7 benötigen, können Sie diese von diesem Downloadpfad herunterladen: <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3294.2-27 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3294.2-27.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3294.2-27.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3294.2-27.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3294.2-27 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3294.2-27.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3294.2-27.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3294.2-27.x86_64.rpm) | 
| Debian-Paket für Ubuntu 18.04 | 14.0.3294.2-27 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3294.2-27_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3294.2-27_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3294.2-27_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu19-february-2020"></a><a id="CU19"></a> CU 19 (Februar 2020)

Dies ist das CU19-Release (Cumulative Update 19) von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die Version von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] für dieses Release ist 14.0.3281.6. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4535007](https://support.microsoft.com/help/4535007).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3281.6-2 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3281.6-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3281.6-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3281.6-2.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3281.6-2 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3281.6-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3281.6-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3281.6-2.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3281.6-2 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3281.6-2_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3281.6-2_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3281.6-2_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu18-december-2019"></a><a id="CU18"></a> CU18 (Dezember 2019)

Dies ist das CU18-Release (Cumulative Update 18) von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die Version von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] für dieses Release ist 14.0.3257.3. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4527377](https://support.microsoft.com/help/4527377).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3257.3-13 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3257.3-13.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3257.3-13.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3257.3-13.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3257.3-13 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3257.3-13.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3257.3-13.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3257.3-13.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3257.3-13 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3257.3-13_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3257.3-13_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3257.3-13_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

### <a name="added-support"></a>Hinzugefügte Unterstützung

- Change Data Capture wird nun ab dem kumulativen Update 18 für SQL Server 2017 für Linux unterstützt.
- Transaktionsreplikation wird nun ab dem kumulativen Update 18 für SQL Server 2017 für Linux unterstützt.

### <a name="remarks"></a>Bemerkungen

Container von SQL Server 2017 verfügen nun wie unten anhand von Beispielen beschrieben über ein neues Taggingmuster.

- `mcr.microsoft.com/mssql/server:<SQL Server Version>-<update>-<Linux Distribution>-<Linux Distribution Version>`

  Dadurch wird für das Containerimage mit der im Tag beschriebenen Kombination ein Pull ausgeführt.

- `mcr.microsoft.com/mssql/server:<SQL Server Version>-latest`

    Hiermit wird für die aktuelle SQL Server-Version auf der aktuellen unterstützten Ubuntu-Version ein Pull ausgeführt.

**Beispiele:**

`mcr.microsoft.com/mssql/server:2017-CU18-ubuntu-16.04`

Dadurch wird für das CU18-Release von SQL Server 2017, das auf dem Container von Ubuntu 16.04 basiert, ein Pull ausgeführt.

`mcr.microsoft.com/mssql/server:2017-latest`

Hiermit wird für die aktuelle Version von SQL Server 2017 (als der Artikel verfasst wurde, handelte es sich um CU18), die auf dem Container von Ubuntu 16.04 basiert, ein Pull ausgeführt.

> [!NOTE]
> Wir veröffentlichen zukünftig keine Container mit anderen Taggingmustern für Container von SQL Server 2017 mehr.


## <a name="cu17-october-2019"></a><a id="CU17"></a> CU17 (Oktober 2019)

Dies ist das CU17-Release (Cumulative Update 17) von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release ist 14.0.3238.1. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4515579](https://support.microsoft.com/help/4515579).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3238.1-19 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3238.1-19.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3238.1-19.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3238.1-19.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3238.1-19 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3238.1-19.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3238.1-19.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3238.1-19.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3238.1-19 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3238.1-19_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3238.1-19_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3238.1-19_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu16-august-2019"></a><a id="CU16"></a> CU16 (August 2019)

Dies ist das CU16-Release (Cumulative Update 16) von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet 14.0.3223.3. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4508218](https://support.microsoft.com/help/4508218).

### <a name="whats-new"></a>Neuerungen

|Neue Funktion oder Update | Details |
|:---|:---|
| MS DTC-Unterstützung | Unterstützung für den Microsoft Distributed Transaction Coordinator (MS DTC) für SQL Server 2017. Weitere Informationen finden Sie unter [Konfigurieren des Microsoft Distributed Transaction Coordinator (MS DTC) unter Linux](sql-server-linux-configure-msdtc.md). |

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3223.3-15 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3223.3-15.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3223.3-15.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3223.3-15.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3223.3-15 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3223.3-15.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3223.3-15.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3223.3-15.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3223.3-15 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3223.3-15_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3223.3-15_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3223.3-15_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu15-may-2019"></a><a id="CU15"></a> CU15 (Mai 2019)

Dies ist das CU15-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3162.1. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4498951](https://support.microsoft.com/help/4498951).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3162.1-1 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3162.1-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3162.1-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3162.1-1.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3162.1-1 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3162.1-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3162.1-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3162.1-1.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3162.1-1 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3162.1-1_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3162.1-1_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3162.1-1_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu14-mar-2019"></a><a id="CU14"></a> CU14 (März 2019)

Dies ist das CU14-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3076.1. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4484710](https://support.microsoft.com/help/4484710).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3076.1-2 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3076.1-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3076.1-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3076.1-2.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3076.1-2 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3076.1-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3076.1-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3076.1-2.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3076.1-2 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3076.1-2_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3076.1-2_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3076.1-2_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu13-dec-2018"></a><a id="CU13"></a> CU13 (Dezember 2018)

Dies ist das CU13-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3048.4. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4466404](https://support.microsoft.com/help/4466404).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3048.4-1 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3048.4-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3048.4-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3048.4-1.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3048.4-1 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3048.4-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3048.4-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3048.4-1.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3048.4-1 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3048.4-1_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3048.4-1_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3048.4-1_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu12-oct-2018"></a><a id="CU12"></a> CU12 (Oktober 2018)

Dies ist das CU12-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3045.24. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4464082](https://support.microsoft.com/help/4464082).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3045.24-1 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3045.24-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3045.24-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3045.24-1.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3045.24-1 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3045.24-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3045.24-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3045.24-1.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3045.24-1 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3045.24-1_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3045.24-1_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3045.24-1_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu11-sept-2018"></a><a id="CU11"></a> CU11 (September 2018)

Dies ist das CU11-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3038.14. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4462262](https://support.microsoft.com/help/4462262).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3038.14-2 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3038.14-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3038.14-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3038.14-2.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3038.14-2 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3038.14-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3038.14-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3038.14-2.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3038.14-2 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3038.14-2_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3038.14-2_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3038.14-2_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu10-aug-2018"></a><a id="CU10"></a> CU10 (August 2018)

Dies ist das CU10-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3037.1. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4342123](https://support.microsoft.com/help/4342123).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3037.1-2 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3037.1-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3037.1-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3037.1-2.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3037.1-2 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3037.1-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3037.1-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3037.1-2.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3037.1-2 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3037.1-2_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3037.1-2_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3037.1-2_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu9-gdr2-aug-2018"></a><a id="CU9-GDR2"></a> CU9-GDR2 (August 2018)

Hierbei handelt es sich um ein Sicherheitsupdate, das das zuvor veröffentlichte CU (CU9) für [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] enthält. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3035.2. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4293805](https://support.microsoft.com/help/4293805).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3035.2-1 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3035.2-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3035.2-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3035.2-1.x86_64.rpm)| 
| SLES RPM-Paket | 14.0.3035.2-1 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3035.2-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3035.2-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3035.2-1.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3035.2-1 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3035.2-1_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3035.2-1_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3035.2-1_amd64.deb)<br/> |

## <a name="gdr2-aug-2018"></a><a id="GDR2"></a> GDR2 (August 2018)

Hierbei handelt es sich um ein Sicherheitsupdate, das nur die Sicherheitskorrekturen von GDR2 (und GDR1) für [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] enthält.  Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.2002.14. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4293803](https://support.microsoft.com/help/4293803).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.2002.14-1 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-14.0.2002.14-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-ha-14.0.2002.14-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-fts-14.0.2002.14-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.2002.14-1 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-14.0.2002.14-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-ha-14.0.2002.14-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-fts-14.0.2002.14-1.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.2002.14-1 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server/mssql-server_14.0.2002.14-1_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.2002.14-1_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.2002.14-1_amd64.deb) |

## <a name="cu9-jul-2018"></a><a id="CU9"></a> CU9 (Juli 2018)

Dies ist das CU9-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3030.27. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4341265](https://support.microsoft.com/help/4341265).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3030.27-1 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3030.27-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3030.27-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3030.27-1.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3030.27-1 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3030.27-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3030.27-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3030.27-1.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3030.27-1 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3030.27-1_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3030.27-1_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3030.27-1_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu8-jun-2018"></a><a id="CU8"></a> CU8 (Juni 2018)

Dies ist das CU8-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3029.16. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4338363](https://support.microsoft.com/help/4338363).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3029.16-1 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3029.16-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3029.16-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3029.16-1.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3029.16-1 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3029.16-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3029.16-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3029.16-1.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3029.16-1 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3029.16-1_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3029.16-1_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3029.16-1_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu7-may-2018"></a><a id="CU7"></a> CU7 (Mai 2018)

Dies ist das CU7-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3026.27. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4229789](https://support.microsoft.com/help/4229789).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3026.27-2 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3026.27-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3026.27-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3026.27-2.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3026.27-2 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3026.27-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3026.27-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3026.27-2.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3026.27-2 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3026.27-2_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3026.27-2_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3026.27-2_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu6-apr-2018"></a><a id="CU6"></a> CU6 (April 2018)

Dies ist das CU6-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3025.34. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4101464](https://support.microsoft.com/help/4101464).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3025.34-3 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3025.34-3.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3025.34-3.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3025.34-3.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3025.34-3 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3025.34-3.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3025.34-3.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3025.34-3.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3025.34-3 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3025.34-3_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3025.34-3_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3025.34-3_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu5-mar-2018"></a><a id="CU5"></a> CU5 (März 2018)

Dies ist das CU5-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3023.8. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4092643](https://support.microsoft.com/help/4092643).

### <a name="known-upgrade-issue"></a>Bekanntes Upgradeproblem

Wenn Sie ein Upgrade für ein vorheriges Release auf CU5 vornehmen, kann [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mit der folgenden Fehlermeldung fehlschlagen:

```
Error: 4860, Severity: 16, State: 1.
Cannot bulk load. The file "C:\Install\SqlTraceCollect.dtsx" does not exist or you don't have file access rights.
Error: 912, Severity: 21, State: 2.
Script level upgrade for database 'master' failed because upgrade step 'msdb110_upgrade.sql' encountered error 200, state
```

Verwenden Sie die folgenden Befehle zum Aktivieren des SQL Server-Agents und Neustarten von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], um diesen Fehler zu beheben:

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true
sudo systemctl start mssql-server
```

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3023.8-5 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3023.8-5.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3023.8-5.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3023.8-5.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3023.8-5 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3023.8-5.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3023.8-5.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3023.8-5.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3023.8-5 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3023.8-5_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3023.8-5_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3023.8-5_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu4-feb-2018"></a><a id="CU4"></a> CU4 (Februar 2018)

Dies ist das CU4-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3022.28. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4056498](https://support.microsoft.com/help/4056498).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

> [!NOTE]
> Ab CU4 wird der SQL Server-Agent nicht mehr als separates Paket installiert. Er ist nun im Engine-Paket enthalten und muss für die Verwendung aktiviert werden.

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3022.28-2 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3022.28-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3022.28-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3022.28-2.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3022.28-2 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3022.28-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3022.28-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3022.28-2.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3022.28-2 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3022.28-2_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3022.28-2_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3022.28-2_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="gdr1-jan-2018"></a><a id="GDR1"></a> GDR1 (Januar 2018)

Hierbei handelt es sich um ein Sicherheitsupdate, das nur die Sicherheitskorrekturen von GDR1 für [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] enthält. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.2000.63. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4057122](https://support.microsoft.com/help/4057122).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.2000.63-3 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-14.0.2000.63-3.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-ha-14.0.2000.63-3.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-fts-14.0.2000.63-3.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.2000.63-3 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-14.0.2000.63-3.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-ha-14.0.2000.63-3.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-fts-14.0.2000.63-3.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.2000.63-3 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server/mssql-server_14.0.2000.63-3_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.2000.63-3_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.2000.63-3_amd64.deb) |

## <a name="cu3-jan-2018"></a><a id="CU3"></a> CU3 (Januar 2018)

Dies ist das CU3-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3015.40. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4052987](https://support.microsoft.com/help/4052987).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3015.40-1 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3015.40-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3015.40-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3015.40-1.x86_64.rpm)</br>[RPM-SQL Server-Agent-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3015.40-1.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3015.40-1 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3015.40-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3015.40-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3015.40-1.x86_64.rpm)</br>[RPM-SQL Server-Agent-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3015.40-1.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3015.40-1 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3015.40-1_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3015.40-1_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3015.40-1_amd64.deb)</br>[Debian-SQL Server-Agent-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3015.40-1_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu2-nov-2017"></a><a id="CU2"></a> CU2 (November 2017)

Dies ist das CU2-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3008.27. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/4052574](https://support.microsoft.com/help/4052574).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3008.27-1 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3008.27-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3008.27-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3008.27-1.x86_64.rpm)</br>[RPM-SQL Server-Agent-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3008.27-1.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3008.27-1 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3008.27-1.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3008.27-1.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3008.27-1.x86_64.rpm)</br>[RPM-SQL Server-Agent-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3008.27-1.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3008.27-1 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3008.27-1_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3008.27-1_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3008.27-1_amd64.deb)</br>[Debian-SQL Server-Agent-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3008.27-1_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu1-oct-2017"></a><a id="CU1"></a> CU1 (Oktober 2017)

Dies ist das CU1-Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.3006.16. Informationen zu den Fehlerbehebungen und Verbesserungen in diesem Release finden Sie unter [https://support.microsoft.com/help/KB4053439](https://support.microsoft.com/help/4038634).

### <a name="package-details"></a>Paketdetails

Für manuelle oder offline durchgeführte Paketinstallationen können Sie die RPM- und Debian-Pakete herunterladen, die in der folgenden Tabelle aufgeführt werden:

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.3006.16-3 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3006.16-3.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3006.16-3.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3006.16-3.x86_64.rpm)</br>[RPM-SQL Server-Agent-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3006.16-3.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.3006.16-3 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3006.16-3.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3006.16-3.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3006.16-3.x86_64.rpm)</br>[RPM-SQL Server-Agent-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3006.16-3.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.3006.16-3 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3006.16-3_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3006.16-3_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3006.16-3_amd64.deb)</br>[Debian-SQL Server-Agent-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3006.16-3_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="ga-oct-2017"></a><a id="GA"></a> Allgemein verfügbares Release (Oct 2017)

Dies ist das allgemein verfügbare Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Version für dieses Release lautet: 14.0.1000.169.

### <a name="package-details"></a>Paketdetails

In der folgenden Tabelle werden Paketinformationen und Downloadlinks für die RPM- und Debian-Pakete aufgeführt. Sie müssen diese Pakete nicht direkt herunterladen, wenn Sie die Schritte der folgenden Installationshandbücher befolgen:

- [Installieren des SQL Server-Pakets](sql-server-linux-setup.md)
- [Installieren des Volltextsuchepakets](sql-server-linux-setup-full-text-search.md)
- [Install SQL Server Agent package (Installieren des SQL Server-Agent-Pakets)](sql-server-linux-setup-sql-agent.md)
- [Install SQL Server Integration Services (Installieren von SQL Server Integration Services)](sql-server-linux-setup-ssis.md)

| Paket | Paketversion | Downloads |
|-----|-----|-----|
| Red Hat RPM-Paket | 14.0.1000.169-2 | [RPM-Engine-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.1000.169-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.1000.169-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.1000.169-2.x86_64.rpm)</br>[RPM-SQL Server-Agent-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.1000.169-2.x86_64.rpm)</br>[SSIS-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM-Paket | 14.0.1000.169-2 | [RPM-Engine-Paket (mssql-server)](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.1000.169-2.x86_64.rpm)</br>[RPM-Hochverfügbarkeitspaket](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.1000.169-2.x86_64.rpm)</br>[RPM-Paket für Volltextsuche](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.1000.169-2.x86_64.rpm)</br>[RPM-SQL Server-Agent-Paket](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.1000.169-2.x86_64.rpm) | 
| Debian-Paket für Ubuntu 16.04 | 14.0.1000.169-2 | [Debian-Engine-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.1000.169-2_amd64.deb)</br>[Debian-Hochverfügbarkeitspaket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.1000.169-2_amd64.deb)</br>[Debian-Paket für Volltextsuche](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.1000.169-2_amd64.deb)</br>[Debian-SQL Server-Agent-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.1000.169-2_amd64.deb)<br/>[SSIS-Paket](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="known-issues"></a>Bekannte Probleme

In den folgenden Abschnitten werden bekannte Probleme mit dem allgemein verfügbaren Release von [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] für Linux beschrieben.

#### <a name="general"></a>Allgemein

- Der Name des Hosts, auf dem [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] installiert ist, darf maximal 15 Zeichen lang sein. 

    - **Lösung:** Ändern Sie den Namen unter /etc/hostname in einen Namen, der maximal 15 Zeichen enthält.

- Wenn Sie die Systemzeit manuell auf einen früheren Zeitpunkt festlegen, aktualisiert [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] die interne Systemzeit in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nicht mehr.

    - **Lösung:** Starten Sie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] neu.

- Nur Installationen mit einzelner Instanz werden unterstützt.

    - **Lösung:** Wenn Sie mehrere Instanzen auf einem bestimmten Host verwenden möchten, ziehen Sie die Verwendung von VMs (virtuelle Computer) oder Docker-Containern in Betracht. 

- Der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Konfigurations-Manager kann keine Verbindung mit [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für Linux herstellen.

- Die Standardsprache der **sa**-Anmeldung ist Englisch.

    - **Lösung:** Ändern Sie die Sprache für die **sa**-Anmeldung mithilfe der Anweisung **ALTER LOGIN**.

#### <a name="databases"></a>Datenbanken

- Die Masterdatenbank kann nicht mit dem Hilfsprogramm „mssql-conf“ verschoben werden. Andere Systemdatenbanken können mit „mssql-conf“ verschoben werden.

- Beim Wiederherstellen einer Datenbank, die in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für Windows gesichert wurde, müssen Sie die Klausel **WITH MOVE** in der Transact-SQL-Anweisung verwenden.

- Gewisse Algorithmen (Suites mit Verschlüsselungsverfahren) für TLS (Transport Layer Security) funktionieren in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für Linux nicht ordnungsgemäß. Dies führt zu Verbindungsfehlern beim Herstellen einer Verbindung mit [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sowie beim Herstellen von Verbindungen zwischen Replikaten in Hochverfügbarkeitsgruppen.

   - **Lösung:** Passen Sie das Konfigurationsskript **mssql.conf** für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für Linux an, um problematische Suites mit Verschlüsselungsverfahren zu deaktivieren, indem Sie wie folgt vorgehen:

      1. Fügen Sie Folgendes unter „/var/opt/mssql/mssql.conf“ hinzu.

         ```
         [network]
         tlsciphers= AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:!ECDHE-RSA-AES128-GCM-SHA256:!ECDHE-RSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES128-GCM-SHA256:!ECDHE-ECDSA-AES256-SHA384:!ECDHE-ECDSA-AES128-SHA256:!ECDHE-ECDSA-AES256-SHA:!ECDHE-ECDSA-AES128-SHA:!ECDHE-RSA-AES256-SHA384:!ECDHE-RSA-AES128-SHA256:!ECDHE-RSA-AES256-SHA:!ECDHE-RSA-AES128-SHA:!DHE-RSA-AES256-GCM-SHA384:!DHE-RSA-AES128-GCM-SHA256:!DHE-RSA-AES256-SHA:!DHE-RSA-AES128-SHA:!DHE-DSS-AES256-SHA256:!DHE-DSS-AES128-SHA256:!DHE-DSS-AES256-SHA:!DHE-DSS-AES128-SHA:!DHE-DSS-DES-CBC3-SHA:!NULL-SHA256:!NULL-SHA
         ```

         > [!NOTE]
         > Im vorangehenden Code negiert `!` den Ausdruck. Damit wird OpenSSL mitgeteilt, dass die folgende Verschlüsselungssammlung nicht verwendet werden soll.  

      1. Starten Sie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mithilfe des folgenden Befehls neu.

         ```bash
         sudo systemctl restart mssql-server
         ```

- [!INCLUDE[ssSQL14](../includes/sssql14-md.md)]-Datenbanken unter Windows, die In-Memory-OLTP nutzen, können nicht in [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] für Linux wiederhergestellt werden. Führen Sie zunächst ein Upgrade auf [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] oder [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] unter Windows für die Datenbanken durch, und verschieben Sie sie anschließend per Backup/Wiederherstellung oder durch Trennen/Anfügen in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für Linux, um eine [!INCLUDE[ssSQL14](../includes/sssql14-md.md)]-Datenbank wiederherzustellen, die In-Memory-OLTP nutzt.

- Unter Linux wird die Benutzerberechtigung **ADMINISTER BULK OPERATIONS** zurzeit nicht unterstützt.

#### <a name="networking"></a>Netzwerk

Features, die ausgehende TCP-Verbindungen aus dem sqlservr-Prozess nutzen, z. B. Verbindungsserver oder Verfügbarkeitsgruppen, funktionieren möglicherweise nicht, wenn beide der folgenden Bedingungen erfüllt werden:

1. Der Zielserver wurde mit einem Hostnamen anstelle einer IP-Adresse angegeben.

1. IPv6 ist im Kernel der Quellinstanz deaktiviert. Alle der folgenden Tests müssen bestanden werden, um zu überprüfen, ob IPv6 im Kernel Ihres Systems aktiviert ist:

   - `cat /proc/cmdline` gibt die boot-Befehlszeile des aktuellen Kernels aus. Die Ausgabe darf `ipv6.disable=1` nicht enthalten.
   - Das Verzeichnis „/proc/sys/net/ipv6/“ muss vorhanden sein.
   - Ein C-Programm, das `socket(AF_INET6, SOCK_STREAM, IPPROTO_IP)` aufruft, sollte erfolgreich sein – der Systemaufruf muss „fd ! = -1“ zurückgeben und darf nicht mit EAFNOSUPPORT fehlschlagen.

Der genaue Fehler hängt vom Feature ab. Bei Verbindungsservern zeigt sich dieser Fehler in Form eines Anmeldungstimeouts. Bei Verfügbarkeitsgruppen schlägt die `ALTER AVAILABILITY GROUP JOIN`-DDL auf dem sekundären Replikat nach 5 Minuten mit einem Timeout beim Download der Konfiguration fehl.

Führen Sie einen der folgenden Schritte aus, um dieses Problem zu umgehen:

1. Verwenden Sie IP-Adressen anstelle von Hostnamen, um das Ziel der TCP-Verbindung anzugeben.

1. Aktivieren Sie IPv6 im Kernel, indem Sie `ipv6.disable=1` aus der boot-Befehlszeile entfernen. Die Vorgehensweise hierfür hängt von der Linux-Verteilung und dem Bootloader (z. B. GRUB) ab. Wenn Sie IPv6 deaktivieren möchten, können Sie dennoch deaktivieren, indem Sie `net.ipv6.conf.all.disable_ipv6 = 1` in der `sysctl`-Konfiguration festlegen (z.B. `/etc/sysctl.conf`). Dadurch wird weiterhin verhindert, dass der Netzwerkadapter des Systems eine IPv6-Adresse erhält, und ermöglicht dennoch die Funktion der sqlservr-Features.

#### <a name="network-file-system-nfs"></a>Network File System (NFS)
Wenn Sie **NFS-Remotefreigaben (Network File System)** in der Produktion verwenden, beachten Sie die folgenden Anforderungen für die Unterstützung:

- Verwenden Sie NFS in der Version **4.2 oder höher**. Ältere NFS-Versionen unterstützen nicht die erforderlichen Features, wie das Erstellen von Fallocate- oder Sparsedateien, die häufig in modernen Dateisystemen enthalten sind.
- Verwenden Sie nur die Verzeichnisse vom Typ **/var/opt/mssql** in der NFS-Bereitstellung. Andere Dateien, wie z. B. die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Systembinärdateien, werden nicht unterstützt.
- Stellen Sie sicher, dass NFS-Clients beim Einbinden der Remotefreigabe die Option „NOLOCK“ verwenden.

#### <a name="localization"></a>Lokalisierung

- Wenn Ihr Gebietsschema während des Setups nicht Englisch (en_us) ist, müssen Sie die UTF-8-Codierung in Ihrer Bash-Sitzung bzw. Ihrem Terminal verwenden. Wenn Sie die ASCII-Codierung verwenden, kann ein Fehler ähnlich dem Folgenden auftreten:

   ```
   UnicodeEncodeError: 'ascii' codec can't encode character u'\xf1' in position 8: ordinal not in range(128)
   ```

   Wenn Sie die UTF-8-Codierung nicht verwenden können, führen Sie das Setup mit der Umgebungsvariable MSSQL_LCID aus, um Ihre Sprachauswahl festzulegen.

   ```bash
   sudo MSSQL_LCID=<LcidValue> /opt/mssql/bin/mssql-conf setup
   ```

- Beim Ausführen des Befehls „mssql-conf setup“ und Durchführen einer nicht-englischen Installation von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], werden falsche erweiterte Zeichen nach dem lokalisierten Text „Configuring SQL Server...“ (SQL Server wird konfiguriert...) angezeigt. Bei Installationen basierend auf Sprachen mit nicht-lateinischen Zeichen kann der Text vollständig fehlen. Der fehlende Text sollte die folgende lokalisierte Zeichenfolge darstellen: „Die Lizenzierungs-PID wurde erfolgreich verarbeitet. Die neue Edition ist „[\<Name\>-Edition]“. Diese Zeichenfolge wird nur zu Informationszwecken ausgegeben, und das nächste kumulative Update für [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] wird dieses Problem für alle Sprachen beheben. Dies wirkt sich nicht auf die erfolgreiche Installation von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] aus. 

#### <a name="full-text-search"></a>Volltextsuche

- In diesem Release sind nicht alle Filter verfügbar, einschließlich Filter für Office-Dokumente. Eine Liste aller unterstützten Filter finden Sie unter [Install SQL Server Full-Text Search on Linux (Installieren der SQL Server-Volltextsuche unter Linux)](sql-server-linux-setup-full-text-search.md#filters).

#### <a name="sql-server-integration-services-ssis"></a><a id="ssis"></a> SQL Server Integration Services (SSIS)

- Das Paket **mssql-server-is** wird in diesem Release nicht für SUSE unterstützt. Es wird derzeit für Ubuntu und Red Hat Enterprise Linux (RHEL) unterstützt.

- Mit [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] unter Linux CTP 2.1 Refresh und höher können [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]-Pakete ODBC-Verbindungen unter Linux verwenden. Diese Funktion wurde mit den Treibern von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] und MySQL ODBC getestet, sollte jedoch auch mit einem Unicode-Treiber verwendet werden können, der der ODBC-Spezifikation entspricht. Zur Entwurfszeit können Sie entweder einen DSN oder eine Verbindungszeichenfolge angeben, um eine Verbindung mit den ODBC-Daten herzustellen. Sie können aber auch die Windows-Authentifizierung verwenden. Weitere Informationen hierzu finden Sie im [Blogbeitrag mit der Ankündigung zur ODBC-Unterstützung unter Linux](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/).

- Die folgenden Features werden in diesem Release nicht unterstützt, wenn Sie SSIS-Pakete unter Linux ausführen:
  - [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]-Katalogdatenbank
  - Geplante Paketausführung des SQL Agent
  - Windows-Authentifizierung
  - Drittanbieterkomponenten
  - Change Data Capture (CDC)
  - [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] Scale Out
  - Azure Feature Pack für SSIS
  - Hadoop- und HDFS-Unterstützung
  - Microsoft Connector for SAP BW

Eine Liste der integrierten SSIS-Komponenten, die derzeit nicht unterstützt oder nur bedingt unterstützt werden, finden Sie unter [Limitations and known issues for SSIS on Linux (Einschränkungen und bekannte Probleme für SSIS unter Linux)](sql-server-linux-ssis-known-issues.md#components).

Weitere Informationen zu SSIS unter Linux finden Sie in den folgenden Artikeln:
-   [Blog post announcing SSIS support for Linux (Blogbeitrag mit Ankündigung der SSIS-Unterstützung für Linux)](https://blogs.msdn.microsoft.com/ssis/2017/05/17/ssis-helsinki-is-available-in-sql-server-vnext-ctp2-1/).
-   [Installieren von SSIS (SQL Server Integration Services) unter Linux](sql-server-linux-setup-ssis.md)
-   [Extrahieren, Transformieren und Laden von Daten unter Linux mit SSIS](sql-server-linux-migrate-ssis.md)

#### <a name="sql-server-management-studio-ssms"></a><a id="ssms"></a> SQL Server Management Studio (SSMS)

Die folgenden Einschränkungen gelten für [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] unter Windows mit Verbindung zu [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für Linux.

- Wartungspläne werden nicht unterstützt.

- Verwaltungs-Data Warehouse und der Datensammler in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] werden nicht unterstützt. 

- Benutzeroberflächenkomponenten von [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], die über Optionen für die Windows-Authentifizierung oder für Windows-Ereignisprotokolle verfügen, funktionieren unter Linux nicht. Sie können diese Features mit anderen Optionen (z. B. SQL-Anmeldungen) weiterhin verwenden. 

- Die Anzahl beizubehaltender Protokolldateien kann nicht geändert werden.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Schnellstarts finden Sie Informationen zu den ersten Schritten:

- [Installation unter Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Installation unter SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Installation unter Ubuntu](quickstart-install-connect-ubuntu.md)
- [Ausführung in Docker](quickstart-install-connect-docker.md)
- [Provision a SQL VM in Azure (Bereitstellen einer SQL-VM in Azure)](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)
- [Ausführen und Verbinden – Cloud](quickstart-install-connect-clouds.md)

Antworten auf häufig gestellte Fragen finden Sie unter [Häufig gestellte Fragen zu SQL Server für Linux](sql-server-linux-faq.md).
