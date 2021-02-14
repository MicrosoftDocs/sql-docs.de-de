---
description: Zurücksetzen des Elementrevisionsverlaufs (Master Data Services)
title: Zurücksetzen des Elementrevisionsverlaufs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: d39d3474-20e7-429f-9c8d-fcc4eb0854fc
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ec99fc4d464037b4051da46e293efb4b2fb6969a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355287"
---
# <a name="rollback-member-revision-history-master-data-services"></a>Zurücksetzen des Elementrevisionsverlaufs (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Ein Elementrevisionsverlauf wird jedes Mal aufgezeichnet, wenn ein Element geändert wird. Sie können einen Elementrevisionsverlauf auf eine frühere Version zurücksetzen.  
  
## <a name="prerequisites"></a>Voraussetzungen  
  
-   Sie benötigen die Berechtigung, mindestens eines der Attribute des ausgewählten Elements zu aktualisieren. Wenn Sie einen Revisionsverlauf zurücksetzen, werden alle Attributwerte, die aktualisiert werden können, auf die vorherigen Versionswerte zurückgesetzt.  
  
-   Der Revisionsverlauf steht nur dann zur Verfügung, wenn der Transaktionsprotokolltyp der Entität "Element" ist.  
  
 **Zurücksetzen eines Elementrevisionsverlaufs**  
  
1.  Klicken Sie in Master Data Manager auf "Explorer".  
  
2.  Wählen Sie die Entität und das Element aus, das zurückgesetzt werden soll.  
  
3.  Klicken Sie auf **Verlauf anzeigen**.  
  
4.  Wählen Sie die Revision aus, die zurückgesetzt werden soll, und klicken Sie anschließend auf **Zurücksetzen**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Element Revisions Verlauf &#40;Master Data Services&#41;](../master-data-services/member-revision-history-master-data-services.md)   
 [Ändern des Transaktionsprotokolltyps von Entitäten &#40;Master Data Services&#41;](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md)  
  
  
