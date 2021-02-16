---
title: Seite „Datenbanken auswählen“ (Assistenten für neue Verfügbarkeitsgruppen und zum Hinzufügen von Datenbanken)
description: In diesem Artikel wird die Seite „Datenbanken auswählen“ der Assistenten für neue Verfügbarkeitsgruppen und zum Hinzufügen von Datenbanken erläutert, auf die über die grafische Benutzeroberfläche von SQL Server Management Studio zugegriffen werden kann.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.adddatabasewizard.selectdatabases.f1
- sql13.swb.newagwizard.selectdatabases.f1
ms.assetid: 929c5e15-d087-438d-b1f2-aa97c5f8bff8
author: cawrites
ms.author: chadam
ms.openlocfilehash: a25b2d75c6ee143f005677a250d59b4c0df32a22
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352654"
---
# <a name="select-databases-page-new-availability-group-wizard-and-add-database-wizard"></a>Select Databases Page (New Availability Group Wizard and Add Database Wizard)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Hilfethema werden die Optionen der Seite für das **Angeben von Datenbanken** beschrieben. Dieses Thema gilt für den [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)] und den [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] von [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
##  <a name="select-databases-options"></a><a name="PageOptions"></a> Optionen für "Datenbanken auswählen"  
 Im Raster **Benutzerdatenbanken auf dieser SQL Server-Instanz** wird jede lokale Benutzerdatenbank aufgelistet. Es gibt folgende Spalten:  
  
 **Name**  
 Zeigt den Namen einer lokalen Benutzerdatenbank an.  

 **Größe**  
 Zeigt die Datenbankgröße an, wenn die Größe für den Assistenten verfügbar ist.  
  
 **Status**  
 Zeigt einen Link an, dessen Text angibt, ob eine bestimmte Datenbank die Voraussetzungen erfüllt, um einer Verfügbarkeitsgruppe hinzugefügt zu werden. Wenn der Status "**Voraussetzungen erfüllt**" lautet, können Sie die Datenbank der Verfügbarkeitsgruppe hinzufügen. Wenn eine Datenbank nicht alle Voraussetzungen erfüllt, stellt der Link **Status** eine kurze Erklärung bereit, warum die Datenbank nicht geeignet ist. Um weitere Informationen zu erhalten, klicken Sie auf den Link.  
  
 Sie können den Assistenten auf der Seite **Datenbank auswählen** belassen, während Sie eine Datenbank bearbeiten, um eine Voraussetzung zu erfüllen. Wenn Sie zur Seite **Datenbanken auswählen** zurückkehren, klicken Sie auf **Aktualisieren** , um das Raster zu aktualisieren.  
  
 **Kennwort**  
 Wenn die Datenbank einen Datenbank-Hauptschlüssel enthält, geben Sie das Kennwort für den Datenbank-Hauptschlüssel ein.  
  
 **Aktualisieren**  
 Klicken Sie, um das Raster zu aktualisieren. Dies ist nützlich, nachdem Sie eine Datenbank bearbeitet haben, um eine Voraussetzung zu erfüllen.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Verwenden des Dialogfelds Neue Verfügbarkeitsgruppe &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Verwenden des Assistenten zum Hinzufügen von Datenbanken zu Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
  
