---
description: Löschen eines Modells (Master Data Services)
title: Löschen eines Modells
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- deleting models [Master Data Services]
- models [Master Data Services], deleting models
ms.assetid: f0ad3cc4-aed7-47c8-94bc-2971fe9fe871
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4e7465c52f6141e0eb78cc01c74d338c688d1c7c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338988"
---
# <a name="delete-a-model-master-data-services"></a>Löschen eines Modells (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Löschen Sie ein Modell, um das Modell und alle zugehörigen Daten aus [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]zu entfernen.  
  
> [!NOTE]  
>  Nach Abschluss dieser Prozedur sind alle Objekte und alle Daten aus allen Versionen des Modells permanent gelöscht.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung verfügen, auf den Funktionsbereich **System Verwaltung** zuzugreifen.  
  
-   Sie müssen ein Modelladministrator sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-delete-a-model"></a>So löschen Sie ein Modell  
  
1.  Klicken Sie in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]auf **Systemverwaltung**.  
  
2.  Zeigen Sie auf der Seite **Modellansicht** auf der Menüleiste auf **Verwalten** , und klicken Sie auf **Modelle**.  
  
3.  Wählen Sie auf der Seite **Modelle verwalten** im Raster die Zeile des Modells aus, das Sie löschen möchten.  
  
4.  Klicken Sie auf **Löschen**.  
  
5.  Klicken Sie im Bestätigungsdialogfeld auf **OK**.  
  
6.  Klicken Sie im zusätzlichen Bestätigungsdialogfeld auf **OK**.  
  
 Die Spalte **Status** im Raster zeigt den Status des Vorgangs für das Modell. Wenn Sie auf die Schaltfläche **Modell speichern** klicken, wird das ![Aktualisier](../master-data-services/media/mds-model-status-updating.png "Wird aktualisiert") Bare Bild angezeigt, das angibt, dass das Modell aktualisiert wird. Wenn beim Erstellen oder Bearbeiten eines Modells Fehler auftreten, wird das ![Fehler](../master-data-services/media/mds-model-status-error.png "Fehler") Bild angezeigt. Andernfalls ist der Status OK, und das Bild ![OK](../master-data-services/media/mds-model-status-ok.png "OK") angezeigt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Modelle &#40;Master Data Services&#41;](../master-data-services/models-master-data-services.md)   
 [Erstellen eines Modells &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md)  
  
  
