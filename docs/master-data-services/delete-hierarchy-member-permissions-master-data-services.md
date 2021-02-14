---
description: Löschen von Hierarchieelementberechtigungen (Master Data Services)
title: Löschen von Hierarchieelementberechtigungen
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- deleting member permissions [Master Data Services]
- members [Master Data Services], deleting permissions
- permissions [Master Data Services], deleting member permissions
ms.assetid: 7f22d5e2-70c1-422c-99c2-e995a47d812a
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: aff4e2f70faf333daf9c977d2a7fdfed2d15a169
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350243"
---
# <a name="delete-hierarchy-member-permissions-master-data-services"></a>Löschen von Hierarchieelementberechtigungen (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Löschen Sie in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]Modellobjektberechtigungen, um erfolgte Zuweisungen zu entfernen.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung verfügen, auf den Funktionsbereich **Benutzer- und Gruppenberechtigungen** zuzugreifen.  
  
-   Sie müssen ein Modelladministrator sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-delete-hierarchy-member-permissions"></a>So löschen Sie Hierarchieelementberechtigungen  
  
1.  Klicken Sie in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]auf **Benutzer- und Gruppenberechtigungen**.  
  
2.  Wählen Sie auf der Seite **Benutzer** oder **Gruppen** die Zeile für den Benutzer oder die Gruppe aus, den bzw. die Sie bearbeiten möchten.  
  
3.  Klicken Sie auf **Ausgewähltem Benutzer bearbeiten**.  
  
4.  Klicken Sie auf die Registerkarte **Hierarchieelemente** .  
  
5.  Wählen Sie aus der Liste **Modell** ein Modell aus.  
  
6.  Wählen Sie aus der Liste **Version** eine Version aus.  
  
7.  Klicken Sie auf **Bearbeiten**.  
  
8.  Suchen Sie den Strukturknoten mit der Berechtigung im Bereich **Hierarchieelementberechtigungen** .  
  
9. Klicken Sie auf den Strukturknoten, und klicken Sie im Kontextmenü auf **Keine** .  
  
    > [!NOTE]  
    >  Sie können keine Berechtigung von einem Benutzer entfernen, wenn die Berechtigung von einer Gruppe geerbt wird. Sie müssen stattdessen die Berechtigung von der Gruppe entfernen.  
  
10. Klicken Sie auf **Speichern**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Hierarchie Element Berechtigungen &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)   
 [Zuweisen von Hierarchieelementberechtigungen &#40;Master Data Services&#41;](../master-data-services/assign-hierarchy-member-permissions-master-data-services.md)  
  
  
