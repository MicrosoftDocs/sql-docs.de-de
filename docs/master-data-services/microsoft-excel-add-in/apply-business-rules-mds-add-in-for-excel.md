---
description: Anwenden von Geschäftsregeln (MDS-Add-In für Excel)
title: Anwenden von Geschäftsregeln
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: cd106345-f561-4966-88d3-a69139b2bd78
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 6ac9299f7d1b8b436a1ccbbe06eef16df9ea3e7c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341444"
---
# <a name="apply-business-rules-mds-add-in-for-excel"></a>Anwenden von Geschäftsregeln (MDS-Add-In für Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Wenden Sie in [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)] Geschäftsregeln an, wenn Sie Daten überprüfen und bestätigen möchten, dass sie gültig sind. Sie können Überprüfungen korrigieren und die Daten erneut veröffentlichen.  
  
> [!NOTE]  
>  Die Datenüberprüfung findet automatisch statt, wenn Sie Daten veröffentlichen. Weitere Informationen finden Sie unter [Überprüfung &#40;Master Data Services&#41;](../../master-data-services/validation-master-data-services.md).  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über den Zugriff auf den Funktionsbereich **Explorer** verfügen.  
  
-   Sie müssen über ein aktives Arbeitsblatt mit von MDS verwalteten Daten verfügen.  
  
### <a name="to-apply-business-rules"></a>So wenden Sie Geschäftsregeln an  
  
1.  Klicken Sie in der Gruppe **Veröffentlichen und Validieren** auf **Regeln anwenden**.  
  
    > [!NOTE]  
    >  Die Anzahl der Elemente (Zeilen), die gleichzeitig überprüft werden, richtet sich nach einer Einstellung in [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)]. Weitere Informationen finden Sie unter [Business Rule Settings](../../master-data-services/system-settings-master-data-services.md#BusinessRules).  
  
2.  Die Daten werden anhand von Geschäftsregeln überprüft, und zwei Statusspalten werden angezeigt. Wenn diese Spalten nicht automatisch angezeigt werden, klicken Sie in der Gruppe **Veröffentlichen und Validieren** auf **Status anzeigen** , um sie anzuzeigen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht: Importieren von Daten aus Excel &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)  
  
  
