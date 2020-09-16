---
description: Verschieben von Elementen in einer Projektmappe
title: Verschieben von Elementen in einer Projektmappe
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- moving items
- solutions [SQL Server Management Studio], item relocation
- relocating items
ms.assetid: b40a24eb-f528-4e70-b26e-5eaf6e0ed1ed
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 871f86e949eb8c4567b6998f5f664de554e4f466
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88497291"
---
# <a name="move-items-in-a-solution"></a>Verschieben von Elementen in einer Projektmappe
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Projektelemente in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]-Projekten sind Abfragen, Verbindungen und sonstige Dateien. Sie können Abfragen und sonstige Dateien zwischen Projekten im Projektmappen-Explorer verschieben. Verbindungen können aber nicht verschoben werden.  
  
### <a name="to-move-items-in-solution-explorer"></a>So verschieben Sie Elemente im Projektmappen-Explorer  
  
1.  Wählen Sie im Projektmappen-Explorer das Element aus, das Sie verschieben möchten.  
  
2.  Klicken Sie im Menü **Bearbeiten** auf **Ausschneiden**.  
  
3.  Wählen Sie den Zielordner im Projektmappen-Explorer aus.  
  
4.  Klicken Sie im Menü **Bearbeiten** auf **Einfügen**.  
  
Sie können Elemente durch Ziehen von Abfragen und sonstigen Dateien innerhalb des Projektmappen-Explorers verschieben. Beim Ziehen sehen Sie das Ergebnis des Ziehvorgangs. Wenn Abfragen aus einem Projekttyp in einen anderen verschoben werden, können die Abfragen im Zielprojekt möglicherweise als sonstige Dateien betrachtet werden.  
  
> [!NOTE]  
> Wenn eine verbundene Abfrage verschoben wird, wird nicht die Verbindung in das Zielprojekt verschoben. Die Abfrage verliert die Verbindung, nachdem sie in das Zielprojekt verschoben wurde.  
  
## <a name="see-also"></a>Weitere Informationen  
[Projektmappen-Explorer](../../ssms/solution/solution-explorer.md)  
[Kopieren von Elementen in einer Projektmappe](../../ssms/solution/copy-items-in-a-solution.md)  
  
