---
description: Erfordern von Attributwerten (Master Data Services)
title: Erfordern von Attributwerten
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], requiring attribute values
- attributes [Master Data Services], requiring values
ms.assetid: a360ef13-0c34-43b8-a87e-2f5d8732d30e
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 8e863605632b82b18f0dbe824fd45adbeaf22274
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353772"
---
# <a name="require-attribute-values-master-data-services"></a>Erfordern von Attributwerten (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Erfordern Sie in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]Attributwerte, wenn Sie sicherstellen möchten, dass die Masterdaten vollständig sind.  
  
> [!NOTE]  
>  Elemente, denen domänenbasierte Attributwerte fehlen, werden nicht in abgeleiteten Hierarchien angezeigt, die auf diesen Beziehungen basieren.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung verfügen, auf den Funktionsbereich **System Verwaltung** zuzugreifen.  
  
-   Sie müssen ein Modelladministrator sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-require-attribute-values"></a>So erfordern Sie Attributwerte  
  
1.  Klicken Sie in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]auf **Systemverwaltung**.  
  
2.  Zeigen Sie auf der Menüleiste auf **Verwalten** , und klicken Sie auf **Geschäftsregeln**.  
  
3.  Wählen Sie auf der Seite **Geschäftsregeln** in der Dropdownliste **Modell** ein Modell aus.  
  
4.  Wählen Sie in der Dropdownliste **Entität** eine Entität aus.  
  
5.  Wählen Sie in der Dropdownliste **Member Types** (Elementtypen) einen Typ des Elements aus, auf das die Geschäftsregel angewendet werden soll.  
  
6.  Klicken Sie auf **Hinzufügen**.  
  
7.  Geben Sie in das Feld **Name** einen Namen für die neue Geschäftsregel ein.  
  
8.  Optional können Sie im Feld **Beschreibung** die Beschreibung der Geschäftsregel eingeben.  
  
9. Klicken Sie im **Then** -Bereich auf **Hinzufügen**. Ein Bereich wird angezeigt.  
  
10. Wählen Sie in der Dropdownliste **Operator** eine **erforderliche Aktion** aus.  
  
11. Wählen Sie ein Attribut aus der Dropdownliste **Attribut** aus.  
  
12. Klicken Sie auf **Speichern**. Eine neue Zeile wird dem **Then** -Raster hinzugefügt.  
  
13. Klicken Sie auf **Speichern**.  
  
14. Klicken Sie auf **Alle veröffentlichen**.  
  
15. Klicken Sie im Bestätigungsdialogfeld auf **OK**. Der Wert in der Spalte **Geschäftsregelstatus** ist **Aktiv**.  
  
## <a name="next-steps"></a>Nächste Schritte  
  
-   Führen Sie zum Anwenden von Geschäftsregeln auf Daten eine der folgenden Prozeduren aus:  
  
    -   [Überprüfen von bestimmten Elementen auf Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
    -   [Überprüfen einer Version anhand von Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Master Data Services von Geschäftsregeln &#40;&#41;](../master-data-services/business-rules-master-data-services.md)   
 [Abgeleitete Hierarchien &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)  
  
  
