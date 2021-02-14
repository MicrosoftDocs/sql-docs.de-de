---
description: Erstellen eines Blattelements (Master Data Services)
title: Erstellen eines Blattelements
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- leaf members [Master Data Services], creating
- creating leaf members [Master Data Services]
- members [Master Data Services], creating leaf members
ms.assetid: 0499d3b3-d508-4d43-a740-19cf53ade9f1
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ffffd1fa28597a41ba1b8f7631e52f6264bb8203
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272671"
---
# <a name="create-a-leaf-member-master-data-services"></a>Erstellen eines Blattelements (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]erstellen Sie ein Blattelement, wenn Sie dem System Masterdaten hinzufügen möchten. Wenn Sie Daten in einer Massenoperation hinzufügen möchten, verwenden Sie stattdessen Stagingtabellen. Weitere Informationen finden Sie unter  [Importieren von Daten aus Tabellen &#40;Master Data Services&#41;](../master-data-services/import-data-from-tables-master-data-services.md)  
  
 Sie können zum Importieren von Daten auch [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] verwenden.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung für den Zugriff auf den Funktionsbereich **Explorer** verfügen.  
  
-   Sie müssen mindestens die Berechtigung **Erstellen** oder **Aktualisieren** für das Blattmodellobjekt für die Entität besitzen, der Sie ein Element hinzufügen. Die Berechtigung "Erstellen" ermöglicht die Erstellung eines Elements sowie die Bearbeitung des Attributs "Code". Die Berechtigung "Aktualisieren" ermöglicht auch die Aktualisierung anderer Attribute.  
  
     Weitere Informationen finden Sie unter [Sicherheit &#40;Master Data Services&#41;](../master-data-services/security-master-data-services.md).  
  
### <a name="to-create-a-leaf-member"></a>So erstellen Sie ein Blattelement  
  
1.  Wählen Sie auf der [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] -Startseite aus der Liste **Modell** ein Modell aus.  
  
2.  Wenn Sie Benutzer sind, wählen Sie eine geöffnete Version aus der Liste **Version** aus. Wenn Sie Administrator sind, wählen Sie eine Version mit dem Status „Geöffnet“ oder „Gesperrt“ aus der Liste **Version** aus.  
  
3.  Klicken Sie auf **Explorer**.  
  
4.  Zeigen Sie auf der Menüleiste auf **Entitäten** , und klicken Sie auf den Namen der Entität, der Sie ein Element hinzufügen möchten.  
  
5.  Klicken Sie auf **Mitglied hinzufügen**.  
  
6.  Vervollständigen Sie die Felder im Bereich **Details** .  
  
     Bei einem domänenbasierten Attribut, auf das ein Filter angewendet wird, wird die Liste der Attributwerte durch das übergeordnete Filterattribut eingeschränkt.  
  
     Weitere Informationen zu übergeordneten Filterattributen und domänenbasierten Attributen finden Sie unter [Erstellen eines domänenbasierten Attributs &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md)  
  
7.  Dies ist optional. Geben Sie im Feld **Anmerkungen** einen Kommentar dazu ein, warum das Element hinzugefügt wurde. Alle Benutzer, die Zugriff auf das Element haben, können die Anmerkung anzeigen.  
  
8.  Klicken Sie auf **OK**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen Sie ein konsolidiertes Element &#40;Master Data Services&#41;](../master-data-services/create-a-consolidated-member-master-data-services.md)   
 [Elemente &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)  
  
  
