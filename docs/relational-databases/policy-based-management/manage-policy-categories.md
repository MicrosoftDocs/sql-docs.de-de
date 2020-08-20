---
description: Richtlinienkategorien verwalten
title: Richtlinienkategorien verwalten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.dmf.policycategories.f1
ms.assetid: d188a819-731f-4029-98aa-780d3299a0ce
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 26b15222f570304f321337a4b3574c3fa2f3b945
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88494004"
---
# <a name="manage-policy-categories"></a>Richtlinienkategorien verwalten
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie einzelne oder alle verfügbaren Richtlinien in einer Kategorie unter Verwendung von [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] oder [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] auf die gesamte Instanz von [!INCLUDE[tsql](../../includes/tsql-md.md)]angewendet werden.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
     [Security](#Security)  
  
-   **Anwenden von Kategorierichtlinien auf eine SQL Server-Instanz mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Wenn das Kontrollkästchen [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]Datenbankabonnements beauftragen **bei Verwendung von** nicht aktiviert ist, muss die Richtlinienkategorie einzeln auf jeden relevanten Bereich des Servers angewendet werden, wie z. B. auf eine oder mehrere Datenbanken oder Tabellen.  
  
-   Wenn Sie eine Richtlinienkategorie angeben, die nicht vorhanden ist, wird eine neue Richtlinienkategorie erstellt. Das Abonnement wird für alle Datenbanken beauftragt, wenn Sie die gespeicherte Prozedur ausführen. Wenn Sie dann das beauftragte Abonnement für die neue Kategorie löschen, ist das Abonnement nur für die Datenbank gültig, die Sie als *target_object*angegeben haben. Weitere Informationen zum Ändern der Einstellung eines beauftragten Abonnements finden Sie unter [sp_syspolicy_update_policy_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-update-policy-category-transact-sql.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Diese gespeicherte Prozedur wird im Kontext des aktuellen Besitzers der gespeicherten Prozedur ausgeführt.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-apply-category-policies-to-a-sql-server-instance"></a>So wenden Sie Kategorierichtlinien auf eine SQL Server-Instanz an  
  
1.  Klicken Sie im **Objekt-Explorer**auf das Pluszeichen, um den Server zu erweitern, auf dem Sie die Kategorierichtlinien anwenden.  
  
2.  Klicken Sie auf das Pluszeichen, um den Ordner **Verwaltung** zu erweitern.  
  
3.  Klicken Sie mit der rechten Maustaste auf **Richtlinienverwaltung** , und wählen Sie **Kategorien verwalten**aus.  
  
     Die folgenden Informationen sind im Dialogfeld **Richtlinienkategorien verwalten** verfügbar:  
  
     **Name**  
     Der Name der Richtlinienkategorie.  
  
     **bei Verwendung von**  
     Zwingt alle Datenbanken in der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Richtlinien in der Richtlinienkategorie durchzusetzen.  
  
4.  Aktivieren oder deaktivieren Sie einzelne oder alle Kontrollkästchen unter **Datenbankabonnements beauftragen** , um diese Richtlinienkategorie auf die SQL Server-Instanz anzuwenden.  
  
5.  Wenn Sie fertig sind, klicken Sie auf **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-apply-category-policies-to-a-sql-server-instance"></a>So wenden Sie Kategorierichtlinien auf eine SQL Server-Instanz an  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    USE msdb;  
    GO  
    -- configures the specified database to subscribe to a policy category that is named 'Table Naming Policies'.  
    EXEC dbo.sp_syspolicy_add_policy_category_subscription @target_type = N'DATABASE'  
    , @target_object = N'AdventureWorks2012'  
    , @policy_category = N'Table Naming Policies';  
    GO  
    ```  
  
 Weitere Informationen finden Sie unter [sp_syspolicy_add_policy_category_subscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-add-policy-category-subscription-transact-sql.md).  
  
  
