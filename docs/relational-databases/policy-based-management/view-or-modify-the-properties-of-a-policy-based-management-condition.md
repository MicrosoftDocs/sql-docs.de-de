---
title: Anzeigen oder Ändern der Eigenschaften einer Bedingung der richtlinienbasierten Verwaltung
description: In diesem Artikel erfahren Sie, wie Sie die Eigenschaften einer Bedingung der richtlinienbasierten Verwaltung anzeigen oder ändern. Dazu verwenden Sie SQL Server Management Studio (SSMS) oder Transact-SQL (T-SQL).
ms.custom: ''
ms.date: 10/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, view policy conditions
- Policy-Based Management, modify policy conditions
ms.assetid: 890d7384-8444-4767-bb6f-f5debb155747
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ffdb11c75726469d61c54c9a2cc12ff18a31ca1a
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076182"
---
# <a name="view-or-modify-the-properties-of-a-policy-based-management-condition"></a>Anzeigen oder Ändern der Eigenschaften einer Bedingung der richtlinienbasierten Verwaltung
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie Sie die Eigenschaften einer Bedingung der richtlinienbasierten Verwaltung mithilfe von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] oder [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] in [!INCLUDE[tsql](../../includes/tsql-md.md)]ändern.  
  

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  

  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die Mitgliedschaft in der PolicyAdministratorRole-Rolle in der msdb-Datenbank.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-view-or-modify-a-conditions-properties"></a>So zeigen Sie die Eigenschaften einer Bedingung an bzw. ändern sie  
  
1.  Klicken Sie im **Objekt-Explorer** auf das Pluszeichen, um den Server zu erweitern, der die Bedingungen enthält, die Sie anzeigen oder ändern möchten.  
  
2.  Klicken Sie auf das Pluszeichen, um den Ordner **Verwaltung** zu erweitern.  
  
3.  Klicken Sie auf das Pluszeichen, um **Richtlinienverwaltung** zu erweitern.  
  
4.  Klicken Sie auf das Pluszeichen, um den Ordner **Bedingungen** zu erweitern.  
  
5.  Klicken Sie mit der rechten Maustaste auf die Bedingung, die Sie anzeigen oder bearbeiten möchten, und wählen Sie **Eigenschaften** aus. Weitere Informationen zu den Optionen im **Dialogfeld „Bedingung öffnen –** _Bedingungsname_“ finden Sie unter [Dialogfeld „Neue Bedingung erstellen“ oder „Bedingung öffnen“, Seite „Allgemein“](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-general-page.md), [Dialogfeld „Bedingung öffnen“, Seite „Abhängige Richtlinien“](../../relational-databases/policy-based-management/open-condition-dialog-box-dependent-policies-page.md), [Dialogfeld „Neue Bedingung erstellen“ oder „Bedingung öffnen“, Seite „Beschreibung“](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-description-page.md) und [Dialogfeld „Erweiterte Bearbeitung &#40;Bedingung&#41;“](../../relational-databases/policy-based-management/advanced-edit-condition-dialog-box.md).  
  
6.  Wenn Sie fertig sind, klicken Sie auf **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-view-a-conditions-properties"></a>So zeigen Sie die Eigenschaften einer Bedingung an  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    USE msdb;  
    GO  
    SELECT name,  
       description,  
       facet,  
       expression,  
       is_name_condition,  
       obj_name  
    FROM syspolicy_conditions;  
    GO  
  
    ```  
  
 Weitere Informationen finden Sie unter [yspolicy_conditions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syspolicy-conditions-transact-sql.md).  
  
  
