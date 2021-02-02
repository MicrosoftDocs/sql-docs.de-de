---
title: Anzeigen von Facets der richtlinienbasierten Verwaltung für ein Objekt
description: In diesem Artikel wird erläutert, wie Sie in SQL Server Management Studio (SSMS) alle Facets der richtlinienbasierten Verwaltung anzeigen, die auf ein bestimmtes SQL Server-Objekt angewendet wurden.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, view facets
ms.assetid: 5f423b9f-a6c4-41a7-9d8d-8f4926ce1fb4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c30bbe5fdd1b88420ae22cde6053487869d126b2
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077028"
---
# <a name="view-policy-based-management-facets-on-a-sql-server-object"></a>Anzeigen der Facets der richtlinienbasierten Verwaltung für ein SQL Server-Objekt
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie Sie alle Facets der richtlinienbasierten Verwaltung, die auf ein bestimmtes SQL Server-Objekt in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] angewendet wurden, mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]anzeigen.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Sicherheit](#Security)  
  
-   **Anzeigen aller Facets in einem Objekt mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die Mitgliedschaft in der PolicyAdministratorRole-Rolle in der msdb-Datenbank.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-view-all-of-the-facets-in-an-object"></a>So zeigen Sie alle Facets in einem Objekt an  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ein Instanzobjekt, eine Datenbank oder ein Datenbankobjekt, und klicken Sie dann auf **Facets**.  
  
2.  Wählen Sie im Dialogfeld **Facets anzeigen -** _Objektname_ in der **Facetliste** ein Facet aus, dessen Eigenschaften Sie anzeigen möchten. Weitere Informationen zu den Optionen in diesem Dialogfeld finden Sie unter [View Facets Dialog Box](../../relational-databases/policy-based-management/view-facets-dialog-box.md).  
  
3.  Wenn Sie fertig sind, klicken Sie auf **OK**.  
  
  
