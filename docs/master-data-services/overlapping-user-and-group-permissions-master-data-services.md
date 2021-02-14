---
title: Überlappende Benutzer- und Gruppenberechtigungen
description: Erfahren Sie, wie Berechtigungen von Gruppenmitgliedschaften und Berechtigungen, die Benutzern zugewiesen werden, auf den Registerkarten Modelle und Hierarchie Elemente in Master Data Services interagieren.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- users [Master Data Services], resolving permissions
- permissions [Master Data Services], user and group overlaps
- groups [Master Data Services], resolving permissions
ms.assetid: 31c3cf7d-17d4-4474-b6a7-ffcb9fc45b37
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 9df096d687fbd0ea2730c328ee3a4d25b062967f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340263"
---
# <a name="overlapping-user-and-group-permissions-master-data-services"></a>Überlappende Benutzer- und Gruppenberechtigungen (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Benutzerberechtigungen basieren auf:  
  
-   Berechtigungen aus Gruppenmitgliedschaften  
  
-   Dem Benutzer explizit zugewiesenen Berechtigungen  
  
 Wenn ein Benutzer mehreren Gruppen angehört und diese Gruppen Zugriff auf [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]haben, gelten die folgenden Regeln:  
  
-   Mit **Verweigern** werden alle anderen Berechtigungen überschrieben. Wenn die Objektberechtigung in einer Gruppe **Verweigern** lautet, so ist diese Berechtigung effektiv.  
  
-   Die Zugriffsberechtigung setzt sich aus allen gültigen Gruppenberechtigungen zusammen. Wenn die Objektberechtigung in einer Gruppe **Erstellen** und in einer anderen Gruppe **Aktualisieren** lautet, so lautet die effektive Berechtigung **Erstellen** und **Aktualisieren**.  
  
 Diese Regeln gelten sowohl für die Registerkarte **Modelle** als auch für die Registerkarte **Hierarchieelemente** . Berechtigungen werden für jede Registerkarte aufgelöst und dann kombiniert. Weitere Informationen finden Sie unter [Vorgehensweise: Festlegen von Berechtigungen &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md).  
  
> [!NOTE]  
>  Sie können die Auflösung überlappender Benutzer- und Gruppenberechtigungen in der Benutzeroberfläche anzeigen. Sowohl die Registerkarte **Modelle** als auch die Registerkarte **Hierarchieelemente** verfügt über eine Dropdownliste, aus der Sie **Effektiv** auswählen können, um effektive Berechtigungen anzuzeigen.  
  
## <a name="example-1"></a>Beispiel 1  
 ![mds_conc_user_group_ex_1](../master-data-services/media/mds-conc-user-group-ex-1.gif "mds_conc_user_group_ex_1")  
  
 Der Benutzer gehört der Gruppe 1 und der Gruppe 2 an.  
  
 Der Benutzer verfügt für die Entität „Product“ über die Berechtigung **Lesen** .  
  
 Gruppe 1 verfügt für die Entität „Product“ über die Berechtigung **Aktualisieren** .  
  
 Gruppe 2 verfügt für die Entität „Product“ über die Berechtigung **Lesen** .  
  
 Ergebnis: Die effektive Berechtigung des Benutzers für die Entität „Product“ lautet **Aktualisieren** .  
  
## <a name="example-2"></a>Beispiel 2  
 ![mds_conc_user_group_ex_2](../master-data-services/media/mds-conc-user-group-ex-2.gif "mds_conc_user_group_ex_2")  
  
 Der Benutzer gehört der Gruppe 1 und der Gruppe 2 an.  
  
 Der Benutzer verfügt für die Entität „Product“ über die Berechtigung **Lesen** .  
  
 Gruppe 1 verfügt für die Entität „Product“ über die Berechtigung **Aktualisieren** .  
  
 Gruppe 2 verfügt für die Entität „Product“ über die Berechtigung **Verweigern** .  
  
 Ergebnis: Die effektive Berechtigung des Benutzers für die Entität „Product“ lautet **Verweigern** .  
  
## <a name="example-3"></a>Beispiel 3  
 ![mds_conc_user_group_ex_3](../master-data-services/media/mds-conc-user-group-ex-3.gif "mds_conc_user_group_ex_3")  
  
 Der Benutzer gehört der Gruppe 1 und der Gruppe 2 an.  
  
 Der Benutzer verfügt für eine Gruppe von Elementen unter einem Hierarchieknoten über die Berechtigung **Aktualisieren** .  
  
 Gruppe 1 verfügt für eine Gruppe von Elementen unter einem Hierarchieknoten über die Berechtigung **Lesen** .  
  
 Gruppe 2 verfügt für eine Gruppe von Elementen unter einem Hierarchieknoten über die Berechtigung **Lesen** .  
  
 Ergebnis: Die effektive Berechtigung des Benutzers für die Elemente lautet **Aktualisieren** .  
  
## <a name="see-also"></a>Weitere Informationen  
 [Wie Berechtigungen &#40;Master Data Services bestimmt werden&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md)   
 [Überlappende Modell- und Elementberechtigungen &#40;Master Data Services&#41;](../master-data-services/overlapping-model-and-member-permissions-master-data-services.md)  
  
  
