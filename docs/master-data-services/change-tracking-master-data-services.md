---
description: Änderungsnachverfolgung (Master Data Services)
title: Change Tracking
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- change tracking [SQL Server]
ms.assetid: 5e879c65-0d38-454f-9a20-62a6e72c89f7
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 60fd195f5588ef38ab0a6e3c0c7ccbc4162bdc90
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272811"
---
# <a name="change-tracking-master-data-services"></a>Änderungsnachverfolgung (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]können Sie Änderungsnachverfolgungsgruppen verwenden, um Aktionen durchzuführen, wenn sich ein Attributwert ändert. Verwenden Sie die Änderungsnachverfolgung, wenn Sie nicht wissen, was der neue Wert ist, aber wissen möchten, ob eine Änderung aufgetreten ist.  
  
## <a name="configuring-change-tracking"></a>Konfigurieren der Änderungsnachverfolgung  
 Um die Änderungsnachverfolgung zu konfigurieren, fügen Sie einer Änderungsnachverfolgungsgruppe ein Attribut hinzu. Die Gruppe kann ein oder viele Attribute enthalten. Erstellen Sie dann eine Geschäftsregel, um die Aktion zu definieren, die ausgeführt wird, wenn sich eines der Attribute in der Gruppe ändert.  
  
> [!NOTE]  
>  Änderungsnachverfolgungs-Geschäftsregeln stufen bereitgestellte (importierte) Daten als geändert ein.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Taskbeschreibung|Thema|  
|----------------------|-----------|  
|Hinzufügen von Attributen zu einer Änderungsnachverfolgungsgruppe.|[Hinzufügen von Attributen zu einer Änderungsnachverfolgungsgruppe &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md)|  
|Erstellen einer Geschäftsregel, die Aktionen auf der Grundlage von Änderungen an Attributwerten initiiert.|[Initiieren von Aktionen auf der Grundlage von Attributwertänderungen &#40;Master Data Services&#41;](../master-data-services/initiate-actions-based-on-attribute-value-changes-master-data-services.md)|  
  
## <a name="related-content"></a>Verwandte Inhalte  
  
-   [Überprüfung &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md)  
  
-   [Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
-   [Attribute &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)  
  
  
