---
title: Unterstützte Versions- und Editionsupgrades in SQL Server 2016
description: In diesem Artikel finden Sie Informationen zu den unterstützten Versions- und Editionsupgrades für SQL Server 2016.
ms.custom: ''
ms.date: 06/27/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- components [SQL Server], adding to existing installations
- versions [SQL Server], upgrading
- upgrading SQL Server, upgrades supported
- cross-language support
ms.assetid: 702359c4-6ca9-42a8-860c-a95a802898a1
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 591dc50594dbd7ed321188de0a6892e7adf125a8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341791"
---
# <a name="supported-version--edition-upgrades-sql-server-2016"></a>Unterstützte Versions- und Editionsupgrades in SQL Server 2016

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  Sie können ein Upgrade von [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]und [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]ausführen. In diesem Artikel werden die unterstützten Upgradepfade von diesen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Versionen sowie die unterstützten Editionsupgrades für [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] aufgeführt.  
  
## <a name="pre-upgrade-checklist"></a>Prüfliste vor dem Upgrade  
  
-   Bevor eine Edition von [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] auf eine andere Edition aktualisiert wird, sollten Sie überprüfen, ob die derzeit verwendete Funktionalität in der Edition, die Ziel des Upgrades ist, unterstützt wird.  
  
-   Aktivieren Sie vor dem Upgrade auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]die Windows-Authentifizierung für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent, und überprüfen Sie die erforderliche Standardkonfiguration. Dabei muss das Konto des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Diensts Mitglied der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Gruppe der Systemadministratoren sein.  
  
-   Um auf [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]aktualisieren zu können, müssen Sie ein unterstütztes Betriebssystem ausführen. Weitere Informationen finden Sie unter [Hardware- und Softwareanforderungen für die Installation von SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
-   Das Upgrade wird blockiert, wenn noch ein Neustart aussteht.  
  
-   Das Upgrade wird blockiert, wenn der Windows Installer-Dienst nicht ausgeführt wird.  
  
## <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien  
  
-   Versionsübergreifende Instanzen von [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] werden nicht unterstützt. Die Versionsnummern der [!INCLUDE[ssDE](../../includes/ssde-md.md)]-, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]- und [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Komponenten innerhalb einer Instanz von [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]müssen identisch sein.  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] ist nur für 64-Bit-Plattformen verfügbar. Ein plattformübergreifendes Upgrade wird nicht unterstützt. Sie können keine 32-Bit-Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup auf systemeigenes 64-Bit aktualisieren. Sie können jedoch Datenbanken von einer 32-Bit-Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]sichern oder trennen und sie in einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (64-Bit) wiederherstellen oder anfügen, wenn die Datenbanken nicht in der Replikation veröffentlicht sind. Sie müssen alle Anmeldenamen und anderen Benutzerobjekte in den Systemdatenbanken „master“, „msdb“ und „model“ wiederherstellen.  
  
-   Sie können während des Upgrades der vorhandenen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]keine neuen Funktionen hinzufügen. Nachdem Sie eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] aktualisiert haben, können Sie Funktionen mit dem [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]-Setup hinzufügen. Weitere Informationen finden Sie unter [Hinzufügen von Funktionen zu einer Instanz von SQL Server 2016 &#40;Setup&#41;](./add-features-to-an-instance-of-sql-server-setup.md).  
 
-   Failovercluster werden im WOW-Modus nicht unterstützt.  
  
-   Upgrades von einer Evaluation Edition einer früheren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Version werden nicht unterstützt.

-   Wenn Sie ein Upgrade von RC1 oder früheren Versionen von SQL Server 2016 auf RC3 oder höher durchführen, muss PolyBase vor dem Upgrade deinstalliert und danach wieder installiert werden.
  
## <a name="upgrades-from-earlier-versions-to-sssql15-md"></a>Upgrades von früheren Versionen auf [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]  
 
SQL Server 2016 unterstützt ein Upgrade von folgenden Versionen von SQL Server:
 
- [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] SP4 oder höher
- [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] SP3 oder höher
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 oder höher
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] oder höher 
 
> [!NOTE]  
> Informationen zum Aktualisieren von Datenbanken aus [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] finden Sie unter [SQL Server 2016-Unterstützung für SQL Server 2005](#SupportFor2005).  
  
In der nachfolgenden Tabelle sind die unterstützten Szenarien für das Upgrade von früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]aufgeführt.  
  
|Upgrade von|Unterstützter Upgradepfad|  
|------------------|----------------------------|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Enterprise|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Developer|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Entwickler|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Standard|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Small Business|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Workgroup|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Express |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Datacenter|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Enterprise|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Developer|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Entwickler|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Small Business|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Standard|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Workgroup|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Express |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Enterprise|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Developer|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Entwickler <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Standard|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Express |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express <br/> <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Business Intelligence|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Evaluation|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Entwickler|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Enterprise|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Entwickler|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Entwickler <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Standard|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Express |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Entwickler|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Business Intelligence|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Evaluation|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Entwickler|  
|[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] Release Candidate* |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise |  
|[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] Entwickler |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise | 

 \* Microsoft-Unterstützung zum Aktualisieren von Release Candidate-Software ist speziell für Kunden vorgesehen, die am Technology Adoption Program (TAP) teilgenommen haben. 
   
###  <a name="sssql15-md-support-for-ssversion2005"></a><a name="SupportFor2005"></a> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]-Unterstützung für [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]  
In diesem Abschnitt wird die [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] -Unterstützung für [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]erläutert. In [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]können Sie die folgenden Schritte ausführen:  
  
-   Anfügen einer [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]-Datenbank (MDF-/LDF-Dateien) an eine [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]-Instanz der Datenbank-Engine.  
  
-   Wiederherstellen einer [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]-Datenbank auf einer [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]-Instanz der Datenbank-Engine mithilfe einer Sicherung.  
  
-   Sichern eines [!INCLUDE[ssASversion2005](../../includes/ssasversion2005-md.md)] -Cubes und Wiederherstellen für [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].  
  
> [!NOTE]  
> Wenn für eine [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]-Datenbank ein Upgrade auf [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] ausgeführt wird, ändern sich der Datenbank-Kompatibilitätsgrad von 90 in 100. In [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] sind 100, 110, 120 und 130 gültige Werte für den Kompatibilitätsgrad der Datenbank. In [ALTER DATABASE Kompatibilitätsgrad &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) wird erläutert, wie sich eine Änderung des Datenbank-Kompatibilitätsgrads auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anwendungen auswirken kann.  
  
Szenarien, die in der Liste nicht aufgeführt sind, werden nicht unterstützt. Das gilt z. B. für:  
  
-   Installieren von [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] und [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] auf demselben Computer (parallel).  
  
-   Verwenden einer [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] -Instanz als Mitglied der Replikationstopologie, die eine [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] -Instanz umfasst.  
  
-   Konfigurieren der Datenbankspiegelung zwischen [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] - und [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] -Instanzen.  
  
-   Sichern des Transaktionsprotokolls mit Protokollversand zwischen [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] - und [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] -Instanzen.  
  
-   Konfigurieren von Verbindungsservern zwischen [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] - und [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] -Instanzen.  
  
-   Verwalten einer [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] -Instanz von [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Management Studio aus.  
  
-   Anfügen eines [!INCLUDE[ssASversion2005](../../includes/ssasversion2005-md.md)] -Cubes in [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Management Studio.  
  
-   Herstellen einer Verbindung mit [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] von [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Management Studio aus.  
  
-   Verwalten eines [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] -Diensts von [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Management Studio aus.  
  
-   Unterstützung von benutzerdefinierten Integration Services-Drittanbieterkomponenten für [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] sowie Ausführen und Upgrade der Komponenten.  
  
## <a name="sssql15-md-edition-upgrade"></a>[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Editionsupgrade  
In der folgenden Tabelle sind die unterstützten Szenarien für das Editionsupgrade in [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]aufgeführt.  
  
Schrittweise Anweisungen zum Ausführen eines Editionsupgrades finden Sie unter [Aktualisieren auf eine andere Edition von SQL Server 2016 &#40;Setup&#41;](../../database-engine/install-windows/upgrade-to-a-different-edition-of-sql-server-setup.md).  
  
|Upgrade von|Upgrade auf|  
|------------------|----------------|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (Server+CAL und Core)**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise |  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise Evaluation**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (Server+CAL- oder Core-Lizenz) <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Entwickler <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> Upgrades von Evaluation (kostenlose Edition) auf eine kostenpflichtige Edition werden für eigenständige Installationen, für gruppierte Installationen jedoch nicht unterstützt. Diese Begrenzung gilt nicht für eigenständige Instanzen, die auf einem Failovercluster von Windows installiert sind, der Mitglied einer Verfügbarkeitsgruppe ist.|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (Server+CAL- oder Core-Lizenz)|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (Server+CAL- oder Core-Lizenz) <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (Server+CAL- oder Core-Lizenz) <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express*|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (Server+CAL- oder Core-Lizenz) <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Entwickler <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
  
Außerdem können Sie ein Editionsupgrade zwischen [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (Server+CAL-Lizenz) und [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (Core-Lizenz) ausführen:  
  
|Editionsupgrade von|Editionsupgrade auf|  
|--------------------------|------------------------|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (Server+CAL-Lizenz)**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (Core-Lizenz)|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (Core-Lizenz)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (Server+CAL-Lizenz)|  
  
 \* Gilt auch für [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express with Tools und [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express with Advanced Services.  
  
 ** Das Ändern der Edition eines [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] -Failoverclusters ist nur beschränkt möglich. Die folgenden Szenarien werden bei [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] -Failoverclustern nicht unterstützt:  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise in [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer, Standard oder Evaluation.  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer in [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard oder Evaluation.  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard in [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation.  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation in [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard.  
  
## <a name="see-also"></a>Weitere Informationen  

[Editionen und unterstützten Funktionen von SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)     
[Hardware- und Softwareanforderungen für die Installation von SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)     
[Aktualisieren auf SQL Server 2016](../../database-engine/install-windows/upgrade-sql-server.md)    
  
