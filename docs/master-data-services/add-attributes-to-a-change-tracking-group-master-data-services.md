---
description: Hinzufügen von Attributen zu einer Änderungsnachverfolgungsgruppe (Master Data Services)
title: Attribute Änderungsnachverfolgung Gruppe hinzufügen
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- change tracking groups [Master Data Services]
- attributes [Master Data Services], change tracking groups
- change tracking groups [Master Data Services], adding attributes
ms.assetid: e153eb5f-70ca-4c6f-89d8-1f937ed3917d
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 72bf0900fc46c566ae766f4ab7705ccc0bca47de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272921"
---
# <a name="add-attributes-to-a-change-tracking-group-master-data-services"></a>Hinzufügen von Attributen zu einer Änderungsnachverfolgungsgruppe (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Fügen Sie in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]einer Änderungsnachverfolgungsgruppe Attribute hinzu, wenn Sie Änderungen an den Werten des Attributs nachverfolgen möchten.  
  
> [!NOTE]  
>  Nachdem Sie einer Änderungsnachverfolgungsgruppe ein Attribut hinzugefügt haben, wird das Attribut in der [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Datenbank als geändert markiert, wenn die Werte für das Attribut geändert werden. Erstellen Sie eine Geschäftsregel, um auf der Grundlage der Änderung zu handeln.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung verfügen, auf den Funktionsbereich **System Verwaltung** zuzugreifen.  
  
-   Sie müssen ein Modelladministrator sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Attribute müssen vorhanden sein, um sie der Änderungsnachverfolgungsgruppe hinzufügen zu können. Weitere Informationen finden Sie unter [Erstellen eines Textattributs &#40;Master Data Services&#41;](../master-data-services/create-a-text-attribute-master-data-services.md).  
  
### <a name="to-add-attributes-to-a-change-tracking-group"></a>So fügen Sie einer Änderungnachverfolgungsgruppe Attribute hinzu  
  
1.  Klicken Sie in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]auf **Systemverwaltung**.  
  
2.  Wählen Sie auf der Seite **Modell verwalten** im Raster ein Modell aus, und klicken Sie dann auf **Entitäten**.  
  
3.  Wählen Sie auf der Seite **Entität verwalten** die Zeile der Entität aus, für die Sie ein Attribut erstellen möchten.  
  
4.  Klicken Sie auf **Attribute**.  
  
5.  Fügen Sie auf der Seite **Attribute verwalten** einen der folgenden Schritte aus.  
  
    -   Wenn das Attribut für Blattelemente bestimmt ist, wählen Sie **Blatt** im Listenfeld **Elementtypen** aus.  
  
    -   Wenn das Attribut für konsolidierte Elemente bestimmt ist, wählen Sie **Konsolidiert** im Listenfeld **Elementtypen** aus.  
  
    -   Wenn das Attribut für Sammlungen bestimmt ist, wählen Sie **Sammlung** im Listenfeld **Elementtypen** aus.  
  
6.  Wählen Sie die Zeile für das Attribut, das Sie bearbeiten möchten, und klicken Sie anschließend auf **Bearbeiten**.  
  
7.  Aktivieren Sie das Kontrollkästchen **Änderungsverfolgung aktivieren** .  
  
8.  Geben Sie im Feld **Änderungsverfolgungsgruppe** eine Zahl für die Gruppe ein.  
  
9. Klicken Sie auf **Attribut speichern**.  
  
     Für das bearbeitete Attribut wird die Spalte **Änderungsnachverfolgungsgruppe aktivieren** des Rasters geändert zu **Ja (Gruppe: Gruppennummer eingegeben)**.  
  
10. Wiederholen Sie diese Prozedur für alle Attribute, die Sie in die Gruppe einschließen möchten. Verwenden Sie die gleiche Änderungsnachverfolgungs-Gruppennummer für jedes Attribut in der Gruppe.  
  
## <a name="next-steps"></a>Nächste Schritte  
  
-   [Initiieren von Aktionen auf der Grundlage von Attributwertänderungen &#40;Master Data Services&#41;](../master-data-services/initiate-actions-based-on-attribute-value-changes-master-data-services.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen Sie ein Text Attribut &#40;Master Data Services&#41;](../master-data-services/create-a-text-attribute-master-data-services.md)   
 [Erstellen eines domänenbasierten Attributs &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md)  
  
  
