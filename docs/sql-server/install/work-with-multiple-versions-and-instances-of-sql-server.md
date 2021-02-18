---
title: Arbeiten mit mehreren Versionen und Instanzen
description: In diesem Artikel erfahren Sie, wie Sie mehrere SQL Server-Instanzen installieren oder SQL Server auf einem Computer installieren, auf dem bereits ältere SQL Server-Versionen installiert sind.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- concurrent installations [SQL Server]
- versions [SQL Server], multiple
- side-by-side installations [SQL Server]
- multiple SQL Server component versions
- installing SQL Server, side-by-side installations
- Setup [SQL Server], side-by-side installations
- 64-bit edition [SQL Server]
- 32-bit edition [SQL Server]
- editions [SQL Server], side-by-side installations
ms.assetid: 93acefa8-bb41-4ccc-b763-7801f51134e0
author: cawrites
ms.author: chadam
ms.openlocfilehash: c5290748f7a3a99909f124e17011e2d652557682
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336041"
---
# <a name="work-with-multiple-versions-and-instances-of-sql-server"></a>Arbeiten mit mehreren Versionen und Instanzen von SQL Server

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Sie können mehrere Instanzen von SQL Server installieren oder SQL Server auf einem Computer installieren, auf dem bereits frühere SQL Server-Versionen installiert sind.

Die folgenden SQL Server-bezogenen Elemente sind bei Installation mehrerer Instanzen auf demselben Computer kompatibel:

- Datenbank-Engine

- Analysis Services

- Reporting Services (in SQL Server 2016 und früher). Ab SQL Server 2016 sind SQL Server Reporting Services (SSRS) eine separate Installation. 


Sie können frühere Versionen von SQL Server auf einem Computer aktualisieren, auf dem bereits andere SQL Server-Versionen installiert sind. Unterstützte Upgradeszenarien finden Sie unter [Unterstützte Versions- und Editionsupgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md).
  
## <a name="version-components-and-numbering"></a>Versionskomponenten und Nummerierung

 Die folgenden Konzepte sind nützlich, um das Verhalten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für parallele Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]zu verstehen.
  
 Das Standardformat der Produktversion für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist MM.nn.bbbb.rr, wobei die einzelnen Segmente wie folgt definiert sind:
  
 MM - Hauptversion  
  
 nn - Nebenversion  
  
 bbbb - Buildnummer  
  
 rr - Buildrevisionsnummer  
  
 Bei jeder Haupt- oder Nebenversion von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]wird die Versionsnummer erhöht, damit sie sich von früheren Versionen unterscheidet. Diese Änderung an der Version wird zu vielen Zwecken verwendet. Dazu gehören das Anzeigen von Versionsinformationen an der Benutzeroberfläche, die Steuerung der Ersetzung von Dateien während eines Upgrades, die Anwendung von Service Packs sowie die funktionale Differenzierung zwischen den aufeinanderfolgenden Versionen.
  
### <a name="components-shared-by-all-versions-of-ssnoversion"></a>Von allen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

 Bestimmte Komponenten werden von allen Instanzen aller installierten Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]gemeinsam genutzt. Wenn Sie unterschiedliche Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf demselben Computer parallel installieren, werden diese Komponenten automatisch auf die neueste Version aktualisiert. Solche Komponenten werden im Allgemeinen automatisch deinstalliert, wenn die letzte Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deinstalliert wird.
  
 Beispiele: der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Browser und Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] VSS Writer.
  
### <a name="components-shared-across-all-instances-of-the-same-major-version-of-ssnoversion"></a>Von allen Instanzen derselben [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Versionen, die die gleiche Hauptversion aufweisen, werden einige Komponenten von allen Instanzen gemeinsam genutzt. Wenn die freigegebenen Komponenten während des Upgrades ausgewählt werden, werden die vorhandenen Komponenten auf die neueste Version aktualisiert.
  
Beispiele: [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)], [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]und die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Onlinedokumentation.
  
### <a name="components-shared-across-minor-versions"></a>Von Nebenversionen gemeinsam genutzte Komponenten

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Versionen, die die gleiche Haupt- und Nebenversion aufweisen, nutzen Komponenten gemeinsam.
  
Beispiel: .
  
### <a name="components-specific-to-an-instance-of-ssnoversion"></a>Für eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Einige [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Komponenten oder -Dienste gehören speziell zu einer bestimmten Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sie werden auch als instanzabhängig bezeichnet. Sie nutzen die gleiche Version wie ihre Hostinstanz und werden ausschließlich für diese Instanz verwendet.
  
Beispiele: [!INCLUDE[ssDE](../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]und [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
### <a name="components-that-are-independent-of-the-ssnoversion-versions"></a>Von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Versionen unabhängige Komponenten

Bestimmte Komponenten werden während [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Setup installiert, aber sind von den Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]unabhängig. Sie werden möglicherweise von Hauptversionen oder von allen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Versionen gemeinsam genutzt.  

Beispiele: Microsoft Sync Framework, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
Weitere Informationen über die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Installation finden Sie unter [Installieren von SQL Server 2016 vom Installations-Assistenten aus &#40;Setup&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md). Weitere Informationen zur Deinstallation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact finden Sie unter [Deinstallieren einer vorhandenen SQL Server-Instanz &#40;Setup&#41;](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md).  
  
## <a name="using-ssnoversion-side-by-side-with-previous-versions-of-ssnoversion"></a>Parallele Verwendung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Sie können [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Computer installieren, auf dem bereits Instanzen einer früheren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Version ausgeführt werden. Wenn auf dem Computer bereits eine Standardinstanz vorhanden ist, muss [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als benannte Instanz installiert werden.  

Die folgende Tabelle zeigt die parallele Unterstützung für jede Version von SQL Server in allgemein unterstützten Windows-Versionen mit installierten erforderlichen .NET-Versionen:

| Vorhandene Instanz | Unterstützung der parallelen Installation| 
|-------------------|----------------------------|
| SQL Server 2019 | SQL Server 2008 bis SQL Server 2017| 
| SQL Server 2017 | SQL Server 2008 bis SQL Server 2016| 
| SQL Server 2016 | SQL Server 2008 bis SQL Server 2014| 

Weitere Informationen finden Sie unter [Verwenden von SQL Server in Windows 8 und höher](https://support.microsoft.com/help/2681562/using-sql-server-in-windows-8-and-later-versions-of-windows-operating). 

  
> [!CAUTION]  
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep bietet keine Unterstützung für die parallele Installation von vorbereiteten [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Instanzen und früheren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Versionen auf demselben Computer. Sie können jedoch mehrere vorbereitete Instanzen der gleichen Hauptversion von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] parallel auf dem gleichen Computer installieren. Weitere Informationen finden Sie unter [Considerations for Installing SQL Server Using SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md).  
>
> SQL Server 2016 und höhere Versionen können nicht parallel mit früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Computer installiert werden, auf dem Windows Server 2008 R2 Server Core SP1 ausgeführt wird. Weitere Informationen zu Server Core-Installationen finden Sie unter [Installieren von SQL Server 2016 unter Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md).  
  


## <a name="preventing-ip-address-conflicts"></a>Verhindern von Konflikten mit IP-Adressen

Wenn eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failoverclusterinstanz und eine eigenständige [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]-Instanz parallel installiert sind, achten Sie darauf, dass Konflikte mit TCP-Portnummern für die IP-Adressen vermieden werden. Konflikte treten in der Regel auf, wenn in zwei [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Instanzen die Verwendung des TCP-Standartports (1433) konfiguriert wurde. Um Konflikte zu vermeiden, konfigurieren Sie in einer Instanz die Verwendung eines nicht standardmäßigen festen Ports. Die Konfiguration eines festen Ports kann in der Regel in der eigenständigen Instanz am einfachsten vorgenommen werden. Wenn [!INCLUDE[ssDE](../../includes/ssde-md.md)] für die Verwendung anderer Ports konfiguriert wird, wird verhindert, dass ein unerwarteter IP-Adressen-/TCP-Port-Konflikt auftritt, der den Start einer Instanz blockiert, wenn eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failoverclusterinstanz ein Failover zu dem Standbyknoten ausführt.
  
## <a name="see-also"></a>Weitere Informationen

* [Hardware- und Softwareanforderungen für die Installation von SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)
* [Installieren von SQL Server über den Installations-Assistenten &#40;Setup&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)
* [Unterstützte Versions- und Editionsupgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)
* [Aktualisieren von SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)
* [Editionen und unterstützte Features von SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md) 
* [Editionen und unterstützten Funktionen von SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)
* [Editionen und unterstützten Funktionen von SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)
* [Abwärtskompatibilität](/previous-versions/sql/sql-server-2016/cc280407(v=sql.130))