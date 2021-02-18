---
title: 'SQL Server 2019: Hardware- und Softwareanforderungen'
description: Eine Liste der Hardware-, Software- und Betriebssystemanforderungen für die Installation und Ausführung von SQL Server 2019
ms.custom: sqlfreshmay19
ms.date: 09/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- Setup [SQL Server], software
- software [SQL Server]
- installing SQL Server, software
- operating systems [SQL Server], SQL Server requirements
- Setup [SQL Server], cross-language support
- operating systems [SQL Server], cross-language support
- network connections [SQL Server], requirements
- disk space [SQL Server], SQL Server installations
- WOW [SQL Server]
- Setup [SQL Server], hardware
- dependencies [SQL Server], SQL Server installations
- cluster hardware requirements [SQL Server]
- endpoints [SQL Server], SQL Server installations
- Internet [SQL Server], SQL Server installations
- hardware [SQL Server]
- Windows on Windows [SQL Server]
- installing SQL Server, hardware
- Setup Configuration Checker
- SCC [SQL Server]
- operating systems [SQL Server]
- space [SQL Server], SQL Server installations
- system configuration checker
- installing SQL Server, cross-language support
- Internet [SQL Server]
- space [SQL Server]
- extended system support [SQL Server]
- 64-bit edition [SQL Server]
- failover clustering [SQL Server]
- failover clustering [SQL Server], hardware requirements
- 32-bit edition [SQL Server]
- locales [SQL Server], SQL Server installations
- cross-language support
- disk space [SQL Server]
- localized SQL Server versions
ms.assetid: 09bcf20b-0a40-4131-907f-b61479d5e4d8
ms.author: chadam
author: cawrites
ms.openlocfilehash: 52c29a7de29a324d7bda2fba4cb52073cf62d752
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352431"
---
# <a name="sql-server-2019-hardware-and-software-requirements"></a>SQL Server 2019: Hardware- und Softwareanforderungen
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

In diesem Artikel sind die Mindestanforderungen an die Hardware und Software aufgeführt, um SQL Server 2019 auf dem Windows-Betriebssystem zu installieren und auszuführen.

Die Hardware- und Softwareanforderungen für andere Versionen von SQL Server finden Sie unter:
- [SQL Server 2016 und 2017](hardware-and-software-requirements-for-installing-sql-server.md)
- [SQL Server unter Linux](../../linux/sql-server-linux-setup.md#system)
- [Big Data-Cluster](../../big-data-cluster/deployment-guidance.md)

##  <a name="hardware-requirements"></a><a name="pmosr"></a> Hardwareanforderungen  
 Der folgende Arbeitsspeicher- und Prozessoranforderungen gelten für alle Editionen von [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]:  
  
|Komponente|Anforderung|  
|---------------|-----------------|  
|Festplatte|[!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] erfordert mindestens 6 GB verfügbaren Festplattenspeicher.<br/><br/> Der erforderliche freie Festplattenspeicher ist von den installierten [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] -Komponenten abhängig. Weitere Informationen finden Sie unter [Anforderungen an den Festplattenspeicherplatz](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#HardDiskSpace) weiter unten in diesem Artikel. Informationen zu unterstützten Speichertypen für Datendateien finden Sie unter [Speichertypen für Datendateien](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#StorageTypes).|  
|Überwachen|[!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] erfordert eine Super VGA-Grafikkarte mit einer Mindestauflösung von 800x600 Pixel.|  
|Internet|Zur Nutzung des Internets ist ein Internetzugang erforderlich (möglicherweise gebührenpflichtig).|  
|Arbeitsspeicher \*|**Minimum:**<br/><br/> Express-Editionen: 512 MB<br/><br/> Alle anderen Editionen: 1 GB<br/><br/> **Empfohlen.**<br/><br/> Express-Editionen: 1 GB<br/><br/> Alle anderen Editionen: Mindestens 4 GB. Mit zunehmender Datenbankgröße sollte außerdem der Speicher erhöht werden, um eine optimale Leistung sicherzustellen.|  
|Prozessorgeschwindigkeit:|**Mindestens:** x64-Prozessor: 1,4 GHz<br/><br/> **Empfohlen.** 2,0 GHz oder schneller|  
|Prozessortyp|x64-Prozessor: AMD Opteron, AMD Athlon 64, Intel Xeon mit Intel EM64T-Unterstützung, Intel Pentium IV mit EM64T-Unterstützung|  
  
> [!NOTE]  
> Die Installation von [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] wird nur auf x64-Prozessoren unterstützt. Es wird nicht mehr auf x86 Prozessoren unterstützt.  
  
 \* Zur Installation der [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]-Komponente in [!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)] (DQS) sind mindestens 2 GB RAM erforderlich. Dies unterscheidet sich von den Arbeitsspeichermindestanforderungen von [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]. Informationen zum Installieren von DQS finden Sie unter [Install Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md).  


##  <a name="software-requirements"></a><a name="hwswr"></a> Softwareanforderungen  

Die folgenden Anforderungen gelten für alle Installationen:  
  
|Komponente|Anforderung|  
|---------------|-----------------|  
|Betriebssystem|Windows 10 TH1 1507 oder höher<br/><br>Windows Server 2016 oder höher<br/><br/>
|.NET Framework|Die mindestens erforderlichen Betriebssysteme umfassen die mindestens erforderliche Version von .NET Framework.|  
|Netzwerksoftware|Die unterstützten Betriebssysteme für [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] verfügen über integrierte Netzwerksoftware. Benannte Instanzen und Standardinstanzen einer eigenständigen Installation unterstützen die folgenden Netzwerkprotokolle: Freigegebener Arbeitsspeicher, Named Pipes und TCP/IP<br/><br/> |  

Beim[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup werden die folgenden vom Produkt benötigten Softwarekomponenten installiert:  
  
   - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client    
   - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setuphilfsdateien  


> [!IMPORTANT]
> Es gibt zusätzliche Hardware- und Softwareanforderungen für die PolyBase-Funktion. Weitere Informationen finden Sie unter [Get started with PolyBase](../../relational-databases/polybase/polybase-guide.md)(Erste Schritte mit PolyBase).  
  

##  <a name="operating-system-support"></a><a name="TOP_Principal"></a> Betriebssystemunterstützung 

Die folgende Tabelle zeigt, welche Editionen von SQL Server 2019 mit welchen Windows-Versionen kompatibel sind:  
  

| SQL Server-Edition:               | Enterprise | Entwickler | Standard | Web | Express |  
| :------------------------         | :--------- | :-------- | :------- | :-- | :------ | 
| Windows Server 2019 Datacenter    |    Ja     |    Ja    |    Ja   | Ja |   Ja   |
| Windows Server 2019 Standard      |    Ja     |    Ja    |    Ja   | Ja |   Ja   |
| Windows Server 2019 Essentials    |    Ja     |    Ja    |    Ja   | Ja |   Ja   |
| Windows Server 2016 Datacenter    |    Ja     |    Ja    |    Ja   | Ja |   Ja   |
| Windows Server 2016 Standard      |    Ja     |    Ja    |    Ja   | Ja |   Ja   |
| Windows Server 2016 Essentials    |    Ja     |    Ja    |    Ja   | Ja |   Ja   |
| Windows 10 IoT Enterprise         |    Nein       |    Ja    |    Ja   | Nein  |   Ja   |
| Windows 10 Enterprise             |    Nein       |    Ja    |    Ja   | Nein  |   Ja   |
| Windows 10 Professional           |    Nein       |    Ja    |    Ja   | Nein  |   Ja   |
| Windows 10 Home                   |    Nein      |    Ja    |    Ja   | Nein  |   Ja   |
| &nbsp; | &nbsp; |

### <a name="server-core-support"></a>Server Core-Unterstützung

Installationen von SQL Server 2019 werden im Server Core-Modus folgender Windows Server-Editionen unterstützt:

:::row:::
    :::column:::
        Windows Server 2019 Core
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Windows Server 2016 Core
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
:::row-end:::

Weitere Informationen zum Installieren von SQL Server unter Server Core finden Sie unter [Installieren von SQL Server unter Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md). 


##  <a name="cross-language-support"></a><a name="CrossLanguageSupport"></a> Sprachübergreifende Unterstützung  
 Weitere Informationen zu sprachübergreifender Unterstützung und Überlegungen zur Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in lokalisierten Sprachen finden Sie unter [Lokale Sprachversionen in SQL Server](../../sql-server/install/local-language-versions-in-sql-server.md).  
  
##  <a name="disk-space-requirements"></a><a name="HardDiskSpace"></a> Speicherplatzanforderungen  
 Während der Installation von [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]erstellt Windows Installer temporäre Dateien auf dem Systemlaufwerk. Überprüfen Sie, ob Sie über mindestens 6,0 GB freien Speicherplatz auf dem Systemlaufwerk für diese Dateien verfügen, bevor Sie Setup ausführen, um [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]zu installieren oder zu aktualisieren. Diese Anforderung gilt auch dann, wenn Sie Komponenten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Nichtstandard-Laufwerk installieren.  
  
 Die tatsächlichen Anforderungen für den Festplattenspeicherplatz basieren auf der Systemkonfiguration und den Funktionen, die Sie installieren möchten. In der folgenden Tabelle sind die Speicherplatzanforderungen für die Komponenten von [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] aufgeführt.  
  
|**Feature**|**Erforderlicher Speicherplatz**|  
|-----------------|--------------------------------|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] und Datendateien, Replikation, Volltextsuche und Data Quality Services|1480 MB|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] (wie oben beschrieben) mit R-Services (datenbankintern)|2744 MB|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] (wie oben beschrieben) mit dem PolyBase-Abfragedienst für externe Daten|4194 MB|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] und Datendateien|698 MB|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|967 MB|  
|[!INCLUDE[rsql_platform](../../includes/rsql-platform-md.md)] (Eigenständig)|280 MB|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] – SharePoint|1203 MB|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Add-In für SharePoint-Produkte|325 MB|  
|[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]|121 MB|  
|Konnektivität der Clienttools|328 MB|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|306 MB|  
|Clientkomponenten (außer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Onlinedokumentation und Integration Services-Tools)|445 MB|  
|[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]|280 MB|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Onlinedokumentation zum Anzeigen und Verwalten von Hilfeinhalten*|27 MB|  
|Alle Funktionen|8030 MB|  
  
 *Die Onlinedokumentation erfordert 200 MB Festplattenspeicherplatz.  
  
##  <a name="storage-types-for-data-files"></a><a name="StorageTypes"></a> Speichertypen für Datendateien  
 Für Datendateien werden folgende Speichertypen unterstützt:  
  
- Lokaler Datenträger 
    - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt aktuell Laufwerke, die standardmäßig eine native Sektorgröße von 512 Byte und 4 KB haben.  Festplatten mit einer Sektorgröße von mehr als 4 KB führen möglicherweise zu Fehlern, wenn versucht wird, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datendateien darauf zu speichern.  Weitere Informationen zur Unterstützung von Festplattensektorgrößen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] finden Sie unter [Begrenzungen der Unterstützung von Festplattensektorgrößen in SQL Server](https://support.microsoft.com/kb/926930). 
    - Bei der[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Failoverclusterinstallation wird nur der lokale Datenträger zum Installieren der tempdb-Dateien unterstützt. Stellen Sie sicher, dass der für die tempdb-Daten und die Protokolldateien angegebene Pfad auf allen Clusterknoten gültig ist. Sind die tempdb-Verzeichnisse auf dem Failoverzielknoten während des Failovers nicht verfügbar, wird die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Ressource nicht online geschaltet.
- Freigegebener Speicher  
- [Direkte Speicherplätze \(S2D\)](/windows-server/storage/storage-spaces/storage-spaces-direct-overview)  
- SMB-Dateifreigabe  
    - Der SMB-Speicher wird bei eigenständigen oder gruppierten Installationen nicht für [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Datendateien unterstützt. Verwenden Sie stattdessen den direkt angeschlossenen Speicher, ein SAN (Storage Area Network) oder S2D. 
    - Der SMB-Speicher kann von einem Windows File Server oder einem SMB-Speichergerät eines Drittanbieters gehostet werden. Bei Verwendung von Windows File Server sollte die Windows File Server-Version 2008 oder höher verwendet werden. Weitere Informationen zum Installieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit SMB-Dateifreigabe als Speicheroption finden Sie unter [Installieren von SQL Server mit SMB-Dateifreigabe als Speicheroption](../../database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option.md)aufgeführt.  
  
  
  
##  <a name="installing-ssnoversion-on-a-domain-controller"></a><a name="DC_support"></a> Installieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Domänencontroller  
 Aus Sicherheitsgründen sollte [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] nicht auf einem Domänencontroller installiert werden. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup wird die Installation auf einem Computer, der als Domänencontroller fungiert, nicht blockieren, es gelten jedoch die folgenden Einschränkungen:  
  
- Sie können keine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienste auf einem Domänencontroller unter einem lokalen Dienstkonto ausführen.    
- Nachdem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Computer installiert wurde, können Sie den Computer nicht von einem Domänenmitglied zu einem Domänencontroller ändern. Sie müssen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deinstallieren, bevor Sie den Hostcomputer zu einem Domänencontroller ändern.    
- Nachdem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Computer installiert wurde, können Sie den Computer nicht von einem Domänencontroller zu einem Domänenmitglied ändern. Sie müssen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deinstallieren, bevor Sie den Hostcomputer zu einem Domänenmitglied ändern.   
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden nicht unterstützt, wenn es sich bei den Clusterknoten um Domänencontroller handelt.   
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird auf einem schreibgeschützten Domänencontroller nicht unterstützt. Beim Setup von[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] können keine Sicherheitsgruppen erstellt oder [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienstkonten für einen schreibgeschützten Domänencontroller bereitgestellt werden. In diesem Szenario tritt ein Setupfehler auf. 
- Eine Failoverclusterinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird in einer Umgebung, in der nur auf einen schreibgeschützten Domänencontroller zugegriffen werden kann, nicht unterstützt. 
  
## <a name="installation-media"></a>Installationsmedien

Relevante Installationsmedien können Sie unter den folgenden Hyperlinks herunterladen: 
  
- [SQL Server Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-sql-server-2019)
- [Neueste Updates für Microsoft SQL Server](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

Alternativ können Sie auch eine [Azure-VM mit SQL Server erstellen](/azure/virtual-machines/windows/sql/quickstart-sql-vm-create-portal), obwohl SQL Server auf einer VM aufgrund des Virtualisierungsmehraufwands langsamer ausgeführt wird als in einer nativen Umgebung.


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich über die Hardware- und Softwareanforderungen für die Installation von SQL Server informiert haben, können Sie mit dem [Planen einer SQL Server-Installation](../../sql-server/install/planning-a-sql-server-installation.md) beginnen oder sich die [Überlegungen zur Sicherheit bei SQL Server-Installationen](../../sql-server/install/security-considerations-for-a-sql-server-installation.md) durchlesen.