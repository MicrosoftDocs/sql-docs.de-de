---
description: Automatisches Generieren von Codeattributwerten (Master Data Services)
title: Automatisches Generieren von Codeattributwerten
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 19b354ee-2906-4cc7-ba2f-32b4543bddcf
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 82d501f5d7c5a4a5b79e55571a7351225833680b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341488"
---
# <a name="automatically-generate-code-attribute-values-master-data-services"></a>Automatisches Generieren von Codeattributwerten (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] können Sie Werte für das Codeattribut einer Entität automatisch generieren lassen, wenn Sie möchten, dass dem Codewert bei jeder Erstellung eines neuen Elements automatisch eine ganze Zahl zugewiesen wird.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung verfügen, auf den Funktionsbereich **System Verwaltung** zuzugreifen.  
  
-   Sie müssen ein Modelladministrator sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Die Entität muss vorhanden sein. Weitere Informationen finden Sie unter [Erstellen einer Entität &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md).  
  
### <a name="to-automatically-generate-code-values"></a>So generieren Sie automatisch Codewerte  
  
1.  Klicken Sie in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]auf **Systemverwaltung**.  
  
2.  Wählen Sie auf der Seite **Modell verwalten** die Zeile für das Modell aus, das die Entität enthält, die Sie bearbeiten möchten, und klicken Sie dann auf **Entitäten**.  
  
3.  Wählen Sie auf der Seite **Entität verwalten** die Zeile der Entität aus, für die Sie Code generieren möchten, und klicken Sie dann auf **Bearbeiten**.  
  
4.  Aktivieren Sie das Kontrollkästchen **Codewerte automatisch erstellen** .  
  
5.  Geben Sie im Feld **Start bei** eine Zahl ein, die inkrementiert werden soll. Wenn Elemente bereits vorhanden sind, wird der Code auf Grundlage des höchsten vorhandenen Werts festgelegt. Wenn der höchste vorhandene Codewert z.B. 299 ist, wird der Codewert des nächsten Elements auf 300 festgelegt.  
  
6.  Klicken Sie auf **Speichern**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Automatische Code Erstellung &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md)   
 [Automatisches Generieren von anderen Attributwerten als Code &#40;Master Data Services&#41;](../master-data-services/automatically-generate-attribute-values-other-than-code-master-data-services.md)  
  
  
