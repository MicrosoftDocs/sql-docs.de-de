---
title: Deinstallieren und Entfernen von Master Data Services | Microsoft-Dokumentation
description: Mit diesem Deinstallationsvorgang werden Master Data Services-Ordner und -Dateien entfernt sowie der Konfigurations-Manager für Master Data Services auf dem lokalen Computer deinstalliert.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: efc2431c-588b-42e7-b23b-c875145a33f6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4f57ed3d099afa9f85dcc72f489bac6bbcb713fc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336102"
---
# <a name="uninstall-and-remove-master-data-services"></a>Deinstallieren und Entfernen von Master Data Services
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

   Um die [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]-Funktion von einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu deinstallieren, befolgen Sie die Schritte unter [Deinstallieren einer vorhandenen SQL Server-Instanz &#40;Setup&#41;](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md) und geben auf der Seite **Funktionen auswählen**[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] als zu entfernende Funktion an. Durch den Deinstallationsvorgang werden [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Ordner und -Dateien entfernt und [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] vom lokalen Computer deinstalliert.  
  
 Um Datenverluste oder Auswirkungen auf andere Computer im System zu verhindern, werden bestimmte Elemente vom Deinstallationsvorgang nicht entfernt oder geändert. Überprüfen Sie anhand der folgenden Tabelle, welche Elemente beibehalten und welche Elemente entfernt werden sollten.  
  
|Element|BESCHREIBUNG|  
|----------|-----------------|  
|Ordner und Dateien|Beim Deinstallationsvorgang werden die meisten Ordner und Dateien aus dem Installationspfad entfernt.<br /><br /> Die Ordner Master Data Services und MDSTempDir werden durch den Deinstallationsvorgang jedoch nicht aus dem Installationspfad gelöscht. Nach Abschluss der Deinstallation können Sie diese Ordner manuell aus dem Dateisystem löschen. Weitere Informationen finden Sie unter [Ordner- und Dateiberechtigungen &#40;Master Data Services&#41;](../../master-data-services/folder-and-file-permissions-master-data-services.md).|  
|[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] Assemblys|Der Deinstallationsvorgang entfernt [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Assemblys aus dem Global Assembly Cache (GAC).|  
|Datenbank|Der Deinstallationsvorgang wirkt sich nicht auf die [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Datenbank aus. Die Datenbank bleibt in der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Instanz erhalten, sodass keine Daten verloren gehen, z. B. Masterdaten, Modellobjekte, Benutzer- und Gruppenberechtigungen, Geschäftsregeln usw.<br /><br /> Wenn Sie die Datenbank nicht benötigen und sie in Zukunft voraussichtlich nicht mit einer anderen [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Website oder -Anwendung verbinden, können Sie die Datenbank aus der Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] löschen, die diese hostet. Weitere Informationen finden Sie unter [Löschen einer Datenbank](../../relational-databases/databases/delete-a-database.md).|  
|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] und Web.config|Der Deinstallationsvorgang entfernt den Ordner WebApplication aus dem Dateisystem. Der Ordner WebApplication enthält die Webanwendungsdateien und die Datei Web.config für [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)].<br /><br /> **\*\* Wichtig \*\*** : Vor der Deinstallation empfiehlt es sich, die Datei „Web.config“ an einen anderen Speicherort zu kopieren, damit benutzerdefinierte Einstellungen oder andere Informationen in der Datei erhalten bleiben. Die Datei Web.config kann nach Abschluss des Deinstallationsvorgangs nicht wiederhergestellt werden.|  
|IIS-Elemente|Der Deinstallationsvorgang wirkt sich nicht auf Anwendungspools, Websites oder Webanwendungen in Internetinformationsdiensten (IIS) auf dem lokalen Computer aus. Da der Deinstallationsvorgang den Ordner WebApplication und die Datei Web.config für [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]entfernt, stellen alle [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Webanwendungen, die diese Dateien erfordern, keine Inhalte mehr bereit. Wenn Benutzer versuchen, auf die Webanwendung zuzugreifen, wird der folgende HTTP-Fehler 500.19 (interner Serverfehler) ausgegeben: „Auf die angeforderte Seite kann nicht zugegriffen werden, da die zugehörigen Konfigurationsdaten für die Seite ungültig sind.“<br /><br /> Wenn Sie die Website oder die Anwendung und den Anwendungspool, der die Website oder Anwendung bereitstellt, nicht mehr benötigen, können Sie diese mit einem IIS-Tool löschen. Weitere Informationen finden Sie unter [IIS 7.0: Vorgangshandbuch](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732976(v=ws.10)) im [!INCLUDE[msCoName](../../includes/msconame-md.md)] TechNet.|  
|Gruppe **MDS_ServiceAccounts**|Nach Abschluss des Deinstallationsvorgangs bleiben die Windows-Gruppe **MDS_ServiceAccounts** und alle Dienstkonten, die der Gruppe hinzugefügt wurden, erhalten. Wenn Sie die Gruppe und die Konten nicht mehr benötigen, können Sie sie entfernen.|  
|Registrierung|Der Deinstallationsvorgang entfernt alle [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] -Registrierungsschlüssel aus der Windows-Registrierung.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Installieren von Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
