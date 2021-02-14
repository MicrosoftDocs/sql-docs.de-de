---
description: Zuweisen von Hierarchieelementberechtigungen (Master Data Services)
title: Zuweisen von Hierarchieelementberechtigungen
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- permissions [Master Data Services], assigning member permissions
- members [Master Data Services], assigning permissions
ms.assetid: e1b8b46a-7cd1-4a7d-9345-dd7df081e145
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: eba91bdad75f15bc3482f3752c7fe04b90afec71
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339505"
---
# <a name="assign-hierarchy-member-permissions-master-data-services"></a>Zuweisen von Hierarchieelementberechtigungen (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Weisen Sie Hierarchieelementen Berechtigungen zu, um Benutzer oder Gruppen Zugriff auf die Anzeige von Daten im Funktionsbereich **Explorer** von [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]zu geben.  
  
 Hierarchieelementberechtigungen sind optional. Sie bieten zusätzliche Granularität für die Modellierung von Objektberechtigungen; diese sind erforderlich.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung verfügen, auf den Funktionsbereich **Benutzer- und Gruppenberechtigungen** zuzugreifen.  
  
-   Sie müssen ein Modelladministrator sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-assign-hierarchy-member-permissions"></a>So weisen Sie Hierarchieelementberechtigungen zu  
  
1.  Klicken Sie in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]auf **Benutzer- und Gruppenberechtigungen**.  
  
2.  Wählen Sie auf der Seite **Benutzer** oder **Gruppen** die Zeile für den Benutzer oder die Gruppe aus, den bzw. die Sie bearbeiten möchten.  
  
3.  Klicken Sie auf **Ausgewähltem Benutzer bearbeiten**.  
  
4.  Klicken Sie auf die Registerkarte **Hierarchieelemente** .  
  
5.  Wählen Sie aus der Liste **Modell** ein Modell aus.  
  
6.  Wählen Sie aus der Liste **Version** eine Version aus.  
  
7.  Wählen Sie aus der Liste **Hierarchie** eine Hierarchie aus.  
  
8.  Klicken Sie auf **Bearbeiten**.  
  
9. Erweitern Sie die Struktur, und klicken Sie auf den Hierarchieknoten, dem Sie Berechtigungen zuweisen möchten.  
  
10. Wählen Sie im Menü eine Kombination der Berechtigungen **Erstellen**, **Lesen, Aktualisieren** , **Löschen** oder **Verweigern** aus.  
  
11. Klicken Sie auf **Speichern**.  
  
    > [!NOTE]  
    >  Hierarchieelementberechtigungen werden nicht sofort wirksam. Weitere Informationen finden Sie unter [Sofortiges Anwenden von Elementberechtigungen &#40;Master Data Services&#41;](../master-data-services/immediately-apply-member-permissions-master-data-services.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Löschen von Hierarchie Element Berechtigungen &#40;Master Data Services&#41;](../master-data-services/delete-hierarchy-member-permissions-master-data-services.md)   
 [Zuweisen von Berechtigungen für Modell Objekte &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)   
 [Berechtigungen für Hierarchieelemente &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)  
  
  
