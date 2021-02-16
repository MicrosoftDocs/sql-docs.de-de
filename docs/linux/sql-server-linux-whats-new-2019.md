---
title: Neuigkeiten zu SQL Server 2019 für Linux
description: In diesem Artikel sind die Neuigkeiten für SQL Server 2019 für Linux zusammengestellt.
author: VanMSFT
ms.author: vanto
ms.date: 03/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: d6978bfa68d3a9b922e46454481618e448e91b96
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346203"
---
# <a name="whats-new-for-sql-server-2019-on-linux"></a>Neuigkeiten zu SQL Server 2019 für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel sind die wichtigsten Features und Dienste beschrieben, die für SQL Server 2019 für Linux verfügbar sind. Informationen über Paketdownloads und bekannte Probleme finden Sie unter [Versionshinweise](sql-server-linux-release-notes-2019.md).

## <a name="ubuntu-1804-supported"></a>Ubuntu 18.04-Unterstützung

Ubuntu 18.04 wird ab SQL Server 2019 CU3 unterstützt. Weitere Informationen finden Sie im Schnellstart unter [Installieren von SQL Server und Erstellen einer Datenbank in Ubuntu](quickstart-install-connect-ubuntu.md?view=sql-server-linux-ver15&preserve-view=true).

## <a name="rhel-8-supported"></a>RHEL 8-Unterstützung

RHEL 8 wird ab SQL Server 2019 CU1 unterstützt. Weitere Informationen finden Sie im Schnellstart unter [Installieren von SQL Server und Erstellen einer Datenbank in Red Hat](quickstart-install-connect-red-hat.md?view=sql-server-linux-ver15&preserve-view=true).

## <a name="updates"></a>Aktualisierungen

Die Updates wurden in SQL Server 2019 unter Linux vorgenommen:

| Neue Funktion oder Update | Details |
|:-----|:-----|
|Replikationsunterstützung |[SQL Server Replication on Linux (SQL Server-Replikation unter Linux)](sql-server-linux-replication.md)
|Unterstützung für den Microsoft Distributed Transaction Coordinator (MSDTC) |[How to configure MSDTC on Linux (Vorgehensweise: Konfigurieren von MSDTC unter Linux)](sql-server-linux-configure-msdtc.md) |
|OpenLDAP-Unterstützung für AD-Drittanbieter |[Tutorial: Use Active Directory authentication with SQL Server on Linux (Tutorial: Verwenden der Azure Active Directory-Authentifizierung mit SQL Server unter Linux)](sql-server-linux-active-directory-authentication.md) |
|Machine Learning unter Linux |[Configure Machine Learning on Linux (Konfigurieren von Machine Learning unter Linux)](sql-server-linux-setup-machine-learning.md) |
|`tempdb`-Verbesserungen | Standardmäßig erstellt eine neue Installation von SQL Server für Linux mehrere `tempdb`-Datendateien, deren Anzahl sich nach der Anzahl von logischen Kernen richtet (bis zu 8 Datendateien). Das gilt nicht für direkte Upgrades der Neben- oder Hauptversion. Jede `tempdb`-Datei ist 8 MB groß und wird automatisch um 64 MB vergrößert. Dieses Verhalten ähnelt dem der SQL Server-Standardinstallation unter Windows. |
| PolyBase unter Linux | [Installieren Sie PolyBase](../relational-databases/polybase/polybase-linux-setup.md) für Nicht-Hadoop-Connectors unter Linux.<br/><br/>[PolyBase type mapping (Typzuordnung mit PolyBase)](../relational-databases/polybase/polybase-type-mapping.md) |
| Unterstützung von Change Data Capture (CDC) | Change Data Capture (CDC) wird jetzt unter Linux für SQL Server 2019 unterstützt. |
| Microsoft Container Registry | [Microsoft Container Registry](https://azure.microsoft.com/blog/microsoft-syndicates-container-catalog/) ersetzt nun Docker Hub für neue offizielle Microsoft-Containerimages, einschließlich [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]. |
| Container ohne Root-Berechtigung | Mit [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] wird die Möglichkeit eingeführt, sicherere Container zu erstellen, indem der [!INCLUDE[sql-server](../includes/ssnoversion-md.md)]-Prozess standardmäßig als Nicht-Root-Benutzer gestartet wird. Weitere Informationen finden Sie unter [Erstellen und Ausführen von SQL Server-Containern als Nicht-Root-Benutzer](./sql-server-linux-docker-container-security.md#buildnonrootcontainer). |

## <a name="next-steps"></a>Nächste Schritte

Um SQL Server für Linux zu installieren, gehen Sie gemäß einem der folgenden Tutorials vor:

- [Installation unter Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md?view=sql-server-linux-ver15&preserve-view=true)
- [Installation unter SUSE Linux Enterprise Server](quickstart-install-connect-suse.md?view=sql-server-linux-ver15&preserve-view=true)
- [Installation unter Ubuntu](quickstart-install-connect-ubuntu.md?view=sql-server-linux-ver15&preserve-view=true)
- [Ausführung in Docker](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true)
- [Provision a SQL VM in Azure (Bereitstellen einer SQL-VM in Azure)](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)

Antworten auf häufig gestellte Fragen finden Sie unter [Häufig gestellte Fragen zu SQL Server für Linux](sql-server-linux-faq.md). Weitere Verbesserungen, die in SQL Server 2019 eingeführt wurden, finden Sie unter [Neues in SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true).

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]