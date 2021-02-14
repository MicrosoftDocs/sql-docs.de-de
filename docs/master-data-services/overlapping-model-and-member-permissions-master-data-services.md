---
description: Überlappende Modell- und Elementberechtigungen (Master Data Services)
title: Überlappende Modell- und Elementberechtigungen
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- models [Master Data Services], effective permissions
- permissions [Master Data Services], model and member overlaps
- members [Master Data Services], effective permissions
ms.assetid: 9fd7a555-43bf-4796-a8b6-1ca63a291216
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f3227ef69915e606f514b5ecf36e5f1656db733e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340280"
---
# <a name="overlapping-model-and-member-permissions-master-data-services"></a>Überlappende Modell- und Elementberechtigungen (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Die einem Element zugewiesene Berechtigung kann mit einer einem Modellobjekt zugewiesenen Berechtigung überlappen. Bei einer Überlappung tritt die restriktivere Berechtigung in Kraft.  
  
 Wenn ein Element über eine Berechtigung verfügt, die sich von der des zugehörigen Modellobjekts unterscheidet, gelten die folgenden Regeln:  
  
-   Mit **Verweigern** werden alle anderen Berechtigungen überschrieben.  
  
-   **Administratorrechte** auf der Modellebene setzen alle anderen Berechtigungen außer Kraft und werden auf untergeordneten Ebenen in "Alle" (CRUD) geändert.  
  
-   Die effektive Zugriffsberechtigungen überschneidet sich mit Berechtigungen für Elemente und Attribute.  
  
     Wenn Elementberechtigungen beispielsweise **Erstellen** und **Aktualisieren** beinhalten, lautet die Berechtigung für Attribute **Aktualisieren**. Die effektive Berechtigung lautet **Aktualisieren**.  
  
 Das folgende Bild zeigt, welche Berechtigungen für einen einzelnen Attributwert wirksam werden, wenn Attributberechtigungen sich von Elementberechtigungen unterscheiden.  
  
 ![mds_conc_security_member_overlap_table](../master-data-services/media/mds-conc-security-member-overlap-table.gif "mds_conc_security_member_overlap_table")  
  
## <a name="example-1"></a>Beispiel 1  
 ![mds_conc_overlap_model_1](../master-data-services/media/mds-conc-overlap-model-1.gif "mds_conc_overlap_model_1")  
  
 Auf der Registerkarte **Modelle** verfügt die Product-Entität über die zugewiesene Berechtigung **Aktualisieren** . Diese Berechtigung wird von allen Attributen in der Entität geerbt.  
  
 Auf der Registerkarte **Hierarchieelemente** verfügt der Unterkategorieknoten "Mountain Bikes" in einer abgeleiteten Hierarchie über die zugewiesene Berechtigung **Aktualisieren** .  
  
 Ergebnis: In **Explorer** verfügt der Benutzer über die Berechtigung **Aktualisieren** für alle Attributwerte für alle Elemente im Knoten "Mountain Bikes". Alle anderen Elemente und Attribute werden ausgeblendet.  
  
 ![mds_conc_overlap_model_example_1](../master-data-services/media/mds-conc-overlap-model-example-1.gif "mds_conc_overlap_model_example_1")  
  
## <a name="example-2"></a>Beispiel 2  
 ![mds_conc_overlap_model_2](../master-data-services/media/mds-conc-overlap-model-2.gif "mds_conc_overlap_model_2")  
  
 Auf der Registerkarte **Modelle** verfügt das Subcategory-Attribut über die zugewiesene Berechtigung **Aktualisieren** .  
  
 Auf der Registerkarte **Hierarchieelemente** wird dem Unterkategorieknoten "Mountain Bikes" in einer abgeleiteten Hierarchie die **Leseberechtigung** explizit zugewiesen.  
  
 Ergebnis: In **Explorer** verfügt der Benutzer über die **Leseberechtigung** für die Subcategory-Attributwerte für die Elemente im Knoten "Mountain Bikes". Alle anderen Elemente und Attribute werden ausgeblendet.  
  
 ![mds_conc_overlap_model_example_2](../master-data-services/media/mds-conc-overlap-model-example-2.gif "mds_conc_overlap_model_example_2")  
  
## <a name="example-3"></a>Beispiel 3  
 ![mds_conc_overlap_model_3](../master-data-services/media/mds-conc-overlap-model-3.gif "mds_conc_overlap_model_3")  
  
 Auf der Registerkarte **Modelle** verfügt das Subcategory-Attribut über die zugewiesene **Leseberechtigung** .  
  
 Auf der Registerkarte **Hierarchieelemente** wird der Unterkategorie "Mountain Bikes" in einer abgeleiteten Hierarchie die Berechtigung **Aktualisieren** explizit zugewiesen.  
  
 Ergebnis: In **Explorer** verfügt der Benutzer über die **Leseberechtigung** für die Attributwerte. Alle anderen Elemente und Attribute werden ausgeblendet.  
  
 ![mds_conc_overlap_model_example_2](../master-data-services/media/mds-conc-overlap-model-example-2.gif "mds_conc_overlap_model_example_2")  
  
## <a name="see-also"></a>Weitere Informationen  
 [Wie Berechtigungen &#40;Master Data Services bestimmt werden&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md)   
 [Überlappende Benutzer- und Gruppenberechtigungen &#40;Master Data Services&#41;](../master-data-services/overlapping-user-and-group-permissions-master-data-services.md)  
  
  
