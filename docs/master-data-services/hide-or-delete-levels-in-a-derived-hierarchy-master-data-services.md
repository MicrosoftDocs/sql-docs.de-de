---
description: Ausblenden oder Löschen von Ebenen in einer abgeleiteten Hierarchie (Master Data Services)
title: Ausblenden oder Löschen von Ebenen in einer abgeleiteten Hierarchie
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- derived hierarchies, hiding levels
- derived hierarchies, deleting levels
ms.assetid: e00582b9-9415-4b66-b4a7-9f590d83875f
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e1c63de9accf8b3de74793753f40ee2e01d0546b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344769"
---
# <a name="hide-or-delete-levels-in-a-derived-hierarchy-master-data-services"></a>Ausblenden oder Löschen von Ebenen in einer abgeleiteten Hierarchie (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]können Sie eine Ebene in einer abgeleiteten Hierarchie ausblenden, wenn die Ebene zur Gruppierung benötigt wird, aber nicht angezeigt werden soll. Löschen Sie eine Ebene, wenn Sie sie nicht zur Gruppierung verwenden möchten.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung verfügen, auf den Funktionsbereich **System Verwaltung** zuzugreifen.  
  
-   Sie müssen ein Modelladministrator sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-hide-or-delete-levels-in-a-derived-hierarchy"></a>So löschen oder blenden Sie Ebenen in einer abgeleiteten Hierarchie aus  
  
1.  Klicken Sie in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]auf **Systemverwaltung**.  
  
2.  Zeigen Sie auf der Menüleiste auf **Verwalten** , und klicken Sie auf **Abgeleitete Hierarchien**.  
  
3.  Wählen Sie auf der Seite **Verwaltung abgeleiteter Hierarchien** aus der Liste **Modell** ein Modell aus.  
  
4.  Wählen Sie die Zeile der abgeleiteten Hierarchie aus, die Sie bearbeiten möchten.  
  
5.  Klicken Sie auf **Bearbeiten**.  
  
6.  Im Bereich **Aktuelle Ebenen** :  
  
    -   Um eine Ebene auszublenden, klicken Sie auf eine andere als die oberste oder unterste Ebene. Wählen Sie aus der Liste **Sichtbar** den Eintrag **Nein** aus. Klicken Sie dann auf **Ausgewähltes Hierarchieelement speichern**.  
  
    -   Um die oberste Ebene zu löschen, klicken Sie auf **Ausgewähltes Hierarchieelement löschen**. Klicken Sie im Bestätigungsdialogfeld auf **OK**. Sie können nur die oberste Ebene löschen.  
  
## <a name="see-also"></a>Weitere Informationen  
    
 [Abgeleitete Hierarchien &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)  
  
  
