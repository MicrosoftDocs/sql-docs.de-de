---
description: Entfernen oder Löschen eines Elements oder Projekts
title: Entfernen oder Löschen eines Elements oder Projekts
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- deleting project items
- projects [SQL Server Management Studio], item removal
- removing project items
ms.assetid: 3fd92434-70f5-466e-bef0-7e0fd73ddb1c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5e8389848928507d29be094faaf626f99ba9c8ef
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88491827"
---
# <a name="remove-or-delete-an-item-or-project"></a>Entfernen oder Löschen eines Elements oder Projekts
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Projektelemente in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]-Projekten sind Abfragen, Verbindungen und sonstige Dateien. Sie können Projektabfragen und sonstige Dateien aus der Projektmappe löschen, ohne die Dateien aus dem Speicher zu löschen. Entfernen Sie ein Projekt oder Element, wenn es in der aktuellen Projektmappe nicht nützlich ist und in eine andere Projektmappe eingefügt werden soll.  
  
### <a name="to-remove-a-project-item"></a>So entfernen Sie ein Projektelement  
  
1.  Wählen Sie im Projektmappen-Explorer das Projektelement aus, das Sie entfernen möchten.  
  
2.  Klicken Sie im Menü **Bearbeiten** auf **Entfernen**.  
  
3.  Klicken Sie im Bestätigungsdialogfeld auf **Entfernen** , um das Element aus dem Projekt zu entfernen.  
  
Ein entferntes Element ist weiterhin im Dateisystem vorhanden. Deshalb können Sie ein entferntes Element wieder zu seiner ursprünglichen oder zu einer anderen Projektmappe hinzufügen.  
  
#### <a name="to-remove-a-project"></a>So entfernen Sie ein Projekt  
  
1.  Wählen Sie im Projektmappen-Explorer das Projekt aus, das Sie entfernen möchten.  
  
2.  Klicken Sie im Menü **Bearbeiten** auf **Entfernen**.  
  
3.  Klicken Sie im Bestätigungsdialogfeld auf **OK**, um das Projekt aus der Projektmappe zu entfernen.  
  
Sie können ein Projekt dauerhaft löschen, müssen jedoch zuerst alle Verweise auf das Projekt aus [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] -Projektmappen entfernen und dann die zugeordneten Dateien mithilfe von [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Explorer dauerhaft aus dem Speicher löschen.  
  
#### <a name="to-delete-a-project"></a>So löschen Sie ein Projekt  
  
1.  Entfernen Sie im Projektmappen-Explorer das Projekt, das Sie löschen möchten, aus der Projektmappe.  
  
2.  Suchen und markieren Sie in Windows Explorer die Dateien, die dem zu löschenden Projekt oder Element zugeordnet sind.  
  
3.  Klicken Sie im Menü **Datei** auf **Löschen**.  
  
## <a name="see-also"></a>Weitere Informationen  
[Projektmappen-Explorer](../../ssms/solution/solution-explorer.md)  
[Hinzufügen neuer Elemente zu einem Projekt](../../ssms/solution/add-new-items-to-a-project.md)  
[Hinzufügen vorhandener Elemente zu einem Projekt](../../ssms/solution/add-existing-items-to-a-project.md)  
  
