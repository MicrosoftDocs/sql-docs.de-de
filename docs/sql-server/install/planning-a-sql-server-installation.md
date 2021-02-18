---
title: Planen einer SQL Server-Installation | Microsoft-Dokumentation
description: Dieser Artikel unterstützt Sie bei der Planung der Installation von SQL Server. Er enthält Links zu Ressourcen, die für die SQL Server-Installation erforderlich sind.
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: quickstart
helpviewer_keywords:
- installing SQL Server, planning
ms.assetid: b1d56f2f-603f-48f2-b902-c715f14a6db9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 948855113c0ae31484c2323e8ab482307ecf8f3d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336139"
---
# <a name="planning-a-sql-server-installation"></a>Planen einer SQL Server-Installation
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Um [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]zu installieren, führen Sie folgende Schritte aus:  
  
-   Überprüfen Sie die Installationsanforderungen, Systemkonfigurationsprüfungen und Sicherheitsaspekte für eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installation.  
  
-   Führen Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup aus, um eine höhere Version zu installieren bzw. darauf zu aktualisieren. Lesen Sie vor dem Upgrade den Artikel [Aktualisieren von SQL Server](../../database-engine/install-windows/upgrade-sql-server.md).  
  
-   Verwenden Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Hilfsprogramme, um [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]zu konfigurieren.  
  
 Unabhängig von der Installationsmethode ist es erforderlich, dass Sie den Softwarelizenzbedingungen als Einzelperson oder im Auftrag einer juristischen Person zustimmen, sofern die Verwendung der Software in keiner separaten Vereinbarung geregelt ist, z. B. einem [!INCLUDE[msCoName](../../includes/msconame-md.md)] -Volumenlizenzvertrag oder einem Vertrag eines Drittanbieters mit einem ISV oder OEM.  
  
 Die Lizenzbedingungen werden in der Setup-Benutzeroberfläche angezeigt, damit Sie diese lesen und akzeptieren können. Unbeaufsichtigte Installationen (mit den Parametern `/Q` oder `/QS`) müssen den Parameter `/IAcceptSQLServerLicenseTerms` enthalten. Laden Sie die Lizenzbedingungen separat unter [Microsoft SQL Server Lizenzbedingungen und -informationen](https://www.microsoft.com/Licensing/product-licensing/sql-server.aspx) herunter, und lesen Sie sie. Informationen zu Volumenlizenzbedingungen finden Sie unter [Lizenzbedingungen und -dokumentation](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=53). Informationen zu älteren Versionen von SQL Server finden Sie unter [Microsoft-Lizenzbestimmungen](https://go.microsoft.com/fwlink/?LinkID=148209).  
  
> [!NOTE]  
>  Abhängig davon, wie Sie die Software erworben haben (z. B. durch [!INCLUDE[msCoName](../../includes/msconame-md.md)] -Volumenlizenzierung), unterliegt die Verwendung der Software möglicherweise zusätzlichen Bestimmungen.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 [Neuigkeiten in der SQL Server-Installation](../../sql-server/install/what-s-new-in-sql-server-installation.md)  
 Dieser Artikel beschreibt die Details zu den neuen oder verbesserten Installationsfunktionen in dieser Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Hardware- und Softwareanforderungen für die Installation von [SQL Server 2016 und 2017](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md), [SQL Server 2019](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md) oder [SQL Server für Linux](../../linux/sql-server-linux-setup.md). In diesem Artikel finden Sie die Mindestanforderungen an Hardware und Software, die erforderlich sind, um eine SQL Server-Instanz zu installieren und auszuführen. .  
  
 [Überlegungen zur Sicherheit bei SQL Server-Installationen](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
 Dieser Artikel beschreibt einige bewährte Sicherheitsmethoden, die Sie vor dem Installieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und nach dem Installieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] berücksichtigen sollten.  
  
 [Konfigurieren von Windows-Dienstkonten und -Berechtigungen](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  
 Dieser Artikel beschreibt die Standardkonfiguration von Diensten in dieser Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und die Konfigurationsoptionen für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienste, die Sie während und nach der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Installation festlegen können.  
  
 [Netzwerkprotokolle und Netzwerkbibliotheken](../../sql-server/install/network-protocols-and-network-libraries.md)  
 Dieser Artikel beschreibt die Standardkonfiguration von Netzwerkprotokollen in dieser Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und die verfügbaren Konfigurationsoptionen.  
  
 [Verwenden mehrerer Versionen und Instanzen von SQL Server](../../sql-server/install/work-with-multiple-versions-and-instances-of-sql-server.md)  
 Dieser Artikel beschreibt die Überlegungen zum Installieren mehrerer Versionen und Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Lokale Sprachversionen in SQL Server](../../sql-server/install/local-language-versions-in-sql-server.md)  
 Dieser Artikel beschreibt die lokalisierten Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="related-sections"></a>Verwandte Abschnitte  
 [Installieren von SQL Server](../../database-engine/install-windows/install-sql-server.md)  
 Dieser Abschnitt bietet eine Übersicht über andere Installationsoptionen, die zur Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]zur Verfügung stehen.  
  
 [Installieren von SQL Server Business Intelligence-Funktionen](../../sql-server/install/install-sql-server-business-intelligence-features.md)  
 In diesem Abschnitt der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setupdokumentation wird erläutert, wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Funktionen, die Teil der Microsoft BI-Plattform sind, installiert werden.  
  
 [Aktualisieren von SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
 Der Abschnitt enthält eine Übersicht über das Upgrade von einer früheren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Deinstallieren von SQL Server](../../sql-server/install/uninstall-sql-server.md)  
 Lesen Sie diesen Abschnitt, wenn Sie eine vorhandene Instanz von [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] vollständig deinstallieren und das System für die Neuinstallation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]vorbereiten möchten.  
  
 [SQL Server-Failoverclusterinstallation](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)  
 In diesem Abschnitt der Dokumentation zum [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup wird die Installation und Konfiguration von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Failoverclustern beschrieben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Installieren von SQL Server von der Eingabeaufforderung](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)   
 [Lösungen mit hoher Verfügbarkeit &#40;SQL Server&#41;](../../database-engine/sql-server-business-continuity-dr.md)   
 [Vor dem Installieren des Failoverclusterings](../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)   
 [Upgrade SQL Server Using the Installation Wizard (Setup) (Upgrade von SQL Server mithilfe des Installations-Assistenten (Setup))](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)