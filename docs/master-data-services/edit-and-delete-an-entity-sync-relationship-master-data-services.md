---
description: Bearbeiten und Löschen einer Entitäten-Synchronisierungspartnerschaft (Master Data Services)
title: Bearbeiten und Löschen einer Entitäten-Synchronisierungspartnerschaft
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 9a5e37f3-352e-45a6-b4a0-6f98f83b4bd8
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: d9789966bd50c937d7365ed11bc72c30623a4508
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341734"
---
# <a name="edit-and-delete-an-entity-sync-relationship-master-data-services"></a>Bearbeiten und Löschen einer Entitäten-Synchronisierungspartnerschaft (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Entitäten-Synchronisierung ist eine unidirektionale und wiederholbare Synchronisierung zwischen Entitätsversionen. Sie bietet eine Möglichkeit, Entitätsdaten zwischen verschiedenen Modellen freizugeben. Sie können eine Synchronisierungspartnerschaft, die Sie erstellt haben, bearbeiten und löschen.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 Voraussetzungen zum Bearbeiten einer Entitäten-Synchronisierungspartnerschaft.  
  
-   Sie müssen über die Berechtigung verfügen, auf den Funktionsbereich "Systemverwaltung" zuzugreifen. Weitere Informationen finden Sie unter [Berechtigungen für Funktionsbereiche &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
-   Sie müssen ein Modelladministrator des Zielmodells sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Sie müssen für die Quellentität und deren Entitätsattribute und -elemente mindestens über Lesezugriff verfügen.  
  
 Voraussetzungen zum Löschen einer Entitäten-Synchronisierungspartnerschaft.  
  
-   Sie müssen über die Berechtigung verfügen, auf den Funktionsbereich "Systemverwaltung" zuzugreifen. Weitere Informationen finden Sie unter [Berechtigungen für Funktionsbereiche &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
-   Sie müssen ein Modelladministrator des Zielmodells sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
 Berücksichtigen Sie Folgendes, wenn Sie eine Entitäten-Synchronisierungspartnerschaft bearbeiten.  
  
-   Die Quell- und Zielentität müssen sich in unterschiedlichen Modellen befinden  
  
-   Für einen Versionsstatus der Zielentität muss kein Commit ausgeführt werden.  
  
-   Sobald eine Synchronisierungspartnerschaft eingerichtet wurde, wird das Ziel sofort mit der Quelle synchronisiert.  
  
-   Eine Zielentitätsversion kann keine Quellentitätsversion einer anderen Synchronisierungspartnerschaft sein.  
  
 Berücksichtigen Sie Folgendes, wenn Sie eine Entitäten-Synchronisierungspartnerschaft ausführen.  
  
-   Nur Blattelemente werden kopiert.  
  
-   Domänenbasierte Attribute werden nicht kopiert.  
  
-   Vorläufig gelöschte Elemente werden nicht kopiert.  
  
-   Synchronisierung generiert keine Zielentitätstransaktionen/-verläufe.  
  
 **So bearbeiten Sie eine Entitäten-Synchronisierungspartnerschaft**  
  
1.  Klicken Sie im Master Data Manager auf **Systemverwaltung**.  
  
2.  Zeigen Sie auf der Seite **Modellansicht** auf der Menüleiste auf **Verwalten** , und klicken Sie auf **Entitätensynchronisierung**.  
  
3.  Wählen Sie auf der Seite **Entity Sync Maintenance** (Entitätssynchronisierungsverwaltung) eine Synchronisierungspartnerschaft im Raster aus.  
  
4.  Klicken Sie auf **Bearbeiten**. Auf der rechten Seite wird ein Bereich angezeigt.  
  
5.  Ändern Sie die **Häufigkeit**. Wählen Sie **Synchronisierung bei Bedarf** oder **Automatische Synchronisierung** aus, und legen Sie die Häufigkeit fest.  
  
6.  Klicken Sie auf **Speichern**.  
  
 **So löschen Sie eine Entitäten-Synchronisierungspartnerschaft**  
  
1.  Klicken Sie im Master Data Manager auf **Systemverwaltung**.  
  
2.  Zeigen Sie auf der Seite **Modellansicht** auf der Menüleiste auf **Verwalten** , und klicken Sie auf **Entitätensynchronisierung**.  
  
3.  Wählen Sie auf der Seite **Entity Sync Maintenance** (Entitätssynchronisierungsverwaltung) eine Synchronisierungspartnerschaft im Raster aus.  
  
4.  Klicken Sie auf **Löschen**.  
  
5.  Klicken Sie im Bestätigungsdialogfeld auf **OK**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen und Ausführen einer Entitäten-Synchronisierungs Partnerschaft &#40;Master Data Services&#41;](../master-data-services/create-and-execute-an-entity-sync-relationship-master-data-services.md)   
 [Entitäten-Synchronisierungspartnerschaft &#40;Master Data Services&#41;](../master-data-services/entity-sync-relationship-master-data-services.md)  
  
  
