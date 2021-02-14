---
title: Rekursive Hierarchien
description: In Master Data Services enthält eine rekursive Hierarchie eine rekursive Beziehung, die vorhanden ist, wenn eine Entität über ein domänenbasiertes Attribut verfügt, das auf der Entität basiert.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- recursive hierarchies [Master Data Services]
- hierarchies [Master Data Services], recursive hierarchies
ms.assetid: 9408c6ea-d9c4-4a0b-8a1b-1457fb6944af
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e5aaae9f766832d64c3ca9e59940b933fbcda1b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353709"
---
# <a name="recursive-hierarchies-master-data-services"></a>Rekursive Hierarchien (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]ist eine rekursive Hierarchie ist eine abgeleitete Hierarchie, die eine rekursive Beziehung einschließt. Eine rekursive Beziehung ist vorhanden, wenn eine Entität über ein domänenbasiertes Attribut auf Grundlage der Entität selbst verfügt.  
  
## <a name="recursive-hierarchy-example"></a>Beispiel für eine rekursive Hierarchie  
 Ein typisches Beispiel für eine rekursive Hierarchie ist eine Organisationsstruktur. In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]erstellen Sie dazu eine Entität "Employee" mit einem domänenbasierten Attribut "Manager". Das Manager-Attribut wird aus der Liste der Mitarbeiter aufgefüllt. In dieser Beispielorganisation können alle Mitarbeiter Manager sein.  
  
 ![mds_conc_recursive_table_w_data](../master-data-services/media/mds-conc-recursive-table-w-data.gif "mds_conc_recursive_table_w_data")  
  
 Sie können eine abgeleitete Hierarchie erstellen, die die Beziehung zwischen der Employee-Entität und dem domänenbasierten Attribut "Manager" hervorhebt.  
  
 ![mds_conc_recursive_UI_structure](../master-data-services/media/mds-conc-recursive-ui-structure.gif "mds_conc_recursive_UI_structure")  
  
 Um jedes Element nur einmal in die Hierarchie einzuschließen, können Sie NULL-Beziehungen verankern. Wenn Sie dies tun, werden Elemente mit leeren domänenbasierten Attributwerten auf der obersten Ebene der Hierarchie angezeigt.  
  
 ![mds_conc_recursive_UI_example_anchored](../master-data-services/media/mds-conc-recursive-ui-example-anchored.gif "mds_conc_recursive_UI_example_anchored")  
  
 Wenn Sie keine NULL-Beziehungen verankern, werden Elemente mehrmals eingeschlossen. Alle Elemente werden auf der obersten Ebene angezeigt. Sie werden auch unter den Elementen angezeigt, deren Attribute sie sind.  
  
 ![mds_conc_recursive_UI_example_nonanchored](../master-data-services/media/mds-conc-recursive-ui-example-nonanchored.gif "mds_conc_recursive_UI_example_nonanchored")  
  
 In diesem Beispiel befindet sich Marcia auf der obersten Ebene. Sie ist kein Manager von Mitarbeitern, da sie nicht als domänenbasierter Attributwert für andere Employee-Elemente verwendet wird. Robert dagegen hat unter sich eine Ebene, da Marcia Robert als Managerattributwert hat.  
  
## <a name="rules"></a>Regeln  
  
-   Eine abgeleitete Hierarchie kann höchstens eine rekursive Beziehung aufweisen. Sie kann jedoch andere abgeleitete Beziehungen aufweisen (z. B. eine abgeleitete Hierarchie, die eine rekursive Beziehung zwischen Manager und Mitarbeiter aufweist, kann ebenso Beziehungen zwischen Land und Manager und zwischen Mitarbeiter und Store aufweisen).  
  
-   Sie können Elementen in einer rekursiven Hierarchie (auf der Registerkarte **Hierarchieelemente** ) keine Elementberechtigungen zuweisen.  
  
-   Rekursive Hierarchien können keine kreisförmigen Beziehungen einschließen. Katherine kann z. B. nicht Sandeeps Managerin sein, wenn Sandeep ihr Manager ist. Auch kann Katherine nicht ihre eigener Managerin sein.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Taskbeschreibung|Thema|  
|----------------------|-----------|  
|Erstellen Sie eine abgeleitete Hierarchie.|[Erstellen einer abgeleiteten Hierarchie &#40;Master Data Services&#41;](../master-data-services/create-a-derived-hierarchy-master-data-services.md)|  
|Ändern Sie den Namen einer vorhandenen abgeleiteten Hierarchie.|[Ändern des Namens einer abgeleiteten Hierarchie &#40;Master Data Services&#41;](../master-data-services/change-a-derived-hierarchy-name-master-data-services.md)|  
|Löschen Sie eine vorhandene abgeleitete Hierarchie.|[Löschen einer abgeleiteten Hierarchie &#40;Master Data Services&#41;](../master-data-services/delete-a-derived-hierarchy-master-data-services.md)|  
  
## <a name="related-content"></a>Verwandte Inhalte  
  
-   [Domänenbasierte Attribute &#40;Master Data Services&#41;](../master-data-services/domain-based-attributes-master-data-services.md)  
  
-   [Abgeleitete Hierarchien &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)  
  
  
