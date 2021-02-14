---
title: Master Data Services-Datenbank
description: Die Master Data Services-Datenbank enthält alle Informationen für das Master Data Services System und ist für eine Master Data Services-Bereitstellung von zentraler Bedeutung.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- database [Master Data Services], about the database
- database [Master Data Services]
ms.assetid: 5f590cc1-6ec2-4b8c-a598-03de0f6051a0
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 59d430ea44a83d3795d7d3e3473917ed1c14345f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338202"
---
# <a name="master-data-services-database"></a>Master Data Services-Datenbank

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Die Datenbank enthält alle Informationen für das [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -System. Sie ist wesentlich für eine [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Bereitstellung. Die [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Datenbank:  
  
-   Speichert die vom [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -System benötigten Einstellungen, Datenbankobjekte und Daten.  
  
-   Enthält Stagingtabellen zur Verarbeitung von Daten aus Quellsystemen.  
  
-   Stellt ein Schema und die Datenbankobjekte zum Speichern von Masterdaten aus Quellsystemen bereit.  
  
-   Unterstützt die Versionsfunktionalität, einschließlich der Überprüfung von Geschäftsregeln und E-Mail-Benachrichtigungen.  
  
-   Stellt Sichten für Abonnementssysteme bereit, die Daten aus der Datenbank abrufen müssen.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
-   [Stagingtabelle für Blattelemente &#40;Master Data Services&#41;](../master-data-services/leaf-member-staging-table-master-data-services.md)  
  
-   [Konsolidierte Elementstagingtabelle &#40;Master Data Services&#41;](../master-data-services/consolidated-member-staging-table-master-data-services.md)  
  
-   [Stagingtabelle für Beziehungen &#40;Master Data Services&#41;](../master-data-services/relationship-staging-table-master-data-services.md)  
  
-   [Fehler des Stagingprozesses &#40;Master Data Services&#41;](../master-data-services/staging-process-errors-master-data-services.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen einer Master Data Services Datenbank](../master-data-services/install-windows/create-a-master-data-services-database.md)   
 [Datenbankobjekt-Sicherheits &#40;Master Data Services&#41;](../master-data-services/database-object-security-master-data-services.md)   
 [Datenbankanmeldenamen, -benutzer und -rollen &#40;Master Data Services&#41;](../master-data-services/database-logins-users-and-roles-master-data-services.md)  
  
  
