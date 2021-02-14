---
description: Überprüfen von bestimmten Elementen auf Geschäftsregeln (Master Data Services)
title: Überprüfen von bestimmten Elementen auf Geschäftsregeln
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- applying business rules [Master Data Services]
- business rules [Master Data Services], applying to select members
ms.assetid: 2288ef43-5392-47ea-b651-ec25e5692a14
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: da4e42431ed84d67bd0ffa46f428dba23edcb215
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338181"
---
# <a name="validate-specific-members-against-business-rules-master-data-services"></a>Überprüfen von bestimmten Elementen auf Geschäftsregeln (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Wenn Sie Teilmengen von Elementen aktualisieren oder mit Geschäftsregeln abgleichen möchten, können Sie in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]Geschäftsregeln selektiv anwenden.  
  
> [!NOTE]  
>  Wenn Sie Geschäftsregeln auf alle Elemente in einer Version eines Modells anwenden möchten, gehen Sie auf die Seite [Überprüfen einer Version anhand von Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md).  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung für den Zugriff auf den Funktionsbereich **Explorer** verfügen.  
  
-   Sie müssen mindestens die Berechtigung **Aktualisieren** für das Modellobjekt haben, auf das Sie Geschäftsregeln anwenden möchten.  
  
### <a name="to-apply-business-rules-selectively"></a>So wenden Sie Geschäftsregeln selektiv an  
  
1.  Wählen Sie auf der [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] -Startseite aus der Dropdownliste **Modell** ein Modell aus.  
  
2.  Wählen Sie aus der Dropdownliste **Version** eine Version aus.  
  
3.  Klicken Sie auf die Registerkarte **Explorer** .  
  
4.  Zeigen Sie in der Menüleiste auf **Entitäten** und klicken Sie auf den Namen der Entität mit den Elementen, auf die Sie Regeln anwenden möchten.  
  
5.  Klicken Sie auf **Regeln anwenden**. Die Geschäftsregeln werden nur auf die im Raster angezeigten Elemente angewendet.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Überprüfen einer Version anhand von Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)   
 [Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
  
