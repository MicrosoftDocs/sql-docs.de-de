---
description: Erstellen einer neuen Bedingung der richtlinienbasierten Verwaltung
title: Erstellen einer neuen Bedingung der richtlinienbasierten Verwaltung | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, creating policy conditions
ms.assetid: 8a612f7e-6c70-49db-a4de-48431e097cc5
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f8eef68a189d9e3c3c8cce16de803ce3b87f0207
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048816"
---
# <a name="create-a-new-policy-based-management-condition"></a>Erstellen einer neuen Bedingung der richtlinienbasierten Verwaltung
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie Sie eine Bedingung der richtlinienbasierten Verwaltung in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]erstellen.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Security](#Security)  
  
-   **Erstellen einer Bedingung mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die Mitgliedschaft in der PolicyAdministratorRole-Rolle in der msdb-Datenbank.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-create-a-condition"></a>So erstellen Sie eine Bedingung  
  
1.  Klicken Sie im **Objekt-Explorer** auf das Pluszeichen, um den Server zu erweitern, auf dem Sie die Bedingung der richtlinenbasierten Verwaltung erstellen möchten.  
  
2.  Klicken Sie auf das Pluszeichen, um den Ordner **Verwaltung** zu erweitern.  
  
3.  Klicken Sie auf das Pluszeichen, um **Richtlinienverwaltung** zu erweitern.  
  
4.  Klicken Sie auf das Pluszeichen, um den Ordner **Facets** zu erweitern.  
  
5.  Klicken Sie mit der rechten Maustaste auf das Facet, in dem Sie eine neue Bedingung erstellen möchten, und wählen Sie **Neue Bedingung** aus.  
  
6.  Geben Sie im Dialogfeld **Neue Bedingung erstellen** im Feld **Name** den Namen der neuen Bedingung ein.  
  
7.  Bestätigen Sie das richtige Facet in der Liste **Facet** , oder wählen Sie ein anderes Facet aus.  
  
8.  Erstellen Sie unter **Ausdruck** Bedingungsausdrücke, indem Sie eine Faceteigenschaft im Feld **Feld** zusammen mit dem dazugehörigen Operator und Wert auswählen. Wenn Sie mehrere Ausdrücke hinzufügen, können die Ausdrücke mit **Und** oder **Oder** verbunden werden. Weitere Informationen zu den Optionen in diesem Dialogfeld finden Sie unter [Dialogfeld 'Neue Bedingung erstellen' oder 'Bedingung öffnen', Seite 'Allgemein'](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-general-page.md), [Dialogfeld 'Neue Bedingung erstellen' oder 'Bedingung öffnen', Seite 'Beschreibung'](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-description-page.md) und [Dialogfeld 'Erweiterte Bearbeitung &#40;Bedingung&#41;'](../../relational-databases/policy-based-management/advanced-edit-condition-dialog-box.md).  
  
9. Wenn Sie fertig sind, klicken Sie auf **OK**.  
  
  
