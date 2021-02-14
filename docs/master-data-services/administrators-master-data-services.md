---
title: Administrators
description: 'Erfahren Sie mehr über die Typen von Administratoren in Master Data Services: Modell Administratoren, Entitäts Administratoren und Administrator.'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- administrators [Master Data Services], about administrators
- administrators [Master Data Services]
- models [Master Data Services], administrators
ms.assetid: d330aa4e-6ade-4b09-b376-1b15d6c78f7d
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: bfe03a31b5d8f35770f8ded41e4d8fdb86fb9d30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339664"
---
# <a name="administrators-master-data-services"></a>Administratoren (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In diesem Artikel werden die verschiedenen Administratortypen in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]beschrieben: Modelladministrator, Entitätsadministrator und Administrator.  
  
## <a name="model-administrators"></a>Modelladministratoren  
 In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] ist ein Modell Administrator ein Benutzer, dem die **Administrator** Berechtigung für das Modell Objekt der obersten Ebene auf der Registerkarte **Modell Objekte** zugewiesen ist. Wenn ein Benutzer die Administrator Berechtigung für ein bestimmtes Modell besitzt, werden alle anderen Berechtigungen für untergeordnete Objekte des Modells (sowohl Modell Objekt-als auch Element Berechtigungen) von der **Administrator** Berechtigung des Modells übertrumpft und werden effektiv ignoriert.  
  
-   Wenn der Benutzer Zugriff auf den Funktionsbereich **Explorer** hat, kann er alle Masterdaten in diesem Bereich hinzufügen, löschen und aktualisieren.  
  
-   Wenn der Benutzer Zugriff auf andere Funktionsbereiche hat, kann der Benutzer alle im Funktionsbereich verfügbaren administrativen Tasks ausführen.  
  
 Jedes Modell kann mehrere Administratoren aufweisen. Ein Benutzer kann als Modelladministrator für ein Modell, mehrere oder alle Modelle in der [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Bereitstellung definiert sein.  
  
 Er wird entweder in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] oder programmgesteuert als Modelladministrator festgelegt. Weitere Informationen finden Sie unter [Erstellen eines Modelladministrators &#40;Master Data Services&#41;](../master-data-services/create-a-model-administrator-master-data-services.md).  
  
## <a name="entity-administrators"></a>Entitätsadministratoren  
 In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] handelt es sich bei einem Entitäts Administrator um einen Benutzer mit Administrator Berechtigungen, der dem Entitäts Objekt auf der Registerkarte Modell Objekte zugewiesen ist. Wenn ein Benutzer über Administrator Berechtigungen für eine Entität verfügt, werden alle anderen Berechtigungen für die untergeordneten Objekte der Entität (sowohl Modell Objekt-als auch Element Berechtigungen) von den Administrator Berechtigungen abgelöst und ignoriert.  
  
-   Wenn der Benutzer Zugriff auf den Funktionsbereich **Explorer** hat, kann er alle Masterdaten in diesem Bereich hinzufügen, löschen und aktualisieren.  
  
-   Wenn die Entitätsänderungen eine Administratorberechtigung erfordern, kann ein Entitätsadministrator die Änderungen prüfen und Changesets anschließend zulassen oder ablehnen.  
  
 Jede Entität kann mehrere Administratoren besitzen. Jeder Benutzer kann als Entitätsadministrator für eine, mehrere oder alle Entitäten definiert sein.  
  
 Er wird entweder in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] oder programmgesteuert als Entitätsadministrator festgelegt. Weitere Informationen finden Sie unter [Erstellen eines Entitätsadministrators &#40;Master Data Services&#41;](../master-data-services/create-an-entity-administrator-master-data-services.md).  
  
## <a name="master-data-services-super-user"></a>Master Data Services-Administrator  
 In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]können Sie einem Benutzer Berechtigungen für den Funktionsbereich „Administrator“ zuweisen. Ein Benutzer mit Berechtigungen für den Funktionsbereich „Administrator“ verfügt über Administratorberechtigungen für alle Modelle und über Berechtigungen für alle anderen Funktionsbereiche. Informationen zu den Berechtigungen für Funktionsbereiche finden Sie unter [Berechtigungen für Funktionsbereiche &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
 Der Standardadministrator wird für das **Administratorkonto** angegeben, wenn mithilfe des [Datenbankerstellungs-Assistenten &#40;Master Data Services-Konfigurations-Manager&#41;](../master-data-services/create-database-wizard-master-data-services-configuration-manager.md) die [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]-Datenbank erstellt wird.  
  
 Der Administrator verfügt über die folgenden Berechtigungen:  
  
-   Zugriff auf alle Funktionsbereiche  
  
-   Hinzufügen, Löschen und Aktualisieren aller Masterdaten für alle Modelle im Funktionsbereich **Explorer**  
  
 Sie können mehreren Benutzer bzw. Benutzergruppen Administratorberechtigungen zuweisen.  
  
## <a name="comparing-administrator-types"></a>Vergleichen von Administratortypen  
  
|Administratortyp|Beschreibung|  
|------------------------|-----------------|  
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] Administrator|Im [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] zugewiesene Berechtigungen wirken sich nicht auf den Zugriff des Administrators aus.<br /><br /> Kann ein Administrator sein, je nach den explizit zugewiesenen oder von einer Gruppe geerbten Berechtigungen für Funktionsbereiche.<br /><br /> Verfügt automatisch über alle Berechtigungen für alle Modelle.<br /><br /> Verfügt automatisch über Zugriff auf alle Funktionsbereiche.|  
|Modelladministrator|Kann ein Modelladministrator sein, je nach den explizit zugewiesenen oder von einer Gruppe geerbten Adminberechtigungen.<br /><br /> Verfügt nur über Zugriff auf Funktionsbereiche, für die ihm eine Berechtigung gewährt wurde.<br /><br /> Verfügt automatisch über alle Berechtigungen für alle Objekte und Elemente in dem bestimmten Modell.|  
|Entitätsadministrator|Kann ein Entitätsadministrator sein, je nach den explizit zugewiesenen oder von einer Gruppe geerbten Administratorberechtigungen.<br /><br /> Verfügt nur über Zugriff auf Funktionsbereiche, für die ihm eine Berechtigung gewährt wurde.<br /><br /> Verfügt automatisch über alle Berechtigungen für alle Objekte und Elemente in der bestimmten Entität.<br /><br /> Können die ausstehenden Changesets genehmigen, wenn die Entitätsänderungen genehmigt werden müssen.|  
  
## <a name="external-resources"></a>Externe Ressourcen  
 Blogbeitrag [Sicherheitsverbesserungen](/archive/blogs/e7/improvements-to-autoplay), auf msdn.com.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen Sie einen Modell Administrator &#40;Master Data Services&#41;](../master-data-services/create-a-model-administrator-master-data-services.md)   
 [Erstellen einer Master Data Services Datenbank](../master-data-services/install-windows/create-a-master-data-services-database.md)   
 [Benachrichtigungen &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md)  
  
