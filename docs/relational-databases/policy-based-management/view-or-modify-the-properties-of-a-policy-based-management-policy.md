---
title: Anzeigen oder Ändern der Eigenschaften einer Richtlinie der richtlinienbasierten Verwaltung
description: Hier erfahren Sie, wie Sie die Eigenschaften einer Richtlinie der richtlinienbasierten Verwaltung für SQL Server mithilfe von SQL Server Management Studio (SSMS) oder Transact-SQL (T-SQL) anzeigen oder ändern.
ms.custom: seo-lt-2019
ms.date: 10/06/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, modify policies
- Policy-Based Management, view policies
ms.assetid: ba805504-5db5-4731-a8da-a0e89cb20c37
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 1947e5d6b1363cfab43607882f09988074d25eed
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076988"
---
# <a name="view-or-modify-the-properties-of-a-policy-based-management-policy"></a>Anzeigen oder Ändern der Eigenschaften einer Richtlinie der richtlinienbasierten Verwaltung
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie Sie die Eigenschaften einer Richtlinie der richtlinienbasierten Verwaltung mithilfe von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] oder [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] in [!INCLUDE[tsql](../../includes/tsql-md.md)]ändern.  
  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die Mitgliedschaft in der PolicyAdministratorRole-Rolle in der msdb-Datenbank.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-view-the-properties-of-all-policies-on-an-object"></a>So zeigen Sie die Eigenschaften aller Richtlinien für ein Objekt an  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf einen Server, ein Serverobjekt, eine Datenbank oder ein Datenbankobjekt, zeigen Sie auf **Richtlinien** , und wählen Sie dann **Anzeigen** aus. Weitere Informationen zu den verfügbaren Optionen im Dialogfeld **Richtlinien anzeigen –** _Objektname_ finden Sie unter [Dialogfeld „Richtlinien anzeigen“](../../relational-databases/policy-based-management/view-policies-dialog-box.md).  
  
2.  Wenn Sie fertig sind, klicken Sie auf **Schließen**.  
  
#### <a name="to-view-or-modify-a-specific-policys-properties"></a>So zeigen Sie die Eigenschaften einer bestimmten Richtlinie an bzw. ändern sie  
  
1.  Klicken Sie im **Objekt-Explorer** auf das Pluszeichen, um den Server zu erweitern, der die Richtlinie der richtlinienbasierten Verwaltung enthält, die Sie anzeigen oder ändern möchten.  
  
2.  Klicken Sie auf das Pluszeichen, um den Ordner **Verwaltung** zu erweitern.  
  
3.  Klicken Sie auf das Pluszeichen, um **Richtlinienverwaltung** zu erweitern.  
  
4.  Klicken Sie auf das Pluszeichen, um den Ordner **Richtlinien** zu erweitern.  
  
5.  Klicken Sie mit der rechten Maustaste auf die Richtlinie, die Sie anzeigen oder ändern möchten, und wählen Sie **Eigenschaften** aus. Weitere Informationen zu den verfügbaren Optionen im Dialogfeld **Richtlinie öffnen –** _Richtlinienname_ finden Sie unter [Dialogfeld „Neue Richtlinie erstellen“ oder „Richtlinie öffnen“, Seite „Allgemein“](../../relational-databases/policy-based-management/create-new-policy-or-open-policy-dialog-box-general-page.md) oder [Dialogfeld „Neue Richtlinie erstellen“ oder „Richtlinie öffnen“, Seite „Beschreibung“](../../relational-databases/policy-based-management/create-new-policy-or-open-policy-dialog-box-description-page.md).  
  
6.  Wenn Sie fertig sind, klicken Sie auf **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-view-a-policys-properties"></a>So zeigen Sie die Eigenschaften einer Richtlinie an  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    USE msdb;  
    GO  
    SELECT name,  
       execution_mode,  
       description,  
       is_enabled,  
       job_id  
    FROM syspolicy_policies;  
    GO  
    ```  
  
 Weitere Informationen finden Sie unter [syspolicy_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syspolicy-policies-transact-sql.md).  
  
  
