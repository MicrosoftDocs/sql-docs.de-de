---
title: Aktualisieren der SQL Server-Verwaltungstools | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Unterstützung für das Upgrade von SQL Server-Verwaltungstools und von Verwaltungskomponenten wie SQL Server-Agent.
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- management tools, upgrading
ms.assetid: 1dab50b9-d16c-49a1-9ecc-af72adb6c378
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 76aeabce135b9690cdc9209618d13b2b3505e3a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336354"
---
# <a name="upgrade-sql-server-management-tools"></a>Aktualisieren der SQL Server-Verwaltungstools

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] unterstützt ein Upgrade von [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher. Dieser Artikel beschreibt die Unterstützung und das Verhalten des Upgrades von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verwaltungstools und Verwaltungskomponenten wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent, Datenbank-E-Mail, Wartungsplänen, XPStar und XPWeb.  
  
> [!IMPORTANT]  
>  Bei lokalen Installationen müssen Sie das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup als Administrator ausführen. Wenn Sie das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup von einer Remotefreigabe ausführen, müssen Sie ein Domänenkonto verwenden, das Lese- und Ausführungsberechtigungen auf der Remotefreigabe hat.  
  
## <a name="known-upgrade-issues"></a>Bekannte Upgradeprobleme  
Beachten Sie die folgenden Punkte, bevor Sie auf [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]aktualisieren:  
  
### <a name="for-all-upgrade-scenarios"></a>Für alle Aktualisierungsszenarien:  
  
- Vor dem Aktualisieren des MSX-Servers sollte ein Upgrade aller TSX-Server durchgeführt werden. Weitere Informationen über MSX/TSX in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]finden Sie unter [Automatisierte Verwaltung in einem Unternehmen](../../ssms/agent/automated-administration-across-an-enterprise.md).  
  
-   Alle Komponenten in einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] müssen gleichzeitig aktualisiert werden. Die Versionsnummern der [!INCLUDE[ssDE](../../includes/ssde-md.md)]-, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]- und [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Komponenten innerhalb einer Instanz von [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]müssen identisch sein.  
  
-   Sie können während der Aktualisierung auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Komponenten zu einer vorhandenen Installation von [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]hinzufügen. Weitere Informationen finden Sie unter [Upgrade SQL Server 2016 Using the Installation Wizard (Setup) (Aktualisieren von SQL Server 2016 mithilfe des Installations-Assistenten (Setup))](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md).  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Clienttools wie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Optimierungsratgeber, sqlcmd und osql werden nicht auf [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]aktualisiert. Die Clienttools werden stattdessen parallel zu Tools älterer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Versionen ausgeführt. [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] unterstützt den Import von Einstellungen aus früheren Versionen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Clienttools.  
  
-   Die Authentifizierung vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird während des Upgrades von der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung auf die Windows-Authentifizierung aktualisiert. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung wird in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
-   Daten für Aufträge und Warnungen werden während des Updates auf [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]beibehalten.  
  
-   Wird SQL Mail in der zu aktualisierenden Instanz verwendet, werden zugehörige XPs nach dem Update unterstützt und aktiviert. Andernfalls sind sie deaktiviert.  
  
-   Datenbank-E-Mail, die auch als SQLiMail bezeichnet wird, wird mit der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Komponente von [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]aktualisiert. Standardmäßig ist Datenbank-E-Mail nach dem Update deaktiviert. Alle Schema-Updates sollten nach dem Upgrade mit einem Updateskript abgeglichen werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Unterstützte Versions- und Editionsupgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)   
 [Abwärtskompatibilität_gelöscht](/previous-versions/sql/sql-server-2016/cc280407(v=sql.130))   
 [Upgrade SQL Server Using the Installation Wizard (Setup) (Upgrade von SQL Server mithilfe des Installations-Assistenten (Setup))](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)  
  
