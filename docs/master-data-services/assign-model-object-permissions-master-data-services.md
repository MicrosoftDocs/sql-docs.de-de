---
description: Zuweisen von Berechtigungen für Modellobjekte (Master Data Services)
title: Zuweisen von Berechtigungen für Modellobjekte
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- models [Master Data Services], assigning object permissions
- permissions [Master Data Services], assigning model object permissions
ms.assetid: 4b80148d-2318-415c-9479-28c240e48bcd
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ff78828f0ab43d8b6121a93835f42ff7568653c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339544"
---
# <a name="assign-model-object-permissions-master-data-services"></a>Zuweisen von Berechtigungen für Modellobjekte (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Weisen Sie in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]Berechtigungen zu Modellobjekten hinzu, wenn Sie einem Benutzer oder einer Gruppe Zugriff auf Daten im Funktionsbereich **Explorer** von [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]gewähren oder einen Benutzer oder eine Gruppe als Administrator festlegen möchten.  
  
> [!NOTE]  
>  Wenn Sie einem Modell eine Berechtigung zuweisen, wird allen anderen Modellen die Berechtigung implizit verweigert. Wenn Sie keine Modellobjektberechtigungen zuweisen, kann der Benutzer oder die Gruppe auf keine Daten in **Explorer** zugreifen.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung verfügen, auf den Funktionsbereich **Benutzer- und Gruppenberechtigungen** zuzugreifen.  
  
-   Sie müssen ein Modelladministrator sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-assign-model-object-permissions"></a>So weisen Sie Modellobjektberechtigungen zu  
  
1.  Klicken Sie in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]auf **Benutzer- und Gruppenberechtigungen**.  
  
2.  Wählen Sie auf der Seite **Benutzer** oder **Gruppen** die Zeile für den Benutzer oder die Gruppe aus, den bzw. die Sie bearbeiten möchten.  
  
3.  Klicken Sie auf **Ausgewähltem Benutzer bearbeiten**.  
  
4.  Klicken Sie auf die Registerkarte **Modelle** .  
  
5.  Wählen Sie optional ein Modell aus der Liste **Modell** aus.  
  
6.  Klicken Sie auf **Bearbeiten**.  
  
7.  Erweitern Sie die Struktur, und klicken Sie auf das Modellobjekt, dem Sie Berechtigungen zuweisen möchten.  
  
8.  Wählen Sie im Menü eine Kombination aus Lesen, Erstellen, Aktualisieren und Löschen oder Verweigern aus.  
  
9. Wählen Sie auf der obersten Ebene des Modells **Admin** aus, wenn Sie einen Benutzer zum Modelladministrator ernennen müssen.  
  
10. Klicken Sie auf **Speichern**.  
  
## <a name="next-steps"></a>Nächste Schritte  
  
-   (Optional) [Zuweisen von Hierarchieelementberechtigungen &#40;Master Data Services&#41;](../master-data-services/assign-hierarchy-member-permissions-master-data-services.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Löschen von Berechtigungen für Modell Objekte &#40;Master Data Services&#41;](../master-data-services/delete-model-object-permissions-master-data-services.md)   
 [Modell Objekt Berechtigungen &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [Erstellen eines Modelladministrators &#40;Master Data Services&#41;](../master-data-services/create-a-model-administrator-master-data-services.md)  
  
  
