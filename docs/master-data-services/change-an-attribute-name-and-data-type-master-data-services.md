---
description: Ändern eines Attributnamens und Datentyps (Master Data Services)
title: Ändern eines Attributnamens und Datentyps
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- attributes [Master Data Services], changing name
ms.assetid: d348f238-f59d-41c7-ad20-3ccd55bfd9e5
author: lrtoyou1223
ms.author: lle
manager: erikre
ms.openlocfilehash: d139a8c1bfc46c838d3720eb906faa33bfec2d09
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272851"
---
# <a name="change-an-attribute-name-and-data-type-master-data-services"></a>Ändern eines Attributnamens und Datentyps (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]können Sie den Namen eines Attributs ändern.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung verfügen, auf den Funktionsbereich **System Verwaltung** zuzugreifen.  
  
-   Sie müssen ein Modelladministrator sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-change-an-attribute-name-and-type"></a>So ändern Sie einen Attributnamen und -typ  
  
1.  Klicken Sie in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]auf **Systemverwaltung**.  
  
2.  Wählen Sie auf der Seite **Modell verwalten** im Raster ein Modell aus, und klicken Sie dann auf **Entitäten**.  
  
3.  Wählen Sie auf der Seite **Entität verwalten** die Zeile der Entität aus, für die Sie ein Attribut erstellen möchten.  
  
4.  Klicken Sie auf **Attribute**.  
  
5.  Fügen Sie auf der Seite **Attribute verwalten** einen der folgenden Schritte aus.  
  
    -   Wenn das Attribut für Blattelemente bestimmt ist, wählen Sie **Blatt** im Listenfeld **Elementtypen** aus.  
  
    -   Wenn das Attribut für konsolidierte Elemente bestimmt ist, wählen Sie **Konsolidiert** im Listenfeld **Elementtypen** aus.  
  
    -   Wenn das Attribut für Sammlungen bestimmt ist, wählen Sie **Sammlung** im Listenfeld **Elementtypen** aus.  
  
6.  Wählen Sie die Zeile für das Attribut, das Sie bearbeiten möchten, und klicken Sie anschließend auf **Bearbeiten**.  
  
7.  Geben Sie im Feld **Name** den aktualisierten Namen für das Attribut ein. Eine Liste von Wörtern, die nicht als Attributnamen verwendet werden sollten, finden Sie unter [reservierte Wörter &#40;Master Data Services&#41;](../master-data-services/reserved-words-master-data-services.md).  
  
8.  Wählen Sie in der Liste **Attributtyp** einen anderen Typ aus.  
  
9. Klicken Sie auf **Speichern**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen Sie ein Text Attribut &#40;Master Data Services&#41;](../master-data-services/create-a-text-attribute-master-data-services.md)   
 [Löschen eines Attributs &#40;Master Data Services&#41;](../master-data-services/delete-an-attribute-master-data-services.md)   
 [Attribute &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)  
  
  
