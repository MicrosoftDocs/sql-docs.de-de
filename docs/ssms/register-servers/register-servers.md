---
description: Registrieren von Servern
title: Registrieren von Servern
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.sqlserverregisteredserver.dhelp
helpviewer_keywords:
- connections [SQL Server], registered servers
- registering servers
- servers [SQL Server], registering
- server management [SQL Server], registering servers
- server registration [SQL Server]
ms.assetid: c2a2513e-fa09-419c-99e7-a12d57c5a0db
author: markingmyname
ms.author: maghan
ms.manageR: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: dd50ab7a771dd0cd479716c94fa26110198822b0
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88468548"
---
# <a name="register-servers"></a>Registrieren von Servern

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Durch das Registrieren eines Servers in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] können Sie die Serververbindungsinformationen für zukünftige Verbindungen speichern. Es gibt drei Möglichkeiten, einen Server in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]zu registrieren.  
  
1.  Lokale Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden beim ersten Start von [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] nach der Installation automatisch registriert.  
  
2.  Der automatische Registrierungsprozess kann aber jederzeit gestartet werden, um die Registrierung von lokalen Serverinstanzen wiederherzustellen.  
  
3.  Und letztlich können Sie einen Server mit dem Tool für registrierte Server von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]registrieren.  
  
## <a name="benefits-of-registered-servers"></a>Vorteile von registrierten Servern  
 Die Option "Registrierte Server" bietet folgende Möglichkeiten:  
  
-   Registrieren von Servern zum Beibehalten der Verbindungsinformationen.  
  
-   Ermitteln, ob ein registrierter Server ausgeführt wird.  
  
-   Einfaches Herstellen einer Verbindung zwischen dem Objekt-Explorer und dem Abfrage-Editor mit einem registrierten Server.  
  
-   Bearbeiten oder Löschen der Registrierungsinformationen für einen registrierten Server.  
  
-   Erstellen von Servergruppen.  
  
-   Bereitstellen benutzerfreundlicher Namen für registrierte Server durch Angeben eines Werts im Feld **Name des registrierten Servers** , der von dem in der Liste **Servername** abweicht.  
  
-   Bereitstellen detaillierter Beschreibungen für registrierte Server.  
  
-   Bereitstellen detaillierter Beschreibungen registrierter Servergruppen.  
  
-   Exportieren registrierter Servergruppen.  
  
-   Importieren registrierter Servergruppen.  
  
-   Anzeigen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Protokolldateien für Online- oder Offlineinstanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="related-tasks"></a>Related Tasks  
 Erste Schritte mit registrierten Servern finden Sie in den folgenden Themen:  
  
|**Beschreibung**|**Thema**|  
|---------------------|---------------|  
|Registrieren lokaler Serverinstanzen|[Registrieren eines verbundenen Servers &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/register-a-connected-server-sql-server-management-studio.md)|  
|Registrieren eines Servers|[Erstellen eines neuen registrierten Servers &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/create-a-new-registered-server-sql-server-management-studio.md)|  
|Anzeigen registrierter Server|[Anzeigen von registrierten Servern in SQL Server Management Studio](../../tools/sql-server-management-studio/view-registered-servers-in-sql-server-management-studio.md)|  
|Entfernen eines registrierten Servers|[Entfernen eines registrierten Servers &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/remove-a-registered-server-sql-server-management-studio.md)|  
|Ändern der Registrierung eines Servers|[Ändern der Registrierung eines Servers &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/change-a-server-s-registration-sql-server-management-studio.md)|  
|Herstellen einer Verbindung zu einem registrierten Server|[Herstellen einer Verbindung zu einem registrierten Server &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/connect-to-a-registered-server-sql-server-management-studio.md)|  
|Trennen eines registrierten Servers|[Trennen der Verbindung mit einem registrierten Server &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/disconnect-from-a-registered-server-sql-server-management-studio.md)|  
|Verschieben eines registrierten Servers bzw. einer registrierte Servergruppe|[Verschieben eines registrierten Servers oder einer registrierten Servergruppe &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/move-a-registered-server-or-registered-server-group.md)|  
|Ändern des Namens eines registrierten Servers oder einer Servergruppe|[Ändern des Namens eines registrierten Servers oder einer registrierten Servergruppe &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/change-the-name-of-registered-server-or-registered-server-group.md)|  
|Erstellen oder Bearbeiten einer Servergruppe|[Erstellen oder Bearbeiten einer Servergruppe &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/create-or-edit-a-server-group-sql-server-management-studio.md)|  
|Entfernen einer Servergruppe|[Entfernen einer Servergruppe &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/remove-a-server-group-sql-server-management-studio.md)|  
|Exportieren von Informationen zum registrierten Server|[Exportieren von Informationen zum registrierten Server &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/export-registered-server-information-sql-server-management-studio.md)|  
|Importieren von Informationen zum registrierten Server|[Importieren von Informationen zum registrierten Server &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/import-registered-server-information-sql-server-management-studio.md)|  
|Erstellen eines zentralen Verwaltungsservers und einer Servergruppe|[Erstellen eines zentralen Verwaltungsservers und einer Servergruppe &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/create-a-central-management-server-and-server-group.md)|  
|Gleichzeitiges Ausführen von Anweisungen für mehrere Server|[Gleichzeitiges Ausführen von Anweisungen für mehrere Server &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/execute-statements-against-multiple-servers-simultaneously.md)|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Remoteserver](../../database-engine/configure-windows/remote-servers.md)  
  
  
